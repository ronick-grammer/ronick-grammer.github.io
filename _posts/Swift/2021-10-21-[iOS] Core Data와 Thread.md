---
title:  "[iOS, CoreData] Core Data와 Thread"
excerpt: "[iOS, CoreData] Core Data와 Thread"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - iOS
  - Swift
---

# Core Data와 viewContext
(코어 데이터의 정의와 코어 데이터 스택에 대해 알고 있다고 가정하고 글을 작성합니다. [이 링크](https://ronick-grammer.github.io/ios/swift/iOS-Core-Data/)를 참조하세요)

코드로 코어 데이터를 건든다면 가장 많이 사용하게 되는 것이 NSManagedObjectContext(viewContext)인데 이 녀석 중요하다. 왜 중요하냐면 이 녀석을 통해서 데이터를 불러오고 저장하는 요청을 할 것이기 때문이다.
그리고 또 한가지 중요한 것은 이 viewContext는 바로 main queue/thread 에서 실행 되는 녀석이라는 점이다. 알아채신 분이 있겠지만 main queue/thread 라는 단어 얘기를 들으면 바로 "UI와 관련된 매우 
중요한 작업 영역이구나~ 그렇기 때문에 절대로 멈춰지면(잠시라도) or 블록되면 안되는 영역이구나~" 라는 생각이 바로 떠올라야 한다. 그렇기 때문에 "NSManagedObjectContext는 Thread Safe 하지 않다"라고 할 수
있다.
<br><br>
스레드 개념까지 나와서 너무 먼산까지 가는게 아닌가 싶지만 그렇지 않다. 메인 스레드는 UI가 처리되는 매우매우 중요한 영역인데 데이터를 불러오느라 메인 스레드에서 UI 처리를 제대로 수행하지 못하게 된다면?
그렇게 되면 사용자는 "왜 화면이 안타나는겨" or "아씨 왜 안눌러져 짜증나게" 라는 말을 내뱉으면서 앱의 내부 블록 상황을 모른채 그대로 앱을 삭제하고 다시는 다운받지 않을 것이다. 그렇다. 우리는 대량의 데이터를 가져오기
위해서 UI의 작업 영역인 main queue/thread를 사용하면 안되는 것이다. 그렇다면 다음과 같이 물을 수 있다 <br><br>
"Persistent Container가 제공하는 NSManagedContext는 viewContext 이놈 하나이니까 백그라운드 영역에서 처리하면 되잖아? 병렬처리 큐를 이용해서 스레드에서 처리하게 하면 되겠네!"

```swift
var container: NSPersistentContainer? = (UIApplication.shared.delegate as? AppDelegate)?.persistentContainer
var viewContext = container?.viewContext
.
.
.
DispatchQueue.global().async {
  let request: NSFetchRequest<Person> = Person.fetchRequest()
  request.predicate = NSPredicate(format: "name = %@", personInfo.name)

  do {
      let people = try viewContext.fetch(request) // 멈춰!!

  } catch {
      throw error
  }
}
```

아니, 안된다. 절대 안된다. 앞서 언급한 적이 있다. "viewContext는 바로 main queue/thread 에서 실행 되는 녀석이라는 점이다." Foreground 영역에서 작업이 이루어지는 viewContext를 Background 영역에서
작업을 처리한다는 것은 논리적으로 맞지도 않고, 저대로 실행하면 Xcode는 에러를 발생시키거나 앱 실행 도중 Crush를 발생시킨다. 바로 아래 처럼 하면 된다.

```swift

var container: NSPersistentContainer? = (UIApplication.shared.delegate as? AppDelegate)?.persistentContainer
var viewContext = container?.viewContext // 여기서 사용안함
.
.
.

// 백그라운드 스레드에서 대량의 데이터를 가져와서 UI Block 을 방지하자
container?.performBackgroundTask { [weak self] context in // self를 weak로 캡쳐해서 참조 카운트 증가를 막자

  let request: NSFetchRequest<Person> = Person.fetchRequest()
  request.predicate = NSPredicate(format: "name = %@", personInfo.name)

  do {
      let people = try context.fetch(request) // 계속해

  } catch {
      throw error
  }
}
```

위의 코드에서 보이듯이 container(NSPersistentContainer)의 performBackgroundTask를 실행함으로써 background 스레드에서 작업할 수 있는 또다른 context(NSManagedObjectContext)를 클로저를 통해
반환받고 이를 사용해서 데이터를 불러올 수 있다. 이렇게 해서 Main Thread 의 Block 상황이 생기는걸 막을 수 있다. 
<br><br>
그러나 예기치 않게 viewContext가 메인 스레드가 아닌 다른 스레드에서 사용되어야 한다면? 그것도 방법이 있다.
<br>

```swift
viewContext.perform(block: () -> Void) 
```

위 명령은 마치 "내가 생성된 큐(여기서는 viewContext이니 main queue가 된다)에서 명령을 처리하겠어!" 라고 말하는 것과 같다. 그러니까 내가 만약 내가 생성된 큐에서 실행되고 있는게 아니면 심각한 
오류가 발생할 테니까, 내가 생성된 큐로 다시 날 보내서 실행하라는 것이다. 백문이 불여일견이라고 했다. 아래 코드를 보면 이해가 좀더 쉬울 것이다.

```swift

var container: NSPersistentContainer? = (UIApplication.shared.delegate as? AppDelegate)?.persistentContainer
var viewContext = container?.viewContext
.
.
.

container?.performBackgroundTask { [weak self] context in
  .
  .
  .
  
   self?.viewContext.perform { // viewContext가 생성된 큐안에서 안전하게 실행되는 것을 보장!
      if Thread.isMainThread { // viewContext가 MainThread에 있는지 없는지 체크해보세요
          print("on main thread")
      } else {
          print("off main thread")
      }

      // 전부 가져오기
      if let personCount = (try? self?.viewContext.fetch(Person.fetchRequest()))?.count {
          print("\(personCount) person")
      }
}
```



---
title:  "[TDD] TDD Expectation"
excerpt: "[TDD] TDD Expectation"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - TDD
  - IOS
  - Swift
---

## 비동기 처리 테스트
오늘은 특정 메서드 호출로 인한 값의 직접적인 변화에 대한 테스트가 아니라 비동기 처리를 위한 테스트를 어떻게 하는지에 대해 알아보도록 하자. iOS에서 비동기 처리를 하는 방법은 대표적으로 세가지 방법이 있다.

- 클로저를 특정 이벤트가 발생하였을 때 콜백함수로 사용한다.
- 델리게이트 메서드를 특정 이벤트가 발생하였을 때 호출한다.
- 특정 이벤트에 대한 Notification을 관찰하여 처리한다.

### expectation과 waiter
비동기에 대해 테스트를 하기 위해서 비동기 이벤트가 발생하였을 때 이를 체크해야만 한다. XCTest에서는 비동기 이벤트가 발생하였을 때 expectation(예상)이 fulfill(충족: 그대로 해석 X)될 때까지 wait(대기)
하는 테스트 과정을 가진다.

- expectation: fullfill이 되어 성공했다는 신호를 wait에게 알리게 된다.
- wait: expectation이 fullfill 될때 까지 기다리거나 특정 시간만큼 기다린다. 특정 시간이 지나면 테스트는 실패한것으로 간주한다.

코드를 계속 작성하면서 expectation에 대해 어떻게 설명할까 고민했는데, 표현이 완전한 것은 아니지만 일종의 '관찰자'라고 이해해도 좋다는 생각을 했다. 특정 이벤트가 발생할 때 fulfill 하는 과정이 특정 이벤트가 
발생하는 것을 관찰한다는 내용과 부합한다고 느껴진다.

### 클로저가 콜백함수로 사용될 때의 테스트

실제 테스트 코드를 통해서 자세히 보도록 하자.

```swift 
func testAppModel_whenStateChanges_executesCallback() {
  // given
  givenInProgress()
  var observedState = AppState.notStarted

  // expectation 객체 생성
  let expected = expectation(description: "callBack happend")
  
  // 상태값이 바뀌면 실행
  sut.stateChangedCallback = { model in
    observedState = model.appState
    expected.fulfill()
  }

  // when
  sut.pause()

  // then
  // 3
  wait(for: [expected], timeout: 1)
  XCTAssertEqual(observedState, .paused)

}
```

위 코드에서는 pause() 이벤트가 발생하여 상태값이 바뀌면 stateChangedCallback 클로저가 실행된다. wait는 expected가 fulfill()을 호출할 때까지 기다리고 있다가
fulfill 상태가 되거나 1초가 지나면 다음 라인을 실행하게 된다. 다만 fullfill 되지 않고 1초가 지나면 테스트는 실패하게 되고 observedState가 pause 상태가 아니어도 테스트는 실패결과로 나타난다.
<br>
번외로 assert 함수를 통한 테스트 없이 wait함수만으로 테스트 케이스의 성공, 실패 여부를 확인할 수 있지만, 이는 권장되지 않는다. 단순히 비동기 이벤트가 발생했고 안했고를 테스트하는게 중요한게 아니라 '비동기 이벤트로 인해 생긴 특정 결과값'에 대한 테스트를 하는게 TDD에서는 더 중요하기 때문이다.

### 버튼의 상태 이벤트 테스트하기
버튼의 상태를 관찰하다가 특정 이벤트가 발생하여 버튼의 텍스트의 변화가 생기는 것을 테스트 하는 코드의 예시를 보자.

```swift

class ButtonObserver: NSObject {
  
  var expectation: XCTestExpectation? 
  weak var button : UIButton?
  
  func observe(_ button: UIButton, expectation: XCTestExpectation) {
    self.expectation = expectation
    self.button = button
    
    // ButtonObserver(self)가 titleLabel.text를 관찰하도록 함
    button.addObserver(self, forKeyPath: "titleLabel.text", options: [.new], context: nil) 
  }
  
  override func observeValue(forKeyPath keyPath: String?, of object: Any?, change: [NSKeyValueChangeKey : Any]?, context: UnsafeMutableRawPointer?) {
    expectation?.fulfill() // 타이틀에 변화가 생기면 fulfill
  }
  
  deinit {
    button?.removeObserver(self, forKeyPath: "titleLabel.text")
  }
}

.
.
.


func testController_whenCaught_buttonLabelIsTryAgain() {
  // given
  let sut = Given()
  let exp = expectation(description: "button title change")
  let observer = ButtonObserver()
  observer.observe(sut.startButton, expectation: exp) // 관찰 시작

  // when
  whenButtonTitleChanges() // 버튼 타이틀 바뀌는 이벤트 호출

  // then
  waitForExpectations(timeout: 1) // 현재 테스트 케이스내의 모든 expectaion이 fulfill 될 때까지 기다림
  let text = sut.startButton.title(for: .normal)
  XCTAssertEqual(text, AppState.caught.nextStateButtonLabel)
}
```

<br>
위 코드는 버튼의 타이틀 변경이 생길 때를 관찰하고 있다가 변경이 생기면 expectation이 fulfill 된다. ButtonObserver의 다음 코드를 유심히 보자.

```swift
// ButtonObserver(self)가 titleLabel.text를 관찰하도록 함
button.addObserver(self, forKeyPath: "titleLabel.text", options: [.new], context: nil) 
```

<br>
관찰당할 버튼은 자신을 관찰할 객체 self 를 지정하고 자신의 titleLabel.text를 그 관찰 대상으로 지정한다. addObserver메서드는 KVO Notification을 받을 수 있는 관찰 객체를 등록하도록 하는데,
KVO Notification은 Key Value Observing 로서 관찰당할 객체와 연관된 key path의 value에 대한 변화 알림이라고 보면 된다.
다시 돌아와서 그렇기 때문에 titleLabel.text에 변화가 생길때 마다 다음 메서드가 호출된다.

```swift
override func observeValue(forKeyPath keyPath: String?, of object: Any?, change: [NSKeyValueChangeKey : Any]?, context: UnsafeMutableRawPointer?) {
  expectation?.fulfill() // 타이틀에 변화가 생기면 fulfill
}
```

<br>
하지만 이렇게 ButtonObserver 클래스를 만들지 않고 더 간단한 방법으로 값의 변화를 관찰하는 방법이 있다. 바로 KVO를 가지는 expectaion을 생성하는 것! ButtonObserver와 관련된 모든 코드를 지우고
아래와 같은 코드로 대체하면 간단히 구현된다. 버튼의 keyPath의 value가 변할 때 exp는 자동으로 fulfill이 된다.

```swift
let exp = keyValueObservingExpectation(for: sut.startButton as Any, keyPath: "titleLabel.text")
```

### Notification(알림) 테스트
wait 함수는 테스트 케이스내의 XCTestExpectaion 객체가 fulfill이 될 때까지 기다리는 역할을 하였다. 자세히 살펴보자.

```swift
wait(for: [XCTestExpectation], timeout: TimeInterval)
```

<br>
보이듯이 여러 XCTestExpectation 객체를 받아 XCTestExpectation 객체들이 fulfill 되면 다음 줄을 실행하게 된다. 단 하나의 XCTestExpectation 객체라도 fulfill 되지 않거나 timeout이 초과되면 다음 줄을 실행하지 않으며 이는 테스트가 실패하는 결과를 가져온다.
<br><br>
현재까지는 XCTestExpectation 객체가 직접 fulfill() 메서드를 호출하여 fulfill 되게 하였지만, Notification을 테스트 할 때는 다르다. 다음 코드를 보자.

```swift
func testPostOne_gerneratesANotification() {
  // given
  let exp = expectation(forNotification: AlertNotification.name, object: sut, handler: nil)

  // when
  sut.postAlert(alert: AlertNotification.name)

  wait(for: [exp], timeout: 1)
}
```

<br>
위 테스트 케이스는 AlertNotification.name 이라는 이름을 가진 알림이 sut에 전해질 때(post 될 때) exp가 fulfill 되는 것을 테스트한다. 그리고 이에 대한 앱 코드의 postAlert 부분을 보면

```swift
.
.
.
let notificationCenter: NotificationCenter

func postAlert(alert: String) {

  let notification = Notification(
    name: AlertNotification.name,
    object: self,
    userInfo: [AlertNotification.Keys.alert: alert])

    notificationCenter.post(notification) // 이때 테스트 케이스의 exp가 fulfill된다.
}
```

<br>
바로 위처럼 notification이 post 됨으로서 직접 fulfill() 해줄 필요없이 자동으로 fulfill 된다. 참고로 userInfo의 값은 동일한 이름의 AlertNotification.name이 post 되었을 때, 딕셔너리의 타입을 사용하여 같은 이름의 알림이지만 다른 메시지를 가지는 알림을 전달할 수 있다.

### fulfill되는 횟수 지정
XCTestExpectation 객체는 자신이 몇번 fulfill되어야 하는지 설정할 수 있다.

```swift
let exp = expectation(description: "callBack happend")

exp.expectedFulfillmentCount = 2 // expectation이 두번 fullfill 되는 것을 만족해야 함
```

<br>
expectedFulfillmentCount를 특정 횟수만큼 설정하면 wait함수는 exp가 그 특정횟수만큼 fulfill될때 까지 기다린다.


### 특정 이벤트가 일어나지 않을 것에 대한 테스트
지금까지는 특정 이벤트가 발생하는 것에 초점을 맞추어 테스트를 작성해왔지만, 좋은 테스트란 그 반대인, 일어나지 않는 이벤트가 발생하는 경우도 포함되어야 한다. 다음 코드는 이러한 설정을 가능하게 해준다.

```swift
let exp = expectation(description: "callBack happend")

exp.isInverted = true
```

isInverte 프로퍼티는 expectation이 fullfill 되면 '실패', wait의 timeout이 초과되면 '성공' 하게 되는 조건으로 설정한다. 그렇게 되면 waiter는 expectation이 실패할 때까지 혹은 timeout이
초과될 때까지 기다리는 반대 조건으로 설정되는 것이다.

### Notification에서 expectaion이 fulfill되는 조건 설정하기
전에 notification에 대한 expectation 객체를 생성하였을 때 handler에 nil을 설정을 하였었다.

```swift
let exp = expectation(forNotification: AlertNotification.name, object: sut, handler: nil)
```

<br>
handler가 nil이면 AlertNotification.name의 알림이 sut 객체에 전달되었을 때 디폴트로 exp가 fulfill된다. 이처럼 handler 클로저를 이용하여 알림이 전달되었을 때 expctation이 fulfill되는 조건을 
설정할 수가 있다.

```swift
let exp = expectation(forNotification: AlertNotification.name, object: sut) { notification -> Bool in
  return notification.alert == Alert("whatever") // 같아야만 fulfill 됨
}
```

### 다수의 expectaion이 fulfill되어야 할 때
여러 expectaion이 하나의 테스트 케이스에서 fulfill되어야 할 때 wait 함수는 다음과 같이 설정할 수 있다.

```swift 
let expectations = [
  expectation(description: "1"),
  expectation(description: "2"),
  expectation(description: "3")
]

.
.
.

wait(expectations, timeout: 1, enforceOrder: true)
```

<br>
위 코드에서 wait함수는 XCTestExpectation의 array 객체를 받아서 모든 expectaion들이 fulfill될 때까지 기다린다. 또한 enforceOrder를 true로 설정하면 array로 받은 XCTestExpectation의 객체들이 순서대로 fulfill되어야만 한다. 즉, 위 예시라면, "1" -> "2" -> "3" 순서대로!

<br><br><br>
ps. 진짜 너무 오래 걸린다,,
   
   
   
   







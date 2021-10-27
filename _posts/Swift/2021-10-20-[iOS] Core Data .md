---
title:  "[iOS, CoreData] Core Data란"
excerpt: "[iOS, CoreData] Core Data란"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - iOS
  - Swift
---

# Core Data란?

<p align="center">
  <img src= "https://user-images.githubusercontent.com/73280175/139062834-3780e220-23ac-4543-8689-9837f17b0cf7.png" width="40%"> 
</p>

앱의 디바이스 내부에 데이터를 저장하기 위한 애플의 iOS 프레임워크이다. '데이터 저장' 이라고 하니 데이터베이스가 아닌가 하고 햇갈릴 수도 있다(나도 그랬다..). 코어 데이터가 마치 데이터베이스 처럼 작동하는 듯 보이면서
실제로 다른 글들을 보면 SQLite와 코어 데이터와의 연관성을 말해주니 데이터베이스인것 처럼 보이지만, 코어데이터는 SQLite를 데이터의 영구 저장소로서 사용할 뿐 코어데이터 자체는 명백히 '프레임 워크'라는 것이 사실이다.
<br><br>
코어 데이터는 데이터베이스의 기능들을 가지고 있으면서 그 밖에 오브젝트 그래프를 관리하거나, 데이터 등의 변화를 감지하는 이벤트를 발생시켜 UI 데이터를 동기화 시키는데 유용하다.
코어 데이터의 주로 다음과 같이 사용된다. 
- 데이터의 캐싱을 위해 디바이스 내부에 영구적으로 저장하기 위해 사용된다.
- Undo 와 Redo 같은 기능을 수행하기 위해 사용된다.

데이터를 서버로 부터 매번 불러오는 것은 디바이스에 부담이니 데이터가 디바이스 내부에 저장되어 있다면 그것을 가져와서 처리하는 것이 효율적이다. 또한 데이터 변경같은 히스토리 데이터를 코어 데이터를 이용해 저장하고 불러오는
Undo와 Redo 기능을 구현하는데 사용할 수가 있다.


## 코어 데이터 스택
코어데이터를 사용하기 전에 그 구조를 먼저 훑어 보는 것이 좋다. 정말 훑어 보기만 하자. 한번에 이해가 되지 않을 것이기에 사용을 해본 다음 다시 보면 이해하는데 도움이 될것이다.

<p align="center">
  <img src= "https://user-images.githubusercontent.com/73280175/139062838-5e9fd080-1190-4ee6-b62c-20a9b0ef99f3.png" width="80%"> 
</p>

### Model: NSManagedObjectModel
Core Data를 사용하기 위해서 .xcdatamodeld 확장자를 가지는 데이터 모델 파일을 가지는데 이것이 NSManagedObjectModel이라고 보면 된다. 모델의 타입, 프로퍼티 그리고 관계를 정의한다. 이처럼 
.xcdatamodeld 에서는 에디터 형식으로 마치 관계형 데이터베이스의 테이블을 만드는 것처럼 내부 프로퍼티들의 타입과 이름을 정의해 엔티티를 만들수 있고, 엔티티들 사이의 관계를 정해주는 등 
다양한 정의를 설정해줄 수 있다.

### Context: NSManagedObjectContext
Context라는 용어 자체가 설명하기가 되게 까다로운데, "어떤 객체를 핸들링하기 위한 접근 수단" 이라고 보면 될 것 같다. 이 말에서 '어떤 객체'란 ManagedObject를 말하며 ManagedObject는 위의 Model에서
정의된 엔티티의 객체라고 보면 된다. 즉, Model에서 만들어진 엔티티는 클래스, Context의 ManagedObject는 이 클래스의 인스턴스라고 할 수 있다. Core Data 를 건들게 된다면 
가장 많이 사용하게 될 클래스가 될 것이다.

### Store Coordinator: NSPersistentStoreCoordinator
위 그림에서 Store Coordinator 아래에 데이터 저장소가 붙여저 있는 것에서 예측할 수 있듯이, Core Data 를 통해 저장된 데이터에 접근할 수 있는 코디네이터이다(그냥 코디네이터라고 부르자). 
Store Coordinator 는 코어 데이터 스택의 심장부라 할 수 있다. 왜냐하면 이놈이 없으면 Model과 Context는 아무 일도 할 수가 없다. 디바이스 앱의 실질적인 데이터를 관리함과 동시에 Model과 Context는 
Store Coordinator와 연결되어 있다. 이렇게 됨으로써 Context가 데이터 관련 요청을 하면 Store Coordinator는 Model을 참조하여 객체 데이터를 반환 혹은 저장소에 저장한다.

### Persistent Container: NSPersistentContainer
마지막으로 이놈은 위의 코어 데이터 스택 생성과 관리를 해주는 컨테이너라고 보면 된다. 위의 코어데이터 구조 사진을 코드로 보면 아래와 같다

<p>
  <img src= "https://user-images.githubusercontent.com/73280175/139064044-bc7ae091-66c2-4f55-acec-6e11990f0e8f.png" width="70%"> 
</p>

main bundle에서 "DataModel"의 이름을 가지는 Model을 기반으로 Persistent Container를 생성해주고 있다. 이 Persistent Container를 들여다 보면


<p>
  <img src= "https://user-images.githubusercontent.com/73280175/139064064-84e244b3-5ac6-4933-b0e8-0375c7a3d782.png" width="70%"> 
</p>

바로 위처럼 코어 데이터 스택 구조를 이루는 Model(managedObjectModel), Context(viewContext) 그리고  Store Coordinator(persistentStoreCoordinator)를 가지고 있는 것을 볼 수 있다.











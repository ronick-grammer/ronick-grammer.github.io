---
title:  "[TDD] TDD Expressions"
excerpt: "[TDD] TDD Expressions"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - TDD
  - IOS
  - Swift
---

## TDD Expressions
제목을 뭐라고 해야 할지 모르겠다. 'TDD 표현 방식'이라고 하기에는 이번에 다룰 내용이 그 범위에 포함된다고 하기 애매하다. 이번 포스팅에서는 
<br><br>

- TDD Assert 메서드
- 뷰 컨트롤러 기능 테스트
- TDD Cycle을 효과적으로 준수하는 법

<br>

에 대해서 다룬다.


## Assert Methods
XCTest 에서 테스트를 할 수 있도록 하는 몇몇 assert 함수들을 소개하자면 아래와 같다.

- 동일 여부: XCTAssertEqual, XCTAssertNotEqual
- 참거짓 여부: XCTAssertTrue, XCTAssertFalse
- nil값 여부: XCTAssertNil, XCTAssertNotNil
- 비교: XCTAssertLessThan, XCTAssertGreaterThan, XCTAssertLessThanOrEqual, XCTAssertGreaterThanOrEqual
- 에러: XCTAssertThrowsError, XCTAssertNoThrow

함수들 이름에서 어떠한 값을 테스트 하는 것인지 쉽게 알 수 있다. 참고로 XCTest는 실패하지 않는 테스트 케이스는 예외없이 전부 다 '성공'햇다는 결과를 나타내게 된다.
이 말은 각 테스트 케이스에 테스트하는 assert 함수가 없거나, 심지어 아무 코드가 없어도 '성공'했다고 판단하게 된다. 

```swift 
class DataModelTests: XCTestCase {
  
  var sut: DataModel!
  
  // 코드는 있지만 assert 함수가 없음, 그래도 성공
  func test_whenNoAssert_thenSucceed() {
    // when
    sut.test()
    
    // then 없음..
  }
  
  // 그냥 다 없음, 그래도 성공
  func test_whenNothing_thenSucceed() {
    // .......
  }
  
}
```

<br><br>
assert 함수들은 단지 있고 없고, 맞고 틀리고 같은 true false 의 여부를 가지고 성공과 실패를 나누기 때문에 이 assert 기능을 하는 함수를 직접 구현할 수가 있다.

```swift 
func XCTAssertReverseThenTrue(test: bool) {
  .
  .
  .
  .
  
  test = !test

  XCTAssertTrue(test)
}
```
<br><br>
각 테스트 케이스에서 하나의 assert만 사용하고 있고 테스트 케이스의 이름을 알맞게 지었다면 굳이 메시지를 작성해주지 않아도 된다. 테스트 케이스의 이름이 어떤 테스트인지 대변해 주고 있기 때문이다.

```swift 
// 테스트 케이스의 이름만으로 어떤 테스트인지 명확히 알 수 없거나,
func testModel_whatever() { 
  // 하나의 테스트 케이스에 다수의 assert함수가 존재할 경우 메시지를 작성해 주는 것이 좋다.
  XCTAssertFalse(sut.goalReached, "goalReached Should be false when the model is created")
  XCTAssertTrue(!sut.goalReached, "goalReached should be true when .........")
}

// 테스트 케이스의 이름만으로 어떤 테스트인지 유추할 수 있으므로, 메시지 작성 굳이 작성 ㄴㄴ
func testModel_whenStarted_goalIsNotReached() { 
  XCTAssertFalse(sut.goalReached)
}

```

## 뷰 컨트롤러 기능 테스트
뷰 컨트롤러를 테스트할 때 중요하게 여겨야 할 것은 뷰나 조작에 관한 테스트를 직접적으로 하지 않는 것이다. 대신 뷰 컨트롤러의 로직이나 상태에 관해 테스트를 진행해야 한다. 
<br><br>
기능 테스트는 뷰 컨트롤러의 메서드들에 대해서 진행을 하는데, 이때 이 메서드들은 UI와 상호작용하는 callBack이나 delegate 메서드 같은 각 기능별로 분리된 메서드들이다.
<br><br>
기능 테스트를 할 때 중요한 것이 바로 아키텍처 패턴인데, MVVM 이나 VIPER 패턴을 적용하면 한층 더 테스트를 수월하게 할 수 있게 해준다. MVVM을 들어 설명하자면
뷰 컨트롤러가 가지고 있던 유닛 테스트 로직을 ViewModel로 분리해 테스트가 가능하다. 뷰컨트롤러는 UI에 관한 로직을, ViewModel은 비즈니스 로직을 담아 기능을 나누어 역할이 구분되게 한다.

### 뷰 컨트롤러를 테스트할 때의 주의점 feat 스토리보드
iOS 앱의 주기에서 뷰 컨트롤러는 스토리보드에 의해 생성되고 메모리에 할당된다. 그러고 나서 모든 뷰 컨트롤러의 코드가 실행된다. 하지만 테스트 코드 작성시 뷰 컨틀로러를 직접 생성하고 사용하게 되면
테스트가 통과되지 않는데, 그 이유는 스토리 보드로 부터 생성된 뷰 컨트롤러의 초기 상태와 테스트 코드에서 직접 생성한 뷰 컨트롤러의 초기 상태가 같지 않기 때문이다.
<br><br>
즉, 테스트 코드 작성시 스토리 보드 기반의 뷰 컨트롤러를 코드로 생성하게 되면 스토리보드와 연결된 @IBOutlet 프로퍼티는 사용가능한 상태가 아니라서 이에 대한 테스트는 불가능하다.
그렇기에 스토리보드로 부터 생성된 뷰 컨트롤러를 테스트해야한다. 그러기 위해서는 약간의 트릭을 써야한다.

```swift
import UIKit
@testable import FitNess

// 앱이 일반적으로 실행 되었을 때, 앱 윈도우의 루트 뷰 컨트롤러를 반환
func loadRootViewController() -> RootViewController {
  let window = UIApplication.shared.windows[0]
  return window.rootViewController as! RootViewController
}

// 원하는 뷰 컨트롤러를 루트 뷰 컨트롤러로부터 찾아서 반환
extension RootViewController {
  
  var someViewController: someViewController {
    return children.first { $0 is SomeViewController }
      as! SomeViewController
  }
  
}
```

<br><br>

다만 이 방식으로 뷰 컨트롤러를 테스트에서 사용할 때 유의해야 할 것은 하나의 뷰 컨트롤러를 생성받아 공용으로 사용을 한다는 점이다. 각 테스트 케이스가 실행된 후 컨트롤러의 상태가 다음 테스트 케이스에서도 유지가 되어 영향을 주므로 이에 대해 reset 시켜줘야 한다.

```swift
override func tearDown() {
  // restart 메서드 안에는 테스트에서 사용되는 모든 변수들을 초기화해준다.
  someViewController.restart()

  super.tearDown()
}
```

<br><br>

위와 같은 방식이 번거롭다면 다음과 같이 UIStoryboard를 직접 생성하여 완전히 '새로운 뷰 컨트롤러'를 테스트에 사용하는 방식이 있다.

```swift
var sut: SomeViewController!

override func setUp() {
  super.setUp()
  let storyboard = UIStoryboard(name: "Main", bundle: nil)
  sut = storyboard.instantiateViewcontroller(withIdentifier: "someViewController") as! SomeViewController
}

override func tearDown() {
  sut = nil
  super.tearDown()
}
```

바로 위와 같이 UIStoryboard를 직접 생성해서 그로부터 뷰 컨트롤러를 매 테스트 케이스마다 새로 만듬으로써 각 테스트 케이스는 독립적인 뷰 컨트롤러를 테스트 할 수가 있게 된다.

### TDD Cycle를 잘 준수하고 있을까?
TDD기반으로 개발을 했다고 한들 그 규칙을 깜빡하고 앱 코드를 먼저 작성했다든가 하는 등의 실수로 인해 TDD Cycle 규칙을 어기고 있는지 아니면 잘 준수하고 있는지 확인하는 방법이 있다.
<br>

<p align="center"><img width="928" alt="1" src="https://user-images.githubusercontent.com/73280175/147872590-da78a097-a972-4d55-85fc-081dc3babb92.png"> </p>

<br>

위 처럼 XCode의 Product->Scheme->Edit Scheme->Options 탭 으로 가서 "Code Coverage"를 체크해 준다. 그 후 테스트를 진행하고 Report navigator에 가면 최근 실행한 테스트에 대한 Build, Coverage 그리고 Log가 나온다. Coverage를 클릭해보자.

<br>

<p align="center"><img width="1440" alt="2" src="https://user-images.githubusercontent.com/73280175/147872595-8b49cb17-6b43-4eb8-9944-a9e409c80db9.png"> </p>
                                                                                                                                                   
<br>

모든 테스트에서 앱의 실제 코드가 사용되는 비중이 percentage로 표현되고 있다. 100%에 가까울수록 테스트에서 많은 앱의 코드가 사용되고 있는 것이며, 이는 곧 TDD Cycle을 잘 준수하고 있다는 것을 보여준다.
왜냐하면 TDD Cycle에서는 테스트 코드를 먼저 작성하고 그에 기반한 앱 코드를 작성하기 때문에 이 순서를 꾸준히 잘 지켜간다면 100%에 자연스럽게 가까워지게 된다.
<br><br>
앱 코드의 어느 부분이 테스트가 안되었는지 확인하려면 100%가 아닌 앱 파일을 더블클릭해 보면 알 수 있다. 우측의 빨간색 부분에 마우스를 대면 테스트 되지 않은 코드들을 알 수가 있다. 이런식으로 테스트 되지 않은
앱 코드가 있다면 이에 대해서 테스트 코드를 작성하면 된다!

<br>

<p align="center"><img width="1440" alt="3" src="https://user-images.githubusercontent.com/73280175/147874073-18a70a34-6c21-4449-9971-5ca174c64042.png"></p>

<br>









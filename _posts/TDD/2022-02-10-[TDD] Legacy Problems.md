---
title:  "[TDD] 레거시 문제"
excerpt: "[TDD] 레거시 문제"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - TDD
  - IOS
  - Swift
---

### 레거시 프로젝트의 테스트는 어떻게 진행해야 할까?
유닛 테스트나 문서화가 덜 되어 있거나, 앱이 굉장히 느리거나, 앱의 코드 자체가 이해하기 어려운 구조로 작성되어 있는 레거시 프로젝트에서 TDD를 진행하는 방법의 단계를 보면 아래와 같다.

1. Change Point: 추가하거나 변경해야할 부분을 확인한다. 
2. Test Point: 변경으로 인해 영향받을 부분을 확인한다.
3. 의존성을 푼다.
4. TDD 진행
   - 테스트 코드를 작성한다.
   - 추가하거나 변경하려던 앱 코드를 작성하고 리팩토링을 한다.

일반적인 프로젝트의 TDD와 레거시 프로젝트의 TDD의 차이점은 일반적인 프로젝트의 TDD는 테스트코드에 기반하여 앱 코드를 작성했다면 레거시 프로젝트의 TDD는 앱코드에 기반한 테스트코드를 작성한다.

#### 1. Change Point: 추가하거나 변경해야할 부분을 확인한다. 
레거시 앱에 기능을 추가하거나 기존 기능을 리팩토링하기 위한 테스트 코드를 작성하기 위해서는 앱 코드의 어느 부분을 변경할 것인지 정확히 알아야 한다.
<br>
예를 들어, 변경이 필요한 클래스나 파일을 확인한다. 

#### 2. Test Point: 변경으로 인해 영향받을 부분을 확인한다.
현재 레거시 앱의 동작은 그대로 유지하면서 앱에 기능을 변경하거나 추가하려면 기존 코드에 대한 테스트 코드를 작성해야 한다.
<br>
이를 Characterization Test 라고 하며, 아래는 그 방법이다.

1. 테스트 코드를 작성할 테스트 파일을 만든다.
2. 실패할 만한 테스트 코드를 작성한다.
3. 실패를 통해 앱의 특정 동작을 확인한다.
4. 특정 동작의 결과에 기반하여 테스트 코드를 수정하고 성공시킨다.

#### 3. 의존성을 푼다.
테스트하려는 앱 코드가 특정 요소에 의존적이라서 테스트하기에 까다롭다면 그 요소를 Mocking 하여 일정한 결과를 출력하도록 한다. 
<br>
에를 들어 다음과 같이 앱 코드에 실제로 네트워크 통신을 하는 객체를 반환하는 프로퍼티가 있다면,

```swift
class CalendarViewController: UIViewController {

  var api: API { return (UIApplication.shared.delegate as! AppDelegate).api }
  .
  .
  .
  
}
```

테스트할 때는 mock 객체를 주입받을 수 있도록 변경한다.

```swift
class CalendarViewController: UIViewController {

  var api: API = (UIApplication.shared.delegate as! AppDelegate).api
  .
  .
  .
  
}
```

위처럼 하면 앱이 동작할 때는 그대로 사용하고, 테스트 할때는 아래와 같은 mock 객체를 주입하면 된다. 

```swift
class MockAPI: API {

  var mockEvents: [Event] = []
  
  override func getOrgChart() {
    .
    .
    .
  }
}
```

```swift
class CalendarViewControllerTests: XCTestCase {
  
  var sut: CalendarViewController!
  var mockAPI: MockAPI!
  
  override func setUp() {
    super.setUp()
    sut = UIStoryboard(name: "Main", bundle: nil)
      .instantiateViewController(withIdentifier: "Calendar") as? CalendarViewController
    
    mockAPI = MockAPI()
    sut.api = mockAPI
  }
  
  override func tearDown() {
    mockAPI = nil
    sut = nil
    super.tearDown()
  }
```

#### 4. TDD 진행
이 단계에서 부터는 이제 기존의 TDD를 진행한다. 위의 단계에서 의존성을 풀어냈기 때문에 TDD를 진행할 수 있게 된다.

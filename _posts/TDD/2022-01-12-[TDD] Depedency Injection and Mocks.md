---
title:  "[TDD] Depedency Injection and Mocks"
excerpt: "[TDD] Depedency Injection and Mocks"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - TDD
  - IOS
  - Swift
---

## 앱의 외부적인 요인 테스트
지금까지는 앱의 특정 이벤트 이후, 영향을 받은 값에 대해 테스트를 해왔다. 이번 포스팅에서는 시스템 의존성을 띄는 코드의 테스트나 실제 외부 통신을 하지 않고도 테스트를 하는 방법에 대해 알아보자.

### 실제 객체를 대신해보자: Test Doubles
Test Doubles은 실제 객체 대신 대역인 객체를 만들어 테스트를 하기 위해 사용하는 방법이라고 정의할 수 있다. 그리고 이 방법은 다음과 같이 몇가지로 나뉜다.

- Stub: 객체를 대신하여 미리 만들어진 응답(canned response)을 제공한다. 프로토콜에 정의된 하나의 메서드를 실행하며 빈 값인 nil을 반환한다.

- Fake: 주로 로직을 가지고 있으며 실제 데이터를 제공하기 보다 '테스트 데이터'를 제공한다. 실제 네트워크 연결을 통해서 데이터를 읽거나 쓰지 않고 로컬 Json 파일을 읽고 쓰는 가짜 네트워크 매니저를 사용하게 된다.

- Mock: 실제 객체를 만들기에 비용이 너무 크거나 의존성이 강할때, 테스트를 하기 위해 생성하는 가짜 객체이다. 테스트에 필요한 값과 이벤트를 제공한다.

- Partial mock: Mock이 실제 객체를 100% 모방한 가짜 객체였다면 Partial mock은 그 일부만 override해서 모방한 가짜 객체이다. 


### CMPedometer
유저의 활동 데이터를 얻는 방법은 여러가지가 있지만 그중에서도 CMPedometer API가 가장 사용하기가 쉽다.
<br><br>
여기서 말하는 유저의 활동 데이터란 걷기 횟수나 이동 거리 등을 의미한다.

- CMPedometer API를 사용하려면 CMPedometer 객체가 필요
```swift
import CoreMotion // CMPedometer 객체를 사용하기 위해서 CoreMotion 임포트
.
.
.
let cmPedometer = CMPedometer
```

하지만 이 CMPedometer를 테스트하기에는 무리가 있다.
<br><br>
CMPedometer의 이벤트로 인한 출력 값은 디바이스에 너무 의존적이라서 테스트를 하기에는 테스트 값이 일관성있지가 않다.
<br><br>
이러한 예측 불확실성으로 인해 CMPedometer가 아닌 이에 대한 Mock 객체를 테스트하는 것이 적합하다.

### Mocking
CMPedometer 객체의 mock 객체를 만들어보자. 
<br><br>
CMPedometer의 Mock 객체를 만드려면 Pedometer의 정의부와 구현부를 분리해야 한다.
<br>
그러기 위해서 사용하는 두 가지 패턴이 존재한다.
- Facade: 복잡한 클래스들의 집합에 대한 단순화된 인터페이스를 제공하는 디자인 패턴

- Bridge: 밀접하게 관련된 클래스 집합을 구현 계층과 추상 계층으로 분할할 수 있는 디자인 패턴


### Bridge 과 Fadade 패턴을 사용하여 Pedometer Mock 객체 만들기
<br>

```swift
protocol Pedometer {
  func start()
  
  var pedometerAvailable: Bool { get }
}
```

그리고 CMPedometer가 위에서 정의한 Pedometer 프로토콜을 채택하도록 한다.

```swift
extension CMPedometer: Pedometer {
  var pedometerAvailable: Bool {
    return CMPedometer.isStepCountingAvailable() &&
           CMPedometer.isDistanceAvailable() &&
           CMPedometer.authorizationStatus() != .restricted
  }

  func start() {
    startEventUpdates { CMPedometerEvent?, Error? in
    // 복잡한 코드 작성
      .
      .
      .
    }
  }
}
```

그 후 테스트할 대상 클래스를 다음과 같이 구현한다.

```swift
.
.
class AppModel {
  .
  .
  let pedometer: Pedometer

  // default CMPedometer는 언제든지 Mock 객체로 대채가능
  init(pedometer: Pedometer = CMPedometer()) { 
    self.pedometer = pedomerter 
  }

  func startPedometer() {
    pedometer.start() // 복잡한 코드 X
  }
  .
  .
}
```

그리고 이제 테스트를 위한 Pedometer의 Mock 클래스를 구현해보자.

```swift
import CoreMotion
@testable import Fitness

class MockPedometer: Pedometer {
  private(set) var started: Bool = false
  
  var pedometerAvailable: Bool = true
  
  func start() {
    started = true
  }
}
```

### Mock객채를 이용한 테스트
위에서 만든 Pedometer Mock 클래스를 이용하여 테스트 코드를 작성해보자.

```swift
var sut: AppModel!
var mockPedometer: MockPedometer!

override func setUp() {
  super.setUp()
  mockPedometer = MockPedometer()
  sut = AppModel(pedometer: mockPedometer)
}

override func tearDown() {
  mockPedometer = nil
  sut = nil
  super.tearDown()
}

func testAppModel_whenStarted_startsPedometer() {
  // given
  ......
  
  // when
  sut.startPedometer()
  
  // then
  XCTAssertTrue(mockPedometer.started)
}
```

### 정리: Mock 객체 테스트 패턴
다른 외부적인 요소인 네트워크 API의 결과값에 의존적이거나 위처럼 디바이스에 의존적인 상황에서 Mock 객체를 테스트하는 패턴은 아래처럼 일정하게 할 수 있다.

1. 테스트하고자 하는 대상 클래스와 관련된 프로토콜을 만들어서 그 내부에 사용될 프로퍼티와 메서드를 정의한다.

2. 위에서 만든 프로토콜을 대상 클래스가 채택하여 앱이 작동했을 때 실제로 사용하게 될 내용들을 구현한다

3. Mock: 위에서 만든 프로토콜을 채택하여 테스트 내용을 구현하여 테스트 한다.


### 디바이스가 아닌 시뮬레이터일 경우
시뮬레이터는 유저의 활동 데이터를 수집할 수 있는 기능이 없기 때문에(GPS 없음, 자이로스코프 없음),  
<br><br>
시뮬레이터에서 앱이 실행될 때 이에 대해 동작하도록 가상의 객체를 따로 만들어서 쓸 수 있다.

```swift 
class SimulatorPedometer: Pedometer {
  .
  .
  .
  
  var distance = 0.0
  .
  .
  .
  func start()
    ...
  }
  
  func stop() {
    ...
  } 
}

.
.
.
static var pedometerFactory: (() -> Pedometer) = {
  #if targetEnvironment(simulator)
  return SimulatorPedometer() // 시뮬레이터 사용하는 객체 반환
  else 
  return CMPedometer() // 실제 디바이스에서 사용하는 Pedometer 객체 반환
}
```


### Partial Mock
테스트하고자 하는 클래스가 있는데 특정 메서드가 불렸을 때,
<br><br>
원래 메서드의 내용이 아닌 다른 내용을 테스트 하고 싶다면 Partial Mock을 사용할 수 있다.
<br><br>

아래처럼 테스트하고자 하는 클래스가 있고, updaState 메서드의 내용을 다르게 해서 테스트 하고 싶다면

```swift 
IBDesignable
class ChaseView: UIView {
  let nessieView = UIImageView()
  let runnerView = UIImageView()

  private var runnerComplete: CGFloat = 0
  private var nessieComplete: CGFloat = 0

  var state: AppState = .notStarted {
    didSet {
      nessieView.image = state.nessieImage
      runnerView.image = state.runnerImage
    }
  }
  
  required init?(coder aDecoder: NSCoder) {
    super.init(coder: aDecoder)
    ...
  }

  override init(frame: CGRect) {
    super.init(frame: frame)
    ...
  }

  func updateState(runner: Double, nessie: Double) {
    runnerComplete = CGFloat(runner)
    nessieComplete = CGFloat(nessie)
    setNeedsLayout()
  }
}
```

<br>
위 클래스의 Partial mcok 클래스를 만들면 다음과 같다.

```swift
@testable import FitNess

class ChaseViewPartialMock: ChaseView {
  var updateStateCalled = false
  var lastRunner: Double?
  var lastNessie: Double?
  
  override func updateState(runner: Double, nessie: Double) {
    updateStateCalled = true
    lastRunner = runner
    lastNessie = nessie
    super.updateState(runner: runner, nessie: nessie)
  }
}
```





   







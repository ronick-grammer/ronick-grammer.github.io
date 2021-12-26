---
title:  "[TDD] TDD Cycle"
excerpt: "[TDD] TDD Cycle"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - TDD
  - IOS
  - Swift
---

## TDD Cycle
저번 포스트에서는 [TDD가 무엇인가](https://ronick-grammer.github.io/tdd/ios/swift/TDD-TDD%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80/)에 대해서 다루었다. 오늘은 TDD를 직접
구현해가면서 TDD Cycle이 무엇인지 그리고 이에 맞추어 테스트 코드를 작성해 보도록 한다. 우선 TDD Cycle에 대해 간단히 보고 넘아가도록 하자.

<br><br>
<p align="center"><img width="50%" alt="TDD Cycle" src="https://user-images.githubusercontent.com/73280175/147403508-5945fa00-2d5e-4aa4-bebc-dd55b4f9c9cb.png">
</p>
<br>

전 포스트에서 세가지 단계에 대해서 설명을 했지만 TDD Cycle 역시 유사하다. TDD Cycle은 세가지 단계를 가지고 이를 반복하는 형태이다. 참고로 Red는 테스트 코드의 성공, Green은 성공을 의미한다.

- Red: 실패하는 테스트 코드를 작성한다. 여기서 중요한 건 컴파일 실패도 실패하는 테스트 코드에 속한다. 
<br>

- Green: 이후에 테스트에 통과하는 프로덕션 코드를 작성한다. 유의해야할 것은 테스트를 통과하기 위한 '최소한의 코드만' 작성해서 최소한을 넘어선 코드 작성으로 인해 발생할 수 있는 불필요한 테스트 실패를 방지한다.
<br>

- Refector: 마지막으로 코드를 개선하는 단계이다. 중복적으로 사용되는 프로퍼티나 메서드를 제거해서 하나만 공통적으로 사용하게 하고 복잡한 로직의 코드를 단순화 시키거나 어려운 네이밍을 개선하도록 한다. 
또한 주석을 달 때에도 해당 코드가 어떻게 동작하는지에 대해서가 아니라 왜 해당 코드가 작성되었는지에 대해서 설명하도록 한다. 

## 테스트의 세분화
TDD Cycle 에 대해서 알고 이제 TDD가 전체적으로 어떤 방식인지는 알았는데, 각 테스트는 어떻게 이루어지도록 코드를 작성해야 할까? 우선 우리는 세가지 상황이 필요하다.

- Given: 초기값이다. 어떤 값이 주어지고
- When: 어떠한 동작(이벤트)이 발생할 때,
- Then: 예상되는 특정한 결과값을 도출한다

예를 들자면
- 현금 인출기가 주어지고(Given)
- 사용자가 현금인출기를 통해서 입급을 했을 때(When)
- 현금인출기의 잔액은 0000원 된다(Then)

이를 좀더 Programmatic 하게 해석하면
- 현금 인출기 Class 인스턴스가 주어지고(Given)
- 현금 인출기 Class 인스턴스의 deposit(입금) 메서드를 호출했을 때(When)
- 현금 인출기 Class 인스턴스의 availableFunds 프로퍼티는 0000이 된다(Then)

자, 이제 구현을 시작해 보자!

## TDD 구현
여기서는 Test Target을 생성하지 않고, Playground에서 작성한다는 것을 참고 바란다.
우선 테스트 코드를 작성하기 위해 XCTest 클래스를 상속해서 테스트 클래스를 만들어야 한다. XCTestCase는 XCTest 프레임워크를 import해야 사용할 수 있다. XCTestCase를 상속하는 클래스는 내부의 메서드들을
각각 하나의 테스트 케이스로 정의해서 테스트를 구현할 수 있다.
<br><br>

위의 TDD Cycle에서 가장 첫 번째 단계인 'Red'는 실패하는 코드를 의도적으로 작성하는 것이었다. 현금인출기를 만드는 것을 실패하도록 하는 코드를 작성하자.

```swift

class CashRegisterTests: XCTestCase {

  func testInitAvailableFunds_setsAvailableFunds() { // 실패하는 테스트
    let availableFunds = Decimal(100)
    let sut = CashRegister(availableFunds: availableFunds)
  
    XCTAssertEqual(sut.availableFunds, availableFunds)
  }
  
}

CashRegisterTests.defaultTestSuite.run() // 모든 테스트 케이스(메서드) 실행

```

XCTAssertEqual은 테스트하려는 값이 예상하는 값과 같은지 확인하는 메서드이다. 실행을 해보면 알겠지만, 아니 실행하기도 전에 실패할 것이란 것을 알 수 있다. 왜냐하면 CashRegister 클래스를 정의하지 않았기
떄문이다. 이처럼 TDD에서는 앱의 구현코드인 프로덕션 코드를 먼저 작성하지 않고 테스트코드를 먼저 작성함으로써 테스트코드에 기초하는 프로덕션 코드를 작성해나가는 개발방식이다. 
<br><br>
여기서 중요하게 보아야 할 것은 바로 테스트클래스의 이름과 내부의 테스트케이스인 메서드의 이름이다. 전체적으로 어떤 테스트를 하는 것인지 클래스 이름을 짓고, 그 내부에서 세부적으로 어떤 유닛 테스트를 하는지 쉽게 이해가
가능하도록 테스트 케이스의 이름을 잘 짓는 것은 매우 중요한 포인트이다.
<br><br>
돌아와서 "컴파일 에러나는데 이걸 테스트 실패라고 할수 있나?" 라고 궁금해할 수 있다. 답은 "그렇다"이다. 프로덕션 코드의 부재로 인한 테스트 코드의 실패는 당연히 컴파일 에러로 이어진다. 그리고 이러한 컴파일 에러 역시 TDD의
'Red' 단계에 포함된다. 이제 이러한 프로덕션 코드의 부재를 채워서 테스트를 성공시키는 단계인 'Green'단계로 진입하도록 하자.

```swift

class CashRegister {
  var availableFunds: Decimal
  
  init(availableFunds: Decimal) {
    self.availableFunds = availableFunds
  }

}

class CashRegisterTests: XCTestCase {

  func testInitAvailableFunds_setsAvailableFunds() { // 성공
    let availableFunds = Decimal(100)
    let sut = CashRegister(availableFunds: availableFunds)
    
    XCTAssertEqual(sut.availableFunds, availableFunds)
  }
  
}

CashRegisterTests.defaultTestSuite.run() // 모든 테스트 케이스(메서드) 실행

```

이후에 실행을 시키면 테스트가 통과하게 된다. TDD Cycle의 Green 단계를 완료하였다. 자 이제 마지막 단계인 Refectoring으로 진입하려는데 위의 코드에서는 더이상 코드를 손을 볼 곳이 없다.
해당 테스트 케이스는 완료되었다. 그럼 다음 테스트 케이스로 넘어가 보도록하자. TDD Cycle은 각 기능이 추가될때 마다 반복되는 테스트 싸이클이기 때문에 다시 'Red'단계부터 시작한다.
이번에는 사용자가 현금 인출기를 통해 돈을 입금했을 때를 테스트하는 코드를 작성해보자.


'Red' 단계: 실패하는 테스트 코드 작성

```swift

class CashRegister {
  var availableFunds: Decimal

  init(availableFunds: Decimal) {
    self.availableFunds = availableFunds
  }

}

class CashRegisterTests: XCTestCase {
  
  func testInitAvailableFunds_setsAvailableFunds() {
    let availableFunds = Decimal(100)
    let sut = CashRegister(availableFunds: availableFunds)
    
    XCTAssertEqual(sut.availableFunds, availableFunds)
  }
  
  func testAddItem_oneItem_addsCostToTransactionTotal() { // 실패하는 테스트 케이스. 
    
    // given
    let availableFunds = Decimal(100)
    let sut = CashRegister(availableFunds: availableFunds)
    let itemCost = Decimal(42)
    
    // when
    sut.addItem(itemCost)
    
    // then
    XCTAssertEqual(sut.transactionTotal, itemCost)
  }
 
}

CashRegisterTests.defaultTestSuite.run() // 모든 테스트 케이스(메서드) 실행

```

위의 코드는 addItem 메서드와 transactionTotal 프로퍼티가 정의되지 않았으므로 테스트 케이스는 실패한다. 'Green'단계로 진입하여 테스트코드가 성공하도록 하자.

```swift

class CashRegister {
  var availableFunds: Decimal
  var transactionTotal: Decimal = 0
  
  init(availableFunds: Decimal) {
    self.availableFunds = availableFunds
  }
  
  func addItem(_ cost: Decimal) {
    transactionTotal += cost
    availableFunds += cost
  }

}

class CashRegisterTests: XCTestCase {
  
  func testInitAvailableFunds_setsAvailableFunds() {
    let availableFunds = Decimal(100)
    let sut = CashRegister(availableFunds: availableFunds)
    
    XCTAssertEqual(sut.availableFunds, availableFunds)
  }
  
  func testAddItem_oneItem_addsCostToTransactionTotal() { // 성공하는 테스트 
    
    // given
    let availableFunds = Decimal(100)
    let sut = CashRegister(availableFunds: availableFunds)
    let itemCost = Decimal(42)
    
    // when
    sut.addItem(itemCost)
    
    // then
    XCTAssertEqual(sut.transactionTotal, itemCost)
  }
 
}

CashRegisterTests.defaultTestSuite.run() // 모든 테스트 케이스(메서드) 실행

```

자, 위의 테스트 코드가 성공하도록 했으므로 'Green' 단계를 완료하였다. Refectoring 단계로 진입할 차례인데, Refectoring을 할 만한 포인트를 찾을 수 있을까? 우선 그전에 TDD 에서 Refactoring은 
여러가지가 있지만 여기서는 중복되는 메서드나 프로퍼티의 사용이 있을 때 이를 개선하여 공통으로 하나만 사용하도록 한다. 위 코드에서는 testInitAvailableFunds_setsAvailableFunds 메서드와 
testAddItem_oneItem_addsCostToTransactionTotal 메서드에서 availableFunds와 sut이 중복적으로 사용되고 있다. 이에 대해서 Refectoring을 진행해보도록 하자.

```swift

import Foundation
import XCTest

class CashRegister {
  var availableFunds: Decimal
  var transactionTotal: Decimal = 0
  
  init(availableFunds: Decimal) {
    self.availableFunds = availableFunds
  }
  
  func addItem(_ cost: Decimal) {
    transactionTotal += cost
  }
}

class CashRegisterTests: XCTestCase {
  
  // 공통으로 사용하는 프로퍼티
  var availableFunds: Decimal! 
  var sut: CashRegister!
  
  override func setUp() { // 이곳에서 초기화, 각 테스트 케이스 실행전 호출
    super.setUp()
    availableFunds = 100
    sut = CashRegister(availableFunds: availableFunds)
  }
  
  override func tearDown() { // 이곳에서 해제, 각 테스트 케이스 종료후 호출
    availableFunds = nil
    sut = nil
    super.tearDown()
  }
  
  func testInitAvailableFunds_setsAvailableFunds() {
    XCTAssertEqual(sut.availableFunds, availableFunds)
  }
  
  func testAddItem_oneItem_addsCostToTransactionTotal() {
    
    // given
    let itemCost: Decimal = 42
    
    // when
    sut.addItem(itemCost)
    
    // then
    XCTAssertEqual(sut.transactionTotal, itemCost)
  }
    
}

CashRegisterTests.defaultTestSuite.run()

```

?? 갑자기 setUp 메서드는 뭐고 tearDown 메서드는 뭐일까? 중복으로 사용되는 변수들을 하나로 공통 사용하도록 한다고 했는데 이들을 초기화 해주고 해제해주는 메서드이다. 이 두 메서드는 XCTestCase를 상속하면 사용할
수 있는 메서드들이다.

- setUp(): 각 테스트 메서드가 실행되기 전에 호출되며 이곳에서 중복적으로 사용되던 프로퍼티의 하나로서 공통 사용을 위해 초기화 하거나 중복적인 로직을 수행한다.
- tearDown(): 각 테스트 메서드가 실행되고 난 후에 호출되며, setUp에서 초기화 해줬던 프로퍼티를 nil로 만들어 메모리에서 오랫동안 자리잡고 있지 않도록 해 퍼포먼스에 악영향을 끼치지 않도록 한다.
super.tearDown()은 모든 프로퍼티를 nil로 설정 후 넣는다.

사실 tearDown()에서 nil을 넣어주는 이유를 자세하게 이해하지는 못하였다.(피드백 환영합니당)

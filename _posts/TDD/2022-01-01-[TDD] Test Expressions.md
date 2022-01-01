---
title:  "[TDD] TDD Cycle"
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
  
  func test_whenNoAssert_thenSucceed() { // 코드는 있지만 assert 함수가 없음, 그래도 성공
    // when
    sut.test()
    
    // then 없음..
  }
  
  func test_whenNothing_thenSucceed { // 그냥 다 없음, 그래도 성공
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









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
오늘은 특정 메서드 호출로 인한 값의 직접적인 변화에 대한 테스트가 아니라 비동기 처리를 위한 클로저의 실행이 원활하게 되는지 테스트하는 방법에 대해서 알아보도록 하자.
우선 이를 위한 두가지를 알아야한다.

- expectation: 클로저가 성공적으로 실행되면 fullfill이 되어 성공했다는 신호를 wait에게 알리게 된다.
- wait: expectation이 fullfill 될때 까지 기다리거나 특정 시간만큼 기다린다.

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

위 코드에서 보이듯이 pause() 이벤트가 발생하여 상태값이 바뀌면 stateChangedCallback 클로저가 실행된다. wait는 expected가 fulfill()을 호출할 때까지 기다리고 있다가
fulfill 상태가 되거나 1초가 지나면 다음 라인을 실행하게 된다.











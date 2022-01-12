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

### 실제 객체를 대신해보자: Test Double
Test Double은 실제 객체 대신 대역인 객체를 만들어 테스트를 하기 위해 사용하는 방법이라고 정의할 수 있다. 그리고 이 방법은 다음과 같이 몇가지로 나뉜다.

- Stub: 객체를 대신하여 미리 만들어진 응답(canned response)을 제공한다. 프로토콜에 정의된 하나의 메서드를 실행하며 빈 값인 nil을 반환한다.

- Fake: 주로 로직을 가지고 있으며 실제 데이터를 제공하기 보다 '테스트 데이터'를 제공한다. 실제 네트워크 연결을 통해서 데이터를 읽거나 쓰지 않고 로컬 Json 파일을 읽고 쓰는 가짜 네트워크 매니저를 사용하게 된다.




   







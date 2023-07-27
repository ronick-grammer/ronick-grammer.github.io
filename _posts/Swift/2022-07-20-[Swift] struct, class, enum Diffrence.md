---
title:  "[Swift] struct, class, enum 차이"
excerpt: "[Swift] struct, class, enum 차이"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Swift
---

### struct
- 값 타입
- 상속 지원 X
- 스택(Stack)에 할당

### class
- 참조 타입
- 상속 지원 O
- 힙(Heap)에 할당
- ARC로 부터 관리받음

### enum
- 값 타입
- 상속 지원 X
- 스택(Stack)에 할당

### struct, class, enum 의 차이점
값(value) 타입인 struct와 enum은 원본 인스턴스를 복사하거나 메서드에 인자로 전달할 경우 인스턴스 값을 가진 새로운 복사본 인스턴스가 Stack에 할당된다. 따라서 어떤 하나의 인스턴스의 값을 변경해도 다른 복사본들에게는 영향을 미치지 않는다.

반면, 참조(refrence) 타입인 class는 원본 인스턴스를 대입하거나 메서드에 인자로 전달할 경우 원본 인스턴스가 할당된 Heap 메모리 위치(주소)에 대한 참조체가 전달된다.

결론적으로, 값 타입은 그 모든 데이터들이 Stack 메모리 구조상 위에 바로 할당되고(push), 해제시에 가장 위의 데이터가 메모리에서 해제(pop)되는 후입선출 구조이기에 매우 빠르다. 참조 타입은 그 데이터들이 Heap 메모리에 할당되는데 이 과정에서 Heap에 할당할 만한 크기의 메모리 공간을 계산하고 할당해야하며, 할당한 이후에는 이 할당된 Heap 메모리상 주소에 대한 정보를 Stack에 또 할당해야 한다. 그리고 이 참조 타입 데이터를 사용할 때 Stack 메모리에 할당된 Heap 메모리상 주소 데이터를 찾아 이 Heap 메모리상 주소에 할당된 나머지 데이터를 찾아야하므로 비용이 값타입보다 크다.

### class 보다 struct를 사용하는게 좋은 경우

- 연관된 간단한 값들을 캡슐화하는 것이 목적일 경우

- 값을 참조하는게 아닌 복사하는 것이 합당할 경우

- 다른 타입으로부터 상속을 받거나, 자신을 상속할 필요가 없는 경우

- Objective-C 를 사용하는 컴포넌트 관련 작업을 해야하는 것이 아닌 경우


### 📝 참고 사이트

- [Struct vs. Class in Swift](https://www.linkedin.com/pulse/struct-vs-class-swift-muhammad-asad-chattha/)
- [Struct vs Class (Understanding Swift Performance)](https://medium.com/@linhairui19/struct-vs-class-understanding-swift-performance-part-1-f46a6811c8f2)
- [Choosing Between Structures and Classes](https://developer.apple.com/documentation/swift/choosing-between-structures-and-classes)

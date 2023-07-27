---
title:  "[Swift] class 성능 최적화"
excerpt: "[Swift] class 성능 최적화"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Swift
---

### Dispatch
메서드를 호출할 때, 어떤 메서드(or 프로퍼티)를 호출할지 결정하는 메커니즘

- `Static Dispatch`: 컴파일 시점에 어떤 메서드(or 프로퍼티) 를 호출할지 결정한다. 값 타입인 struct, enum 에서는 메서드 호출시 Static Dispatch를 통해서 즉시 메서드를 호출한다(`Direct Call`). Dynamic Dispatch가 동작할 때보다 성능적 이점이 있다.

- `Dynamic Dispatch`: 런타임 중에 어떤 메서드(or 프로퍼티)를 호출할지 결정한다. 때문에 성능적 오버헤드가 따른다. vTable(Virtual Table)에서 호출해야하는 정확한 메서드를 런타임중에 찾게된다. 참조 타입인 class 에서 메서드 호출시 Dynamic Dispatch 를 통해 vTable에서 호출해야하는 메서드의 메모리 주소를 찾아 호출한다(`Indirect Call`). class는 상속이 가능하고 따라서 override 역시 가능하기에 상속관계에 있는 subclass가 어떤 메서드를 호출해야하는지 결정해야하기 때문이다.
*(`설령 superclass 를 상속하는 subclass 가 없거나 override 하는 메서드(or 프로퍼티)가 없다고 하더라도 그 가능성으로 인해 class 는 Dynamic Dispatch 메커니즘에 메서드(프로퍼티) 호출이 결정된다`)

### class에서 Static Dispatch 가 실행되도록 하는 방법

#### `final` 키워드 사용
- class 선언부에 `final` 키워드를 사용하여 class 내부의 모든 메서드, 프로퍼티가 상속관계에서 오버라이드가 불가능함을 컴파일러에게 알려 Static Dispatch가 동작하도록 한다.

- class 내부에서 선택적으로 메서드, 프로퍼티에 `final`을 사용하여 이들이 상속관계에서 오버라이드가 불가능함을 컴파일러에게 알려 Static Dispatch가 동작하도록 한다.

#### `private` 키워드 사용
- class 선언부에 `private` 키워드를 사용하여 class 내부의 모든 메서드, 프로퍼티가 상속관계에서 오버라이드되지 않을 경우 컴파일러가 `final` 로 자동 추론하여 Static Dispatch가 동작하도록 한다.

- class 내부에 선택적으로 메서드, 프로퍼티에 `private`를 사용하고 이들이 상속관계에서 오버라이드 되지 않을 경우 컴파일러가 `final`로 자동 추론하여 Static Dispatch가 동작하도록 한다.

#### WMO(Whole Module Optimization) 사용
- Xocde 프로젝트 세팅에서 `compilation mode`를 `Whole Module` 로 설정(기본으로 설정되어 있음)

- 일반적으로 컴파일러는 파일단위로 컴파일을 하여 한 파일의 클래스를 다른 파일의 클래스가 상속을 할때 오버라이드하는 메서드(or 프로퍼티)가 없으면 `final`로 자동 추론을 할 수가 없다. 하지만 WMO 로 설정되어 있으면 모듈단위로 컴파일하여 각 다른 파일에 존재하는 클래스들간의 상속관계에서 오버라이드가 없을 경우 `final`로 자동 추론이 가능하게 되어 Static Dispatch 가 동작되도록 한다.


### 📝 참고 사이트
- [Increasing Performance by Reducing Dynamic Dispatch](https://developer.apple.com/swift/blog/?id=27)
- [Dispatch를 이용한 성능 최적화](https://babbab2.tistory.com/145)
- [9 Ways to Boost Your Swift Code Performance](https://www.linkedin.com/pulse/9-ways-boost-your-swift-code-performance-avi-tsadok/)

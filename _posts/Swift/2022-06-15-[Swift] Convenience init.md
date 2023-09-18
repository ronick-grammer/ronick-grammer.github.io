---
title:  "[Swift] Convenience init"
excerpt: "[Swift] Convenience init"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Swift
---

### Designated Initializer (지정 생성자)
- 클래스를 생성하기 위한 기본 생성자이다.
- 클래스 내부의 모든 프로퍼티들을 초기화한다.
- 상속관계에 있다면 부모클래스의 모든 프로퍼티들 역시 초기화해야 한다.

### Convenience Initializer (편의 생성자)
- 클래스를 쉽게 생성하기 위한 생성자이다.
- 클래스를 생성시 모든 프로퍼티가 아닌 일부에 대해서만 초기화를 하고 나머지 프로퍼티는 `default` 값으로 초기화를 시켜줄 수 있다.
- `Designated Initializer`를 호출해야 한다.

### Initializer Delegation
클래스가 상속관계에 있을때 or 클래스가 `Designated Initializer`와 `Convenience Initializer` 둘다 가지고 있을 때 생성자가 또다른 생성자를 호출하는 방식이다. `Initializer Delegation` 호출을 위한 룰은 아래와 같다.

- Rule 1: 상속관계에 있을때 `Designated initializer`는 부모클래스의 `Designated initializer` 를 호출해야 한다.
- Rule 2: `Convenience Initializer`는 같은 클래스에 정의된 다른 생성자를 호출해야한다.
- Rule 3: `Convenience Initializer`는 결국에는 `Designated Initializer`를 호출해야한다.

### Two-Phase Initialization: 클래스 생성 과정
Swift에서는 클래스를 생성하는 2단계 과정이 존재한다.

- 1단계: 클래스 내부에 선언된 프로퍼티가 초기화된다.
    - 1:  `Designated` 혹은 `Convenience Initializer` 가 호출된다.
    
    - 2: 클래스를 위한 힙 메모리가 할당된다. 아직 메모리에 데이터가 초기화된 상태는 아니다.
    
    - 3: `Initializer Delegation`으로 인해 마지막에 `Designated Initializer`에서 클래스의 모든 프로퍼티들이 초기화된다. 메모리에 데이터가 초기화된 상태이다.
    
    - 4: 상속관계에 있을경우 부모 클래스의 생성자를 호출함으로서 `Initializer Delegation`을 수행한다. 
    
    - 5: 위 과정을 최상위 부모 클래스의 `Designated Initializer` 가 호출될 때까지 진행한다.
    
    - 6: 최상위 부모 클래스의 `Designated Initializer`가 호출됨으로서 상속관계에 있는 모든 클래스의 프로퍼티들이 초기화되었고, 메모리에 모든 데이터 역시 초기화 된 상태가 된다.

- 2단계: 클래스 내부의 모든 프로퍼티가 초기화 된 후, 이 프로퍼티들을 접근/수정 할 수 있다.
  - 1: 최상위 부모 클래스의 `Designated Initializer`가 호출 된 후에야 `Designated Initializer` 내에서 클래스의 프로퍼티와 메서드, `self`에 접근할 수 있게 되고, 차례로 바로 아래의 자식 클래스 `Designated Initializer` 내에서 클래스의 프로퍼티와 메서드에 접근할 수 있게 된다.
  
  - 2:  마지막으로 모든 `Convenience Initializer` 내에서 클래스의 프로퍼티와 메서드, `self`에 접근할 수 있게 된다.
  
  
### 클래스 생성자 Safety-Check
Swift는 Two-Phase Initialization 을 에러없이 안전하게 수행하기 위한 안전 방침을 따른다.

- **Safety Check 1**: 상속관계에서 자식 클래스의 `Designated Initializer` 는 부모 클래스의 생성자를 호출(`Initializer Delegation`)하기 전에  해당 자식 클래스의 모든 프로퍼티들이 초기화가 되어 있어야 한다.

- **Safety Check 2**:자식 클래스의 `Designated Initializer`는 부모 클래스의 생성자를 호출하기 전에 부모 클래스로 부터 상속받은 프로퍼티를 초기화 해서는 안된다. 상속받은 프로퍼티를 자식 클래스의 `Designated Initializer`에서 사전에 초기화를 진행해버리면 이후에 부모 클래스의 생성자에서 한번더 초기화를 시도하기 때문에 의도치 않은 값이 담기게 된다.

- **Safety Check 3**: `Convenience Initializer`는 클래스의 다른 생성자를 호출하기 전에 프로퍼티를 초기화해서는 안된다. 그렇지 않으면 이후에 호출되는 `Designated Initializer`에 의해 의도치 않은 값이 담기게 된다.

- **Safety Check 4**: 모든 생성자는 상속관계에 있는 모든 클래스들의 모든 프로퍼티가 초기화 되기 전까지는 모든 프로퍼티, 메서드 그리고 `self`에 접근/사용할 수 없다.

  
### 📝 참고 사이트
- [Initialization](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/initialization/#Two-Phase-Initialization)

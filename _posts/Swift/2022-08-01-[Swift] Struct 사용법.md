---
title:  "[Swift] Struct 사용법"
excerpt: "[Swift] Struct 사용법"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Swift
---

### Struct란
- 여러 다른 타입의 데이터들을 담을 수 있는 데이터 타입이다. 
- 값 타입이며, 힙(heap)이 아닌 스택(stack) 에 데이터가 할당된다.

### Struct 정의
```swift
struct Person {
    var name: String
    var age: Int
    
     func greet() {
        print("Hello, I'm \(name) and I'm \(age) years old.")
    }
```
#### struct 메서드 내에서 프로퍼티를 변경하고자 할때에는 `mutating` 키워드를 사용한다.
```swift
mutating func greet() {
     self.name = "이름"
}
```

### 참조타입이 아닌 값 타입을 사용해야 하는 경우
하나의 인스턴스를 공유해야 하는 경우가 아니라 각각의 인스턴스를 사용해야 하는 것이 합당하면 `struct` 를 사용한다.
아래의 예시는 참조타입인 `class` 였다면 하나의 인스턴스를 공유하지만, `struct` 이기에 값 복사가 이루어져 다른 인스턴스를 사용하게 된다.

```swift
// create an instance of Person
var person1 = Person(name: "Alice", age: 25)

// create another instance by copying person1
var person2 = person1

// modify person2's name
person2.name = "Bob"

// print person1's name
print(person1.name) // Alice

// print person2's name
print(person2.name) // Bob
```

### Swift 에서 Struct를 사용해야만 하는 주요 이유
Swift 에서 `struct` 는 `class` 가 가지는 거의 모든 기능을 제공하고 있다.
- 생성자
- 프로퍼티와 메서드
- extension
- Protocol 채택 

하지만, `class` 가 가지는 OOP 로서의 중요한 **'상속'** 기능을 지원하고 있지는 않다. 상속이 OOP 관점에서 중요한 이유는 부모 클래스로부터 물려받는 메서드와 프로퍼티들이 재사용 가능하며, 이들을 `overriding` 하여 다형성을 구현할 수 있기 때문이다. 

#### struct로 구현하는 다형성과 POP(Protocol Oriented Programming)
Swift 언어가 공식적으로 지향하는 프로그래밍 패러다임이 있는데 이가 바로 **'프로토콜 지향 프로그래밍(POP)'** 이다. 
POP 는 참조타입은 `class` 보다 값타입인 `struct` 를 지향하는 프로그래밍 패러다임이다. 참조 타입으로부터의 오류 최소화와 자요로움을 추구한다. POP의 특징은 아래와 같다.

- 코드의 중복을 최소화 한다. 
- 상속과 달리 필요한 것만 골라서 사용할수 있으며 참조추적 비용이 없어 가볍다
- 값 타입도 상속처럼 공통된 기능을  쉽게 구현할 수 있다
- Class 는 하나의 상속만 가능하고 수직적인 구조이지만 Protocol을 사용하면 수평적인 확장이 가능해진다
- generic을 활용하여 자료형에 구애받지 않는 범용 코드를 작성할 수 있다.

POP를 적극 활용하면 상속을 지원하지 않는 `struct`에서도 다형성을 충분히 구현할 수 있게 된다.

```swift
// define a protocol for vehicles
protocol Vehicle {
    // define some properties
    var currentSpeed: Double { get set }

    // define some methods
    func start()
    func stop()
}

extension Vehicle {
    func start() {
        print("Starting...")
    }

    func stop() {
        print("Stopping...")
    }
}

struct Car: Vehicle {
    var currentSpeed = 0.0

    mutating func stop() {
        print("Stopping...(overriding)")
        currentSpeed = 0
    }

    func honk() {
        print("Beep beep!")
    }
}

var car = Car()

car1.start() // "Starting..."
car1.stop() // "Stopping...(overriding)"

car1.honk() // "Beep beep!"
```



### 📝 참고 사이트
- [Why You Should Use Structs to Model Data and Logic in Swift](https://blog.devgenius.io/why-you-should-use-structs-to-model-data-and-logic-in-swift-5b24523daf5f)
- [Choosing Between Structures and Classes](https://developer.apple.com/documentation/swift/choosing-between-structures-and-classes)

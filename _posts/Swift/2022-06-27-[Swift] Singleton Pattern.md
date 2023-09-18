---
title:  "[Swift] Singleton Pattern"
excerpt: "[Swift] Singleton Pattern"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Swift
---

### Singleton 패턴이란
- 애플리케이션에서 단 ‘하나’의 인스턴스만 생성해서 사용하는 디자인 패턴이다. 
- iOS 에서는 `URLSession.shared`, `UserDefaults.standard` , `FileManager.default` 등이 싱글톤 패턴 기반으로 만들어져 있다.

#### Singleton 객체 생성방법
`static let` 을 사용하고, 클래스 내부에 pirvate init 을 해서 외부에서 생성할 수 없도록 한다.

```swift
class Singleton {
    static let shared = Singleton()
    
    private init() {}
}
```

#### Swift에서의 Singleton 객체
- 타 언어에서 일반적으로 Singleton 객체는 멀티 스레드 환경에 의해 여러 Singleton 객체가 생성될 수 있어 thread-safe 하지 않지만, Swift 에서는 `dispatch_once`가 자동 적용되기 때문에 단 하나의 싱글톤 객체만이 생성되어 thread-safe 하다.

- Swift 에서 Singleton 객체는 `static` 키워드로 선언될 경우 `lazy` 속성을 그대로 물려받게 됨으로 용할 때 최초로 객체가 생성이 된다. 이는 멀티 스레드 환경에서 동시에 접근되더라도 하나의 객체가 생성됨을 보장한다.

***`dispatch_once`: App 라이프 사이클에서 단 한번만 실행되도록 보장해 주는 것을 의미하며 Thread Safe을 보장 받기 위해 사용한다.**

#### Singleton 패턴의 장점
- 메모리에 한번만 할당되기 때문에 메모리 낭비를 줄일 수 있다.
- 전역적으로 접근하여 사용하기 때문에 데이터 공유와 수정이 쉽다.

#### Singleton 패턴의 장점
- `private init` 을 통해 객체 생성을 막음으로서 Mock 객체를 만들 수 없어 테스트하기에 용이하지 않다
-  **S**OLID 원칙중 하나인 **SRP**(Single Responsibility Principle), 이하 **"하나의 객체는 하나의 책임을 가져야한다."** 는 원칙을 위반한다. 여러 객체에서 전역적으로 Singleton 객체를 사용하기에 Singleton 객체를 사용하는 여러 객체에 의해 여러 책임을 가질 수 있다.


### 📝 참고 사이트
- [Managing a Shared Resource Using a Singleton](https://developer.apple.com/documentation/swift/managing-a-shared-resource-using-a-singleton#Overview)
- [Swift) 싱글톤 패턴(Singleton Pattern)](https://babbab2.tistory.com/66)
- [Singleton Class in Swift](https://medium.com/@nimjea/singleton-class-in-swift-17eef2d01d88)
- [Swift: Singleton, 싱글톤 패턴](https://medium.com/hcleedev/swift-singleton-%EC%8B%B1%EA%B8%80%ED%86%A4-%ED%8C%A8%ED%84%B4-b84cfe57c541)
- [[iOS - Swift] 스위프트에서의 singleton 싱글톤 동작 이해하기 (lazy, thread safe)](https://ios-development.tistory.com/1211)
-[[iOS] 싱글턴패턴(Singleton Pattern) 구현하기 (in Objective-C, Swift)](https://lxxyeon.tistory.com/101#:~:text=Concurrency%20%EC%B2%98%EB%A6%AC%20%EB%B0%A9%EB%B2%95-,Q.,%EB%B3%B4%EC%9E%A5%20%EB%B0%9B%EA%B8%B0%20%EC%9C%84%ED%95%B4%20%EC%82%AC%EC%9A%A9%ED%95%A9%EB%8B%88%EB%8B%A4.)
- [The Singleton Design Pattern](https://devdeejay.medium.com/the-singleton-design-pattern-12c04c8d84e)

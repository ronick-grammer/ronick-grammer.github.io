---
title:  "[Swift] Optional"
excerpt: "[Swift] Optional"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Swift
---
### Optional 이란
- 래핑(wrapping)된 값 혹은 값의 부재(`nil`)를 나타내는 enum타입이다.
  - `case some(value)` : 값이 존재하는 경우의 associated value를 래핑한 케이스
  - `case none`: 값이 존재하지 않는 경우의 케이스

### Optional 타입 선언 방식
- `let value : type?` 혹은 `let value: Optional<type>`

### Optional Binding
- Swift 가 제공하는 몇가지 Optional Bidning 을 통해서 옵셔널 값을 언래핑(unwrapping)할 수 있다.

  - ***if문 (블록 내에서만 사용 가능)***
   ```swift
   if let unwrapped = optional 변수 { ... }
   ```
  - ***guard 문***
   ```swift 
    guard let unwrapped = optional 변수 else { ... return }
   ```
  - ***switch-case 문 (각 case 문 안에서만 사용 가능)***
   ```swift 
    switch optional 변수 {
       case let unwrapped:
       ....
     }
   ```
   
### Optional Chaining
- 연쇄적으로 옵셔널 변수의 프로퍼티와 메서드에 안전하게 접근할 수 있는 방법이다.
 ``` swift 
 wrapped?.method()?.property
 ```

### Nil-Coalescing Operator (병합 연산자)
- 옵셔널 변수가 nil 일 경우 default 로 값을 지정해줄 수 있다.
 ``` swift 
 wrapped ?? value
 ```
 
### 강제 Unwrapping
- 옵셔널 변수가 값을 가지고 있다는 확신하에 강제적으로 옵셔널 변수를 unwrapping 하는 방법이다.
- 옵셔널 변수가 nil일 경우 강제 언래핑을 진행하면 크래시가 발생한다.

``` swift 
wrapped!
```

### 📝 참고 사이트
- [Optional](https://developer.apple.com/documentation/swift/optional#Using-the-Nil-Coalescing-Operator)

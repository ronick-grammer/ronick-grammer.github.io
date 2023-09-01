---
title:  "[Swift] String은 Subscript로 접근이 안되는 이유"
excerpt: "[Swift] String은 Subscript로 접근이 안되는 이유"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Swift
---

### String의 요소는 Int형을 파라미터로 사용해 subscript로 접근이 안되는 이유
- Collection 타입인 `Array`는 `Int`형 index를 파라미터로 넘겨주어 `subscript`를 통해 해당 요소에 접근할 수 있지만,
```swift
let number: [Int] = [1, 2, 3, 4, 5]
number[1]  // 2
```
-  `String`은 `String.Index` 구조체를 파라미터로 넘겨주어 `subscript`를 통해 해당 요소에 접근해야 한다.
```swift
let str: String = "123456"
let index = str.index(str.startIndex, offsetBy: 1)
str[index] // "2"
str[1] // error

```

#### Swift의 문자는 유니코드를 준수하여 표기된다
- 유니코드 문자는 크기가 가변적이기 때문에 하나의 문자가 하나의 바이트보다 클 수 있다.
- 하나의 문자가 하나이상의 바이트를 차지할 수 있기때문에, 일반적인 문자열 배열처럼(다른 언어의 문자열 배열처럼) 1 바이트 단위인 인덱스로 특정 문자에 접근할 수가 없다.
- 유니코드 표현방식으로는 `utf8(8bit)`, `utf16(16bit)`, `utf32(32bit)`가 존재하며 Swift에서는 `Unicode Scalar(21bit)`가 존재한다.

***유니코드: 존재하는 모든 문자를 모든 플랫폼에 일관되게 표현하고 다룰 수 있도록 설계된 산업 표준 문자코드이다**

#### Swift 의 `String`, `Character` 타입은 `Unicode Scalar` 값으로 이루어져 있다.
- Character 타입은 하나이상의 Unicode Scalar 값으로 구성되어 있다.
```swift
let character1: Character = "\u{D55C}"                  // '한'
let character2: Character = "\u{1112}\u{1161}\u{11AB}"   // 'ᄒ, ᅡ, ᆫ' == '한'
```
- String 타입은 하나이상의 `Character` 타입(or `Unicode Scalar`들)으로 구성되어 있다.
```swift
let character2: Character = "\u{1112}\u{1161}\u{11AB}"   // 'ᄒ, ᅡ, ᆫ' == '한'
let str1 = "\u{1112}\u{1161}\u{11AB}국" // "한국"
let str2 = "\(character2)국"            // "한국"
let str3 = "한국"                        // "한국"
```

#### `String`은 `subscript`를 통해 요소에 접근할때 순차적으로 순회한다.
- 일반적인 CollectionType(`Array` 등) 은 `RandomAccessCollection` 프로토콜을 채택하고 있어 O(1) 의 시간복잡도로 요소에 접근한다.
```swift
let number = [1, 2, 3, 4, 5]
number[1] // 2
```
- `String`은 `BidirectionalCollection` 프로토콜을 채택하여 순차적으로 요소를 조회하면서 타겟 요소까지 접근하여 구성하고 있는 `Unicode Scalar` 값의 갯수에 상응하는 O(n)의 시간복잡도가 소요된다.
   - `String` 을 이루는 `Character`(or `Unicode Scalar`들)의 `Unicode Scalar` 값 갯수가 다를 수 있기 때문에 순차적으로 `Unicode Scalar` 요소들을 하나하나 순회하여 하나의 문자(`ex. "\u{1112}\u{1161}\u{11AB}" // "한"` )를 찾는다.
   
#### `String`의 요소를 `Int`형 파라미터를 사용해 `subscript`로 접근할 수 있는 방법

```swift
extension String {
    subscript(idx: Int) -> String? {
        guard (0..<count).contains(idx) else {
            return nil
        }
        let target = index(startIndex, offsetBy: idx)
        return String(self[target])
    }
}

let str = "ronick"
str[2] // "n"
str[7] // nil
```   

### 📝 참고 사이트
- [[Swift] Unicode Scalar 그리고 문자열 count 시간 복잡도 관계](https://velog.io/@haze5959/Swift-Unicode-Scalar-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EB%AC%B8%EC%9E%90%EC%97%B4-count-%EC%8B%9C%EA%B0%84-%EB%B3%B5%EC%9E%A1%EB%8F%84-%EA%B4%80%EA%B3%84#:~:text=Unicode%20Scalar%EB%9E%80%3F,%EC%9C%BC%EB%A1%9C%20%EC%A0%91%EA%B7%BC%ED%95%98%EA%B8%B0%20%EC%9C%84%ED%95%9C%20%EB%B0%A9%EB%B2%95.&text=UTF%2D32%EB%9E%91%20%EA%B1%B0%EC%9D%98%20%EB%8F%99%EC%9D%BC,%EA%B0%80%20%EB%AA%A8%EC%97%AC%20Character%EB%A5%BC%20%EC%9D%B4%EB%A3%AC%EB%8B%A4.)
- [왜 문자열을 배열처럼 다루지 못하지?](https://medium.com/@esung/swift%EC%9D%98-%EB%AC%B8%EC%9E%90%EC%97%B4%EA%B3%BC-%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C-af37a5d503a4)
- [Swift의 String index는 왜 정수가 아니지? 왜 구하기 어려울까?](https://leeari95.tistory.com/41)
- [Unicode Representations of Strings](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/stringsandcharacters/#Unicode-Representations-of-Strings)

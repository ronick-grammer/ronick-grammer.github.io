---
title:  "[Swift] AnyObject"
excerpt: "[Swift] AnyObject"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Swift
---

### AnyObject
- 참조 타입인 class 인스턴스를 저장할 수 있는 프로토콜 타입
- 값 타입인 `Int`, `Double`, `String` 등은 저장 불가능
- 어떤 타입일지 모르는 클래스 객체의 포인터 주소인 Objective-C 의 `id` 타입에 대응한다.
- Objective-C 클래스로 브릿징 되는 `NSNumber`나 `NSString`를 저장할 때 사용된다.

### Any
- 값 타입, 참조 타입 모두 저장할 수 있는 타입

### 공통
- 컴파일 타임이 아닌 런타임에 타입이 정해지기 때문에 컴파일 타임에는 타입을 알 수가 없다.
- `as`, `as?` `as!`  키워드들을 사용하여 타입 캐스팅을 진행한 다음 사용하여야 한다.
  
### 📝 참고 사이트
- [AnyObject](https://developer.apple.com/documentation/swift/anyobject#discussion)
- [[Swift] AnyObject, Plist, NSUserDefault](http://jhyejun.com/blog/anyobject-plist-nsuserdefault)
- [objective-c-개발자의-swift-사용하기-4](https://lifetimecoding.wordpress.com/2015/11/29/objective-c-%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%B4-swift-%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5-4/)
- [Any & AnyObject in iOS](https://nitinagam.medium.com/any-anyobject-in-ios-803515bd95a6)

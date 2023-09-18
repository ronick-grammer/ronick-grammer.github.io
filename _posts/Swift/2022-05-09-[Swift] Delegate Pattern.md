---
title:  "[Swift] Delegate Pattern"
excerpt: "[Swift] Delegate Pattern"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Swift
---

### Delegate 패턴이란
- 객채의 이벤트 제어권에 대한 책임을 위임받아 대신 처리해주는 디자인 패턴
- 대표적으로 iOS 에서 제공하는 `UITableViewDelegate`, `UITableViewDataSource`, `UICollectionViewDelegate` .. 등이 있으며 iOS에서 발생하는 UI 이벤트에 대한 처리를 `delegate` 프로토콜을 채택하는 `UIViewController` 에서  대신 처리해주도록 한다.

#### 이벤트 제어권에 대한 책임을 정의하는 delegate 프로토콜을 정의한다
```swift
protocol DelegateProtocol {
    func tappedButton(value: String)
    ...
}
```

#### 위임해주는 객채는 delegate라는 위임자 프로퍼티를 가진다
```swift
class Delegator {
    var delegate: DelegateProtocol?
    
    lazy var button: UIButton = {
        let button = UIButton()
        button.addTarget(self, action: #selector(tappedButton), for: .touchUpInside)
        return button
    }()
    
    init() {}
    
    @objc func tappedButton() {
        delegate?.tappedButton(value: "Tapped Button")
    }
}
```

#### 위임받는 객체는 이 delegate 프로토콜을 채택하고 내부에 정의된 메서드를 구현한다
```swift
class Delegatee: DelegateProtocol {
    
    var delegator = Delegator()
    
    init() {
        delegator.delegate = self
    }
    
    func tappedButton(value: String) {
        print("value: ", value)
    }
}
```

### 위임해주는 객체의 위임자 프로퍼티인 delegate는 weak 키워드로 선언하는 것이 좋다
위임받는 객체(delegatee)에서 위임해주는 객체(delegator)의 delegate에 위임받는 객체를 참조하게 하는 순간(`delegator.delegate = self`) '강한(`strong`)참조가 발생하여 '순환참조(`retain cycle`)' 가 발생할 위험이 있다.

이 때문에 delegate 프로토콜 타입을 `weak` 키워드를 선언함으로서 참조 카운트(`reference count`)를 증가시키지 않도록하여 순환참조를 방지하도록 할 필요가 있다. 이때 **protocol 은 값타입도 참조타입도 아닌 추상타입이기 때문에 delegate 프로토콜이 `AnyObject` 프로토콜을 채택하게 하여 컴파일러에게 참조타입임을 알려야 한다**. 이렇게 해야만 `weak` 키워드를 사용하여 참조 카운트를 관리할 수 있다.

```swift
protocol DelegateProtocol: AnyObject {
    func tappedButton(value: String)
    ...
}

class Delegator {
    weak var delegate: DelegateProtocol?
    ...
```

### Delegate 패턴을 사용하는 이유
iOS 에서 delegate 패턴은 어떤 뷰의 이벤트에 대한 처리를 다른 뷰에서 해줄때 사용할 수 있는 유용한 패턴이다. delegate 패턴을 사용하지 않게 될 경우, 전역변수를 사용한다거나 `UserDefaults` 등을 사용하여 무분별하게 로컬에 데이터를 저장하고 사용함으로서 불편성과 위험성을 증가시키는 코드를 작성하게 될 수 있다.



### 📝 참고 사이트
- [Using Delegates to Customize Object Behavior](https://developer.apple.com/documentation/swift/using-delegates-to-customize-object-behavior)
- [Delegation Pattern in Swift](https://medium.com/@nimjea/delegation-pattern-in-swift-4-2-f6aca61f4bf5)
- [Delegate Pattern In Swift](https://medium.com/@armanabkar/delegate-pattern-in-ios-a26f2b329097)
- [값이냐 참조냐, 그것이 문제로다](https://velog.io/@eddy_song/value-reference-decision)
- [[iOS 면접질문] Delegate는 retain이 될까?](https://fomaios.tistory.com/entry/iOS-%EB%A9%B4%EC%A0%91%EC%A7%88%EB%AC%B8-Delegate%EB%8A%94-retain%EC%9D%B4-%EB%90%A0%EA%B9%8C)
- [iOS) Protocol 생성 시 AnyObject 상속받기](https://gyuios.tistory.com/130)

---
title:  "[SwiftUI] ObservableObject 프로토콜 "
excerpt: "[SwiftUI] ObservableObject 프로토콜"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - SwiftUI
---
# ObservableObjects 프로토콜
앞선 포스팅에서 [MVVM 패턴](https://ronick-grammer.github.io/swiftui/SwiftUI-MVVM-%ED%8C%A8%ED%84%B4/)에 대해 다루었다. MVVM 패턴에서 View 와 ModelView가 상호 액션을 취할때
어떤 방식으로 이루어지는지는 설명하지 않았는데 이곳에서 하고자 한다.

## ObservableObject 프로토콜이란
ObservableObject를 해석하여 힌트를 얻어보자. "관찰가능한 오브젝트". 말그대로 관찰이 가능한 오브젝트라는 뜻이며 이 프로토콜을 따르는 클래스나 구조체는 외부에서 관찰될 수 있는 형태를 갖추게 된다.
ObservableObject 프로토콜을 따르고 내부에 @Property 래퍼 프로퍼티를 선언한다. 이로써 ExampleViewModel은 '게시자'가 되었다. 즉 MVVM의 ViewModel은 게시자이다. 

```swift
import SwiftUI

class ExampleViewModel: ObservableObject {
    @Published var user: User
    
    init() {
        self.user = User(userName: "ronick", fullName: "Ronick Kim", age: 19)
    }
    
    func getOlder() {
        self.user.age = self.user.age + 1
    }
}
```

이제 이 게시자를 구독하면 되는데 이 구독자의 역할을 MVVM 에서 View가 담당하게 된다. View는 ViewModel이 게시자로서 게시(@Publised)하고 있는 user 프로퍼티의 값을 항상 관찰(observe)하고 있다가
그 값이 변경되면 즉시 구독알림을 받게되고 뷰의 랜더링을 다시 하게 된다.

```swift
import SwiftUI

struct ExampleView: View {
    @ObservedObject var viewModel = ExampleViewModel()

    var body: some View {
        Button(action: { viewModel.getOlder() }, label: {
            Text("\(viewModel.user.age)")
                .frame(width: 120, height: 60)
                .foregroundColor(.white)
                .background(Color.blue)
        })
    }
}
```
  




---
title:  "[SwiftUI] EnvironmentObject"
excerpt: "[SwiftUI] EnvironmentObject"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - SwiftUI
---


# EnvironmentObject? Singleton??
데이터를 주도하는 방식으로 [State](https://ronick-grammer.github.io/swiftui/SwiftUI-상태-프로퍼티와-상태-바인딩/), [ObservableObject](https://ronick-grammer.github.io/swiftui/SwiftUI-Observable/) 를 앞서 포스팅하였다.
그리고 다른 하나로는 EnvironmentObject 가 있다.
EnvironmentObject를 사용하면서 "이거 싱글톤 아니야?" 라는 생각이 먼저 들었다. 싱글톤은 "하나만 생성하여 주입하고 사용하자"라는 개념에서 시작한 디자인 패턴중 하나이다. Spring 프레임워크를 공부하면서 지독하게
봐왔던 녀석이기에 EnvironmentObject의 사용패턴이 이와 비슷함을 어렵지 않게 알 수 있었다. 그리고 바로 이점이 상태바인딩과 ObservableObject의 바인딩 방식과는 다르다는 걸 설명할 수 있는 요소가 된다.
 
## @State, @ObservableObject, @EnvironmentObject 
SwiftUI에서의 대표적인 위 세가지 프로퍼티 래퍼들은 쓰임새의 목적도 그리고 쓰임새의 방법도 다르다.
 
### @State
한 뷰의 상태를 레이아웃에 있는 다른 뷰들과 바인딩하여 데이터 동기화를 시킨다. 참조체를 전달함으로써 부모 뷰의 상태와 하위뷰가 바인딩된다.

### @ObservableObject
MVVM 패턴에서 View와 ViewModel의 상호 액션을 취할때 사용한다. @State 방식과 마찬가지로 동일한 데이터(구독객체)에 접근하게 하려면 해당 뷰에 참조체를 전달해야한다.

### @EnvironmentObject
MVVM 패턴에서 View와 ViewModel의 상호 액션을 취할때 사용하지만 ObservableObject와 다른 점은 마치 싱글톤처럼 사용될 수 있다는 데에 있다. EnvironmentObject는 한 객체가 사용될 부모 뷰의 레이아웃이 구성될때 최초로 초기화를 하여 사용될 수 있도록 구성한다. 그리고 이 객체는 SwiftUI 환경(메모리)에 저장되며 뷰에서 뷰로 참조체를 전달할 필요 없이 부모 뷰의 모든 하위 뷰에서 접근에 가능하다. 싱글톤 처럼 하나의 객체가 메모리에 생성되어 사용할 수 있게 되는 것이다. 
 
## 언제 @EnvironmentObject를 사용할까?
사실 @EnvironmentObject로 데이터를 주도하는 방식을 @ObservableObject로도 구현할 수 있다. 동일한 구독객체에 접근한다고 한다면 상위 뷰에서 하위뷰로 계속해서 참조체를 전달하면 되는 일이다. 그러나 그건 하위 뷰의 수가 적을 때는 충분히 감당할 수가 있다. 동일한 객체에 접근해야 하는 하위 뷰가 다수라면 참조체를 일일이 전달해야하는 번거롭고 복잡한 상황이 연출될 수 있다. 이럴 경우에 @Environment를 사용한다면 참조체를 전달할 필요없이 선언만으로 동일한 객체에 접근할 수가 있다. 마치 전역변수처럼 접근할 수 있는 이러한 특성때문에 [몇몇 사람들은 Environment 사용을 아예 금하거나 줄이라고 권장하고 있기도 하다.](https://betterprogramming.pub/the-best-way-to-use-environment-objects-in-swiftui-d9a88b1e253f)

```swift
class GameSettings: ObservableObject { // 마찬가지로 ObservableObject 프로토콜을 따른다.
    @Published var score = 0 // 구독할 변수
}

struct FirstView: View {
    @EnvironmentObject var settings: GameSettings // ContentView에서 선언된 settings이다. 즉 동일한 하나의 객체

    var body: some View {
        Text("Score: \(settings.score)")
    }
}

struct SecondView: View {
    @EnvironmentObject var settings: GameSettings // ContentView에서 선언된 settings이다. 즉 동일한 하나의 객체

    var body: some View {
      VStack {
          Text("Score: \(settings.score)")

          NavigationLink(destination: ThirdView()) { // 참조체를 전달하지 않는다!!
                Text("Show Third View")
          }
    }
}

struct ThirdView: View {
    @EnvironmentObject var settings: GameSettings // ContentView에서 선언된 settings이다. 즉 동일한 하나의 객체

    var body: some View {
        Text("Score: \(settings.score)")
    }
}


struct ContentView: View {
    @StateObject var settings = GameSettings() // 동일하게 접근될 객체

    var body: some View {
        NavigationView {
            VStack {
                Button("Increase Score") {
                    settings.score += 1
                }

                NavigationLink(destination: FirstView()) { // 참조체를 전달하지 않는 것에 주목해보자.
                    Text("Show First View")
                }
                
                NavigationLink(destination: SecondView()) {
                    Text("Show Second View")
                }
            }
            .frame(height: 200)
        } // 이제 NavigationView 의 하위 뷰들에서 동일한 GameSetting() 객체인 settings에 접근할 수 있다.(FirstView, SecondView)
        .environmentObject(settings) 
    }
}

```
 
 

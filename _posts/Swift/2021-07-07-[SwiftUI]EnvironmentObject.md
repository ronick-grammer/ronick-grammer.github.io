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
데이터를 바인딩하여 이를 주도하는 방식으로 [State](https://ronick-grammer.github.io/swiftui/SwiftUI-상태-프로퍼티와-상태-바인딩/), [ObservableObject](https://ronick-grammer.github.io/swiftui/SwiftUI-Observable/) 를 앞서 포스팅하였다.
그리고 다른 하나로는 EnvironmentObject 가 있다.
EnvironmentObject를 사용하면서 "이거 싱글톤 아니야?" 라는 생각이 먼저 들었다. 싱글톤은 "하나만 생성하여 주입하고 사용하자"라는 개념에서 시작한 디자인 패턴중 하나이다. Spring 프레임워크를 공부하면서 지독하게
봐왔던 녀석이기에 EnvironmentObject의 사용패턴이 이와 비슷함을 어렵지 않게 알 수 있었다. 그리고 바로 이점이 상태바인딩과 ObservableObject의 바인딩 방식과는 다르다는 걸 설명할 수 있는 요소가 된다.
 
## @State, @ObservableObject, @EnvironmentObject 
SwiftUI에서의 대표적인 위 세가지 프로퍼티 래퍼들은 쓰임새의 목적도 그리고 쓰임새의 방법도 다르다.
 
### @State
한 뷰의 상태를 레이아웃에 있는 다른 뷰들과 바인딩하여 데이터 동기화를 시킨다.

### @ObservableObject
MVVM 패턴에서 View와 ViewModel의 상호 액션을 취할때 사용한다. @State 방식과 마찬가지로 다른뷰와 바인딩하여 동일한 데이터(구독객체)에 접근하려면 참조체를 전달해야한다.

### @EnvironmentObject
MVVM 패턴에서 View와 ViewModel의 상호 액션을 취할때 사용하지만 ObservableObject와 다른 점은 마치 싱글톤처럼 사용될 수 있다는 데에 있다. EnvironmentObject는 한 객체가 사용될 최상위 뷰에
 
 
 
 

---
title:  "[SwiftUI] MVVM 디자인 패턴"
excerpt: "[SwiftUI] MVVM 디자인 패턴"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - SwiftUI
<!-- tags:
  - Database 
last_modified_at: 2021-01-04 -->
---

# MVVM 디자인 패턴

디자인 패턴 몇가지만 알면 개발이 참 쉬워지는 것 같다. 어떻게 코드를 분리해야 할지 알기 때문에 설계 시간도 줄일수 있고 코드보기도 좋고 여러모로 편리하다. iOS에 입문한지 얼마 되지 않은 상태에서 많은 회사 공고를 
둘러보았는데 유독 눈에 띄는 자격요건이 보인다. "MVVM 디자인 패턴 사용 경험자"

  
## MVVM 패턴 구성요소

<p align="center"> <img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Swift/MVVM 패턴.png" width="70%"></p>

MVVM 패턴은 Model, View, ViewModel 로 이루어져있는 디자인 패턴이다. 각 요소가 하는 역할은 대략적으로 다음과 같다.

  
  - Model <br>
  데이터의 모델이다. 즉 데이터를 담아두기 위한 구조체라고 할 수 있다. 모델의 데이터(프로퍼티)가 변경되면 ViewModel에게 알리게 된다.
  
  ```swift 
  struct User { 
    let username: String
    let email: String
    let profileImageUrl: String
    let fullname: String
    
    var isFollowed: Bool? = false
  }
  ```
  
  - View <br>
  화면에 보이는 UI를 나타낸다. View는 해당 View의 로직을 처리하는 ViewModel을 소유한다. Model 역시 소유할 수 있으나 UI에 나타내는 용도로만 사용하고 데이터는 변경하지 않는다.
  
  ```swift
  struct ProfileView: View {
    let user: User
    @ObservedObject var viewModel: ProfileViewModel
    
    init(user: User) {
        self.user = user
        self.viewModel = ProfileViewModel(user: user)
    }
    
    var body: some View {
        Button(action: { viewModel.follow()} ) {
          Text("Follow")
          .frame(width: 172, height: 32)
          .foregroundColor(.white)
          .background(Color.blue)
        }
        
        if viewModel.user.isFollowed! {
            Text("IsFollowed!!")
        }
     }
  }
  ```
  
  - ViewModel <br>
  백엔드 혹은 비즈니스 로직을 처리한다. ViewModel은 Model을 소유하며 그 데이터를 갱신한다. Model로 부터 데이터 변경이 생기면 바인딩 되어 있는 View와 상호 액션을 취하게 된다.
  
  ```swift
  class ProfileViewModel: ObservableObject {
    @Published var user: User
    
    init(user: User) {
        self.user = user
        .
        .
    }
    
    func follow() {
        self.user.isFollowed = true
    }
  }
  .
  .
  
  ```
  
<br>
  
위의 설계대로 "Data and user action binding" 처럼 ViewModel과 View는 서로 바인딩되고, View가 ViewModel에 정의된 action 함수(위 코드에서는 follow 함수) 를 호출 하게 되면 VideModel이 소유한 Model의 데이터(isFollowed)가 변경되고 이와 바인딩된 View는 새로 뷰를 게시하게 된다.

---
title:  "[SwiftUI] LazyVStack"
excerpt: "[SwiftUI] LazyVStack"
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
# LazyVStack
- 스마트폰의 화면은 작다. 모든 컨텐츠(사진 글 등)를 전부 보여주기에는 제한적이다. 때문에 화면 밖에 있는 컨텐츠들을 보여주기 위해서 우리는 손가락으로 스크롤을 내려가며 컨텐츠들을 읽어 나간다.
  그 컨텐츠 데이터들을 처음에 전부 다 생성하여 보여줄 것인지, 아니면 스크롤해서 그 컨텐츠가 보여줄 시기에만 보여줄 것인지 우리는 선택할 수가 있다.
  
- LazyVStack은 스크롤해서 특정 컨텐츠의 위치가 노출될 때에 비로소 생성시켜준다. 최적화를 시킬때 사용한다.

- LazyVStack은 ScrollView 안에서 사용이 된다.

<center><img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Swift/LazyVStack.PNG" width = 70%></center>
<center><img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/OS/fd/paged segmentation.JPG"></center>
<br>
<br>

### 예시)

```swift
ScrollView {
    LazyVStack(alignment: .leading) {
        ForEach(1...100, id: \.self) {
            Text("Row \($0)") 
        }
    }
}
```




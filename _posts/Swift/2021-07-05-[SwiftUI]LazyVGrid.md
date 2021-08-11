---
title:  "[SwiftUI] 그리드 뷰"
excerpt: "[SwiftUI] 그리드 뷰"
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
# LazyVGrid

- UIKit 기반으로 그리드 뷰를 구현을 하려면 UICollectionView를 사용해서 상당히 많은 코드를 짜야 한다. 하지만 SwiftUI에서는 매우 간단한 UI 코드를 제공한다. 바로 LazyVGrid 라는 컨테이너 뷰(Container View)이다.
- 
- 하지만 LazyVStack이 단독으로 쓰이는 경우는 없고 다음과 같이 쓰이는 경우가 대부분이다.

  - ScollView : 스크롤이 가능하게 하는 컨테이너 뷰이다. LazyVStack은 스크롤 기능을 가지고 있지 않으므로 컬럼의 갯수가 커짐으로 인해 스크린을 벗어나는 요소들을 볼 수가 없다.
  - ForEach: 그리드뷰의 각 요소를 하나하나 넣기에는 무리가 있으므로 반복문으로 나타내준다.

- 사용 예)

```swift
import SwiftUI

struct FeedView: View {
    private let items = [GridItem(), GridItem(), GridItem()]
    private let width = UIScreen.main.bounds.width / 3
    
    var body: some View {
        ScrollView {
            LazyVGrid(columns: items, spacing: 3, content: {
                ForEach(0 ..< 14){ _ in
                    Rectangle()
                        .frame(width: width, height: width)
                }
            })
        }
    }
}
```
- 결과
<p align="center"> <img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Swift/LazyVGrid.png" width="20%"></p>
  




---
title:  "에스컬레이터 모델링과 애니메이션"
excerpt: "에스컬레이터 머델링과 애니메이션"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg
  
tags:
  - Solo Game Development
  - 1인 게임 개발
  - Unity
  - Maya
  
categories:
  - 게임개발[TheOrb]
---
## 에스컬레이터 씬 만들기 in Maya

이번에는 에스컬레이터에서 여러 장애물을 피해야 하는 씬을 구현하기로 했다. 에스컬레이터를 모델링 해야 했는데 움직이도록 애니메이션 하려면 마야에서 좀 특별한 기능을 써야했다.
내가 만든 에스컬레이터 모델은 완전 둥근 타원형이 아니기 때문에 단순히 rotation 값을 올려주고 줄여주는 것으로는 에스컬레이터가 움직이는 것처럼 보이지는 않는다. <br>
그렇기 때문에 마야의 Point on Poly 기능을 사용하여 좀 다른 방식으로 에스컬레이터가 작동하는 걸 애니메이션화 시켜야 했다. <br><br>

아래 보이는 것처럼 에스컬레이터 모델의 모든 vertex 에 rig 를 하나하나 붙여주고 Point on Poly 작업을 해준다. 그럼으로 인해 각 rig들은 각 vertex와 붙어 다닌다.
그러고 나서 모든 rig 들에 대한 하나의 Controller(에스컬레이터를 감싸는 파란줄)을 만들어서 이걸 회전 시키면 에스컬레이터의 작동 애니메이션을 진행할 수 있다!


<br>

![escalatorOperation](https://user-images.githubusercontent.com/73280175/105182071-f54e9f00-5b6f-11eb-905e-69d54f780f7e.gif)

<br><br>



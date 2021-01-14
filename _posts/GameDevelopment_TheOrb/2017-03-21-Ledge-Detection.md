---
title:  "Ledge Detection 구현하기"
excerpt: "Ledge Detector"

toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg
tags:
  - Ledge Detection
  - Ledge
categories:
  - Game Development-The Orb
---
## Ledge Detection

'Ledge' 란 사전 어원으로는 "절벽에서 (선반처럼) 튀어나온 바위" 라고 한다[출처: 네이버]. 게임상에서 어떤 캐릭터의 앞에 벽이 있을 때 그 캐릭터는 
그 벽을 넘어가야 하는데 그럴려면 본인앞에 있는 절벽의 높이를 알아야 한다. 


### Ledge Detecttion 구현 정보 얻기 

Ledge 의 위치를 알기 위해서는 오브젝트(절벽)의 높이를 단순히 얻는 것 만으로는 부족하다. 왜냐하면 그 절벽은 정사각형이나 직사각형만이 아니고
아래 그림처럼 울퉁불퉁한 모형일 경우가 더 많을 것이기 때문이다. 좀더 정교하게 하려면 다른 방법이 필요하다

![2-Considerations](https://user-images.githubusercontent.com/73280175/104543194-5eae4980-5668-11eb-865b-e0b571d5c51f.jpg)

[ 해당 출처 링크: http://cinderflame.com/ledge-climbing-1/ ]

위의 자료처럼 캐릭터 주변에 위에서부터 레이저를 쏴서 높이를 측정하는것! 그렇지만! 나의 게임 The Orb 같은 경우는 배경과 캐릭터는 3D 게임이지만
게임 조작방식은 2D 방식인 Side Scroller 게임이다. 그렇기에 조작방식이 3D 게임인 경우에 위 방식처럼 하는 것이 좋겠지만 이 게임은 2D 조작 방식이기에 
이것을 그대로 사용하면 미약하겠지만 불필요한 성능저하를 일으킬 것이다. 좀더 2D 에 어울리는 Ledge Detection 을 구현해보자!


### Ledge Detection 을 구현하다

캐릭터 앞에 하나의 레이저를 쏴서 그 레이저가 앞의 오브젝트(절벽)에 닿으면 그 절벽의 Ledge 위치를 알아내게 구현하였다.
그리고 나서 캐릭터가 그 Ledge의 위치에 도달하면 캐릭터는 바로 그때 절벽에 올라가는 모션을 취하게 된다.

![ledgeCheck1-edit](https://user-images.githubusercontent.com/73280175/104544147-6ff85580-566a-11eb-969d-8348221f4e54.gif)


![ledgeCheck2-edit](https://user-images.githubusercontent.com/73280175/104544148-725aaf80-566a-11eb-9753-67fe945a51ba.gif)

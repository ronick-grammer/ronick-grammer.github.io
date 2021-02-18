---
title:  "NPC Rato 모델링하기 "
excerpt: "NPC Rato 모델링"
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
  - Game Development-Project Limbo
---

## Rato 캐릭터 구상하기
아래 그림은 처음에 이 게임 프로젝트인 '프로젝트 림보'의 세계관을 구상할 때 처음으로 그렸던 그림이다. 캐릭터 이름이 Rato인 이유는 Rat(쥐)에 여러 모음을 붙여보다가 'o' 발음이 가장 입에 붙었기 때문이다.
Rato는 게임상에서 나중에 Karin의 조력자 역할을 하게 된다.

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/10-Rato/KarinNRatoNBird_modeling.jpg" width="70%">
</p>

## Rato 모델링하기 in Maya
Rato를 모델링은 다른 캐릭터와는 다르게 가방을 소지 하고 있다는 점이다. 다만 가방 오브젝트 모델이 Rato와 combine 되지 않아 있기 때문에 애니메이션 모션을 만들어 줄때 가방의 움직임 제어를 위한 컨트롤러를
따로 만들어야 했다. 꼬리에 Rigging을 하던 과정은 전에 Erik의 넥타이를 Rigging하던 과정과 비슷하였다.

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/10-Rato/Rato_modeling_Maya.gif">
</p>
<br>

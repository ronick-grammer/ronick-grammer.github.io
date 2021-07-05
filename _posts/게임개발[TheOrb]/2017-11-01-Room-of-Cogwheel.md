---
title:  "톱니바퀴 방"
excerpt: "톱니바퀴 방"
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
## 톱니바퀴 방 구상 디자인

일상은 돌고 돈다는 의미에서 톱니바퀴를 떠올리고 그것을 게임에 주입하기로 하였다


![톱니바퀴방 진입](https://user-images.githubusercontent.com/73280175/104830003-59960800-58bd-11eb-96a5-0c068b0baef3.jpg)



## 톱니바퀴 방에 진입하기 위한 캐릭터 애니메이션 

방에 진입하려면 톱니바퀴를 돌려서 문을 열어야 하는데 그에 대한 캐릭터의 애니메이션을 만들었다.

![spinCogwheel_Maya](https://user-images.githubusercontent.com/73280175/104830044-c0b3bc80-58bd-11eb-85cb-32b916f6b35a.gif)


## 톱니바퀴 방 구현

플레이어가 톱니바퀴를 돌리면 그에 맞게 문도 열리게 된다. 


![spinTheCogwheel_edit](https://user-images.githubusercontent.com/73280175/104830051-d7f2aa00-58bd-11eb-890b-53a3c87f13c3.gif)


## 추락사 구현

추가적으로 플레이어가 높은 곳에서 떨어지면 죽는 것도 구현하였다! 캐릭터가 마지막으로 땅에 닿아있는 y position 과 착지 했을 때 
y position 의 offset 을 구하여 특정 높이 이상이면 추락사가 되도록 하면 된다.


![deathOnFalling_edit](https://user-images.githubusercontent.com/73280175/104830049-d45f2300-58bd-11eb-90ab-03f12a44bd97.gif)


---
title:  "감시 카메라 씬 "
excerpt: "감시 카메라 씬"
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
  - Game Development-The Orb
---
## 감시 카메라 씬 구상하기

톱니바퀴 방의 장애물을 해쳐 나오고 나서 감시 카메라가 있는 곳에 진입하는 장면을 구상해 보았다. 역시 그림을 대략 그려보는게 훨씬 낫다. <br>

![감시카메라](https://user-images.githubusercontent.com/73280175/104845918-3d28b880-591b-11eb-9e41-c2ed437fd078.jpg)

<br>

## 감시 카메라 씬 구현하기 in Unity

이제 구상한 대로 씬을 구현해보자. 감시 카메라는 정해진 패턴으로 움직이고 쏘아대는 빛의 범위에 들어가면 플레이어는 게임오버가 된다.
단, 박스 뒤에 있으면 걸리지 않게 된다.

![GetIntoSecurityCameraSection_edit](https://user-images.githubusercontent.com/73280175/104845925-44e85d00-591b-11eb-8b05-52de29fa9220.gif)

<br><br>

![securityCameraCatch_edit](https://user-images.githubusercontent.com/73280175/104845931-47e34d80-591b-11eb-8d06-e4199fa326fc.gif)



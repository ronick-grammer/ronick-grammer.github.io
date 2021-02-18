---
title:  "돌다리 부수기 "
excerpt: "돌다리 부수기"
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

## 몬스터 Tall Watcher를 물리칠 방법 구상하기
몬스터에게서 계속 피해다니기만 하는 모습이 너무 나약해 보여서 물리칠 방법을 구상해보았다. Tall Watcher의 공격패턴은 뛰어올라 착지하는 힘으로 플레이어를 누르는 것이다.
이 착지의 힘으로 지형을 부셔서 떨어뜨려보면 자연스러운 연출을 만들수 있을거라고 봤다.

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/9-stoneBridgeBroke/tallWatcherFalls.jpg" width="70%">
</p>

## 돌다리 만들기 in Maya
부수는 맛은 나무 다리보다 돌다리가 더 있을거 같았다. 우선 미리 돌다리가 부서졌다고 가정을 하고 조각을 낸 뒤에 붙여준다. 

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/9-stoneBridgeBroke/StoneBridge_Maya.gif">
</p>
<br>

## Tall Watcher 낙하 씬 in Unity
돌다리도 두들겨 보고 건너야 한다.
<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/9-stoneBridgeBroke/TallWatcherFalls.gif">
</p>
<br>

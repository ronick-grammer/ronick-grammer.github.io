---
title:  "몬스터 Ballbat의 공격 패턴"
excerpt: "몬스터 Ballbat의 공격 패턴"
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



## 공격 패턴 구상하기
Ballbat은 날아다니는 몬스터인 만큼 공격 패턴을 여러가지로 구상해볼 수 있었다. 


<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/4-ballBatsAttckPattern/BallBatsAttackPattern_01.jpg" width="40%">
</p>

<br>
하지만 결국 저 여러 공격 패턴 중 위에서 두 번째 공격 패턴만 구현하기로 하였다. 이유는 나머지 패턴들이 구현하기 까다롭기 때문이다. 우선 가장 간단해 보이는 패턴을 구현하고 나머지는 차츰차츰 추가로 구현해 나가면 된다.

  
## Ballbat 공격 패턴 구현 in Unity
Ballbat이 Karin을 공격하는 패턴 과정은 다음과 같다.
1. 카린의 위치까지 추적하여 직진한다.
2. Ballbat의 collider에 Karin의 충돌이 감지되면 카린의 추적을 멈추고 마지막으로 감지된 Karin의 위치에 몸통박치기 공격을 한다.
3. 공격이 끝나면 배경 저편으로 서서히 사라진다.

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/4-ballBatsAttckPattern/ballBatsAttack.gif">
</p>


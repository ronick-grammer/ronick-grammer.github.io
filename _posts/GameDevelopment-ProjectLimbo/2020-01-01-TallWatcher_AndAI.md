---
title:  "몬스터 Tall Watcher와 AI "
excerpt: "몬스터 Tall Watcher와 AI"
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



## 기괴한 몬스터를 만들어보자
몽환적이고 기괴한 게임의 분위기에 맞게 몬스터 역시 기괴하게 만들어 보고 싶었다. Tall Watcher는 하나의 거대한 눈을 거진 큰 키의 몬스터이다. 눈에서는 불빛을 쏴서 어둡고 안개가 낀 숲속에서 
플레이어를 찾고 공격하는 역할을 한다.


<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/8-TallWatcherAndDetection/TallWatcherDetection.jpg" width="70%">
</p>

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/8-TallWatcherAndDetection/TallWatcher_modeling_Maya.jpg">
</p>
<br>

## Detection Controller
본 게임 프로젝트가 3D Side Scrolling 게임 이다 보니 캐릭터가 움직일 수 있는 방향은 x 축과 y 축 뿐이다. 그렇기 때문에 몬스터들 역시 같은 x축에 두는 것이 일반적이긴 하나, 
현재 이 게임의 그래픽은 3D 이기 때문에 이러한 특성을 잘 살리고 싶었다. 그 특성이란 바로 z 축을 활용하는 것! Tall Watcher 는 z 축에 위치시켜 Karin을 찾게 하면 더욱 실감나게 숨고
도망치는 플레이를 유도할 수 있을거라 생각했다.
<br><br>
또한 Tall Watcher는 Detection Controller를 가지고 있는데, 이는 플레이어를 감지할 몬스터의 중심 범위안에서 바라보는 방향을 기준으로 또 다른 범위를 만들어 플레이어를 감지할 수 있다.

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/8-TallWatcherAndDetection/detectionController.gif">
</p>
<br>

## PathFollow Controller
Tall Watcher가 공격을 마치고 원래 위치로 돌아가기 위한 스크립트를 작성해야 했는데 우선 Unity의 Navogation 기능을 이용하여 Path의 첫위치로 다시 돌아가도록 하도록 하였다.
그러면 Tall Watcher는 다시 path 를 따라서 움직이면서 플레이어를 감지할 것이다.

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/8-TallWatcherAndDetection/pathFollow.gif">
</p>
<br>

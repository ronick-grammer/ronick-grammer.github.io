---
title:  "3D 모델 소스 분리화"
excerpt: "3D 모델 소스 분리화"
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



## 3D 모델 작업 시간을 줄이기 위한 시도
"3D 모델을 만들때마다 매번 처음부터 작업을 해야할까?" 문득 이런 생각이 들었다. 간단한 캐릭터 뿐만 아니라, 만들기 쉬워 보이는 배경 오브젝트까지 모델링에 드는 작업소요시간은
정말 무시못하게 크다. 그래서 작업시간을 줄여보고자 아이디어를 생각해보았다. 
<br><br>
"비슷한 종류(유형)의 3D 오브젝트는 그 종류(유형)의 기초 모델링 오브젝트를 가지고 만들면 되지 않을까?" 
<br><br>
이러한 생각의 유래는 현실 세계에서도 찾아볼 수있다. 옷가게에 전시되어 마네킹에 옷을 입힐 때에 마네킹까지 처음부터 만들어서 옷을 전시하지 않는다. 마네킹의 스타일을 바꾸고 싶다면
마네킹을 없애고 새로 만드는 것이 아니라, 옷을 바꾸면 되는 것이다. 이것은 3D 모델링 작업에도 적용을 해보고자 하였다.
<br><br>
인간형 캐릭터 3D 모델링 작업을 할 경우 기초 신체 부분들을 각각 분리하여 나중에 다른 캐릭을 모델링 할 때에 재사용한다. 그 요점들은 다음과 같다. 참고로 Maya 에서 작업할 때의 상황이다.
<br>

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/5.5-modelingPartition/modelingPartition_sketch.jpg" width="70%">
</p>

1. 각 신체 부분을 모델링한다. 이때 각 부분들은 Combine 하지 않고 분리된 상태로 둔다.
2. 각 분리된 신체 부분들이 Append될 곳들의 Edge와 Vertex 갯수를 정확히 맞춘다.
3. 이렇게 각 분리된 기초 신체 부분 모델들은 나중에 캐릭터에 맞게 변형할 때에도 Append될 부분의 Edge와 Vertex 갯수는 반드시 유지되어야 한다.
4. 3번의 이유로 절대 각 분리된 신체 모델들을 Smooth하여 Edge와 Vertex 갯수를 변경하지 않는다. 나중에 Append하기에 매우 곤란해 진다.



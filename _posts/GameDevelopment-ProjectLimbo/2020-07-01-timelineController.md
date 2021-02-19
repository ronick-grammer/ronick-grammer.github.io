---
title:  "타임라인 컨트롤러 구현"
excerpt: "타임라인 컨트롤러 구현"
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

## 유니티의 타임라인(Timeline) 기능
"타임라인이란 에디터 창을 사용하여 씬의 게임 오브젝트에 연결된 트랙과 클립을 시각적으로 정렬하고 컷씬, 시네마틱 영상, 게임플레이 시퀀스를 생성할 수 있습니다." 라고 유니티 공식 메뉴얼에 나와있다.
 간단히 말해서 애니메이션 씬을 제작할 수 있도록 해주는 유니티의 에디터이다. 이 기능을 알기 전에는 컷 씬을 도데체 어떻게 만들지 감조차 안잡혔었다. 현재는 이 프로젝트에서 가장 많이 사용하는 유니티의 기능이 되어버렸다.

## 타임라인 컨트롤러 구상
여러 애니메이션 씬들이 있기 때문에 이 애니메이션 씬들을 관리할 하나의 컨트롤러가 필요하다. 여기서 말한 애니메이션 씬들이란 유니티 내에서는 타임라인 애셋을 의미한다. 구성도는 전에 포스팅 하였던
[대화 시스템 구현](https://ronick-grammer.github.io/game%20development-project%20limbo/dialogueSystem/)과 동일하다.

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Timeline/timeline_system.jpg" width="70%">
</p>

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Timeline/timeline_controller.jpg">
</p>
<p align="center">
(Timeline Controller)
</p>
<br>

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Timeline/timeline_container.jpg">
</p>
<p align="center">
(Timeline)
</p>
<br>

바로 위에 있는 사진은 Timeline Container로 특정 씬에 대한 타임라인을 담는 컨테이너라고 보면 된다. Timeline Container의 인스펙터 변수들의 설명은 아래와 같다.

- Time Line Asset: 특정 애니메이션 씬을 담은 타임라인 에셋.
- Instant Start Of Time Line: 해당 타임라인을 게임 시작과 동시에 실행을 할지 말지 결정한다.
- Can Input: 타임라인 진행중에 키를 눌러서 움직임이 가능하게 할지 불가능하게 할지 결정한다. 
- Start With Button: 특정 버튼을 눌러서 타임라인을 시작할지 말지를 결정한다.
- Start With Facing Right: 플레이어가 오른쪽을 바라보고 있을때 타임라인을 실행한다.
- Start With Facing Left: 플레이어가 왼쪽을 바라보고 있을때 타임라인을 시작한다. Start With Facing Right 역시 true 라면 양쪽 어느쪽이어도 타임라인을 실행한다.
- Has Dialogue: 대사를 담은 컨테이너를 가지고 있다면 타임라인의 진행 중이나 진행 후에 바로 대화를 시작한다.
- Has Next Timeline: 해당 타임라인이 끝나고 실행할 다음 타임라인이 있는지 없는지 확인한다.
- Start Timeline After Dialogue: 대화가 끝난 후 특정 타임라인 씬을 실행한다.
- Script_Dialogue Trigger: Has Dialogue 인스펙터 변수가 true 라면 나타나며, 특정 씬의 대사들을 담은 컨테이너이다.
- StartTime: 타임라인이나 대화를 현재 타임라인이 실행되고 몇초 뒤에 실행할 것인지에 대한 인스펙터 변수

## Dialogue System in Unity

위의 구상과 구현을 바탕으로 나온 결과이다.

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Timeline/timeline.gif">
</p>





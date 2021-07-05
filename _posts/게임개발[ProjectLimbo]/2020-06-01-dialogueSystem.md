---
title:  "대화 시스템 구현"
excerpt: "대화 시스템 구현"
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
  - 게임개발[ProjectLimbo]
---

## 대화 시스템 구상하기
많은 게임들이 스토리 진행을 위해 게임 내에 텍스트를 이용한 대화 시스템을 활용하고 있다. 본 게임 프로젝트도 스토리 중심의 게임인 만큼 대화 시스템의 구현은 필수적이었다. 
우선 두 가지 방법을 생각해 보았다.

1. 대화 씬 하나에 대한 모든 캐릭터들의 대사들을 그 대화 씬에 저장한다.
2. 각 캐릭터들 자신들이 하는 대사를 본인한테 저장을 하고, 나중에 대화 씬이 있을때 그 캐릭에게서 대사들을 빼낸다. 

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Dialogue/dialogueSystem_01.jpg">
</p>

여러 자료를 참고하였지만 1번의 방법이 대부분은 많이 사용되는 것 같았다. 2번의 방법같은 경우, 캐릭터들이 하는 모든 대화 씬의 대사들을 캐릭터들에 저장을 하면 이게 어떤 대화 씬에 쓰이는지 알기가 어려웠다.
반면 1번을 사용하면 대사들 자체가 어떤 캐릭터가 하는 대사인지 알면서 동시에 그 대사들이 해당 대화 씬에 저장이 이미 되어있기 때문에 구분하는데에 어려움을 느낄 필요가 없다. 그래서 1번을 구현하기로 하였다.

## 대화 시스템(Dialogue System Controller) 구현하기
위에서 구상을 다 마쳤으니 이제 구현을 하면 된다. 우선, 대화들이 여러개 있기 때문에 이런 대화들을 관리할 하나의 대화 시스템 컨트롤러가 필요하다. 즉 하나의 대화 시스템 컨트롤러가 여러개의 대화들을 하나씩 
처리할 수 있도록 하면 된다.


<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Dialogue/dialogue_system.jpg" width="70%">
</p>

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Dialogue/dialogueSystemController.jpg">
</p>
<p align="center">
(Dialogue Controller)
</p>
<br>

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Dialogue/dialogueTrigger.jpg">
</p>
<p align="center">
(Dialogue)
</p>
<br>

바로 위에 있는 사진은 Dialogue Trigger로 특정 씬에 대한 대사들을 담는 컨테이너라고 보면 된다. Dialogue Trigger의 인스펙터 변수들의 설명은 아래와 같다.

- Can Input Keys During Dialogue: 이름그대로 대화중에 키를 눌러서 움직임이 가능하게 할지 불가능하게 할지 결정한다.
- Destroy: 대화가 끝나고 해당 대사들을 담은 컨테이너를 삭제할지 말지 결정한다.
- Start With Button: 특정 버튼을 눌러서 대화를 시작할지 말지를 결정한다.
- Start With Facing Right: 플레이어가 오른쪽을 바라보고 있을때 대화를 시작한다.
- Start With Facing Left: 플레이어가 왼쪽을 바라보고 있을때 대화를 시작한다. Start With Facing Right 역시 true 라면 양쪽 어느쪽이어도 대화를 시작한다.
- Dialogue: 캐릭터들의 여러 대사를 적을 수 있다. (캐릭터 이름) : (대사) 처럼 ':' 을 기준으로 왼쪽 텍스트(캐릭터 이름)는 노란색으로 오른쪽(대사)은 흰색으로 나눈다.
- Get Item: 해당 대화가 끝나면 특정 아이템을 얻는다.
- Start Timeline After Dialogue: 대화가 끝난 후 특정 타임라인 씬을 실행한다.

## Dialogue System in Unity

위의 구상과 구현을 바탕으로 나온 결과이다.

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/Dialogue/dialgoue.gif">
</p>





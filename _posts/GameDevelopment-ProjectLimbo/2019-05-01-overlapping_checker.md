---
title:  "Overlapping Checker"
excerpt: "Overlapping Checker"
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



## 사물과의 충돌을 확인 하는 방법 in Unity
게임 안에서 충돌을 구현하는 것은 매우 필수적이다. 예를 들어 적이 던진 창에 맞는 것은 곳 충돌을 의미하고 그 충돌 여부를 확인함으로서 플레이어에게 데미지를 입힐 수 있기 있다. 그 밖에 충돌은 특히나 액션 게임에서 
필수적으로 구현되어야 한다. 유니티에는 대표적으로 두 개의 충돌 구현 방법이 있다.
- Collider Component: 콜라이더라 불리는 이 컴포넌트는 유니티가 기본적으로 제공하는 컴포넌트이며 소스코드의 구현없이 원하는 오브젝트에 간단히 추가하여 사용할 수 있다. 종류 별로 여러가지 콜라이더가 있지만
대표적으로는 아래 세 가지가 주로 사용된다. 왼쪽순으로 Box Collider, Sphere Collider, Character Controller 라 불린다 
<br><br>
이 밖에 Mesh Collider 라는 콜라이더 종류 중 하나가 있는데, 오브젝트의 mesh, 즉 오브젝트의 형태에 맞게 자동으로 Collider 형태가 들어 지는 것을 말한다. 그러나 이 방법은 게임의 퍼포먼스 면에서 
최악을 자랑하므로 사용하는 것을 권하지 않는다. Mesh Collider 는 오브젝트 형태에 맞게 수 많은 vertex(버텍스)를 생성하고 연결하는 방식으로 Collider 를 생성하기 때문에 
Collider 자체가 수학적인 연산을 계속해서 수행하는 만큼 게임의 최적화에 방해가 된다. 그렇기 때문에 최소한의 vertex 를 가지는 아래의 다른 세 가지(box collider 등)가 사용되는 것이 좋다.

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/1-2-overlapping checker/unity-collider-types.png">
</p>

- Physics.Overlap: 이것 또한 충돌을 구현하는 하나의 방법이다. Collider Component 와 다른 점은 컴포넌트로서 오브젝트에 추가하는 것이 아니라 script로 직접 충돌을 구현할 수 있게 해준다는 것이다.
Collider Component 는 다른 물체와 충돌할 때마다 충돌 여부 함수를 호출 하지만 Physics.Overlap 을 사용하면 원하는 레이어에 속하는 오브젝트만 충돌 여부를 확인한다. 
<br><br>
나는 충돌을 당하는 오브젝트에는 Box Collider 를 추가하였고 충돌 여부를 확인 하는 오브젝트는 Overlapping Checker 라는 script를 작성하여 컴포넌트로 추가해주었다.
<br>
<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/1-2-overlapping checker/overlaping checker.jpg">
</p>


## Overlapping Checker 구현
아래 gif 에서는 안나타나지만 저 절벽 오브젝트에는 Box Collider Component 가 부착되어 있다. 플레이어 캐릭터는 총 세 개의 Overlapping Checker 가 있다
- checkGrounded_above: 충돌 여부를 확인하는 Overlapping Checker 로서 '위'에 있는 오브젝트의 충돌을 감지한다. 예) 점프를 했을 때 천장에 닿으면 바로 떨어지게 하도록
- checkGrounded_bottom: 충돌 여부를 확인하는 Overlapping Checker 로서 '아래'에 있는 오브젝트의 충돌을 감지한다. 예) 바닥에 닿아있는지 항상 확인을 한다. 착지 했을 때 착지 애니메이션 발동
- checkGrounded_front: 충돌 여부를 확인하는 Overlapping Checker 로서 '정면'에 있는 오브젝트의 충돌을 감지한다. 예) 앞으로 점프 했을때 벽과 충돌하면 바로 착지 하도록

<p align="center">
<img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/1-2-overlapping checker/Overlapping-Checker.gif">
</p>







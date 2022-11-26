---
title:  "[Clean Architecture] 3부 10장 리뷰"
excerpt: "[Clean Architecture] 3장 10장 리뷰"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Code Review
  - IOS
  - Swift
---

## ISP: 인터페이스 분리 원칙

인터페이스 분리 원칙(ISP)는 아래 다이어그램에서 유래하였다.

<img width=80% alt="1" src="https://user-images.githubusercontent.com/73280175/204081630-893193e1-b3b8-4050-8b50-2f58a60961b9.png">

- 위 세 `User`는 `OPS` 클래스의 오퍼레이션을 사용하고 있다.
- 각 `User`는 번호에 맞는 각 `op` 메서드만을 사용하고 있다

위 그림에서 `OPS`가 정적 타입 언어로 작성된 클래스라고 했을때 `User1`에서는 `op2`와 `op3`를 사용하지 않음에도 `User1`은 두 메서드에 의존하게 된다.

이러한 의존성으로 인해 `op2`나 `op3` 메서드가 변경되면 해당 메서드와 관련없는 `User1` 역시 영향(컴파일, 재배포 등)을 받게 된다.

해결책: 위 문제는 오퍼레이션을 인터페이스 단위로 분리하여 해결할 수 있다. 각 `User`에 대한 `OPS`를 만들어 이에 대해서만 의존하게 만든다.

<img width=80% alt="2" src="https://user-images.githubusercontent.com/73280175/204081627-ed50c01f-ec44-4743-8f3c-c1005d5f062a.png">

    

### ISP와 언어

- **정적 타입 언어**는 사용자가 `import`, `include` 등의 타입 선언문을 사용하도록 강제한다. 이러한 선언문으로 인해 소스코드 의존성이 발생한다.
- **동적 타입 언어**는 선언문이 존재하지 않는다(루비, 파이썬 등) 대신 런타임에 추론이 발생한다. 동적 타입 언어를 사용하면 유연하며 결합도가 낮은 시스템을 개발할수 있다.

### ISP와 아키텍처

- ISP를 사용하는 이유는 필요이상으로 많은 걸 포함하는 모듈에 의존하는 것을 피해서 불필요한 재컴파일과 재배포를 방지하기 위해서이다.

<img width=80% alt="3" src="https://user-images.githubusercontent.com/73280175/204081620-229855b2-c319-4fa6-a80e-1e8e80012ffb.png">

위 그림에서는 `System S`가 `Framework F`에 의존하고 `Framework F`는 `Database D`에 의존한다.

`System S` 와는 관계없는 기능이 `Database D`에 포함된다고 했을 때, `Database D` 내부가 변경되면 `Framework F` 를 컴파일 및 재배포를 진행해야 하고, 따라서 `System S` 역시 이에 영향을 받게 된다.

**결론적으로, 불필요한 의존도는 예상치 못한 문제에 봉착하게 한다.**


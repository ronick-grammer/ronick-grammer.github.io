---
title:  "[iOS] custom experiences with Group Activities"
excerpt: "[iOS] custom experiences with Group Activities"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - iOS
  - WWDC
---

# ****Build custom experiences with Group Activities****

- 그룹 액티비티는 디바이스들을 통해 사용자들간에 미디어 경험을 공유할 수 있도록 해준다.

- 그룹 액티비티 API의 가장 핵심 기능중 하나는 공통적으로 유저들이 서로 같이 실시간으로 공유 할 수 있게 하는 것이다.

- 그룹 액티비티를 적용하는 두가지 방법
    - Activity creation
    - Session management
    
- Activity creation
    
    <img width="1503" alt="1" src="https://user-images.githubusercontent.com/73280175/172108312-ad839ae3-ab11-4544-9765-5bdc31433ece.png">
    
    - 액티비티를 활성화하여 액티비티를 설정한다. 미디어 그룹 액티비티와는 다르게, 커스텀 액티비티만 이 설정을 진행한다.
    - 액티비티를 설정할때, 유저를 대상으로 무엇을 공유하게 할지 깊이 생각해봐야 한다.
    
- Session Management
    
    <img width="1504" alt="2" src="https://user-images.githubusercontent.com/73280175/172108345-2f672d66-4ed9-415c-854d-a96ed4289c62.png">
    
    - 액티비티 그룹을 사용하여 커스텀 데이터를 주고받도록 한다.
    - 세가지 단계
      1. Receive session
      2. Preparing for playback —> Configure session : 커스텀 데이터를 주고받을수 있도록 하는 단계
      3. Join session
    - GroupSessionMessenger: GroupSession 내에서 유저들간에 raw 데이터나 구조화된 데이터(구조체 or 클래스)를 주고받을수 있는 API를 제공함. 

## GroupActivity Capability 생성

<img width="821" alt="3" src="https://user-images.githubusercontent.com/73280175/172108350-ba75fa64-1823-48dc-87b1-2d6fbeec59fe.png">

`GroupActivity`생성을 위해서는 `Signing & capabilities` - `All` 탭에서 `+capablity` 를 클릭후, “Group Activities” 를 클릭한다.

## GroupActivity 객체 생성

<img width="814" alt="4" src="https://user-images.githubusercontent.com/73280175/172108354-b138d4d1-4aad-4d49-8063-ffe1b57658a0.png">

GroupActivity 객체를 만들기 위해서 `GroupActivity` 프로토콜을 채택한다. 그리고 필수로 요구되는 `metadata` 를 설정해준다. 여기서 중요한 것은 커스텀 액티비티로 생성하려는 것이기 때문에 `type`을 `.generic` 으로 해준다.

## GroupActivity 객체를 활성화시키기(activate)

위에서 만든 `DrawTogether` GroupActivity 객체를 활성화시키려면 다음과 같은 코드를 작성한다.

`DrawTogether().activate()`

## GroupSession 가져오기

<img width="681" alt="5" src="https://user-images.githubusercontent.com/73280175/172108358-89bcfec6-7774-4c07-878a-aadf0bb9eff3.png">

## GroupSession Join 하기

위에서 가져온 GroupSession Join을 다음과 같은 코드로 실행할 수 있다.

`session.join()`

## 데이터 송수신 하기

- 위에서 만든 `GroupSession`을 파리미터로 `GroupSessionMessenger` 객체를 만들어서 송수신한다.
- 특정 데이터를 보내기 위해서 `struct` 타입의 데이터를 만들어 줄 수 있다.

<img width="1512" alt="6" src="https://user-images.githubusercontent.com/73280175/172108367-23eec1d6-068d-4b4b-a6db-23c42069ae05.png">

데이터 송신시에 발생하는 에러는 올바르게 처리해줘야 한다.

- `GroupSessionMessenger`는 선입선출로서 여러 유저들로부터 가장 먼저 온 데이터를 우선적으로 처리한다.
- `send` API 사용시 메시지의 크기가 너무 크면 에러가 던져진다.
- `GroupSessionMessenger` 는 이미지나 영상같은 큰 데이터같은 데이터 송수신에는 적합하지 않다.
- 메시지를 빠른 시간안에 연속적으로 송신하게 되면 에러가 던져질 수 있다.

## Late-joiners 처리

Late-joiners란 `두개 이상의 디바이스들이 이미 Group Session에 join 된 상태에서 도중에 하나의 디바이스가 join을 한 디바이스 혹은 유저`를 말한다.

<img width="1508" alt="7" src="https://user-images.githubusercontent.com/73280175/172108376-c197648d-3b19-4dd9-9233-2d17642dd67f.png">

두 디바이스가 이미 join 중에 다른 하나의 디바이스가 join하게 되면 다음과 같이 이전의 두 디바이스간에 전송된 최신 데이터를 받아오지 못하는 문제가 발생한다.

<img width="1512" alt="8" src="https://user-images.githubusercontent.com/73280175/172108382-cf2dd5de-ed91-4c6b-badf-3ecfa94896c7.png">

이를 해결하기 위해 중간에 한 디바이스가 join하게 되면 다른 기존의 두 디바이스들은 활성된 디바이스 프로퍼티들의 신호를 관찰하고 있다가 해당 신호가 잡히면 최근 데이터를 중간에 join한 디바이스에게 보낸다.

<img width="1512" alt="9" src="https://user-images.githubusercontent.com/73280175/172108386-4fe92f57-a2da-425b-afde-dfbd4f410604.png">

이를 코드로 옮기면 다음과 같다. 우선 최신 캔버스 데이터를 중간에 join한 디바이스에게 보내야 하기 때문에 이를 위한 구조체를 만든다.

<img width="497" alt="10" src="https://user-images.githubusercontent.com/73280175/172108390-2f6fc091-9afd-41a3-97e2-fd9ea72cbafa.png">

그리고 해당하는 최신 데이터를 수신하는 코드를 작성한다.

<img width="1011" alt="11" src="https://user-images.githubusercontent.com/73280175/172108394-2bcd32a1-bad8-464b-8a17-838f8a93192b.png">

<br>

<img width="886" alt="12" src="https://user-images.githubusercontent.com/73280175/172108399-244792d6-08cf-468d-b376-ab8abb207664.png">

마지막으로 중간에 join하는 새 디바이스에게만 catch-up 메시지를 보내도록 한다.

<img width="1009" alt="13" src="https://user-images.githubusercontent.com/73280175/172108410-c14b30cd-0556-4844-94fc-3e02cd47da74.png">

## Changing Activity

지금까지 Group Session을 이용하여 특정 activity를 실행하기 위한 기본 설정을 마쳤다.

한가지 생각해보아야 할 것은 드로잉 캔버스를 변경한다던가 시청중인 영화를 변경하는 등의 activity를 완전히 변경하기 위해 무엇을 해야 할 것인가 이다.

<img width="1512" alt="14" src="https://user-images.githubusercontent.com/73280175/172108420-3336a54d-bb57-4f89-8bdf-8fce9121682f.png">

두가지 방법이 존재한다.

- **New session:** 애초에 새로운 Group Session을 생성하는 방법
    - `GroupActivity.prepareForActivation()` : Group Session을 시작했던 같은 API를 호출함으로 콘텐츠를 변경한다.
    - Group Session상에서의 입장과 퇴장을 깔끔하게 처리하기 때문에 사용자들간의 일정한 상태 유지에 좋다.
    - 유저가 새로운 기록이나 영상을 찾는 등, 기존 activity를 종료하고 새로운 activity를 시작하는데 용이하다.
    - 시스템에게 변경사항을 알려주고, 시스템은 이를 유저에게 알린다.
    - `GroupActivity.prepareForActivation()` 를 호출하면 마치 처음 Group Session을 시작하는 것처럼 새로운 Group Session을 받고 시작한다.
- **Updating activity:** 현재 Group Session의 Activity를 업데이트 해주는 방법
    - 하나의 음원이 재생되고 다른 음원이 재생되는 것처럼, 여러 애플리케이션이 있고 그 애플리케이션들 간의 이동 혹은 전환을 위해 사용한다.
    - `GroupSession.activity = newActivity` 을 사용하여 새로운 액티비티로 전환한다.

## GroupState observation

GroupSession을 시작하면 아래 그림과 같이 UI를 변경하려면 어떻해야 할까?

<img width="825" alt="15" src="https://user-images.githubusercontent.com/73280175/172108428-51827511-565c-464e-ae57-169cd23140eb.png">

`GroupStateObserver.isEligibleForGroupSession` 을 사용하면 디바이스가 Group Session에 참여가능한지 여부를 확인할 수 있다.

<img width="1117" alt="16" src="https://user-images.githubusercontent.com/73280175/172108442-f7e405a8-29fd-4615-8efb-e0439e7c267e.png">



\[출처]: [WWDC 강연: Build custom experiences with Group Activities](https://developer.apple.com/videos/play/wwdc2021/10187/)






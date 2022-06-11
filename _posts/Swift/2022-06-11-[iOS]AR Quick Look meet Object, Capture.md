---
title:  "[iOS] AR Quick Look, meet Object Capture"
excerpt: "[iOS] AR Quick Look, meet Object Capture"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - iOS
  - WWDC
---

## AR Quick Look, meet Object Capture

<img width="1145" alt="1" src="https://user-images.githubusercontent.com/73280175/173174193-ac9bfdb1-fcdd-4907-8061-1bc384ca8ee5.png">

- AR Quick Look은 iOS에 내장된 3D 모델 AR 뷰어이다.
- 사파리 브라우저, 메신저, 파일 등에 사용 가능하다.
- 프로젝트(or 프로덕트) 데모 등에 유용하게 사용될 수 있다.

<br><br>

### 3D Object 생성

- **이전에는** AR Quick Look을 사용해 3D 컨텐츠를 만들려면 외부 3D 모델링 소프트웨어를 사용하여야 했다.

<img width="1012" alt="2" src="https://user-images.githubusercontent.com/73280175/173174192-11174b61-a389-4a43-96e4-b502d0417329.png">


- 하지만 21년도에 3D 모델링 소프트웨어 없이 iOS 자체적으로 `RealityKit`에 내장된 Object Capture 라는 API 를 제공하여 USDZ 파일을 생성하는 단계를 간소화 시켰다.
- `Object Cature` API는 실제 사물의 이미지들을 사용하여 높은 수준의 3D 모델을 생성해낸다.
- 하지만 Object Capture 하는 것은 AR Quick Look에 바로 보여지는 USDZ파일을 생성하기 위한 많은 부하비용을 발생시킨다.

<img width="1379" alt="3" src="https://user-images.githubusercontent.com/73280175/173174189-fa6d2dd8-bf4c-408e-9719-8a3c84acd901.png">

- 상호적인 커스텀 동작을 추가 하고 싶다면  `Reality Composer` 라는 것을 사용할 수 있다. 예를 들어 Tap 이벤트나 카메라 액션같은 기능을 적용할 수 있다.

<img width="1452" alt="4" src="https://user-images.githubusercontent.com/73280175/173174187-871e4590-8252-4242-a70f-cc95fa0f5299.png">

<br><br>

- Object Cature를 사용하여 3D 오브젝트를 만드는 과정 <br><br>
  첫째, 우선 실제 사물을 모든 각도에서 찍는다.

  <p>
  <img width="30%" alt="5-1" src="https://user-images.githubusercontent.com/73280175/173174186-a00a0bec-5f9d-47ef-bfd5-cb1823e9ee3a.png">
  <img width="30%" alt="5-2" src="https://user-images.githubusercontent.com/73280175/173174185-4cddd4ae-c0e3-40d3-9cb9-d357c599a14e.png">
  <img width="30%" alt="5-3" src="https://user-images.githubusercontent.com/73280175/173174184-284c1713-9580-4304-b251-8d280ad437a9.png">
  </p> 
  <br>

  둘째, 그 후 Object Cautre를 사용하여 USDZ 모델을 생성한다.

  <img width="994" alt="6" src="https://user-images.githubusercontent.com/73280175/173174183-2be578c4-b542-453e-aa3c-c2c88254486a.png">
  <br><br>

  셋째, 위에서 만든 USDZ 모델에다가 Reality Composer를 사용하여 커스텀 이벤트를 생성할 수도 있다.

  <img width="974" alt="7" src="https://user-images.githubusercontent.com/73280175/173174182-a1bbcec9-c729-491b-bd89-7a2657a89db7.png">

<br><br>

### Exporting USDZ 파일

- 위에서 만든 3D 모델의 USDZ 파일을 export 할 때 어떤 디지털 설정을 적용할 것인지가 중요하다.

<img width="1305" alt="8" src="https://user-images.githubusercontent.com/73280175/173174181-c360dc91-4194-41f7-be37-53086d0406b3.png">

- 대체로 파일사이즈가 작으면 작을 수록 3D 모델의 정교함이 떨어지지만, AR Quick Look을 사용하기 위해서는 작은 파일 사이즈가 권장된다.

 

<img width="1265" alt="9" src="https://user-images.githubusercontent.com/73280175/173174178-18a2bd67-976c-4067-9154-8bf0376e9326.png">

<img width="1269" alt="10" src="https://user-images.githubusercontent.com/73280175/173174177-0fedd2c1-be12-4b15-9278-be40799742de.png">

- `Reduced`는 적은 비용이기 때문에 여러 개의 애셋을 하나의 scene에 보여줄 때와 웹용으로 사용할 때 적합하다.
- `Medium`은 `Reduced` 보다는 큰 비용이지만 보다 더 정교한 하나의 애셋을 하나의 scene에 보여주고 싶거나 앱용으로 적합하다.
- `Reduced`와 `Medium` 설정이 적용된 USDZ 파일을 각각 export해서 항상 비교도록 하는 것이 권장된다.
- 다양한 기기에서 테스트해볼 것을 권장한다.

<br><br>

### AR Quick Look을 사용하여 3D 모델 보이기

굉장히 간단한 방법으로 AR Quick Look을 사용하여 웹이나 앱에 3D 모델을 보여줄 수가 있다.


**앱**

<img width="1402" alt="11" src="https://user-images.githubusercontent.com/73280175/173174174-17c714c9-1202-4a9e-99e9-fd1c32c948a6.png">

- AR Quick Look을 사용하기 위해서는  `Quick Look Framework`를 import 한다.
- `QLPreviewController` 객체를 생성하여 present 시킨다.
- `QLPreviewControllerDataSource` Protocol을 채택하여 몇개의 Preview를 보여줄지와 각 Preview는 어떤 형태를 가지는지 등을 설정한다.

<br>

**웹**

<img width="963" alt="12" src="https://user-images.githubusercontent.com/73280175/173174171-c81261bb-97d5-4118-95d3-3a68e2732c5f.png">

- `href=”alarm-clock.usdz”` : 로컬에 저장되어 있는 usdz 파일
- `rel=”ar”` : 사진에 보이는 AR 뱃지 아이콘
- `allowsContentScaling=0` : 스케일링 설정 disable


<br><br>

### AR Quick Look의 활용

**이커머스**

- 소비자는 자신이 원하는 상품을 3D로 바로 접할 수 있다.
- Reality Composer를 사용하여 상품을 터치시에 색이나 모양을 변경하는 이벤트를 추가할 수 있다.

<img width="1086" alt="13" src="https://user-images.githubusercontent.com/73280175/173174170-d2615738-a1df-4a5c-8635-97e1cea53c4a.png">

<br>

**박물관 or 전시회**

- 직접 박물관에 가지 않고도 전시품을 관람할 수 있다.
- Reality Composer를 사용하여 터치시 도슨트의 전시품 설명을 청취할 수 있다.

<img width="1105" alt="14" src="https://user-images.githubusercontent.com/73280175/173174166-74edf5ff-4020-4905-bd32-0a60f3e4cc99.png">

\[출처]: [WWDC 강연: AR Quick Look, meet Object Capture](https://developer.apple.com/videos/play/wwdc2021/10078/)






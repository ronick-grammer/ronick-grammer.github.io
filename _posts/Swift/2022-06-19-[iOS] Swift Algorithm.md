---
title:  "[iOS] Swift Algorithm"
excerpt: "[iOS] Swift Algorithm"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - iOS
  - WWDC
---

## Swift Algorithms

- `Swift Algorithm` 은 오픈소스 패키지이다.
- `Swift Algorithm`에 내장된 알고리즘 기능을 사용하면 코드를 더 간결하게 작성할 수 있다.

**Swift Algorithm**이 제공하는 알고리즘 메서드를 살펴보기 전에 **Swift Standard Library**가 제공하는 기본 알고리즘 메서드를 살펴보도록 하자.

### **map**

아래처럼 채팅방을 구성한다고 해보자. 흔히 많이 사용하는 `map` 이 `for` 보다 일반적인 상황에서 더 간결한 코드를 보여줄 수 있고, 성능면에서도 우수하다. 이유는 `map` 은 배열 사이즈 재설정을 위한 중간 할당을 하지않고 공간을 재사용하기 때문이다.

<img width="1139" alt="1" src="https://user-images.githubusercontent.com/73280175/174475073-29e9b632-355f-4a51-be82-94f95f7667ff.png">
<br>

### **compactMap**

위의 채팅방(사진)에서 사진들만 모으는 코드는 다음과 같이 역시 간결하게 나타낼 수 있다.

`message`가 사진이면 이를 모은다. 일반적인 `for` 문보다 `filter`와 `map`으로 간결하게 나타낼수 있다.

<img width="1125" alt="2" src="https://user-images.githubusercontent.com/73280175/174475076-8eb8a070-9008-41fb-90bb-cfe1829abe06.png">

하지만 위의 코드를 `compactMap`으로 처리 가능하다. `compactMap`은 nil인 요소를 제외한 요소들로 이루어진 배열을 반환해준다.

<img width="785" alt="3" src="https://user-images.githubusercontent.com/73280175/174475078-49aaaf89-9a2d-447d-8a70-66f12cc99186.png">
<br>

### **joined, flatMap**

하나의 요소를 배열로 mapping 해주는 주는 코드를 살펴보자.

![4](https://user-images.githubusercontent.com/73280175/174475079-7326cdb7-9ac7-4089-a608-87e52074ec3e.png)

하지만 위의 여러 배열들을 하나로 묶어주고 싶다면 어떻게 해야 할까? `joined()` 를 사용하면 된다.

![5](https://user-images.githubusercontent.com/73280175/174475082-e57cc6e5-bab7-4779-9bb3-8986b83a573e.png)

그리고 위의 구현을 `flatMap` 으로 간단히 처리 가능하다.

![6](https://user-images.githubusercontent.com/73280175/174475083-295cb8f6-ac2a-445d-9550-70fcea0a5d23.png)
<br>

### **prefix, suffix**

배열의 가장 마지막 요소들만을 보여주고 싶은 코드를 살펴보자. `prefix` 는 요소의 가장 앞부분만을 반환한다.

![7](https://user-images.githubusercontent.com/73280175/174475086-b617cb76-b2a1-44e5-9c87-2ab694818259.png)

반면 `suffix` 는 배열의 가장 뒷부분을 보여준다. 결론적으로 위와 동일한 결과를 보여주며 같은 결과라도 다른 알고리즘을 적용하여 유동적으로 대응할 수 있다.

![8](https://user-images.githubusercontent.com/73280175/174475087-31ed75fb-de3d-412c-aefd-d23f63d4f4a7.png)
<br>

## **알고리즘 성능**

알고리즘의 각 chain(메서드)에서 중간 배열을 할당한다면, 일반 반복문보다 느릴수 있다. 하지만 swift에서는 보다 효율적으로 이 문제를 해결하고 있다.

- `joined()` 는 배열을 메모리에 새로 할당하지 않고 `FlattenSequence`를 반환한다. 여기서 `FalttenSequence`는 `lazy adaptor`로 불린다. 이는 배열처럼 사용되지만 한번 더 감싼 `wrapper`인 셈이다. 그렇기에 생성하기 위한 비용이 들지 않아 효율적이다. 게다가 “lazy” 하기 때문에 사용이 될때 할당이 된다.
- 그 결과, FlattenSequence와 같은 lazy adaptor는 알고리즘 체인이 성능면에서 우수할 수 있도록 해준다.
- 아래의 코드에서는 바로 `PhotoItem`으로 반환되지 않고 `ArraySlice`로, `reversedCollection`으로 wrrapping 된다.

<img width="795" alt="9" src="https://user-images.githubusercontent.com/73280175/174475089-76f7d60c-800e-4405-aaa3-3a42e2870455.png">

- `compactMap` 앞에 `lazy`를 적용하면 lazy하게 만들어 줄수 있다. `lazy` 는 매우 큰 컬렉션에서 적은 수의 요소들만 사용하고자 할때 유용하다.

<img width="1136" alt="10" src="https://user-images.githubusercontent.com/73280175/174475091-55899635-3995-4500-b165-9e72a490c53a.png">

- 반면 그럼에도 배열이 필요하거나 사용하고 싶다면 Array로 감싸면 된다.

<img width="484" alt="11" src="https://user-images.githubusercontent.com/73280175/174475092-4fe3f327-a2d5-43c4-a96f-622809e1f633.png">
<br>

## **Swift Algorithm Package**

Swift Algorithm Package는 Swift Standard Library가 아직 제공하지 않는 알고리즘 기능을 제공한다.

<img width="1143" alt="12" src="https://user-images.githubusercontent.com/73280175/174475093-2dd78b2b-c1ea-4703-a7c6-dfdc2443ec31.png">

Swift Algorithm 이 제공하는 몇몇 interation 관련 알고리즘 메서드들은 다음과 같다.
<br><br>

### windows(ofCount: Int)

컬렉션 요소들을 하나씩 전진하여 특정 count만큼 반복적으로 잘라 각각 배열로 반환해준다. 위에서 설명했듯이 subsequence를 반환하기 때문에 메모리에 공간을 할당하지 않는다. 

<img width="1030" alt="13" src="https://user-images.githubusercontent.com/73280175/174475094-7c4fd4f0-96b0-4857-8cb5-142c1c511e80.png">
<br>

### adjasonedPairs()

`windows(ofCount: Int)` 와 기능은 비슷하지만 subsequence가 아닌 tuple을 반환한다.

<img width="1026" alt="14" src="https://user-images.githubusercontent.com/73280175/174475096-e96230a3-5712-47eb-8ec0-494030b3ff37.png">
<br>

### chunks(ofCount: Int)

windows(ofCount: Int)와 기능은 비슷하지만 요소들을 하나씩 전진시켜 자르지 않고, count만큼 컬렉션에서 요소들을 잘라 반환한다. 컬렉션의 모든 요소가 count만큼 알맞게 나눠지지 않으면 나머지요소들이 반환된다.

<img width="1077" alt="15" src="https://user-images.githubusercontent.com/73280175/174475098-ac1edd6a-269e-42a9-a065-02a0aa946606.png">
<br>

### chunked(on:)

컬렉션의 요소들을 돌며 특정 옵션에 맞는지에 대한 bool 값과 그에 대한 요소들로 구성된 배열의 튜플을 반환한다. 다음은 `chunked(on:)`을 사용하여 여러 숫자의 요소로 구성된 컬렉션에서 연속되는 소수의 요소들을 배열로 묶어 반환하는 예시이다.

<img width="1119" alt="16" src="https://user-images.githubusercontent.com/73280175/174475099-c922455d-2057-41ec-a0bb-5b96d512587d.png">

이와 다른 옵션으로 이전 값과 현재 값이 다른지에 대한 로직을 만들때 `chunked(on:)`을 사용할 수 있다.

<img width="801" alt="17" src="https://user-images.githubusercontent.com/73280175/174475100-35d57b3b-b4a9-48ae-9bc0-4e8a8994d2dc.png">

위의 지식을 바탕으로, 컬렉션에서 현재 요소가 생성된 시간과 이전에 생성된 요소의 시간차가 1시간 이내인 요소들을 분리해 배열로 만들고 그 사이에 각 배열들의 마지막 요소를 삽입해 다시 합치는 로직을 작성해 보자.

우선 컬렉션의 요소들중 각 시간차가 한시간 이내인 것들을 묶어 각 배열로 만들기 위해 `chunked`를 사용한다.

여기서 사용할 `chunked`는 predicate, 즉 조건을 사용하여 요소들을 분리해 배열로 만들 수 있다.

<img width="1198" alt="18" src="https://user-images.githubusercontent.com/73280175/174475102-ab9db5f2-c451-44c5-a050-2a67bddd0c46.png">

그 이후 `joined` 를 사용하여 시간단위로 요소들이 분리된 배열들 사이에 각 배열의 마지막 요소를 삽입해 합쳐준다.

<img width="1158" alt="19" src="https://user-images.githubusercontent.com/73280175/174475104-2661d4b1-6532-49a6-9cf6-18abcd062c8c.png">

그리고 맨 앞에 `lazy`를 적용해줄 수가 있다.

<img width="1150" alt="20" src="https://user-images.githubusercontent.com/73280175/174475106-b9b09214-23cb-414c-ab62-826adcda5991.png">

주의할 점이 있는데, `lazy`가 언제나 해결책이 될수는 없다는 점이다. 시퀀스를 한번만 사용해야 한다면 `lazy`는 효율적으로 사용될 수 있지만 그렇지 않고, 시퀀스를 반복적으로 사용해야 하면 다음과 같은  코드는 `mapping`, `chunking`, `joining`을 매번 반복하게 되고 이는 비용을 크게 소모한다.

<img width="767" alt="21" src="https://user-images.githubusercontent.com/73280175/174475107-fe36facd-c51a-445d-afdf-7315837e3015.png">

이럴 경우는 `Array`로 한번 감싸주면 효율적으로 작업을 이어나갈 수 있다.

<img width="770" alt="22" src="https://user-images.githubusercontent.com/73280175/174475109-16938e34-f7b8-47e6-8adf-3337e1bbf06c.png">

이처럼 Swift Standard Library와 Algorithm Package에서 제공하는 알고리즘 기능은 `Sequence`와 `Collection` Protocol을 따르는 `Array`나 `String` 같은 타입들에서 사용가능하다.


[출처]: [WWDC 강연: Meet the Swift Algorithms and Collections packages](https://developer.apple.com/videos/play/wwdc2021/10256/)

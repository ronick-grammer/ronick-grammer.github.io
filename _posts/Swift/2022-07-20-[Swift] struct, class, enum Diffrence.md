---
title:  "[Swift] struct, class, enum 차이"
excerpt: "[Swift] struct, class, enum 차이"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Swift
---

## struct와 class와 enum의 차이
- `struct`: 값 타입으로 상속 기능을 지원하지 않는다. 원본 인스턴스를 복사하거나 메서드에 인자로 전달할 경우 인스턴스 값을 가진 새로운 복사본 인스턴스가 생성된다. 따라서 어떤 하나의 인스턴스의 값을 변경해도 다른 복사본들에게는 영향을 미치지 않는다.

- `class`: 참조 타입으로 상속 기능을 지원한다. 원본 인스턴스를 대입하거나 메서드에 인자로 전달할 경우 원본 인스턴스의 메모리 위치에 대한 참조체가 전달된다. 떄문에 어떤 하나의 참조체를 이용하여 인스턴스 데이터를 변경하면 모든 참조체의 데이터가 변경된다.

- `enum`: 유사한 종류의 값들을 공통적인 종류의 이름으로 한곳에 모아놓은 데이터 타입이며 값 타입이다. 일반적으로 enum은 내부에 여러 case 타입을 정의하여 case에 따라 switch case 문으로 간결하게 처리할때 사용된다.

## class 보다 struct를 사용하는게 좋은 경우
- 연관된 간단한 값들을 캡슐화하는 것이 목적일 경우

- 값을 참조하는게 아닌 복사하는 것이 합당할 경우

- 다른 타입으로부터 상속을 받거나, 자신을 상속할 필요가 없는 경우

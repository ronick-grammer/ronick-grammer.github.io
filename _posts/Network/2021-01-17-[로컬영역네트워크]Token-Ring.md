---
title:  "[로컬영역네트워크] Token Ring"
excerpt: "[로컬영역네트워크] Token Ring"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Network

tags:
  - Network
  - Token Ring
  
<!-- tags:
  - Database 
last_modified_at: 2021-01-04 -->
---
# Token Ring (토큰 링) 이란?

Token(토큰)이라는 송신 권리를 나타내는 데이터가 양동이 릴레이처럼 흐르는 토큰 패키징 방식을 사용하여 통신을 하는 네트워크 규격이다.


![Token Ring](https://user-images.githubusercontent.com/73280175/104833266-a89e6600-58da-11eb-9758-9fcadfa0e1a3.jpeg)



- 보통 네트워크에는 송신 데이터가 하나도 없을 때는 이 Token이 단독으로 흐르고 있는데, 각 컴퓨터는 Token을 수신하여 거기에 아무 데이터가 존재하지 않으면
그대로 다음 컴퓨터로 흘려보낸다.

- 송신하고 싶은 데이터가 발생한 컴퓨터는 이 Token이 수중에 들어오는 것을 기다렸다가 Token 을 잡으면 그 뒤에 송신 데이터를 붙여서 보낸다. 그 다음에
Token을 받은 컴퓨터는 그 데이터가 자기 앞으로 온 것인지를 체크하고 자기 앞으로 온 것이 아니면 그대로 다음 컴퓨터에게 전달한다. 자기 앞으로 온 경우에는 데이터를
꺼내고 Token 만을 다시 보낸다.

- 이와 같은 구조에 의해 송수신을 관리하기 때문에 Ethernet에 있는 충돌(콜리전) 이라는 개념은 원칙적으로 발생하지 않는다.


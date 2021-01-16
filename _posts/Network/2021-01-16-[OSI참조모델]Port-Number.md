---
title:  "[OSI참조모델] 포트 번호(Port Number)"
excerpt: "[OSI참조모델] 포트 번호(Port Number)"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Network
tags:
  - Network
  
<!-- tags:
  - Database 
last_modified_at: 2021-01-04 -->
---

# 포트 번호(Port Number) 란? 

TCP/IP 세계에서는 IP 주소를 바탕으로 통신을 수행한다. 그런데 컴퓨터상에서는 여러 프로그램이 움직이고 있으며 이러한 프로그램들이 동시에 통신을 하는 경우도 
생각할 수 있다. IP 주소로는 네트워크상의 어디인가에 존재하는 컴퓨터인지까지만 알 수 있을뿐 그 컴퓨터상의 어느 프로그램으로 패킷을 전달하면 좋을지는 모른다.
포트번호(Port Number)란 바로 이를 위해 이용하는 번호이다.

- 포트 번호란 프로그램의 연결구라고 보면 된다. IP 주소가 가리키는 컴퓨터의 어떤 포트 번호로 패킷을 보낼지에 따라 어떤 서비스(프로그램)와 통신할 지가 결정된다.

- 프로그램 측에서는 0 ~ 65,535 까지의 범위로 자신의 전용 연결구를를 마련하여 데이터를 주고 받는다.

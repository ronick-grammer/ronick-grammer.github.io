---
title:  "스토리지(Storage)"
excerpt: "스토리지(Storage)"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Database
<!-- tags:
  - Database 
last_modified_at: 2021-01-04 -->
---
## 스토리지(storage)

* 단일 디스크로 처리할 수 없는 대용량의 데이터를 저장하기 위해 서버와 저장장치를 연결하는 기술

* 스토리지의 종류에는 DAS, NAS, SAN 이 있다.

## DAS(Direct Attached Storage)

1. 서버에서 장치를 관리

2. 직접 연결하므로 속도가 빠르고 설치 및 운영 용이

3. 초기 구축비용이 저렴

4. 직접 연결방식이므로 다른 서버에서 접근이 안되며 파일 공유가 불가능

5. 확장성및 유연성이 상대적으로 떨어짐



## NAS(NatworkAttachedStorage)

1. 별도의 파일 관리 기능이 있는 NAS Storage가 내장된 저장장치를 직접 관리

2. Ethernet 스위치를 통해서 다른 서버에서도 파일에 접근할수 있고 장소에 구애받지 않고 저장장치에 쉽게 접근 가능

3. 확장성 및 유연성 우수

4. 접속 증가시 성능 저하



## SAN(Storage Area Network)

1. DAN 의 빠른 처리와 NAS의 파일 공유 장점이 있으며 서버와 저장 장치를 연결하는 전용 네트워크를 별도로 구성하는 방식

2. 파이버 채널(FC) 스위치를 이용하여 네트워크를 구성. 파이버 채널이란 전송 속도를 기가바이트로 높이기 위한 네트워크 기술.

3. 서버들이 저장장치 및 파일 공유 가능하며 확장성, 유연성 그리고 가용성이 뛰어남

4. 높은 트랜잭션 처리에 효과적이나 기존 시스템의 경우 업그레이드가 필요하고, 초기 설치 시에는 별도의 네트워크를 구축해야 하므로 비용이 많이 듬



참고: "시나공 정보처리기사 필기"

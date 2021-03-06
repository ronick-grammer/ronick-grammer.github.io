---
title:  "데이터 베이스 이중화(Database Replication)"
excerpt: "데이터 베이스 이중화(Database Replication)"
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
## 데이터 베이스 이중화

1. 데이터 베이스가 트렌젝션을 수행하고 있을 때 예기치 못할 문제가 발생하여 시스템오류로 서비스 중단이나 물리적 손상이 발생할 시, 이를 복구 하기 위해 동일한 데이터 베이스를 복제 하여 관리하는 것을 말한다.

2. 데이터 베이스 이중화 기법은 eager 기법과 lazy 기법이 있다.

* eager 기법: 트랜젝션 수행중 데이터 변경이 발생화면 즉시 이중화된 모든 데이터베이스에 변경내용을 저장.
 
* lazy 기법: 트렌젝션 수행이 끝나면 하나의 새로운 트렌젝션을 생성하여 각 데이터베이스에 전달. 
 

## 클러스터링(Clustering)

1. 두대 이상의 서버가 하나의 서버처럼 운영하는 기술

2. 고가용성 클러스터링과 병렬 처리 클러스터링이 있다.

* 고가용성 클러스터링: 하나의 서버에 장애가 발생하면 다른 서버가 받아 서비스를 수행하여 중단을 방지하는 방식. 가장 일반적으로 사용된다.

* 병렬 처리 클러스터링: 전체 처리율을 높이기 위해 하나의 작업을 여러 서버에서 분산 처리하는 방식.


참고: "시나공 정보처리기사 필기"

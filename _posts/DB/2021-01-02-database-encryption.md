---
title:  "데이터 베이스 보안 / 암호화"
excerpt: "데이터 베이스 보안 / 암호화"
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
## 데이터 베이스 보안 / 암호화

1. 데이터 베이스 보안이란 권한이 없는 사용자가 데이터 베이스의 일부 또는 전체에 대해서 데이터에 접근하는 것을 금지하는 것.


2. 암호화란 데이터를 송신자가 보낼때 수신자가 이외에는 그 내용을 알수 없도록 평서문을 암호문으로 변환하는 것.


* 암호화: 평서문을 말그대도 정보보호를 위해 암호문으로 바꾸는 과정


* 복호화: 암호화된 문장을 다시 평문으로 바꾸는 과정

 
3. 개인키 암호 방식(Primary Ket Encrytion) = 비밀키 암호 방식

* 동일한 키로 데이터를 암호화 하고 복호화를 한다.

* 비밀키는 제 3자에게 노출 시키지 않고 권한이 있는 사용자에게만 주어진다.

* DES(Data Encryption Standard) 가 대표적인 알고리즘며, 64bit 평문 블록을 56bit의 16개 키를 이용하여 16회의 계산 단계를 거쳐 64 bit의 암호문을 얻는다.

4. 공개키 암호 방식(Public Key Encryption)

* 서로 다른 키로 암호화와 복호화를 한다.

* 암호화할때 사용하는 키는 사용자에게 공개. 복호화할때 사용하는 키는 관리자가 관리.

* 공개키 암호화 기법은 비대칭 암호화  방식이라고도 하며, 대표적으로 RSA(Rivest Shamir Adleman) 이 있다.



---
title:  "[Swift] Identifiable 프로토콜"
excerpt: "[Swift] Identifiable 프로토콜"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Swift
<!-- tags:
  - Database 
last_modified_at: 2021-01-04 -->
---
# 메모리 관리
- 집마다 고유의 주소가 있듯이 메모리 역시 주소를 통해 접근하는 저장 장치이다. 이때의 주소(address)란 서로 다른 위치를 구분하기 위해 사용하는 일련의 숫자로 구성된다.
- 컴퓨터에서는 byte 단위로 메모리 주소를 부여하기 때문에 32비트 주소 체계를 사용하면 2^32 바이트만큼의 메모리 공간에 서로 다른 주소를 할당할 수 있다.
- 보통 4KB(2^12 byte)단위로 묶어서 페이지(page)라는 하나의 행적구역을 만든다. 페이지 하나의 크기가 2^12바이트이므로 페이지 내에서 바이트별 위치 구분을 위해서는 12비트가 필요하다. 따라서 총 
  32비트의 주소 중 하위 12비트는 페이지 내에서의 주소를 나타내게 된다.
  
## 1. 주소 바인딩
- 논리적 주소(logical address) : 프로그램이 실행을 위해 메모리에 적재되면 그 프로세스를 위해 생성되는 독자적인 주소(공간)이다. CPU는 프로세스마다 독립적으로 갖는 논리적 주소에 근거해 명령을 실행한다. 
  논리적 주소는 각 프로세스마다 독립적으로 할당되며 0번지부터 시작된다.
  
- 물리적 주소(physical address) : 물리적 메모리에 실제로 올라가는 위치를 말한다. 보통 물리적 메모리의 낮은 주소 영역에는 운영체제가 올라가고, 높은 주소 영역에는 사용자 프로세스들이 올라간다.

- 주소 바인딩(address biding) : 프로세스의 논리적 주소를 물리적 메모리 주소로 연결시켜주는 작업이다. 

- 주소 바인딩 방식은 프로그램이 적재되는 물리적 메모리의 주소가 결정되는 시기에 따라 세 가지로 분류할 수 있다.

<center><img src = "https://raw.githubusercontent.com/ronick-grammer/ronick-grammer.github.io/main/assets/images/OS/fd/paged segmentation.JPG"></center>

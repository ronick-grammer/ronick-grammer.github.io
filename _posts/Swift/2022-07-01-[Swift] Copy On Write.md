---
title:  "[Swift] Copy On Write"
excerpt: "[Swift] Copy On Write"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Swift
---

### Copy-On-Wirte (COW)
Copy-On-Write 란 데이터간에 값 복사가 이루어질때, 바로 복사가 이루어 지지 않고 데이터 수정시에 값복사가 이루어지는 메커니즘을 의미한다.

### Copy-On-Write 동작 조건
- 값 타입(`enum`, `struct`, `tuple`..)에만 동작한다
- 값 타입이지만 단일 데이터 타입(Int, Double 등)에는 동작하지 않는다
- Collection 타입(`String`, `Array`, `Set`, `Dictionary` ..)에서 동작한다
*[`String` 의 경우 문자길이가 15 를 초과해야만 Copy-On-Write가 동작하는 것처럼 보인다.](https://forestjae.tistory.com/30)
- 데이터의 값 복사 후, 한쪽 데이터의 값을 수정해야만 동작한다.

### Copy-On-Write 동작 과정
1. Collection 타입인 A와 B를 선언
2. A를 B에 대입 `B = A` : 동일한 메모리를 가르키고 있음(할당받은 메모리 주소가 같음)
3. A or B 의 값을 수정: 값을 수정한 변수에 새로운 메모리를 할당하여 복사 (Copy-On-Write)

### Copy-On-Write 의 이점
단일 데이터가 아닌 복수 데이터(Collection type)를 복사한 후 수정이 없을 경우, 메모리 할당에 드는 오버헤드를 줄일 수 있어 성능에 도움을 준다. 

### Copy-On-Write 의 단점
데이터 복사후 첫 수정시에 새로운 메모리 할당이 이루어지고 값 복사가 이루어지므로 이때 실행속도가 저하될 수 있다.

### 📝 참고 사이트
- [What is copy on write?](https://www.hackingwithswift.com/example-code/language/what-is-copy-on-write)
- [Understanding Swift Copy-on-Write mechanisms](https://medium.com/@lucianoalmeida1/understanding-swift-copy-on-write-mechanisms-52ac31d68f2f)
- [[Swift] COW(Copy-on-Write) by forestjae](https://forestjae.tistory.com/30)
- [Swift) COW (Copy-on-Write) by babbab2](https://babbab2.tistory.com/18)

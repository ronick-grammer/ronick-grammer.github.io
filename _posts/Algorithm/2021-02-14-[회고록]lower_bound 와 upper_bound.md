---
title:  "[회고록] lower_bound 와 upper_bound"
excerpt: "[회고록] lower_bound 와 upper_bound"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Algorithm
<!-- tags:
  - Database 
last_modified_at: 2021-01-04 -->
---
# lower_bound 와 upper_bound
이진 탐색과 관련하여 아주 유용한 C++ STL 함수가 있다. lower_bound(begin, end, key) 와 upper_bound(begin, end, key) 이다. 이 두 함수는 이진 탐색을 활용하여 key 값을 기준으로 값을 찾고 
그 위치를 반환한다. 물론 이진 탐색을 기반으로한 STL 함수이기 때문에 !!!반드시!!! 정렬후에 사용하여야 한다.
<br>
### lower_bound(begin, end, key)
lower_bound 함수는 key 이상인 값의 위치, 즉 key 값 이상이거나 같은 값의 최초 위치를 반환한다.

### upper_bound(begin, end, key)
lower_bound 함수는 key 보다 큰 값의 위치, 즉 key 값을 초과하는 값의 최초위치를 반환한다.

### 사용 예
정수 벡터를 사용한 예가 가장 이해하기 쉽겠지만 그건 너무 쉬우니 string 벡터를 활용한 두 함수의 사용 예를 들어보았다. 아래 사용 예는 카카오 신입 공채 문제중 하나인 
[가사 검색](https://programmers.co.kr/learn/courses/30/lessons/60060) 문제에서 활용될 수 있다.


```c++
int main(void){
    vector<string> v;
    v.push_back("frodo");
    v.push_back("front");
    v.push_back("frost");
    v.push_back("frame");
    v.push_back("kakao");

    sort(v.begin(), v.end()); // 이진 탐색을 진행할 것이기에 오름차순 정렬
    //frame, frodo, front, frost, kakao 순으로 정렬됨
    vector<string>::iterator s1 = upper_bound(v.begin(), v.end(), "frozz");
    vector<string>::iterator s2 = lower_bound(v.begin(), v.end(), "froaa");

    //frame, frodo, front, frost, kakao 순으로 정렬됨

    // froaa 보다 크거나 같은 문자열의 최초 위치
    cout<<(s2 - v.begin())<<'\n';
    // frozz 보다 큰 문자열의 위치
    cout<<(s1 - v.begin())<<'\n';

    // 결과값은 1 과 4 
    // froaa 이상인 값의 최초 위치는 frodo 의 위치인 1
    // frozz 의 값을 초과하는 최초의 위치는 kakao 의 위치인 4
}
```





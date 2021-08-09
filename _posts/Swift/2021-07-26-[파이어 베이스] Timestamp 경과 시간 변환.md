---
title:  "[Swift, Firebase] Timestamp 경과 시간 변환"
excerpt: "[Swift, Firebase] Timestamp 경과 시간 변환"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Swift, Firebase
<!-- tags:
  - Database 
last_modified_at: 2021-01-04 -->
---
# 파이어베이스 Timestamp 경과 시간 변환
- 경과 시간 변환.. 단어 선택이 모호하지만 딱히 다른 단어가 떠오르지 않는다. 쉽게 말하면 인스타그램 게시물 등록 시간을 보면 '2초 전', '17시간 전', '6일 전' 처럼 표시되어 있는 것을 본 적이 있을 것이다. 
  파이어베이스 Timestamp를 이처럼 변환시켜 보자.

<br>
<br>

### 예시) Timestamp 경과 시간 변환
```swift

import Firebase

struct TimestampString {
    static func dateString(_ timestamp: Timestamp) -> String {
        let formatter = DateComponentsFormatter()
        formatter.allowedUnits = [.second, .minute, .hour, .day, .weekOfMonth] // 시간 단위 설정
        formatter.maximumUnitCount = 1 // 시간 단위를 몇개를 나타낼 것인가
        formatter.unitsStyle = .abbreviated // 단위의 가장 앞글자 약어(s, m, h, d, w 등)으로 설정
        
        // 한글로 변환
        var calender = Calendar.current
        calender.locale = Locale(identifier: "ko")
        formatter.calendar = calender
        
        // 만들어진 시간부터 지금(Date())까지 얼마만큼의 시간이 걸렸는지 계산해서 차이(difference)를 반환
        return formatter.string(from: timestamp.dateValue(), to: Date()) ?? ""
    }
```

---
title:  "[Swift] Decodable 프로토콜"
excerpt: "[Swift] Decodable 프로토콜"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Swift
---
# Decodable 프로토콜
- 우선 공식 문서의 정의를 보면 "A type that can decode itself from an external representation." 라고 나와 있다.

- 즉, 외부의 데이터 표현식을 내부에 정의된 모델에 맞게 매핑 시켜서 가져오게 해주는 프로토콜이다.

- json 으로 예를 들면, json 데이터를 내부에 정의된 struct 나 class 인스턴스에 알맞게 매핑시켜 주는 거라 할 수 있겠다.

<br>
<br>

## 예시) 

```swift
import UIKit

struct User {
    let number: Int
    
    let userName: String

    let fullName: String

    enum CodingKeys: String, CodingKey { // 각 프로퍼티 명과 일치
        case number

        case userName

        case fullName
    }
}

extension User: Decodable {
    init(from decoder: Decoder) throws {
        // 각 키(enum)에 맞게 매핑
        let container = try decoder.container(keyedBy: CodingKeys.self)
        number = try container.decode(Int.self, forKey: .number)
        userName = try container.decode(String.self, forKey: .userName)
        fullName = try container.decode(String.self, forKey: .fullName)
    }
}

let jsonString = """
                {
                    "number": 1,
                    "userName": "batman",
                    "fullName": "Bruce Wane"
                }
                """

    let jsonData = jsonString.data(using: .utf8)

    do {
        let jsonData = jsonData
        
        // 매핑 (User struct의 init 호출됨)
        let dataModel = try JSONDecoder().decode(User.self, from: jsonData!)

        print(dataModel) // User(number: 1, userName: "batman", fullName: "Bruce Wane") 출력
    } catch let error {
        print("DEBUG: \(error.localizedDescription)")
  }

```

<br>
<br>

## 예시) 파이어베이스 연동시

```swift
import FireBase
import FirebaseFirestoreSwift

guard let uid = Auth.auth().currentUser?.uid else { return } // 현재 로그인 유저의 id를 가져오기

COLLECTION_USERS.document(uid).getDocument { snapshot, _ in // 현재 유저 id를 가지는 필드들 가져오기
  // 필드의 json 데이터를 User 클래스로 매핑하기
  guard let user = try? snapshot?.data(as: User.self) else { return }
}
```

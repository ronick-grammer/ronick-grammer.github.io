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
# Identifable 프로토콜
- 인스턴스의 유일성을 보장하기 위해 ID 값을 설정할 것을 강제하는 프로토콜

- Identifiable 프로콜의 정의는 아래와 같은데 보이는 것 처럼 id 값을 반드시 가지도록 하고 있다.

```swift
public protocol Identifiable {

    /// A type representing the stable identity of the entity associated with
    /// an instance.
    associatedtype ID : Hashable

    /// The stable identity of the entity associated with this instance.
    var id: Self.ID { get }
}
```

<br>
<br>

### 예시) 직접 키 할당
```swift
struct User: Identifiable {
    
    var id: Int // Identifiable 프로토콜을 따르기에 반드시 id 라는 프로퍼티를 가지고 있어야 한다.
    var userName: String
}

let user = User(id: 1, userName: "ronick")
```

<br>
<br>

### 예시) 유일키 생성

```swift
struct User: Identifiable {
    
    let id = UUID() // Identifiable 프로토콜을 따르기에 반드시 id 라는 프로퍼티를 가지고 있어야 한다.
    var userName: String
}

let user = User(userName: "ronick")
print(user.id) //   "2358318B-BFDD-449B-A86A-2B381E10F722" 식으로 출력
```

<br>
<br>

### 예시) 파이어베이스 연동하여 사용시

```swift
import FireBase
import FirebaseFirestoreSwift

struct User: Identifiable, Decodable {
    
    @DocumentID var id: String? // 파이어베이스의 documentId, 즉, uid를 매핑받을 프로퍼티
    var userName: String
}

guard let uid = Auth.auth().currentUser?.uid else { return } // 현재 로그인 유저의 id를 가져오기

COLLECTION_USERS.document(uid).getDocument { snapshot, _ in // 현재 유저 id를 가지는 필드들 가져오기
  // 필드의 json 데이터를 User 클래스로 매핑하기
  guard let user = try? snapshot?.data(as: User.self) else { return }
  print("DEBUG: User is \(user.id)") // "fv6lrDlhYAaB6wwAnfL55uRhbzl2" 식으로 출력
}
```

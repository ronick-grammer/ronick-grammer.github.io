---
title:  "운영체제의 개요"
excerpt: "운영체제의 개요"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Spring
<!-- tags:
  - Database 
last_modified_at: 2021-01-04 -->
---
# DTO 란?
DTO 란 Data Transfer Object 로서 데이터 전송 객체이다. 보통 Entity 클래스가 있으면 이에 대한 DTO 클래스를 생성하여 데이터에 접근한다. 이런 DTO 클래스는 json 데이터를 보낼 때와 받을 때 사용된다.
<br><br>

## Entity 클래스에 직접 데이터에 접근하지 않고 DTO 클래스를 생성하는 이유
Entity 클래스는 데이터베이스의 데이터들과 직접적으로 매핑되는 관계에 있기 때문에 내부에 Getter와 Setter를 두는 것은 위험하며 신중하지 못하다. 그렇기 때문에 Entity 클래스와 DTO 클래스를 분리하여 그 역할을 구분한다.
즉, Entity 클래스는 DB와의 데이터 연동 및 매핑에, DTO 클래스는 json 같은 객체와의 데이터 연동 및 매핑에 사용한다.


UserDTO.java

```java
public class UserDTO {
    final private Long seq;
    final private Email email;
    final private Passwd passwd;

    public UserDTO(Long seq, Email email, Passwd passwd) {
        this.seq = seq;
        this.email = email;
        this.passwd = passwd;
    }

    public Long getSeq() {
        return seq;
    }

    public Email getEmail() {
        return email;
    }

    public Passwd getPasswd() {
        return passwd;
    }
}
```


 





































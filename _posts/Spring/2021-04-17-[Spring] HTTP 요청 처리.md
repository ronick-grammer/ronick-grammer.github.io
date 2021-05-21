---
title:  "[Spring] HTTP 요청 처리"
excerpt: "[Spring] HTTP 요청 처리"
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
# 스프링 환경에서 HTTP 요청을 처리하는 방법

### - Get, Post 등의 HTTP 요청에 대한 응답으로서 객체(클래스)를 반환하면 객체가 JSON으로 변환되어 반환된다. 

```java
@ResponseBody        
@GetMapping("/api")
public User getUser(){
  return User("name", "password"); // {"name": "name", "password": "password"} 
}
```
\* 해당 매핑 메서드의 컨트롤러 클래스위에 @Controller 가 아닌 @RestController 를 사용하면 매핑 메서드 위에 @ResponseBody를 써줄 필요가 없다.

### - Path 경로에 동적인 id에 따른 처리를 할 수 있다.
```java       
@GetMapping("/api/{id}")
public void example(@PathVariable("id") long id){
  ....
  ....
}
```
\*  GetMapping의 {id} 와 @PathVariable("id") 의 id 는 서로 일치해야 한다.
    
### - Request로 들어오는 JSON 데이터를 HashMap 으로 받을 수 있다. 
```java       
public void example(@RequestBody HashMap<String, Object> h){
  String value = h.get("key").toString();
}
```
\*  그러나 HashMap 보다는 객체인 DTO 로 받는게 더 선호된다. DTO는 클래스를 새로 만들어야 하지만 데이터를 더욱 명시적으로 받을 수 있다.

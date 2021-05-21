---
title:  "[Spring] Mock 테스트"
excerpt: "[Spring] Mock 테스트"
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
## Mock과 가짜 객체

### 단위 테스트를 하려는데 외부 모듈과 연동하는 객체에 의존한다면 어떻게 테스트를 하는 것이 효율 적일까?
스프링에서 테스트를 하려다 보면 외부 모듈과 연동이 되어 있기 때문에 테스트를 하기가 좀 까다로울 수가 있다. 예를 들어 userService가 userRepository에 의존하고 있고 userRepository에서 DB 데이터를 다룬다면, userService의 일반적인 테스트코드를 작성하고 실행했을 때, userService와 userRepository 사이에 의존성이 존재하므로 실제 DB 와의 연동이 불가피하다. 단순히 userService 로직이 정확히 동작하는지 확인만 하고자 할 때, 불필요한 DB 연동없이 테스트 코드를 작성할 수 있게 하려면 우리는 DB 와 연동하여 데이터를 조작하는 코드를 담고있는 userRepository를 가짜 객체로 만들어야 한다. 이렇게 하면 userRepository는 빈껍데기인 가짜객체가 되고 DB와 연동이 일어나지 않으므로 효율적으로 테스트를 진행할 수 있다. 다만 userRepository가 아무런 기능도 하지 않는 빈껍데기가 되어버린 가짜 객체가 되었기 때문에, userService 내에서 userRepository의 특정 메서드를 호출하면 우리가 대신해서 호출된 메서드에서는 특정값을 반환한다는 것을 명시해주어야만 한다. 

- @Mock : 가짜 객체를 만든다. 어느정도 의존성이 필요하지 않을 때 사용하며 직접 DI를 해주어야 한다.
- @InjectMocks : @Mock 으로 가짜 객체가 된 기존에 의존하는 객체를 주입받는다. 

- @MockBean : 스프링의 Application Context가 해당 객체를 Mock 객체 빈으로 등록하며 이 @MockBean으로 선언된 객체와 같은 이름과 타입으로 이미 빈이 등록되어 있다면 해당 빈은 선언한 @MockBean으로 대체된다. 의존성이 있는 객체를 테스트 할 때 사용된다.<br>
\* 참고로 MockBean 의 사용 예시를 보고 또보고 수백번을 봐도 동작과정이 이해가 안된다..

```java
@Mock // userRepository는 가짜 객체가 되었다.
private UserRepository userRepository;

@InjectMocks // userService가 의존하는 위에서 Mock 객체가 된 userRepository를 주입(DI)받는다.
private UserService userService;

@Test
void getUsers() throws Exception { // 모든 유저 얻기
    // given
    List<User> users = getUserList();

    // when : userRepository는 빈껍데기인 가짜객체이기 때문에 아무런 기능을 하지 않는다. 그래서 우리가 특정 메서드 호출에 대한 반환값을 명시해주어야 한다.
    doReturn(users).when(userRepository).findAll(); 

    // do : userService.findUsers() 메서드는 userRepository.findAll() 메서드가 호출된 반환값을 반환한다. 그렇기 때문에 users가 위에서 명시한 것처럼 users 가 반환된다.
    List<User> userList = userService.findUsers();

    assertThat(userList.size(), is(users.size())); // 사이즈가 같다는 것을 확인할 수 있다. 
}
```

### MockMvc 으로 API 테스트를 하는 방법

그렇다면 단위 테스트가 아닌 컨트롤러의 API 테스트를 할 때는 Mock 을 이용한 테스트가 있는 걸까? 존재한다. 정확히는 MockMvc로 테스트를 할 수가 있다. 

- MockMvc : 애플리케이션을 서버에 배포하지 않고도 스프링의 MVC 테스트를 할 수 있게 해준다. 

```java
@Autowired
private MockMvc mockMvc;

@Test
void 코멘트_목록_가져오기() throws Exception{
    Long userId = 2L;
    Long postId = 4L;

    mockMvc.perform(get("/api/user/"+userId+"/post/"+postId+"/comment/list")
            .accept(MediaType.APPLICATION_JSON))
            .andExpect(status().isOk())
            .andDo(print());
}
```





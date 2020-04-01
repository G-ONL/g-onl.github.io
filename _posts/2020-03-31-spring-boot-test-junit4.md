---
layout: post
title: "Spring Boot Test (Junit 4) - 미완성"
comments: true
category: spring
description: >
  Test하는 방법 (+Junit 4)에 대해 알아보아요
---

## 경험으로 비춰보는 테스트의 필요성

테스트의 장점은 많은 것들이 있지만 그 중에서도 개발자의 불안감을 없애주는게 제일 크지 않을까 싶다.

지금 뒤늦게 투입된 프로젝트에서는 테스트 코드가 없는 상태에서 시스템 개발을 하고 마무리 단계에 있는데 어떤 수정사항이 생기면 제일 먼저 떠오르는 생각이 "고치다가 Side Effect이 나면 어쩌지..?" 이 생각이 제일 먼저 든다.

또한 위에 분들도 성능, 효율보다는 수정폭이 제일 적어서 Side Effect이 안나는 방향으로만 수정을 하시려고 한다. 물론 이렇게 개발을 하고 테스트를 하더라도 Side Effect이 다른곳에서 한 번씩 발생한다.

테스트 코드가 있었더라면 고칠 때 마다 주저하게 되는 부담감과 불안감, side effect이 많이 해소되지 않을까 싶다.

뒤늦게 투입된 상태이고, 신입이기에 배우고 익혀야 할께 많았다지만 모두 핑계라 생각하고, 다음번에 새 프로젝트에 투입이 된다면 테스트 코드부터 작성을 꼭 해야겠다.

## Junit

Junit 홈페에지를 가보면 Junit을 "programmer-friendly testing framework for Java" 라고 소개를 한다. 즉 프로그래머한테 친화적인 java 테스팅 프레임워크이다. CUnit(c), PyUnit(python).. 각각 언어들에 맞는 테스팅 프레임워크가 있다.
현재 Junit 5가 있지만 4도 아직 많이 사용함으로 우선 4를 익혀보고 이후에 5버전을 따로 정리하려 한다.


## 형태부터 살펴보자.

- 제일 먼저 의존성이 추가가 되어있어야 합니다.
~~~
 testImplementation('org.springframework.boot:spring-boot-starter-test')
~~~
dependencies 사이에 의존성을 추가해줍니다. 

Junit 4에서 단일 jar였지만 5로 가면서 JUnit Platform, JUnit Jupiter, JUnit Vintage 모듈로 쪼개졌다.

Junit 4로 작성된 코드가 잘 작동하기 위해서는 JUnit Vintage 모듈이 필수적이다.

위의 의존성을 추가하면 Junit vinatge 모듈도 같이 들어온다.

- 테스트를 하기 위한 class들을 만들어줍니다.

SampleService.java
~~~java
@Service
class SampleService {
 
 public String getName(){
   return "woopaloopa";
 }
}
~~~


SampleController.java

~~~java
@RestController
class SampleController {

  @Autowired
  SampleService sampleService;
  
  @GetMapping("/")
  public String hello(){
    return "Hello " + sampleService.getName();
  }
}
~~~

- Test를 작성하도록 합니다.

~~~java
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
class SampleControllerTest {

  @Autowired
  MockMvc mockMvc;

  @Test
  void Controller_get_Name() throws Exception {
    String hello = "Hello";
    String name = "woopaloopa";
    mockMvc.perform(get("/"))
        .andExpect(status().isOk())
        .andExpect(content().string(hello + " " + name))
        .andDo(print());
  }
}

~~~

제일 기본적인 형태의 테스트 형식 입니다.

- @RunWith

https://www.baeldung.com/spring-boot-testing 에 써있는 설명으로 대체하겠습니다.

~~~
@RunWith(SpringRunner.class) is used to provide a bridge between Spring Boot test features and JUnit. Whenever we are using any Spring Boot testing features in our JUnit tests, this annotation will be required.
~~~

@RunWith(SpringRunner.class)은 Junit과 Spring Boot test의 feature들을 연결하는 역할을 한다.
그래서 JUnit을 테스트 할 때, 어떤 Spring Boot test features을 사용하더라도 꼭 이 Annotation이 필요하다.

=> RunWith의 기능으로는 Test를 실행 할 때 JUnit에 내장된 Runner를 사용하는 대신 SpringRunner.class를 사용하게 끔 한다고 합니다.
SpringRunner는 ApplicationContext에 로딩되는 거를 도와주고, @Autowired를 적어놓은 테스트 인스턴스들을 반으로 만들어 가지고 있게끔 한다.
즉 많은 기능들을 지원하기에 SpringRunner를 사용하면 된다. SpringRunner.class을 사용하면 @MockBean, @SpyBean들을 Annotation으로 쓸 수 있다.

이게 Junit 5부터는 RunWith 대신 ExtendWith Annotation이 사용되는데 그 거는 5를 포스팅 할 때 다루도록 하겠습니다.

- @SpringBootTest

이 Annotation의 기본값은 WebEnvironment = WebEnvironment.MOCK 입니다.

이는 내장 톰캣이 구동을 안 합니다. 그러다보니 빠르게 테스트를 실행 할 수 있습니다.

MOCK말고 다른 값들은 RANDOM_PORT, NONE, DEFINED_PORT (이것도 default는 8080) 이 있습니다.

SpringBootTest는 @SpringBootApplication을 찾아서 테스트를 위해 빈들을 모두 생성합니다.

- @AutoConfigureMockMvc

이름에서도 알 수 있듯이 MockMvc에 대한 기본 자동 설정들을 해주기 때문에 따로 MockMvc를 설정 안하고 @Autowired로 주입받아서 사용 할 수 있습니다.

현재 WebEnvironment 상태가 기본 값인 MOCK으로 되어있기 때문에 실제 내장 서버가 작동되지는 않고 목업이 된 서블릿이 떠서 그 곳에 요청을 하기 위해서는 MockMvc를 통해서 요청을 날릴 수 있습니다.



~~~java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
@AutoConfigureMockMvc
class SampleControllerTest {

  @Autowired
  MockMvc mockMvc;

  @Test
  void Controller_get_Name() throws Exception {
    String hello = "Hello";
    String name = "woopaloopa";
    mockMvc.perform(get("/"))
        .andExpect(status().isOk())
        .andExpect(content().string(hello + " " + name))
        .andDo(print());
  }
}

~~~

MockMvc를 사용을 이 때도 할 수 있고, RANDOM_PORT 때는 실제 내장 톰캣이 사용이 되므로 TestRestTemplate을 사용 할 수 있습니다.

~~~java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
class SampleControllerTest {

  @Autowired
  TestRestTemplate testRestTemplate;

  @Test
  void Controller_get_Name() {
    String hello = "Hello";
    String name = "woopaloopa";
    String result = testRestTemplate.getForObject("/", String.class);
    assertThat(result).isEqualTo(hello + " " + name);
  }
}
~~~

Spring 5에서 webflux가 나오면서 WebTestClient가 나왔습니다. 다른점은 Async를 지원한다는 것입니다.

~~~java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
@AutoConfigureWebTestClient
class SampleControllerTest {

  @Autowired
  WebTestClient webTestClient;

  @Test
  void Controller_get_Name() {
    String hello = "Hello";
    String name = "woopaloopa";
    webTestClient.get().uri("/").exchange()
        .expectStatus().isOk()
        .expectBody(String.class).isEqualTo(hello + " " + name);
  }
}
~~~

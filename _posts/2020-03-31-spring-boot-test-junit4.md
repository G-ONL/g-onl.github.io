---
layout: post
title: "Spring Boot Test (Junit 4) - 미완성"
comments: true
category: spring
description: >
  Test하는 방법 (+Junit 4)에 대해 알아보아요
---

제일 기본이 되는 테스트의 형태입니다.

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

@SpringBootTest의 Default값은 WebEnvironment.MOCK 이다. 이는 내장 톰캣이 구동을 안 합니다.

MOCK말고 다른 값들은 RANDOM_PORT, NONE, DEFINED_PORT (이것도 default는 8080) 이 있습니다.

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

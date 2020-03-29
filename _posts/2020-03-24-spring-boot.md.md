---
layout: post
title: "Spring Boot"
comments: true
category: Spring
decription: >
  SpringBoot가 무엇인지, 어떻게 사용하는 것인지에 대해 알아봅시다.
---

## Spring Boot

Spring Boot는 [스프링 공식 문서](https://spring.io/projects/spring-boot)에서도 간단하게 정의가 되어있습니다.

~~~
Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can "just run".

We take an opinionated view of the Spring platform and third-party libraries so you can get started with minimum fuss. Most Spring Boot applications need minimal Spring configuration.
~~~

- Spring Boot를 사용하면 실행할 수 있는 상용화 급의 Spring기반 Application을 독립적으로 쉽게 구성하고 만들 수 있습니다.

-  third-party 라이브러리에 대한 설정, Spring Boot에 대한 설정을 최소한으로만 하면 된다.
   (즉 대부분의 사람들이 많이 쓰는 설정들을 바탕으로 기본 설정은 되어있다.)


## Spring Boot의 특징

- Create statnd-alone Spring applications
  독립형 Spring application을 만든다.

- Embed Tomcat, Jetty or Undertow directly (no need to deploy WAR files)
  톰캣,제티,언더토우를 내장하고 있다. (WAR 파일을 배포할 필요가 없다.)

- Provide opinionated 'starter' dependencies to simplify your build configuration


- Automatically configure Spring and 3rd party libraries whenever possible

- Provide production-ready features such as metrics, health checks, and externalized configuration

- Absolutely no code generation and no requirement for XML configuration

## Spring Boot의 구조

폴더구조

## @SpringBootApplication

@SpringBootApplication 사진

주요 Annotation 설명

- @ComponentScan

- @EnableAutoConfiguration

빈 등록 순서

~~~
@SpringBootApplication 안에 숨어있는 @EnableAutoConfiguration
빈은 두 단계로 나눠서 읽힘

@ComponentScan이 첫번째로

@ComponentScan은 Annotation이 붙어있는 클래스부터 하위 패키지를 싹 찾아서 Component가 달려있는 애노테이션을 찾아서 빈으로 등록해줌.

@Configuration, @Repository, @Service, @Controller, @RestController

다른 패키지에 있으면 빈으로 등록 x

자기 자신도 가능


@EnableAutoConfiguration은 Spring meta file (spring-boot-autoconfigure 프로젝트 밑에 META-INF 밑에 spring.factories 파일에 configuration 들이 쭉 있음 (기본 설정들)

조건에 따라서 달라짐.




~~~

## EnableAutoConfiguration
직접 AutoConfiguration 체험해보기
~~
@Configuration
src/main/resource/META-INF/spring.factories 파일을 만든다.
spring.gactories 안에 자동 설정 파일 추가
mvn install

다른 프로젝트에서 사용가능


이렇게 두 단계로 나뉘다 보니

끌어와서 사용하는 프로젝트에서 빈으로 등록을 하면 이후에 EnableAutoConfiguration으로 등록된 빈이 덮어씌어진다.


해결방법

@ConditionalOnMissingBean 을 적용해주면 됩니다.
이 타입의 빈이 없으면 그 떄 등록을 해준다.

빈 설정을 장황하게 해야하는가?


application.properties에 원하는 값으로 설정하면 안되나??

holoman.name = 이제 ~~
holoman.how-long = ~~
holoman.howLong 도 가능

@ConfigurationProperties("holoman")

~~


## Spring boot 의 Embed Servlet Contanier

~~~

내장 서블릿 컨테이너

- 스프링 부트는 서버가 아니다.
   - 톰캣 객체 생성
   - 포트 설정
   - 톰캣에 컨텍스트 추가
   - 서블릿 만들기
   - 톰캣에 서블릿 추가
   - 컨텍스트에 서블릿 맵핑
   - 톰캣 실행 및 대기

- 이 모든 과정을 생략해주게끔 하는 마법은 스프링 부트의 자동 설정.
  ServletWebServerFactoryAutoConfiguration (서블릿 웹 서버 생성)
   - TomcatServletWebServerFactoryCustomizer(서버 커스터마이징)
  DispatcherServlet ( httpServlet )
   - 서블릿 만들고 등록

서블릿 컨테이너는 달라질 수 있다. 
디스패쳐 서블릿은 안바뀜
그래서 나눠짐

~~~



## 참고자료

- 백기선님의 인프런 강좌 [스프링 부트 개념과 활용](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8)
- [스프링 공식 문서](https://spring.io/projects/spring-boot)
- [스프링 철저 입문](http://www.yes24.com/goods/detail/59192207)

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg2NTg0MDcyNF19
-->
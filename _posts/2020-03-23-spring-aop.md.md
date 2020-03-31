---
layout: post
title: "Spring Aop"
comments: true
category: spring
decription: >
  Aop가 무엇인지, 어떻게 사용하는 것인지에 대해 알아봅시다.
---

## AOP(Aspect Oriented Programming)


AOP 관점 지향 프로그래밍으로 횡단 관심사의 분리를 허용함으로써 모듈성을 증가시키는 것이 목적인 프로그래밍 기법입니다.

비즈니스 로직과는 별개로 중복된 행위들이 많이 일어나는 것들이 있습니다. 예를 들어 AOP 설명할 때 자주 등장하는 로깅, 트랜잭션, 보안 ..

이러한 로깅이나 트랜잭션, 보안의 경우 중요하지만 비즈니스 로직과 함께 계속해서 중복되게 사용되기에는 비효율적이고 코드가 지저분 해지기 때문에 모듈화를 시키는 것입니다.

그러면 비즈니스 로직과 분리가 되므로 OOP에 좀 더 가까워지겠죠.


## Spring AOP

자바 진영에는 이러한 AOP를 구현하기 위해 AspectJ가 만들어졌습니다.

AspectJ는 다양한 적용 시점(컴파일, 로드타임)를 가지고 있고, 수많은 기능들이 있지만 반대로 그렇기 때문에 사용하기 위해서는 사용자가 설정을 해줘야 하는 것들이 많았습니다.

그래서 스프링쪽에서 AspectJ에서 필요한 기능들 모아서 스프링 AOP를 만들게 되었습니다.

예를 들어 AspectJ는 위에서 언급했듯이 다양한 적용 시점을 가지고 있지만 스프링 AOP 런타임 시에만 적용이 됩니다. 

이렇게 제한적인 이유는 스프링 AOP는 모든 AOP기능을 제공하는 것이 목적이 아니라 스프링 IoC와 연동을 해서 엔터프라이즈 애플리케이션에서 가장 흔한 문제에 대한 해결책을 제공하는 것이 목적이기 때문입니다.

그렇기에 저희에게 일어날 수 있는 흔한 대부분의 문제를 큰 설정없이 편하게 해결 할 수 있습니다. 

스프링 AOP 스프링 빈에만 AOP를 적용할 수 있습니다.

런타임시에 빈을 만들 때 프록시 빈을 만들어서 적용.

프록시 패턴을 사용하는 이유는 기존의 코드를 변경하지 않고 부가 기능을 추가한다던가 접근 제어를 할 수 있기 때문입니다.

## AOP의 주요 키워드

AOP에는 키워드들이 여러개 있습니다.

Aspect , Advice, PointCut, Target, JoinPoint.

- Aspect : AOP 이름에도 들어가 있는 이 Aspect는 간단하게 모듈에 해당합니다.
- Advice : Aspect 모듈에서 해야하는 일, 즉 실제적으로 중복되었던 일에 해당합니다.
- PointCut : Aspect 모듈에서 Advice가 어디에 적용되는 지를 나타내는 부분입니다.
- Target : 위에서 언급했던 Aspect가 실제 적용되는 클래스들, 즉 중복되었던 코드들을 거둬낸 클래스들이겠죠.
- Join Point : (ex) 메서드 실행시점에, 메서드가 끝나고, 메서드 실행 전에 ..) 이렇게 끼워 넣을 수 있는 지점들을 Join Point라고 합니다.
               스프링 AOP에서는 메소드 조인포인트만 제공이 되고 있습니다.
        
                
이렇게 말로만 설명을 하면 사실 이해가 잘 가지 않을 수 있습니다.

밑에서 직접 코드를 보며 단어들의 의미를 익혀보겠습니다.

- ExampleAspect

~~~java
@Slf4j
@Component
@Aspect
public class ExampleAspect {

  @Around("@annotation(ExampleLogging)")
  public Object logging(ProceedingJoinPoint pjp) throws Throwable {
    log.debug("============ pre  ============= ");
    Object method = pjp.proceed();
    log.debug("============ end  ============= ");
    return method;
  }
}
~~~
- ExampleEventService
~~~ java
@Service
public class ExampleEventService implements EventService{

  @ExampleLogging
  @Override
  public void createEvent() {
    System.out.println("===createEvent");
  }

  @ExampleLogging
  @Override
  public void publishEvent() {
    System.out.println("===publishEvent");
  }

  @ExampleLogging
  @Override
  public void deleteEvent() {
    System.out.println("===deleteEvent");
  }
}
~~~

- ExampleLogging

~~~java
@Retention(RetentionPolicy.CLASS)
public @interface ExampleLogging {

}
~~~

매칭을 시켜보면 
- Aspect : ExampleAspect에 해당
- Advice : logging() 메서드에 해당 (실제 로깅하는 로직)
- PointCut : @annotation(PostLogging) => ExampleLogging 이라는 Annotation이 달려있는 메서드에 Advice를 적용한다. 
- Target : ExampleEventService 클래스, 더 정확히는 그 안에 ExampleLoggin이 적힌 메서드들이 Target이겠죠.
- Join Point : @Around Annotation의 의미가 @ExampleLogging이라는 Annotation을 단 Target이 실행되기 전 후에 Advice를 적용한다는 의미입니다.
               Advice가 언제 합류를 할 지를 명확히 나태내는 이 지점들이 조인 포인트라고 할 수 있습니다.

그림으로 보면 다음과 같습니다.

![관계도](https://user-images.githubusercontent.com/22094017/77385979-fcc8de80-6dcc-11ea-9806-76cc1f459f08.PNG)


## AOP로 변해가는 과정

위에서 최종족으로 AOP가 적용된 간단한 예제를 보았습니다. 그러면 처음에는 어떤 상태였고 어떻게 진행이 되어갔는지에 대해서 살펴보겠습니다.

처음에는 ExampleLoggin Annotation과 ExampleAspect라는 클래스가 없고, ExampleEventService라는 클래스만 있었습니다.

- ExampleEventService

~~~java
@Slf4j
@Service
public class ExampleEventService implements EventService{

  @Override
  public void createEvent() {
    log.debug("============ pre  ============= ");
    System.out.println("===createEvent");
    log.debug("============ end  ============= ");
  }

  @Override
  public void publishEvent() {
    log.debug("============ pre  ============= ");
    System.out.println("===publishEvent");
    log.debug("============ end  ============= ");
  }

  @Override
  public void deleteEvent() {
    log.debug("============ pre  ============= ");
    System.out.println("===deleteEvent");
    log.debug("============ end  ============= ");
  }
}
~~~

이렇게 메서드 위 아래로 로깅을 하기 위해서는 코드들이 반복되었습니다.

코드들이 반복되면서 나오는 문제점들은 다양하게 있습니다.

- 변경 사항이 일어나게 되면 모두 일일이 수정을 해야된다는 점.
- 코드가 지저분 해져서 한 눈에 파악하기 힘들다는 점(비즈니스 로직과 짬뽕).
- 불필요한 작업으로 인한 속도 지연.


우선 프록시 패턴부터 살펴보겠습니다.

- SimpleEventService
~~~java
@Service
public class ExampleEventService implements EventService{

  @Override
  public void createEvent() {
    System.out.println("===createEvent");
  }

  @Override
  public void publishEvent() {
    System.out.println("===publishEvent");
  }

  @Override
  public void deleteEvent() {
    System.out.println("===deleteEvent");
  }
}
~~~

- ProxySimpleEventService

~~~java
@Slf4j
@Primary
@Service
public class ProxySimpleEventService implements EventService{

  @Autowired
  SimpleEventService SimpleEventService;

  @Override
  public void createEvent() {
    log.debug("============ pre  ============= ");
    simpleEventService.createEvent();
    log.debug("============ end  ============= ");
  }

  @Override
  public void publishEvent() {
    log.debug("============ pre  ============= ");
    simpleEventService.publishEvent();
    log.debug("============ end  ============= ");
  }

  @Override
  public void deleteEvent() {
    log.debug("============ pre  ============= ");
    simpleEventService.deleteEvent();
    log.debug("============ end  ============= ");
  }
}
~~~

우선 SimpleEventService부터 살펴보게 되면 코드가 원하는 비즈니스 로직만을 남기고 불필요했던 부분(로깅)이 사라졌습니다.

이에 따라 코드를 파악하기 좀 더 쉬워지겠죠.

하지만 ProxySimpleEventService라는 클래스가 부가적으로 하나가 더 생기게 되었고, 여전히 반복적인 코드(로깅)는 남아있습니다.

이렇게 되었을 때 문제점은 클래스들이 추가되면 Proxy 클래스가 부가적으로 계속 생성해줘야하고 코드를 반복적으로 생성해줘야 합니다.

이후에는 Dynamic Proxy, Spring의 AutoProxyFactoryBean ..등을 이용해서 점점 문제점을 해결해 왔습니다.

그리고 최종적으로 Aspect와 Annotation,Bean.. 을 사용한 간결한 패턴이 등장했습니다.

- ExampleAspect

~~~java
@Slf4j
@Component
@Aspect
public class ExampleAspect {

  @Around("@annotation(ExampleLogging)")
  public Object logging(ProceedingJoinPoint pjp) throws Throwable {
    log.debug("============ pre  ============= ");
    Object method = pjp.proceed();
    log.debug("============ end  ============= ");
    return method;
  }
}
~~~
- ExampleEventService
~~~ java
@Service
public class ExampleEventService implements EventService{

  @ExampleLogging
  @Override
  public void createEvent() {
    System.out.println("===createEvent");
  }

  @ExampleLogging
  @Override
  public void publishEvent() {
    System.out.println("===publishEvent");
  }

  @ExampleLogging
  @Override
  public void deleteEvent() {
    System.out.println("===deleteEvent");
  }
}
~~~

- ExampleLogging

~~~java
@Retention(RetentionPolicy.CLASS)
public @interface ExampleLogging {

}
~~~

내가 사용할 부분의 메서드에 Annnotation만 달아주면 끝.
비즈니스 로직을 최대한 해치지도 않고, 중복도 없고, 문제점들이 해소가 된 것을 알 수 있습니다.

그러면 Aspect에 대해 좀 더 살펴보겠습니다.

## Advice 종류 5가지

스프링 AOP의 경우에는 (5가지 시점을 지원합니다.)

- Before Advice : 조인 포인트 전에 수행된다.예외가 발생하는 경우만 제외하고 항상 실행된다.
![Before](https://user-images.githubusercontent.com/22094017/77384420-aeb1dc00-6dc8-11ea-9ed5-390cee0fe299.PNG)
- After Returning Advice : 조인 포인트가 정상적으로 종료한 후에 실행된다. 예외가 발생하면 실행되지 않는다.
![After Returning](https://user-images.githubusercontent.com/22094017/77384518-f0428700-6dc8-11ea-8f8b-7a4fe211c471.PNG)
- After Throwing Advice : 조인 포인트에서 예외가 발생했을 때 실행된다.예외가 발생하지 않고 정상적으로 종료하면 실행되지 않는다.
![After Throwing](https://user-images.githubusercontent.com/22094017/77384504-e1f46b00-6dc8-11ea-89e7-a6870225e30f.PNG)
- After Advice : 조인 포인트 완료 후 실행된다.예외 발생 여부와 관계 없이 항상 실행한다.
![After](https://user-images.githubusercontent.com/22094017/77384473-ce490480-6dc8-11ea-8559-d9b9bf634eac.PNG)
- Around Advice : 조인 포인트 전후에 실행된다.
![Around](https://user-images.githubusercontent.com/22094017/77384491-d7d26c80-6dc8-11ea-9675-6ec526a47e07.PNG)



## 용어에 대한 견해

- 백기선님의 [Q&A 1](https://www.inflearn.com/questions/21173)
- 백기선님의 [Q&A 2](https://www.inflearn.com/questions/29032)

백기선님의 의견을 위에서 주로 다뤘는데 Advice는 로직에 해당이 되고 JoinPoint는 합류점 언제 합류를 하느냐 (@Around, @Before..)에 초점을 맞추시고 설명을 해주셨다.

- 이동욱님의 블로그 [기억보단 기록을](https://jojoldu.tistory.com/71)

이동욱님의 블로그에서는 Advice는 Aspect가 "무엇"을 "언제"할지를 정의하고 있다고 하셨다. (언제 => @Around, @Before ...)
JoinPoint의 경우 Advice가 적용 될 수 있는 위치를 이야기한다고 하셨습니다. 즉 Target의 메서드 (Advice에서 파라미터로 넘어오는 값)

- [공식 문서](https://docs.spring.io/spring/docs/4.3.12.RELEASE/spring-framework-reference/html/aop.html) 

- Join point: a point during the execution of a program, such as the execution of a method or the handling of an exception. In Spring AOP, a join point always represents a method execution.
 메서드 실행이나 예외 발생 시점을 이야기한다. 스프링 AOP에서는 메서드 실행만 이야기한다. 

- Advice: action taken by an aspect at a particular join point. Different types of advice include "around," "before" and "after" advice. (Advice types are discussed below.) Many AOP frameworks, including Spring, model an advice as an interceptor, maintaining a chain of interceptors around the join point.
(특정 조인 포인트에서 Aspect에 의해 실행 될 부분)


## 참고자료

- 백기선님의 인프런 강좌 [스프링 프레임워크 핵심 기술](https://www.inflearn.com/course/spring-framework_core)
- + 백기선님의 [Q&A 1](https://www.inflearn.com/questions/21173)
- + 백기선님의 [Q&A 2](https://www.inflearn.com/questions/29032)
- 이동욱님의 블로그 [기억보단 기록을](https://jojoldu.tistory.com/71)
- [스프링 철저 입문](http://www.yes24.com/goods/detail/59192207)

<!--stackedit_data:
eyJoaXN0b3J5IjpbOTU1NDMyODFdfQ==
-->

---
layout: post
title: "IoC컨테이너"
comments: true
category: spring
decription: >
   IoC Container가 무엇인지, 어떻게 사용하는 것인지에 대해 알아봅시다.
---

# IoC Container

IoC = Inversion of Control (제어의 역전)에 대해 알아봅시다.

IoC Container안의 객체들을 빈(Bean)이라고 하는데 이러한 빈들을 관리하는 곳을 IoC Container라고 합니다.

IoC Container를 사실상 Bean Factory라고 생각을 하면 되고, 그 Bean Factory를 ApplicationContext가 상속받아서 구현이 되어있습니다.

![BeanFactory](https://user-images.githubusercontent.com/22094017/76275617-0fb8ba80-62c7-11ea-92ac-2b53f53511c8.PNG)

Spring에서는 이 ApplicationContext를 이용해서 Bean의 관리, 의존성 관리를 하게 됩니다.

직접적으로 ApplicationContext를 코드상에서 사용할 일은 거의 없습니다.

IoC Container가 주로 하게 되는 일은 **빈을 만들고 엮어주고 제공하는 것** 입니다.

1. 빈을 만들어 주고
2. 엮어주고
3. 제공하는 것

크게 세 가지로 나눠 볼 수 있습니다.

## 빈 생성

빈을 만들어주는 방법도 다양하게 있습니다.
 
### 1. Annotation을 이용하는 방법

### 2. 직접 등록(@Bean)

### 3. 자바 Configuration

### 4. xml 파일을 이용하는 방법

### 5. 상속

첫 번째의 Annotation을 이용하는 방법은 단순하게 클래스 위에 @Component, @Controller, @Service, @Repository 등을 붙여주게 되면 Spring이 구동이 되면서 @ComponentScan을 통해서 위의 Annotation으 붙은 클래스들을 빈으로 생성시켜 줍니다. 주로 **개발자가 직접 작성한 Class를 Bean으로 등록하기 위한 Annotation** 입니다.

두 번째도 Annotation을 이용하는 것이지만 용도가 조금 다른데 @Bean은 **개발자가 직접 제어가 불가능한 외부 라이브러리등**을 Bean으로 만들려고 할 때 주로 사용됩니다.

그러면 이렇게 빈으로 등록해서 관리하는 것이 어떤 이점이 있을까?
### 1.	Scope에 따른 이점

### 2.	Life Cycle 관리 가능

### 3.	의존성(Dependeny) 관리 가능

빈의 Scope는 기본적으로 싱글톤 scope을 가지고 있습니다.

이에 반대말로는 프로토타입이라고 합니다. 즉 우리가 기존의 자바를 이용할 때 객체를 생성자를 이용해서 직접 생성해주고 필요할 때마다 생성을 하던 것은 프로토타입이라고 생각하면 됩니다.
싱글톤의 Scope을 가지고 있게 된다는 것은 어디서 그 객체를 호출을 해도 동일한 레퍼런스를 바라보고 있는 똑같은 객체를 준다는 말이고, 이는 멀티쓰레드 환경에서 동일 객체를 보장 할 수 있습니다. 이러한 이점말고도 객체를 필요할 때마다 생성을 해주는 것은 자원을 사용한다는 것에서도 성능 저하가 있을 수 있습니다. 미리 애플리케이션이 실행 될 때 빈들을 모두 만들어 놓고 필요할 때 마다 만들어 놓은 빈을 가지고 사용하는 것이 성능상에도 이점이 있습니다.

빈의 Life Cycle에 있어서는 나중에 다뤄보도록 하겠습니다.
간단하게 말하면 빈의 생성 주기가 있는데 그것을 이용해서 빈이 초기화 되기 전 혹은 관계를 형성 하기 전, 후 등 시점에 따라 다양한 액션을 처리해 줄 수 있습니다.

마지막으로 의존성 관리인데 기본적으로 IoC Container안에 있는 객체들끼리 의존성을 주고받을 수 있다. 이렇게 IoC Container안에서 Bean의 의존성을 주고 받을 수 있다보니 이에 따른 여러 이점이 있을 수 있습니다. (재사용성, 테스트 용이, 코드의 단순화, 종속적인 코드의 수 감소,결합도가 낮아짐에 따른 유연성 및 확장성 향상 ..)  사실 모두 연결되어있는 말 입니다. 결합도가 낮아지면 코드의 단순화와 종속적인 코드의 수의 감소, 테스트 용이한 것, 재사용성등 모든 것을 따로 오게 합니다.

## 의존성 주입

빈끼리 엮여준다는 것이 바로 의존성을 주입하는 것인데요. 의존성을 외부에서 주입함으로써 얻는 이점은 위에서 간단하게 말씀을 드렸고, 여기서는 그러면 어떻게 의존성을 주입하는지 알아보겠습니다.

의존성을 주입하는 방법에는 세 가지가 있습니다.

### 1.	필드

### 2.	Setter

### 3.	생성자

하나씩 코드로 살펴보겠습니다.

먼저 필드로 의존성을 주입하는 방법을 보겠습니다.
![Autowired](https://user-images.githubusercontent.com/22094017/76220656-36410c00-625b-11ea-88c8-2ef6072c58af.png)

그 다음은 Setter를 이용한 주입 방법을 보겠습니다.

![Setter](https://user-images.githubusercontent.com/22094017/76220686-42c56480-625b-11ea-88e4-a7dff7b23cd6.png)

마지막으로 생성자를 이용한 주입 방법을 보겠습니다.

![Construct](https://user-images.githubusercontent.com/22094017/76220666-3b05c000-625b-11ea-9e38-1b6cf59d607c.png)

생성자의 경우는 @Autowired가 붙어 있지 않는걸 볼 수 있습니다.
이는 Spring 4.3이후로 생성자가 한 개이고, 생성자의 파라미터가 빈으로 등록되어 있다면 @Autowired를 생략 가능합니다. 또한 저 3개의 방법 중에서도 공식적으로 생성자를 통한 주입을 권장하고 있습니다. 

그 이유는 순환참조, Coupling 등의 문제가 있기도 하고, 테스트를 하기에도 생성자를 통한 주입을 할 떄가 편하기 때문입니다.

생성자에 일일이 적는 것은 귀찮고, 수정시에도 바꿔야 하기 때문에 Lombok의 Annotation을 이용하면 간단하게 만들 수 있습니다.

![Construct_Lombok](https://user-images.githubusercontent.com/22094017/76220679-40fba100-625b-11ea-90f9-7256e2553ba2.png)


두 개 이상의 동일한 빈이 있을 때는 어떤 것을 Autowired 해야 할지 모르기 때문에 오류가 발생을 .

![error](https://user-images.githubusercontent.com/22094017/76285273-b3639400-62e2-11ea-9825-27eb5a0d9913.PNG)

이렇게 Application을 시작하면 밑에 에러처럼 어떤거를 주입해야 될지 모르겠다는 것을 알 수 있습니다.


해결 방법으로는 

### 1.	@Primary

![primary](https://user-images.githubusercontent.com/22094017/76285275-b494c100-62e2-11ea-8780-d4cdf57b0a55.PNG)
@Primary를 이용하면 동일한 인터페이스를 상속받은 애들 중에서 특별한 언급이 없으면 @Primary를 붙인 빈을 주입시킵니다.

### 2.	@Qualifier

![Qualifier](https://user-images.githubusercontent.com/22094017/76285278-b65e8480-62e2-11ea-9650-b2655f0703e6.PNG)
@Qualifier는 그 때 당시에 주입을 받을 때 어떤 빈을 주입받을지 정할 수 있습니다.

![동시에](https://user-images.githubusercontent.com/22094017/76285281-b8284800-62e2-11ea-825b-26f764843a85.PNG)
@Primary 와 @Qualifier를 동시에 사용하게 되면 일반적으로는 Primary를 적어놓은 빈을 주입하다가 @Qualifier가 써있는 필드값에 만 거기에 적어놓은 빈을 넣습니다.

### 3.	해당 클래스명으로 변수명 설정

![field](https://user-images.githubusercontent.com/22094017/76285282-b9597500-62e2-11ea-81ba-34aa785af6da.PNG)

이 방법은 변수명을 빈 이름과 동일하게 적어주게 되면 다른 Annotation없이 적용이 됩니다.

### 4.	List,Map으로 여러 개를 받을 수도 있다.(해결법이라기 보다는 다른 용도)

![List](https://user-images.githubusercontent.com/22094017/76285271-b199d080-62e2-11ea-900a-c75ef6fb5375.PNG)

이 방법은 해결법이라기 보다는 용도가 조금 다를 수 있다. 아예 List로 중복된 빈 들을 모두 받아서 사용할 수 있습니다.

여기서 Primary를 사용하는게 타입의 안전성과 비즈니스 로직상의 코드를 건드리지 않아도 되기에 제일 추천합니다.

다만 상황에 따라 조금 다를 수 있으니 잘 취사선택을 해야겠습니다.


## ApplicationContext 제공

의존성을 주입하고 나면 사실 주입을 받은 상태의 것을 사용을 바로 하면 됩니다. 즉 2,3이 합쳐져 있다고 생각을 하면 됩니다.

ApplicationContext를 이용하면 Bean과 관련된 다양한 메서드를 사용할 수 있습니다. 

![ApplicationContext](https://user-images.githubusercontent.com/22094017/76221365-51604b80-625c-11ea-94a5-05558a027d00.png)



## 참고자료

- 백기선님의 인프런 강좌 [스프링 프레임워크 핵심 기술](https://www.inflearn.com/course/spring-framework_core)
- [스프링 철저 입문](http://www.yes24.com/goods/detail/59192207)

<!--stackedit_data:
eyJoaXN0b3J5IjpbMjE0MDk5NDE0MF19
-->
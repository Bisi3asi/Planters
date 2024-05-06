# [면접 대비] [Spring] 빈출 기술 질문 & 정답 정리

<br>

> [기술 질문 & 정답 활용법]
> 1. 질문에 대한 답변을 생각한 후 말해봅니다.
> 2. 답변 후 정답과 비교해보는 식으로 진행해보세요.
> 3. 반복하시면서 습득하시면 좋습니다.

<br>

<details>
<summary><strong>Q. Spring과 Spring Boot, Spring MVC의 차이점에 대해 설명해주세요.</strong></summary>

<br>

* **Spring은 Java 기반 애플리케이션 개발을 지원하는 오픈소스 프레임워크이다.**
  * POJO만을 이용해 복잡성을 제거하고, 가벼운 코드로 기업용 애플리케이션을 제작하는 기능을 제공하는데 그 목적이 있다.
  * Spring에서는 개발자가 직접 스프링 컨테이너 구성, 빈 객체 등록, 의존성을 설정해야 하는 번거로움이 있다.
  
<br>

* **반면, Spring Boot는 기존 Spring에서 보일러 플레이트 코드를 최소화하고 자동 구성을 통해 빠르게 애플리케이션 개발에 착수할 수 있도록 하는 Spring 프레임워크의 종류이다.**
  * Spring과의 차이점
    * 자동 의존성 주입과 빈 객체 등록, 스프링 컨테이너 구성이 가능하다.
    * Tomcat과 같은 임베디드 서버를 제공해 jar 파일로 엑스포트가 가능하다.
    * Spring MVC, Spring Data JPA, Spring Security 등 다른 Spring Framework 모듈에 대해 선택적으로 자동 설정이 가능하다.   

<br>

* Spring MVC는 웹 애플리케이션 기반 개발에 있어 MVC 패턴 관련 설정을 제공하는 Spring 프레임워크의 종류이다.
  * DispatcherServlet, ViewResolver, ModelAndView 등의 기능을 제공한다.

<br>

> 참고 : Spring 공식 사이트는 Spring Boot와 Spring MVC 또한 프레임워크로 규정하고 있다. <br>
> `프레임워크 내부의 프레임워크로 이해하면 편할 것 같다..`

<br>

[#REFERENCE, Spring vs Spring Boot vs Spring MVC](https://velog.io/@lucaschoi/Spring-vs-Spring-boot-vs-Spring-MVC)

<br>

</details>

<br>

<details>
<summary><strong>Q. Spring IOC에 대해 설명해주세요.</strong></summary>

<br>

* IOC는 Inversion Of Control의 약자로, 제어의 역전이라는 사전적 의미를 가지고 있다.
* **이는 객체의 생성과 생명주기 관리를 프레임워크가 담당하게 하며, 개발자는 비즈니스 로직에 더 집중하게 하는 디자인 패턴이자 편의 기능을 의미한다**.
* Spring에서의 IOC는 Application Context가 해당 역할을 담당하며, Application Context는 Bean 객체들의 생성, 설정, 관리 등 객체의 전체 생명주기를 담당한다.
* 이러한 방식은 개발자가 직접 객체 간 의존성을 관리하지 않아도 된다.

<br>

* [#REFERENCE, 스프링 프레임워크의 IOC](https://f-lab.kr/insight/understanding-spring-ioc-di?gad_source=1&gclid=Cj0KCQjw_-GxBhC1ARIsADGgDjtex4IqBSZkF97OPRnSYJty7VVlel7DXEVZBet95rQok90s84iRm6gaAsueEALw_wcB)
* [#REFERENCE, BeanFactory와 ApplicationContext](https://velog.io/@saint6839/BeanFactory-%EC%99%80-ApplicationContext%EC%9D%98-%EC%B0%A8%EC%9D%B4)

<br>

<details>
<summary><strong>QQ. Spring IOC 컨테이너에 대해 설명해주세요.</strong></summary>

<br>

* **IOC 컨테이너란, IOC 디자인 패턴에 의해 생성된 객체들의 생명주기와 의존성을 관리하는 컨테이너이다.**
* 이렇게 IOC 컨테이너에 의해 관리되는 객체를 Spring Bean이라고 한다.
* IOC 컨테이너의 종류에는 크게 Bean Factory와 Application Context가 있다.
  * Bean Factory : Spring Bean의 생명주기를 관리 및 의존성 설정을 담당하는 기본적인 IOC 컨테이너이자 최상위 인터페이스
  * Application Context : Bean Factory에서 확장된 형태로, 국제화 기능 및 이벤트 관리 기능이 추가

<br>

<details>
<summary><strong>QQQ. Spring IOC 컨테이너의 역할에 대해 설명해주세요.</strong></summary>

<br>

* **애플리케이션 시행 시점에 Spring Bean 오브젝트를 인스턴스화 하고, DI를 실시한 이후 최초로 애플리케이션을 실행할 하나의 Bean을 제공한다.**

</details>
</details>
</details>

<br>

<details>

<summary><strong>Q. Spring Bean에 대해 설명해주세요.</strong></summary>

<br>

* **Spring Bean이란 IOC 컨테이너에 의해 관리되는 객체를 의미한다.**
* IOC 패턴에 의해 생성과 제어권이 개발자가 아닌 스프링 프레임워크에서 관리되는 객체이다.
* 등록 방법 : xml을 통한 방법, 어노테이션을 통한 방법
  * (@Configuration-@Bean, @Component 이후 Component Scan, @SpringBootApplication을 통한 Component Scan 등)
* 이러한 Spring Bean은 IOC 컨테이너에 의해 의존성 주입(DI, Dependency Injection) 된다.

<br>

> Spring Bean과 Java Bean의 차이
> * Java Bean : Java로 작성된 객체이며, 데이터 표현을 목적으로 한다.
>   * private 속성의 멤버 변수를 가지고 있으며, 멤버 변수에 대한 설정자와 접근자를 가진다. 
> * Spring Bean : IOC 컨테이너에 의해 생명주기가 관리되는 Java 객체를 의미한다.

</details>

<br>

<details>
<summary><strong>Q. Spring @Bean과 @Component에 대해 설명해주시고, 차이점을 설명해주세요. </strong></summary>

<br>

* **두 어노테이션 모두 Spring IOC 컨테이너에 Bean을 등록하기 위해 사용된다.**
  * 두 어노테이션 모두 선언된 객체를 기반으로 실행 시점에 인스턴스 객체를 1회 (싱글톤) 생성한 후 이용한다.
* **두 어노테이션의 차이점은 선언하는 레벨의 차이로, @Bean은 메소드 레벨에서, @Component는 클래스 레벨에서 선언된다는 차이가 있다.**
* @Bean
  * 개발자가 컨트롤이 불가능한 외부 라이브러리가 제공하는 객체의 메소드에 사용된다.
  * 외부 라이브러리 클래스 레벨에 @Configuration을 명시하며, 메소드 레벨에 @Bean을 명시해 반환되는 객체를 수동으로 Bean으로 등록한다.
    * @Configuration의 내부에는 @Component가 포함되어 있어 런타임 시 컴포넌트 스캔이 가능하다.
* @Component
  * 개발자가 컨트롤이 가능한 내부 클래스 레벨에 @Component를 명시해 해당 객체를 Bean으로 등록한다.
  
<br>

[#REFERENCE, @Bean과 @Component의 차이](https://youngjinmo.github.io/2021/06/bean-component/)

</details>

<br>

<details>
<summary><strong>Q. Spring의 DI에 대해 설명해주세요.</strong></summary>

<br>

* **DI는 Dependency Injection의 약자로, 의존성 주입을 의미한다.**
  * DI는 외부에서 객체 간의 관계를 결정하는 것으로, 객체를 직접 생성하는 것이 아닌 외부에서 생성후 주입시켜주는 방식이다.
  * 즉, DI를 통해 객체 간의 관계를 동적으로 주입해 유연성을 확보하고 결합도를 낮출 수 있다.
* **즉, Spring DI는 IOC 컨테이너에 의해 생성된 Java Bean 객체를 필요로 하는 외부 컴포넌트에 관계를 동적으로 주입하는 과정이다.**

<br>

<details>
<summary><strong>QQ. Spring DI의 종류와 작동 방식에 대해 설명해주세요. </strong></summary>

<br>

* **Spring DI에는 필드 주입과 Setter 주입, 생성자 주입이 있다.**
* Field 주입 (Field 주입)
    * 기초적인 주입 방식으로 Spring에서는 필드에 @Autowired를 명시해 의존성을 주입한다.
    * 장점
        * 가독성, 사용하기 편리하다.
    * 단점
        * 스프링 DI 컨테이너에서만 동작할 수 있는 의존성으로 인해 Java 코드 선에서는 테스트가 불가능하다.
        * 불변성이 보장되지 않는다.
        * 순환참조 문제가 발생할 수 있다.
        * 의존성이 특정 컨테이너에 의해 숨겨지게 된다.
* Setter 주입 (Setter Injection)
    * 선택적이며, 변경 가능한 의존 관계에 사용된다.
    * Spring Bean을 선택적으로 등록할 수 있다.
* 생성자 주입 (Constructor Injection)
    * 생성자 호출 시점에 한 번만 호출되므로, 해당 객체의 불변 상태를 보장한다.
    * Null Pointer Exception을 방지 할 수 있다.
    * 해당 객체가 바꿔치기 당할 일이 없으며, 호출하는 객체의 고유성과 명확성을 보장한다.

<br>

> DI Framework의 핵심 아이디어는 관리되는 클래스가 DI Container에 대한 의존성이 없어야 한다는 것이다.
> * 즉, 필요 의존성을 전달하면 독립적으로 인스턴스화 될 수 있는 POJO여야 한다.
> * 그러나, Spring DI에서의 필드 주입은 필요한 의존성을 가진 Class를 곧바로 인스턴스화 시킬 수 없다는 단점이 있다.
> * `POJO` Plain Old Java Object의 줄임말로, 이후 후술한다.

<br>

[#REFERENCE, DI의 세가지 방법](https://velog.io/@gillog/Spring-DIDependency-Injection-%EC%84%B8-%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95)

</details>

<br>

<details>
<summary><strong>QQ. Spring DI에서 의존성 주입 과정에 대해 설명해주세요.</strong></summary>

<br>

* **Spring IOC 컨테이너에서 타입(해당 클래스)을 이용해 의존 대상 객체를 검색하고, 이를 할당할 수 있는 Bean 객체를 찾아 주입한다.**
* **이 과정을 autowiring이라고 부른다.** 
* 이 과정에서 해당하는 클래스의 메타 데이터를 읽어오기 위해 Java Reflection API가 사용된다.
* 빈을 생성한 후에 IOC 컨테이너에 의해 Bean 객체가 autowiring 되며, autowire 된 시점 이후 Bean 객체의 초기화가 진행된다.

</details>

</details>

<br>

<details>
<summary><strong>Q. POJO는 무엇이며, Spring Framework 입장에서 POJO는 무엇이 될 수 있을지 설명해주세요.</strong></summary>

<br>

* **POJO는 Plain Old Java Code의 약자로, 외부 인터페이스나 API에 종속되지 않는 Java 순수 코드이다.**
  * POJO는 특정 환경에 종속되지 않기 때문에 유연하며, 단위 테스트가 용이하다.
* **Spring Framework 입장에서 POJO는 비즈니스 로직과 도메인 단의 작업을 수행하는 대상이다.**
  * 이는 비즈니스 로직과 도메인이 POJO가 되게끔 구성해야 됨을 의미한다.
  * 따라서 POJO로 비즈니스 로직과 도메인이 구성될 수 있게끔 인프라 로직을 분리하는 역할을 수행하는 것이 Spring AOP이다.

</details>

<br>

<details>
<summary><strong>Q. AOP가 무엇인지, 그리고 Spring AOP에 대해 설명해주세요.</strong></summary>

<br>

* **AOP는 Aspect Oriented Programming의 약자로, 관점 지향 프로그래밍을 의미한다.**
* **AOP는 어떠한 로직을 부가 로직(인프라 로직), 핵심적인 로직(비즈니스 로직)으로 나누어, 각 로직을 관점(Aspect) 단위로 묶어 모듈화하는 기법이다.**
  * `ex)` 하나의 기능에 포함되는 부가 로직(파일 입출력, 서비스 호출 등)을 보일러 플레이트 코드로 간주해, 이를 각자 모듈화하는 것
* 즉, AOP는 재사용되는 코드를 모듈화하여, 개발자로 하여금 비즈니스 로직을 구현하는데 집중하는 기능을 한다.

<br>

* **Spring AOP는 이러한 AOP를 지원하는 기술로, 트랜잭션 관리, 로깅 등의 인프라 로직을 각각 모듈화해 필요 시 호출하는 기능을 지원한다.**
* 따라서, AOP를 통해 인프라 로직을 분리함으로써, 개발자는 비즈니스 로직 구현에 집중할 수 있다.
* 대표적인 빌트인 AOP에는 @Transactional, @Secured, @Pre/PostAuthorized가 있다.
* 개발자는 관점에 따라 @Aspect를 통해 AOP를 커스텀할 수 있다.

<br>

[#REFERENCE, Spring AOP, 알고 쓰자](https://velog.io/@dkwktm45/Spring-AOP%EB%A5%BC-%EC%95%8C%EA%B3%A0-%EC%82%AC%EC%9A%A9-%EB%B0%A9%EB%B2%95%EC%9D%84-%EC%95%8C%EC%9E%90)


</details>

<br>

<details>
<summary><strong>Q. Spring의 Dispatcher Servlet에 대해 설명해주세요.</strong></summary>

<br>

* **Dispatcher Servlet은 Http Protocol로 들어오는 모든 요청을 가장 먼저 받아 적합한 컨트롤러에 위임해주는 Front Controller이다.**
* Servlet은 클라이언트의 요청에 따라 웹페이지를 동적으로 구성해주는 자바 클래스이다.
* 기본적으로 Servlet은 한개의 요청에 하나의 Servlet을 구성한다.
* 요청이 많을 시 다수의 Servlet이 생성되어 다수의 컨트롤러를 관리해야 한다는 문제점이 있다.
* **따라서, Front Controller 패턴을 사용해 중앙에서 하나의 Servlet이 요청을 받아 Handler Mapping을 통해 적합한 컨트롤러로 분배하는 방식으로 개선한 것이 Dispatcher Servlet이다.**

<br>

<details>
<summary><strong>QQ. Spring에서 Front Controller 패턴에 대해 설명해주세요. </strong></summary>

<br>

* **Front Controller 패턴은 기존의 클라이언트 요청마다 요청에 맞는 컨트롤러(서블렛)를 작성하는 방식을 개선하기 위해 등장한 패턴이다.**
* **따라서, 프론트 컨트롤러 패턴은 모든 요청을 받는 하나의 컨트롤러를 위임해 앞단에 두어 컨트롤러 공통영역을 처리한 이후 해당 요청을 수행할 컨트롤러를 호출해 요청을 전달하는 방식을 의미한다.**
* 장점
  * 모든 컨트롤러에서 공통적으로 작성되는 보일러플레이트 코드를 줄일 수 있다.
  * 보안, 라우팅 등 공통적인 기능을 캡슐화 할 수 있다.
* Spring Framework에서는 Spring MVC와 함께 이러한 Front Controller 패턴이 준용되며, 이를 구현한 개체가 Dispatcher Servlet이다.

</details>

<br>

<details>
<summary><strong>QQ. Dispatcher Servlet의 동작 흐름에 대해 설명해주세요. </strong></summary>

<br>

1. 클라이언트 요청을 디스패처 서블릿이 받는다.
2. 요청 정보를 통해 요청을 위임할 컨트롤러를 찾는다.
3. 요청을 컨트롤러로 위임할 핸들러 어댑터를 찾아 전달한다.
4. 핸들러 어댑터가 컨트롤러로 요청을 위임한다.
5. 비즈니스 로직 처리 -> 컨트롤러가 반환값을 반환한다.
6. 핸들러 어댑터가 반환값을 처리한 후 디스패처 서블릿으로 넘긴다.
7. 디스패처 서블릿이 서버의 응답을 클라이언트로 반환한다.

</details>

<br>

[REFERENCE, Spring의 Dispatcher Servlet](https://mangkyu.tistory.com/18)

</details>

<br>

<details>
<summary><strong>Q. Spring에서 CORS 에러를 해결하기 위한 방법을 알려주세요.</strong></summary>

<br>

* 이하의 3가지 방법이 대표적으로 있다.
1. WebMvcConfigurer를 구현한 Configuration 클래스를 통해 addCorsMappings를 통해 재정의하는 방법
2. Spring Security에서 CorsConfigurationSource를 Bean으로 등록하고 SecurityConfig에 추가하는 방법
3. 클래스 레벨에 @CrossOrigin을 설정하는 방법

</details>

<br>

<details>
<summary><strong>Q. Spring Bean의 스코프의 의미와 종류에 대해 설명해주세요.</strong></summary>

<br>

* **Spring Bean의 스코프는 Bean이 존재할 수 있는 생명주기이자 범위를 뜻한다.**
* 종류로는 Singleton, Application, Prototype, Web(Session, Request, Application)이 있다.
  * Singleton : 기본 스코프로서 스프링 IOC 컨테이너(애플리케이션)의 시작과 종료까지 유지되는 가장 넓고, 포괄적인 범위의 스코프이다.
  * Prototype : Bean 생성과 DI 시점까지만 유지되는 스코프
    * 요청이 오면 항상 새로운 인스턴스를 생성하며 반환하고, 이후에 관리하지 않으므로 프로토타입을 받은 객체가 관리해야 한다.
  * Web
    * Request : 각각의 요청이 들어오고 응답이 나갈때까지 유지되는 스코프
    * Session : 세션이 생성되고 종료 시까지 유지되는 스코프
    * Application : 웹의 서블릿 컨텍스트와 같은 범위로 유지되는 스코프

<br>

[#REFERENCE, Spring Bean의 스코프](https://mangkyu.tistory.com/117)

</details>

<br>

<details>
<summary><strong>Q. Spring Web MVC에서 매 요청 시 각 스레드가 생성될텐데, 이 때 1개의 Controller의 생성만 보장할 수 있는 이유에 대해 설명해주세요. </strong></summary>

<br>

* **각 스레드 생성에 따른 매 요청 시 1개의 Controller의 생성을 보장하는 데에는 Spring IOC가 Spring Bean을 생성할 시 싱글톤 패턴으로 생성하기 때문이다.**
1. @Controller는 @Component의 어노테이션을 가지고 있기에, @Controller로 구현된 컨트롤러는 IOC 컨테이너에 의해 Spring Bean으로 관리된다.
2. 이러한 Spring Bean은 스코프를 지정해주지 않는 이상 기본적으로 Singleton 스코프이다.
2. Spring Bean은 IOC 컨테이너의 Application Context에 의해 실행 이후 생성되며, 실제 사용되는 시점에 DI를 통해 초기화된다.
3. 즉, 각각 다른 스레드에서 동일한 컨트롤러를 호출해도 Spring Bean에 의해 관리되기 때문에 동일한 컨트롤러임이 보장된다.
4. 의존성 주입 시점에 각기 다른 인스턴스를 받길 원하면 스코프 수명을 Singleton이 아닌 Prototype 등으로 조정해야 한다. 

</details>

<br>
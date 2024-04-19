### Controller 계층에서는 반드시?
DispatcherServlet이 Controller 파일을 찾고, 논리적 주소가 매핑된 메서드를 찾기 위해 Controller 파일에 @Controller와 @RequestMapping이 기술되어 있어야 한다.

### @RequestMapping이란?
어떤 Uri로 요청을 보내면 Controller에서 어떤 방식으로 처리할지 정의하는 과정을 거친다. 이때 보내온 요청을 어떤 메서드와 연결하는 매핑 어노테이션이 **@RequestMapping**이다. @RequestMapping는 메서드 레벨과 클래스 레벨 모두에서 사용할 수 있다.

### @RequestMapping의 속성
@RequestMapping에는 다양한 속성들을 사용할 수 있다. 그 중 대표적이고 자주 애용되는 속성이 value 속성과 method 속성이다.

- value 속성
  value 속성은 보내온 요청을 받을 Url를 설정하기 위해 기술되는 속성이다.

- method 속성
  method 속성은 GET, POST, PUT, DELETE, PATCH 중 어떤 형태로 보내온 요청을 받을지를 설정하는 속성이다.

이 두 가지 속성을 종합해보면

```java 
@RequestMapping(value = "/eample", method = RequestMethod.POST)
```
이런 형태로 사용된다.

### Ex1
그럼 이제 이 내용들을 가지고 메서드 레벨에서 /example이라는 내용을 가진 GET, POST, DELETE, PUT, PATCH 형태의 @RequestMapping을 만들어보자.

```java
@RestController
public class ExampleController{
	// GET
    @RequestMapping(value = "/example", method = RequestMethod.GET)
    public String ExampleGet(...){
    	...
    }
    
    // POST
    @RequestMapping(value = "/example", method = RequestMethod.POST)
    public String ExamplePost(...){
    	...
    }
    
    // DELETE
    @RequestMapping(value = "/example", method = RequestMethod.DELETE)
    public String ExampleDelete(...){
    	...
    }
    
    // PUT
    @RequestMapping(value = "/example", method = RequestMethod.PUT)
    public String ExamplePut(...){
    	...
    }
    
    // PATCH
    @RequestMapping(value = "/example", method = RequestMethod.PATCH)
    public String ExamplePatch(...){
    	...
    }
}
```

### 매핑 어노테이션
EX1에서 다룬 코드에서 @RequestMapping를 기술하다보면 반복되는 부분들이 있다. 뭔가 불편하지 않은가? 한 번 혹은 몇 번만 적는 것이라면 불편하다고 느껴지지 않을테지만 100번 수천번 기술해야 된다면 손만 아파진다. 또한 이를 기술하기 위한 시간도 소모될 것이다. 이런 부분을 해결하기 위해 선배 개발자 중에 누군가가 창조해낸 것이 @GetMapping, @PostMapping, @DeleteMapping, @PutMapping, @PatchMapping 같은매핑 어노테이션이다.

### Ex2
매핑 어노테이션을 사용하여 Ex1를 수정해보자.
```java
@RestController
public class ExampleController{
	
    // GET
    @Mapping(value = "/exampleGet")
    public String ExampleGet(...){
    	...
    }
    
    // POST
    @Mapping(value = "/examplePost")
    public String ExamplePost(...){
    	...
    }
    
    // PUT
    @Mapping(value = "/examplePut")
    public String ExamplePut(...){
    	...
    }
    
    // DELETE
    @Mapping(value = "/exampleDelete")
    public String ExampleDelete(...){
    	...
    }
    
    // PATCH
    @Mapping(value = "/examplePatch")
    public String ExamplePatch(...){
    	...
    }
}
```

### Ex3
Ex1를 Ex2처럼 수정할 수 있다. 이 때 참고할 사항이 하나 있다. value 속성(value = )은 생략이 가능하다는 것이다. 이를 적용해서 Ex3도 기술해보면

```java
@RestController
public class ExampleController{
	
    // GET
    @GetMapping("/exampleGet")
    public String ExampleGet(...){
    	...
    }
    
    // POST
    @PostMapping("/examplePost")
    public String ExamplePost(...){
    	...
    }
    
    // PUT
    @PutMapping("/examplePut")
    public String ExamplePut(...){
    	...
    }
    
    // DELETE
    @DeleteMapping("/exampleDelete")
    public String ExampleDelete(...){
    	...
    }
    
    // PATCH
    @PatchMapping("/examplePatch")
    public String ExamplePatch(...){
    	...
    }
}
```

### 클래스 레벨에서 @RequestMapping
@RequestMapping은 메서드 레벨과 클래스 레벨 모두에서 사용할 수 있다. 이번에는 클래스 레벨에서의 @RequestMapping를 알아보자.

클래스 레벨에서 @RequestMapping는 보내온 요청을 받고 응답을 보낼 때 사용하는 Url에 공통적으로 보여줄 부분을 기술하기 위해 사용한다.

### Ex4
Ex4에서는 클래스 레벨에서의 @RequestMapping의 예시를 다뤄보자.
```java
@RestController
@RequestMapping("/api")
public class ExampleController{
	
    // GET 
    @GetMapping("/exampleGet")
    public String ExampleGet(...){
    	...
    }
    
    // POST
    @PostMapping("/examplePost")
    public String ExamplePost(...){
    	...
    }
    
    // PUT
    @PutMapping("/examplePut")
    public String ExamplePut(...){
    	...
    }
    
    // DELETE
    @DeleteMapping("/exampleDelete")
    public String ExampleDelete(...){
    	...
    }
    
    // PATCH
    @PatchMapping("/examplePatch")
    public String ExamplePatch(...){
    	...
    }
}
```

### 로컬 환경에서?
Ex4를 가지고 로컬 환경에서 작업할 경우, GET 방식으로 "localhost:8080/api/exampleGet" 이라는 요청을 보내올 경우 xampleGet 메서드가 호출된다.

이때 인텔리제이의 경우 로컬 주소를 resources 패키지에서 application.yml 파일이나 application.properties 파일 같은 설정 파일에서 변경할 수 있다. (설정 파일에 관해서는 이 링크를 참고하길.. https://velog.io/@chrios99/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B6%80%ED%8A%B8-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EC%9D%98-%EC%84%A4%EC%A0%95-%ED%8C%8C%EC%9D%BC-%ED%98%95%EC%8B%9D-2%EA%B0%80%EC%A7%80)

POST 방식, DELETE 방식, PUT 방식, PATCH 방식도 앞서 한 GET 방식처럼 실시된다.

### 클래스 레벨에서 @RequestMapping 결론
이렇게 클래스 레벨에서 @RequestMapping을 사용 시 해당 클래스 내의 메서드를 호출하면 자동으로 Url에 해당 내용이 매핑 어노테이션에 기술한 value 값 앞에 붙어 사용자에게 보여진다.

이를 통해 매핑 어노테이션에 기술될 공통적인 value 값을 줄여 반복을 없앨 수 있다.

> 출처 :
https://mungto.tistory.com/436
https://backendcode.tistory.com/227
https://tecoble.techcourse.co.kr/post/2021-06-18-spring-request-mapping/
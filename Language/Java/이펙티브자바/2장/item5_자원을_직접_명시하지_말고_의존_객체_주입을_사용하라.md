# [item5] 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라

## Config class 내부에 자원을 직접 정의 하는 것은 지양하기

환경(dev, prod) 별로 다른 값들이 들어가야 할 때 적절하지 않다. 아래 예시는 자원을 직접 명시하는 방식이다.

```java
@Configuration
public class ItonseConfig {
    private static final String address = "경기도 수원시";
    
    public ItonseConfig() {
        this.address = "경기도 수원시";
    }
}
```

이 방식은 환경에 따라 다른 값을 설정하기 어렵고, 코드의 유연성을 떨어뜨린다.

<br/><br/>

## Dependency Injection(의존성 주입)을 사용하자

환경별로 다른 값을 주입할 수 있도록 설정 파일을 사용하여 의존성 주입을 활용하는 방법을 사용한다.

### application-dev.yml
```java
itonse:
    address: '경기도 수원시'
```
<br/>

### application-prod.yml
```java
itonse:
    address: '경기도 용인시'
```
<br/>

위와 같이 환경 별로 다른 값들을 정의를 한 후, `@Value` 어노테이션을 사용하여 설정 값을 주입받는다.

```java
@Conficuration
public class ItonseConfig {
    @Value("${itonse.address")
    private String address;
}
```

이 방식을 사용하면 코드의 유연성, 재사용성이 향상된다.
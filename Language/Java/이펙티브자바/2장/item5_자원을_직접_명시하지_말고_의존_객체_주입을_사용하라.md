# [item5] 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라
# 자바에서의 적용

많은 클래스가 하나 이상의 자원에 의존한다. 자원을 명시하지 말고 의존 객체 주입을 사용하라는 의미를 한번 생각해보자.

### 먼저 의존 관계 예시

```java
public class Duck() {
    Type type = new RubberDuck();
    
    public Duck() {}
}

public class RubberDuck() extends Type {
    String duckName = "A";
    int size = 100;
    
    public RubberDuck() {}
}
```

여기서 만약 고무 오리 대신 기계 오리로 대체해야 된다면? <br/>
직접 Duck 클래스의 코드를 변경해야 한다. 기계 오리 뿐만 아니라 나중에 장난감 오리, 종이 오리등 다른<br/>
오리로 변경해야 할 때마다 번거롭게 Duck 클래스의 코드를 수정해야 한다.<br/>

이번에는 생성자를 사용해서 외부에서 객체를 주입한다고 해보자.
<br/><br/>

```java
public class Duck() {
    Type type;
    
    public Duck(Type type) {    // 생성자에 객체를 받아 주입
        this.type = type;
    }
}

public class Main() {
    public static void main(String[] arg) {
        Duck duck = new Duck(new RubberDuck());
    }
}
```
Duck 객체를 생성할 때 의존 관계를 띠고 있는 Type 객체를 주입 받는다면 Duck 클래스 내부를 <br/>
수정하지 않아도 된다. 이렇게 **외부에서 의존 관계를 주입하는 것**을 `의존성 주입(DI)`이라고 한다.

<br/>

## 책에서의 예시

많은 클래스가 하나 의상의 자원에 의존을 하는데, 가령 맞춤법 검사기는 사전에 의존을 하게 된다. <br/>
사전은 언어별로 따로 있고 심지어 특수 어휘용 사전을 별도로 두기도 한다. 이처럼 맞춤법 검사기는 <br/>
다른 사전으로 교체할 수 있도록 구조를 만들어야 한다.

먼저 단 하나의 사전으로 맞춤법 검사기를 만든 잘못 된 예시들을 살펴보겠다.

#### 예시1. 정적 유틸리티를 잘못 사용한 예
```java
public class SpellChecker {
    private final Lexicon dictionary = new KoreanLexicon();   // 특정 사전을 사용한다고 명시
    
    private SpellChecker(...) {}   // 객체 생성 방지
    
    public static boolean isValid(String word) {...}
    public static List<String> suggestions(String typo) {...}
}
```
<br/>

#### 예시2. 싱글턴을 잘못 사용한 예
```java
public class SpellChecker {
    private final Lexicon dictionary = new KoreanLexicon();   // 특정 사전을 사용한다고 명시
    
    private SpellChecker(...) {}   // 객체 생성 방지
    public static SpellChecker INSTANCE = new SpellChecker(...);

    public boolean isValid(String word) {...}
    public List<String> suggestions(String typo) {...}
}
```

두 방식 모두 사전을 단 하나만 사용한다고 가정해서 그리 좋은 방식은 아니다. <br/>
위의 방식들에서 SpellChecker가 여러 사전을 사용할 수 있도록 하고자 할 때 이런 아이디어를 떠올릴 수 있다. <br/>
'필드에서 final을 제거하고 다른 사전으로 교체하는 메서드를 추가하면 되지 않을까?'
이 아이디어를 다시 첫 번째, 두 번째 예시에 적용시켜보도록 하겠다.
<br/><br/>

#### 예시1. 정적 유틸리티를 잘못 사용한 예
```java
public class SpellChecker {
    private static Lexicon dictionary = new KoreanLexicon();   // final을 제거
    
    private SpellChecker(...) {}

    public void setDictionary(Lexicon newDictionary) {   // 다른 사전으로 교체하는 메서드 추가
        dictionary = newDictionary;
    }
    
    public static boolean isValid(String word) {...}
    public static List<String> suggestions(String typo) {...}
}
```
예시1 코드에서 dictinary의 final 한정자를 제거해서 변경 가능하게 만들었다.<br/>
이제 SpellChecker에는 다른 사전이 올 수 있지만 또 다른 문제가 생기게 된다. <br/>
바로 **멀티 스레드 환경에서 thread safe 하지 않다는 것**이다.

여러 스레드에서 dictionary에 접근하여 사용할때 전부 같은 값을 공유한다. <br/>
새롭게 객체를 만들면서 다른 사전으로 변경한다 하더라도 다른 스레드에 의해 유지되기가 힘들다.

<br/>

#### 예시2. 싱글턴을 잘못 사용한 예
```java
public class SpellChecker {
    private Lexicon dictionary = new KoreanLexicon();   // final을 제거
    
    private SpellChecker(...) {}   
    public static SpellChecker INSTANCE = new SpellChecker(...);

    public void setDictionary(Lexicon newDictionary) {   // 다른 사전으로 교체하는 메서드 추가
        this.dictionary = newDictionary;
    }
    
    public boolean isValid(String word) {...}
    public List<String> suggestions(String typo) {...}
}
```
<br/>

final을 제거해서 사전은 대체 가능하지만 **문제는 싱글턴이라는 것이다. 인스턴스를 단 하나만 사용할 수 있다.** <br/>
멀티 스레드 환경에서 dictionary에 접근할때 하나의 값을 공유하게되므로 thread safe 하지 않다. <br/>

<br/><br/>
사용하는 자원에 따라 동작이 달라지는 클래스에는 정적 유틸리티 클래스나 싱글턴 방식이 적합하지 않다.

SpellChecker의 목적은 여러 자원 인스턴스를 지원해야 하며, 클라이언트가 원하는 자원(dictionary)을 <br/>
사용해야 한다. 이 조건을 만족하는 간단한 패턴이 있는데, 바로 **인스턴스를 생성할 때 생성자에 필요한 자원을 넘겨주는 방식** 이다.

이는 의존 객체 주입의 한 형태로, 맞춤법 검사기를 생성할 때 의존 객체인 사전을 주입해주면 된다.
<br/><br/>

```java
import java.util.Objects;

public class SpellChecker {
    private final Lexicon dictionary;

    public SpellChecker(Lexicon dictionary) {    << 외부에서 의존성 주입
        this.dictionary = Objects.requireNonNull(dictionary);   << 값이 null이면 NullPointerException 던짐 
    }

    // 기타 메서드
    public boolean isValid(String word) {...}
    public List<String> suggestions(String typo) {...}
    
    public static void main(String[] args) {
        SpellChecker spellChecker = new SpellChecker(new KoreanLexicon());   << KoreanLexicon을 사용하겠다
    }
}
```
<br/>
이렇게 외부에서 의존성을 주입하면 매번 원하는 사전으로 객체를 생성할 수 있다. <br/>
여기서 한가지 더 이점은 dictionary를 final로 선언해서 클라이언트는 어떤사전이 변할 걱정을 하지않아도 된다.<br/>
이 의존 객체 주입 방식은 생성자, 정적 팩터리(item1), 빌더(item2) 모두에 똑같이 응용할 수 있다.

### 변형: 정적 팩토리를 넘겨주는 방식
```java
public class SpellChecker {
    private final Lexicon dictionary;

    public SpellChecker(Lexicon dictionary) {    <<외부에서 의존성 주입
        this.dictionary = Objects.requireNonNull(dictionary);   <<값이 null이면 NullPointerException 던짐
    }
    
    public static SpellChecker withDictionary(Lexicon dictionary) {    << 정적 팩터리 방식 
        return new SpellChecker(dictionary);
    }

    // 기타 메서드
    public boolean isValid(String word) {...}
    public List<String> suggestions(String typo) {...}
    
    public static void main(String[] args) {
        SpellChecker spellChecker = pellChecker.withDictionary(new KoreanLexicon());
    }
}
```

## 요약
의존 객체 주입은 클래스가 자원을 명시적으로 참조하지 않게 하여 결합도를 낮추고 유연성을 높이는 방식입니다.<br/>
이를 통해 다양한 자원을 사용하여 객체를 생성할 수 있고, 코드 변경 없이 객체의 동작을 변경할 수 있습니다.

<br/><br/><br/>

# 스프링 프레임워크에서의 적용 
## Config class 내부에 자원을 직접 정의 하는 것은 지양하기

환경(dev, prod) 별로 다른 값들이 들어가야 할 때 적절하지 않다. 아래 예시는 자원을 직접 명시하는 방식이다.

```java
@Configuration
public class DatabaseConfig {
    private static final String dbUrl = "jdbc:mysql://localhost:3306/dev_db";
}
```

이 방식은 환경에 따라 다른 값을 설정하기 어렵고, 코드의 유연성을 떨어뜨린다.

<br/><br/>

## Dependency Injection(의존성 주입)을 사용하자

환경별로 다른 값을 주입할 수 있도록 설정 파일을 사용하여 의존성 주입을 활용하는 방법을 사용한다.

### application-dev.yml
```java
database:
    url: 'jdbc:mysql://localhost:3306/dev_db'
```
<br/>

### application-prod.yml
```java
database:
    url: 'jdbc:mysql://prod-db-server:3306/prod_db'
```
<br/>

위와 같이 환경 별로 다른 값들을 정의를 한 후, `@Value` 어노테이션을 사용하여 설정 값을 주입받는다.
<br/>

+) `@Value` 어노테이션의 역할은 아래와 같다.
1. 설정 파일(.yml 등)에서 값을 읽어오기
2. 읽어온 값을 Bean의 필드에 주입

<br/>

```java
@Configuration
public class DatabaseConfig {
    @Value("${database.url}")
    private String dbUrl;

    public String getDbUrl() {
        return dbUrl;
    }
}
```

이 방식을 사용하면 코드의 유연성, 재사용성이 향상된다.

<br/><br/>

## 순수 자바에서 @Value 의 의존성 주입 기능 구현하기

스프링에서는 `@Value`어노테이션을 통해 환경별로 다른 값들을 주입받는다. <br/>
이와 유사한 방식으로 설정 파일에서 값을 읽어와 환경별 설정 값을 주입받는 방법을 순수 자바로 구현해보았다.

<br/>

### 1. Properties 파일 준비

.Properties 파일: **key=value** 형식의 텍스트 파일

dev.properties 파일<br/>
`database.url=jdbc:mysql://localhost:3306/dev_db`

prod.properties 파일<br/>
`database.url=jdbc:mysql://prod-db-server:3306/prod_db`

### 2. 환경 변수 로드 및 의존성 주입

```java
...
import java.util.Properties; 

public class DatabaseConfig {
    private String dbUrl;

    public DatabaseConfig(String env) {    << 생성자에서 환경 변수를 받아 의존성 주입 ex) "dev" 주입
        this.dbUrl = loadDatabaseUrl(env);   // 메서드 호출을 통해 "jdbc:mysql://localhost:3306/dev_db" 저장
    }

    private String loadDatabaseUrl(String env) { 
        Properties properties = new Properties();
        // 설정 파일에서 값 읽어오기 (간단한 예시를 보이기 위해 아래 코드를 try-catch문으로 감싸는 것 생략)
        InputStream inputStream = getClass().getClassLoader().getResourceAsStream(propertiesFileName);
        
        properties.load(env + ".properties");    // dev.properties 파일 내용 로드
        // "database.url" 키에 해당하는 값을 반환  ->  "jdbc:mysql://localhost:3306/dev_db"
        return properties.getProperty("database.url");    
    }

    public String getDbUrl() {
        return dbUrl;
    }

    public static void main(String[] args) {
        // dev, prod 환경 설정 사용
        DatabaseConfig devConfig = new DatabaseConfig("dev");
        DatabaseConfig prodConfig = new DatabaseConfig("prod");
        
        // dev, prod DB url 출력
        System.out.println("dev DB url: " + devConfig.getDbUrl());
        System.out.println("prod DB url: " + prodConfig.getDbUrl());
    }
}
```

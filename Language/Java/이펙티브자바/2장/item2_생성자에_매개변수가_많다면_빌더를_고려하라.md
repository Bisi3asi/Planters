# [item2] 생성자에 매개변수가 많다면 빌더를 고려하라.

item1 에서 다루었던 정적 팩터리와 생성자에는 똑같은 제약이 하나 있다.
선택적 매개변수가 많을 때 적절히 대응하기 어렵다는 것이다.

물론 이 같은 경우 점층적 생성자 패턴을 사용해서 선택 매개변수를 1개 받는 생성자 부터
전부 다 받는 생성자까지 늘려가며 만들 수는 있다. 하지만 이렇게 사용 할 경우
클라이언트 코드를 작성하거나 읽기가 어려줘진다는 단점이 있다.

<br/>

### 아래는 필수 및 선택 매개변수가 있는 User 클래스를 점층적 생성자 패턴으로 작성한 예제이다.

```java
public class User {
    private final String name;         // 필수
    private final String phoneNumber;     // 필수
    private final int age;             // 선택
    private final int birthday;        // 선택
}
```
<br/>

위와 같이 필수, 선택 매개변수가 있을 때 점층적 생성자 패턴을 사용하면 다음과 같아진다.

```java
// 필수 매개변수를 받는 생성자
private User(String name, String phoneNumber) {
    this.name = name;
    this.phoneNumber = phoneNumber;
}
// 나이를 추가로 받는 생성자
private User(String name, String phoneNumber, int age) {
    this(name, phoneNumber);
    this.age = age;
}

// 나이와 생일을 추가로 받는 생성자
private User(String name, String phoneNumber, int age, int birthday) {
    this(name, phoneNumber, age);
    this.birthday = birthday;
}
```
<br/>

```java
User user1 = new User("Kim", "01011112222", 30, 1225);
```

여기서 매개변수가 늘어날수록 값을 넣을 자리가 어디인지 헷갈릴 수 있다.

<br/><br/>

### 빌더 패턴의 장점
- 점층적 생성자보다 코드를 읽기가 편하고, 간결하다.
- 자바빈즈(setter 방식)보다 훨씬 안전하다

빌더패턴은 매개변수가 4개 이상은 되어야 값어치를 한다.
하지만 API는 시간이 지날수록 매개변수가 많아지는 경향이 있다. <br/>
생성자나 정적 팩터리 메서드 방식으로 시작했다가 나중에 매개변수가 많아지면 빌더 패턴으로 전환할 수도 있지만 번거롭다. <br/>
그러니 처음부터 빌더로 시작하는 편이 나을 수도 있다.

<br/>

### (추가) 빌더를 Lombok을 사용하지 않고 직접 만들어서 구조 살펴보기

```java
public class User {
    private final String name;          // 필수 
    private final String phoneNumber;   // 필수
    private final int age;             // 선택 
    private final int birthday;        // 선택
    
    public static class Builder {
        private final String name;
        private final String phoneNumber;
        private final int age = 0;
        private final int birthday = 0;
        
        public Builder(String name, String phoneNumber) {
            this.name = name;
            this.phoneNumber = phoneNumber;
        }
        
        public Builder age(int val) {
            age = val;
            return this;
        }
        
        public Builder birthday(int val) {
            birthday = val;
            return this;
        }
        
        public User build() {
            return new User(this);
        }
    }
}
```
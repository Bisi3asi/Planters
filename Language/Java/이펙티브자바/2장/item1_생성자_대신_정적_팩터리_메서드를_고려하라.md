## 2장 들어가기

다루는 주제: 객체 생성과 파괴

- 객체를 만들어야 할 때와 만들지 말아야 할 때를 구분하는 방법
- 올바른 객체 생성 방법과 불필요한 생성을 피하는 방법
- 제때 파괴됨을 보장하고 파괴 전에 수행해야 할 정리 작업을 관리하는 요령

2장에서는 위의 내용들을 다루게 됩니다.

---

# [item1] 생성자 대신 정적 팩터리 메서드를 고려하라.

### 정적 팩터리 메서드란?
정적 팩터리 메서드는 **객체를 생성하기 위한 메서드로**, 일반적인 생성자와 달리 이름을 가질 수 있으며 <br/>
다양한 형태로 객체를 생성할 수 있습니다.

<br/>

## 생성자와 정적 팩터리 메서드 비교

### 생성자

```java
public class User {
    private String name;
    private int age;
    
    public User(String name, int age) {   // 이름을 가질 수 없다.
        this.name = name;
        this.age = age;
    }
}
```
<br/>

### 정적 팩터리 메서드

```java
public class User {
    private String name;
    private int age;
    
    public static User ofNameAndAge(String name, int age) {    // 이름을 가질 수 있다.
        User user = new User();
        user.name = name;
        user.age = age;
        return user;
    }
}
```
<br/><br/>

## 정적 팩터리 메서드는 언제 사용하면 좋을까?

정적 팩터리 메서드는 다음과 같은 상황에서 유용하게 사용될 수 있습니다.

#### Entity를 DTO로 변환할 때
- Entity를 response용 DTO로 변환하는 경우.

#### DTO를 VO로 변환할 때
- DTO를 비즈니스 로직에서 사용하는 객체인 VO로 변환하는 경우.

<br/>

### Entity를 DTO로 변환할 때 사용 예시

#### 생성자 이용한 변환
```java
@Getter
public class UserDTO {
    private Long id;
    private String name;
    private int age;

    // 생성자를 통한 변환
    public UserDTO(User user) {
        this.id = user.getId();
        this.name = user.getName();
        this.age = user.getAge();
    }
}
```
<br/>

#### 정적 팩터리 메서드를 이용한 변환
```java
@Getter
public class UserDTO {
    private Long id;
    private String name;
    private int age;

    // 정적 팩터리 메서드를 통한 변환
    public static UserDTO from(User user) {
        UserDTO userDTO = new UserDTO();
        userDTO.id = user.getId();
        userDTO.name = user.getName();
        userDTO.age = user.getAge();
        return userDTO;
    }
}
```
<br/><br/>

## 정적 팩터리 메서드 사용의 장단점

### 장점
1. 이름을 가질 수 있다. (이름만 잘 지으면 반환될 객체의 특성을 쉽게 묘사할 수 있다) <br/>
    - UserDTO.from(User user)는 User 객체를 UserDTO로 변환함을 명확히 나타냅니다.
2. 인스턴스를 매번 생성할 필요는 없다(불필요한 객체 생성을 피할 수 있다.)
3. 입력 매개변수에 따라 다른 클래스의 객체를 반환할 수 있다.
```java
public class PetFactory { 
    public static Pet createPet(String type) {
        if ("dog".equalsIgnoreCase(type)) {
            return new Dog();
        } else if ("cat".equalsIgnoreCase(type)) {
            return new Cat();
        } else {
            throw new IllegalArgumentException("Unknown pet type");
        }
    }
}
```
<br/>

### 단점
1. 정적 팩터리 메서드만 제공하면 생성자가 없으므로 상속받은 클래스를 만들 수 없다.
2. 정적 팩터리 메서드는 개발자가 찾기 어렵다.<br/>
    - 일반적인 생성자처럼 명확하게 드러나지 않기 때문에, 사용자가 이를 찾기 어려울 수 있습니다.
    - 이는 메서드 이름을 널리 알려진 규약을 따라 짓는 식으로 문제 완화할 수 있습니다. (아래 예시)
        - from: 매개변수를 하나 받아, 해당 타입의 인스턴스를 반환하는 형변환 메서드
        - of: 매개변수를 여러개 받아 적합한 타입의 인스턴스를 반환하는 집계 메서드

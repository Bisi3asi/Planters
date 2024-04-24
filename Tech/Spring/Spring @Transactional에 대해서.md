## Transaction?

트랜잭션에 대해서 알아보자.

본래 뜻은 “**거래**”이며, 보통 “**업무 처리의 최소 단위**” 또는 “**하나의 논리적인 작업 단위**”로 부른다.

이는 트랜잭션이 하나의 작업을 처리하면서 더 이상 쪼개질 수 없는 명령들의 모임이며, 한 번에 실행되어야 하는 명령들의 모임이기 때문이다.

**트랜잭션을 중요시하며 사용하는 이유가 뭘까?**

이는 트랜잭션이 아래 `ACID 원칙`을 보장하기 때문이다.

- **원자성 (A; Atomicity)**
    - 원자성은 트랜잭션 내의 각 명령이 일부만 성공하거나 실패하지 않도록 하는 성질이다.
    - 트랜잭션이 정상적으로 처리 되었을 때는 `커밋(commit)`하여 작업을 완료하고, 오류가 발생했을 때는 `롤백(rollback)`으로 작업을 실행하기 전 상태로 복구 할 수 있기 때문에 작업 전체가 성공하거나 실패하거나의 선택지만 존재한다.
- **일관성 (C; Consistency)**
    - 일관성은 트랜잭션 처리 전과 처리 후 데이터에 모순이 없게 유지하도록 하는 성질이다.
    - 정합성이라고도 불리며, 트랜잭션이 진행되는 동안 데이터베이스가 변경되더라도 변경되지 않은 데이터베이스를 바탕으로 작업을 수행하기 때문에 데이터의 일관성을 유지할 수 있다.
- **독립성 (I; Isolation)**
    - 독립성은 트랜잭션 수행 시 다른 트랜잭션의 연산 작업이 끼어들지 못하도록 보장하는 성질이다.
    - 격리라고도 부르며, 트랜잭션의 완료되기 전 까지 다른 트랜잭션은 특정 트랜잭션의 결과를 참조할 수 없다는 의미이다.
    - 이와 관련된 격리 수준, 낙관적 락, 비관적 락에 대해서도 아래에서 다뤄볼 예정이다.
- **영속성 (D; Durability)**
    - 성공적으로 수행된 트랜잭션은 영원히 반영되어야 한다는 성질이다.
    - 지속성이라고도 불리며, 말 그대로 정상적으로 수행된 트랜잭션은 영구적으로 데이터베이스에 반영되어야 한다는 의미이다.

## @Transactional?

@Transactional 어노테이션은 Spring에서 데이터 작업의 실행단위를 명시하기 위해 사용된다고 볼 수 있다.

### 사용법

@Transactional 은 클래스 단위에도 사용이 가능하고, 메서드 단위에도 사용할 수 있다.

클래스 단위에 사용하면, 모든 메서드에 자동으로 @Transactional이 적용된다.

메서드 단위에만 사용하면, 단일 메서드에만 @Transactional이 적용된다.

```java
@Service
@Transactional // 클래스 단위에 @Transactional 사용
public class SomeService {

	@Transactional // 메서드 단위에 @Transactional 사용
	public void some() { ... }
	
	// 클래스 단위에 사용했다면, @Transactional 적용
	public void something() { ... }
	
}
```

### 구조

각 속성에 대한 내용을 다루기 전에 먼저 @Transactional의 작동 구조에 대해서 살펴보는게 도움이 될 것이다.

![](https://i.imgur.com/lDZ3z9u.png)

클라이언트가 요청 시 Spring AOP에 의해 생성된 프록시 객체가 이를 대신하여 받게 된다.

생성된 프록시 객체에서는 부가기능을 수행하게 되는데 이에 트랜잭션의 관리가 포함되어있다.

그렇기에 @Transactinal 어노테이션이 작성되어 있다면 프록시 객체에서 처리되기 때문에 별도의 연결 유지 혹은 트랜잭션 시작, 완료, 복구 등에 대한 작업을 해줄 필요가 없다.

이는 트랜잭션 관리나 로깅과 같은 부가기능에 대한 로직 분리와 중복되는 처리를 줄이기 위해 설계된 구조이다.

위에서 설명한 내용은 Spring boot에서 이용하는 `CGLIB Proxy` 방식을 기준으로 설명한 내용이다.

관련한 자세한 내용이 필요하다면 아래 글을 참고하길 바란다.

[Spring AOP와 Proxy](https://www.notion.so/Spring-AOP-Proxy-00dc8d6b99cb40e1baf297efb59e0998?pvs=21)

### 속성

**value**

```java
@Transactional(value="transactionManager")
public void some() { ... }
```

value 속성은 트랜잭션 매니저를 설정할 때 사용된다.

기본적으로 Spring은 단일 데이터 소스를 사용하거나 여러 데이터 소스를 사용하더라도 각 데이터 소스에 맞는 트랜잭션 매니저를 선택하고 사용하기 때문에 문제가 되지는 않는다.

하지만 버전이 다른 DB를 두 개 사용한다거나 특정 기능 수행(`JTA` 등)을 위해 트랜잭션 매니저를 선택해야하는 경우에는 명시적인 설정이 필요할 수 있다.

그런 경우가 아니더라도 여러 데이터 소스를 사용하는 경우 이를 명시해주면 코드를 더 명확하게 할 수 있고, 트랜잭션 관리를 더 세밀하게 할 수 있다.

- **트랜잭션 매니저?**

  트랜잭션 매니저는 Spring의 데이터베이스 트랜잭션을 관리하는 역할을 한다.

    ```java
    public interface PlatformTransactionManager extends TransactionManager {
        TransactionStatus getTransaction(@Nullable TransactionDefinition definition) throws TransactionException;
    
        void commit(TransactionStatus status) throws TransactionException;
    
        void rollback(TransactionStatus status) throws TransactionException;
    }
    ```

  `PlatformTransactionManager` 라는 트랜잭션 추상화 인터페이스를 제공하여 다양한 플랫폼 (JDBC, JPA, Hibernate 등) 에 대한 트랜잭션의 동작(begin, commit, rollback)을 제어하고 트랜잭션의 상태를 추적한다.

  즉, @Transactional을 사용하게 되면 아래와 같이 동작하게 된다.

  ![](https://i.imgur.com/WK4LK41.png)

  프록시 객체에서 트랜잭션은 트랜잭션 매니저에 의해 관리된다.

    - 프록시 객체가 트랜잭션 매니저를 통해 트랜잭션을 시작하면`DataSourceUtils`의 `getConnection()` 을 통해서 커넥션을 제공받는다.
        - 트랜잭션 동기화 매니저에 의해 관리되고 있는 커넥션이 있는 경우 해당 커넥션을 제공하고 그렇지 않은 경우 새로운 커넥션을 생성해 제공한다.
        - 또한, 해당 커넥션이 트랜잭션 동기화 매니저에 바인딩 되어 있는지를 검사하여 바인딩하는 작업을 진행한다.
        - 이렇게 프록시 객체로 반환 된 커넥션은 트랜잭션 유지를 위해 사용된다.
    - 이후 프록시 객체가 비즈니스 로직을 호출하면 구현 객체는 `DataSourceUtils` 의 `getConnection()` 을 통해 커넥션을 제공받아 데이터베이스 작업을 처리한다.
        - 트랜잭션 동기화 매니저에 바인딩 되어있는 커넥션을 가져와 데이터베이스 작업을 진행한다.
    - 트랜잭션 매니저는 트랜잭션이 완료된 후 성공 여부에 따라 커밋이나 롤백을 진행한다.
    - 위 과정에서 트랜잭션 매니저는 `releaseConnection()`을 호출하여 커넥션을 종료시킨다.
        - 트랜잭션이 완료되면 트랜잭션 동기화 매니저의 `unbindResource()` 가 자동으로 호출되어 바인딩 된 커넥션이 해제된다.
        - releaseConnection()은 해당 커넥션이 더 이상 트랜잭션 동기화 매니저에 의해 관리되고 있지 않다면 종료하도록 한다.
        - 이는 트랜잭션 매니저를 통해 트랜잭션을 관리하지 않았을 당시에 비즈니스 로직에서의 직접적인 커넥션 관리는 DAO, Repository를 사용할 시점에 종료될 수 있는 문제를 방지하게 하기 위함이다.

  위와 같이 트랜잭션 매니저는 내부에서 `DataSourceUtils` 와 트랜잭션 동기화 매니저를 활용하여 데이터베이스 트랜잭션과 커넥션을 관리한다.

  추가로 `DataSourceUtils` 는  `ConnectionHolder` 를 통해서 커넥션을 저장하는데, 이는 내부적으로 `ThreadLocal`을 사용하기 때문에 멀티 쓰레드 환경에서도 안전하게 커넥션을 제공할 수 있게 해준다.

  내용을 글로만 봐서는 내용이 잘 이해되지 않을 수 있다.

  실제 DataSourceUtils 코드를 첨부하니 이와 함께 보면 도움이 될 것이다.

    - `doGetConnection()`

        ```java
        	public static Connection doGetConnection(DataSource dataSource) throws SQLException {
        		Assert.notNull(dataSource, "No DataSource specified");
        
        		ConnectionHolder conHolder = (ConnectionHolder) TransactionSynchronizationManager.getResource(dataSource);
        		if (conHolder != null && (conHolder.hasConnection() || conHolder.isSynchronizedWithTransaction())) {
        			conHolder.requested();
        			if (!conHolder.hasConnection()) {
        				logger.debug("Fetching resumed JDBC Connection from DataSource");
        				conHolder.setConnection(fetchConnection(dataSource));
        			}
        			return conHolder.getConnection();
        		}
        		// Else we either got no holder or an empty thread-bound holder here.
        
        		logger.debug("Fetching JDBC Connection from DataSource");
        		Connection con = fetchConnection(dataSource);
        
        		if (TransactionSynchronizationManager.isSynchronizationActive()) {
        			try {
        				// Use same Connection for further JDBC actions within the transaction.
        				// Thread-bound object will get removed by synchronization at transaction completion.
        				ConnectionHolder holderToUse = conHolder;
        				if (holderToUse == null) {
        					holderToUse = new ConnectionHolder(con);
        				}
        				else {
        					holderToUse.setConnection(con);
        				}
        				holderToUse.requested();
        				TransactionSynchronizationManager.registerSynchronization(
        						new ConnectionSynchronization(holderToUse, dataSource));
        				holderToUse.setSynchronizedWithTransaction(true);
        				if (holderToUse != conHolder) {
        					TransactionSynchronizationManager.bindResource(dataSource, holderToUse);
        				}
        			}
        			catch (RuntimeException ex) {
        				// Unexpected exception from external delegation call -> close Connection and rethrow.
        				releaseConnection(con, dataSource);
        				throw ex;
        			}
        		}
        
        		return con;
        	}
        ```

    - `doReleaseConnection()`

        ```java
        	public static void doReleaseConnection(@Nullable Connection con, @Nullable DataSource dataSource) throws SQLException {
        		if (con == null) {
        			return;
        		}
        		if (dataSource != null) {
        			ConnectionHolder conHolder = (ConnectionHolder) TransactionSynchronizationManager.getResource(dataSource);
        			if (conHolder != null && connectionEquals(conHolder, con)) {
        				// It's the transactional Connection: Don't close it.
        				conHolder.released();
        				return;
        			}
        		}
        		doCloseConnection(con, dataSource);
        	}
        ```

- **JTA?**

  `JTA (Java Transaction API)`는 분산 트랜잭션을 관리하는 데 사용되는 Java 표준 API이다.

  여러 DB, 메시지 큐, 웹 서비스 등을 사용할 때 분산된 트랜잭션을 처리하는데 사용된다.

  서로 다른 DB 뿐만 아니라 서로 다른 서비스에서 발생하는 다른 트랜잭션을 하나의 트랜잭션을 통해 저장, 롤백할 수 있도록 만들어졌다.


---

**propagation**

```java
@Transactional(propagation = Propagation.[전파 레벨]) 
public void some() { ... }
// 전파 레벨 : REQUIRED, REQUIRES_NEW, NESTED, MANDATORY, SUPPORTS, NOT_SUPPORTED, NEVER
```

propagation은 트랜잭션의 전파 레벨을 설정하는 옵션이다.

전파 레벨에 대해 살펴보자면 아래와 같다.

**`REQUIRED`**

```java
@Transactional (propagation = Propagation.REQUIRED) 
public void some() {
	something();
	// ...
}

@Transactional // 생략도 가능
public void something() {
	// ...
}
```

`REQUIRED`는 Transactional 전파 레벨의 기본 설정이며 생략이 가능하다.

선언된 메서드에 이미 실행 중인 트랜잭션이 있다면 기존 트랜잭션을 활용하고 없다면 새로운 트랜잭션을 시작한다.

- 기존에 사용하던 트랜잭션이 있다면 이를 프록시 객체에서 인식하고 해당 트랜잭션 내에서 작업을 진행한다.
- 트랜잭션이 없다면 트랜잭션 매니저를 통해 새 커넥션을 생성하여 받아 트랜잭션을 시작한다.

즉, 위와 같은 상황에서는 상위 메서드의 트랜잭션을 활용하여 진행하는 것이다.

이는 하위 메서드에 `@Transactional`을 사용하지 않아도 동일한 결과가 나오기 때문에, 상위 메서드의 트랜잭션을 하위 메서드까지 적용시키고 싶은 경우라면 사용하지 않아도 된다.

기본 값이기에 일반적으로 가장 많이 사용되며 트랜잭션을 적용시키고 싶은 곳에 사용하면 된다.

---

**`REQUIRES_NEW`**

```java
@Transactional
public void some() {
	something();
	// ...
}

@Transactional (propagation = Propagation.REQUIRES_NEW) 
public void something() {
	// ...
}
```

`REQUIRES_NEW` 는 `@Transactional` 이 선언된 메서드마다 새로운 트랜잭션을 시작한다.

- 기존에 사용하던 트랜잭션이 있더라도 새로운 트랜잭션을 생성하여 작업을 진행한다.
- 기존에 사용하던 트랜잭션은 대기 상태로 두고, 새로운 트랜잭션이 시작되기 때문에 새 트랜잭션에서 오류가 발생하더라도 기존 트랜잭션에 롤백이 전파되지 않는다.

즉, 위와 같은 상황이라면 some()의 트랜잭션과 something()의 트랜잭션이 별개로 동작하여 something() 트랜잭션의 결과와 상관없이 some() 의 트랜잭션을 마무리하게 된다.

기존 트랜잭션이 있음에도 새 트랜잭션을 시작하기 때문에 아래와 같은 상황에 사용할 수 있을 것 같다.

- **부분적인 커밋이나 롤백이 필요한 경우**
- **중요한 작업에 대한 안정성 보장이 필요한 경우**
    - 부분 커밋이 가능하기에 기존 트랜잭션이 롤백되더라도 실행되어야하는 중요한 하위 작업이 있다면 이를 처리할 수 있다.
    - 또한 부분 롤백이 가능하기에 기존 트랜잭션의 작업이 중요한 경우에도 이를 끝까지 처리할 수 있다.

---

**`NESTED`**

```java
@Transactional
public void some() {
	something();
	// ...
}

@Transactional (propagation = Propagation.NESTED) 
public void something() {
	// ...
}
```

`NESTED`는 중첩 트랜잭션을 지원하는 데이터베이스에서만 사용할 수 있다.

- `Oracle`, `PostgreSQL`, `MySQL 8.0` 이상, `H2` 등

기존 트랜잭션이 있다면 `SAVEPOINT`를 남기고 중첩 트랜잭션을 시작하며, 없다면 새로운 트랜잭션을 시작한다.

- 하위 메서드의 트랜잭션이 커밋되더라도 기존 트랜잭션이 롤백된다면 전체 트랜잭션이 롤백된다.
- 하위 메서드의 작업이 롤백된 경우에는 `SAVEPOINT`를 남긴 부분까지 부분 롤백된다.

`REQUIRES_NEW`는 기존 트랜잭션과 하위 트랜잭션이 모두 독립적으로 커밋 및 롤백이 가능하지만, `NESTED`의 커밋은 기존 트랜잭션과 함께 진행되고 롤백은 부분적으로 가능하다는 차이가 있다.

기존 트랜잭션과 함께 커밋되지만, 부분 롤백이 가능하기 때문에 아래와 같은 상황에 사용할 수 있을 것 같다.

- **부분적인 롤백이 필요한 경우**
- **중요한 작업에 대한 안정성 보장이 필요한 경우**
    - 부분 롤백이 가능하기에 기존 트랜잭션의 작업이 중요한 경우 이를 끝까지 진행할 수 있다.

---

**`MANDATORY`**

```java
@Transactional
public void some() {
	something();
	// ...
}

@Transactional (propagation = Propagation.MANDATORY) 
public void something() {
	// ...
}
```

`MANDATORY`는 기존 트랜잭션이 있다면 이를 활용하고 없다면 예외를 발생시킨다.

위와 같은 상황에서는 기존 트랜잭션을 활용하여 작업을 진행한다.

이 옵션은 아래와 같은 상황에 사용할 수 있을 것 같다.

- **트랜잭션의 일관성을 유지하고 싶을 때**
    - 해당 메서드가 기존에 유지되고 있던 트랜잭션 내부에서만 실행되게 할 수 있다.
    - 이는 별도의 트랜잭션에서 실행되는 것을 방지하여 트랜잭션의 범위를 정할 수 있다.

---

**`SUPPORTS`**

```java
@Transactional
public void some() {
	something();
	// ...
}

@Transactional (propagation = Propagation.SUPPORTS) 
public void something() {
	// ...
}
```

`SUPPORTS`는 기존 트랜잭션이 있다면 이를 활용하지만 없다면 트랜잭션 없이 작업을 진행한다.

위와 같은 상황에서는 기존 트랜잭션을 활용하여 작업을 진행한다.

이 옵션은 아래와 같은 상황에 사용할 수 있을 것 같다.

- **트랜잭션을 유동적으로 사용해야 하는 경우**
    - 하나의 메서드에서 조건부로 트랜잭션을 실행해야하는 경우에 채택할 수 있을 것 같다.
- **트랜잭션으로 인한 오버헤드를 감소시키고 싶은 경우**
    - 트랜잭션을 시작하고 관리하는 과정에서 리소스가 사용되기 때문에 조건부로 트랜잭션이 필요한 경우에 사용하여 리소스 낭비를 줄일 수 있을 것이다.

---

**`NOT_SUPPORTED`**

```java
@Transactional
public void some() {
	something();
	// ...
}

@Transactional (propagation = Propagation.NOT_SUPPORTED) 
public void something() {
	// ...
}
```

`NOT_SUPPORTED`는 기존 트랜잭션이 있다면 트랜잭션을 중지시키고 작업을 진행하며 없다면 트랜잭션 없이 작업을 진행한다.

위와 같은 상황에서는 기존 트랜잭션을 중지시키고 작업을 진행한 후 트랜잭션을 끝낸다.

이 옵션은 아래와 같은 상황에 사용할 수 있을 것 같다.

- **트랜잭션 내부에서 실행되지 않아야하는 경우**
- **트랜잭션으로 인한 오버헤드를 감소시키고 싶은 경우**
    - 트랜잭션을 시작하고 관리하는 과정에서 리소스가 사용되기 때문에 트랜잭션이 필요하지 않은 경우에 사용할 수 있다.

---

**`NEVER`**

```java
@Transactional
public void some() {
	something();
	// ...
}

@Transactional (propagation = Propagation.NOT_SUPPORTED) 
public void something() {
	// ...
}
```

`NEVER`은 기존 트랜잭션이 있다면 예외를 발생시키며, 없더라도 트랜잭션을 생성하지 않는다.

위와 같은 상황에서는 기존 트랜잭션이 있기 때문에 예외를 발생시킨다.

이 옵션은 아래와 같은 상황에 사용할 수 있을 것 같다.

- **트랜잭션 내부에서 실행되지 않아야하는 경우**
    - 트랜잭션 내부에서 실행된다면 `IllegalTransactionStateException`이 발생한다.

---

**isolation**

```java
@Transactional(isolation = Isolation.[격리 레벨])
// 격리 레벨 : READ_UNCOMMITTED, READ_COMMITTED, REPEATABLE_READ, SERIALIZABLE
public void some() {
	// ...
}
```

`isolation`은 트랜잭션의 격리 수준을 설정하는 속성이다.

동시에 여러 트랜잭션이 실행될 때 각 트랜잭션에 대한 접근성에 대해 설정할 수 있다.

아무것도 설정하지 않은 경우, `DEFAULT`는 사용하는 DBMS의 격리 수준을 따라간다.

동시에 여러 트랜잭션을 실행하다보면 아래와 같은 동시성 문제들이 발생할 수 있다.

- **Dirty Read**
    - 변경사항이 반영되지 않은 데이터를 다른 트랜잭션에서 읽었을 때 발생할 수 있는 데이터 불일치 문제
    - A 트랜잭션에서 아직 커밋되지 않은 B 트랜잭션의 데이터를 읽어와 작업을 진행한 경우
- **Non-Repeatable Read**
    - 트랜잭션 내에서 같은 데이터를 여러 번 조회 할 때 동일한 쿼리가 다른 결과를 반환하는 문제
    - A 트랜잭션이 여러 번 조회를 시도할 떄, B 트랜잭션에 의해 데이터가 업데이트 되어 다른 결과를 얻게되는 상황
- **Phantom Read**
    - 다른 트랜잭션에서 수행되는 작업으로 인해 기존 트랜잭션 내에서 동일한 쿼리가 다른 결과를 반환하는 문제
    - A 트랜잭션이 데이터에 접근했을 때, B 트랜잭션에 의해 새로 추가되거나 삭제되어 없던 결과가 생기거나 있던 결과가 사라지는 상황

`Non-Repeatable Read`는 데이터 업데이트로 인한 다른 결과를 얻게 된다면,

`Phantom Read`는 데이터 삽입/삭제로 인한 다른 결과를 얻게 된다는 차이점이 있다.

이러한 동시성 문제를 해결하기 위해서 아래와 같은 격리 수준을 설정해줄 수 있다.

`READ_UNCOMMITTED`

```java
@Transactional(isolation = Isolation.READ_UNCOMMITTED)
public void some() {
	// ...
}
```

`READ_UNCOMMITTED`는 가장 낮은 격리 수준으로 다른 트랜잭션에서 아직 처리 중인 데이터, 커밋되지 않은 데이터에 대한 접근을 허용한다.

MongoDB의 기본 격리 수준으로 설정되어 있다.

이 옵션은 모든 동시성 문제 (`Dirty Read`, `Non-Repeatable Read`, `Phantom Read`)가 발생할 수 있다.

하지만 그만큼 처리 속도가 가장 빠르기 때문에 데이터 일관성을 조금 지키지 못하더라도 좋은 성능을 내야할 때 사용된다.

---

`READ_COMMITTED`

```java
@Transactional(isolation = Isolation.READ_COMMITTED)
public void some() {
	// ...
}
```

`READ_COMMITTED`는 가장 많이 사용되는 격리 수준으로 다른 트랜잭션에서 아직 처리 중인 데이터, 커밋 되지 않은 데이터에 대한 읽기를 허용하지 않는다.

Postgres, Oracle, SQL Server DB의 기본 격리 수준으로 설정되어 있다.

이 옵션은 `Dirty Read` 문제를 방지할 수 있으나 작업 중인 데이터를 수정하거나 새로운 데이터를 추가하는 것을 막지 않으므로 `Non-Repeatable Read`, `Phantom Read`가 발생할 수 있다.

- `READ_COMMITED`는 말 그대로 커밋된 메시지만 읽어올 수 있다.
- 그렇기에 다른 트랜잭션에서 작업 중인 데이터에 접근하더라도 작업 중인 값이 아닌 이전에 커밋 되어있던 값을 언두 로그를 통해 불러온다.

- **언두 로그 (Undo Log)**

  언두 영역이라고도 불리는 언두 로그는 데이터를 변경하는 작업이 진행되었을 때, 변경되기 이전의 데이터를 기록해두는 영역이다.

    - 트랜잭션 롤백으로 인한 데이터 복구 작업 시에도 언두 로그를 참조하여 복구한다.
    - 위에서 언급했던 것 처럼 `READ_COMMITTED` 격리 수준에서 언두 로그를 활용하여 이전 커밋 기록을 가져온다.

---

`REPEATABLE_READ`

```java
@Transactional(isolation = Isolation.REPEATABLE_READ)
public void some() {
	// ...
}
```

`REPEATABLE_READ`는 특정 트랜잭션이 작업 중인 데이터를 다른 트랜잭션이 읽거나 수정할 수 없게 한다.

MySQL의 기본 격리 수준으로 설정되어 있다.

이 옵션은 `Dirty Read`와 `Non-Repetable Read` 문제를 방지할 수 있으나 새로운 데이터를 추가하는 것을 막진 않으므로 `Phantom Read`가 발생할 수 있다.

- `REPEATABLE_READ` 또한 `READ_COMMITTED`와 같이 읽기를 시도할 시 이전에 커밋 되어있던 값을 언두 로그를 통해 불러온다.

---

`SERIALIZABLE`

```java
@Transactional(isolation = Isolation.SERIALIZABLE)
public void some() {
	// ...
}
```

`SERIALIZABLE`은 가장 강력한 격리 수준으로 여러 트랜잭션을 순차적으로 진행시킨다.

그렇기에 여러 트랜잭션이 동시에 같은 테이블의 정보를 액세스 할 수 없다.

이 옵션은 모든 동시성 문제(`Dirty Read`, `Non-Repeatable Read`, `Phantom Read`)를 방지할 수 있어 가장 안전하지만, 여러 트랜잭션을 순차적으로 진행시키기 때문에 성능이 떨어진다.

그렇기에 극도로 안전한 작업이 필요한 경우가 아니라면 사용하지 않는게 좋다.

---

**readOnly**

```java
@Transactional(readOnly = true)
// true, false (기본 값)
public void some() {
	// ...
}
```

`readonly`는 트랜잭션을 읽기 전용으로 사용하고 싶을 때 사용하는 속성이다.

일반적으로 조회를 위한 메서드에 `readonly = true`를 사용하며, 이는 성능에 도움을 준다.

- **JPA에서 더티체킹을 진행하지 않아 성능에 이점을 가져갈 수 있다.**
    - 영속성 컨텍스트에서 더티 체킹을 진행하기 위해서는 업데이트 이전의 스냅샷을 저장한다.
    - `readonly = true`로 설정하면 데이터의 변경이 일어나지 않기에 더티 체킹을 진행하지 않아도 된다.
    - 그렇기에 이전의 스냅샷을 저장하지 않아도 되어서 성능에 이점을 가져갈 수 있다.
- **DB 이중화 구성(master/slave)을 사용할 때 자동으로 slave를 통해 읽게하여 DB 부하를 줄일 수 있다.**
    - 간단한 프로젝트에서는 단일 구성 DB를 사용하지만 실제 서비스에서는 DB의 장애를 빠르게 복구하고 트래픽을 분산시키기 위해서 실시간 복제본 DB을 운용하는 Master-Slave 구조를 사용하는 `Replication` 방식을 사용한다.
    - 이런 구조에서 `readonly = true` 는 slave DB를 통해서 데이터를 가져오기 때문에 트래픽을 분산시킬 수 있다는 이점이 생긴다.
- **TransactionId를 부여하지 않아 리소스 낭비를 줄일 수 있다. / `MySQL - InnoDB`**
    - TransactionId는 트랜잭션 관리, 복구 등을 위해서 트랜잭션 시작 시 발급된다.
    - 조회용 메서드의 경우는 트랜잭션을 데이터를 변경하지 않기 때문에 TransactionId를 발급하지 않아 오버헤드를 줄일 수 있다.

추가로 위에서 설명하지는 않았지만 `readonly = true`를 설정하게 됬을 때 하이버네이트의 플러시 모드가 `MANUAL`로 변경된다.

- **Flush Mode?**

  영속성 컨텍스트의 변경사항을 DB에 반영하여 동기화하는 작업을 플러시라고 한다.

  JPA는 2개, hibernate는 4개의 플러시 작업을 관리할 수 있는 플러시 모드를 제공한다.

    - **AUTO**
        - JPA와 hibernate 모두 지원한다.
        - 아래와 같은 상황에 플러시를 진행한다.
            - `flush()`를 직접 호출했을 때
            - 트랜잭션이 커밋되었을 때
            - 영속성 컨텍스트에 데이터 변경사항이 있으며, 해당 데이터가 포함된 DB 테이블에 조회하는 쿼리를 실행하기 전
    - **COMMIT**
        - JPA와 hibernate 모두 지원한다.
        - 아래와 같은 상황에 플러시를 진행한다.
            - `flush()`를 직접 호출했을 때
            - 트랜잭션이 커밋되었을 때
    - **ALWAYS**
        - hibernate에서만 지원한다.
        - 아래와 같은 상황에 플러시를 진행한다.
            - 모든 쿼리를 실행하기 전
    - **MANUAL**
        - hibernate에서만 지원한다.
        - 아래와 같은 상황에 플러시를 진행한다.
            - `flush()`를 직접 호출했을 때


위에서 `readonly = true`를 설정하게 되면 플러시 모드를 `MANUAL`로 설정한다고 했지만 우리는 JPA를 사용하고 있으니 지원하지 않는 것이라고 생각할 수 있다.

- 우리는 JPA를 사용하는게 아니라 Spring Data JPA를 사용하고 있다.
- 둘 다 이름에 JPA가 들어가며 여기저기 글에서 줄여 사용해 혼동이 올 수 있지만 꼭 기억하자.

우리가 사용하는 건 Spring Data JPA이고 이는 JPA를 구현한 hibernate 구현체를 더 쉽게 사용할 수 있도록 도와주는 모듈이기 때문에 `MANUAL` 모드를 사용하는 데에는 문제가 없다.

그럼 readonly를 사용하는 것에 대해서도 결론을 내려보자.

**조회용 메서드일 경우 `@Transactional(readonly = true)`를 사용하는 것과 아예 `@Transactional`을 사용하지 않는 것 중 뭐가 좋을까?**

뭐든지 그렇듯 `@Transactional(readonly = true)` 도 상황에 맞게 적절히 사용해야한다.

- **Lazy loading이 필요한 경우**
    - 영속성 컨텍스트는 트랜잭션 내에서 유지되기 때문에 지연 로딩이 필요하다면 `@Transactional(readonly = true)`을 사용해야한다.
- **Replication 방식을 사용하기에 트래픽 분산이 필요한 경우**
    - 위에서 설명했다시피 slave DB로 트래픽 분산을 시키고 싶다면 `@Transactional(readonly = true)`을 사용해야 한다.

위와 같은 상황이 아니라면 간단한 조회 메서드의 경우 `@Transactional(readonly = true)`을 사용하지 않는 것도 고려해볼만 하다.

- 기본적으로 `@Transactional`이 사용되면 프록시 객체에서 트랜잭션을 시작하고 관리하고 커밋하기 때문에 리소스가 사용될 수 밖에 없다.
- 지연 로딩을 고려할 필요가 없고, 중요도가 낮은 조회 작업이기에 트랜잭션을 사용할 필요가 없다면 `@Transactional` 어노테이션을 아예 사용하지 않는 것도 고려해 볼 만 하다.

---

**timeout**

```java
@Transactional(timeout = 60)
// -1 (기본 값), 초 단위로 지정
public void some() {
	// ...
}
```

`timeout`은 트랜잭션에 제한시간을 지정하는 속성이다.

기본값으로 -1이 설정되어 있기에 트랜잭션에 제한 시간이 설정되어 있지 않다.

DB의 작업이 예상보다 오래 걸리는 경우에 `timeout`을 통해 트랜잭션을 롤백시키기 위한 목적으로 사용된다.

Spring boot에서는 전역 설정을 통해 기본 timeout을 설정할 수도 있다.

```java
spring.transaction.default-timeout=60
```

---

**rollbackFor, rollbackForClassName**

```java
@Transactional(rollbackFor = {Exception.class}) 
@Transactional(rollbackForClassName = {"java.lang.Exception"})
public void some() {
	// ...
}
```

`rollbackFor` 과 `rollbackForClassName`은 지정된 예외가 발생했을 때 롤백하기 위한 속성이다.

두 속성은 같은 기능을 하며 단순히 예외를 지정하는 방식만이 다르다.

`@Transactional`은 기본적으로 `Error`와 `RuntimeException`와 같은 체크되지않은 상황에 대해서만 롤백을 진행한다.

그렇기에 체크된 특정 예외가 발생하였을 때의 롤백을 설정한다고 생각하면된다.

- **Error, Checked Exception, Unchecked Exception?**

  ![](https://i.imgur.com/AMGHnQe.png)

  Java의 예외 처리는 크게 3가지로 나눌 수 있다.

    - **Error**
        - 시스템에서 처리할 수 없는 비정상적인 상황이 발생했을 경우 Error가 발생한다.
        - 흔히 들어본 `OutOfMemoryError`, `StackOverflowError`등이 이에 해당된다.
    - **Checked Exception**
        - 체크된 예외는 RuntimeException을 제외한 Exception 하위 예외들로 반드시 예외처리를 해주어야한다.
        - 흔히 봤을만한 `FileNotFoundException`, `ClassNotFoundException`, `IOException`등이 이에 해된다.
    - **Unchecked Exception**
        - 체크되지 않은 예외는 `RuntimeException`의 하위 예외들로 체크 예외와는 반대로 예외처리를 강제하지 않는다.
        - 흔히 경험해볼 수 있는 `NullPointException`, `ArrayIndexOutOfBoundsException`등이 이에 해당된다.


---

**noRollbackFor, noRollbackForClassName**

```java
@Transactional(noRollbackFor = {RunTimeException.class}) 
@Transactional(noRollbackForClassName = {"java.lang.Exception.RunTimeException"})
public void some() {
	// ...
}
```

`noRollbackFor` 과 `noRollbackForClassName`은 지정된 예외가 발생했을 때 롤백하지 않기 위한 속성이다.

두 속성은 같은 기능을 하며 단순히 예외를 지정하는 방식만이 다르다.

위에서 언급한 `rollbackFor`과 `rollbackForClassName`의 반대 개념으로 체크되지 않은 특정 예외가 발생했을 때 롤백하지 않기위해 사용한다고 생각하면 된다.

---

## 부록 - 낙관적 락, 비관적 락

내용을 마무리하기 전에 트랜잭션과 밀접한 관련이 있는 락에 대해서도 알아보려고 한다.

### 락?

![예쁜 돌들과는 무관합니다.](https://i.imgur.com/oyBaGvs.png)

`락(Lock)`은 여러 사용자가 동시에 같은 데이터를 접근하는 상황에 데이터의 무결성과 일관성을 지키기 위해서 사용된다.

이러한 동시성 관리를 위한 두 가지 성격의 락에 대해서와 우리가 사용하는 기술과 어떤 접점이 있는지에 대해서 알아보자.

### 낙관적 락 (Optimistic Lock)

낙관적 락은 동시성 문제가 발생하지 않을 것이라고 가정하는 낙관적인 방법이다.

![](https://i.imgur.com/5OCnQwz.png)

데이터를 읽을 때는 락을 걸지 않고, 업데이트 할 때만 이전 데이터와 현재 데이터의 `version`을 검사하여 충돌이 발생하는지 확인한다.

- 낙관적 락은 번호, 해시코드, timestamp 같은 요소로 `version`을 관리한다.
- 업데이트 시 `version`을 확인하여 충돌이 발생했음을 확인하며, 충돌이 발생했을 시 업데이트를 진행하지 않는다.

말 그대로 데이터에 충돌이 일어나지 않을 것이라고 가정하기 때문에 조회 시의 불필요한 락 경쟁을 줄일 수 있다.

- 조회 시에는 버전을 확인하기 않기에 조회가 위주인 경우에는 충돌이 일어날 가능성이 적기에 적합하다.
- 하지만 빈번한 업데이트가 일어나는 곳에서는 충돌이 발생할 가능성이 높기 때문에 적합하지 않다.

위와 같이 낙관적 락의 경우는 버전에 의한 검사를 사용하기 때문에 DB의 기능을 사용하지 않고 애플리케이션 단계에서 처리한다고 말한다.

낙관적 락에 대해 마무리하기 전에 궁금증에 자문자답 해보려고 한다.

- **낙관적 락은 트랜잭션을 필요로 하지 않는다?**
    - 몇몇 글에서 낙관적 락은 트랜잭션을 필요로 하지 않는다는 글을 보았다.
    - 이는 아마 낙관적 락 자체로도 동시성을 해결할 수 있으므로 트랜잭션을 사용하지 않아도 된다라는 의미로 해석된다.
    - 낙관적 락에서 데이터 충돌이 발생했을 때, 애플리케이션에서 충돌 로직을 해결하기 위한 로직을 작성한 시나리오들을 사용한다. (재시도, 사용자에게 알림 등)
    - 위와 같은 해결책 중 트랜잭션에서의 롤백도 시나리오가 될 수 있기에 “트랜잭션을 사용하지 않아도 된다”로 오해하지 말자.

---

### 비관적 락 (Pessimistic Lock)

비관적 락은 동시성 문제가 발생할 것이라고 가정하는 비관적인 방법이다.

비관적 락은 트랜잭션이 시작될 때 `Shared Lock`이나 `Exclusive Lock`을 걸게 된다.

- **Shared Lock, Exclusive Lock**

  **공유 락 (Shared Lock)**

    - 다른 트랜잭션이 작업하고 있는 데이터에 대한 추가적인 공유 락은 허용하지만, 베타 락은 허용하지 않는 잠금이다.
        - 여러 트랜잭션이 하나의 데이터에 공유 락을 생성할 수 있기에 다른 트랜잭션이 읽고 있는 데이터를 읽을 수 있다.
        - 다른 트랜잭션이 읽고 있기에 공유 락이 걸려있는 데이터를 변경할 수 없다.

  **배타 락 (Exclusive Lock)**

    - 같은 데이터에 다른 트랜잭션이 생성되는 것을 허용하지 않는 잠금이다.
        - 하나의 데이터를 변경하는 작업을 진행하고 있어서 베타 락이 걸려있다면 그 데이터는 다른 트랜잭션에서 읽거나 변경할 수 없다.
        - 트랜잭션을 사용하지 않는 읽기는 가능하다.

![](https://i.imgur.com/WGL3mS7.png)

데이터에 접근 시 미리 락으로 선점을 하기 때문에 해당 트랜잭션이 종료되기 전까지 다른 트랜잭션에게 제한이 걸린다.

- `Shared Lock` 을 걸었을 경우에는 다른 트랜잭션도 조회를 위한 Shared Lock을 생성해서 조회해야한다.
- `Exclusive Lock` 의 경우는 해당 데이터을 사용하고 있는 다른 트랜잭션이 없을 때 까지 대기하여 작업을 수행한다.

위와 같이 비관적 락의 경우는 DB의 락을 사용하여 동시성을 제어한다.

---

### 격리 수준 속성과의 관계

**격리 수준과 락은 둘 다 동시성 제어를 위해 사용되지만 목적에 차이가 있다.**

- 격리 수준은 트랜잭션이 다른 트랜잭션의 중간 결과에 얼마나 접근 할 수 있는지에 대해 조절하기에 트랜잭션 간의 격리가 목적이다.
- 하지만 락은 데이터에 대한 접근 자체에 목적을 둔다.

격리 수준을 높히는 것만으로는 동시성이 높은 상황에서 발생할 수 있는 모든 문제를 해결할 수 없다.

- 격리 수준이 `REPEATABLE_READ`일 때
    - A가 계좌 잔액을 조회하는 동안 B는 계좌에 돈을 입금하려고 한다.
    - `REPEATABLE_READ`는 A가 데이터를 처리하는 동안 B가 해당 데이터를 변경하지 못하게 막을 수는 있지만, 새로운 돈을 입금 하는 것 (새로운 행을 추가하는 것)을 막을 수 없다.
    - **PHANTOM_READ 문제 발생**
- **SERIALIZABLE 을 사용하면 되는 거 아닌가?**
    - `SERIALIZABLE`로 해결이 가능하긴하다.
    - 하지만 `SERIALIZABLE` 트랜잭션이 순차적으로 처리되기 때문에 **성능 저하**와 **데드락**이 발생할 수 있는 여지가 있다.

이런 문제는 더 낮은 격리 수준과 락을 같이 사용하면 성능과 같이 데이터의 일관성과 무결성을 챙길 수 있다.

- 격리 수준은 `REPEATABLE_READ`, 락은 `비관적 락` 사용
    - A는 계좌 잔액을 조회, B는 계좌에 돈을 입금 하려고 함.
    - `REPEATABLE_READ` 에 의해 A가 데이터를 조회하는 동안 B가 돈을 입금하더라도 A의 조회 결과에 반영되지 않음.
    - `비관적 락`에 의해 A가 계좌 잔액을 조회하는 동안 공유 락을 생성하고, B는 대기하였다가 조회가 끝난 후 배타 락을 획득하여 입금을 진행함.
- **데이터의 일관된 조회를 보장하면서, 필요한 경우 데이터 변경을 안전하게 처리할 수 있게 함.**

격리 수준은 REPEATABLE_READ, 락은 낙관적 락을 사용한 경우도 궁금할 것 같아서 다른 예제로 준비해봤다.

- 격리 수준은 `REPEATABLE_READ`, 락은 `낙관적 락` 사용
    - A와 B가 상품 X에 대한 주문을 함.
    - A가 주문을 완료하여 상품 X의 재고를 감소시키고, `REPEATABLE_READ` 에 의해 A의 트랜잭션이 완료될 때까지 B는 A가 변경한 재고의 변경사항을 볼 수 없음.
    - B도 주문을 완료하지만 `낙관적 락`을 사용하기에 주문을 마무리하기 전에 상품 X의 재고가 충분한지 확인하기 위해 데이터의 변경 사항을 검증함.
    - 검증 과정에서 A에 의한 재고 감소가 반영되며, 재고가 부족하다면 B에게 알림.
- **동시성이 높은 환경에서 데이터 변경 시 충돌을 방지할 수 있음.**
- [위 상황과 비슷한 트러블 슈팅 글을 발견하여 공유한다.](https://hudi.blog/troubleshooting-of-category-role-concurrency-issue/)

이처럼 격리 수준과 락의 적절한 조합을 통해 데이터의 안정성을 높이고 성능을 개선할 수 있다.

---

### 읽기 전용 속성과의 관계

`@Transactional(readOnly=true)` 를 사용했을 때, 낙관적 락에 영향을 미칠 수 있다는 글을 봤다.

내용은 아래와 같다.

> `@Transactional(readOnly=true)`를 사용하면 이러한 **낙관적 락 동작에 영향**을 미치게 된다.
>
>
> 만일 `@Transactional(readOnly=true)`로 설정한 메서드에 엔티티를 수정하는 로직이 있을 경우, 해당 트랜잭션이 엔티티를 수정하는 것이 아니라 읽기 전용으로 설정했기 때문에 버전 번호를 확인하지 못할 수 있다.
> 이때 충돌을 감지하지 못하고 동시에 발생한 트랜잭션의 변경 사항을 덮어쓰게 되어 **데이터 불일치 문제**가 발생할 수 있다.
>

요약하자면,  `@Transactional(readOnly=true)` 를 설정한 메서드에서 엔티티를 수정하는 로직을 넣지 않도록 조심해야한다는 내용이다.

조회용 메서드에 수정하는 로직을 넣는다는 전제 자체가 문제이지만, 만일 조회용 메서드에 수정하는 로직을 넣는다고 하더라도 직접 `flush()`를 진행하지 않는 이상 업데이트가 되지 않기에 낙관적 락이 적용된 도메인 객체의 `version`도 올라가지 않는다.

그렇기에 동시에 발생한 트랜잭션의 변경 사항을 덮어쓰게 될 일은 없다.

또한 조회용 메서드로 설정하더라도 조회는 되기 때문에 버전을 확인하고 충돌을 감지할 수는 있다.

**그렇기에 `@Transactional(readOnly=true)`이 낙관적 락에 영향을 줄 수 있다기 보다는 `@Transactional(readOnly=true)`에는 조회 로직만 작성하고 수정 로직을 넣지 않아야 한다는 점을 더 알아두면 될 것 같다.**

---

## 정리

트랜잭션은 데이터 작업에 있어 매우 중요한 요소이다.

트랜잭션을 사용함으로써 작업범위를 정할 수 있고, 커밋과 롤백을 이용하여 중요한 작업은 더 비중을 높게 둘 수도 있으며, 특정상황에 대한 처리와 성능의 향상까지 기대해 볼 수 있다.

그렇기에 좀 복잡하더라도 트랜잭션에 대해서 이해한다면 좋은 결과물을 만드는데 도움이 될 것이다.

추가로 글을 정리하다가 읽어보면 좋을 것 같은 글을 찾게되어 공유한다.

[@Transactional의 해로움 (channel.io)](https://channel.io/ko/blog/bad-transactional)

## 참고

---

[[MYSQL] 📚 트랜잭션(Transaction) 개념 & 사용 💯 완벽 정리](https://inpa.tistory.com/entry/MYSQL-📚-트랜잭션Transaction-이란-💯-정리)

[[Spring DB] 4. 스프링의 트랜잭션 매니저](https://everenew.tistory.com/266)

[Spring & JTA(분산 트랜잭션)](https://devssul.tistory.com/33)

[[Spring] 트랜잭션의 전파 설정별 동작](https://deveric.tistory.com/86)

[[Spring] @Transactional 어노테이션 이해하기(1) 전파유형(Propagation) 과 격리수준(Isolation)](https://colevelup.tistory.com/34)

[[Spring] Spring 트랜잭션의 세부 설정(전파 속성, 격리수준, 읽기전용, 롤백/커밋 예외 등) - (2/3)](https://mangkyu.tistory.com/169)

[Non-Repeatable Read vs Phantom Read?](https://stackoverflow.com/questions/11043712/non-repeatable-read-vs-phantom-read)

[@Transactional(readOnly = true)를 왜 붙여야 하나요](https://hungseong.tistory.com/74)

[JPA, Hibernate, 그리고 Spring Data JPA의 차이점](https://suhwan.dev/2019/02/24/jpa-vs-hibernate-vs-spring-data-jpa/)

[JPA & Hibernate Flush Mode](https://keencho.github.io/posts/jpa-flush/)

[[Java] Checked Exception vs Unchecked Exception 정리](https://devlog-wjdrbs96.tistory.com/351)

[[JPA] @Transactional(readOnly = true)의 효과와 사용 시 주의할 점](https://seungjjun.tistory.com/256)

[[JPA] 낙관적 락(Optimistic Lock)과 비관적 락(Pessimistic Lock)에 대해](https://mozzi-devlog.tistory.com/37)

[JPA의 낙관적 락과 비관적 락을 통해 엔티티에 대한 동시성 제어하기](https://hudi.blog/jpa-concurrency-control-optimistic-lock-and-pessimistic-lock/)

[낙관적 락과 비관적 락은 갱신 손실 문제를 어떻게 해결하는가](https://velog.io/@balparang/낙관적-락과-비관적-락은-Race-Condition을-어떻게-해결하는가)
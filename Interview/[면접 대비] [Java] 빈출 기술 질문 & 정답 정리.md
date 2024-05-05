# [면접 대비] [Java] 빈출 기술 질문 & 정답 정리

<br>
<details>
<summary><strong>Q. JAVA의 실행 방식을 설명해 주세요.</strong></summary>

1. javac가 자바 소스코드인 .java 파일을 읽어 자바 바이트 코드인 .class 파일로 변환한다.
2. Class Loader가 class 파일을 JVM으로 로딩한다.
3. 로딩된 class 파일들은 실행 엔진을 통해 해석된다.
4. 해석된 바이트 코드는 Runtime Data Areas에 배치되어 작업이 수행된다.
</details>

<br>

<details>
<summary><strong>Q. JVM이 무엇인가요?</strong></summary>

* **JVM은 Java Virtual Machine의 약자로, 스택 기반의 가상 머신이다.**
* JVM의 주요 역할은 Java 애플리케이션을 클래스 로더를 통해 읽어 Java API와 함께 실행시키는 것이다. 
* 또한 JVM은 GC(가비지 컬렉션)을 통한 메모리 관리를 수행한다.

<br>
<details>
<summary><strong>QQ. JVM의 구조를 설명해주세요.</strong></summary>
    
* **JVM의 구조는 크게 Garbage Collector, Execution Engine, Class Loader, Runtime Data Area로 나눠진다**.
    * Garbage Collector : 힙 영역에서 사용되지 않은 객체들을 주기적으로 제거한다.
    * Class Loader : JVM 내로 .class 파일을 로드해 Runtime Data Area에 배치한다.
    * Execution Engine : Runtime Data Area에 배치된 .class 파일의 바이트 코드를 실행한다.
      * 실행 시 읽는 방식 (Interpreter)
        1. 인터프리터 방식 : 바이트 코드를 한줄씩 실행한다.
           * 최초 JVM이 차용하던 방식, 느린 속도, JIT보다 저렴한 비용
        2. JIT 방식 : 인터프리터가 반복되는 코드를 발견하면 코드를 어셈블러와 같은 네이티브 코드로 신속하게 실행한다. 
           * 인터프리터에서 발전된 방식, 네이티브 코드 변환에 의한 신속한 변환, 높은 비용
        ```agsl
        JVM은 인터프리터 방식과 JIT 방식을 모두 차용한다.
        일정한 기준이 넘어가면 인터프리터 방식에서 JIT 방식을 통해 바이트 코드를 변환한다.
        ```
    * Runtime Data Area : 프로그램 실행 중에 사용되는 데이터가 저장되는 영역
  
    <br>
  
<details>
<summary><strong>QQQ. Runtime Data Area의 구조를 설명해주세요.</strong></summary>
    
* Runtime Data Area는 Method, Stack, Heap, PC register, Native Method Stack 총 5개의 Area로 구성된다.
  * Method Area : 모든 쓰레드가 공유하는 메모리 영역
    * Class, Interface, Method, Field, Static 변수 등의 바이트 코드를 보관한다.
  * Heap Area : new 키워드로 생성된 객체와 배열이 생성되는 영역
    * 가비지 콜렉터가 주기적으로 참조되지 않는 힙 영역의 객체와 배열을 제거해 메모리를 확보한다.
  * Stack Area : 메소드 호출 시 해당 메소드의 지역변수, 매개변수 등 메소드 내부의 내용을 저장하는 영역
    * 메소드 별로 Stack Area에 할당되는 공간을 Stack Frame이라 함
    * 메소드 수행 종료 후 스택 프레임은 삭제됌
  * PC register Area : 쓰레드 시작 시 생성되어 쓰레드별 한개씩 존재
    * 쓰레드가 수행중인 JVM 명령의 주소를 저장
  * Native Method Stack Area : 자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역
</details>
</details>
</details>

<br>

<details>
<summary><strong>Q. GC, 가비지 콜렉팅에 대해 설명해주세요.</strong></summary>

* **가비지 콜렉팅은 가비지 콜렉터에 의해 런타임 데이터 영역의 힙 영역에서 사용하지 않는 객체를 제거하고, 메모리를 확보하는 작업을 총칭한다.**
* 이러한 객체를 제거하는 작업이 필요한 이유는 자바 언어는 개발자가 메모리를 직접 해제해 줄 수 없는 언어이기 때문이다.
* GC를 수행할 때는 GC 수행 스레드 이외의 모든 스레드가 정지하며, 이를 stop-the-world라고 한다.
* GC는 Minor GC와 Major GC로 나뉘어지며, 이들은 각각 객체의 생명주기에 따라 Young / Old 한 객체의 참조 메모리를 해제하는 작업이다.
    * Minor GC : 비교적 짧은 생명주기(Young Generation 영역)의 객체 메모리를 해제하는 작업
      1. 객체가 할당될 시 최초 Eden 영역에 할당된다.
      2. Eden 영역이 가득찬다면, 참조가 남아있는 객체를 mark하고 이를 survivor 영역으로 복사한다. (mark and sweep)
      3. Minor GC가 발생할 때마다, 참조가 남아있는 객체는 mark and sweep에 의해 survivor 영역으로 이동한다. 해당 survivor에서도 살아있는 객체는 다음 age의 survivor 영역으로 이동한다.
      4. 이렇게 특정 survivor의 age에 임계하면, old generation 영역으로 해당 객체가 이동한다. (promotion)
    * Major GC : 오랫동안 살아남은 객체(Old Generation 영역) 중 객체 메모리를 해제하는 작업
      1. Minor GC와 다르게 old generation에서의 mark and sweep에서 mark는 삭제될 객체를 mark한다.
> Major GC(Old generation)와 Minor GC(young generation) 로 영역이 구분되는 이유?
> 
> 대부분의 객체는 금방 접근 불가능한 상태가 되며, 아주 일부의 객체만 살아남는다. <br>
> 따라서 오래된 객체에서 젊은 객체의 참조는 아주 적게 존재한다. <br>
> 따라서, JVM은 가비지 컬렉터를 Major와 Minor로 나누어 힙 전체에서 가비지 컬렉팅을 하지 않고, <br>
> young generation인 Minor GC를 수행하므로써 주기적으로 메모리 낭비를 방지한다.

<br>

* 메모리는 단편화된 상태로 Mark And Sweep 이후 메모리를 일정 영역에 응집한다. (Compaction)
    * 이러한 메모리 Mark And Sweep, Compaction 작업을 Mark-Sweep-Compact 알고리즘이라 한다.

<br>

[#REFERENCE, Garbage Collector의 동작 원리](https://sihyung92.oopy.io/java/garbage-collect/1)

</details>

<br>

<details>
<summary><strong>Q. 정적 (static) 변수에 대해 설명해주세요.</strong></summary>

* static 은 정적, 공통의 의미로서, JVM에서 클래스 로더가 메소드 메모리 영역에 적재하는 멤버이다.
* static 을 통해 생성된 정적 멤버는 클래스 별로 관리가 된다.
* static 을 통해 생성된 정적 멤버는 모든 객체가 공유하며 하나의 멤버를 어디서든지 참조할 수 있다.
* 그러나, 메소드 메모리 영역에 존재하는 static 멤버는 GC의 관리 영역 밖에 있으므로 프로그램 종료 시 까지 메모리가 할당된 채로 존재한다.
  * 따라서, 과도한 static 의 사용은 메모리적 악영향을 야기할 수 있다.

</details>

<br>

<details>
<summary><strong>Q. 접근 제어자에 대해 설명해주세요.</strong></summary>

> 객체 지향 설계 기법 중 캡슐화와 정보 은닉에 대한 학습이 선행되면 좋습니다.
* **접근 제어자는 클래스 멤버 선언 시 사용하여 접근 권한을 설정하는 키워드를 의미한다.**
* 접근 제어자를 설정하는 이유는 객체지향적 관점 설계에서의 캡슐화와 정보 은닉 개념과 연관이 있다.
  * 캡슐화 : 관련 있는 데이터를 한 곳에 모아 관리하고, 외부로부터 보호하는 것
    * `ex)` 완성된 클래스를 사용하는 개발자는 private 멤버에 접근할 필요가 없고, 사용할 기능만 외부에 표출함으로써 개발 효율 증가 가능
  * 정보 은닉 : 접근 제한 설정을 통해 보여주고 싶은 객체 내부 정보를 선택적으로 외부에 보여주는 것
    * `ex)` private 멤버를 가지고 있는 완성된 클래스는 컴파일 시 private 멤버의 내용을 확인할 수 없으며, 개발자가 사용 시 정보 은닉을 통해 클래스 사용 간 내부 필드 관련 개발 실수 방지 가능 
* 접근 제어자의 종류
  * private : 클래스 내부에서만 접근 가능
  * default : 동일 패키지에서만 접근 가능
  * protected : 상속받은 클래스 내에서 접근 가능
  * public  : 전체 영역에서 접근 가능

</details>

<br>

<details>
<summary><strong>Q. 원시 타입과 참조 타입에 대해 설명해주세요. </strong></summary>

**원시타입은 실제 데이터(값) 을 저장하는 타입으로, 런타임 데이터 영역의 스택 영역에 저장된다.**
* 원시 타입은 항상 값이 존재해야 하며, 멤버 변수가 초기화 될 때 기본값을 가진다.
* 총 8개의 타입이 있다.(괄호는 바이트 크기)
  * boolean(1), byte(1), short(2), char(2), int(4), float(4), long(8), double(8)
* **참조타입은 생성된 객체의 주소 값을 저장하는 타입으로, 런타입 데이터 영역의 힙 영역에 저장된다.**
  * 참조타입은 null pointer를 가질 수 있다.


</details>

<br>

<details>
<summary><strong>Q. Annotation에 대해 설명해주세요. </strong></summary>

* **애너테이션은 인터페이스를 기반으로 한 문법으로 해당 코드를 설명하는 메타 데이터의 한 형태이다.**
  * 기능 : 사전적 의미의 주석 기능도 수행하지만, 고차원적으로 기능 주입도 수행한다.
  * 역할 : 메타 데이터의 일종으로 프로그램의 정보를 제공하지만, 프로그램의 일부는 아니다.
  * 특징 : 
    * javac에게 코드 문법 에러를 체크하도록 정보를 제공 `ex)` @SuppressWarning
    * 개발 툴이 빌드나 배치 시 코드를 자동으로 생성하게 정보를 제공 `ex)` @Override
    * 이미 만들어진 빌트인 애너테이션과, 커스텀 어너테이션을 생성하기 위한 메타 에너테이션이 있다.
    * 이러한 어노테이션의 정보 읽기와 동작에는 Java Reflection API가 사용된다.
      * 메타 애너테이션의 종류
        * @Retention : 애너테이션 유지 범위 지정 (컴파일 시점 전 / 클래스 / 런타임)
        * @Inherit : 애너테이션 상속 범위 지정
        * @Target : 타입, 필드, 파라미터 등 해당 애너테이션을 어디에 사용할지 지정

<br>

[#REFERENCE, Java Annotation](https://nesoy.github.io/articles/2018-04/Java-Annotation)

</details>

<br>

<details>
<summary><strong>Q. Java Reflection API에 대해 설명해주세요. </strong></summary>

* **Java Reflection API는 구체적인 클래스 정보에 접근하게 해주는 자바 API이다.**
* Reflection API는 런타임 데이터 영역의 메소드 영역에 저장되어 있는 클래스 정보를 가져온다.
* 런타임에 동적으로 타입을 분석하고 정보를 가져오므로 JVM을 최적화할 수 없어 성능 오버헤드 발생 가능성이 있다.
* 예시
  * A.getClass, A.getMethod, 이후 method.invoke
  * Spring IOC에서 Bean을 동적으로 호출해 의존성 주입 시 Reflection API를 사용한다.

<br>

[#REFERENCE, reflection-api](https://tecoble.techcourse.co.kr/post/2020-07-16-reflection-api/)

</details>

<br>

<details>
<summary><strong>Q. Generic에 대해 설명해주세요. </strong></summary>

* **Generic은 클래스, 인터페이스, 메서드를 정의할 때 타입을 파라미터로 사용해 타입 안전성을 제공한다.**
* 객체의 타입을 제네릭으로 명시함으로써 컴파일 시 타입 체크 기능을 제공한다.
* 이를 통해 제네릭은 타입 캐스팅의 번거로움을 줄여주며, 코드 복잡성 감소, 타입 안정성을 보장하는 효과가 있다.


</details>

<br>

<details>
<summary><strong>Q. 클래스와 객체가 무엇이며, 차이점에 대해 설명해주세요. </strong></summary>

* **클래스는 동일한 속성을 가진 데이터의 집합체를 의미한다.**
  * 클래스는 이러한 동일 속성을 가진 실제 사물, 정보인 객체를 정의하는 틀, 설계도의 역할을 한다.
* **객체는 실제 식별 가능한 객체 혹은 사물, 정보를 의미한다.**
  * 이렇게 클래스에서 생성된 객체 단위를 인스턴스(실체화)로 통칭한다.
* **따라서, 클래스는 추상적인 개념이며, 객체는 클래스를 실체화한 개념에서 차이점이 있다.**

</details>

<br>

<details>
<summary><strong>Q. 인터페이스와 추상 클래스가 무엇이며, 차이점에 대해 설명해주세요. </strong></summary>

* **인터페이스는 구현 객체가 같은 동작을 수행하는 것을 보장하기 위해 구현한다.**
  * 다중 상속이 가능하며, 인터페이스 구현 관계 간 연관관계가 없을 수 있다.
* **추상 클래스는 객체의 추상적 상위 개념으로 공통된 개념을 표현할 때 사용한다.**
  * 단일 상속이 가능하며, 상속 집합 관계간 연관관계가 있다.
* **따라서, 인터페이스와 추상 클래스에는 단일 / 상속 여부 및 연관관계에 있어 차이점이 있다.**

</details>

<br>

<details>
<summary><strong>Q. Overriding과 Overloading이 무엇이며, 차이점에 대해 설명해주세요. </strong></summary>

* **오버라이딩은 상위 클래스의 메소드를 재정의하는 것을 의미한다.**
  * 객체지향적 설계 관점에서 다형성을 보장한다.
  * @Override를 명시함으로써 컴파일 타임에 해당 메소드가 오버라이딩임을 명시하고 안정성을 보장한다.
  * 런타임 다형성이다.
* **오버로딩은 같은 클래스 내 동일 메소드 이름을 가지지만 매개변수 타입, 개수가 다르게 구현될 수 있는 것을 의미한다.**
  * 컴파일 타임 다형성이다.
* **따라서 오버로딩과 오버라이딩의 차이는 다형성을 어느 시점에서 보장하는지와, 각각의 사용 용도 / 기능에 차이점이 있다.**

</details>

<br>

<details>
<summary><strong>Q. Collection Framework에 대해 설명해주세요. </strong></summary>

* **컬렉션 프레임워크는 Java Collection에서 널리 알려져 있는 Stack, Queue, LinkedList 등 자료구조를 효율적으로 java 내에서 사용할 수 있게 만들어 놓은 라이브러리이다.**
  * List, Set은 Collection Interface를 상속받지만, Map 인터페이스는 구조상의 차이라 별도로 정의된다.

</details>

<br>

<details>
<summary><strong>Q. 동등성(equality)과 동일성(identity)이 무엇이며, 차이점이 무엇인지 설명해주세요.  </strong></summary>

* **동등성은 객체가 동일한 논리적 기준을 가지고 있음을 의미하며, 동일성은 객체의 메모리 내 주소값이 같음을 의미한다.**
  * 동등성(equality)
    * 논리적으로 같은 기준를 지녔는지를 확인하며 이에 따라 동등성의 기준이 필요하다.
    * 개발자가 원한다면 equals 메소드를 오버라이드해 동등성의 판단 기준을 정의한다.
      * 기본적으로 equals 메소드는 주소값이 동일한지만 체크한다. (==)
  * 동일성(identity)
    * 객체가 동일한 메모리 주소값을 가지고 있는 것을 의미한다.
    * == 로 기본적으로 정의되어 있다.

<br>

[#REFERENCE, 동일성과 동등성](https://creampuffy.tistory.com/140)

</details>

<br>

<details>
<summary><strong>Q. Checked Exception과 Unchecked Exception이 무엇이며, 차이점이 무엇인지 설명해주세요.  </strong></summary>

* **Checked Exception은 반드시 예외 처리를 해야하는 특징을 가지고 있는 Exception이며, 컴파일 단계에서 발생할 수 있는 Exception을 의미한다.**
  * 정형적인 Format에 맞춘 Exception을 정의하므로 해당 기능에 필수적인 Exception을 강제하여 오류를 방지한다.
  * FileNotFoundException, IOException 등이 있다.
  * 오류가 발생해도 Transaction은 rollback되지 않고, 정상적으로 commit된다.
* **Unchecked Exception은 RuntimeException을 상속받는 하위 Exception으로, 말 그대로 예외 처리를 강제하지 않는 Exception이다.**
  * 개발자들에 의해 실수로 발생하는 것이므로 에러를 강제하지 않는다.
  * NPE, ArrayIndexOutOfBoundsException 등이 있다.
* `+` Error는 시스템 자체에 비정상적인 상황이 발생했음을 의미한다. 처리할 수 있는 방법이 없다.
  * OutOfMemoryError나 StackOverFlowError 등이 있다.
  * 오류 발생 시 Transaction은 rollback된다.
* **따라서, Checked Exception과 Unchecked Exception은 RuntimeException의 상속 유무와, rollback 유무에서 차이점이 있다.**

<br>

[#REFERENCE, Error, Checked Exception, Unchecked Exception](https://devlog-wjdrbs96.tistory.com/351)

</details>

<br>

<details>
<summary><strong>Q. Mutable과 Immutable이 무엇이며, 차이점이 무엇인지 설명해주세요. </strong></summary>

* **Mutable은 객체의 수정을 허용하는 가변성이 있는 객체이다.**
  * 종류 : List, HashMap, StringBuilder, StringBuffer 등
  * 병렬 처리 시 값을 보장할 수 없다. (thread-unsafe)
    * thread-safe 하게 String을 가변적으로 사용하려면 String Buffer를 사용해야 한다. 
* **Immutable은 객체의 수정을 허용하지 않는 불변성이 있는 객체이다.**
  * 종류 : String, Wrapper Class 등
  * 병렬 처리 시 값을 보장한다. (thread-safe)
* **따라서 두 속성의 차이는 병렬 처리에서의 문제와, 객체 수정에서 차이점이 있다.**

<br>

[#REFERENCE, 동일성과 동등성](https://creampuffy.tistory.com/140)

</details>

<br>

<details>
<summary><strong>Q. 객체지향이 무엇인지 설명해주세요. </strong></summary>

* **객체 지향(Object Oriented)은 실세계의 개체를 같은 속성과 행위(메서드)가 결합한 형태의 개체로 표현하는 기법이다.**
* 객체지향의 기법에는 캡슐화, 정보 은닉, 다형성, 추상화, 의존 관계 생성 등이 있다.
* Java에서는 이러한 객체 지향 설계와 기법을 클래스 및 객체, 접근 제어자, 상속, 캐스팅 등으로 구현한다.

</details>

<br>

<details>
<summary><strong>Q. SOLID 원칙에 대해 설명해주세요. </strong></summary>

* **SOLID 원칙은 객체 지향을 추구하기 위한 설계 원칙이다.**
  * Single Responsibility Principle (단일 책임 원칙) : 하나의 클래스는 하나의 목적과 책임을 가지고 있어야 한다는 원칙
    * 다른 4원칙의 기초 원칙
  * Open Close Principle (개방-폐쇄 원칙) : 소프트웨어의 구성요소는 확장에는 열려있고 변경에는 닫혀있어야 한다는 원칙
    * 해석 : 변경될 것은 계속 변경되게 두고, 변경되지 않아야 할 것은 닫혀있게끔 해야한다는 원칙
  * Liskov Substitution Principle (리스코프 치환 원칙) : 하위 클래스는 언제나 자신의 상위 클래스로 교체할 수 있어야 한다는 원칙
    * 해석 : 즉, 상속되는 객체는 부모 객체를 완전히 대체해도 아무런 문제가 없도록 설계해야 한다는 것
  * Interface Segregation Principle (인터페이스 분리 원칙) : 자신이 사용하지 않는 인터페이스는 만들지도 말아야 한다는 원칙
    * 해석 : 즉, 클라이언트들이 꼭 필요한 메서드와 인터페이스만 이용할 수 있게 불필요한 인터페이스는 설계하지 말아야 한다는 것
  * Dependency Inversion Principle (의존성 치환 원칙) : 추상을 매개로 관계를 최대한 느슨하게 만들어야 한다는 것
    * 해석 : 즉, 변경이 없는 것(유사 불변)의 속성에 대해서 관계를 맺어야 변경이 적다는 것

<br>

</details>

<br>

<details>
<summary><strong>Q.직렬화와 역직렬화에 대해 설명해주세요. </strong></summary>

* **직렬화는 시스템 내부에서 사용되는 객체 또는 데이터를 외부에서 사용할 수 있도록 바이트 형태로 데이터를 변환하는 기술이다.**
  * 직렬화의 종류에는 JSON, CSV, XML, Binary 등이 있다. (Java 직렬화는 별개이며, Java 시스템 <-> Java 시스템 통신 간 사용한다)
* **반대로, 역직렬화는 시스템 외부에서 사용되었던 바이트 형태의 데이터를 내부에서 사용할 수 있도록 데이터를 변환하는 기술이다.**
* **Java의 직렬화와 역직렬화 기술이 필요한 이유는 데이터의 메모리 구조에 기반한다.**
  * Java의 원시 타입은 스택 영역에 저장되며, 디스크에 저장하거나 통신할 때 그 값이 유지된다.
  * 반면 Java의 참조 타입은 힙 메모리에 저장되며, 스택은 이 힙 메모리를 참조하는 구조로, 주소값만을 가지고 있다.
  * 따라서 참조 타입의 경우에는 프로그램을 종료하고 다시 실행 시에 주소 값을 가져오더라도 해당 값의 데이터는 날아가버리므로 직렬화에 의한 데이터 보존과 통신이 필요하다.
* **즉, 직렬화/역직렬화는 JVM의 메모리에 상주되어 있는 객체 데이터를 영속화하고, 이를 다른 시스템으로 통신하기 위해 사용한다.**

<br>

[#REFERENCE, java 직렬화와 역직렬화](https://steady-coding.tistory.com/576)

</details>

<br>

<details>
<summary><strong>Q. Java에서 null을 안전하게 다루는 방법에 대해 설명해주세요. </strong></summary>

1. 디버깅 환경에서는 Assert를 활용해 값을 강제하여 null을 방어할 수 있다.
2. Optional을 사용해 리턴 타입에서 조건부 null을 반환하고 이에 따른 분기를 설계할 수 있다.
3. 또한 개발자는 사전 조건과 사후 조건을 명확히 해 계약에 의한 설계(Design By Contract)를 실천해야 한다.

<br>

[#REFERENCE, Design By Contract](https://kukim.tistory.com/76)

</details>

<br>

<details>
<summary><strong>Q. Java의 동시성 이슈(공유 자원 접근)에 대해 설명해주세요. </strong></summary>

* **Java의 동시성 이슈는 멀티 스레드 환경에서 동일한 자원에 접근할 시 스레드 간 경쟁 상태로 인해 개발자가 의도하지 않은 방향으로 값이 조회, 변경, 교착되는 문제를 의미한다.**
  * Java의 동시성 이슈의 발생 원인과 현상
    * 경쟁 상태 : 두 개 이상의 스레드가 공유 자원에 동시에 접근해 데이터의 일관성(정합성)이 깨져버리는 문제
    * 교착 상태 : 두 개 이상의 스레드가 서로가 가진 자원을 기다리며 무한히 대기하는 문제
    * 메모리 일관성 문제 : 공유된 변수의 값을 캐시에 저장하여 사용하는 경우, 캐시와 메모리의 값이 일치하지 않는 문제
  * Java의 동시성 이슈 해결 방법
    * 암시적 Lock 사용 : Synchronized를 사용해 하나의 스레드에서만 자원을 사용하게 잠금
    * 명시적 Lock 사용 : Lock 인터페이스를 사용해 하나의 스레드에서만 자원을 사용하게 잠금
    * 불변 객체 사용 : 애초에 값이 변경될 일 없는 String, Wrapper class, final 키워드 등을 활용한다.
    
<br>

[#REFERENCE, Java 멀티 스레드 환경에서의 동시성 문제와 대책](https://everydayyy.tistory.com/154) 

</details>

<br>

<details>
<summary><strong>Q. JDK와 JRE의 차이점에 대해 설명해주세요. </strong></summary>

* **JDK는 Java Development Kit의 약자로, 개발자가 Java를 활용하여 개발하는데 사용하는 도구이며 이는 JRE를 포함하고 있다.**
* **JRE는 Java Runtime Environment의 약자로, 단순 Java 프로그램을 실행시키기 위한 환경과 도구이다.**
* **따라서, JDK는 Java 개발 목적의 도구이며, JRE는 Java 실행 환경 목적의 도구라는 차이점이 있다.**
  * 따라서, 일반적으로 개발 환경에서는 JDK를 설치하며, 운영 환경에서는 JRE를 설치한다.

</details>
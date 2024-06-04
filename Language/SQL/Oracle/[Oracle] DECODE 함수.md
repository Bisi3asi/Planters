# [Oracle] DECODE 함수
> **목차**
> 1. [DECODE 함수란?](#1)
> 2. [DECODE 함수 사용 방법과 예제](#2)
> 3. [DECODE 다중 조건](#3)

<br/>

### 1. DECODE 함수란? <a id="1"></a>
> **DECODE 함수는 조건문을 통해 컬럼에 대한 값 변환 시 사용한다.** 
> * 프로그래밍 언어의 IF ELSE, IF THEN 과 같은 기능을 수행한다.
> * CASE WHEN 과 달리 SQL의 표준 스펙이 아니며, ORACLE 단독의 함수이다.

<br/>

### 2. DECODE 함수 사용 방법과 예제 <a id="2"></a>

<br/>

**[기본식]**

1. `DECODE(Column A, Value B, Output AB, Output N)` : Column A = B 일 시 AB를, A ≠ B 일 시 N을 출력한다.
2. `DECODE(Column A, Value B, Output `
# [Oracle] DECODE 함수
> **목차**
> 1. [DECODE 함수란?](#1)
> 2. [DECODE 함수 사용 방법과 예제](#2)
> 3. [DECODE 다중 조건과 예제](#3)
> 4. [DECODE 와 CASE WHEN THEN 의 차이](#4)

<br/>

### 1. DECODE 함수란? <a id="1"></a>
> **DECODE 함수는 조건문을 통해 컬럼에 대한 값 변환 시 사용한다.** 
> * 프로그래밍 언어의 IF ELSE, IF THEN 과 같은 기능을 수행한다.

<br/>

### 2. DECODE 함수 사용 방법과 예제 <a id="2"></a>

<br/>

**[기본식]**

1. `DECODE(Column A, Value B, Output AB, DEFAULT_VALUE)` : Column A = B 일 시 AB를, A ≠ B 일 시 DEFAULT_VALUE를 반환한다.
    * DEFAULT_VALUE는 생략할 수 있으며, 생략 시 null을 반환한다.
2. `DECODE([Column A, Value B, Output AB, Value C, Output AC, ... ], DEFAULT_VALUE)` : Column A = B 일 시 AB를, A = C일 시 AC를, 그 외의 값일 시 DEFAULT_VALUE를 출력한다.
    * 위와 같이 특정 컬럼에 대응하는 여러 값에 따른 조건 분기와 반환값을 작성할 수 있다.

<br/>

**[사용 예제]**

<br/>

1. 예제 데이터 생성
```oracle
INSERT INTO EMPLOYEES (EMPLOYEE_ID, NAME, DEPARTMENT_ID, SALARY) VALUES (1, 'John Doe', 10, 5000);
INSERT INTO EMPLOYEES (EMPLOYEE_ID, NAME, DEPARTMENT_ID, SALARY) VALUES (2, 'Jane Smith', 20, 6000);
INSERT INTO EMPLOYEES (EMPLOYEE_ID, NAME, DEPARTMENT_ID, SALARY) VALUES (3, 'Robert Brown', 10, 5500);
INSERT INTO EMPLOYEES (EMPLOYEE_ID, NAME, DEPARTMENT_ID, SALARY) VALUES (4, 'Emily Davis', 30, 7000);
INSERT INTO EMPLOYEES (EMPLOYEE_ID, NAME, DEPARTMENT_ID, SALARY) VALUES (5, 'Michael Wilson', 20, 6500);
```

<br/>

2. DECODE 함수 활용 질의
> `DECODE` 함수를 활용하여 부서 ID에 따라 부서명을 반환하는 쿼리를 작성한다. <br/>
```oracle
--- 이하의 쿼리는 부서 ID에 따라 DECODE로 분기해 각각의 부서명으로 변환하여 표시한다. 
SELECT
    EMPLOYEE_ID,
    NAME,
    DECODE(DEPARTMENT_ID, 
           10, 'Sales', 
           20, 'Marketing', 
           30, 'IT', 
           'Unknown') AS DEPARTMENT_NAME,
    SALARY
FROM EMPLOYEES;
```

<br/>

3. 질의 결과

| EMPLOYEE_ID | NAME          | DEPARTMENT_NAME | SALARY |
|-------------|---------------|-----------------|--------|
| 1           | John Doe      | Sales           | 5000   |
| 2           | Jane Smith    | Marketing       | 6000   |
| 3           | Robert Brown  | Sales           | 5500   |
| 4           | Emily Davis   | IT              | 7000   |
| 5           | Michael Wilson| Marketing       | 6500   |

<br/>

### 3. DECODE 다중 조건과 예제 <a id="3"></a>
> DECODE는 DECODE문 내부에 중첩하여 다중 조건을 사용할 수 있다.

**[사용 예제]**

<br/>

1. 예제 데이터 생성(위와 동일)
```oracle
INSERT INTO EMPLOYEES (EMPLOYEE_ID, NAME, DEPARTMENT_ID, SALARY) VALUES (1, 'John Doe', 10, 5000);
INSERT INTO EMPLOYEES (EMPLOYEE_ID, NAME, DEPARTMENT_ID, SALARY) VALUES (2, 'Jane Smith', 20, 6000);
INSERT INTO EMPLOYEES (EMPLOYEE_ID, NAME, DEPARTMENT_ID, SALARY) VALUES (3, 'Robert Brown', 10, 5500);
INSERT INTO EMPLOYEES (EMPLOYEE_ID, NAME, DEPARTMENT_ID, SALARY) VALUES (4, 'Emily Davis', 30, 7000);
INSERT INTO EMPLOYEES (EMPLOYEE_ID, NAME, DEPARTMENT_ID, SALARY) VALUES (5, 'Michael Wilson', 20, 6500);
```

<br/>

2. DECODE 함수 활용 질의
> `DECODE` 함수를 활용하여 부서 ID와 그 내부의 SALARY에 따라 BONUS를 다중 조건으로 반환하는 쿼리를 작성한다. <br/>
```oracle
SELECT
    NAME,
    DEPARTMENT_ID,
    SALARY,
    DECODE(DEPARTMENT_ID,
           10, DECODE(SALARY, -- 부서 ID가 10인 경우
                      5000, 500, -- SALARY(판매액)이 5000인 경우 500
                      5500, 550, -- SALARY(판매액)이 5500인 경우 550
                      0),  
           20, DECODE(SALARY, -- 부서 ID가 20인 경우
                      6000, 600, -- SALARY(판매액)이 6000인 경우 600
                      6500, 650, -- SALARY(판매액)이 6500인 경우 650
                      0),  
           30, DECODE(SALARY, -- 부서 ID가 30인 경우
                      7000, 700, -- SALARY(판매액)이 7000인 경우 700
                      0),  
           0) AS BONUS -- 그 외의 경우
FROM EMPLOYEES;
```

<br/>

3. 질의 결과

| NAME           | DEPARTMENT_ID | SALARY | BONUS |
|----------------|---------------|--------|-------|
| John Doe       | 10    | 5000  | 500   |
| Jane Smith     | 20    | 6000  | 600   |
| Robert Brown   | 10    | 5500  | 550   |
| Emily Davis    | 30    | 7000  | 700   |
| Michael Wilson | 20    | 6500  | 650   |

<br/>

### 4. DECODE 와 CASE WHEN THEN 의 차이<a id="4"></a>
* `DECODE` 함수는 등가 조건만 가능하다. 
    * 즉, A 칼럼의 값이 B Value 이거나, C Value 이거나, 이러한 조건을 모두 만족하지 않는 등에 대한 분기만 설정이 가능하다.
    * 반면, `CASE WHEN THEN`은 범위 조건에 대한 분기 설정이 가능하다.
* `DECODE` 는 ORACLE에서만 지원하는 스펙이다.
    * SQL에 보다 범용적으로 사용되는 동일 기능의 분기문은 `CASE WHEN THEN` 이다.
* 그럼에도 불구하고 DECODE를 쓰는 이유 : **단일 조건 / 간략한 다중 조건에 대해 CASE WHEN THEN보다 구현이 간편하다.**
    * `CASE WHEN A = B THEN 1` 은 `DECODE (A, B, 1)` 과 동일한 기능을 한다.   
# [Oracle] 재귀형 쿼리(Recursive Query)
> **목차**
> 1. [재귀형 쿼리의 정의](#1)
> 2. [재귀형 쿼리의 원리](#2)
> 3. [재귀형 쿼리 사용 예제](#3)
> 4. [재귀형 쿼리의 용도](#4)

<br/>

### 1. 재귀형 쿼리(Recursive Query)의 정의 <a id="1"></a>
> **재귀형 쿼리(Recursive Query)는 SQL 문장에서 자기 자신을 반복적으로 호출하여 계층 구조 데이터를 탐색할 때 사용한다.**
> * 한 행(Row)이 다른 행의 부모나 자식이 되는 구조에서 루트부터 하위 노드까지, 혹은 하위에서 루트까지 관계를 따라가며 결과를 생성한다.

---

### 2. 재귀형 쿼리(Recursive Query)의 원리 <a id="2"></a>
재귀형 쿼리는 **Anchor 쿼리**와 **Recursive 쿼리**로 구성된다.

1. **Anchor Member**
    - 최초(루트) 데이터를 조회한다.
    - 한 번만 실행된다.

2. **Recursive Member**
    - Anchor 결과를 기반으로 자식 데이터를 찾는다.
    - 재귀적으로 반복 실행되며, 더 이상 결과가 나오지 않을 때 종료된다.

3. **UNION ALL**
    - Anchor와 Recursive 결과를 합쳐 전체 계층 데이터를 완성한다.

실행 순서는 다음과 같다:
`Anchor → Recursive(1차) → Recursive(2차) → ... → Recursive(n차)`

Oracle은 이 반복 과정을 내부적으로 수행하며, 결과를 하나의 계층 구조로 반환한다.

---

### 3. 재귀형 쿼리(Recursive Query) 사용 예제 <a id="3"></a>

예제는 Oracle을 기준으로 작성하였다.

#### 1) 테스트 데이터 생성
```oracle
CREATE TABLE employee (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    manager_id NUMBER
);

INSERT INTO employee VALUES (1, 'CEO', NULL);
INSERT INTO employee VALUES (2, 'Director A', 1);
INSERT INTO employee VALUES (3, 'Director B', 1);
INSERT INTO employee VALUES (4, 'Manager A1', 2);
INSERT INTO employee VALUES (5, 'Manager A2', 2);
INSERT INTO employee VALUES (6, 'Manager B1', 3);
INSERT INTO employee VALUES (7, 'Staff A2-1', 5);
COMMIT;
```

#### 2) 조직도 트리 구조 조회
```oracle
WITH RECURSIVE org_tree (emp_id, emp_name, manager_id, level_no) AS (
    -- Anchor: 최상위 관리자
    SELECT emp_id, emp_name, manager_id, 1 AS level_no
    FROM employee
    WHERE manager_id IS NULL

    UNION ALL

    -- Recursive: 하위 직원 탐색
    SELECT e.emp_id, e.emp_name, e.manager_id, o.level_no + 1
    FROM employee e
    JOIN org_tree o ON e.manager_id = o.emp_id
)
SELECT LPAD(' ', (level_no - 1) * 4) || emp_name AS hierarchy, level_no
FROM org_tree
ORDER BY level_no, emp_id;
```

**실행 결과**

| HIERARCHY       | LEVEL_NO |
|-----------------|-----------|
| CEO             | 1 |
|     Director A  | 2 |
|         Manager A1 | 3 |
|         Manager A2 | 3 |
|             Staff A2-1 | 4 |
|     Director B  | 2 |
|         Manager B1 | 3 |

#### 3) 특정 직원 상위 계층 조회
```oracle
WITH RECURSIVE upper_chain (emp_id, emp_name, manager_id) AS (
    SELECT emp_id, emp_name, manager_id
    FROM employee
    WHERE emp_name = 'Staff A2-1'

    UNION ALL

    SELECT e.emp_id, e.emp_name, e.manager_id
    FROM employee e
    JOIN upper_chain u ON e.emp_id = u.manager_id
)
SELECT * FROM upper_chain;
```

**실행 결과**

| EMP_ID | EMP_NAME   | MANAGER_ID |
|--------:|------------|------------|
| 7       | Staff A2-1 | 5 |
| 5       | Manager A2 | 2 |
| 2       | Director A | 1 |
| 1       | CEO        | NULL |

### 4. 재귀형 쿼리(Recursive Query)의 용도 <a id="4"></a>
재귀형 쿼리는 단순한 데이터 조회를 넘어 다양한 계층 구조 상황에서 활용된다.

| 활용 분야 | 설명 |
|------------|------|
| 조직 구조 | 직원-관리자, 부서-상위 부서 등 상하 관계 탐색 |
| 상품 카테고리 | 대분류 → 중분류 → 소분류 구조 조회 |
| BOM (Bill of Materials) | 제품 구성 부품의 단계별 구조 탐색 |
| 그래프/경로 탐색 | 노드 간 연결 관계 추적 |
| 트리 구조 평탄화 | 계층 구조 데이터를 단일 테이블 형태로 변환 |

재귀형 쿼리를 활용하면, 별도의 프로그래밍 반복문 없이 SQL만으로 계층 탐색을 구현할 수 있다.

**Source:**
- [Oracle 공식 문서: Hierarchical Queries with CONNECT BY](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/Hierarchical-Queries.html)
- [Oracle Database SQL Language Reference - Recursive Subquery Factoring (WITH RECURSIVE)](https://docs.oracle.com/en/database/oracle/oracle-database/21/sqlrf/SELECT.html#GUID-6F86E3C2-8E03-4C59-BE5A-1B40D1D78B7D)

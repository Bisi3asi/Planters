# [ORACLE, LV4] 서울에 위치한 식당 목록 출력하기

### 문제 설명

<p>다음은 식당의 정보를 담은 <code>REST_INFO</code> 테이블과 식당의 리뷰 정보를 담은  <code>REST_REVIEW</code> 테이블입니다. <code>REST_INFO</code> 테이블은 다음과 같으며 <code>REST_ID</code>, <code>REST_NAME</code>, <code>FOOD_TYPE</code>, <code>VIEWS</code>, <code>FAVORITES</code>, <code>PARKING_LOT</code>, <code>ADDRESS</code>, <code>TEL</code>은 식당 ID, 식당 이름, 음식 종류, 조회수, 즐겨찾기수, 주차장 유무, 주소, 전화번호를 의미합니다.</p>
<table class="table">
        <thead><tr>
<th>Column name</th>
<th>Type</th>
<th>Nullable</th>
</tr>
</thead>
        <tbody><tr>
<td>REST_ID</td>
<td>VARCHAR(5)</td>
<td>FALSE</td>
</tr>
<tr>
<td>REST_NAME</td>
<td>VARCHAR(50)</td>
<td>FALSE</td>
</tr>
<tr>
<td>FOOD_TYPE</td>
<td>VARCHAR(20)</td>
<td>TRUE</td>
</tr>
<tr>
<td>VIEWS</td>
<td>NUMBER</td>
<td>TRUE</td>
</tr>
<tr>
<td>FAVORITES</td>
<td>NUMBER</td>
<td>TRUE</td>
</tr>
<tr>
<td>PARKING_LOT</td>
<td>VARCHAR(1)</td>
<td>TRUE</td>
</tr>
<tr>
<td>ADDRESS</td>
<td>VARCHAR(100)</td>
<td>TRUE</td>
</tr>
<tr>
<td>TEL</td>
<td>VARCHAR(100)</td>
<td>TRUE</td>
</tr>
</tbody>
      </table>
<p><code>REST_REVIEW</code> 테이블은 다음과 같으며 <code>REVIEW_ID</code>, <code>REST_ID</code>, <code>MEMBER_ID</code>, <code>REVIEW_SCORE</code>, <code>REVIEW_TEXT</code>,<code>REVIEW_DATE</code>는 각각 리뷰 ID, 식당 ID, 회원 ID, 점수, 리뷰 텍스트, 리뷰 작성일을 의미합니다.</p>
<table class="table">
        <thead><tr>
<th>Column name</th>
<th>Type</th>
<th>Nullable</th>
</tr>
</thead>
        <tbody><tr>
<td>REVIEW_ID</td>
<td>VARCHAR(10)</td>
<td>FALSE</td>
</tr>
<tr>
<td>REST_ID</td>
<td>VARCHAR(10)</td>
<td>TRUE</td>
</tr>
<tr>
<td>MEMBER_ID</td>
<td>VARCHAR(100)</td>
<td>TRUE</td>
</tr>
<tr>
<td>REVIEW_SCORE</td>
<td>NUMBER</td>
<td>TRUE</td>
</tr>
<tr>
<td>REVIEW_TEXT</td>
<td>VARCHAR(1000)</td>
<td>TRUE</td>
</tr>
<tr>
<td>REVIEW_DATE</td>
<td>DATE</td>
<td>TRUE</td>
</tr>
</tbody>
      </table>
<hr>

<h5>문제</h5>

<p><code>REST_INFO</code>와 <code>REST_REVIEW</code> 테이블에서 서울에 위치한 식당들의 식당 ID, 식당 이름, 음식 종류, 즐겨찾기수, 주소, 리뷰 평균 점수를 조회하는 SQL문을 작성해주세요. 이때 리뷰 평균점수는 소수점 세 번째 자리에서 반올림 해주시고 결과는 평균점수를 기준으로 내림차순 정렬해주시고, 평균점수가 같다면 즐겨찾기수를 기준으로 내림차순 정렬해주세요. </p>

<hr>

<h5>예시</h5>

<p><code>REST_INFO</code> 테이블이 다음과 같고</p>
<table class="table">
        <thead><tr>
<th>REST_ID</th>
<th>REST_NAME</th>
<th>FOOD_TYPE</th>
<th>VIEWS</th>
<th>FAVORITES</th>
<th>PARKING_LOT</th>
<th>ADDRESS</th>
<th>TEL</th>
</tr>
</thead>
        <tbody><tr>
<td>00028</td>
<td>대우부대찌개</td>
<td>한식</td>
<td>52310</td>
<td>10</td>
<td>N</td>
<td>경기도 용인시 처인구 남사읍 처인성로 309</td>
<td>031-235-1235</td>
</tr>
<tr>
<td>00039</td>
<td>광주식당</td>
<td>한식</td>
<td>23001</td>
<td>20</td>
<td>N</td>
<td>경기도 부천시 산업로8번길 60</td>
<td>031-235-6423</td>
</tr>
<tr>
<td>00035</td>
<td>삼촌식당</td>
<td>일식</td>
<td>532123</td>
<td>80</td>
<td>N</td>
<td>서울특별시 강서구 가로공원로76가길</td>
<td>02-135-1266</td>
</tr>
</tbody>
      </table>
<p><code>REST_REVIEW</code> 테이블이 다음과 같을 때</p>
<table class="table">
        <thead><tr>
<th>REVIEW_ID</th>
<th>REST_ID</th>
<th>MEMBER_ID</th>
<th>REVIEW_SCORE</th>
<th>REVIEW_TEXT</th>
<th>REVIEW_DATE</th>
</tr>
</thead>
        <tbody><tr>
<td>R000000065</td>
<td>00028</td>
<td><code>soobin97@naver.com</code></td>
<td>5</td>
<td>부찌 국물에서 샤브샤브 맛이나고 깔끔</td>
<td>2022-04-12</td>
</tr>
<tr>
<td>R000000066</td>
<td>00039</td>
<td><code>yelin1130@gmail.com</code></td>
<td>5</td>
<td>김치찌개 최곱니다.</td>
<td>2022-02-12</td>
</tr>
<tr>
<td>R000000067</td>
<td>00028</td>
<td><code>yelin1130@gmail.com</code></td>
<td>5</td>
<td>햄이 많아서 좋아요</td>
<td>2022-02-22</td>
</tr>
<tr>
<td>R000000068</td>
<td>00035</td>
<td><code>ksyi0316@gmail.com</code></td>
<td>5</td>
<td>숙성회가 끝내줍니다.</td>
<td>2022-02-15</td>
</tr>
<tr>
<td>R000000069</td>
<td>00035</td>
<td><code>yoonsy95@naver.com</code></td>
<td>4</td>
<td>비린내가 전혀없어요.</td>
<td>2022-04-16</td>
</tr>
</tbody>
      </table>
<p>SQL을 실행하면 다음과 같이 출력되어야 합니다.</p>
<table class="table">
        <thead><tr>
<th>REST_ID</th>
<th>REST_NAME</th>
<th>FOOD_TYPE</th>
<th>FAVORITES</th>
<th>ADDRESS</th>
<th>SCORE</th>
</tr>
</thead>
        <tbody><tr>
<td>00035</td>
<td>삼촌식당</td>
<td>일식</td>
<td>80</td>
<td>서울특별시 강서구 가로공원로76가길</td>
<td>4.50</td>
</tr>
</tbody>
</table>

---

<br>

### 0. 문제 접근 방식

> 두 테이블에서 INNER JOIN을 통한 단순 `REST_INFO`의 `REST_ID`로 GROUP BY 연산이 제한된다. <br>
> <br> 따라서 이하 두 순서로 나눠 접근해야한다.
> 1. `REST_ID` 별 `REST_SCORE`의 평균 산출을 위한 별도 서브쿼리 생성 및 INNER JOIN
> 2. 서브쿼리 내부에서 요구사항에 맞게 반올림 처리

<br>

### 1. 서브쿼리, GROUP BY, INNER JOIN

1) `REST_REVIEW` 테이블의 `REST_ID` 와 `REVIEW_SCORE` 칼럼을 가진 별도 서브쿼리 테이블을 생성한다.
    ```ORACLE
        SELECT REST_ID, REVIEW_SCORE
        FROM REST_REVIEW
    ```

<br>

2) GROUP BY를 통해 `REST_ID` 별 평균 `REVIEW_SCORE`를 구한다.
    ```ORACLE
        SELECT REST_ID, AVG(REVIEW_SCORE) SCORE
        FROM REST_REVIEW
        GROUP BY REST_ID
    ```
   
    **[실행 결과]**
    ![1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FLESLG%2FbtsGFDWxkWU%2F97rfKvnm3O5rX8kgNfzufk%2Fimg.png)

<br>

3) `REST_ID` 테이블과 `REST_REVIEW` 테이블을 JOIN한다.
    * 문제에 맞는 WHERE 추가 및 SELECT 세팅
    ```ORACLE
   SELECT i.REST_ID, REST_NAME, FOOD_TYPE, FAVORITES, ADDRESS, SCORE
   FROM REST_INFO i
   INNER JOIN
   (
      SELECT REST_ID, AVG(REVIEW_SCORE) SCORE
        FROM REST_REVIEW
        GROUP BY REST_ID
   ) r
   ON i.REST_ID = r.REST_ID
   WHERE i.ADDRESS LIKE '서울%' # 서울시 위치 조건
   ORDER BY 6 DESC, 4 DESC # 정렬 조건
   ```

   **[실행 결과]**
   ![2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FKiHDk%2FbtsGEkcqllE%2FwPg3mKiDGUOmbFeBRLDwlk%2Fimg.png)

<br>

### 2. `ROUND`, 반올림 함수
>`ROUND(COLUMN, 반올림해 표기할 소수점 자리 수)` 를 활용해 반올림 정제한다.

1. 세번째 자리 수에서 반올림 해야하므로 두번째 자리 수까지 표기한다. 
<br> 그러므로 `ROUND`는 `ROUND(AVG(REVIEW_SCORE), 2)` 로 사용한다.
   ```oracle
   SELECT i.REST_ID, REST_NAME, FOOD_TYPE, FAVORITES, ADDRESS, SCORE
   FROM REST_INFO i
   INNER JOIN
   (
      SELECT REST_ID, ROUND(AVG(REVIEW_SCORE), 2) SCORE
      FROM REST_REVIEW
      GROUP BY REST_ID
   ) r
   ON i.REST_ID = r.REST_ID
   WHERE i.ADDRESS LIKE '서울%'
   ORDER BY 6 DESC, 4 DESC
   ```
   
    **[실행 결과 / 정답]**
   ![3](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FqTNZQ%2FbtsGC4gK2ms%2FUJr5ZQDMCoAK76OCMyK4p1%2Fimg.png)

<br>

```agsl
** 🤔 주의 **
이하부터는 작성자의 재량으로 문제 요구사항에 더욱 적합한 풀이를 진행했음
해당 사이트 상 정답은 상기 코드이므로 하단의 내용은 참고만 할 것
```

<br>

### 3. 한술 더 떠서, 소수점 형식을 고정하는 `TO_CHAR, FM`
> 근데, 문제에서 풀이 예시에는 소수점 형식이 반올림된 두자리 수로 고정되어 있다.
> <br> 웹사이트 상에서는 `ROUND` 만 작성해도 답이 인정되지만,
> <br> 보다 요구사항에 근접한 실행 결과를 위해 이하 사항을 적용해보았음.

<br>

1. `TO_CHAR(COLUMN, 'FM0.00')`은 해당 수의 표기 형식을 `0.00` 과 같은 방식으로 고정한다.
   * `FM`은 `FORMAT MODEL`의 약자로, 말 그대로 숫자 포맷을 지정하겠다는 것이다.
   * `FM9.99` 에서 9는 해당 자리의 숫자를 의미하고 값이 없을 경우 소수점 이상은 공백으로, 이하는 0으로 표시한다.
   * `FM0.00` 에서 0은 해당 자리의 숫자를 의미하고 값이 없을 경우 0으로 표시해 숫자의 길이를 고정적으로 표기한다.
   * ex1) `TO_CHAR(0.40, 'FM9.999')` = `.4` 
     * 9는 가변적인 값으로 0이거나 숫자가 존재하지 않을 시 값을 버린다.
   * ex2) `TO_CHAR(0.40, 'FM0.000')` = `0.400`
     * 0은 해당 값의 길이가 고정적으로 변환되며 숫자가 없을 시 0으로 채운다.

<br>

2. `TO_CHAR(COLUMN, 'FM0.00')` 활용 표기 형식 변환
   ```oracle
   SELECT i.REST_ID, REST_NAME, FOOD_TYPE, FAVORITES, ADDRESS, TO_CHAR(SCORE, '0.00') SCORE
   FROM REST_INFO i
   INNER JOIN
   (
      SELECT REST_ID, ROUND(AVG(REVIEW_SCORE), 2) SCORE
      FROM REST_REVIEW
      GROUP BY REST_ID
   ) r
   ON i.REST_ID = r.REST_ID
   WHERE i.ADDRESS LIKE '서울%'
   ORDER BY 6 DESC, 4 DESC
   ```
   
   **[실행 결과]**
   ![4](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FLq1BI%2FbtsGDKh1Agp%2FpOqRyCpRoUF5cSGPZQIIVk%2Fimg.png)
   
<br>

3. `LEFT JOIN`으로 변경
   > 서울의 모든 식당을 표기하는 게 요구사항이라면<br>
   > 리뷰가 없는 식당도 출력해야 하는 것이 아닐까? 🤔
   ```oracle
   SELECT i.REST_ID, REST_NAME, FOOD_TYPE, FAVORITES, ADDRESS, TO_CHAR(SCORE, '0.00') SCORE
   FROM REST_INFO i
   LEFT JOIN # 변경
   (
      SELECT REST_ID, ROUND(AVG(REVIEW_SCORE), 2) SCORE
      FROM REST_REVIEW
      GROUP BY REST_ID
   ) r
   ON i.REST_ID = r.REST_ID
   WHERE i.ADDRESS LIKE '서울%'
   ORDER BY 6 DESC, 4 DESC
   ```

   **[실행 결과]**
   ![5](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FIXvs5%2FbtsGGnePZfg%2F9gNxz5ND38vWzowgF3vppk%2Fimg.png)

<br>

4. `NVL` 함수로 NULL 값 0 대체
   > `NVL(COLUMN, 지정 값`)은 해당 COLUMN의 값이 NULL일 때 지정한 값으로 변환한다.
   ```oracle
   SELECT i.REST_ID, REST_NAME, FOOD_TYPE, FAVORITES, ADDRESS, 
          TO_CHAR(NVL(SCORE, 0), '0.00') SCORE
   FROM REST_INFO i
   LEFT JOIN # 변경
   (
      SELECT REST_ID, ROUND(AVG(REVIEW_SCORE), 2) SCORE
      FROM REST_REVIEW
      GROUP BY REST_ID
   ) r
   ON i.REST_ID = r.REST_ID
   WHERE i.ADDRESS LIKE '서울%'
   ORDER BY 6 DESC, 4 DESC
   ```

   **[실행 결과]**
   ![6](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FqgMWq%2FbtsGEWI4VzE%2F5MAMyeJmgWLzGFyzMqWhG0%2Fimg.png)

<br>

```agsl
이정도는 되야 올바른 답 아닐까? 🤔
```

<br>

### REFERENCE
https://school.programmers.co.kr/learn/courses/30/lessons/131118
# SQL_BASIC 2주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_2nd_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**2주차 과제**는 1주차 과제처럼 SQL의 필요성이나 느낀점 위주가 아닌, **실제 강의 내용을 바탕으로 개념을 정리하고 학습한 내용을 집중적으로 기록**해주세요. 완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요. 

**👀(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_2nd

### 섹션 3. 데이터 탐색 - 조건, 추출, 요약

### 2-3. 데이터 탐색 (SELECT, FROM, WHERE)

### 2-4. SELECT 연습문제

### 2-5. 집계 (Group By + Having + Sum/Count)



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | 🍽️         |
| 4주차 | 섹션 **3-4** ~ **4-4** | 🍽️         |
| 5주차 | 섹션 **4-4** ~ **4-9** | 🍽️         |
| 6주차 | 섹션 **5-1** ~ **5-7** | 🍽️         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 개념정리 

## 2-3. 데이터 탐색 (SELECT, FROM, WHERE)

~~~
✅ 학습 목표 :
* SQL 쿼리 구조를 이해할 수 있다. 
* SELECT, FROM, WHERE의 핵심 문법을 설명할 수 있다. 
~~~

📌 SQL 쿼리란?
데이터베이스에 저장된 데이터를 조회·추출하기 위한 명령문이다.
데이터는 테이블(Table) 형태로 저장되며, Row(행) 와 Column(열) 으로 구성된다.

예) 포켓몬 테이블: 이름, 타입, 공격력, 특수 공격력 등의 컬럼을 가진 테이블

📌 SQL 쿼리 기본 구조
sqlSELECT
  컬럼1,
  컬럼2,
  컬럼3
FROM 테이블
WHERE
  <조건문>

⚠️ 순서를 반드시 지켜야 한다: SELECT → FROM → WHERE
SELECT → WHERE → FROM 순서로 작성하면 실행 불가!

📌 각 키워드 설명
키워드설명FROM데이터를 확인할 테이블 명시 / 이름이 길면 AS "별칭"으로 별칭 지정 가능 (FROM Table1 AS t1)WHEREFROM에 명시된 테이블의 데이터를 필터링(조건 설정) / 테이블에 있는 컬럼을 조건으로 설정SELECT테이블에 저장된 컬럼 선택 / 여러 컬럼 명시 가능 / col1 AS "별칭"으로 컬럼 이름도 별칭 지정 가능

📌 실습 예시
불 타입 포켓몬을 모두 조회하는 쿼리
~~~sql
SELECT
  *
FROM basic.pokemon
WHERE
  type1 = "Fire"
~~~

* : 모든 컬럼을 출력하겠다는 의미
* EXCEPT(제외할 컬럼) 형태로 특정 컬럼만 제외하고 출력하는 것도 가능

FROM 절의 테이블 경로 구조 (BigQuery 기준)
프로젝트ID.데이터셋.테이블명
예) inflearn-bigquery.basic.pokemon

같은 프로젝트 내에서 쿼리할 때는 프로젝트 ID 생략 가능
여러 프로젝트를 사용할 경우 프로젝트 ID를 반드시 명시해야 함


📌 집합처럼 생각하기

하나의 테이블에서 데이터를 추출할 때: SELECT col FROM Table A
여러 테이블에 데이터가 분산된 경우: 각 테이블에서 추출 후 JOIN으로 연결


## 2-5. 집계 (Group By / HAVING / SUM,COUNT)

~~~
✅ 학습 목표 :
* 데이터를 집계하고 그룹화하는 방법을 설명할 수 있다.
* GROUP BY, HAVING, ORDER BY, 집계함수(SUM/COUNT 등)을 활용하는 방법을 설명할 수 있다.
* having과 where의 차이에 대해서 설명할 수 있다.
~~~

📌 집계(Aggregation)란?
모아서 계산하다 (= 그룹화해서 계산하다)

집계에서 할 수 있는 계산의 종류:

더하기, 빼기
최대값, 최소값
평균
갯수 세기

📌 GROUP BY

같은 값끼리 모아서 그룹화한다


특정 컬럼을 기준으로 데이터를 묶고, 다른 컬럼에서 집계(합, 평균, MAX, MIN 등)를 수행할 수 있다.
집계할 컬럼을 SELECT에 명시하고, 그 컬럼을 꼭 GROUP BY에도 작성해야 한다.

~~~sql
SELECT
  집계할_컬럼1,
  집계함수(COUNT, MAX, MIN, AVG, SUM 등)
FROM Table
GROUP BY
  집계할_컬럼1
~~~

예시: 포켓몬 타입별 평균 공격력과 포켓몬 수 조회
~~~sql
SELECT
  type1,
  AVG(attack) AS avg_attack,
  COUNT(id) AS cnt
FROM basic.pokemon
GROUP BY
  type1
~~~

📌 집계 함수 종류
자주 쓰는 집계 함수:
함수설명COUNT(컬럼)행의 수를 센다 (NULL 제외)SUM(컬럼)합계를 구한다AVG(컬럼)평균을 구한다MAX(컬럼)최대값을 구한다MIN(컬럼)최소값을 구한다COUNT(DISTINCT 컬럼)중복을 제거한 고유한 값의 수를 센다

전체 집계 함수 목록: BigQuery 공식 문서


📌 DISTINCT : 고유값 추출

중복을 제거하고 고유한(Unique) 값만 보고 싶을 때 사용


예: [1, 2, 3, 3, 4] → DISTINCT 적용 → [1, 2, 3, 4]

~~~sql
SELECT
  집계할_컬럼,
  COUNT(DISTINCT count할_컬럼)
FROM table
GROUP BY
  집계할_컬럼
~~~

COUNT vs COUNT(DISTINCT) 차이 예시:

메인 페이지 VIEW 수(전체 이벤트 수) → COUNT(user_id) = 4번
메인 페이지를 VIEW 한 유저 수(중복 제거) → COUNT(DISTINCT user_id) = 3명


📌 ORDER BY : 정렬하기

결과를 특정 컬럼 기준으로 정렬할 때 사용

~~~sql
SELECT
  col
FROM 테이블
ORDER BY <컬럼> <순서>

DESC : 내림차순 (큰 것 → 작은 것)
ASC : 오름차순 (기본값, Default)
ORDER BY는 쿼리의 맨 마지막에 작성한다
~~~

예시: 평균 공격력이 높은 타입 순서로 정렬
~~~sql
SELECT
  type1,
  AVG(attack) AS avg_attack,
  COUNT(id) AS cnt
FROM basic.pokemon
GROUP BY type1
ORDER BY avg_attack DESC
~~~

📌 LIMIT : 출력 개수 제한

쿼리 결과의 Row 수를 제한하고 싶은 경우 사용


쿼리문의 제일 마지막에 작성한다.

~~~sql
SELECT
  col
FROM Table
ORDER BY col DESC
LIMIT 10
~~~

📌 WHERE vs HAVING : 조건 설정의 차이
구분사용 시점대상WHEREGROUP BY 이전Raw 테이블 데이터에 직접 조건 설정HAVINGGROUP BY 이후그룹화된 결과에 조건 설정
~~~sql
SELECT
  컬럼1, 컬럼2,
  COUNT(컬럼1) AS col1_count
FROM <table>
WHERE
  컬럼1 >= 3          -- Raw 데이터 필터링 (GROUP BY 전)
GROUP BY 컬럼1, 컬럼2
HAVING
  col1_count > 3      -- 집계 결과 필터링 (GROUP BY 후)
~~~

💡 핵심 차이: WHERE는 원본 테이블 행에 조건을 걸고, HAVING은 GROUP BY로 집계된 결과에 조건을 건다.


📌 서브 쿼리

SELECT 문 안에 존재하는 또 다른 SELECT 쿼리


FROM 절에 또 다른 SELECT 문을 넣을 수 있다.
괄호로 묶어서 사용한다.
서브 쿼리를 작성하고, 바깥에서 WHERE 조건을 설정하는 것 = 서브 쿼리에서 HAVING으로 하는 것과 동일한 효과


📌 그룹화 활용 포인트
실무에서 GROUP BY를 사용하는 대표적인 경우:

일자별 집계 : 특정 시간에 유저가 한 행동을 일자별로 집계
연령대별 집계 : 특정 연령대에서 더 많이 구매했는가?
특정 타입별 집계 : 특정 제품 타입을 많이 구매했는가?
앱 화면별 집계 : 어떤 화면에 유저가 많이 접근했는가?


# 2️⃣ 학습 인증란

![SQL_week2](images/SQL_week2(1).png)



<br><br>



---

# 3️⃣ 확인문제

## 문제 1

> **🧚Q. 포켓몬 마스터 진아는 포켓몬 데이터 조회하는 SQL문에 재미를 느껴서 혼자서 데이터를 조회하는 쿼리문을 짰습니다. 하지만 세 가지의 오류로 다음 코드가 실행이 안된다고 하는데, 각 오류의 위치와 이유를 설명하고, 올바른 쿼리문으로 수정해보세요.**

~~~sql
# 진아의 SQL Query문 
SELECT name. type
FROM pokemon;
WHERE type = Electric;
~~~



~~~
[오류 1] SELECT name. type
→ name 뒤의 마침표(.)를 쉼표(,)로 바꿔야 한다.
  여러 컬럼을 선택할 때는 쉼표(,)로 구분해야 한다.

[오류 2] FROM pokemon;
→ FROM 절 뒤에 세미콜론(;)이 있으면 쿼리가 거기서 끝나버린다.
  세미콜론은 쿼리의 맨 마지막에만 붙여야 한다. (또는 생략)

[오류 3] WHERE type = Electric;
→ Electric은 문자열(string)이므로 따옴표로 감싸야 한다.
  WHERE type = "Electric" 으로 수정해야 한다.

✅ 올바른 쿼리:
SELECT name, type
FROM pokemon
WHERE type = "Electric";
~~~



## 문제 2

> **🧚Q. 앞서 SQL Query의 오류를 해결한 진아는 기분 좋게 이번에는 포켓몬 데이터에서 타입별 평균 공격력이 60 이상인 타입만 조회하려는 쿼리를 작성하려고 했습니다. 하지만 이번에도 실수를 하여 쿼리문이 실행되지 않거나 잘못된 결과가 나오고 있는데, 쿼리에서 잘못된 부분이 무엇인지 설명하고, 올바르게 수정한 쿼리를 작성해보세요.**

~~~sql
SELECT type, AVG(attack) AS avg_attack
FROM pokemon
WHERE AVG(attack) >= 60
GROUP BY type;
~~~



~~~
[오류] WHERE AVG(attack) >= 60
→ WHERE 절에는 집계 함수(AVG, COUNT, SUM 등)를 사용할 수 없다.
  WHERE는 GROUP BY 이전에 실행되기 때문에, 아직 집계가 이루어지지 않은 상태이다.
  GROUP BY로 집계한 결과에 조건을 걸고 싶을 때는 HAVING을 사용해야 한다.

✅ 올바른 쿼리:
SELECT type, AVG(attack) AS avg_attack
FROM pokemon
GROUP BY type
HAVING AVG(attack) >= 60;
~~~



### 🎉 수고하셨습니다.

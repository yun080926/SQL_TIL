# SQL_BASIC 6주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_6th_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**6주차 과제는 강의 내용을 정리하는 것과 함께, 프로그래머스에서 제공하는 SQL 문제를 직접 풀어보는 실습도 병행합니다.** 강의에서는 **배운 내용을 정리하고 주요 쿼리 예제를 정리**하며, 프로그래머스 문제는 **직접 풀어본 뒤 풀이 과정과 결과, 배운 점을 함께 기록**해주세요. 완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**👀(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_6th

### 섹션 6. 다량의 자료를 연결 : JOIN 

### 5-1. Intro

### 5-2. JOIN 이해하기

### 5-3. 다양한 JOIN 방법

### 5-4. JOIN 쿼리 작성하기 

### 5-5. JOIN을 처음 공부할 때 헷갈렸던 부분

### 5-6. JOIN 연습문제 1~2번

### 5-6. JOIN 연습문제 3~5번

### 5-7. 정리



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | ✅         |
| 5주차 | 섹션 **4-4** ~ **4-9** | ✅         |
| 6주차 | 섹션 **5-1** ~ **5-7** | ✅         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<!-- 여기까진 그대로 둬 주세요-->

<br>

---

# 1️⃣ 개념정리

## 5-2. JOIN 이해하기

~~~
✅ 학습 목표 :
* JOIN에 대한 정의와 필요성에 대해 설명할 수 있다.
~~~

### JOIN이란?
- **서로 다른 데이터 테이블을 연결하는 것**
- **공통적으로 존재하는 컬럼(= Key)** 이 있다면 JOIN 가능
- 보통 id 값을 Key로 많이 사용하고, 특정 범위(예: Date)로 JOIN도 가능

### JOIN이 어려운 이유?
- JOIN 자체가 어려운 것이 아니라, **테이블 구조에 익숙하지 않아서** 어렵게 느끼는 것
- 많은 문제 풀이를 통한 체화가 핵심!

### JOIN을 해야 하는 이유 - 데이터 저장 형태 이해
- 관계형 데이터베이스(RDBMS) 설계 시 **정규화(Normalization)** 과정을 거침
- 정규화는 **중복을 최소화하게 데이터를 구조화**하는 것
  - User Table은 유저 데이터만, Order Table은 주문 데이터만 저장
  - 데이터를 다양한 Table에 나눠 저장 → **필요할 때 JOIN해서 사용**
- 데이터 분석 관점에서는 미리 JOIN되어 있는 것이 편리할 수 있으나, 개발 관점에서는 분리되어 있는 것이 효율적
- 최근 트렌드: **데이터 웨어하우스**에서 JOIN + 필요한 연산 → **데이터 마트** 형태로 가공해서 활용

### 포켓몬 예시로 JOIN 이해하기

| 테이블 | 연결 Key | 설명 |
|---|---|---|
| trainer (트레이너) | id | 트레이너 정보 저장 |
| trainer_pokemon (포획 기록) | trainer_id, pokemon_id | 트레이너-포켓몬 연결 중간 테이블 |
| pokemon (포켓몬) | id | 포켓몬 정보 저장 |

> 트레이너가 어떤 포켓몬을 잡았는지 알려면 세 테이블을 모두 JOIN해야 함



## 5-3. 다양한 JOIN 방법

~~~
✅ 학습 목표 :
* JOIN 방법들의 종류를 설명할 수 있다. 
* 각 JOIN 방법들의 차이점에 대해서 설명할 수 있다. 
~~~

### JOIN의 종류

| JOIN 종류 | 설명 | ON 필수 여부 |
|---|---|---|
| **(INNER) JOIN** | 두 테이블의 **공통 요소(교집합)만** 연결 | O |
| **LEFT JOIN** | 왼쪽 테이블 기준, 오른쪽에서 매칭되는 것 붙임. 없으면 NULL | O |
| **RIGHT JOIN** | 오른쪽 테이블 기준, 왼쪽에서 매칭되는 것 붙임. 없으면 NULL | O |
| **FULL (OUTER) JOIN** | 양쪽 모두 포함. 없는 쪽은 NULL | O |
| **CROSS JOIN** | 두 테이블의 모든 요소를 곱함 (N × M 행 생성) | X |

### 집합 관점으로 이해하기

- **INNER JOIN** → 교집합만 (A ∩ B)
- **LEFT JOIN** → A 전체 + B 매칭분 (없으면 NULL)
- **RIGHT JOIN** → B 전체 + A 매칭분 (없으면 NULL)
- **FULL JOIN** → A + B 전체 합집합 (없는 쪽은 NULL)
- **CROSS JOIN** → 완전 곱집합 (데이터 폭발 주의 ⚠️)

### 처음 배울 때 팁
> 처음에 어렵다면 **LEFT JOIN만 주로 사용해도 충분!**  
> LEFT JOIN 후 특정 컬럼에 IS NULL 조건을 걸면 INNER JOIN과 동일한 효과도 낼 수 있음



## 5-4. JOIN 쿼리 작성하기 

~~~
✅ 학습 목표 :
* JOIN을 사용한 문법에 대해 이해하여 적용할 수 있다.
* JOIN을 활용한 쿼리를 작성할 수 있다. 
~~~

### SQL JOIN 쿼리 작성 흐름

```
1. 테이블 확인     → 테이블에 저장된 데이터, 컬럼 확인
2. 기준 테이블 정의 → 가장 많이 참고할 기준(base) 테이블 정의 (row 수 적은 것 기준 → LEFT에)
3. JOIN Key 찾기   → 여러 Table과 연결할 Key(ON) 정리
4. 결과 예상하기   → 결과 테이블을 손/엑셀로 미리 작성 (정답지 역할)
5. 쿼리 작성/검증  → 예상한 결과와 동일한 결과가 나오는지 확인
```

### 기본 문법

```sql
SELECT
  A.col1,
  A.col2,
  B.col1,
  B.col2
FROM table1 AS A
LEFT JOIN table2 AS B
ON A.key = B.key
```

- `FROM` 바로 아래에 `LEFT JOIN` → `ON` 순서로 작성
- 테이블 이름이 길 수 있으므로 **별칭(Alias)** 을 정의해줄 수 있음 (AS A, AS B)
- CROSS JOIN을 제외한 모든 JOIN은 **ON이 필수**

### JOIN별 쿼리 예시

```sql
-- INNER JOIN
SELECT col
FROM table_a AS A
INNER JOIN table_b AS B
ON A.key = B.key

-- LEFT JOIN
SELECT col
FROM table_a AS A
LEFT JOIN table_b AS B
ON A.key = B.key

-- FULL JOIN
SELECT col
FROM table_a AS A
FULL JOIN table_b AS B
ON A.key = B.key

-- CROSS JOIN (ON 없음)
SELECT col
FROM table_a AS A
CROSS JOIN table_b AS B
```

### 실전 예시: 포켓몬 3개 테이블 JOIN (BigQuery)

```sql
SELECT
  tp.*,
  t.* EXCEPT(id),   -- trainer_id가 tp에 있으니 t의 id는 중복 제거
  p.* EXCEPT(id)    -- pokemon_id가 tp에 있으니 p의 id는 중복 제거
FROM basic.trainer_pokemon AS tp
LEFT JOIN basic.trainer AS t
ON tp.trainer_id = t.id
LEFT JOIN basic.pokemon AS p
ON tp.pokemon_id = p.id
```

- JOIN을 연속으로 써서 **3개 이상의 테이블** 합치기 가능
- 중복 컬럼은 `EXCEPT(컬럼명)` 으로 제거
- 결과: 트레이너 포획 포켓몬 + 트레이너 정보 + 포켓몬 정보를 한 번에 확인 가능



## 5-5. JOIN을 처음 공부할 때 헷갈렸던 부분

### 1) 여러 JOIN 중 어떤 것을 사용해야 할까?

- **하려고 하는 작업의 목적**에 따라 JOIN을 선택
  - 교집합이 필요하다면 → **INNER JOIN**
  - 모두 조합이 필요하다면 → **CROSS JOIN**
  - 그 외 일반적인 경우 → **LEFT JOIN** 추천 (하나를 계속 활용하는 것을 권장)
- 쿼리 작성 전 결과를 예상하고, **중간 결과도 생각하면서** 찾아보기

### 2) 어떤 Table을 왼쪽에 두고, 어떤 Table이 오른쪽에 가야할까?

- **LEFT JOIN의 경우**: 기준이 되는 Table을 왼쪽에 두기
- 기준에는 **기준값이 존재**하고, 우측에 데이터를 계속 추가하는 방식
- 예시:
  - 주문한 유저의 정보를 알고 싶다 → Order(LEFT) + User(RIGHT)
  - 주문하지 않은 유저를 알고 싶다 → User(LEFT) + Order(RIGHT) → Order 컬럼 IS NULL 필터링

### 3) 여러 Table을 연결할 수 있는걸까?

- JOIN의 개수에 **한계는 없음**
- 단, 너무 많이 JOIN하고 있는지 확인 필요 → 실무에서는 **3~5개 정도**가 적당
- 더 많이 해야 하면 **중간 테이블을 만들어서** 쿼리를 나누는 것이 효율적

```sql
SELECT
  table_a.col1,
  table_b.col2,
  table_c.col3
FROM table_a
LEFT JOIN table_b
ON table_a.key = table_b.key
LEFT JOIN table_c
ON table_a.key = table_c.key
```

### 4) 컬럼은 모두 다 선택해야 할까?

- 컬럼 선택은 **데이터를 추출해서 무엇을 하고자 하는지**에 따라 다름
- JOIN이 잘 되었나 확인하기 위해 처음엔 많은 컬럼을 선택해도 괜찮으나,  
  **사용하지 않을 컬럼은 선택하지 않는 것**이 BigQuery에서 비용을 줄일 수 있음
- id 같은 값은 Unique한지 확인하기 위해 자주 사용하므로 id는 자주 가져가는 편

```sql
SELECT
  table_a.*,           -- a 테이블 전체
  EXCEPT (중복컬럼),   -- 중복 컬럼 제거
  table_b.*
FROM table_a
LEFT JOIN table_b
ON table_a.key = table_b.key
```

### 5) NULL이 대체 뭐죠?

- **NULL** = 값이 없음, 알 수 없음
- 0이나 공백('')과 다르게 **값이 아예 없는 것**
- JOIN에서는 **연결할 값이 없는 경우** NULL이 나타남
- LEFT JOIN을 할 때 조건에 맞는 값이 없으면 → 그 자리에 NULL이 들어감

> 💡 화장지 비유: `0 / empty string` = 남은 게 없는 것 / `NULL` = 원래부터 없는 것



## 5-6. JOIN 연습문제 1~5번 

~~~
✅ 학습 목표 :
* 연습문제(3문제 이상) 푼 것들 정리하기
~~~

### 연습문제 1. 트레이너가 보유한 포켓몬들은 얼마나 있는지 알 수 있는 쿼리를 작성해주세요

**문제 조건**
- 확인할 지표 : 포켓몬들이(이름 명시) 얼마나 있는지 알고 싶다! → 포켓몬의 수
- 쿼리 계산 방법 : trainer_pokemon(status가 Active, Training) + pokemon JOIN → GROUP BY 집계(COUNT)
- "보유했다"의 정의 : status가 Active, Training인 경우를 의미
- Released는 방출했다는 것을 의미

**사고 과정**
- 사용할 테이블 : trainer_pokemon, pokemon
- Join KEY : trainer_pokemon.pokemon_id = pokemon.id
- 1)을 먼저 하는 것이 좋을까? 혹은 JOIN을 하고 그 후에 Active, Training을 필터링하는 것이 좋을까?
  - 연산량 관점에서 **먼저 줄이고 JOIN**하는 것이 효율적
  - WHERE 조건을 안하고 JOIN을 한 후에 WHERE 조건을 건다고 하면 379 × Pokemon JOIN이 됨
- JOIN할 때 자주 나올 수 있는 에러 : `Column name id is ambiguous at` → id가 모호하다. 더 구체적으로(Specific하게) 말해달라 → 꼭 어떤 테이블의 컬럼인지 명시해야 함 → id => tp.id

**쿼리**

```sql
SELECT
  p.kor_name,
  COUNT(tp.id) AS pokemon_cnt
FROM (
  -- 서브쿼리로 먼저 필터링 (연산량 절감)
  SELECT
    id,
    trainer_id,
    pokemon_id,
    status
  FROM basic.trainer_pokemon
  WHERE
    status IN ("Active", "Training")
) AS tp
LEFT JOIN basic.pokemon AS p
ON tp.pokemon_id = p.id
GROUP BY
  p.kor_name
ORDER BY
  pokemon_cnt DESC
```

**핵심 포인트**
- 서브쿼리로 먼저 status 필터링 → Row 수 줄이고 → 그 후에 JOIN (연산 효율)
- 복잡하다! → 가독성 있는 쿼리 → **WITH 구문** 활용 가능

---

### 연습문제 2. 각 트레이너가 가진 포켓몬 중에서 'Grass' 타입의 포켓몬 수를 계산해주세요 (단, 편의를 위해 type1 기준으로 계산해주세요)

**문제 조건**
- 확인할 지표 : 트레이너가 보유한 포켓몬 중에서 Grass 타입의 포켓몬 수
- 쿼리 계산 방법 : 트레이너가 보유한 포켓몬 조건 설정 → Grass 타입으로 WHERE 조건을 걸어 COUNT
- 사용할 테이블 : trainer_pokemon, pokemon
- Join KEY : trainer_pokemon.pokemon_id = pokemon.id
- 데이터 특징 : 1번 문제와 동일

**기준 테이블 선택 고민**
- pokemon을 왼쪽에 두면 → pokemon 중에 보유되지 않았던 포켓몬은 trainer_pokemon에 없을 것 → NULL
- trainer_pokemon을 왼쪽에 두면 → 트레이너가 보유했던 포켓몬들을 기반으로 포켓몬 데이터만 추가. NULL이 추가되지 않음
- 기준이 되는 테이블은 **내가 구하고자 하는 데이터가 제일 잘 저장되어 있는가?** 로 판단

**쿼리**

```sql
SELECT
  tp.*,
  p.type1
FROM (
  SELECT
    id,
    trainer_id,
    pokemon_id,
    status
  FROM basic.trainer_pokemon
  WHERE
    status IN ("Active", "Training")
) AS tp
LEFT JOIN basic.pokemon AS p
ON tp.pokemon_id = p.id
WHERE
  type1 = "Grass"
GROUP BY
  type1
ORDER BY
  2 DESC  -- 2 대신에 pokemon_cnt도 가능
```

**핵심 포인트**
- 1번과 동일하게 서브쿼리로 먼저 status 필터링 후 JOIN
- WHERE 절에서 type1 = "Grass" 조건 추가
- `Table "trainer_pokemon" must be qualified with a dataset (e.g. dataset.table)` 에러 → basic 명시 안함 → **basic.trainer_pokemon** 으로 수정

---

### 연습문제 3. 트레이너의 고향(hometown)과 포켓몬을 포획한 위치(location)를 비교하여, 자신의 고향에서 포켓몬을 포획한 트레이너의 수를 계산해주세요

**문제 조건**
- 확인할 지표 : 트레이너 고향과 포켓몬의 포획 위치가 같은 트레이너의 수를 계산하기!
- 쿼리 계산 방법 : trainer(hometown), trainer_pokemon(location) JOIN → hometown = location → 트레이너의 수 COUNT
- 사용할 테이블 : trainer, trainer_pokemon
- Join KEY : trainer.id = trainer_pokemon.trainer_id
- 데이터 특징 : status 상관없이 구해주세요

**기준 테이블 선택 고민**
- trainer_pokemon을 LEFT로 할 것 같음 (실제로 풀다면)
- 하지만 trainer를 왼쪽에 두고 쿼리를 작성

**주의 사항 (JOIN 시 행 증가 현상)**
- trainer에서 특정 트레이너(예: Goh)의 정보는 1개 Row인데, LEFT JOIN하면 RIGHT에서 여러 개가 있을 때 데이터가 더 많아지는 것처럼 보임
- trainer_pokemon에 Goh가 6마리 포켓몬을 가지고 있어서 이렇게 결과가 합쳐진 것
- LEFT에 메타 정보를 두면 헷갈릴 수 있음

**쿼리**

```sql
SELECT
  COUNT(DISTINCT tp.trainer_id) AS trainer_uniq,
  -- COUNT(tp.trainer_id) AS trainer_cnt  # 트레이너와 포켓몬이 같은 건이 43개
FROM basic.trainer AS t
LEFT JOIN basic.trainer_pokemon AS tp
ON t.id = tp.trainer_id
WHERE
  tp.location IS NOT NULL
  AND t.hometown = tp.location
  -- trainer 중에 포켓몬을 찾아보지 못한 trainer가 있으면 NULL 조건을 걸어줘야 함
  -- 지금 데이터에는 trainer 테이블에 있는 trainer들은 모두 포켓몬을 보유
```

**핵심 포인트**
- `COUNT(DISTINCT tp.trainer_id)` → 고유한 트레이너 수 = 28명
- `COUNT(tp.trainer_id)` → 트레이너와 포켓몬 포획 위치가 같은 건수 = 43개
- `tp.location IS NOT NULL` → 포켓몬을 잡아보지 못한 트레이너 제외 목적 (NULL 방지)
- `t.hometown = tp.location` → 고향과 포획 위치가 같은 조건

<br>

<br>

---

# 2️⃣ 확인문제 & 문제 인증

## 프로그래머스 문제 

https://school.programmers.co.kr/learn/courses/30/lessons/131533

> 상품 별 오프라인 매출 구하기

```sql
SELECT
  P.PRODUCT_CODE,
  SUM(P.PRICE * O.SALES_AMOUNT) AS SALES
FROM PRODUCT AS P
INNER JOIN OFFLINE_SALE AS O
ON P.PRODUCT_ID = O.PRODUCT_ID
GROUP BY
  P.PRODUCT_CODE
ORDER BY
  SALES DESC,
  P.PRODUCT_CODE ASC
```

<img width="1385" height="786" alt="image" src="https://github.com/user-attachments/assets/4319ceab-76a3-4642-aedb-f77542a5de81" />


---

https://school.programmers.co.kr/learn/courses/30/lessons/133027

> 주문량이 많은 아이스크림들 조회하기

```sql
SELECT
  F.FLAVOR
FROM FIRST_HALF AS F
INNER JOIN (
  -- 7월 데이터를 맛 기준으로 먼저 합산 (중복 출하번호 처리)
  SELECT
    FLAVOR,
    SUM(TOTAL_ORDER) AS TOTAL_ORDER
  FROM JULY
  GROUP BY FLAVOR
) AS J
ON F.FLAVOR = J.FLAVOR
ORDER BY
  (F.TOTAL_ORDER + J.TOTAL_ORDER) DESC
LIMIT 3
```

<img width="1440" height="777" alt="image" src="https://github.com/user-attachments/assets/25576f12-9209-44be-a6ef-112463164ade" />


---

# 3️⃣ 참고자료

JOIN 에 대해서 그림으로 쉽게 이해할 수 있는 자료들도 있어서 첨부합니다. 아래의 블로그도 학습할 때 같이 참고해주세요.

1. https://data-marketing-bk.tistory.com/entry/SQL-JOIN-%ED%95%9C-%EB%B0%A9%EC%97%90-%EC%A0%95%EB%A6%AC-%EA%B0%9C%EB%85%90%EB%B6%80%ED%84%B0-%EC%BD%94%EB%93%9C%EA%B9%8C%EC%A7%80-%EC%9D%B4%EA%B2%83%EB%A7%8C-%EB%B3%B4%EC%9E%90

2. https://velog.io/@wijoonwu/JOIN

<br>

### 🎉 수고하셨습니다.

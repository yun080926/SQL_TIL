# SQL_BASIC 5주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_5th_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**5주차 과제는 문제 풀이를 중심으로**, 강의에서 제시된 예제 문제 중 **3 문제 이상을 선택하여 직접 풀어본 뒤**, 강의 영상의 풀이와 비교해 **틀린 부분, 맞은 부분, 새롭게 배운 개념**을 구체적으로 정리해주세요. (적어도 4문제는 정리해야 합니다.) 완성된 과제는 Gihub에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**👀(수행 인증샷은 필수입니다.)** 



## SQL_BASIC_5th

### 섹션 5. 데이터 탐색 - 변환

### 4-4. 날짜 및 시간 데이터 이해하기(2) (EXTRACT, DATETIME_TRUNC, PARSE_DATETIME, FROMAT_DATETIME)

### 4-5. 시간 데이터 연습문제 1~2번

### 4-5. 시간 데이터 연습문제 3~5번

### 4-6. 조건문 (CASE WHEN, IF)

### 4-7. 조건문 연습 문제

### 4-8. 정리

### 4-9. BigQuery 공식 문서 확인하는 법

(강의에서 연습문제가 많아서 따로 프로그래머스 문제 과제는 없습니다.)



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | ✅         |
| 5주차 | 섹션 **4-4** ~ **4-9** | ✅         |
| 6주차 | 섹션 **5-1** ~ **5-7** | 🍽️         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<br>



<!-- 여기까진 그대로 둬 주세요-->

---

# 4-4. 날짜 및 시간 데이터 이해하기(2) (EXTRACT, DATETIME_TRUNC, PARSE_DATETIME, FROMAT_DATETIME)

~~~
✅ 학습 목표 :
* 날짜 및 시간 데이터에 대해서 더 자세히 설명할 수 있다. 
* CURRENT_TIME, EXTRACT, DATETIME_TRUNC, PARSE_DATETIME, FROMAT_DATETIME 을 설명할 수 있다. 
~~~

## CURRENT_DATETIME

현재 DATETIME을 반환하는 함수. 괄호 안에 타임존을 지정할 수 있음.
```sql
SELECT
  CURRENT_DATE() AS current_date,
  CURRENT_DATE("Asia/Seoul") AS asia_date,
  CURRENT_DATETIME() AS current_datetime,
  CURRENT_DATETIME("Asia/Seoul") AS current_datetime_asia;
```

> ⚠️ 타임존을 지정하지 않으면 UTC 기준 → 한국 시간(KST)과 9시간 차이 발생

---

## EXTRACT

DATETIME에서 **특정 부분(연/월/일/시/분 등)만 추출**할 때 사용.
```sql
EXTRACT(part FROM datetime_col)
```

| part | 설명 |
|------|------|
| YEAR / MONTH / DAY | 연, 월, 일 |
| HOUR / MINUTE / SECOND | 시, 분, 초 |
| DAYOFWEEK | 요일 (일요일=1 ~ 토요일=7) |
| WEEK / MONTH / QUARTER | 주차, 월, 분기 |
```sql
SELECT
  EXTRACT(YEAR FROM DATETIME "2024-01-02 14:00:00") AS year,   -- 2024
  EXTRACT(MONTH FROM DATETIME "2024-01-02 14:00:00") AS month, -- 1
  EXTRACT(DAY FROM DATETIME "2024-01-02 14:00:00") AS day,     -- 2
  EXTRACT(HOUR FROM DATETIME "2024-01-02 14:00:00") AS hour;   -- 14
```

**DAYOFWEEK 예시** (일=1, 월=2, 화=3 ... 토=7)
```sql
EXTRACT(DAYOFWEEK FROM datetime_col)
-- 월~금 필터: WHERE EXTRACT(DAYOFWEEK FROM ...) BETWEEN 2 AND 6
```

> 💡 EXTRACT vs DATETIME_TRUNC 언제 쓸까?
> - 숫자로 뽑아서 집계/필터링 → EXTRACT
> - 시간을 잘라서 그대로 DATETIME 유지 → DATETIME_TRUNC

---

## DATETIME_TRUNC

DATETIME을 **특정 단위로 잘라내기** (시간 자르기).
지정한 단위 이하는 모두 0(또는 최솟값)으로 초기화됨.
```sql
DATETIME_TRUNC(datetime_col, part)
```
```sql
-- "2024-03-02 14:42:13"을 각 단위로 자르면
DATETIME_TRUNC(..., DAY)   → 2024-03-02T00:00:00
DATETIME_TRUNC(..., MONTH) → 2024-03-01T00:00:00
DATETIME_TRUNC(..., YEAR)  → 2024-01-01T00:00:00
DATETIME_TRUNC(..., HOUR)  → 2024-03-02T14:00:00
```

> 💡 자주 쓰는 상황: 1시간 단위 수요 집계, 일/월별 집계 시 GROUP BY 기준으로 활용

---

## PARSE_DATETIME

**문자열 → DATETIME 타입**으로 변환.
DB에 문자열로 저장된 날짜 데이터를 DATETIME으로 파싱할 때 사용.
```sql
PARSE_DATETIME('문자열의 형태', '변환할 문자열') AS datetime

-- 예시
PARSE_DATETIME('%Y-%m-%d %H:%M:%S', '2024-01-11 12:35:35')
-- 결과: 2024-01-11T12:35:35
```

---

## FORMAT_DATETIME

**DATETIME 타입 → 특정 형태의 문자열**로 변환.
```sql
FORMAT_DATETIME('형식', datetime_col) AS formatted

-- 예시
FORMAT_DATETIME("%c", DATETIME "2024-01-11 12:35:35")
-- 결과: Wed, Jan 11 12:35:35 2024
```

| 자주 쓰는 포맷 | 의미 |
|---|---|
| %Y | 4자리 연도 |
| %m | 2자리 월 |
| %d | 2자리 일 |
| %H | 24시간 기준 시 |
| %A | 요일 전체 이름 (Monday 등) |

> 📌 포맷 문자는 외울 필요 없음 — 공식 문서 보고 필요할 때 찾아 쓰기

---

## 추가 함수 요약

| 함수 | 용도 |
|---|---|
| LAST_DAY(datetime) | 해당 월의 마지막 날 반환 (월말 정산, D-DAY 계산 등) |
| DATETIME_DIFF(dt1, dt2, part) | 두 DATETIME의 차이 계산 (일/월/주 단위) |

---

## 최종 정리

| 목적 | 함수 |
|---|---|
| 현재 시각 | CURRENT_DATETIME |
| 특정 부분 추출 (숫자) | EXTRACT |
| 특정 단위로 자르기 | DATETIME_TRUNC |
| 두 날짜 차이 | DATETIME_DIFF |
| 문자열 → DATETIME | PARSE_DATETIME |
| DATETIME → 문자열 | FORMAT_DATETIME |
| 월 마지막 날 | LAST_DAY |



# 4-6. 조건문(CASE WHEN, IF)

~~~
✅ 학습 목표 :
* 조건문 함수의 기능을 이해하고, 설명할 수 있다. 
~~~

## 조건문이란?

특정 조건이 참일 때 A, 아니면 B — 조건에 따라 다른 값을 표시하고 싶을 때 사용.

**조건문이 필요한 이유**: 데이터 분석 시 특정 카테고리를 하나로 합치는 전처리가 필요한 경우가 많음.
데이터를 저장하는 쪽과 분석하는 쪽이 나뉘기 때문에, 저장 시 합쳐버리면 나중에 쪼개서 볼 수 없음.
→ 분석할 때 필요한 부분에서 조건 설정해서 변경하는 것이 더 유용.

---

## 1) CASE WHEN

**여러 조건이 있을 경우 유용**.
```sql
SELECT
  CASE
    WHEN 조건1 THEN 조건1이 참일 경우 결과
    WHEN 조건2 THEN 조건2가 참일 경우 결과
    ELSE 그 외 조건일 경우 결과
  END AS 새로운_컬럼_이름
FROM 테이블
```

**예시 — 포켓몬 타입 합치기** (Rock과 Ground를 "Rock&Ground"로 묶기)
```sql
SELECT
  CASE
    WHEN type1 IN ("Rock", "Ground") THEN "Rock&Ground"
    ELSE type1
  END AS new_type1,
  COUNT(*) AS cnt
FROM basic.pokemon
GROUP BY new_type1
```

**⚠️ CASE WHEN 순서 주의**

조건1, 조건2에 둘 다 해당하면 **앞선 조건이 우선** 적용됨.
```sql
-- ❌ 잘못된 순서: attack >= 50이 먼저라서 100 이상도 'Strong'으로 분류됨
CASE
  WHEN attack >= 50 THEN 'Strong'
  WHEN attack >= 100 THEN 'Very Strong'  -- 절대 실행 안 됨
  ELSE 'Weak'
END

-- ✅ 올바른 순서: 더 좁은 조건(큰 값)을 먼저
CASE
  WHEN attack >= 100 THEN 'Very Strong'
  WHEN attack >= 50 THEN 'Strong'
  ELSE 'Weak'
END AS attack_level
```

> 💡 문자열 함수(특정 단어 추출)와 함께 쓸 때 순서 이슈가 자주 발생하므로 주의

---

## 2) IF

**단일 조건일 경우 유용**.
```sql
IF(조건문, True일 때의 값, False일 때의 값) AS 새로운_컬럼_이름
```

**예시**
```sql
SELECT
  IF(1=1, '동일한 결과', '동일하지 않은 결과') AS result1,   -- → '동일한 결과'
  IF(1=2, '동일한 결과', '동일하지 않은 결과') AS result2    -- → '동일하지 않은 결과'
```

---

## 정리

| 함수 | 사용 상황 |
|---|---|
| CASE WHEN | 여러 조건이 있을 경우. **조건의 순서에 주의** |
| IF | 단일 조건일 경우 |



 # 4-5. 시간 데이터 연습문제 & 4-7. 조건문 연습 문제

~~~
✅ 학습 목표 :
* 4-5, 4-7 각각에서 두 문제 이상 (최소 4문제) 푼 내용 정리하기
~~~

## 문제 1. 트레이너가 포켓몬을 포획한 날짜(catch_date) 기준으로, 2023년 1월에 포획한 포켓몬의 수를 계산하세요.

### 풀이 과정에서 배운 것

- `catch_datetime` 컬럼이 실제로는 TIMESTAMP 타입으로 저장되어 있었음
- 컬럼 이름만 믿지 말고 **항상 실제 타입을 직접 확인**해야 함
- TIMESTAMP → DATETIME 변환 시 타임존 지정 필요 (`"Asia/Seoul"`)
- catch_date가 UTC 기준인지 KR 기준인지 확인 필요 → `DATE(DATETIME(catch_datetime, "Asia/Seoul"))`로 비교해서 검증
```sql
-- 데이터 검증 쿼리 (먼저 확인!)
SELECT
  catch_date,
  DATE(DATETIME(catch_datetime, "Asia/Seoul")) AS catch_datetime_kr_date
FROM basic.trainer_pokemon

-- 정답 쿼리
SELECT
  COUNT(DISTINCT id) AS cnt
FROM basic.trainer_pokemon
WHERE
  EXTRACT(YEAR FROM DATETIME(catch_datetime, "Asia/Seoul")) = 2023
  AND EXTRACT(MONTH FROM DATETIME(catch_datetime, "Asia/Seoul")) = 1
```

> **컬럼 설명을 꼭 확인하고 SQL을 작성할 것!**
> 문제 출제 의도대로 풀어도 컬럼 타입이 다르면 틀릴 수 있음. 실무에서도 동일한 상황 발생.

---

## 문제 2. 배틀이 일어난 시간(battle_datetime) 기준으로, 오전 6시에서 오후 6시 사이에 일어난 배틀의 수를 계산하세요.

### 풀이 과정에서 배운 것

- `battle_datetime`(DATETIME)과 `battle_timestamp`(TIMESTAMP)가 실제로 같은 값인지 검증 필요
- `COUNTIF`로 두 컬럼 비교해서 데이터 일치 여부 확인 가능
- `BETWEEN a AND b` → a 이상 b 이하
```sql
-- 데이터 검증
SELECT
  COUNTIF(battle_datetime = DATETIME(battle_timestamp, "Asia/Seoul")) AS same_cnt,
  COUNTIF(battle_datetime != DATETIME(battle_timestamp, "Asia/Seoul")) AS not_same_cnt
FROM basic.battle

-- 정답 쿼리
SELECT
  COUNT(DISTINCT id) AS battle_cnt
FROM basic.battle
WHERE
  EXTRACT(HOUR FROM battle_datetime) BETWEEN 6 AND 18
```

> 강의 수정 사항: `<= 18`을 사용하면 18시가 포함되므로 정확히는 `< 18`이 맞음.
> **EXTRACT로 HOUR를 추출해서 필터링**
```sql
-- 시간대별 배틀 수도 함께 확인 가능
SELECT
  hour,
  COUNT(DISTINCT id) AS battle_cnt
FROM (
  SELECT
    *,
    EXTRACT(HOUR FROM battle_datetime) AS hour
  FROM basic.battle
)
WHERE hour BETWEEN 6 AND 18
GROUP BY hour
ORDER BY hour
```

---

## 문제 3. 각 트레이너별로 포켓몬을 포획한 첫 날(catch_date)을 찾고, 그 날짜를 'DD/MM/YYYY' 형식으로 출력하세요. (2024-01-01 → 01/01/2024)
```sql
SELECT
  trainer_id,
  FORMAT_DATE('%d/%m/%Y', MIN(catch_date)) AS first_catch_date
FROM basic.trainer_pokemon
GROUP BY trainer_id
```

> `MIN(catch_date)`로 가장 빠른 날짜를 구한 뒤, `FORMAT_DATE`로 형식 변환

---

## 문제 4. 배틀이 일어난 날짜(battle_date)를 기준으로, 요일별로 배틀이 얼마나 자주 일어났는지 계산하세요.
```sql
SELECT
  EXTRACT(DAYOFWEEK FROM battle_date) AS day_of_week,
  COUNT(DISTINCT id) AS battle_cnt
FROM basic.battle
GROUP BY day_of_week
ORDER BY day_of_week
```

> DAYOFWEEK: 일요일=1, 월요일=2, ... 토요일=7

---

## 문제 5. 트레이너가 포켓몬을 처음으로 포획한 날짜와 마지막으로 포획한 날짜의 간격이 큰 순으로 정렬하는 쿼리를 작성하세요.
```sql
SELECT
  trainer_id,
  MIN(catch_date) AS first_catch,
  MAX(catch_date) AS last_catch,
  DATE_DIFF(MAX(catch_date), MIN(catch_date), DAY) AS diff_days
FROM basic.trainer_pokemon
GROUP BY trainer_id
ORDER BY diff_days DESC
```

> `DATE_DIFF(나중 날짜, 이전 날짜, DAY)`로 날짜 차이를 일 단위로 계산
> 
# 4-7. 조건문 연습 문제 풀이 정리

## 문제 1. Speed 기준으로 Speed_Category 컬럼 만들기

> 포켓몬의 'Speed'가 70 이상이면 '빠름', 그렇지 않으면 '느림'으로 표시하는 새로운 컬럼 'Speed_Category'를 만들어 주세요.

```sql
SELECT
  name,
  speed,
  CASE
    WHEN speed >= 70 THEN '빠름'
    ELSE '느림'
  END AS Speed_Category
FROM pokemon;
```

- 조건이 2가지일 때는 `IF`, 3가지 이상일 때는 `CASE WHEN`이 더 가독성이 좋다
- `IF`와 `CASE WHEN`은 결과는 동일하지만 상황에 따라 선택해서 쓰는 것이 좋음

---

## 문제 3. total 기준으로 등급 분류하기

> 각 포켓몬의 총점(total)을 기준으로, 300 이하면 'Low', 301에서 500 사이면 'Medium', 501 이상이면 'High'로 분류해 주세요.

```sql
SELECT
  name,
  total,
  CASE
    WHEN total <= 300 THEN 'Low'
    WHEN total BETWEEN 301 AND 500 THEN 'Medium'
    ELSE 'High'
  END AS total_grade
FROM pokemon;
```

- `CASE WHEN`은 위에서부터 순서대로 조건을 평가하므로, **마지막 조건은 `ELSE`로 처리하는 것이 더 안전하고 간결**하다
- `ELSE`가 없을 경우 어떤 조건에도 해당하지 않으면 `NULL`이 반환될 수 있으니 주의 필요

---

### 📌 핵심 정리

| 구분 | 사용 상황 | 문법 |
|---|---|---|
| `IF` | 조건이 2가지일 때 | `IF(조건, 참, 거짓)` |
| `CASE WHEN` | 조건이 3가지 이상일 때 | `CASE WHEN 조건 THEN 값 ELSE 값 END` |
| `ELSE` | 나머지 모든 경우 처리 | 마지막 조건 대신 사용, 누락 시 NULL 반환 주의 |


<br>

<br>

---

# 확인문제

## 문제 1

> **🧚Q. 광윤이는 카페 주문 로그 데이터(order_log)를 분석하여, '오전(0시-11시)'과 '오후(12시-23시)'의 주문 건수를 집계하려고 합니다. 광윤이가 작성한 다음 SQL 쿼리 중 문법적으로 틀렸거나 의도한 결과가 나오지 않는 것을 모두 골라보세요. (복수 선택 가능)**

~~~sql
1. SELECT 
   IF(EXTRACT(HOUR FROM order_time) < 12, '오전', '오후') AS time_type,
   COUNT(*)
   FROM order_log
   GROUP BY time_type;

2. SELECT 
   DATETIME_TRUNC(order_time, HOUR) AS truncated_hour,
   COUNT(*)
   FROM order_log
   WHERE order_time BETWEEN '2021-01-01' AND '2021-12-31'
   GROUP BY order_time;

3. SELECT 
   FORMAT_DATETIME(order_time, '%H') AS order_hour,
   COUNT(*)
   FROM order_log
   GROUP BY 1;

4. SELECT 
    CASE 
      WHEN EXTRACT(HOUR FROM order_time) BETWEEN 0 AND 11 THEN '오전'
      ELSE '오후'
    AS time_group,
    COUNT(*)
   FROM order_log
   GROUP BY time_group;
~~~

<!-- 틀린쿼리에 대한 오류의 원인도 같이 작성해주세요. 문제에서 제공된 order_time 컬럼은 DATETIME type의 데이터를 가지고 있다고 가정합니다. -->

~~~
**틀린 쿼리: 2번, 3번, 4번**
**2번 - 틀림**
```sql
SELECT 
   DATETIME_TRUNC(order_time, HOUR) AS truncated_hour,
   COUNT(*)
   FROM order_log
   WHERE order_time BETWEEN '2021-01-01' AND '2021-12-31'
   GROUP BY order_time;  -- ❌
```

- `SELECT`에서 `truncated_hour`로 truncate된 값을 쓰고 있는데, `GROUP BY`에는 원본 `order_time`을 쓰고 있어서 의도한 시간별 집계가 되지 않음
- `GROUP BY truncated_hour` 또는 `GROUP BY 1`로 수정해야 함

---

**3번 - 틀림**
```sql
SELECT 
   FORMAT_DATETIME(order_time, '%H') AS order_hour,  -- ❌
   COUNT(*)
   FROM order_log
   GROUP BY 1;
```

- `FORMAT_DATETIME`의 인자 순서가 잘못됨
- 올바른 문법: `FORMAT_DATETIME('%H', order_time)`
- **형식 문자열이 첫 번째, 날짜 컬럼이 두 번째** 인자여야 함

---

**4번 - 틀림**
```sql
CASE 
  WHEN EXTRACT(HOUR FROM order_time) BETWEEN 0 AND 11 THEN '오전'
  ELSE '오후'
AS time_group  -- ❌
```

- `CASE WHEN` 블록을 닫는 **`END`가 누락**됨
- `ELSE '오후'` 다음에 `END AS time_group`이 있어야 함

---

**1번 - 정상** ✅
```sql
SELECT 
   IF(EXTRACT(HOUR FROM order_time) < 12, '오전', '오후') AS time_type,
   COUNT(*)
   FROM order_log
   GROUP BY time_type;
```

- BigQuery는 `GROUP BY`에 alias 사용을 허용하므로 문법적으로 올바르고, 의도한 결과도 정상 출력됨

~~~



## 문제 2

> **🧚Q. 예운이는 포켓몬 타입에 따라 설명을 부여하는 쿼리를 작성했습니다. type 1 컬럼의 값에 따라 조건을 분기했으며, 다음 SQL 쿼리를 실행했습니다.**

~~~sql
SELECT name,
       CASE 
         WHEN type1 = 'Fire' THEN 'Hot'
         WHEN type1 = 'Water' THEN 'Cool'
         ELSE 'Normal'
       END AS type_description
FROM pokemon;
~~~

> **다음 중 type_description의 결과가 'Normal'로 출력될 포켓몬은?**

| **name**   | **type1** |
| ---------- | --------- |
| Pikachu    | Electric  |
| Charmander | Fire      |
| Squirtle   | Water     |
| Bulbasaur  | Grass     |

<!-- 근거와 함께 답을 작성해주세요 -->

~~~
**답: Pikachu, Bulbasaur**

| name | type1 | type_description |
|---|---|---|
| Pikachu | Electric | **Normal** |
| Charmander | Fire | Hot |
| Squirtle | Water | Cool |
| Bulbasaur | Grass | **Normal** |

- `CASE WHEN`은 `Fire` → `'Hot'`, `Water` → `'Cool'` 조건만 명시되어 있고, 나머지는 모두 `ELSE 'Normal'`로 처리됨
- `Electric`, `Grass`는 두 조건 모두 해당하지 않으므로 `ELSE` 분기를 타서 **'Normal'** 출력
~~~



<br>

### 🎉 수고하셨습니다.

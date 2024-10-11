
테이블 정의서 만드는 테이블 
```sql
SELECT

c.table_name AS "테이블명",

c.column_name AS "컬럼명",

COALESCE(col_description(a.attrelid, a.attnum), '') AS "컬럼명(한글)",

c.data_type AS "데이터 타입",

CASE

WHEN pgc.contype = 'p' THEN 'PK'

WHEN pgc.contype = 'u' THEN 'UQ'

WHEN pgc.contype = 'f' THEN 'FK (' || rco.table_name || '.' || rco.column_name || ')'

ELSE ''

END AS "제약조건",

CASE

WHEN c.is_nullable = 'NO' THEN 'NO'

ELSE 'YES'

END AS "NULLABLE",

COALESCE(c.column_default, '') AS "DEFAULT"

FROM

information_schema.columns c

LEFT JOIN

pg_attribute a ON a.attrelid = (

SELECT oid FROM pg_class WHERE relname = c.table_name AND relnamespace = (

SELECT oid FROM pg_namespace WHERE nspname = 'public'

)

) AND a.attname = c.column_name

LEFT JOIN

pg_constraint pgc ON pgc.conrelid = a.attrelid AND a.attnum = ANY (pgc.conkey)

LEFT JOIN

information_schema.key_column_usage rco ON rco.constraint_name = pgc.conname

WHERE

c.table_schema = 'public'

ORDER BY

c.table_name, c.ordinal_position;
```


모든 테이블 데이터 삭제
```sql
DO

$$

DECLARE

r RECORD;

BEGIN

-- 외래 키 제약을 무시

EXECUTE 'SET session_replication_role = replica';

  

-- 현재 스키마 내의 모든 테이블에 대해 TRUNCATE 실행

FOR r IN (SELECT tablename FROM pg_tables WHERE schemaname = 'public') LOOP

EXECUTE 'TRUNCATE TABLE ' || quote_ident(r.tablename) || ' CASCADE';

END LOOP;

  

-- 외래 키 제약 복원

EXECUTE 'SET session_replication_role = DEFAULT';

END

$$;

  

INSERT INTO events_count (count) VALUES (57742);

  

SELECT * FROM tags;
```



모든 테이블 돌면서 \_count와  \_count를 제거한 테이블 갯수 비교
```sql
CREATE OR REPLACE FUNCTION get_counts()

RETURNS TABLE(base_table_name TEXT, total_count BIGINT, count_table_name TEXT, count2 TEXT) AS $$

DECLARE

rec RECORD;

base_table_name TEXT;

query TEXT;

count_column TEXT; -- count 테이블의 열 이름을 담을 변수

BEGIN

FOR rec IN

SELECT table_name

FROM information_schema.tables

WHERE table_name LIKE '%_count%'

AND table_schema = 'public'

LOOP

-- '_count'를 제거하여 기본 테이블 이름을 얻습니다.

base_table_name := replace(rec.table_name, '_count', '');

  

-- 기본 테이블이 존재하는지 확인

IF EXISTS (

SELECT 1

FROM information_schema.tables

WHERE table_name = base_table_name

AND table_schema = 'public'

) THEN

-- _count 테이블의 첫 번째 열 이름 가져오기

SELECT column_name INTO count_column

FROM information_schema.columns

WHERE table_name = rec.table_name

AND table_schema = 'public'

LIMIT 1;

  

-- 기본 테이블의 총 카운트 쿼리 생성

query := format('SELECT %L AS base_table_name, COUNT(*) AS total_count, %L AS count_table_name,

(SELECT %I::TEXT FROM %I LIMIT 1) AS count2

FROM %I',

base_table_name,

rec.table_name,

count_column, -- 동적으로 가져온 열 이름 사용

rec.table_name,

base_table_name);

-- 동적으로 쿼리 실행 및 결과 반환

RETURN QUERY EXECUTE query;

END IF;

END LOOP;

END $$ LANGUAGE plpgsql;
```




모든 테이블에서 s붙은거와 안붙은 애들이 있는것들만 출력

```sql
CREATE OR REPLACE FUNCTION get_tables_with_s_variant()

RETURNS TABLE(base_table_name TEXT, plural_table_name TEXT) AS $$

DECLARE

rec RECORD;

base_table_name TEXT;

BEGIN

-- '_s'로 끝나는 테이블들을 검색

FOR rec IN

SELECT table_name::TEXT -- 명시적으로 TEXT로 캐스팅

FROM information_schema.tables

WHERE table_name LIKE '%s'

AND table_schema = 'public'

LOOP

-- '_s'를 제거하여 기본 테이블 이름 추출

base_table_name := LEFT(rec.table_name, LENGTH(rec.table_name) - 1);

  

-- 해당 기본 테이블이 존재하는지 확인

IF EXISTS (

SELECT 1

FROM information_schema.tables

WHERE table_name = base_table_name

AND table_schema = 'public'

) THEN

-- 기본 테이블과 '_s'로 끝나는 테이블을 반환

RETURN QUERY

SELECT base_table_name, rec.table_name::TEXT; -- 두 번째 컬럼도 TEXT로 캐스팅

END IF;

END LOOP;

END $$ LANGUAGE plpgsql;

```


function 삭제
```sql
DROP FUNCTION IF EXISTS get_tables_with_s_variant();
```


id 및  모든 테이블에서 s붙은거와 안붙은 애들이 있는것들만 출력
```sql
CREATE OR REPLACE FUNCTION get_tables_with_s_variant()

RETURNS TABLE (base_table_name TEXT, plural_table_name TEXT, id_column TEXT, table_id BIGINT) AS $$

DECLARE

rec RECORD;

base_table_name TEXT;

plural_table_name TEXT;

id_column TEXT;

query TEXT;

BEGIN

FOR rec IN

SELECT table_name

FROM information_schema.tables

WHERE table_name LIKE '%s'

AND table_schema = 'public'

LOOP

base_table_name := LEFT(rec.table_name, LENGTH(rec.table_name) - 1); -- Remove 's'

-- Define id_column based on base table name

id_column := CASE base_table_name

WHEN 'order' THEN 'order_id'

WHEN 'product' THEN 'product_id'

ELSE 'id' -- 기본적으로 'id'를 사용합니다.

END;

  

-- 실제 테이블의 ID 컬럼이 있는지 확인

IF EXISTS (

SELECT 1

FROM information_schema.columns

WHERE table_name = rec.table_name

AND column_name = id_column

) THEN

-- Create dynamic query, casting the id to BIGINT

query := format('SELECT %L AS base_table_name, %L AS plural_table_name, %L AS id_column, %I::BIGINT AS table_id FROM %I',

base_table_name, rec.table_name, id_column, id_column, rec.table_name);

-- Execute query dynamically and return result

RETURN QUERY EXECUTE query;

END IF;

END LOOP;

END $$ LANGUAGE plpgsql;
```





id가 base_table_name+ \_id 로 나오게 수정
```sql
CREATE OR REPLACE FUNCTION get_tables_with_s_variant()

RETURNS TABLE (base_table_name TEXT, plural_table_name TEXT, id_column TEXT, table_id BIGINT) AS $$

DECLARE

rec RECORD;

base_table_name TEXT;

plural_table_name TEXT;

id_column TEXT;

BEGIN

FOR rec IN

SELECT table_name

FROM information_schema.tables

WHERE table_name LIKE '%s' -- 's'로 끝나는 테이블 찾기

AND table_schema = 'public'

LOOP

base_table_name := LEFT(rec.table_name, LENGTH(rec.table_name) - 1); -- 's' 제거

  

-- 기본 테이블 존재 여부 확인

IF EXISTS (

SELECT 1

FROM information_schema.tables

WHERE table_name = base_table_name

AND table_schema = 'public'

) THEN

-- ID 컬럼 정의

id_column := base_table_name || '_id'; -- base_table_name + '_id'

  

-- 실제 테이블의 ID 컬럼이 있는지 확인

IF EXISTS (

SELECT 1

FROM information_schema.columns

WHERE table_name = rec.table_name

AND column_name = 'id' -- 실제 컬럼은 'id'입니다.

) THEN

-- 동적 쿼리 생성 및 반환 (id를 BIGINT로 캐스팅)

RETURN QUERY EXECUTE format('SELECT %L AS base_table_name, %L AS plural_table_name, ''%s'' AS id_column, id::BIGINT AS table_id FROM %I',

base_table_name, rec.table_name, id_column, rec.table_name);

END IF;

END IF;

END LOOP;

END $$ LANGUAGE plpgsql;
```



### 방법 2: `EXCEPT` 사용하기

`EXCEPT` 연산자를 사용하면, 첫 번째 쿼리의 결과에서 두 번째 쿼리의 결과를 제외하여 차집합을 구할 수 있습니다. 이 방법은 두 테이블의 공통된 컬럼이 `id`일 때 사용할 수 있습니다.

```sql
SELECT id FROM orders

EXCEPT

SELECT id FROM "order";
```





Jsonb 데이터 수정
PostgreSQL 같은 경우는 JSON 필드 내부 중 일부만 수정할 수 없으며 기존 JSON 데이터를 가공하여 만든 데이터를 저장하는 방식입니다.  
그래서 위에서 SELECT한 구문의 결과를 employees 필드에 새로 입력하는 방식입니다.

```sql
INSERT INTO company VALUES(1, '삼성', '[{"name":"홍길동1", "age": "20"}, {"name":"홍길동2", "age": "21"}, {"name":"홍길동3", "age": "22"}]');
```


```sql
UPDATE company
SET employees = employees || '{"name": "공유", "age": "15"}'
WHERE name = '삼성';
```




json 배열에서 인덱스 또는 key에 해당하는 요소를 추출하고 싶을 때가 있습니다. 이때 int 형이 주어지면 배열 내 인덱스로, text 형이 주어진다면 해당 key에 대응하는 요소를 추출합니다.

json 연산자
###  -> 
```sql
SELECT '[{"a":"foo"},{"b":"bar"},{"c":"baz"}]'::jsonb->2;
SELECT '{"a": {"b":"foo"}}'::jsonb->'a'
```

### ->>

```sql
SELECT '[1,2,3]'::jsonb->>2
SELECT '{"a":1,"b":2}'::json->>'b'
```

-> 연산자는 결과를 json 형태로 추출하며, ->> 연산자는 결과를 text 형으로 추출합니다.

```sql
>> SELECT pg_typeof('[{"a":"foo"},{"b":"bar"},{"c":"baz"}]'::jsonb->2); jsonb >> SELECT pg_typeof('[{"a":"foo"},{"b":"bar"},{"c":"baz"}]'::jsonb->>2); text
```



### [**2) #>, #>>**](https://yeongunheo.tistory.com/entry/PostgreSQL-json-jsonb-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%97%B0%EC%82%B0%EC%9E%90#--%--%--%-E%-C%--%--%-E%-E)

PostgreSQL에서 jsonb 컬럼을 조회할 때 중첩된 필드의 특정 조건만을 검색하고 싶을 때가 있습니다.

이때 #>, #>> 연산자를 사용하면 원하는 데이터를 추출할 수 있습니다. 검색 조건으로 활용하는 필드는 콤마로 구분합니다.

#> 연산자는 결과를 json 형태로 추출하며, #>> 연산자는 결과를 text 형으로 추출합니다.

```sql
>> SELECT '{"a": {"b":{"c": "foo"}}}'::jsonb#>'{a,b}';

{"c": "foo"}
```

위 연산자들은 json형으로 저장된 컬럼에 원하는 조건에 해당하는 데이터를 추출하고 싶을 때 사용할 수 있습니다.

```sql
SELECT * 
FROM member 
WHERE memberInfo #>> '{user,hostname}' = 'yeongun'
```

```sql
SELECT * 
FROM member 
WHERE memberInfo ->> 'user' = '{"hostname": "yeongun"}'
```

## [**3. jsonb 전용 연산자**](https://yeongunheo.tistory.com/entry/PostgreSQL-json-jsonb-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%97%B0%EC%82%B0%EC%9E%90#--%--jsonb%--%EC%A-%--%EC%-A%A-%--%EC%--%B-%EC%--%B-%EC%-E%--)

jsonb 타입에서만 사용할 수 있는 연산자도 존재합니다.

### [**1) @>, <@**](https://yeongunheo.tistory.com/entry/PostgreSQL-json-jsonb-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%97%B0%EC%82%B0%EC%9E%90#--%--%--%-E%-C%--%-C%--)

@>, <@ 연산자는 주어진 jsonb 값이 있는지 여부를 반환합니다. 결과는 true 또는 false 중 하나입니다.

```sql
>> SELECT '{"a":1, "b":2}'::jsonb @> '{"b":2}'::jsonb;
true

>> SELECT '{"b":2}'::jsonb <@ '{"a":1, "b":2}'::jsonb;
true
```

### [**2) ?, ?|, ?&**](https://yeongunheo.tistory.com/entry/PostgreSQL-json-jsonb-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%97%B0%EC%82%B0%EC%9E%90#--%--%-F%-C%--%-F%-C%-C%--%-F%--)

?, ?|, ?& 연산자는 jsonb 데이터 내 key가 존재하는지를 판단합니다. 결과는 true 또는 false 중 하나입니다.

? 연산자는 jsonb 데이터 내 해당 key가 존재하는지 여부를 반환합니다.

```sql
>> SELECT '{"a":1, "b":2}'::jsonb ? 'b';
true

>> SELECT '{"a":1, "b":2}'::jsonb ? 'c';
false
```

?|, ?& 연산자는 검색 조건으로 배열이 주어집니다.

?| 연산자는 주어진 배열 내 값들 중 jsonb 데이터의 key로 **하나라도** 존재하면 true를 반환합니다.

```sql
>> SELECT '{"a":1, "b":2, "c":3}'::jsonb ?| array['c', 'd'];
true

>> SELECT '{"a":1, "b":2, "c":3}'::jsonb ?| array['d', 'e'];
false
```

?& 연산자는 주어진 배열 내 값들이 **모두** jsonb 데이터의 key로 존재할 때 true를 반환합니다.

```sql
>> SELECT '["a", "b"]'::jsonb ?& array['a', 'b'];
true

>> SELECT '["a", "b"]'::jsonb ?& array['a', 'c'];
false
```

### [**3) ||**](https://yeongunheo.tistory.com/entry/PostgreSQL-json-jsonb-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%97%B0%EC%82%B0%EC%9E%90#--%--%-C%-C)

|| 연산자는 주어진 2개의 jsonb 데이터를 하나로 합칩니다.

```sql
>> SELECT '["a", "b"]'::jsonb || '["c", "d"]'::jsonb

["a", "b", "c", "d"]
```

### [**4) -**](https://yeongunheo.tistory.com/entry/PostgreSQL-json-jsonb-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%97%B0%EC%82%B0%EC%9E%90#--%---)

- 연산자는 jsonb 데이터 내 주어진 key를 제거합니다.

```sql
>> SELECT '{"a": 1, "b": 2, "c": 3}'::jsonb - 'a';

{"b": 2, "c": 3}
```

text 형이 아닌 int 형이 주어지면 배열 내 특정 인덱스에 해당하는 요소를 제거합니다.

```sql
>> SELECT '["a", "b", "c"]'::jsonb - 1;

["a", "c"]
```

### [**5) #-**](https://yeongunheo.tistory.com/entry/PostgreSQL-json-jsonb-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%97%B0%EC%82%B0%EC%9E%90#--%--%---)

#- 연산자는 주어진 경로에 해당하는 값을 제거합니다.

```sql
>> SELECT '["a", {"b":{"c": "foo"}}]'::jsonb #- '{1,b,c}';

["a", {"b": {}}]
```
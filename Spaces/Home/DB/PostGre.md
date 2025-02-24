
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

  ----------------------------------------- 스키마 지정

DO

$$

DECLARE

r RECORD;

target_schema TEXT := 'webhook'; -- 원하는 스키마 이름

BEGIN

EXECUTE 'SET session_replication_role = replica';

  

FOR r IN (SELECT tablename FROM pg_tables WHERE schemaname = target_schema) LOOP

EXECUTE 'TRUNCATE TABLE ' || quote_ident(target_schema) || '.' || quote_ident(r.tablename) || ' CASCADE';

END LOOP;

END

$$;

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


## **1. json vs jsonb 타입**


json 타입은 **입력된 텍스트 원본**을 저장하고, jsonb 타입은 **decopmose된 바이너리 형식**으로 저장한다는 차이가 있습니다.

json 타입은 입력된 텍스트에 대한 정확한 복사본을 저장하기 때문에, 처리할 때마다 매번 파싱을 다시 해야만 합니다.  
jsonb 타입에 비해 처리 속도가 느리나, 원본 그대로를 저장할 수 있다는 장점이 있습니다.

jsonb 타입은 정제된 데이터를 저장합니다. 데이터를 저장할 땐 추가적인 변환이 필요해 속도가 약간 느리지만, 이후 추가적인 파싱이 필요하지 않으므로 처리 속도가 json 타입에 비해 빠릅니다. 또한, jsonb 타입은 인덱싱을 지원한다는 아주 강력한 이점도 갖고 있습니다.

json 타입은 입력된 텍스트의 정확한 복사본을 저장하기에 토큰 사이의 의미없는 공백도 모두 저장하며, JSON 객체 내 키 순서도 보장합니다. 또한, 중복된 키가 있는 경우에도 모든 key가 중복되어 저장됩니다.

반면 jsonb 타입은 공백을 보존하지 않고, 객체 키의 순서를 보장하지 않으며 중복 객체 key 또한 저장하지 않습니다. 만약 중복 key 값이 입력되면 마지막 key의 값만을 저장합니다.

```sql
>> SELECT '{"bar": "baz", "balance": 7.77, "active":false}'::json;

{"bar": "baz", "balance": 7.77, "active":false}
```

```sql
>> SELECT '{"bar": "baz", "balance": 7.77, "active":false}'::jsonb;

{"bar": "baz", "active": false, "balance": 7.77}
```

일반적으로 대부분의 애플리케이션은 기존 객체 키에 대한 순서 보장과 같은 요구사항이 없는 한 json 데이터를 jsonb 타입으로 저장합니다.

## **2. json 연산자**

### 1) ->, ->>

json 배열에서 인덱스 또는 key에 해당하는 요소를 추출하고 싶을 때가 있습니다. 이때 int 형이 주어지면 배열 내 인덱스로, text 형이 주어진다면 해당 key에 대응하는 요소를 추출합니다.

```sql
>> SELECT '[{"a":"foo"},{"b":"bar"},{"c":"baz"}]'::jsonb->2;

{"c": "baz"}

>> SELECT '{"a": {"b":"foo"}}'::jsonb->'a'

{"b": "foo"}
```

```sql
>> SELECT '[1,2,3]'::jsonb->>2

3

>> SELECT '{"a":1,"b":2}'::json->>'b'

2
```

-> 연산자는 결과를 json 형태로 추출하며, ->> 연산자는 결과를 text 형으로 추출합니다.

```sql
>> SELECT pg_typeof('[{"a":"foo"},{"b":"bar"},{"c":"baz"}]'::jsonb->2);

jsonb

>> SELECT pg_typeof('[{"a":"foo"},{"b":"bar"},{"c":"baz"}]'::jsonb->>2);

text
```

### **2) #>, #>>**

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

## **3. jsonb 전용 연산자**

jsonb 타입에서만 사용할 수 있는 연산자도 존재합니다.

### **1) @>, <@**

@>, <@ 연산자는 주어진 jsonb 값이 있는지 여부를 반환합니다. 결과는 true 또는 false 중 하나입니다.

```sql
>> SELECT '{"a":1, "b":2}'::jsonb @> '{"b":2}'::jsonb;
true

>> SELECT '{"b":2}'::jsonb <@ '{"a":1, "b":2}'::jsonb;
true
```

### **2) ?, ?|, ?&**

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

### **3) ||**

|| 연산자는 주어진 2개의 jsonb 데이터를 하나로 합칩니다.

```sql
>> SELECT '["a", "b"]'::jsonb || '["c", "d"]'::jsonb

["a", "b", "c", "d"]
```

### **4) -**

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

### **5) \#-**

\#- 연산자는 주어진 경로에 해당하는 값을 제거합니다.

```sql
>> SELECT '["a", {"b":{"c": "foo"}}]'::jsonb #- '{1,b,c}';

["a", {"b": {}}]
```


## 모든 행 검색

```java
SELECT * FROM orders WHERE row_to_json(orders)::text LIKE '%21348%';
```


## 모든 행 검색 속도 개선

```sql
SELECT A.*,COUNT(*) OVER() totalCount FROM (
             select *
 from (
     
        SELECT * FROM products
         
        ) a
         
            WHERE EXISTS (
                SELECT 1
            FROM jsonb_each_text(row_to_json(a)::jsonb) AS kv
                WHERE kv.value ILIKE '%' || 'ateez' || '%'
            )
         

        ) A
        LIMIT 50
        OFFSET (1 - 1) * 50
```


```sql
WHERE EXISTS (
                SELECT 1
            FROM jsonb_each_text(row_to_json(a)::jsonb) AS kv
                WHERE kv.value ILIKE '%' || 'ateez' || '%'
            ) 
```

사용 jsonb로  변경 후 검색


`row_to_json` 함수를 사용하여 레코드를 JSON 형식으로 변환한 다음, 이를 JSONB로 변환하여 `jsonb_each_text` 함수를 사용할 수 있기 때문입니다.

### 요약

- **레코드 전체를 JSON으로 변환**: `row_to_json(a)`를 사용하여 레코드 전체를 JSON으로 변환합니다.
- **JSONB로 변환**: 그 후 `::jsonb`를 사용하여 JSON을 JSONB로 변환합니다.
- **모든 타입 지원**: 이 방법은 레코드에 포함된 모든 컬럼의 값을 포함하므로, JSONB 타입이 아니어도 상관없습니다
- 
이 쿼리는 `products` 테이블의 모든 컬럼을 검색하여, 'team'이라는 단어가 포함된 값을 가진 모든 레코드를 찾습니다. JSONB로 변환된 레코드는 다른 타입의 값들도 포함하므로, 다양한 데이터 타입에 대해 일관되게 검색할 수 있습니다.


## id값 숫자일때 +1 -1 하는법


```sql
SELECT (id::BIGINT) + 1 AS next_id, id, (id::BIGINT) + 1 AS pre_id

FROM orders

WHERE id ~ '^[0-9]+$';
```



모든 테이블에서   데이터 검색  notice로 나오게 하는 쿼리

```sql
DO $$
DECLARE
    table_name text;
    column_name text;
    search_value text := '검색할 데이터'; -- 검색할 데이터 지정
    query text;
    result int;
BEGIN
    -- 데이터베이스의 모든 테이블과 컬럼을 순회
    FOR table_name, column_name IN
        SELECT c.table_name, c.column_name
        FROM information_schema.columns c
        JOIN information_schema.tables t
        ON c.table_name = t.table_name
        WHERE t.table_schema = 'public' -- 스키마 이름 지정 (필요 시 변경)
          AND t.table_type = 'BASE TABLE'
    LOOP
        -- 검색할 쿼리 동적 생성
        query := format('SELECT 1 FROM %I.%I WHERE CAST(%I AS text) LIKE %L LIMIT 1', 'public', table_name, column_name, '%' || search_value || '%');
        
        BEGIN
            -- 쿼리 실행 및 검색 결과 저장
            EXECUTE query INTO result;

            -- 결과가 존재하면 NOTICE 출력
            IF result IS NOT NULL THEN
                RAISE NOTICE 'Value found in table %, column %', table_name, column_name;
            END IF;
        EXCEPTION WHEN others THEN
            -- 데이터 타입이 맞지 않거나 에러 발생 시 무시
            CONTINUE;
        END;
    END LOOP;
END $$;
```



order by
```sql
SELECT * FROM orders ORDER BY (line_items->>'quantity')::int DESC;
```


데이터가 없는 테이블 찾기
```sql
SELECT

schemaname,

relname AS tablename

FROM

pg_stat_user_tables

WHERE

n_live_tup = 0;
```


모든 테이블에서 데이터 찾기
```sql
CREATE OR REPLACE FUNCTION find_tables_with_value()

RETURNS TABLE(result_table_name TEXT) AS $$

DECLARE

r RECORD;

col RECORD;

query TEXT;

found BOOLEAN;

BEGIN

FOR r IN

SELECT table_name

FROM information_schema.tables

WHERE table_schema = 'public' -- 원하는 스키마를 지정

LOOP

RAISE NOTICE 'Checking table: %', r.table_name; -- 현재 검사 중인 테이블 이름 출력

-- 각 테이블의 모든 컬럼을 검사

FOR col IN

SELECT column_name

FROM information_schema.columns

WHERE table_schema = 'public' AND table_name = r.table_name

LOOP

query := format('SELECT EXISTS (

SELECT 1

FROM %I

WHERE %I::text ILIKE ''%%7401329033397%%''

)', r.table_name, col.column_name);

-- 쿼리 실행

EXECUTE query INTO found;

  

IF found THEN

result_table_name := r.table_name; -- 변수에 테이블 이름 저장

RETURN NEXT; -- 결과 반환

EXIT; -- 모든 컬럼 검사 후 테이블 발견 시 종료

END IF;

END LOOP;

END LOOP;

END; $$

LANGUAGE plpgsql;

  

-- 함수 호출

SELECT * FROM find_tables_with_value();
```


모든 테이블 데이터 동적으로 넣어서 검사
```sql
CREATE OR REPLACE FUNCTION find_tables_with_value(search_value TEXT)

RETURNS TABLE(result_table_name TEXT) AS $$

DECLARE

r RECORD;

col RECORD;

query TEXT;

found BOOLEAN;

BEGIN

FOR r IN

SELECT table_name

FROM information_schema.tables

WHERE table_schema = 'public' -- 원하는 스키마를 지정

LOOP

RAISE NOTICE 'Checking table: %', r.table_name; -- 현재 검사 중인 테이블 이름 출력

-- 각 테이블의 모든 컬럼을 검사

FOR col IN

SELECT column_name

FROM information_schema.columns

WHERE table_schema = 'public' AND table_name = r.table_name

LOOP

query := format('SELECT EXISTS (

SELECT 1

FROM %I

WHERE %I::text ILIKE ''%%%s%%''

)', r.table_name, col.column_name, search_value);

-- 쿼리 실행

EXECUTE query INTO found;

  

IF found THEN

result_table_name := r.table_name; -- 변수에 테이블 이름 저장

RETURN NEXT; -- 결과 반환

EXIT; -- 모든 컬럼 검사 후 테이블 발견 시 종료

END IF;

END LOOP;

END LOOP;

END; $$

LANGUAGE plpgsql;

  

-- 함수 호출 예시

SELECT * FROM find_tables_with_value('7401329033397');
```



```sql
CREATE OR REPLACE FUNCTION find_tables_with_value(search_value TEXT)

RETURNS TABLE(result_table_name TEXT, result_column_name TEXT) AS $$

DECLARE
    r RECORD;
    col RECORD;
    query TEXT;
    found BOOLEAN;
BEGIN
    -- 테이블 순회
    FOR r IN
        SELECT table_name
        FROM information_schema.tables
        WHERE table_schema = 'public' -- 원하는 스키마를 지정
    LOOP
        RAISE NOTICE 'Checking table: %', r.table_name; -- 현재 검사 중인 테이블 이름 출력
        
        -- 각 테이블의 모든 컬럼을 검사
        FOR col IN
            SELECT column_name
            FROM information_schema.columns
            WHERE table_schema = 'public' AND table_name = r.table_name
        LOOP
            query := format('SELECT EXISTS (
                SELECT 1
                FROM %I
                WHERE %I::text ILIKE ''%%%s%%''
            )', r.table_name, col.column_name, search_value);

            -- 쿼리 실행
            EXECUTE query INTO found;

            IF found THEN
                -- 테이블 이름과 컬럼 이름 반환
                result_table_name := r.table_name;
                result_column_name := col.column_name;
                RETURN NEXT; -- 결과 반환
            END IF;
        END LOOP;
    END LOOP;
END; $$

LANGUAGE plpgsql;


--- 함수 호출 예시
SELECT * FROM find_tables_with_value('8809755505462');
```


다른 테이블 컬럼을 jsonb로 원래 데이터 +로 넣기
```sql
SELECT o.*,
               COALESCE(
                   jsonb_agg(
                       jsonb_set(
                           item,
                           '{image}',
                           COALESCE(p.IMAGE -> 'src', '"default_image_url"')::jsonb
                       )
                   ),
                   '[]'::jsonb -- 빈 배열을 기본값으로 설정
               ) AS order_line_items
        FROM orders o
        LEFT JOIN jsonb_array_elements(o.LINE_ITEMS) AS item ON TRUE
        LEFT JOIN products p ON (item->>'product_id') = p.id
        WHERE o.id = '5327978725557'
        GROUP BY o.id
```


이상태에서 mybatis 사용시 resultMap으로 매핑 

```java
private List<Map<String, Object>> lineItems;
```

```xml
<resultMap id="orderResultMap" type="com.kpop.merch.orders.vo.OrdersVo">  
    <result property="lineItems" column="order_line_items" javaType="java.util.List"/>  
    <!-- 나머지 필드는 자동으로 매핑 -->  
</resultMap>
```


json 안에 json 쓰는 법
```sql
SELECT A.*,COUNT(*) OVER() totalCount FROM (
             select *
 from (
     
        SELECT * FROM checkouts
         
        ) a
         
         
            order by customer -> 'default_address' ->> 'country_code' asc
         
        ) A
        LIMIT 50
        OFFSET (1 - 1) * 50
```


jsonb 배열일때 값 가져오기
```sql
SELECT (variants->0)->>'inventory_item_id' FROM PRODUCTS P;
```

배열 길이 구하기
```sql
SELECT *
FROM PRODUCTS P
WHERE JSON_LENGTH(variants) >= 2;
```


json 배열 값 가져오기

```sql
SELECT variant->>'inventory_item_id' AS inventory_item_id

FROM PRODUCTS P,

LATERAL jsonb_array_elements(P.variants) AS variant

WHERE jsonb_array_length(P.variants) >= 2;
```

```sql
SELECT variant->>'inventory_item_id' AS inventory_item_id
FROM PRODUCTS P, 
LATERAL jsonb_array_elements(P.variants) AS variant
WHERE variant->>'inventory_item_id' = '12345';


SELECT *
FROM products p
LEFT JOIN LATERAL jsonb_array_elements(p.variants) AS variant ON TRUE
LEFT JOIN inventory_items i ON variant->>'inventory_item_id' = i.id;
```


```json
[
  {"id": 1, "name": "Alice"},
  {"id": 2, "name": "Bob"}
]
```


```sql
SELECT data->0->>'id' AS first_id FROM my_table;
```

- **`data->0`**: `JSONB` 배열에서 0번째 요소를 추출합니다. (이 요소는 JSON 객체 형태로 반환됨)
- **`->>'id'`**: 추출한 JSON 객체에서 `id` 필드의 값을 문자열로 반환합니다.

### 숫자로 변환하기:

만약 `id`가 숫자 타입으로 필요하다면, `::INTEGER`를 사용해 변환합니다:

```sql
SELECT (data->0->>'id')::INTEGER AS first_id
FROM my_table;
```



0번째 `id`가 특정 값인 행만 조회하고 싶다면:
```sql
SELECT *
FROM my_table
WHERE (data->0->>'id')::INTEGER = 1;
```



jsonb_array_elements(P.variants):

jsonb_array_elements 함수는 JSONB 배열의 각 요소를 개별 행으로 변환합니다.
P.variants는 PRODUCTS 테이블의 variants 컬럼을 의미하며, 이 컬럼에 저장된 JSONB 배열이 함수의 입력으로 사용됩니다.
이 함수는 배열의 각 요소를 반환하여, 배열에 있는 각 객체를 SQL 쿼리에서 다룰 수 있도록 해줍니다.
LATERAL:

LATERAL 키워드는 해당 행의 다른 컬럼을 참조할 수 있게 해줍니다.
즉, LATERAL을 사용하면 P.variants와 같은 테이블의 다른 컬럼을 참조하여, 각 행에 대해 jsonb_array_elements 함수를 적용할 수 있습니다.
이는 각 행의 variants 배열에 대해 함수가 호출되도록 보장합니다.

LEFT JOIN LATERAL을 빼고 LATERAL만 사용할 수는 있지만, 이 경우 products 테이블의 모든 행을 유지하는 것이 아니라, variants 배열이 있는 행만 반환하게 됩니다. LATERAL을 사용하면 각 행에 대해 variants 배열의 요소를 분리할 수 있지만, LEFT JOIN을 사용하지 않으면 variants가 없는 행은 결과에서 제외됩니다.


```sql
SELECT *
FROM products p,
LATERAL jsonb_array_elements(p.variants) AS variant
LEFT JOIN inventory_items i ON variant->>'inventory_item_id' = i.id;
```



모든 테이블에서 컬럼명 검색

```sql
SELECT table_name, column_name
FROM information_schema.columns
WHERE column_name = 'your_column_name'
  AND table_schema = 'public';  -- 필요에 따라 스키마를 변경하세요.
```


```sql

--- 둘다 가지고 있어야 함

SELECT table_name

FROM information_schema.columns

WHERE column_name IN ('payment_id', 'token')

AND table_schema = 'public' -- 필요에 따라 스키마를 변경하세요.

GROUP BY table_name

HAVING COUNT(DISTINCT column_name) = 2;

  

--- 둘중에 하나만 있어도 가능


SELECT table_name, column_name

FROM information_schema.columns

WHERE column_name IN ('payment_id', 'token')

AND table_schema = 'public'; -- 필요에 따라 스키마를 변경하세요.
```


table이 있는지 확인
```sql
SELECT EXISTS (

            SELECT 1

            FROM pg_catalog.pg_tables

            WHERE tablename = $1

        );


---------------------------- id값이 있는지 확인(유동적으로 변경 가능)

SELECT EXISTS (

            SELECT 1

            FROM information_schema.columns

            WHERE table_name = $1 AND column_name = 'id'

        );
       

```



jsonb 배열 데이터 SUM
```sql
SELECT *, (

SELECT SUM((item->>'quantity')::INTEGER)

FROM jsonb_array_elements(line_items) AS item

) AS total_quantity

FROM orders
```


## trigger
*트리거(Trigger)**는 데이터베이스에서 특정 이벤트(예: **INSERT**, **UPDATE**, **DELETE**)가 발생했을 때 **자동으로 실행되는 SQL 작업**입니다.

---

### **트리거의 주요 특징**

1. **자동 실행**: 데이터 조작 이벤트 발생 시 트리거가 자동 실행.
2. **이벤트 기반**: 특정 테이블에서 작업(INSERT, UPDATE, DELETE)이 발생할 때 동작.
3. **후속 작업 처리**: 로그 기록, 데이터 검증, 복잡한 연산 처리 등에 사용.

---

### **트리거 구성 요소**

1. **대상 테이블**: 트리거가 적용되는 테이블.
2. **이벤트**: 실행 조건 (INSERT, UPDATE, DELETE).
3. **타이밍**:
    - **BEFORE**: 이벤트 실행 전에 트리거 동작.
    - **AFTER**: 이벤트 실행 후에 트리거 동작.
4. **트리거 작업**: 트리거가 실행될 때 수행할 SQL 문장.

---

### **트리거 사용 예시**

#### 1. **로그 기록용 트리거**

- 직원 테이블에서 데이터가 수정될 때 로그 테이블에 기록.

```sql
CREATE TRIGGER log_update
AFTER UPDATE ON employees
FOR EACH ROW
INSERT INTO update_logs(employee_id, updated_at)
VALUES (NEW.id, NOW());
```


#### 2. **데이터 검증**

- 직원의 급여를 업데이트할 때 음수로 입력되지 않도록 검증.

```sql
CREATE TRIGGER check_salary
BEFORE UPDATE ON employees
FOR EACH ROW
BEGIN
  IF NEW.salary < 0 THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Salary cannot be negative';
  END IF;
END;
```








mysql
```sql
DELIMITER $$

  

-- AFTER INSERT 트리거

CREATE TRIGGER update_comment_exists_after_insert

AFTER INSERT ON board_qna

FOR EACH ROW

BEGIN

-- Check if parent_id is not NULL

IF NEW.parent_id IS NOT NULL THEN

UPDATE board_qna

SET comment_exists = 'Y'

WHERE id = NEW.parent_id;

END IF;

END$$

  

-- AFTER UPDATE 트리거

CREATE TRIGGER update_comment_exists_after_update

AFTER UPDATE ON board_qna

FOR EACH ROW

BEGIN

-- Check if parent_id is not NULL

IF NEW.parent_id IS NOT NULL THEN

UPDATE board_qna

SET comment_exists = 'Y'

WHERE id = NEW.parent_id;

END IF;

END$$

  

-- AFTER DELETE 트리거

CREATE TRIGGER update_comment_exists_after_delete

AFTER DELETE ON board_qna

FOR EACH ROW

BEGIN

-- Check if the deleted row had a parent_id

IF OLD.parent_id IS NOT NULL THEN

-- Check if there are no more rows with the same parent_id

IF NOT EXISTS (

SELECT 1

FROM board_qna

WHERE parent_id = OLD.parent_id

) THEN

-- Update comment_exists to 'N' for the parent row

UPDATE board_qna

SET comment_exists = 'N'

WHERE id = OLD.parent_id;

END IF;

END IF;

END$$

  

DELIMITER ;
```

insertm update 될때 자동으로 Y로 수정
delete 될때 자동으로 N으로 수정
```sql
-- AFTER INSERT 트리거 함수

CREATE OR REPLACE FUNCTION update_comment_exists_after_insert()

RETURNS TRIGGER AS $$

BEGIN

-- Check if parent_id is not NULL

IF NEW.parent_id IS NOT NULL THEN

UPDATE board_qna

SET comment_exists = 'Y'

WHERE id = NEW.parent_id;

END IF;

RETURN NEW;

END;

$$ LANGUAGE plpgsql;

  

-- AFTER INSERT 트리거

CREATE TRIGGER trigger_update_comment_exists_after_insert

AFTER INSERT ON board_qna

FOR EACH ROW

EXECUTE FUNCTION update_comment_exists_after_insert();

  

-- AFTER UPDATE 트리거 함수

CREATE OR REPLACE FUNCTION update_comment_exists_after_update()

RETURNS TRIGGER AS $$

BEGIN

-- Check if parent_id is not NULL

IF NEW.parent_id IS NOT NULL THEN

UPDATE board_qna

SET comment_exists = 'Y'

WHERE id = NEW.parent_id;

END IF;

RETURN NEW;

END;

$$ LANGUAGE plpgsql;

  

-- AFTER UPDATE 트리거

CREATE TRIGGER trigger_update_comment_exists_after_update

AFTER UPDATE ON board_qna

FOR EACH ROW

EXECUTE FUNCTION update_comment_exists_after_update();

  

-- AFTER DELETE 트리거 함수

CREATE OR REPLACE FUNCTION update_comment_exists_after_delete()

RETURNS TRIGGER AS $$

BEGIN

-- Check if the deleted row had a parent_id

IF OLD.parent_id IS NOT NULL THEN

-- Check if there are no more rows with the same parent_id

IF NOT EXISTS (

SELECT 1

FROM board_qna

WHERE parent_id = OLD.parent_id

) THEN

-- Update comment_exists to 'N' for the parent row

UPDATE board_qna

SET comment_exists = 'N'

WHERE id = OLD.parent_id;

END IF;

END IF;

RETURN OLD;

END;

$$ LANGUAGE plpgsql;

  

-- AFTER DELETE 트리거

CREATE TRIGGER trigger_update_comment_exists_after_delete

AFTER DELETE ON board_qna

FOR EACH ROW

EXECUTE FUNCTION update_comment_exists_after_delete();
```


```sql
CREATE OR REPLACE FUNCTION update_comment_exists_after_delete()

RETURNS TRIGGER AS $$

BEGIN

-- parent_id가 NULL이 아니면 parent_id 값을 가진 id의 comment_exists를 'N'으로 업데이트

IF OLD.parent_id IS NOT NULL THEN

UPDATE public.board_qna

SET comment_exists = 'N'

WHERE id = OLD.parent_id;

END IF;

  

-- 삭제된 행을 그대로 반환 (트리거가 AFTER DELETE이므로 RETURN OLD)

RETURN OLD;

END;

$$ LANGUAGE plpgsql;

  

-- 트리거 생성

CREATE TRIGGER trigger_update_comment_exists_after_delete

AFTER DELETE ON public.board_qna

FOR EACH ROW

EXECUTE FUNCTION update_comment_exists_after_delete();
```


특정 조건 trigger
```sql
CREATE OR REPLACE FUNCTION update_comment_exists_after_update()

RETURNS TRIGGER AS $$

BEGIN

-- 특정 조건: use_yn 변경되고, parent_id가 NULL이 아닌 경우에만 실행

IF NEW.parent_id IS NOT NULL AND OLD.use_yn IS DISTINCT FROM NEW.use_yn THEN

UPDATE public.board_qna

SET comment_exists = 'N'

WHERE id = NEW.parent_id;

END IF;

  

-- 업데이트된 행 반환

RETURN NEW;

END;

$$ LANGUAGE plpgsql;
```

trigger 삭제

```sql
DROP TRIGGER IF EXISTS trigger_update_comment_exists_after_delete ON public.board_qna;
```

use_yn이 바뀌면 그 이하 parent_id도 같이 use_yn이 바뀜
```sql
CREATE OR REPLACE FUNCTION update_child_use_yn()

RETURNS TRIGGER AS $$

BEGIN

-- 부모 ID의 use_yn을 "N"으로 변경하면, 자식들도 변경

UPDATE board_qna

SET use_yn = 'N'

WHERE parent_id = NEW.id::numeric;

RETURN NEW;

END;

$$ LANGUAGE plpgsql;

  

-- 2. 트리거 생성

CREATE TRIGGER trg_update_child_use_yn

AFTER UPDATE ON board_qna

FOR EACH ROW

WHEN (NEW.use_yn = 'N') -- use_yn이 'N'으로 변경될 때만 실행

EXECUTE FUNCTION update_child_use_yn();
```

### **트리거의 장점**

1. **자동화**: 특정 작업을 수동으로 처리할 필요 없음.
2. **데이터 무결성**: 데이터의 일관성을 유지.
3. **보안 강화**: 민감한 작업에 대한 로그를 자동 기록.

---

### **트리거의 단점**

1. **복잡성 증가**: 로직이 데이터베이스에 숨겨져 관리가 어려움.
2. **성능 부담**: 이벤트 발생 시마다 추가 작업이 실행되어 성능 저하 가능.
3. **디버깅 어려움**: 트리거 오류 추적이 복잡.

`TG_OP`는 PostgreSQL 트리거 함수 내에서 **작업 유형(`INSERT`, `UPDATE`, `DELETE`)을 확인**하는 데 표준적으로 사용하는 가상 변수입니다. 특정 동작을 구현할 때는 `TG_OP` 외에도 PostgreSQL에서 제공하는 다양한 **트리거 관련 변수**를 사용할 수 있습니다.


### PostgreSQL에서 사용할 수 있는 주요 트리거 변수

1. **`TG_OP`**
    
    - 현재 트리거를 실행한 작업 유형(`INSERT`, `UPDATE`, `DELETE`)을 반환합니다.
2. **`NEW`**
    
    - `INSERT` 또는 `UPDATE` 작업에서 새로 삽입되거나 수정된 행을 참조합니다.
    - 예: `NEW.column_name`
3. **`OLD`**
    
    - `UPDATE` 또는 `DELETE` 작업에서 이전 행을 참조합니다.
    - 예: `OLD.column_name`
4. **`TG_TABLE_NAME`**
    
    - 트리거가 호출된 테이블의 이름을 반환합니다.
5. **`TG_TABLE_SCHEMA`**
    
    - 트리거가 호출된 테이블의 스키마를 반환합니다.
6. **`TG_WHEN`**
    
    - 트리거가 호출된 시점(`BEFORE` 또는 `AFTER`)을 반환합니다.
7. **`TG_LEVEL`**
    
    - 트리거가 호출된 레벨(`ROW` 또는 `STATEMENT`)을 반환합니다.
8. **`TG_ARGV[]`**
    
    - 트리거를 정의할 때 전달된 사용자 지정 인수 배열입니다



```sql
CREATE OR REPLACE FUNCTION example_trigger_function()
RETURNS TRIGGER AS $$
BEGIN
    IF TG_OP = 'INSERT' THEN
        RAISE NOTICE 'Triggered by INSERT';
    ELSIF TG_OP = 'UPDATE' THEN
        RAISE NOTICE 'Triggered by UPDATE';
    ELSIF TG_OP = 'DELETE' THEN
        RAISE NOTICE 'Triggered by DELETE';
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```


```sql
CREATE TRIGGER example_trigger
AFTER INSERT OR UPDATE OR DELETE ON your_table
FOR EACH ROW
EXECUTE FUNCTION example_trigger_function();
```


## 스키마 데이터 복제

**중복 키를 무시하고 삽입 (ON CONFLICT DO NOTHING):** PostgreSQL 9.5 이상에서는 `ON CONFLICT` 구문을 사용하여 중복 키가 발생할 경우 무시할 수 있습니다.
```sql
INSERT INTO web.table_name
SELECT * FROM public.table_name
ON CONFLICT (primary_key_column) DO NOTHING;
```

- `primary_key_column`은 해당 테이블의 PK 또는 UNIQUE 제약 조건이 걸려 있는 컬럼입니다.
- 중복된 키가 발생하면 해당 행은 무시되고, 나머지 데이터만 삽입됩니다.

예시

```sql
INSERT INTO web.products
SELECT * FROM public.products
ON CONFLICT (id) DO NOTHING;
```


**중복 데이터를 필터링하여 삽입:** 데이터를 복사하기 전에, `web.table_name`에 이미 존재하는 데이터를 제외하고 삽입할 수 있습니다.

```sql
INSERT INTO web.table_name
SELECT * 
FROM public.table_name
WHERE public.table_name.primary_key_column NOT IN (
    SELECT primary_key_column FROM web.table_name
);
```


**업데이트하거나 삽입 (UPSERT 사용):** 중복 키가 발생하면 데이터를 업데이트하도록 설정할 수도 있습니다.

```sql
INSERT INTO web.table_name
SELECT * FROM public.table_name
ON CONFLICT (primary_key_column) 
DO UPDATE SET column1 = EXCLUDED.column1, column2 = EXCLUDED.column2;
```

- `ON CONFLICT`는 중복된 키가 발생할 경우, `DO UPDATE`를 통해 기존 데이터를 업데이트합니다.
- `EXCLUDED`는 새로 삽입하려는 행의 값을 나타냅니다.


### **2. `WITH ... INSERT / UPDATE` (MERGE와 유사)**

만약 `ON CONFLICT`가 아닌 **좀 더 복잡한 조건을 반영한 `MERGE` 대체 기능**이 필요하다면 `WITH ... INSERT / UPDATE` 구조를 사용할 수 있습니다.


```sql
WITH updated AS (
    UPDATE your_table
    SET name = 'example_updated', value = 200
    WHERE id = 1
    RETURNING *
)
INSERT INTO your_table (id, name, value)
SELECT 1, 'example', 100
WHERE NOT EXISTS (SELECT 1 FROM updated);

```


- 먼저 `UPDATE` 실행 → 조건에 맞는 행을 업데이트 후 `RETURNING *`
- `INSERT` 실행 → 업데이트된 데이터가 없으면 새 데이터 삽입

- `id = 99`가 존재하지 않아서 `UPDATE`가 아무 행도 변경하지 않음
- `updated`가 비어 있음 → `NOT EXISTS (SELECT 1 FROM updated)`는 `TRUE`

- select를 넣은 이유 : where 절로 update를 먼저 실행 시킨후 없을 경우 값 가져오게 하기 위함
- from 이 없어도 실행 가능
- from 이 필요한 경우 : table에서 데이터를 직접 가져와야 할때 ex

```sql
INSERT INTO my_table (id, name)
SELECT some_id, some_name
FROM another_table
WHERE NOT EXISTS (SELECT 1 FROM my_table WHERE my_table.id = another_table.some_id);
```


📌 **이 방식은 `ON CONFLICT`보다 유연하지만 성능이 살짝 떨어질 수 있음**


### **3. `PL/pgSQL`을 활용한 MERGE 함수**

더 복잡한 `MERGE` 로직이 필요하다면 **PL/pgSQL (Stored Procedure)**을 사용해서 직접 `MERGE` 기능을 구현할 수도 있습니다.


```sql
DO $$
BEGIN
    IF EXISTS (SELECT 1 FROM your_table WHERE id = 1) THEN
        UPDATE your_table
        SET name = 'example_updated', value = 200
        WHERE id = 1;
    ELSE
        INSERT INTO your_table (id, name, value)
        VALUES (1, 'example', 100);
    END IF;
END $$;
```


- `IF EXISTS (SELECT 1 FROM your_table WHERE id = 1)` → ID가 존재하면 `UPDATE`
- 존재하지 않으면 `INSERT` 실행



---



**오류를 방지하고 로그 확인:** 중복 데이터만 확인하고 싶다면 `EXCEPT`를 사용하여 어떤 데이터가 충돌하는지 먼저 확인해 볼 수도 있습니다.

```sql
SELECT * 
FROM public.table_name
EXCEPT
SELECT * 
FROM web.table_name;
```



시퀀스 생성


```sql
CREATE SEQUENCE sequence_name
    START WITH start_value
    INCREMENT BY increment_value
    MINVALUE min_value
    MAXVALUE max_value
    CACHE cache_size;


--- 예시
CREATE SEQUENCE my_sequence
    START WITH 1
    INCREMENT BY 1;

---현재 시퀀스 값 조회

SELECT CURRVAL('my_sequence');

---시퀀스 사용 예시

CREATE TABLE my_table (
    id INTEGER DEFAULT NEXTVAL('my_sequence') PRIMARY KEY,
    name VARCHAR(100)
);


---시퀀스 수정

ALTER SEQUENCE my_sequence INCREMENT BY 5;



---시퀀스 삭제

DROP SEQUENCE my_sequence;

```


## SERIAL 데이터 타입


PostgreSQL에서 테이블의 특정 컬럼이 자동으로 1씩 증가하도록 설정하려면, 해당 컬럼에 시퀀스를 사용하여 기본 키를 설정하면 됩니다. 일반적으로 SERIAL 데이터 타입을 사용하여 이 작업을 간편하게 수행할 수 있습니다. SERIAL은 내부적으로 시퀀스를 생성하고, 해당 컬럼에 자동으로 증가하는 값을 할당합니다.


```sql
CREATE TABLE my_table (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100)
);
```


위의 예제에서 id 컬럼은 SERIAL 타입으로 정의되어 있습니다. 이 경우 PostgreSQL은 자동으로 시퀀스를 생성하고, 새로운 행이 삽입될 때마다 id 값이 1씩 증가합니다.

데이터 삽입 예시
이제 my_table에 데이터를 삽입할 때 id 값을 명시하지 않아도 됩니다:


```sql
INSERT INTO my_table (name) VALUES ('Alice');
INSERT INTO my_table (name) VALUES ('Bob');
```


시퀀스 확인
자동으로 생성된 시퀀스의 이름은 일반적으로 table_name_column_name_seq 형식입니다. 예를 들어, 위의 예제에서 생성된 시퀀스는 my_table_id_seq가 됩니다. 이 시퀀스의 현재 값을 확인하려면 다음과 같이 할 수 있습니다:

```sql
SELECT NEXTVAL('my_table_id_seq');
```


2. `GENERATED AS IDENTITY` 사용 (PostgreSQL 10 이상 권장)


```sql
CREATE TABLE example_identity (
    id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY, -- 자동 증가 필드
    name VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```



### 시퀀스 직접 생성 및 연결

직접 시퀀스를 생성하고 테이블 열에 연결할 수도 있습니다.

1. **시퀀스 생성**


```sql
CREATE SEQUENCE custom_sequence START 1 INCREMENT 1;

```


테이블 생성 및 시퀀스 연결
```sql
CREATE TABLE example_custom (
    id INT DEFAULT nextval('custom_sequence') PRIMARY KEY, -- 시퀀스를 기본값으로 사용
    name VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```


시퀀스 값 조정 (선택 사항)

```sql
SELECT setval('custom_sequence', 100); -- 시퀀스 시작 값을 100으로 변경
```


### 차이점

| **특징**         | **SERIAL**             | **GENERATED AS IDENTITY** |
| -------------- | ---------------------- | ------------------------- |
| **시퀀스 생성**     | 자동으로 생성됨               | 자동으로 생성됨                  |
| **시퀀스 이름 지정**  | 수동 제어 가능 (`nextval()`) | 시스템이 완전 관리                |
| **표준 SQL 호환성** | 비표준 (PostgreSQL 전용)    | SQL 표준 준수                 |
| **시퀀스 속성 변경**  | 가능 (`ALTER SEQUENCE`)  | 제한적 (`ALTER TABLE` 필요)    |

### 확인 방법

테이블 생성 후 내부적으로 생성된 시퀀스를 확인하려면 아래 쿼리를 사용할 수 있습니다:

```sql
-- 테이블의 IDENTITY 속성을 확인
SELECT column_name, column_default
FROM information_schema.columns
WHERE table_name = 'example_identity';

-- 관련 시퀀스 확인
SELECT * FROM pg_sequences WHERE sequencename LIKE '%example_identity%';
```

`GENERATED ALWAYS AS IDENTITY`로 생성한 경우에도 PostgreSQL은 내부적으로 시퀀스를 사용하지만, 이를 PostgreSQL이 직접 관리하기 때문에 사용자가 직접 시퀀스를 호출할 필요가 없습니다.



### 1. **테이블 생성 시 시작 번호 설정**

`GENERATED ALWAYS AS IDENTITY` 구문에서 `START WITH`를 사용하면 시작 번호를 지정할 수 있습니다.



```sql
CREATE TABLE example_identity (
    id INT GENERATED ALWAYS AS IDENTITY (START WITH 100 INCREMENT BY 1) PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```


### 2. **기존 테이블의 시작 번호 변경**

이미 생성된 테이블의 IDENTITY 열에 대해 시작 번호를 변경하려면 `ALTER TABLE ... ALTER COLUMN ... RESTART` 명령을 사용합니다.

```sql
ALTER TABLE example_identity ALTER COLUMN id RESTART WITH 100;
```


### 3. **시퀀스 직접 변경 (고급)**

PostgreSQL에서 IDENTITY 열은 내부적으로 시퀀스를 사용하므로, 시퀀스 자체를 직접 변경할 수도 있습니다. 시퀀스 이름은 일반적으로 `[테이블명]_[열명]_seq` 형태입니다


시퀀스 이름 확인
```sql
SELECT sequencename
FROM pg_sequences
WHERE sequencename LIKE '%example_identity%';
```

시퀀스 값 변경
```sql
ALTER SEQUENCE example_identity_id_seq RESTART WITH 100;
```


자기 자신 한테  foreign key 줄수 있음
```sql
CREATE TABLE board (

id SERIAL PRIMARY KEY, -- 고유 식별자

user_id VARCHAR(20) NOT NULL, -- 사용자 ID

title VARCHAR(255), -- 공지 제목

contents TEXT, -- 공지 내용

reg_dt TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- 등록 일시

upt_dt TIMESTAMP, -- 수정 일시

category VARCHAR(50), -- 카테고리

parent_id INT, -- 부모 ID (댓글/대댓글)

CONSTRAINT fk_user FOREIGN KEY (user_id) -- 외래키 제약조건

REFERENCES USERS(user_id) ON DELETE CASCADE,

CONSTRAINT fk_parent FOREIGN KEY (parent_id) -- 자기 참조 외래키

REFERENCES board(id) ON DELETE CASCADE

);
```


부모글 순서 
```sql

SELECT

CASE

WHEN parent_id IS NULL THEN ROW_NUMBER() OVER (

) -- 부모글에 대해서만 번호를 1부터 증가시키기

ELSE NULL -- 자식글은 번호를 부여하지 않음

END AS seq,

a.*

FROM (

SELECT

*

FROM

board

ORDER BY

CASE

WHEN parent_id IS NULL THEN 0 -- 부모글을 우선적으로 정렬

ELSE parent_id -- 자식글은 부모의 id 기준으로 묶여 정렬

END ASC,

reg_dt DESC -- 자식글은 부모 아래에 최신순으로 정렬

) a

ORDER BY

CASE

WHEN parent_id IS NULL THEN id -- 부모 글을 먼저 정렬

ELSE parent_id -- 자식 글은 부모의 id 기준으로 묶여 정렬

END DESC
```



이전행 반환
```sql
WITH CTE AS (
    SELECT 
        bn.*,
        LAG(bn.id) OVER (ORDER BY bn.id) AS previous_id,
        LAG(bn.title) OVER (ORDER BY bn.id) AS previous_title
    FROM 
        board_notice bn
)
SELECT *
FROM CTE
WHERE id = '24'::integer OR id = previous_id;
```



```sql
SELECT 
    bn.*,
    LAG(bn.id) OVER (ORDER BY bn.id) AS previous_id,
    LAG(bn.title) OVER (ORDER BY bn.id) AS previous_title
FROM 
    board_notice bn
WHERE 
    bn.id = '24'::integer;

```



```sql
SELECT *
FROM board_notice
WHERE id = (
    SELECT LAG(id) OVER (ORDER BY id)
    FROM board_notice
    WHERE id = '24'::integer
);
```


- **`LAG` 함수**:
    
    - `LAG(column_name) OVER (ORDER BY column_name)`를 사용하여 이전 행의 값을 가져옵니다.
    - 여기서는 `id`를 기준으로 이전 행의 `id`와 `title`을 가져옵니다.
- **`OVER` 절**:
    
    - `ORDER BY bn.id`를 통해 `id` 순서대로 데이터를 정렬한 후 `LAG`가 이전 데이터를 가져올 수 있도록 설정합니다.


```sql
LAG(bn.id) OVER (ORDER BY bn.id) AS previous_id
```


```sql
COALESCE(LAG(bn.id) OVER (ORDER BY bn.id), 0) AS previous_id
```



다음행 반환

```sql
LEAD(column_name, offset, default_value) 
OVER (PARTITION BY partition_column ORDER BY order_column)
```

- **`column_name`**: 값을 가져올 대상 컬럼입니다.
- **`offset`** _(옵션)_: 가져올 행의 상대적 위치를 설정합니다. 기본값은 1로, 바로 다음 행의 값을 반환합니다.
- **`default_value`** _(옵션)_: 지정된 상대적 위치에 값이 없는 경우 반환할 기본값입니다.
- **`OVER`**: 윈도우(집합) 및 정렬 기준을 지정합니다.
    - **`PARTITION BY`**: 데이터를 파티션(그룹)으로 나눕니다.
    - **`ORDER BY`**: 각 파티션 내에서 정렬 기준을 설정합니다.

### **`LEAD` 예시**

#### 1. 다음 행의 값을 가져오기

```sql
SELECT 
    id, 
    title, 
    LEAD(title) OVER (ORDER BY id) AS next_title
FROM 
    board_notice;
```

- **결과**:
    - 각 행에 대해 `next_title` 컬럼에 다음 행의 제목(`title`)이 표시됩니다.
    - 마지막 행은 다음 행이 없으므로 `NULL`을 반환합니다.

#### 2. 특정 파티션 내에서 다음 행 값 가져오기

```sql
SELECT 
    category_id, 
    id, 
    title, 
    LEAD(title) OVER (PARTITION BY category_id ORDER BY id) AS next_title
FROM 
    board_notice;
```

- **설명**:
    - `category_id`를 기준으로 데이터를 그룹화한 뒤, 각 그룹 내에서 `id` 순으로 정렬하여 다음 행의 `title` 값을 가져옵니다.

#### 3. `offset`과 `default_value` 사용
```sql
SELECT 
    id, 
    title, 
    LEAD(title, 2, 'No Next') OVER (ORDER BY id) AS next_next_title
FROM 
    board_notice;
```

**설명**:

- `offset`이 2로 설정되어 현재 행 기준 2번째 행의 `title` 값을 반환합니다.
- 다음 행이 없으면 기본값 `'No Next'`를 반환합니다.


## TRANSLATE

```sql
SELECT 
    TRANSLATE(a.stock_yn, 'YN', 'OX') AS stock_yn
FROM your_table_name a;
```

- `TRANSLATE` 함수는 첫 번째 인자로 주어진 문자열에서 두 번째 인자에 해당하는 문자를 세 번째 인자에 해당하는 문자로 변환합니다.
- 이 경우, `Y`는 `O`로, `N`은 `X`로 변환됩니다.

이 두 방법은 `CASE`문을 사용하는 것보다 간결하게 변환 작업을 할 수 있습니다.




### **PostgreSQL `RETURNING` 상세 설명**

`RETURNING`은 **PostgreSQL**의 SQL 문법 중 하나로, **`INSERT`, `UPDATE`, `DELETE`**와 함께 사용하여 **영향을 받은 행의 데이터를 바로 반환**하는 기능입니다.


## **1. `RETURNING` 문법의 기본 사용법**

`RETURNING`은 **`INSERT`, `UPDATE`, `DELETE`** 이후 **데이터를 바로 조회**할 수 있습니다.



1.1 `INSERT` 문법 (자동 증가 ID 반환)


```sql
INSERT INTO artists (name, genre) 
VALUES ('John Doe', 'Rock') 
RETURNING id;
```


결과:

```plaintext
id
---
1
```


**설명**:

- `RETURNING id`: 삽입한 행의 `id` 값을 반환.
- `SERIAL`(자동 증가) 컬럼에 유용.


1.2 `UPDATE` 문법 (수정 후 변경된 행 반환)

```sql
UPDATE artists 
SET genre = 'Jazz' 
WHERE name = 'John Doe'
RETURNING *;
```

결과:

```plaintext
id | name     | genre
---+----------+------
1  | John Doe | Jazz
```

**설명**:

- `RETURNING *`: **수정된 전체 행**을 반환.

1.3 `DELETE` 문법 (삭제된 데이터 반환)

```sql
DELETE FROM artists 
WHERE name = 'John Doe' 
RETURNING *;
```


결과:

```plaintext
id | name     | genre
---+----------+------
1  | John Doe | Jazz
```

**설명**:

- `RETURNING *`: **삭제된 행의 모든 데이터**를 반환.


## **2. `RETURNING`의 다양한 활용 예제**

### **2.1 여러 컬럼 반환하기**


```sql
INSERT INTO artists (name, genre) 
VALUES ('Jane Doe', 'Pop') 
RETURNING id, name;
```


결과:


```plaintext
id | name
---+---------
2  | Jane Doe
```


### **2.2 복합 쿼리에서 `RETURNING` 사용**

**서브쿼리에서 `RETURNING` 사용 불가** → 대신 **CTE(Common Table Expression)** 사용 가능


```sql
WITH inserted AS (
    INSERT INTO artists (name, genre)
    VALUES ('Taylor Swift', 'Pop')
    RETURNING id, name
)
SELECT * FROM inserted;
```


결과:

```plaintext
id | name
---+---------
3  | Taylor Swift
```



## 📌 **4. `RETURNING`의 장점과 단점**

### ✅ **장점:**

- **효율적**: `SELECT`를 별도로 실행할 필요 없이, `INSERT/UPDATE/DELETE`와 동시에 데이터 반환.
- **간편함**: 한 번의 쿼리로 데이터 삽입 및 확인 가능.
- **트랜잭션 안정성**: 동일한 트랜잭션 내에서 ID 값을 보장.

### ❌ **단점:**

- **PostgreSQL 전용 기능**: 다른 DBMS(MySQL, Oracle)에서는 지원되지 않을 수 있음.
- **복잡한 서브쿼리 제한**: 일부 복잡한 쿼리에서는 `CTE`를 사용해야 함.

## **5. `RETURNING`과 `LASTVAL()` 비교**

|기능|`RETURNING`|`LASTVAL()`|
|---|---|---|
|**데이터 반환 방식**|`INSERT` 문과 함께 즉시 반환|직전 `SERIAL` 값을 반환|
|**정확성**|✅ 매우 안전 (동일 트랜잭션 내 정확)|❌ 다중 쓰레드 환경에서 부정확할 수 있음|
|**사용법**|`INSERT INTO ... RETURNING id`|`SELECT LASTVAL()`|
|**지원 DB**|PostgreSQL 전용|PostgreSQL 전용|
|**추천 상황**|✅ 권장 방식 (더 안전함)|❌ 사용 주의 필요|


update returing

```sql
UPDATE
products
SET
product_nm = # {
        productNm
    },
    artist_id = # {
        artistId
    }::numeric,
    company_id = # {
        companyId
    }::numeric,
    label_id = # {
        labelId
    }::numeric,
    category_id = # {
        categoryId
    }::numeric,
    media_id = # {
        mediaId
    }::numeric,
    order_st_dt = # {
        orderStDt
    }::timestamp,
    order_ed_dt = # {
        orderEdDt
    }::timestamp,
    new_item_st_dt = # {
        newItemStDt
    }::timestamp,
    new_item_ed_dt = # {
        newItemEdDt
    }::timestamp,
    search_st_dt = # {
        searchStDt
    }::timestamp,
    search_ed_dt = # {
        searchEdDt
    }::timestamp,
    product_cd = # {
        productCd
    },
    barcode = # {
        barcode
    },
    sellcode = # {
        sellcode
    },
    special_code = # {
        specialCode
    },
    factory_price = REPLACE(# {
        factoryPrice
    }, ',', '')::numeric,
    weight = REPLACE(# {
        weight
    }, ',', '')::numeric,
    search_cd = # {
        searchCd
    },
    discount_yn = # {
        discountYn
    },
    refund_yn = # {
        refundYn
    },
    tax_yn = # {
        taxYn
    },
    stock_yn = # {
        stockYn
    },
    import_yn = # {
        importYn
    },
    description = # {
        description
    },
    product_cnt = # {
        productCnt
    }::numeric,
    reg_dt = now(),
    reg_id = # {
        regId
    }
WHERE
id = # {
    pk_ID
}::numeric
RETURNING id
```

update에서 사용 가능



array로 묶는 쿼리문
```sql
SELECT

a_1.product_id,

a_1.product_name,

CASE

WHEN array_length(a_1.tag_array, 1) >= 1 THEN a_1.tag_array[1]

ELSE NULL::character varying

END AS tag_1,

CASE

WHEN array_length(a_1.tag_id_array, 1) >= 1 THEN a_1.tag_id_array[1]

ELSE NULL::integer

END AS tag_1_id,

CASE

WHEN array_length(a_1.tag_array, 1) >= 2 THEN a_1.tag_array[2]

ELSE NULL::character varying

END AS tag_2,

CASE

WHEN array_length(a_1.tag_id_array, 1) >= 2 THEN a_1.tag_id_array[2]

ELSE NULL::integer

END AS tag_2_id

FROM (

SELECT

p.id AS product_id,

p.product_nm AS product_name,

array_agg(t.tag_nm ORDER BY t.tag_nm) AS tag_array,

array_agg(t.id ORDER BY t.tag_nm) AS tag_id_array

FROM

products p

LEFT JOIN

product_tags pt ON p.id = pt.product_id

LEFT JOIN

tags t ON pt.tag_id = t.id

GROUP BY

p.id, p.product_nm

) a_1;
```


## WITH RECURSIVE

재귀적 조인



## 초성 추출

초성 추출

function 생성
```sql
CREATE OR REPLACE FUNCTION public.fn_get_chosung(text)
 RETURNS text
 LANGUAGE plpgsql
AS $function$
DECLARE
    v_text ALIAS FOR $1;
    v_char text;
    v_result text := '';
    v_uni int;
    v_initial text;
    v_vowel text;
    
    -- 초성 리스트
    v_initials text[] := ARRAY['ㄱ', 'ㄲ', 'ㄴ', 'ㄷ', 'ㄸ', 'ㄹ', 'ㅁ', 'ㅂ', 'ㅃ', 'ㅅ', 'ㅆ', 'ㅇ', 'ㅈ', 'ㅉ', 'ㅊ', 'ㅋ', 'ㅌ', 'ㅍ', 'ㅎ'];
    
    -- 중성 리스트
    v_vowels text[] := ARRAY['ㅏ', 'ㅐ', 'ㅑ', 'ㅒ', 'ㅓ', 'ㅔ', 'ㅕ', 'ㅖ', 'ㅗ', 'ㅘ', 'ㅙ', 'ㅚ', 'ㅛ', 'ㅜ', 'ㅝ', 'ㅞ', 'ㅟ', 'ㅠ', 'ㅡ', 'ㅢ', 'ㅣ'];
    
BEGIN
    IF v_text IS NULL OR char_length(v_text) = 0 THEN
        RETURN NULL; -- 입력값이 NULL이거나 빈 문자열이면 NULL 반환
    END IF;

    FOR i IN 1..char_length(v_text) LOOP
        v_char := substring(v_text FROM i FOR 1);
        
        -- 한글인지 확인
        IF v_char >= '가' AND v_char <= '힣' THEN
            v_uni := ascii(v_char) - 44032; -- 유니코드 기준 '가'를 0으로 정규화
            v_initial := v_initials[(v_uni / 588) + 1]; -- 초성 추출
            v_vowel := v_vowels[((v_uni % 588) / 28) + 1]; -- 중성 추출

            v_result := v_result || v_initial || v_vowel;
        
        -- 한글 자음 (초성만 있는 경우)
        ELSIF v_char >= 'ㄱ' AND v_char <= 'ㅎ' THEN
            v_result := v_result || v_char;
            
        -- 한글 모음 (단독 모음)
        ELSIF v_char >= 'ㅏ' AND v_char <= 'ㅣ' THEN
            v_result := v_result || v_char;
            
        -- 영문 대문자 변환
        ELSIF v_char ~ '[a-zA-Z]' THEN
            v_result := v_result || upper(v_char);
            
        -- 숫자는 그대로 유지
        ELSIF v_char >= '0' AND v_char <= '9' THEN
            v_result := v_result || v_char;
            
        -- 그 외 문자 유지
        ELSE
            v_result := v_result || v_char;
        END IF;
    END LOOP;
    
    RETURN v_result;
END;
$function$;

```


모든 컬럼에서 검색 하는 방법

```sql
tag_1_id, tag_2_id, tag_3_id, tag_4_id, tag_5_id, tag_6_id, tag_7_id, tag_8_id, tag_9_id, tag_10_id
```



**1. `ARRAY`를 활용한 간단한 태그 검색**

```sql
SELECT * 
FROM product_info 
WHERE ARRAY[
    tag_1, tag_2, tag_3, tag_4, tag_5,
    tag_6, tag_7, tag_8, tag_9, tag_10
] @> ARRAY['검색할 태그'];
```


**📌 설명**

- `ARRAY[...]` 안에 `tag_1 ~ tag_10`을 넣어서 배열로 변환
- `@>` 연산자를 사용하여 `'검색할 태그'`가 배열 안에 포함되어 있는지 검사
- 한 줄만 추가하면 여러 태그 컬럼을 동시에 검색할 수 있음


🔹 **2. `UNNEST()`를 활용한 더 강력한 검색**


```sql
SELECT * 
FROM product_info, 
LATERAL UNNEST(ARRAY[
    tag_1, tag_2, tag_3, tag_4, tag_5,
    tag_6, tag_7, tag_8, tag_9, tag_10
]) AS tag_name
WHERE tag_name = '검색할 태그';
```


- `LATERAL UNNEST(ARRAY[...])`를 사용하면 각 행을 여러 개의 행으로 변환
- `WHERE tag_name = '검색할 태그'`를 적용하면 하나라도 일치하는 제품을 검색할 수 있음
- **이 방법은 가독성이 뛰어나고 유지보수하기 쉬움**


🔹 **3. 태그 ID로 검색하기 (더 간단하게)**

```sql
SELECT * 
FROM product_info 
WHERE ARRAY[
    tag_1_id, tag_2_id, tag_3_id, tag_4_id, tag_5_id,
    tag_6_id, tag_7_id, tag_8_id, tag_9_id, tag_10_id
] @> ARRAY[123];
```


🔹 **4. 여러 개의 태그 검색 (OR 조건 적용)**


```sql
SELECT * 
FROM product_info
WHERE ARRAY[
    tag_1, tag_2, tag_3, tag_4, tag_5,
    tag_6, tag_7, tag_8, tag_9, tag_10
] && ARRAY['태그1', '태그2'];
```


```sql
SELECT *

FROM product_info

WHERE product_cd = '1'

AND ARRAY[

tag_1, tag_2, tag_3, tag_4, tag_5,

tag_6, tag_7, tag_8, tag_9, tag_10

]::TEXT[] &&

STRING_TO_ARRAY('r1,댄스', ',')::TEXT[]
```

- `&&` 연산자는 **배열 간의 교집합을 검사**
- 즉, `'태그1'` 또는 `'태그2'`를 포함하는 제품을 검색할 수 있음

`STRING_TO_ARRAY()` 함수는 **PostgreSQL에서 문자열을 특정 구분자로 분할하여 배열로 변환하는 함수**입니다.


```sql
STRING_TO_ARRAY(text, delimiter)
```

- `text`: 변환할 문자열
- `delimiter`: 문자열을 나눌 기준이 되는 구분자

실제 동작 예시

```sql
SELECT STRING_TO_ARRAY('신,r1,kpop', ',');
```

🔹 **결과**
```bash
{신,r1,kpop}
```

**`::TEXT[]`가 필요한가?**

`STRING_TO_ARRAY()`의 결과는 **TEXT[] 타입**이므로, 비교 연산을 위해 `::TEXT[]` 형 변환을 해줘야 합니다.

```sql
STRING_TO_ARRAY('신,r1,kpop', ',')::TEXT[]
```


- `::TEXT[]`를 붙이지 않으면 PostgreSQL이 배열 타입으로 인식하지 못할 수 있습니다.
- **형 변환을 통해 `ARRAY && ARRAY` 연산이 가능하도록 만듭니다.**





### **결론**

| 방법                 | 코드                                        | 장점                 | 단점                   |
| ------------------ | ----------------------------------------- | ------------------ | -------------------- |
| `ARRAY @>`         | `WHERE ARRAY[...] @> ARRAY['검색어']`        | 간결하고 빠름            | 추가 필터링이 어려울 수 있음     |
| `UNNEST()`         | `LATERAL UNNEST(ARRAY[...]) AS tag_name`  | SQL이 가독성이 높고 확장 가능 | 성능이 상대적으로 낮을 수 있음    |
| `ARRAY @> (ID 검색)` | `WHERE ARRAY[...] @> ARRAY[태그 ID]`        | 태그 ID 검색 가능        | 태그 이름 검색에는 적절하지 않음   |
| `ARRAY && (OR 검색)` | `WHERE ARRAY[...] && ARRAY['태그1', '태그2']` | 여러 개의 태그를 검색 가능    | 태그 수가 많을 경우 성능 저하 가능 |

## 단어 단위 검색



`to_tsvector` + `plainto_tsquery` 사용 예제:

```sql
SELECT * FROM board_category
WHERE to_tsvector('simple', kr_name || ' ' || en_name) @@ plainto_tsquery('검색어');
```

- `"검색어"`뿐만 아니라 `"검색"`, `"검색하는"`, `"검색했다"` 등 변형된 형태도 검색 가능.
- 검색어를 **단어 단위로 파싱**하여 빠르게 찾음.
- `"검색어 예제"`를 검색하면 **"검색어" 또는 "예제"가 포함된 레코드**를 반환할 수도 있음.


### `to_tsvector('simple', ...)`에서 `'simple'`이 의미하는 것

PostgreSQL의 `to_tsvector()` 함수에서 첫 번째 인자로 들어가는 문자열(`'simple'`)은 **텍스트 검색 설정 (Text Search Configuration)** 을 지정하는 것입니다.


## **1. `simple`은 어떤 역할을 하는가?**

- `'simple'`은 **언어 설정이 없는 기본 토큰화 방식**을 의미합니다.
- 즉, **형태소 분석 없이 공백 및 특수문자를 기준으로 단순하게 단어를 분리**합니다.
- **불용어(stop words)를 제거하지 않고, 단순한 단어 검색만 수행**합니다.

### 📌 `simple` 설정의 동작 방식


```sql
SELECT to_tsvector('simple', 'The quick brown fox jumps over the lazy dog.');
```

결과 :

```bash
'the' 'quick' 'brown' 'fox' 'jumps' 'over' 'lazy' 'dog'
```

- 모든 단어가 그대로 유지됨 (영어 불용어 제거 안됨).
- `'quick'`과 `'quickly'`는 다르게 취급됨 (형태소 분석 없음).

## **2. 다른 언어 설정과 비교 (`english` vs `simple`)**

### 📌 `english` 설정을 사용한 경우


```sql
SELECT to_tsvector('english', 'The quick brown fox jumps over the lazy dog.');
```

결과:
```bash
'quick' 'brown' 'fox' 'jump' 'lazi' 'dog'
```

- **불용어 (`the`, `over`) 제거됨.**
- **형태소 분석 적용됨 (`jumps` → `jump`, `lazy` → `lazi`)**.

### 📌 `simple` vs `english` 비교


|설정 값|불용어 제거|형태소 분석|검색 방식|
|---|---|---|---|
|`simple`|❌ X|❌ X|단순한 토큰화|
|`english`|✅ O|✅ O|단어 정규화 및 불용어 제거|

✅ **즉, `simple`은 입력된 텍스트를 단순히 단어 단위로 분리하지만, `english` 같은 언어 설정은 단어를 정규화하고 불용어를 제거하여 검색 정확도를 높입니다.**


### 한글과 영어가 섞여 있는 경우 (`kr_name || ' ' || en_name`)

1. **`simple` 사용 시**

```sql
SELECT to_tsvector('simple', '안녕하세요 Hello World');
```

결과:

```bash
'안녕하세요' 'hello' 'world'
```

- - 한글과 영어가 그대로 분리됨 (형태소 분석 없음).
    - `"안녕"`을 검색해도 `"안녕하세요"`를 찾지 못함.
- **`english` 사용 시**

```sql
SELECT to_tsvector('english', 'The running man was jumping.');
```


결과:

```sql
'run' 'man' 'jump'
```

- - `running` → `run`, `jumping` → `jump` (형태소 분석됨).
- **`simple`과 `english`를 혼합해야 하나?**
    
    - **한글 검색을 위해서는 `simple`보다 `kr`(한국어 형태소 분석 지원) 설정이 필요**합니다.
    - PostgreSQL 기본 설정에는 `kr`이 없으므로 별도의 **한국어 텍스트 검색 설정**을 추가해야 합니다.
    - 하지만 한글과 영어를 동시에 다루려면 보통 `simple`을 쓰는 것이 무난합니다.





---
출처 - https://yeongunheo.tistory.com/entry/PostgreSQL-json-jsonb-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%97%B0%EC%82%B0%EC%9E%90#--%--json%--vs%--jsonb%--%ED%--%--%EC%-E%--
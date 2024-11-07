
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


---
출처 - https://yeongunheo.tistory.com/entry/PostgreSQL-json-jsonb-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%97%B0%EC%82%B0%EC%9E%90#--%--json%--vs%--jsonb%--%ED%--%--%EC%-E%--
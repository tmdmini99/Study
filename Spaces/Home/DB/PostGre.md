
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
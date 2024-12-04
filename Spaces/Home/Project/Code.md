


### **쿼리 결과 해석**

#### 데이터 예시

|id|parent_id|title|content|writer|created_at|
|---|---|---|---|---|---|
|1|NULL|"게시글1 제목"|"게시글1 내용"|"작성자1"|2024-12-03 12:00|
|2|1|NULL|"댓글1 내용"|"작성자2"|2024-12-03 12:05|
|3|1|NULL|"댓글2 내용"|"작성자3"|2024-12-03 12:10|
|4|2|NULL|"대댓글1-1 내용"|"작성자4"|2024-12-03 12:15|
|5|NULL|"게시글2 제목"|"게시글2 내용"|"작성자5"|2024-12-03 12:20|
|6|5|NULL|"댓글1 내용"|"작성자6"|2024-12-03 12:25


```sql
SELECT 
    id,
    parent_id,
    title,
    content,
    writer,
    created_at
FROM 
    board
ORDER BY 
    CASE 
        WHEN parent_id IS NULL THEN id   -- 게시글은 id 기준으로 정렬
        ELSE parent_id                  -- 댓글/대댓글은 parent_id 기준으로 정렬
    END ASC, 
    created_at ASC;                      -- 동일한 부모 아래 작성 시간 순으로 정렬

```


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



```sql
SELECT  
   CASE       WHEN parent_id IS NULL THEN ROW_NUMBER() OVER (       )  -- 부모글에 대해서만 번호를 1부터 증가시키기  
       ELSE NULL  -- 자식글은 번호를 부여하지 않음  
   END AS seq,  
   a.*FROM (  
   SELECT       *   FROM       board   ORDER BY       CASE           WHEN parent_id IS NULL THEN 0  -- 부모글을 우선적으로 정렬  
           ELSE parent_id  -- 자식글은 부모의 id 기준으로 묶여 정렬  
       END ASC,  
       reg_dt DESC  -- 자식글은 부모 아래에 최신순으로 정렬  
) a  
ORDER BY  
       CASE           WHEN parent_id IS NULL THEN id   -- 부모 글을 먼저 정렬  
           ELSE parent_id  -- 자식 글은 부모의 id 기준으로 묶여 정렬  
       END DESC,  
        CASE       WHEN parent_id IS NOT NULL THEN reg_dt   -- 자식글은 등록일시로 정렬  
       ELSE NULL  -- 부모글에는 영향을 미치지 않음  
   END DESC
```


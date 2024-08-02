
## 0. with 절이란?

### 0.1. with 절 사용방법

with절은 SQL 에서 쿼리를 작성할 때 하나의 서브쿼리 또는 임시 테이블처럼 활용할 수 있는 기능입니다.

with 절의 사용 구문을 살펴보면 아래와 같습니다.
메인 쿼리 위에 WITH AS 구문을 사용해 서브 쿼리를 옮겨 놓는 것이다.   



```sql
WITH 이름 AS (
	SELECT ...
    FROM ...
    WHERE ...
)
SELECT
	....
FROM 이름
WHERE
	....

```

서브쿼리가 여러개라 하더라도 걱정할 필요 없다.

각 서브쿼리를 , 로 구분하여 추가해 주면 된다.

```sql
WITH 이름1 AS (
	SELECT 문
), 
이름2 AS (
	SELECT 문
)
SELECT
	...
FROM 이름1
	LEFT OUTER JOIN 이름2
      ON ....
WHERE
	....
```

**< with 절을 사용하지 않은 경우 >**

```sql
-- 각 cust_id 별 총 주문 합계 구하기 (with 절 사용 X)
select a.cust_id, 
	   seq, 
	   price, 
	   tot_amt 
from receipts a	
join (		
		select cust_id, 
			   sum(seq*price) as tot_amt		
	    from receipts		
	    group by 1	) b	on a.cust_id = b.cust_id;
```

**< with 절을 사용한 경우 >**

```sql
-- 각 cust_id 별 총 주문 합계 구하기 (with 절 사용 O)
with tot_amt as (	
	select cust_id, 
		   sum(seq*price) as tot_amt	
    from receipts	group by 1)  
select a.cust_id, 
	   seq, 
	   price, 
	   tot_amt
from receipts a	join tot_amt b on a.cust_id = b.cust_id;
```

이렇게 with절을 활용해 tot_amt 라는 이름으로 쿼리를 정의한 후 이를 활용해서 본절을 작성합니다.

### 0.2. with 절의 장점

with절을 어떻게 사용하는지 알아봤으니 이제 이를 사용하는 이유에 대해 알아봅시다.

#### 1) 코드의 가독성을 높여준다

우선 제가 생각하는 with절의 가장 직관적인 장점은 **코드의 가독성을 높여준다**는 점입니다.

쿼리를 작성할 때 서브쿼리가 여러번 사용되면 자연스레 쿼리가 길어지고 가독성이 떨어지게 되는데요.

그렇게 되면 각 서브쿼리가 어떤 목적으로, 어떤 의미로 사용되었는지 알기 어렵다는 문제가 발생합니다.

with 절을 활용한다면 이러한 문제를 해결할 수 있다는 장점이 있습니다.

만약 서브쿼리가 많아지더라도 아래와 같이 with절을 여러번 사용함으로써 가독성 문제를 해소할 수 있죠.

```sql
with {
    테이블 명_1
}
as(with절로 저장하고 싶은 SQL 쿼리문 1), {
    테이블 명_2
}
as(with절로 저장하고 싶은 SQL 쿼리문 2), {
    테이블 명_3
}
as(with절로 저장하고 싶은 SQL 쿼리문 3) select * from {
    with절로 저장한 테이블명_1
}
join {
    with절로 저장한 테이블명_2
}
left join {
    with절로 저장한 테이블명_3
};
```

하지만 이 장점만을 위해 with절을 사용하는 것은 with절을 제대로 활용하지 못하는 것이라고 생각될만큼 with 절의 큰 장점은 따로 있습니다.

#### 2) SQL의 성능을 개선시킨다.

with절은 듣기만 해도 설레는(?) SQL 성능 개선이라는 큰 장점을 가지고 있습니다.

서브쿼리와 비슷한 기능을 한다고 하는데 어떻게 with절을 활용해서 어떻게 쿼리의 성능을 높일 수 있을까요?

이는 바로 with 절의 동작 방식 때문에 발생하는 장점입니다.

보다 상세한 이해를 위해 with절의 동작 방식에 대해 알아보도록 합시다.

---

## 1. with 절의 동작 방식

with 절의 동작은 크게 2가지 방식으로 구분됩니다.

#### 1) inline view 방식

inline view 방식은 with 절에 포함된 쿼리에 대해 물리적으로 임시 테이블을 생성하지 않고. **쿼리 그 자체로 저장해두는 방식**입니다.

즉, with 절에서 사용된 쿼리의 결과가 temp table 로 저장되지 않는다는 의미입니다.

그렇기 때문에 with 절로 정의된 테이블이 참조된 횟수만큼 반복 수행됩니다.

![[WITH AS1.png]]


위의 예시처럼 tot_amt 로 저장된 쿼리가 그대로 입력되어 쿼리가 실행된다는 이야기입니다.

주로 이 방식은 with절이 본절에서 1번 사용될 때 활용되는 방식이며, 이러한 동작 방식은 서브쿼리와 다를 것 없는 성능을 보여줍니다.

#### 2) Materialize 방식

materialize 방식은 내부적으로 메모리에 임시 테이블을 생성함으로써 쿼리의 결과값을 저장하고, 반복해서 재사용하는 방식입니다.

이 방식은 with절로 정의된 테이블이 2번 이상 사용될 때 활용되며, 이러한 경우에 성능 개선의 효과를 보이는데요.

2번 이상 서브쿼리로 돌아갈 쿼리 대신 with절을 통해 저장된 결과값을 불러와 실행 횟수를 줄여줌으로써 성능의 개선을 이루어냅니다.

예를 들어보겠습니다.

> 예시)  
> receipts 테이블과 companies 테이블을 join 하여 companies 테이블에 있는 co_cd 를 기준으로 집계하기

**< with절을 사용하지 않은 경우 >**

```sql
	select '001&003' as co_cd, 
		    sum(seq * price) as tot_amt 
	from receipts a 
	join companies b on a.cust_id = b.district where co_cd in ('001', '00t3') 
	group by 1 
	
	union all 
	
	select '002&004' as co_cd, 
			sum(seq * price) as tot_amt 
	from receipts a 
	join companies b on a.cus_id = b.district where co_cd in ('002', '004') 
	group by 1;
```

**< with절을 사용한 경우 >**

```sql
WITH join_table
     AS (SELECT a.*,
                seq * price AS tot_amt,
                b.co_cd
         FROM   receipts a
                join companies b
                  ON a.cust_id = b.district) SELECT '001&003'    AS co_cd,
       SUM(tot_amt) AS tot_amt
FROM   join_table
WHERE  co_cd IN ( '001', '003' )
GROUP  BY 1
UNION ALL
SELECT '002&004'    AS co_cd,
       SUM(tot_amt) AS tot_amt
FROM   join_table
WHERE  co_cd IN ( '002', '004' )
GROUP  BY 1;
```

with 절을 사용하지 않은 경우에는 receipts 테이블과 companies 테이블을 조인한 서브쿼리를 2번 사용하게 되는데요.

이와 달리 with절을 사용한 경우 join을 시켜둔 테이블의 결과값을 사용하게 되어 실행 횟수를 2번에서 1번으로 줄일 수 있습니다.

이러한 동작 방식의 특성 덕분에 with 절을 활용하여 성능의 개선을 이루어낼 수 있는것이죠.

---

## 2. with절 효율적으로 활용하기

이렇게 with 절을 materialize 방식으로 활용하면 성능 개선을 이루어낼 수 있다는 것을 알았습니다.

그리고 inline view 방식으로 활용되더라도 성능 개선의 효과는 없어도 쿼리의 가독성을 높여주니 'with절을 활용하는 것은 어찌됐건 좋은 선택이다' 라고 생각할 수 있습니다. 

하지만 언제나 그렇듯 SQL 에서는 늘 좋은 방식이란 존재하지 않고 이는 with 절도 피해갈 수 없었습니다.

그러니 습관적으로 with절을 사용하기보다는 효율적으로 with절을 활용할 수 있도록 고민을 할 필요가 있습니다.

with절을 활용할 때 고려해야할 부분은 아래와 같습니다.

#### 1) 쿼리 실행 시 I/O 비용이 많이 들지만, 결과 row수가 적은 경우 사용하기

이 부분은 with절의 동작 방식에 대해 이해했다면 어느정도 자연스레 받아들여지는 부분이라고 생각이 됩니다.

with 절의 큰 강점인 materalize 방식은 with절에서 사용된 쿼리의 결과를 임시 테이블에 저장하게 되고, 그 후로 저장된 테이블 값을 불러와 활용하게 됩니다.

그렇기 때문에 여러번의 JOIN 을 통해 cost가 많이 드는 쿼리이지만 결과 row가 적은 경우 아주 큰 효율을 보여줄 수 있습니다.

한 번만 cost를 감당하면 그 후로 결과값을 반복해서 사용할 수 있기 때문이죠.

#### 2)  결과 rows 가 많은 경우에는 materalize 동작 방식은 비효율적일 수 있다.

materalize 방식은 장점만 가득해 보이지만, 사실 시스템에 부하를 줄 수 있는 방식이라고 합니다.

왜냐하면 with절을 통해 임시 테이블을 create 하고 drop 하는 행위를 반복하기 때문입니다.

만약 with절에서 사용된 쿼리의 결과 rows 수가 많다면(몇 만건 이상), 해당 데이터들을 임시 테이블로 저장하는 과정에서 시스템 부하를 줄 수 있다고 합니다.

위 두 Case 를 하나로 묶어 핵심을 요약하자면 아래와 같이 이야기할 수 있습니다.

> with절 내 쿼리를 추출하는데 많은 시간이 소요되지만, 추출 결과가 몇 건 안되어  
> Global Tempory Table 에 저장하고 불러오는 작업량이 적은 경우에 with절을 사용한다.


---
출처 - https://schatz37.tistory.com/46
https://freehoon.tistory.com/188
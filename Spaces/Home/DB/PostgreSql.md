처음 도커로 접속시 

```
docker run --name my-postgres -e POSTGRES_PASSWORD=mysecretpassword -p 5432:5432 -d postgres

docker exec -it my-postgres bash

psql -U postgres

ALTER USER postgres PASSWORD '새로운비밀번호';

```
실행



`SELECT <columns> FROM <table>;` SELECT의 기본 형식

- ex) SELECT actor_id, first_name FROM actor;

- - 쿼리 실행결과 (200개 행)  
        
- `SELECT <columns> FROM <table> LIMIT <int>;` 개만큼 데이터를 조회한다. (위에서부터)
    
    - ex) SELECT actor_id, first_name FROM actor LIMIT 4;

- - 쿼리 실행결과 (4개 행)  
        
- `SELECT <columns> FROM <table> WHERE <조건>;` 조건에 맞는 데이터를 조회한다.
    
    - ex) SELECT * FROM customer WHERE store_id=2 and customer_id=400;

- - store_id = 2 이고, customer_id = 400 인 데이터 (1행)  
        
- `SELECT <columns> FROM <table> WEHRE <조건> BETWEEN <범위1> AND <범위2>;` 범위1과 범위2사이 데이터 출력
    
- `SELECT <columns> FROM <table> WEHRE <조건> IN <범위1>;` 범위1안에 들어있는 데이터 출력
    
- `SELECT <columns> FROM <table> WEHRE <조건> IS NULL;` <조건>이 NULL인 데이터 출력
    
    - ex) SELECT * FROM address WHERE address2 IS NULL;


- - address2가 NULL값을 가진 4개의 데이터행  
        
- `SELECT <columns> FROM <table> WEHRE <조건> IS NOT NULL;` <조건>이 NULL이 아닌데이터 출력
    
- `SELECT <columns> FROM <table> ORDER BY <colomn name> DESC;` 데이터를 내림차순으로 정렬
    
    - ex) SELECT film_id, title FROM film ORDER BY film_id DESC LIMIT 4;


- - film_id를 기준으로 내림차순으로 출력된 데이터  
        
- `SELECT DISTINCT <columns> FROM <table>;` 결과에서 중복 행을 제거. 모든열의 조합에 적용
    
    - ex) SELECT DISTINCT customer_id, staff_id FROM payment;


- - 중복이 모두 제거된 amount  
        
- `SELECT min(<columns>) FROM <table>;` 최소값을 얻는다
    
- `SELECT max(<columns>) FROM <table>;` 최대값을 얻는다
    
    - ex) **`SELECT** **max**(amount) **FROM** payment **WHERE** amount **<** 4;`

---
참조 - https://blog.naver.com/wiseyoun07/221146431131 upsert
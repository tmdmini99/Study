
## pg 라이브러리 설치

cmd창에서 pg 라이브러리 설치

명령어
```
npm install pg
```

![[Pasted image 20240905150108.png]]

![[Pasted image 20240905150849.png]]

인텔리 제이 프로젝트 일시 프로젝트 위치로 가서 실행

확인 하는법
![[Pasted image 20240905151225.png]]


PostgreSQL 연결 코드 작성


```js
const { Pool } = require('pg');

// PostgreSQL 설정 정보
const pool = new Pool({
  user: 'postgres',       // PostgreSQL 사용자 이름
  host: 'localhost',      // PostgreSQL 서버 호스트
  database: 'mydatabase', // 연결할 데이터베이스 이름
  password: 'mypassword', // PostgreSQL 비밀번호
  port: 5432,             // PostgreSQL 서버 포트 (기본값 5432)
});

// 연결 및 쿼리 예제
pool.query('SELECT NOW()', (err, res) => {
  if (err) {
    console.error('Error executing query', err.stack);
  } else {
    console.log('Current Time:', res.rows[0]);
  }
  pool.end(); // 연결 종료
});

```


여기서 `Pool` 객체는 데이터베이스에 대한 연결 풀을 관리합니다. 이를 통해 여러 클라이언트가 동시에 데이터베이스에 연결할 수 있습니다.

### 3. **PostgreSQL 설정**

`Pool` 객체를 생성할 때 다음과 같은 기본 설정을 지정해야 합니다:

- `user`: PostgreSQL 사용자의 이름 (보통 `postgres`)
- `host`: 데이터베이스 서버가 실행되는 호스트 (로컬 개발 환경에서는 `localhost`)
- `database`: 연결하려는 데이터베이스의 이름
- `password`: PostgreSQL 계정의 비밀번호
- `port`: PostgreSQL 서버의 포트 (기본값은 5432)


데이터 조회
```js
pool.query('SELECT * FROM users', (err, res) => {
  if (err) {
    console.error(err);
  } else {
    console.log(res.rows); // 결과 출력
  }
});

```

데이터 삽입
```js
pool.query('INSERT INTO users(name, age) VALUES($1, $2)', ['Alice', 25], (err, res) => {
  if (err) {
    console.error(err);
  } else {
    console.log('Data inserted successfully');
  }
});

```



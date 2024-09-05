
## pg 라이브러리 설치

cmd창에서 pg 라이브러리 설치

명령어
```
npm install pg
```

![[Pasted image 20240905150108.png]]

![[Pasted image 20240905150849.png]]

인텔리 제이 프로젝트 일시 프로젝트 위치로 가서 실행

```
npm install axios

```
도 같이 실행
- **API 요청**: 외부 API나 서버에 GET, POST, PUT, DELETE 등의 HTTP 요청을 보낼 때 사용합니다.
- **데이터 가져오기**: 웹 페이지나 애플리케이션에서 서버로부터 데이터를 가져와야 할 때 사용합니다.
- **서버와의 통신**: 서버와 클라이언트 간에 데이터 전송 및 수신을 처리합니다.

확인 하는법
![[Pasted image 20240905151225.png]]


PostgreSQL 연결 코드 작성


```js
// db.js  
const { Pool } = require('pg');  
  
const pool = new Pool({  
  user: 'postgres',  
  host: 'localhost',  
  database: 'postgres',  
  password: 'postgres',  
  port: 5432,  
});  
  
module.exports = pool;

```


여기서 `Pool` 객체는 데이터베이스에 대한 연결 풀을 관리합니다. 이를 통해 여러 클라이언트가 동시에 데이터베이스에 연결할 수 있습니다.

연동후 api를 통해서 데이터 INSERT와 select 
```js
const express = require('express');  
const router = express.Router();  
const axios = require('axios');  
const pool = require('./db'); // db.js  
  
// 유틸리티 함수: 빈 문자열을 null로 변환  
const normalizeProductData = (product) => {  
  const normalized = {};  
  for (const key in product) {  
    normalized[key] = product[key] === "" ? null : product[key];  
  }  
  return normalized;  
};  
  
// API에서 데이터를 가져와서 데이터베이스에 삽입  
const options = {  
  method: 'GET',  
  url: 'https://tmdals23222.cafe24api.com/api/v2/admin/products',  
  headers: {  
    'Authorization': 'Bearer 3wuvw8foFaRW9FmfHtMLaI',  
    'Content-Type': 'application/json',  
    'X-Cafe24-Api-Version': '2024-06-01'  
  }  
};  
  
axios(options)  
  .then(async function (response) {  
    const products = response.data.products;  
  
    try {  
      for (const product of products) {  
        const normalizedProduct = normalizeProductData(product);  
        const keys = Object.keys(normalizedProduct);  
        const values = keys.map(key => normalizedProduct[key]);  
        const columns = keys.join(', ');  
        const placeholders = keys.map((_, i) => `$${i + 1}`).join(', ');  
  
        const query = `INSERT INTO products (${columns}) VALUES (${placeholders})  
                       ON CONFLICT (product_code) DO NOTHING`;  
  
        await pool.query(query, values);  
      }  
      console.log('Products inserted successfully.');  
    } catch (err) {  
      console.error('Error inserting products:', err.stack);  
    }  
  })  
  .catch(function (error) {  
    console.error('Error making API request:', error);  
  });  
  
// 제품 목록을 데이터베이스에서 가져와서 EJS 뷰에 전달  
router.get('/', async function(req, res, next) {  
  try {  
    const result = await pool.query('SELECT product_no, product_name, price FROM products');  
    const products = result.rows;  
    res.render('index', { title: 'Product List', products });  
  } catch (err) {  
    console.error('Error executing query', err.stack);  
    next(err);  
  }  
});  
  
module.exports = router;
```

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



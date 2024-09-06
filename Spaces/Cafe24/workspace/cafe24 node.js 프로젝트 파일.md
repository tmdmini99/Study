
index.js
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

    'Authorization': 'Bearer yeg3oOcgdSpDfG1hDsDKNM',

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


db.js
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

index.ejs
```html
<!DOCTYPE html>

<html>

<head>

  <title><%= title %></title>

</head>

<body>

  <h1><%= title %></h1>

  <table>

    <thead>

      <tr>

        <th>Product No</th>

        <th>Product Name</th>

        <th>Price</th>

        <!-- 추가적인 열을 여기서 정의합니다 -->

      </tr>

    </thead>

    <tbody>

      <% products.forEach(product => { %>

        <tr>

          <td><%= product.product_no %></td>

          <td><%= product.product_name %></td>

          <td><%= product.price %></td>

          <!-- 추가적인 열을 여기서 표시합니다 -->

        </tr>

      <% }) %>

    </tbody>

  </table>

</body>

</html>
```


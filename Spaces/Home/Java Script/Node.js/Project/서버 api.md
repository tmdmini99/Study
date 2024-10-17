


```js
const express = require('express');
const app = express();
const { Pool } = require('pg');
const pool = new Pool({
  user: 'username',
  host: 'localhost',
  database: 'your_database',
  password: 'password',
  port: 5432,
});

// API 엔드포인트 (데이터 페이징)
app.get('/api/data', async (req, res) => {
  const pageSize = parseInt(req.query.pageSize, 10) || 50;
  const page = parseInt(req.query.page, 10) || 1;
  const offset = (page - 1) * pageSize;

  try {
    const dataResult = await pool.query('SELECT * FROM your_table ORDER BY id LIMIT $1 OFFSET $2', [pageSize, offset]);
    const countResult = await pool.query('SELECT COUNT(*) FROM your_table');

    res.json({
      data: dataResult.rows,
      totalCount: parseInt(countResult.rows[0].count, 10),
    });
  } catch (err) {
    console.error(err);
    res.status(500).json({ error: 'Database query failed' });
  }
});

// 서버 실행
app.listen(3000, () => {
  console.log('Server is running on port 3000');
});

```
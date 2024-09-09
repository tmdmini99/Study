
설치
```
npm install xlsx

```

코드
```js
const express = require('express');
const router = express.Router();
const axios = require('axios');
const pool = require('./db'); // db.js
const XLSX = require('xlsx');
const path = require('path');
const fs = require('fs');

// 유틸리티 함수: 빈 문자열을 null로 변환
const normalizeProductData = (product) => {
  const normalized = {};
  for (const key in product) {
    normalized[key] = product[key] === "" ? null : product[key];
  }
  return normalized;
};

// API에서 데이터를 가져와서 데이터베이스에 삽입
const apiOptions = {
  method: 'GET',
  url: 'https://tmdals23222.cafe24api.com/api/v2/admin/products',
  headers: {
    'Authorization': 'Bearer yeg3oOcgdSpDfG1hDsDKNM',
    'Content-Type': 'application/json',
    'X-Cafe24-Api-Version': '2024-06-01'
  }
};

// 엑셀 파일 경로 설정
const excelFilePath = path.join(__dirname, '../excel/cafe24Data.xlsx');
const baseOutputDir = path.join(__dirname, '../output'); // 파일 저장 폴더

// 엑셀 파일에서 특정 시트의 특정 컬럼만 선택하는 함수
const readSelectedColumnsFromSheet = (sheet) => {
  return new Promise((resolve, reject) => {
    const results = [];
    
    sheet.forEach(row => {
      const selectedData = {};
      const columns = ['분류1', '분류2', '분류3', 'URL'];
      columns.forEach(column => {
        if (row[column] !== undefined) {
          selectedData[column] = row[column];
        }
      });
      results.push(selectedData);
    });

    resolve(results);
  });
};

// 엑셀 파일에서 모든 시트를 읽어오는 함수
const readExcelFile = (filePath) => {
  return new Promise((resolve, reject) => {
    try {
      const workbook = XLSX.readFile(filePath);
      const sheetsData = [];
      
      workbook.SheetNames.forEach(sheetName => {
        const sheet = workbook.Sheets[sheetName];
        const jsonData = XLSX.utils.sheet_to_json(sheet);
        readSelectedColumnsFromSheet(jsonData)
          .then(data => {
            sheetsData.push({ sheetName, data });
            if (sheetsData.length === workbook.SheetNames.length) {
              resolve(sheetsData);
            }
          })
          .catch(reject);
      });
    } catch (err) {
      reject(err);
    }
  });
};

// CSV 데이터와 API JSON 데이터를 처리하는 함수
const processData = async () => {
  try {
    // 1. 엑셀 파일에서 데이터 읽기
    const excelData = await readExcelFile(excelFilePath);

    for (const { sheetName, data } of excelData) {
      for (const item of data) {
        const { '분류1': category1, '분류2': category2, '분류3': category3, URL } = item;
        const categoryDir = path.join(baseOutputDir, category3);
        
        // 카테고리 폴더가 존재하지 않으면 생성
        if (!fs.existsSync(categoryDir)) {
          fs.mkdirSync(categoryDir, { recursive: true });
        }

        // 파일 이름과 경로 설정
        const fileName = path.join(categoryDir, `${category1}_${category2}_${category3}.txt`);
        fs.writeFileSync(fileName, URL, 'utf8');
      }
    }

    console.log('Excel data processed and files created successfully.');

    // 2. API 요청으로 JSON 데이터 가져오기
    const response = await axios(apiOptions);
    const jsonData = response.data;

    // API 응답을 데이터베이스에 삽입
    const products = jsonData.products;
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

    console.log('Products inserted into database successfully.');

  } catch (err) {
    console.error('Error processing data:', err);
  }
};

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

// 서버 시작과 데이터 처리
processData();

module.exports = router;

```
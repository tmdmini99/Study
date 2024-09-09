
터미널에 명령어로 설치
```
npm install csv-parser

```


특정 컬럼만 추출

```js
const fs = require('fs');
const csv = require('csv-parser');
const path = require('path');

// CSV 파일 경로
const csvFilePath = path.join(__dirname, 'your-file.csv');

// 특정 컬럼만 선택하는 함수
const readSelectedColumns = (csvFile, columns) => {
  return new Promise((resolve, reject) => {
    const results = [];
    
    fs.createReadStream(csvFile)
      .pipe(csv())
      .on('data', (row) => {
        const selectedData = {};
        columns.forEach(column => {
          if (row[column] !== undefined) {
            selectedData[column] = row[column];
          }
        });
        results.push(selectedData);
      })
      .on('end', () => {
        resolve(results);
      })
      .on('error', (err) => {
        reject(err);
      });
  });
};

// 사용 예시: 'name', 'price' 컬럼만 추출
readSelectedColumns(csvFilePath, ['name', 'price'])
  .then(data => {
    console.log('Selected Columns:', data);
    // 여기서 DB 삽입이나 추가 처리
  })
  .catch(err => {
    console.error('Error reading CSV:', err);
  });

```

###  설명

- **fs.createReadStream**: CSV 파일을 읽기 위한 스트림을 생성합니다.
- **csv-parser**: CSV 파일을 파싱하고 데이터를 한 줄씩 처리합니다.
- **columns**: 추출하고 싶은 컬럼 이름을 배열로 전달합니다.
- **on('data')**: 각 행의 데이터를 처리하며, 특정 컬럼만 선택해 `results` 배열에 저장합니다.
- **on('end')**: CSV 파일을 모두 읽었을 때 실행되며, 결과를 반환합니다.




특정 컬럼 추출하여 dir 만들고 api를 통해서 json 파일 만들기
```js
const express = require('express');

const router = express.Router();

const axios = require('axios');

const pool = require('./db'); // db.js

const csv = require('csv-parser');

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

const options = {

  method: 'GET',

  url: 'https://tmdals23222.cafe24api.com/api/v2/admin/products',

  headers: {

    'Authorization': 'Bearer TaH1sieBYKTbpmV0Hg1M1G',

    'Content-Type': 'application/json',

    'X-Cafe24-Api-Version': '2024-06-01'

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

  

module.exports = router;

  

// CSV 파일 경로와 출력 디렉토리 설정

const csvFolderPath = path.join(__dirname, '../csv'); // 엑셀 폴더 경로

const outputDir = path.join(__dirname, '../output'); // 파일 저장 폴더

console.log('CSV Folder Path:', csvFolderPath);

console.log('Output Directory Path:', outputDir);

  

// 특정 컬럼만 선택하는 함수

const readSelectedColumns = (csvFile, columns) => {

  return new Promise((resolve, reject) => {

    const results = [];

    fs.createReadStream(csvFile)

      .pipe(csv())

      .on('data', (row) => {

        const selectedData = {};

        columns.forEach(column => {

          if (row[column] !== undefined) {

            selectedData[column] = row[column];

          }

        });

        results.push(selectedData);

      })

      .on('end', () => {

        resolve(results);

      })

      .on('error', (err) => {

        reject(err);

      });

  });

};

  

// 폴더 내 모든 CSV 파일을 읽어오는 함수

const readAllCsvFilesInFolder = (folderPath, columns) => {

  return new Promise((resolve, reject) => {

    fs.readdir(folderPath, (err, files) => {

      if (err) return reject(err);

  

      const csvFiles = files.filter(file => path.extname(file) === '.csv');

      const readPromises = csvFiles.map(file => {

        const filePath = path.join(folderPath, file);

        return readSelectedColumns(filePath, columns);

      });

  

      Promise.all(readPromises)

        .then(results => {

          const allData = results.flat();

          resolve(allData);

        })

        .catch(reject);

    });

  });

};

  

// JSON 데이터를 저장할 파일 경로 설정

const jsonFilePath = path.join(outputDir, 'api_data.json');

  

// CSV 데이터와 API JSON 데이터를 처리하는 함수

const processData = async () => {

  try {

    // 1. 모든 CSV 파일에서 데이터 읽기

    const csvData = await readAllCsvFilesInFolder(csvFolderPath, ['분류1', '분류2', '분류3', 'URL', 'METHOD']);

    // 2. METHOD가 'GET'인 데이터 필터링

    const getRequests = csvData.filter(item => item.METHOD === 'GET');

  

    // 3. 각 CSV 데이터에 대해 API 요청 및 JSON 파일 저장

    for (const item of getRequests) {

      const { '분류1': category1, '분류2': category2, '분류3': category3, URL } = item;

      // URL 유효성 확인

      if (!URL || /{.*?}/.test(URL)) {

        console.log(`Skipping invalid URL: ${URL}`);

        continue; // URL이 유효하지 않거나 중괄호가 포함된 경우 건너뛰기

      }

      const baseURL = 'https://tmdals23222.cafe24api.com'; // Base URL

      console.log(`${URL}`);

      // API 요청 옵션 설정

      const apiOptions = {

        method: 'GET',

        url: `${baseURL}${URL}`, // 절대 경로로 URL을 결합,

        headers: {

          'Authorization': 'Bearer TaH1sieBYKTbpmV0Hg1M1G',

          'Content-Type': 'application/json',

          'X-Cafe24-Api-Version': '2024-06-01'

        }

      };

  

      try {

        // API 요청으로 JSON 데이터 가져오기

        const response = await axios(apiOptions);

        const jsonData = response.data;

  

        // 카테고리 폴더 경로 설정

        const category1Dir = path.join(outputDir, category1);

        const category2Dir = path.join(category1Dir, category2);

        const category3Dir = path.join(category2Dir, category3);

        // 폴더가 존재하지 않으면 생성

        if (!fs.existsSync(category3Dir)) {

          fs.mkdirSync(category3Dir, { recursive: true });

        }

  

        // JSON 데이터를 파일로 저장

        const fileName = path.basename(URL); // 'products'

        const jsonFileName = path.join(category3Dir, `${fileName}.json`);

        fs.writeFileSync(jsonFileName, JSON.stringify(jsonData, null, 2), 'utf8');

        console.log(`JSON data saved successfully to ${jsonFileName}.`);

      } catch (apiErr) {

        console.error('Error making API request:', apiErr);

      }

    }

    // // 3. API 응답을 데이터베이스에 삽입

    // const response = await axios(options);

    // const jsonData = response.data;

    // const products = jsonData.products;

    // for (const product of products) {

    //   const normalizedProduct = normalizeProductData(product);

    //   const keys = Object.keys(normalizedProduct);

    //   const values = keys.map(key => normalizedProduct[key]);

    //   const columns = keys.join(', ');

    //   const placeholders = keys.map((_, i) => `$${i + 1}`).join(', ');

  

    //   const query = `INSERT INTO products (${columns}) VALUES (${placeholders})

    //                  ON CONFLICT (product_code) DO NOTHING`;

  

    //   await pool.query(query, values);

    // }

  

    // console.log('Products inserted into database successfully.');

  

  } catch (err) {

    console.error('Error processing data:', err);

  }

};

  

// 데이터 처리 함수 실행

processData();

  

module.exports = router;
```
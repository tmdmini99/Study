
최종 버전
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

    url: 'https://kpopmerch-inc.myshopify.com/admin/api/2024-07/deprecated_api_calls.json',

    headers: {

      'Content-Type': 'application/json',

      'X-Shopify-Access-Token': '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'

    }

  };

  // console.log(1, options, 1);

  // 비동기 함수 대신 then/catch로 프로미스 처리

  const fetchDataAndInsert = async () => {

    try {

      const response = await axios(options);

      const jsonData = response.data; // 데이터를 response.data에서 가져옵니다

      // console.log(jsonData);

      // const tableName = Object.keys(jsonData)[0]; // "webhooks"

      // const records = jsonData[tableName]; // webhooks 배열 데이터

      // console.log(records);

      // 테이블 삽입 함수 호출

      await insertDataIntoTable(jsonData, `/admin/api/2024-07/deprecated_api_calls.json`);

      // console.log('Data successfully inserted into the table.');

    } catch (error) {

      console.error('Error:', error.message);

    }

  };

  // fetchDataAndInsert();

// 제품 목록을 데이터베이스에서 가져와서 EJS 뷰에 전달

router.get('/shopify', async function(req, res, next) {

  try {

    const result = await pool.query('SELECT product_no, product_name, price FROM products');

    const products = result.rows;

    res.render('index', { title: 'Product List', products });

  } catch (err) {

    // console.error('Error executing query', err.stack);

    next(err);

  }

});

  

module.exports = router;

  

// CSV 파일 경로와 출력 디렉토리 설정

const csvFolderPath = path.join(__dirname, '../csv2'); // 엑셀 폴더 경로

const outputDir = path.join(__dirname, '../shopify'); // 파일 저장 폴더

// console.log('CSV Folder Path:', csvFolderPath);

// console.log('Output Directory Path:', outputDir);

  

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

  
---


  

const processData = async () => {

  try {

    // 1. 모든 CSV 파일에서 데이터 읽기

    const csvData = await readAllCsvFilesInFolder(csvFolderPath, ['분류1', '분류2', '분류3', 'URL', 'METHOD']);

    // 2. METHOD가 'GET'인 데이터 필터링

    const getRequests = csvData.filter(item => item.METHOD.toLowerCase() === 'get');

    // 3. 각 CSV 데이터에 대해 API 요청 및 JSON 파일 저장

    for (const item of getRequests) {

      let { '분류1': category1, '분류2': category2, '분류3': category3, URL } = item;

  

      // URL 유효성 확인

      if (!URL || /{.*?}/.test(URL)) {

        continue;

      } else if (URL.includes('?')) {

        // URL에 '?'가 포함된 경우, 그 뒤에 'status'가 있는지 확인

        if (!URL.includes('?status')) {

          // 'status'가 없으면 '?' 이전 부분까지만 유지

          URL = URL.split('?')[0];

        }

      }

  

      const baseURL = 'https://kpopmerch-inc.myshopify.com'; // Base URL

      let hasNextPage = true;

      let pageInfo = '';

  

      while (hasNextPage) {

        // 페이지네이션을 처리하기 위해 page_info가 있으면 URL에 추가

        // 쿼리 문자열에서 첫 번째 status 이후는 제거

        const cleanUrl = URL.split('?')[0];

        const queryParams = new URLSearchParams(URL.split('?')[1]);

        // pageInfo가 있을 경우에만 status 제거

        if (pageInfo) {

          queryParams.delete('status');

        }

        const apiUrl = pageInfo

          ? `${baseURL}${cleanUrl}?${queryParams.toString()}&page_info=${pageInfo}`

          : `${baseURL}${cleanUrl}?${queryParams.toString()}`;

        console.log(`Making API request to: ${apiUrl}`);

        const apiOptions = {

          method: 'GET',

          url: apiUrl,

          headers: {

            'Content-Type': 'application/json',

            'X-Shopify-Access-Token': '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',

          },

        };

        try {

          // API 요청으로 JSON 데이터 가져오기

          const response = await axios(apiOptions);

          let jsonData = response.data;

          // 테이블명 설정

          const tableName = Object.keys(jsonData)[0];

          const records = jsonData[tableName];

          // JSON 데이터를 파일로 저장할 디렉토리 구조 설정

          const category1Dir = path.join(outputDirs, category1);

          const category2Dir = path.join(category1Dir, category2);

          const category3Dir = path.join(category2Dir, category3);

          if (!fs.existsSync(category3Dir)) {

            fs.mkdirSync(category3Dir, { recursive: true });

          }

          // 파일 이름을 고정 (orders.json)

          const fileName = `${tableName}.json`;

          const jsonFileName = path.join(category3Dir, fileName);

          // 기존 데이터를 읽지 않고, 바로 파일에 추가 저장

          let existingData = {}; // 기본 구조 초기화

          // 파일이 존재하면 기존 데이터를 읽어옴

          if (fs.existsSync(jsonFileName)) {

              const existingFileContent = fs.readFileSync(jsonFileName, 'utf8');

              existingData = JSON.parse(existingFileContent); // 기존 데이터를 JSON으로 변환

          }

          // 기존 데이터 구조 초기화

          if (!existingData[tableName]) {

              existingData[tableName] = Array.isArray(records) ? [] : {}; // records가 배열이면 빈 배열, 객체면 빈 객체로 초기화

          }

          // records 처리

          if (Array.isArray(records)) {

              existingData[tableName] = existingData[tableName].concat(records); // 배열인 경우 추가

          } else {

              existingData[tableName] = records; // 단일 객체일 경우 교체

          }

          // JSON 파일로 데이터 저장

          fs.writeFileSync(jsonFileName, JSON.stringify(existingData, null, 2), 'utf8');

          console.log(`JSON data successfully updated and saved to ${jsonFileName}.`);

          // API 응답 데이터를 데이터베이스에 삽입

          await insertDataIntoTable(jsonData, URL); // 테이블 삽입 함수 호출

          // Shopify API의 Link 헤더에서 페이지네이션 정보 확인

          const linkHeader = response.headers['link'];

          if (linkHeader) {

            const nextLinkMatch = linkHeader.match(/<([^>]+)>;\s*rel="next"/);

            if (nextLinkMatch) {

              const nextUrl = nextLinkMatch[1];

              const nextPageInfoMatch = nextUrl.match(/page_info=([^&]*)/);

              if (nextPageInfoMatch) {

                pageInfo = nextPageInfoMatch[1];

              } else {

                hasNextPage = false;

              }

            } else {

              hasNextPage = false;

            }

          } else {

            hasNextPage = false;

          }

        } catch (apiErr) {

          console.error(`Error making API request to ${apiUrl}:`, apiErr.message || apiErr);

          hasNextPage = false; // 에러 발생 시 페이지네이션 종료

        }

      }

    }

  

  } catch (err) {

    console.error('Error processing data:', err.message || err);

  }

};

  
  

// 데이터베이스에 삽입하는 함수

async function insertDataIntoTable(jsonData, url) {

  const tableName = Object.keys(jsonData)[0];

  let records = jsonData[tableName]; // 데이터 (여기서는 단일 값 57741)


  // "count"일 때 테이블 이름 변경

  let finalTableName = tableName;

  if (tableName === 'count') {

    finalTableName = `${url.split('/').slice(-2, -1)[0]}_count`; // URL의 마지막 부분에서 테이블 이름 결정


  }else if(tableName ==='transactions'){

    finalTableName = `${url.split('/').slice(-3, -2)[0]}_transactions`; // URL의 마지막 부분에서 테이블 이름 결정

  }else if(tableName ===`tags` || tableName ===`authors`){

    // console.log(jsonData);

      const query = `INSERT INTO ${tableName} (${tableName}) VALUES ('${records}')`;

      // console.log(query);

      await pool.query(query);

    return;  

  }

  if (Array.isArray(records)) {

    // 배열인 경우

    for (const record of records) {

      const keys = Object.keys(record);



    const values = keys.map((key) => {

      const value = record[key];

      // console.log(`Key: ${key}, Value:`, value);

      // 값이 배열인 경우

      if (Array.isArray(value)) {

          // 배열이 단순 문자열 배열인지 복잡한 JSON 객체 배열인지 구분

          if (value.length > 0 && typeof value[0] === 'string') {

              // 단순 문자열 배열을 PostgreSQL 배열 형식으로 변환

              return `{${value.map(item => item.toString().replace(/"/g, '""')).join(',')}}`;

          } else {

              // JSON 객체 배열을 JSON 문자열로 변환

              return JSON.stringify(value);

          }

      } else if (typeof value === 'object' && value !== null) {

          // 값이 객체인 경우 JSON 문자열로 변환

          return JSON.stringify(value);

      }

      // 그 외의 경우는 원래 값을 그대로 반환

      return value;

  });


  
  
  

      const columns = keys.join(', ');

      const placeholders = keys.map((_, i) => `$${i + 1}`).join(', ');

      // console.log('18번째 값 (image):', values[17]);

      // console.log(placeholders);

      const query = `INSERT INTO ${finalTableName} (${columns}) VALUES (${placeholders})`;

      // console.log(query);

      // console.log('Values:', values); // 디버깅: 삽입할 값 출력

      try {

        await pool.query(query, values);

        // console.log(`Data inserted into ${finalTableName} successfully.`);

      } catch (error) {

        console.error(`Error inserting data into ${finalTableName}:`, error);

        if (error.code === '23505') {

          // 중복 에러는 무시

          // console.log('Duplicate key error occurred, but ignored.');

      } else {

          // 다른 에러는 콘솔에 출력

          console.error(`Error inserting data into ${finalTableName}:`, error);

      }

      }

    }

  } else if ((typeof records === 'number' || typeof records === 'object' || typeof records === 'string') && records !== null) {

    // records가 객체일 경우

    let keys = Object.keys(records);

    let values = keys.map(key => records[key]);

    let columns = keys.join(', ');

    let placeholders = keys.map((_, i) => `$${i + 1}`).join(', ');

    let query =``;

    if(typeof records === 'number'){

      query = `INSERT INTO ${finalTableName} (count) VALUES (${records}) `;

    // console.log(query);

    }else if(url.split('/').pop().split('.')[0] === 'deprecated_api_calls'){

      columns = Object.keys(jsonData).join(', ');

      placeholders = Object.keys(jsonData).map((_, i) => `$${i + 1}`).join(', ');

      query = `INSERT INTO deprecated_api_calls (${columns}) VALUES (${placeholders}) `;

      values = Object.keys(jsonData).map(key => jsonData[key]);

    }else{

      query = `INSERT INTO ${finalTableName} (${columns}) VALUES (${placeholders}) `;

      // console.log(query);

    }

    try {

      await pool.query(query, values);

      console.log(`Single record inserted into ${finalTableName} successfully.`);

    } catch (error) {

      console.error(`Error inserting data into ${finalTableName}:`, error);

      return;

    }

  } else {

    console.error('Invalid data format: expected an array or object.');

  }

}

  
  
  

// 데이터 처리 함수 실행

// processData();

  

const outputDirs = path.join(__dirname, 'output2'); // JSON 파일 저장 경로

  

const processSingleApiData = async () => {

  try {

    let category1 = 'Orders'; // 분류1

    let category2 = 'Order'; // 분류2

    let category3 = 'Orders Properties'; // 분류3

    let URL = '/admin/api/2024-07/customers.json'; // API의 특정 URL

  

    // URL 유효성 확인

    if (!URL || /{.*?}/.test(URL)) {

      console.log(`Skipping invalid URL: ${URL}`);

      return;

    }

  

    const baseURL = 'https://kpopmerch-inc.myshopify.com'; // Base URL

    let hasNextPage = true;

    let pageInfo = '';

  

    while (hasNextPage) {

      // 페이지네이션을 처리하기 위해 page_info가 있으면 URL에 추가

      // 쿼리 문자열에서 첫 번째 status 이후는 제거

      const cleanUrl = URL.split('?')[0];

      const queryParams = new URLSearchParams(URL.split('?')[1]);

  

      // pageInfo가 있을 경우에만 status 제거

      if (pageInfo) {

        queryParams.delete('status');

      }

  

      const apiUrl = pageInfo

        ? `${baseURL}${cleanUrl}?${queryParams.toString()}&page_info=${pageInfo}`

        : `${baseURL}${cleanUrl}?${queryParams.toString()}`;

  

      console.log(`Making API request to: ${apiUrl}`);

      const apiOptions = {

        method: 'GET',

        url: apiUrl,

        headers: {

          'Content-Type': 'application/json',

          'X-Shopify-Access-Token': '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',

        },

      };

  

      try {

        // API 요청으로 JSON 데이터 가져오기

        const response = await axios(apiOptions);

        let jsonData = response.data;

  

        // 테이블명 설정

        const tableName = Object.keys(jsonData)[0];

        const records = jsonData[tableName];

  

        // JSON 데이터를 파일로 저장할 디렉토리 구조 설정

        const category1Dir = path.join(outputDirs, category1);

        const category2Dir = path.join(category1Dir, category2);

        const category3Dir = path.join(category2Dir, category3);

  

        if (!fs.existsSync(category3Dir)) {

          fs.mkdirSync(category3Dir, { recursive: true });

        }

  

        // 파일 이름을 고정 (orders.json)

        const fileName = `${tableName}.json`;

        const jsonFileName = path.join(category3Dir, fileName);

  

        // 기존 데이터를 읽지 않고, 바로 파일에 추가 저장

        let existingData = {}; // 기본 구조 초기화

  

        // 파일이 존재하면 기존 데이터를 읽어옴

        if (fs.existsSync(jsonFileName)) {

            const existingFileContent = fs.readFileSync(jsonFileName, 'utf8');

            existingData = JSON.parse(existingFileContent); // 기존 데이터를 JSON으로 변환

        }

  

        // 기존 데이터 구조 초기화

        if (!existingData[tableName]) {

            existingData[tableName] = Array.isArray(records) ? [] : {}; // records가 배열이면 빈 배열, 객체면 빈 객체로 초기화

        }

  

        // records 처리

        if (Array.isArray(records)) {

            existingData[tableName] = existingData[tableName].concat(records); // 배열인 경우 추가

        } else {

            existingData[tableName] = records; // 단일 객체일 경우 교체

        }

  

        // JSON 파일로 데이터 저장

        fs.writeFileSync(jsonFileName, JSON.stringify(existingData, null, 2), 'utf8');

        console.log(`JSON data successfully updated and saved to ${jsonFileName}.`);

  

        // API 응답 데이터를 데이터베이스에 삽입

        await insertDataIntoTable(jsonData, URL); // 테이블 삽입 함수 호출

  

        // Shopify API의 Link 헤더에서 페이지네이션 정보 확인

        const linkHeader = response.headers['link'];

        if (linkHeader) {

          const nextLinkMatch = linkHeader.match(/<([^>]+)>;\s*rel="next"/);

          if (nextLinkMatch) {

            const nextUrl = nextLinkMatch[1];

            const nextPageInfoMatch = nextUrl.match(/page_info=([^&]*)/);

            if (nextPageInfoMatch) {

              pageInfo = nextPageInfoMatch[1];

            } else {

              hasNextPage = false;

            }

          } else {

            hasNextPage = false;

          }

        } else {

          hasNextPage = false;

        }

  

      } catch (apiErr) {

        console.error(`Error making API request to ${apiUrl}:`, apiErr.message || apiErr);

        hasNextPage = false; // 에러 발생 시 페이지네이션 종료

      }

    }

  

  } catch (err) {

    console.error('Error processing data:', err.message || err);

  }

};

  

// 함수 실행

processSingleApiData();
```


이전 버전
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

    url: 'https://kpopmerch-inc.myshopify.com/admin/api/2024-07/deprecated_api_calls.json',

    headers: {

      'Content-Type': 'application/json',

      'X-Shopify-Access-Token': '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'

    }

  };

  // console.log(1, options, 1);

  // 비동기 함수 대신 then/catch로 프로미스 처리

  const fetchDataAndInsert = async () => {

    try {

      const response = await axios(options);

      const jsonData = response.data; // 데이터를 response.data에서 가져옵니다

      // console.log(jsonData);

      // const tableName = Object.keys(jsonData)[0]; // "webhooks"

      // const records = jsonData[tableName]; // webhooks 배열 데이터

      // console.log(records);

      // 테이블 삽입 함수 호출

      await insertDataIntoTable(jsonData, `/admin/api/2024-07/deprecated_api_calls.json`);

      // console.log('Data successfully inserted into the table.');

    } catch (error) {

      console.error('Error:', error.message);

    }

  };

  // fetchDataAndInsert();

// 제품 목록을 데이터베이스에서 가져와서 EJS 뷰에 전달

router.get('/shopify', async function(req, res, next) {

  try {

    const result = await pool.query('SELECT product_no, product_name, price FROM products');

    const products = result.rows;

    res.render('index', { title: 'Product List', products });

  } catch (err) {

    // console.error('Error executing query', err.stack);

    next(err);

  }

});

  

module.exports = router;

  

// CSV 파일 경로와 출력 디렉토리 설정

const csvFolderPath = path.join(__dirname, '../csv2'); // 엑셀 폴더 경로

const outputDir = path.join(__dirname, '../shopify'); // 파일 저장 폴더

// console.log('CSV Folder Path:', csvFolderPath);

// console.log('Output Directory Path:', outputDir);

  

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

// const processData = async () => {

//   try {

//     // 1. 모든 CSV 파일에서 데이터 읽기

//     const csvData = await readAllCsvFilesInFolder(csvFolderPath, ['분류1', '분류2', '분류3', 'URL', 'METHOD']);

//     // 2. METHOD가 'GET'인 데이터 필터링

//     const getRequests = csvData.filter(item => item.METHOD === 'get');

//     // 3. 각 CSV 데이터에 대해 API 요청 및 JSON 파일 저장

//     for (const item of getRequests) {

//       let { '분류1': category1, '분류2': category2, '분류3': category3, URL } = item;

  

//       // URL 유효성 확인

//       if (!URL || /{.*?}/.test(URL)) {

//         // console.log(`Skipping invalid URL: ${URL}`);

//         continue;

//       } else if (URL.includes('?')) {

//         URL = URL.split('?')[0];

//         // console.log(`${URL}`);

//       }

  

//       const baseURL = 'https://kpopmerch-inc.myshopify.com'; // Base URL

//       // console.log(`${baseURL}${URL}`);

  

//       // API 요청 옵션 설정

//       const apiOptions = {

//         method: 'GET',

//         url: `${baseURL}${URL}`, // 절대 경로로 URL을 결합

//         headers: {

//           'Content-Type': 'application/json',

//           'X-Shopify-Access-Token': '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',

//         },

//       };

  

//       try {

//         // API 요청으로 JSON 데이터 가져오기

//         const response = await axios(apiOptions);

//         const jsonData = response.data;

  

//         // 테이블명 설정

//         const tableName = Object.keys(jsonData)[0]; // "webhooks"

//         const records = jsonData[tableName]; // webhooks 배열 데이터

  

//         // JSON 데이터를 파일로 저장

//         const category1Dir = path.join(outputDir, category1);

//         const category2Dir = path.join(category1Dir, category2);

//         const category3Dir = path.join(category2Dir, category3);

  

//         if (!fs.existsSync(category3Dir)) {

//           fs.mkdirSync(category3Dir, { recursive: true });

//         }

  

//         const fileName = path.basename(URL); // 'products'

//         const jsonFileName = path.join(category3Dir, `${fileName}`);

//         fs.writeFileSync(jsonFileName, JSON.stringify(jsonData, null, 2), 'utf8');

//         // console.log(`JSON data saved successfully to ${jsonFileName}.`);

  

//         // 4. API 응답 데이터를 데이터베이스에 삽입

//         await insertDataIntoTable(jsonData,`${URL}`); // 테이블 삽입 함수 호출

//       } catch (apiErr) {

//         // console.error('Error making API request:', apiErr);

//       }

//     }

  

//   } catch (err) {

//     // console.error('Error processing data:', err);

//   }

// };

  

// const processData = async () => {

//   try {

//     // 1. 모든 CSV 파일에서 데이터 읽기

//     const csvData = await readAllCsvFilesInFolder(csvFolderPath, ['분류1', '분류2', '분류3', 'URL', 'METHOD']);

//     // 2. METHOD가 'GET'인 데이터 필터링

//     const getRequests = csvData.filter(item => item.METHOD.toLowerCase() === 'get');

//     // 3. 각 CSV 데이터에 대해 API 요청 및 JSON 파일 저장

//     for (const item of getRequests) {

//       let { '분류1': category1, '분류2': category2, '분류3': category3, URL } = item;

  

//       // URL 유효성 확인

//       if (!URL || /{.*?}/.test(URL)) {

//         // console.log(`Skipping invalid URL: ${URL}`);

//         continue;

//       } else if (URL.includes('?')) {

//         URL = URL.split('?')[0];

//         // console.log(`Processing URL without query parameters: ${URL}`);

//       }

  

//       const baseURL = 'https://kpopmerch-inc.myshopify.com'; // Base URL

//       // console.log(`Making request to: ${baseURL}${URL}`);

  

//       let hasNextPage = true;

//       let pageInfo = '';

  

//       while (hasNextPage) {

//         // 페이지네이션을 처리하기 위해 page_info가 있으면 URL에 추가

//         const apiUrl = pageInfo ? `${baseURL}${URL}?page_info=${pageInfo}` : `${baseURL}${URL}`;

  

//         const apiOptions = {

//           method: 'GET',

//           url: apiUrl, // 절대 경로로 URL을 결합

//           headers: {

//             'Content-Type': 'application/json',

//             'X-Shopify-Access-Token': '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',

//           },

//         };

  

//         try {

//           // API 요청으로 JSON 데이터 가져오기

//           const response = await axios(apiOptions);

//           const jsonData = response.data;

  

//           // 테이블명 설정

//           const tableName = Object.keys(jsonData)[0]; // "webhooks"

//           const records = jsonData[tableName]; // "webhooks" 배열 데이터

  

//           // JSON 데이터를 파일로 저장

//           const category1Dir = path.join(outputDir, category1);

//           const category2Dir = path.join(category1Dir, category2);

//           const category3Dir = path.join(category2Dir, category3);

  

//           if (!fs.existsSync(category3Dir)) {

//             fs.mkdirSync(category3Dir, { recursive: true });

//             console.log(`Created directories: ${category3Dir}`);

//           }

  

//           // URL에서 파일 이름 추출하고 확장자 추가

//           const fileName = path.basename(URL); // 'products.json'처럼 확장자 추가

//           const jsonFileName = path.join(category3Dir, `${fileName}`);

  

//           // JSON 파일로 저장

//           fs.writeFileSync(jsonFileName, JSON.stringify(jsonData, null, 2), 'utf8');

//           // console.log(`JSON data saved successfully to ${jsonFileName}.`);

  

//           // 4. API 응답 데이터를 데이터베이스에 삽입

//           await insertDataIntoTable(jsonData, `${URL}`); // 테이블 삽입 함수 호출

//           // console.log(`Data inserted into table from ${URL}`);

  

//           // Shopify API의 Link 헤더에서 페이지네이션 정보 확인

//           const linkHeader = response.headers['link'];

//           console.log(`Link header: ${linkHeader}`);

  

//           // 페이지네이션 처리

//           if (linkHeader) {

//             const nextLinkMatch = linkHeader.match(/<([^>]+)>;\s*rel="next"/);

//             console.log(nextLinkMatch);

//             if (nextLinkMatch) {

//               const nextUrl = nextLinkMatch[1];

//               const nextPageInfoMatch = nextUrl.match(/page_info=([^&]*)/);

//               if (nextPageInfoMatch) {

//                 pageInfo = nextPageInfoMatch[1];

//                 console.log(`Next page info: ${pageInfo}`);

//               } else {

//                 hasNextPage = false;

//               }

//             } else {

//               hasNextPage = false;

//             }

//           } else {

//             hasNextPage = false;

//           }

  

//         } catch (apiErr) {

//           console.error(`Error making API request to ${URL}:`, apiErr.message || apiErr);

//           hasNextPage = false; // 에러 발생 시 페이지네이션 종료

//         }

//       }

//     }

  

//   } catch (err) {

//     console.error('Error processing data:', err.message || err);

//   }

// };

  

const processData = async () => {

  try {

    // 1. 모든 CSV 파일에서 데이터 읽기

    const csvData = await readAllCsvFilesInFolder(csvFolderPath, ['분류1', '분류2', '분류3', 'URL', 'METHOD']);

    // 2. METHOD가 'GET'인 데이터 필터링

    const getRequests = csvData.filter(item => item.METHOD.toLowerCase() === 'get');

    // 3. 각 CSV 데이터에 대해 API 요청 및 JSON 파일 저장

    for (const item of getRequests) {

      let { '분류1': category1, '분류2': category2, '분류3': category3, URL } = item;

  

      // URL 유효성 확인

      if (!URL || /{.*?}/.test(URL)) {

        // console.log(`Skipping invalid URL: ${URL}`);

        continue;

      } else if (URL.includes('?')) {

        // URL에 '?'가 포함된 경우, 그 뒤에 'status'가 있는지 확인

        if (!URL.includes('?status')) {

          // 'status'가 없으면 '?' 이전 부분까지만 유지

          URL = URL.split('?')[0];

          // console.log(`Processing URL without query parameters: ${URL}`);

        }

      }

  

      const baseURL = 'https://kpopmerch-inc.myshopify.com'; // Base URL

      // console.log(`Making request to: ${baseURL}${URL}`);

  

      let hasNextPage = true;

      let pageInfo = '';

  

      while (hasNextPage) {

        // 페이지네이션을 처리하기 위해 page_info가 있으면 URL에 추가

        const apiUrl = pageInfo ? `${baseURL}${URL}?limit=50&page_info=${pageInfo}` : `${baseURL}${URL}?limit=50`;

  

        const apiOptions = {

          method: 'GET',

          url: apiUrl, // 절대 경로로 URL을 결합

          headers: {

            'Content-Type': 'application/json',

            'X-Shopify-Access-Token': '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',

          },

        };

  

        try {

          // API 요청으로 JSON 데이터 가져오기

          const response = await axios(apiOptions);

          const jsonData = response.data;

  

          // 테이블명 설정

          const tableName = Object.keys(jsonData)[0]; // "webhooks"

          const records = jsonData[tableName]; // "webhooks" 배열 데이터

  

          // JSON 데이터를 파일로 저장할 디렉토리 구조 설정

          const category1Dir = path.join(outputDir, category1);

          const category2Dir = path.join(category1Dir, category2);

          const category3Dir = path.join(category2Dir, category3);

  

          if (!fs.existsSync(category3Dir)) {

            fs.mkdirSync(category3Dir, { recursive: true });

            console.log(`Created directories: ${category3Dir}`);

          }

  

          // URL에서 파일 이름 추출하고 확장자 추가

          const fileName = path.basename(URL); // 'products.json'처럼 확장자 추가

          const jsonFileName = path.join(category3Dir, `${fileName}`);

  

          let existingData = {};

          // 파일이 이미 존재하면 기존 데이터를 읽어들임

          if (fs.existsSync(jsonFileName)) {

            const existingFileContent = fs.readFileSync(jsonFileName, 'utf8');

            existingData = JSON.parse(existingFileContent); // 기존 데이터를 JSON으로 변환

          }

  

          // 새로운 데이터를 기존 데이터와 병합

          if (existingData[tableName]) {

            existingData[tableName] = [...existingData[tableName], ...records]; // 배열을 합침

          } else {

            existingData[tableName] = records; // 처음 생성할 때

          }

  

          // JSON 파일로 병합된 데이터 저장

          fs.writeFileSync(jsonFileName, JSON.stringify(existingData, null, 2), 'utf8');

          // console.log(`JSON data successfully updated and saved to ${jsonFileName}.`);

  

          // 4. API 응답 데이터를 데이터베이스에 삽입

          await insertDataIntoTable(jsonData, `${URL}`); // 테이블 삽입 함수 호출

          // console.log(`Data inserted into table from ${URL}`);

  

          // Shopify API의 Link 헤더에서 페이지네이션 정보 확인

          const linkHeader = response.headers['link'];

          console.log(`Link header: ${linkHeader}`);

  

          // 페이지네이션 처리

          if (linkHeader) {

            const nextLinkMatch = linkHeader.match(/<([^>]+)>;\s*rel="next"/);

            console.log(nextLinkMatch);

            if (nextLinkMatch) {

              const nextUrl = nextLinkMatch[1];

              const nextPageInfoMatch = nextUrl.match(/page_info=([^&]*)/);

              if (nextPageInfoMatch) {

                pageInfo = nextPageInfoMatch[1];

                console.log(`Next page info: ${pageInfo}`);

              } else {

                hasNextPage = false;

              }

            } else {

              hasNextPage = false;

            }

          } else {

            hasNextPage = false;

          }

  

        } catch (apiErr) {

          console.error(`Error making API request to ${URL}:`, apiErr.message || apiErr);

          hasNextPage = false; // 에러 발생 시 페이지네이션 종료

        }

      }

    }

  

  } catch (err) {

    console.error('Error processing data:', err.message || err);

  }

};

  
  

// 데이터베이스에 삽입하는 함수

async function insertDataIntoTable(jsonData, url) {

  const tableName = Object.keys(jsonData)[0];

  let records = jsonData[tableName]; // 데이터 (여기서는 단일 값 57741)

    // console.log('ffffffffffffff',records,'dddddd');

    // console.log(Array.isArray(records),'data');  

    // console.log('s',jsonData,'dddddd');

  // "count"일 때 테이블 이름 변경

  let finalTableName = tableName;

  if (tableName === 'count') {

    finalTableName = `${url.split('/').slice(-2, -1)[0]}_count`; // URL의 마지막 부분에서 테이블 이름 결정

    // console.log(finalTableName);

    // console.log(typeof(records));

    // console.log(records !== null);

  }else if(tableName ==='transactions'){

    finalTableName = `${url.split('/').slice(-3, -2)[0]}_transactions`; // URL의 마지막 부분에서 테이블 이름 결정

  }else if(tableName ===`tags` || tableName ===`authors`){

    // console.log(jsonData);

      const query = `INSERT INTO ${tableName} (${tableName}) VALUES ('${records}')`;

      // console.log(query);

      await pool.query(query);

    return;  

  }

  if (Array.isArray(records)) {

    // 배열인 경우

    for (const record of records) {

      const keys = Object.keys(record);

    //   const values = keys.map(key => record[key]);

    //   const values = keys.map((key) => {

    //     // 값이 객체나 배열인 경우 문자열로 변환

    //     if (typeof record[key] === 'object' && record[key] !== null) {

    //         return JSON.stringify(record[key]);

    //     }

    //     // 그 외의 경우는 원래 값을 그대로 반환

    //     return record[key];

    // });

    const values = keys.map((key) => {

      const value = record[key];

      // console.log(`Key: ${key}, Value:`, value);

      // 값이 배열인 경우

      if (Array.isArray(value)) {

          // 배열이 단순 문자열 배열인지 복잡한 JSON 객체 배열인지 구분

          if (value.length > 0 && typeof value[0] === 'string') {

              // 단순 문자열 배열을 PostgreSQL 배열 형식으로 변환

              return `{${value.map(item => item.toString().replace(/"/g, '""')).join(',')}}`;

          } else {

              // JSON 객체 배열을 JSON 문자열로 변환

              return JSON.stringify(value);

          }

      } else if (typeof value === 'object' && value !== null) {

          // 값이 객체인 경우 JSON 문자열로 변환

          return JSON.stringify(value);

      }

      // 그 외의 경우는 원래 값을 그대로 반환

      return value;

  });

//   const values = keys.map((key) => {

//     const value = record[key];

  

//     // 값이 배열인 경우

//     if (Array.isArray(value)) {

//         // 배열이 비어 있는 경우 공백 문자열 반환

//         if (value.length === 0) {

//             return "''";  // 빈 문자열로 처리

//         }

  

//         // 배열을 쉼표로 구분된 문자열로 변환

//         return `'${value.map(item => item.toString().replace(/"/g, '""')).join(',')}'`;

//     } else if (typeof value === 'object' && value !== null) {

//         // 값이 객체인 경우 JSON 문자열로 변환 (단일 인용부호 제거)

//         return JSON.stringify(value);  // JSON 객체는 그대로 전달

//     }

  

//     // 특정 필드가 timestamp와 같이 빈 문자열을 허용하지 않는 경우 null로 처리

//     if (key === 'timestamp_field_name' && (value === '' || value === null)) {

//         return null;  // 'null' 문자열이 아닌 실제 null 값

//     }

  

//     // 값이 null이거나 빈 문자열이면 처리

//     if (value === null || value === '') {

//         return null;  // null 값으로 처리

//     }

  

//     // 그 외의 경우는 원래 값을 그대로 반환

//     return value;

// });

  
  
  
  
  
  

      const columns = keys.join(', ');

      const placeholders = keys.map((_, i) => `$${i + 1}`).join(', ');

      // console.log('18번째 값 (image):', values[17]);

      // console.log(placeholders);

      const query = `INSERT INTO ${finalTableName} (${columns}) VALUES (${placeholders})`;

      // console.log(query);

      // console.log('Values:', values); // 디버깅: 삽입할 값 출력

      try {

        await pool.query(query, values);

        // console.log(`Data inserted into ${finalTableName} successfully.`);

      } catch (error) {

        console.error(`Error inserting data into ${finalTableName}:`, error);

        if (error.code === '23505') {

          // 중복 에러는 무시

          // console.log('Duplicate key error occurred, but ignored.');

      } else {

          // 다른 에러는 콘솔에 출력

          console.error(`Error inserting data into ${finalTableName}:`, error);

      }

      }

    }

  } else if ((typeof records === 'number' || typeof records === 'object' || typeof records === 'string') && records !== null) {

    // records가 객체일 경우

    let keys = Object.keys(records);

    let values = keys.map(key => records[key]);

    let columns = keys.join(', ');

    let placeholders = keys.map((_, i) => `$${i + 1}`).join(', ');

    let query =``;

    if(typeof records === 'number'){

      query = `INSERT INTO ${finalTableName} (count) VALUES (${records}) `;

    // console.log(query);

    }else if(url.split('/').pop().split('.')[0] === 'deprecated_api_calls'){

      columns = Object.keys(jsonData).join(', ');

      placeholders = Object.keys(jsonData).map((_, i) => `$${i + 1}`).join(', ');

      query = `INSERT INTO deprecated_api_calls (${columns}) VALUES (${placeholders}) `;

      values = Object.keys(jsonData).map(key => jsonData[key]);

    }else{

      query = `INSERT INTO ${finalTableName} (${columns}) VALUES (${placeholders}) `;

      // console.log(query);

    }

    try {

      await pool.query(query, values);

      console.log(`Single record inserted into ${finalTableName} successfully.`);

    } catch (error) {

      console.error(`Error inserting data into ${finalTableName}:`, error);

      return;

    }

  } else {

    console.error('Invalid data format: expected an array or object.');

  }

}

  
  
  

// 데이터 처리 함수 실행

processData();
```
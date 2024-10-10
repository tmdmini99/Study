
node js 원래 있던  orders에서 데이터 뽑아서  order에 저장

```js
const express = require('express');

const router = express.Router();

const axios = require('axios');

const pool = require('./db'); // db.js 파일

const csv = require('csv-parser'); // CSV 파일을 파싱하기 위한 라이브러리

const path = require('path'); // 경로 작업을 위한 모듈

const fs = require('fs'); // 파일 시스템 작업을 위한 모듈

const { log } = require('debug/src/browser');

  

// CSV 파일에서 데이터 읽기

const readAllCsvFilesInFolder = async (folderPath, columns) => {

    const results = []; // 결과를 저장할 배열

    const files = fs.readdirSync(folderPath); // 폴더의 모든 파일 읽기

    for (const file of files) {

        if (file.endsWith('.csv')) { // CSV 파일만 처리

            const filePath = path.join(folderPath, file); // 파일 경로 생성

            const data = await new Promise((resolve, reject) => {

                const rows = []; // 각 CSV 파일의 행을 저장할 배열

                fs.createReadStream(filePath) // CSV 파일을 스트림으로 읽기

                    .pipe(csv()) // CSV 파서와 연결

                    .on('data', (row) => {

                        rows.push(row); // 각 행을 배열에 추가

                    })

                    .on('end', () => {

                        resolve(rows); // 모든 행을 읽었으면 배열 반환

                    })

                    .on('error', reject); // 에러 발생 시 reject

            });

            results.push(...data); // 읽은 데이터 결과에 추가

        }

    }

    return results; // 모든 CSV 파일의 데이터 반환

};

  

// API 요청에 대한 테이블을 가져오는 함수

const getTablesWithSVariant = async (category2) => {

    const query = `SELECT * FROM get_tables_with_s_variant() where plural_table_name = LOWER($1) or base_table_name = LOWER($1);`; // SQL 쿼리

    log("실행된 쿼리:", query);

    log("사용된 파라미터:", [category2]);

    const { rows } = await pool.query(query, [category2]); // DB에서 쿼리 실행

    return rows; // 쿼리 결과 반환

};

  

// 주어진 URL에서 API 요청 처리 및 JSON 데이터 저장

const processData = async (csvFolderPath, outputDir) => {

    try {

        // 1. 모든 CSV 파일에서 데이터 읽기

        const csvData = await readAllCsvFilesInFolder(csvFolderPath, ['분류1', '분류2', '분류3', 'URL', 'METHOD']);

        // 2. METHOD가 'GET'인 데이터 필터링 및 {}가 있는 URL만 포함

        const getRequests = csvData.filter(item =>

            item.METHOD.toLowerCase() === 'get' && /{.*?}/.test(item.URL)

        );

  

        // 3. 각 CSV 데이터에 대해 API 요청 및 JSON 파일 저장

        for (const item of getRequests) {

            let { '분류1': category1, '분류2': category2, '분류3': category3, URL } = item;

  

            // 테이블과 ID 값을 가져오는 함수 호출

            const tables = await getTablesWithSVariant(category2);

  

            // 만약 테이블이 없다면 다음 루프로 넘어감

            if (tables.length === 0) {

                log(`분류2에 해당하는 테이블이 없음: ${category2}`);

                continue;

            }

  

            // 필터링된 테이블에 대해 반복하여 API 요청 처리

            for (const table of tables) {

                const { base_table_name, plural_table_name, id_column, table_id } = table;

                log(`처리 중인 테이블: ${plural_table_name}`);

  

                // URL의 {id_column} 부분을 table_id로 대체

                const finalUrl = URL.replace(`{${id_column}}`, table_id);

  

                // URL 유효성 확인

                if (!finalUrl || finalUrl.endsWith("search.json")) {

                    log(`유효하지 않은 URL: ${finalUrl}`);

                    continue; // URL이 유효하지 않으면 넘어감

                }

                // API 요청 및 데이터 처리 코드 작성...

                log(`최종 URL: ${finalUrl}`);

                const baseURL = 'https://kpopmerch-inc.myshopify.com'; // 기본 URL

                console.log(`API 요청: ${finalUrl}`); // API 요청 로그

                const apiOptions = {

                    method: 'GET', // GET 요청

                    url: baseURL+finalUrl,

                    headers: {

                        'Content-Type': 'application/json',

                        'X-Shopify-Access-Token': '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', // Shopify API 액세스 토큰

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

                    const category1Dir = path.join(outputDir, category1);

                    const category2Dir = path.join(category1Dir, category2);

                    const category3Dir = path.join(category2Dir, category3);

  

                    if (!fs.existsSync(category3Dir)) {

                        fs.mkdirSync(category3Dir, { recursive: true }); // 디렉토리 생성

                    }

  

                    // 파일 이름 설정

                    const fileName = path.basename(finalUrl.includes('?') ? finalUrl.split('?')[0] : finalUrl);

                    const jsonFileName = path.join(category3Dir, fileName);

  

                    // 기존 데이터를 읽지 않고, 바로 파일에 추가 저장

                    let existingData = {};

  

                    // 파일이 존재하면 기존 데이터를 읽어옴

                    if (fs.existsSync(jsonFileName)) {

                        const existingFileContent = fs.readFileSync(jsonFileName, 'utf8');

                        existingData = JSON.parse(existingFileContent);

                    }

  

                    // 기존 데이터 구조 초기화

                    if (!existingData[tableName]) {

                        existingData[tableName] = Array.isArray(records) ? [] : {};

                    }

  

                    // records 처리

                    if (Array.isArray(records)) {

                        existingData[tableName] = existingData[tableName].concat(records); // 기존 데이터와 결합

                    } else {

                        existingData[tableName] = records;

                    }

  

                    // JSON 파일로 데이터 저장

                    fs.writeFileSync(jsonFileName, JSON.stringify(existingData, null, 2), 'utf8');

                    console.log(`JSON 데이터가 성공적으로 ${jsonFileName}에 저장되었습니다.`); // 저장 성공 로그

  

                    // API 응답 데이터를 데이터베이스에 삽입

                    await insertDataIntoTable(jsonData, finalUrl);

  

                } catch (apiErr) {

                    console.error(`API 요청 중 에러 발생: ${finalUrl}:`, apiErr.message || apiErr);

                }

                // API 요청 코드 작성...

            }

        }

  

    } catch (err) {

        console.error('데이터 처리 중 에러 발생:', err.message || err);

    }

};

  

// 실행 예시

const csvFolderPath = path.join(__dirname, '../csv2'); // CSV 파일이 있는 폴더 경로

const outputDir = path.join(__dirname, '../shopifyDetail'); // JSON 파일 저장 폴더

// processData(csvFolderPath, outputDir);

  
  

async function insertDataIntoTable(jsonData, url) {

    const tableName = Object.keys(jsonData)[0];

    let records = jsonData[tableName]; // 단순 객체

  

    // "count"일 때 테이블 이름 변경

    let finalTableName = tableName;

    log(tableName, '원본 테이블 이름');

    // 특정 테이블 이름에 따라 처리할 테이블 이름 변경

    if (tableName === 'count') {

        finalTableName = `${url.split('/').slice(-2, -1)[0]}_count`;

    } else if (tableName === 'transactions') {

        finalTableName = `${url.split('/').slice(-3, -2)[0]}_transactions`;

    } else if (tableName === 'tags' || tableName === 'authors') {

        // 단순 JSON 데이터로 tags 또는 authors 테이블에 삽입

        const query = `INSERT INTO ${tableName} (${tableName}) VALUES ($1)`;

        await pool.query(query, [records]); // 직접 records 삽입

        return;

    }

    if (finalTableName === 'order') {

        finalTableName = `"order"`; // 이중 따옴표로 감싸서 예약어 충돌 방지

    }

    // records가 객체일 경우, 객체의 키와 값을 처리

    if (typeof records === 'object' && records !== null) {

        const keys = Object.keys(records); // JSON 키는 테이블의 컬럼

        const values = keys.map(key => {

            const value = records[key];

            if (typeof value === 'object' && value !== null) {

                return JSON.stringify(value); // 객체일 경우 JSON 문자열로 변환

            }

            return value; // 나머지 값 그대로 반환

        });

  

        // 컬럼명과 값을 넣을 자리(placeholders) 설정

        const columns = keys.join(', ');

        const placeholders = keys.map((_, i) => `$${i + 1}`).join(', ');

        // 쿼리 작성

        const query = `INSERT INTO ${finalTableName} (${columns}) VALUES (${placeholders})`;

        try {

            // 쿼리 실행

            await pool.query(query, values);

            console.log(`Record inserted into ${finalTableName} successfully.`);

        } catch (error) {

            if (error.code === '23505') {

                console.log('Duplicate key error occurred, but ignored.');

            } else {

                console.error(`Error inserting data into ${finalTableName}:`, error);

            }

        }

    } else {

        console.error('Invalid data format: expected an object.');

    }

}

  
  
  
  
  
  

  const csvFileName = 'shopify data - 12. Orders.csv'; // 특정 파일명

  

  const processDatas = async (csvFileName, csvFolderPath, outputDir) => {

    try {

        // 1. 선택된 CSV 파일에서 데이터 읽기

        const csvData = await readCsvFile(path.join(csvFolderPath, csvFileName));

  

        // 2. METHOD가 'GET'인 데이터 필터링 및 {}가 있는 URL만 포함

        const getRequests = csvData.filter(item =>

            item.METHOD.toLowerCase() === 'get' && /{.*?}/.test(item.URL)

        );

  

        // 3. 각 CSV 데이터에 대해 API 요청 및 JSON 파일 저장

        for (const item of getRequests) {

            let { '분류1': category1, '분류2': category2, '분류3': category3, URL } = item;

  

            // 분류2를 기준으로 테이블 필터링하여 가져오기

            const tables = await getTablesWithSVariant(category2);

  

            // 만약 테이블이 없다면 다음 루프로 넘어감

            if (tables.length === 0) {

                log(`분류2에 해당하는 테이블이 없음: ${category2}`);

                continue;

            }

  

            // 필터링된 테이블에 대해 반복하여 API 요청 처리

            for (const table of tables) {

                const { base_table_name, plural_table_name, id_column, table_id } = table;

                log(`처리 중인 테이블: ${plural_table_name}`);

  

                // URL의 {id_column} 부분을 table_id로 대체

                const finalUrl = URL.replace(`{${id_column}}`, table_id);

  

                // URL 유효성 확인

                if (!finalUrl || finalUrl.endsWith("search.json")) {

                    log(`유효하지 않은 URL: ${finalUrl}`);

                    continue; // URL이 유효하지 않으면 넘어감

                }

                // API 요청 및 데이터 처리 코드 작성...

                log(`최종 URL: ${finalUrl}`);

                const baseURL = 'https://kpopmerch-inc.myshopify.com'; // 기본 URL

                console.log(`API 요청: ${finalUrl}`); // API 요청 로그

                const apiOptions = {

                    method: 'GET', // GET 요청

                    url: baseURL+finalUrl,

                    headers: {

                        'Content-Type': 'application/json',

                        'X-Shopify-Access-Token': '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', // Shopify API 액세스 토큰

                    },

                };

  

                try {

                    // API 요청으로 JSON 데이터 가져오기

                    const response = await axios(apiOptions);

                    let jsonData = response.data;

                    log(jsonData,33333333333333333333333333);

                    // 테이블명 설정

                    const tableName = Object.keys(jsonData)[0];

                    const records = jsonData[tableName];

  

                    // JSON 데이터를 파일로 저장할 디렉토리 구조 설정

                    const category1Dir = path.join(outputDir, category1);

                    const category2Dir = path.join(category1Dir, category2);

                    const category3Dir = path.join(category2Dir, category3);

  

                    if (!fs.existsSync(category3Dir)) {

                        fs.mkdirSync(category3Dir, { recursive: true }); // 디렉토리 생성

                    }

  

                    // 파일 이름 설정

                    const fileName = path.basename(finalUrl.includes('?') ? finalUrl.split('?')[0] : finalUrl);

                    const jsonFileName = path.join(category3Dir, fileName);

  

                    // 기존 데이터를 읽지 않고, 바로 파일에 추가 저장

                    let existingData = {};

  

                    // 파일이 존재하면 기존 데이터를 읽어옴

                    if (fs.existsSync(jsonFileName)) {

                        const existingFileContent = fs.readFileSync(jsonFileName, 'utf8');

                        existingData = JSON.parse(existingFileContent);

                    }

  

                    // 기존 데이터 구조 초기화

                    if (!existingData[tableName]) {

                        existingData[tableName] = Array.isArray(records) ? [] : {};

                    }

  

                    // records 처리

                    if (Array.isArray(records)) {

                        existingData[tableName] = existingData[tableName].concat(records); // 기존 데이터와 결합

                    } else {

                        existingData[tableName] = records;

                    }

  

                    // JSON 파일로 데이터 저장

                    fs.writeFileSync(jsonFileName, JSON.stringify(existingData, null, 2), 'utf8');

                    console.log(`JSON 데이터가 성공적으로 ${jsonFileName}에 저장되었습니다.`); // 저장 성공 로그

  

                    // API 응답 데이터를 데이터베이스에 삽입

                    await insertDataIntoTable(jsonData, finalUrl);

  

                } catch (apiErr) {

                    console.error(`API 요청 중 에러 발생: ${finalUrl}:`, apiErr.message || apiErr);

                }

                // API 요청 코드 작성...

            }

        }

    } catch (err) {

        console.error('데이터 처리 중 에러 발생:', err.message || err);

    }

};

  

// CSV 파일을 읽는 함수 추가

const readCsvFile = async (filePath) => {

    return new Promise((resolve, reject) => {

        const rows = [];

        fs.createReadStream(filePath)

            .pipe(csv())

            .on('data', (row) => {

                rows.push(row);

            })

            .on('end', () => {

                resolve(rows);

            })

            .on('error', reject);

    });

};

  

// 실행 예시

  

processDatas(csvFileName, csvFolderPath, outputDir);
```






jstl사용하여 split

```jsp
<c:set var="tagsList" value="${fn:split(list.tags, ',')}" />  
<c:forEach var="tag" items="${tagsList}">  
    <span class="badge text-bg-gray">${fn:trim(tag)}</span>  
</c:forEach>
```
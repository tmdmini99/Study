
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



```js
async function insertReqBody(req, res, targetTable) {

    try {

        const json = req.body;

        console.log('Received Request Body:', json);

  

        // records가 단일 객체 또는 배열인지 확인

        const records = Array.isArray(json) ? json : [json];

        const insertedData = [];

  

        for (const record of records) {

            if (typeof record !== 'object' || record === null) {

                console.error('Invalid record format. Expected an object:', record);

                continue;

            }

  

            const keys = Object.keys(record);

            if (keys.length === 0) {

                console.error('No columns to insert for record:', record);

                continue;

            }

  

            // 컬럼 이름을 큰따옴표로 감싸기

            const columns = keys.map(key => `"${key}"`).join(', ');

  

            const values = keys.map((key) => {

                const value = record[key];

  

                if (Array.isArray(value)) {

                    // 배열인 경우, 각 항목을 문자열로 변환한 후 JSON 배열로 변환

                    return JSON.stringify(value);  // 배열을 JSON 문자열로 변환

                } else if (typeof value === 'object' && value !== null) {

                    // 객체인 경우, JSON 문자열로 변환

                    return JSON.stringify(value);  

                } else if (value === null) {

                    return null;

                }

                return value;

            });

  

            const placeholders = keys.map((_, i) => `$${i + 1}`).join(', ');

  

            const query = `INSERT INTO webhook.${targetTable} (${columns}) VALUES (${placeholders}) RETURNING *`;

  

            console.log('Generated Query:', query);

            console.log('Values:', values);

  

            try {

                const result = await pool.query(query, values);

                if (result && result.rows.length > 0) {

                    insertedData.push(result.rows[0]);

                }

            } catch (error) {

                console.error(`Error inserting data into ${targetTable}:`, error.message, {

                    query,

                    values,

                });

            }

        }

  

        res.status(201).json({

            message: 'Data inserted successfully',

            data: insertedData,

        });

    } catch (error) {

        console.error('Error occurred:', error.message);

        res.status(500).json({

            message: 'An error occurred during insertion',

            error: error.message,

        });

    }

}
```


jstl사용하여 split

```jsp
<c:set var="tagsList" value="${fn:split(list.tags, ',')}" />  
<c:forEach var="tag" items="${tagsList}">  
    <span class="badge text-bg-gray">${fn:trim(tag)}</span>  
</c:forEach>
```



모든 id 동적으로 insert
```js
const express = require('express');

const router = express.Router();

const axios = require('axios');

const pool = require('./db'); // db.js 파일

const csv = require('csv-parser'); // CSV 파일을 파싱하기 위한 라이브러리

const path = require('path'); // 경로 작업을 위한 모듈

const fs = require('fs'); // 파일 시스템 작업을 위한 모듈

const { log } = require('debug/src/browser');

  
  

const reservedWords = new Set([

  'absolute', 'action', 'add', 'after', 'all', 'also', 'alter',

  'always', 'analyze', 'and', 'any', 'array', 'as', 'ascending',

  'asymmetric', 'at', 'authorization', 'backward', 'before',

  'begin', 'between', 'bigint', 'binary', 'both', 'by', 'cascade',

  'case', 'cast', 'checkpoint', 'class', 'close', 'cluster',

  'comment', 'commit', 'condition', 'constraint', 'content',

  'continue', 'convert', 'copy', 'create', 'cross', 'current',

  'current_catalog', 'current_date', 'current_role', 'current_schema',

  'current_time', 'current_timestamp', 'current_user', 'cursor',

  'cycle', 'database', 'day', 'deallocate', 'dec', 'decimal',

  'declare', 'default', 'deferrable', 'deferred', 'delete',

  'desc', 'distinct', 'do', 'domain', 'double', 'drop', 'each',

  'else', 'end', 'except', 'exclude', 'exclusive', 'execute',

  'exists', 'explain', 'fetch', 'first', 'float', 'for',

  'force', 'foreign', 'from', 'full', 'function', 'generated',

  'global', 'grant', 'group', 'grouping', 'having', 'hour',

  'if', 'ilike', 'immediate', 'in', 'include', 'index',

  'indexes', 'infinity', 'inner', 'insert', 'instead',

  'int', 'integer', 'intersect', 'interval', 'into',

  'is', 'isnull', 'join', 'key', 'last', 'leading',

  'left', 'like', 'limit', 'listen', 'load', 'local',

  'localtime', 'localtimestamp', 'lock', 'match', 'materialized',

  'maxvalue', 'minvalue', 'mode', 'month', 'move', 'name',

  'names', 'new', 'next', 'no', 'not', 'nothing', 'notify',

  'null', 'nullif', 'numeric', 'of', 'offset', 'old',

  'on', 'only', 'or', 'order', 'ordinality', 'out',

  'over', 'overlaps', 'overlay', 'parameter', 'partition',

  'partitioned', 'plpgsql', 'preceding', 'primary', 'prepare',

  'prepared', 'raise', 'range', 'read', 'real', 'reassign',

  'recursive', 'ref', 'references', 'referencing', 'release',

  'rename', 'repeatable', 'replace', 'reset', 'return',

  'returns', 'revoke', 'right', 'row', 'rows', 'rule',

  'schema', 'scroll', 'search', 'select', 'sequence',

  'sequences', 'set', 'setof', 'show', 'similar', 'simple',

  'smallint', 'some', 'stable', 'standalone', 'star',

  'start', 'statement', 'static', 'stddev', 'stddev_pop',

  'stddev_samp', 'substring', 'symmetric', 'table',

  'tablesample', 'then', 'time', 'timestamp', 'to',

  'trailing', 'transaction', 'treat', 'trigger', 'true',

  'union', 'unique', 'unknown', 'unlisten', 'until',

  'update', 'user', 'using', 'value', 'values',

  'varchar', 'varying', 'view', 'when', 'where',

  'whitespace', 'window', 'with', 'without', 'year', 'token', 'number'

]);

  
  
  

// 특정 CSV 파일에서 데이터 읽기

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

  

// 하나의 {}에 대응하는 함수

// const getTablesWithSVariantSingle = async (tableName) => {

//     const query = `SELECT id FROM ${tableName};`;

//     log("실행된 쿼리:", query);

//     const { rows } = await pool.query(query);

//     return rows;

// };

const getTablesWithSVariantSingle = async (tableName) => {

    // 테이블이 존재하는지 확인

    const checkTableQuery = `

        SELECT EXISTS (

            SELECT 1

            FROM pg_catalog.pg_tables

            WHERE tablename = $1

        )`;

    const checkTableRes = await pool.query(checkTableQuery, [tableName]);

  

    if (!checkTableRes.rows[0].exists) {

        log(`테이블 ${tableName}이 존재하지 않습니다.`);

        return []; // 테이블이 없으면 빈 배열을 반환

    }

  

    // 테이블에 'id' 컬럼이 존재하는지 확인

    const checkColumnQuery = `

        SELECT EXISTS (

            SELECT 1

            FROM information_schema.columns

            WHERE table_name = $1 AND column_name = 'id'

        )`;

    const checkColumnRes = await pool.query(checkColumnQuery, [tableName]);

  

    if (!checkColumnRes.rows[0].exists) {

        log(`테이블 ${tableName}에 'id' 컬럼이 존재하지 않습니다.`);

        return []; // 컬럼이 없으면 빈 배열을 반환

    }

  

    const query = `SELECT id FROM ${tableName};`;

    log("실행된 쿼리:", query);

    const { rows } = await pool.query(query);

    return rows;

};

  
  

// 두 개의 {}에 대응하는 함수

// const getTablesWithSVariantDouble = async (tableName1, tableName2) => {

//     const query1 = `SELECT id FROM ${tableName1};`;

//     const query2 = `SELECT id FROM ${tableName2};`;

  

//     log("실행된 첫 번째 쿼리:", query1);

//     const { rows: rows1 } = await pool.query(query1);

  

//     log("실행된 두 번째 쿼리:", query2);

//     const { rows: rows2 } = await pool.query(query2);

  

//     return { table1Data: rows1, table2Data: rows2 };

// };

  

const getTablesWithSVariantDouble = async (tableName1, tableName2) => {

    // 첫 번째 테이블 존재 여부 확인

    const checkTableQuery1 = `

        SELECT EXISTS (

            SELECT 1

            FROM pg_catalog.pg_tables

            WHERE tablename = $1

        )`;

    const checkTableRes1 = await pool.query(checkTableQuery1, [tableName1]);

  

    if (!checkTableRes1.rows[0].exists) {

        log(`테이블 ${tableName1}이 존재하지 않습니다.`);

        return { table1Data: [], table2Data: [] }; // 첫 번째 테이블이 없으면 빈 데이터 반환

    }

  

    // 두 번째 테이블 존재 여부 확인

    const checkTableQuery2 = `

        SELECT EXISTS (

            SELECT 1

            FROM pg_catalog.pg_tables

            WHERE tablename = $1

        )`;

    const checkTableRes2 = await pool.query(checkTableQuery2, [tableName2]);

  

    if (!checkTableRes2.rows[0].exists) {

        log(`테이블 ${tableName2}이 존재하지 않습니다.`);

        return { table1Data: [], table2Data: [] }; // 두 번째 테이블이 없으면 빈 데이터 반환

    }

  

    // 첫 번째 테이블에 'id' 컬럼이 존재하는지 확인

    const checkColumnQuery1 = `

        SELECT EXISTS (

            SELECT 1

            FROM information_schema.columns

            WHERE table_name = $1 AND column_name = 'id'

        )`;

    const checkColumnRes1 = await pool.query(checkColumnQuery1, [tableName1]);

  

    if (!checkColumnRes1.rows[0].exists) {

        log(`테이블 ${tableName1}에 'id' 컬럼이 존재하지 않습니다.`);

        return { table1Data: [], table2Data: [] }; // 첫 번째 테이블에 'id' 컬럼이 없으면 빈 데이터 반환

    }

  

    // 두 번째 테이블에 'id' 컬럼이 존재하는지 확인

    const checkColumnQuery2 = `

        SELECT EXISTS (

            SELECT 1

            FROM information_schema.columns

            WHERE table_name = $1 AND column_name = 'id'

        )`;

    const checkColumnRes2 = await pool.query(checkColumnQuery2, [tableName2]);

  

    if (!checkColumnRes2.rows[0].exists) {

        log(`테이블 ${tableName2}에 'id' 컬럼이 존재하지 않습니다.`);

        return { table1Data: [], table2Data: [] }; // 두 번째 테이블에 'id' 컬럼이 없으면 빈 데이터 반환

    }

  

    // 첫 번째 쿼리 실행

    const query1 = `SELECT id FROM ${tableName1};`;

    log("실행된 첫 번째 쿼리:", query1);

    const { rows: rows1 } = await pool.query(query1);

  

    // 두 번째 쿼리 실행

    const query2 = `SELECT id FROM ${tableName2};`;

    log("실행된 두 번째 쿼리:", query2);

    const { rows: rows2 } = await pool.query(query2);

  

    return { table1Data: rows1, table2Data: rows2 };

};

  
  
  

// API 요청에 대한 ID 값을 가져오는 함수

const getTablesWithSVariant = async () => {

    const query = `select id as pid from locations ;`; // orders 테이블에서 id를 조회

    log("실행된 쿼리:", query);

    const { rows } = await pool.query(query); // DB에서 쿼리 실행

    return rows; // 쿼리 결과 반환

};

  

// 주어진 URL에서 API 요청 처리 및 JSON 데이터 저장

// 데이터 처리 및 API 요청

const processData = async (csvFilePath, outputDir) => {

    try {

      const csvData = await readAllCsvFilesInFolder(csvFilePath, ['URL', 'METHOD', '분류1', '분류2', '분류3']);

      const getRequests = csvData.filter(item =>

        item.METHOD && typeof item.METHOD === 'string' && item.METHOD.toLowerCase() === 'get' && /{.*?}/.test(item.URL)

      );

      for (const item of getRequests) {

        let count = 0;

        let { '분류1': category1, '분류2': category2, '분류3': category3, URL } = item;

        if (URL.includes('count')) {

          console.log(`URL에 'count' 포함됨, 건너뜁니다: ${URL}`);

          continue;

        }

        const matches = URL.match(/{(.*?)}/g);

        if (!matches || matches.length === 0) {

          console.log(`URL에서 대체할 값 없음: ${URL}`);

          continue;

        }

        let finalUrl = URL;

        let idMap = {};

        if (matches.length === 1) {

          const key = matches[0].replace(/[{}]/g, '');

          const tableName = `${key.replace(/_id$/, 's')}`;

          // 테이블 데이터 불러오기

          const tableData = await getTablesWithSVariantSingle(tableName);

          if (tableData.length === 0) {

            console.log(`테이블 ${tableName}에서 데이터를 찾을 수 없음`);

            continue;

          }

          // 각 데이터를 동적으로 바꿔서 API 요청

          for (const data of tableData) {

            const value = data.id;

            idMap[key] = value;

            // URL을 동적으로 바꾸기 전에 최신 값으로 업데이트

            let tempUrl = finalUrl;

            if (tempUrl.includes(`{${key}}`)) {

              tempUrl = tempUrl.replace(`{${key}}`, value);

              console.log(`최종 URL (${key} 대체됨): ${tempUrl}`);

            }

            // API 요청 실행

            await handleApiRequest(tempUrl, category1, category2, category3);

          }

        } else if (matches.length === 2) {

          const key1 = matches[0].replace(/[{}]/g, '');

          const key2 = matches[1].replace(/[{}]/g, '');

          const tableName1 = `${key1.replace(/_id$/, 's')}`;

          const tableName2 = `${key2.replace(/_id$/, 's')}`;

          // 각 테이블 데이터를 동적으로 가져오기

          const { table1Data, table2Data } = await getTablesWithSVariantDouble(tableName1, tableName2);

          if (table1Data.length === 0 || table2Data.length === 0) {

            console.log(`테이블 ${tableName1} 또는 ${tableName2}에서 데이터를 찾을 수 없음`);

            continue;

          }

          // 두 테이블의 데이터를 동적으로 처리

          for (const data1 of table1Data) {

            const value1 = data1.id;

            idMap[key1] = value1;

            for (const data2 of table2Data) {

              const value2 = data2.id;

              idMap[key2] = value2;

              // URL을 동적으로 바꾸기 전에 최신 값으로 업데이트

              let tempUrl = finalUrl;

              if (tempUrl.includes(`{${key1}}`)) {

                tempUrl = tempUrl.replace(`{${key1}}`, value1);

                console.log(`최종 URL (${key1} 대체됨): ${tempUrl}`);

              }

              if (tempUrl.includes(`{${key2}}`)) {

                tempUrl = tempUrl.replace(`{${key2}}`, value2);

                console.log(`최종 URL (${key2} 대체됨): ${tempUrl}`);

              }

              // API 요청 실행

              await handleApiRequest(tempUrl, category1, category2, category3);

              count++;

            log(count, " : count 0000000000000000000000000000000000000000000000000000000000000000000000000000000000");

            }

          }

        } else {

          console.log(`지원하지 않는 {} 개수: ${matches.length}`);

          continue;

        }

      }

    } catch (err) {

      console.error('데이터 처리 중 에러 발생:', err.message || err);

    }

  };

  // API 요청과 데이터를 처리하는 별도의 함수 (최종 URL을 처리하고 json 데이터를 삽입)

  const handleApiRequest = async (finalUrl, category1, category2, category3) => {

    const baseURL = 'https://kpopmerch-inc.myshopify.com';

    const apiOptions = {

      method: 'GET',

      url: baseURL + finalUrl,

      headers: {

        'Content-Type': 'application/json',

        'X-Shopify-Access-Token': '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',

      },

    };

    try {

      const response = await axios(apiOptions);

      let jsonData = response.data;

      const tableName = Object.keys(jsonData)[0];

      const records = jsonData[tableName];

      const category1Dir = path.join(outputDir, category1);

      const category2Dir = path.join(category1Dir, category2);

      const category3Dir = path.join(category2Dir, category3);

      if (!fs.existsSync(category3Dir)) {

        fs.mkdirSync(category3Dir, { recursive: true });

      }

      const fileName = path.basename(finalUrl.includes('?') ? finalUrl.split('?')[0] : finalUrl);

      const jsonFileName = path.join(category3Dir, fileName);

      let existingData = {};

      if (fs.existsSync(jsonFileName)) {

        const existingFileContent = fs.readFileSync(jsonFileName, 'utf8');

        existingData = JSON.parse(existingFileContent);

      }

      if (!existingData[tableName]) {

        existingData[tableName] = Array.isArray(records) ? [] : {};

      }

      if (Array.isArray(records)) {

        existingData[tableName] = existingData[tableName].concat(records);

      } else {

        existingData[tableName] = records;

      }

      fs.writeFileSync(jsonFileName, JSON.stringify(existingData, null, 2), 'utf8');

      console.log(`JSON 데이터가 성공적으로 ${jsonFileName}에 저장되었습니다.`);

      await insertDataIntoTable(jsonData, finalUrl);

    } catch (apiErr) {

      console.error(`API 요청 중 에러 발생: ${finalUrl}:`, apiErr.message || apiErr);

    }

  };

  
  

// 데이터베이스에 API 응답 데이터를 삽입하는 함수

async function insertDataIntoTable(jsonData, url) {

    const tableName = Object.keys(jsonData)[0];

    let records = jsonData[tableName]; // 데이터

  

    console.log('Table Name:', tableName);

    console.log('Records:', records); // records의 내용을 로그로 출력

  

    let finalTableName = tableName;

  

    if (tableName === 'count') {

        // "count"일 경우 테이블 이름 변경

        finalTableName = `${url.split('/').slice(-2, -1)[0]}_count`;

        // count 값만 들어오는 경우 특별히 처리

        if (typeof records === 'number') {

          const query = `INSERT INTO ${finalTableName} (count) VALUES ($1)`;

          await pool.query(query, [records]); // 단일 숫자 값 삽입

          return;  // 더 이상 진행하지 않고 종료

        } else {

          console.error('Invalid data format for count:', records);

          return;

        }

      } else if (tableName === 'transactions') {

        finalTableName = `${url.split('/').slice(-3, -2)[0]}_transactions`;

      } else if (tableName === 'tags' || tableName === 'authors') {

        const query = `INSERT INTO ${tableName} (${tableName}) VALUES ($1)`;

        await pool.query(query, [records]); // 단일 레코드 삽입

        return;  

      }

  

    // records가 배열인지 또는 객체인지 확인

    if (Array.isArray(records)) {

        // 배열인 경우

        for (const record of records) {

            await insertRecord(record, finalTableName);

        }

    } else if (typeof records === 'object' && records !== null) {

        // 객체인 경우

        await insertRecord(records, finalTableName);

    } else {

        console.error('Invalid data format: expected an array or an object.');

        console.log('Received records:', records); // records의 내용을 로그로 출력

    }

}

function escapeIdentifier(identifier) {

    if (typeof identifier !== 'string') {

        throw new Error('Invalid identifier: must be a string.');

    }

    if (reservedWords.has(identifier.toLowerCase())) {

        return `"${identifier}"`; // 예약어일 경우 따옴표로 감싼다

    }

    return identifier; // 예약어가 아닐 경우 그대로 반환

}

  

async function insertRecord(record, finalTableName) {

    const keys = Object.keys(record);

    const values = keys.map((key) => {

        const value = record[key];

  

        if (Array.isArray(value)) {

            if (value.length > 0 && typeof value[0] === 'string') {

                return `{${value.map(item => item.toString().replace(/"/g, '""')).join(',')}}`;

            } else {

                return JSON.stringify(value);

            }

        } else if (typeof value === 'object' && value !== null) {

            return JSON.stringify(value);

        }

        return value;

    });

  

    const columns = keys.map(col => escapeIdentifier(col)).join(', ');

    finalTableName = escapeIdentifier(finalTableName);

    const placeholders = keys.map((_, i) => `$${i + 1}`).join(', ');

    const query = `INSERT INTO ${finalTableName} (${columns}) VALUES (${placeholders})`;

  

    try {

        await pool.query(query, values);

        console.log(`Record inserted into ${finalTableName} successfully.`);

    } catch (error) {

        console.error(`Error inserting data into ${finalTableName}:`, error.message);

        if (error.code === '23505') {

            console.log('Duplicate key error occurred, but ignored.');

        } else {

            console.error(`Error inserting data into ${finalTableName}:`, error);

        }

    }

}

  
  
  

// CSV 파일 경로와 출력 디렉토리 설정

const csvFolderPath = path.join(__dirname, '../csv2'); // 엑셀 폴더 경로

const outputDir = path.join(__dirname, '../shopify4'); // 파일 저장 폴더

// console.log('CSV Folder Path:', csvFolderPath);

// console.log('Output Directory Path:', outputDir);

  

// 특정 컬럼만 선택하는 함수

const readSelectedColumns = (csvFile, columns = []) => {

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

  const readAllCsvFilesInFolder = (folderPath, columns = []) => {

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

  

processData(csvFolderPath, outputDir);

  
  

module.exports = router;
```
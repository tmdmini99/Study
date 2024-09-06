
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
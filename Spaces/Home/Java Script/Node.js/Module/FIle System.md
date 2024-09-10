
#  Node.js 내장 모듈인 File System(fs) 사용법


## 사용법

###     Directory

- 디렉토리 체크 및 생성

```js
const fs = require('fs');

//Directory 존재 여부 체크
const directory = fs.existsSync("./sample")//디렉토리 경로 입력

//Directory가 존재 한다면 true 없다면 false
console.log("Boolan : ", directory);

//Directory 생성
fs.mkdirSync("생성 디렉토리 경로")
 
//보통 Directory가 없다면 새로 만들어야 한다면 아래와 같은 코드를 만들어 사용할 수 있다.
 
if(!directory) fs.mkdirSync("생성 디렉토리 경로");

OR

if(!fs.existsSync("./sample")) fs.mkdirSync("생성 디렉토리 경로");
```

- 디렉토리 삭제

```js
const fs = require('fs');

//비동기 방식으로 디렉토리 삭제
fs.rmdir("./sample",{ recursive: true }, err => {
    console.log("err : ", err);
})

//동기 방식으로 디렉토리 삭제
try {
    fs.rmdirSync("./sample", { recursive: true });

    console.log(`sample is deleted!`);
} catch (err) {
    console.error(`Error while deleting sample.`);
}
```

##    File

- 파일 정보 읽기

```js
const fs = require('fs');

//비동기 방식으로 파일 정보 읽기
fs.readFile('./sample.txt', 'utf8', (err, data) => {
    if (err) throw err;
    console.log(data); // 파일 데이터 정보
});

//동기 방식으로 파일 정보 읽기
const file = fs.readFileSync('./sample.txt', 'utf8')

console.log("file : ", file);
```


- 파일 생성

```js
const fs = require('fs');

//비동기 방식으로 파일 정보 읽기
const file = fs.readFileSync('./sample.txt', 'utf8')

/**
 * 비동기 방식으로 새로운 파일 만들기
 * "./sample2.txt" : 파일 생성할 경로
 * file : 파일 데이터
 */
fs.writeFile("./sample2.txt", file, (err) =>{
    console.log(err);
})

/**
 * 동기 방식으로 새로운 파일 만들기
 * "./sample2.txt" : 파일 생성할 경로
 * file : 파일 데이터
 */
fs.writeFileSync("./sample2.txt", file)
```

비동기 방식
```js
const fs = require('fs');
const path = require('path');

// 현재 디렉토리에 sample.txt 파일 경로 설정
const filePath = path.join(__dirname, 'sample.txt');

// "안녕하세요" 데이터를 sample.txt에 쓰기
fs.writeFile(filePath, '안녕하세요', 'utf8', (err) => {
  if (err) {
    console.error('파일 쓰기 중 오류 발생:', err);
  } else {
    console.log('sample.txt에 데이터가 성공적으로 저장되었습니다.');
  }
});

```

동기 방식
```js
const fs = require('fs');
const path = require('path');

const filePath = path.join(__dirname, 'sample.txt');

try {
  fs.writeFileSync(filePath, '안녕하세요', 'utf8');
  console.log('sample.txt에 데이터가 성공적으로 저장되었습니다.');
} catch (err) {
  console.error('파일 쓰기 중 오류 발생:', err);
}

```


- `fs.writeFile()`: **비동기**로 파일에 데이터를 쓰기 때문에 작업이 끝날 때까지 기다리지 않고, 콜백으로 완료를 처리합니다.
- `fs.writeFileSync()`: **동기**로 파일에 데이터를 쓰기 때문에 작업이 끝날 때까지 다른 코드가 실행되지 않으며, 바로 예외를 처리할 수 있습니다.


![[Node.js fs1.png]]

- 파일 삭제

```js
const fs = require('fs');

//비동기 방식으로 파일 삭제
fs.unlink("./smple2.txt", err => {
    
    if(err.code == 'ENOENT'){
        console.log("파일 삭제 Error 발생");
    }
});


try {

	//동기 방식으로 파일 삭제
    fs.unlinkSync("./sample222.txt")

} catch (error) {

    if(err.code == 'ENOENT'){
        console.log("파일 삭제 Error 발생");
    }
}
```

Error가 발생하지 않으면 파일은 자연스럽게 삭제가 됩니다.

에러가 발생하면 아래와 같이 출력됩니다.

보통 에러를 처리 안 하면 첫 번째 이미지와 같이 내용이 나옵니다.

![[Node.js fs2.png]]
Error 처리 X

![[Node.js fs3.png]]
Error 처리 O

- 파일 체크

```js
const fs = require('fs');

//비동기 방식으로 파일 체크
fs.stat("./sample.txt", (err, stats) =>{
    if (err.code === "ENOENT") {
        console.log("파일이 존재하지 않습니다.");
    }
})

// 동기 방식으로 파일 체크
try {

  fs.statSync("./sample.txt");

} catch (error) {

	//파일이 없다면 에러 발생
    if (error.code === "ENOENT") {
       console.log("파일이 존재하지 않습니다.");
    }
}
```


---
출처 - https://any-ting.tistory.com/21
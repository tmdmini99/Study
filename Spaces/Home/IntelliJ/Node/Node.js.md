## Node.js 설치 프로세스
### Node.js 다운로드 및 설치

#### 1. 공식 웹사이트 접속

- [Node.js 공식 홈페이지에 방문](https://nodejs.org/)하여 다운로드 페이지로 이동합니다.

#### 2. 원하는 버전 선택

![[Node.js 설치1.png]]

- LTS 버전
    - 장기 지원을 받는 안정적인 버전입니다.
    - 기업용 또는 중요 애플리케이션 개발에 권장됩니다.
- Current 버전
    - 최신 기능을 제공하며, 개인 프로젝트나 최신 기술을 실험하고 싶은 개발자에게 적합합니다.


![[Node.js 설치2.png]]



## Node.js 환경 확인

![[Node.js 설치3.jpg]]


- Node.js 버전 확인
    - 명령 프롬프트나 터미널을 열고 node -v를 입력합니다.
- NPM 버전 확인
    - npm -v 명령어로 NPM(노드 패키지 매니저)의 버전을 확인합니다.


## NPM 업데이트

Node.js를 사용하면서 NPM은 자주 업데이트되므로, 다음 명령어로 최신 상태를 유지하는 것이 좋습니다


```sql
npm install -g npm
```


![[Node.js 설치4.png]]

## 1. IntelliJ에서 Node.js 개발 환경 설정하기 & 프로젝트 생성

### 1. Plug-in 설치

IntelliJ에서 Node.js 개발을 위해 필요한 플러그인을 설치해야 합니다.

플러그인은 IntelliJ에 특정 언어 또는 기능을 추가할 수 있는 확장 모듈이기 때문에 설치를 함으로써 Nodejs를 사용할 수 있습니다.

Setting 열기: Ctrl + Alt + S를 눌러 IntelliJ의 설정 창을 엽니다.


![[Node.js 설치5.png]]

#### 플러그인 설치

설정 창에서 왼쪽 패널의 Plugins 탭을 선택한 후, 상단의 Marketplace 탭을 클릭합니다.

여기서 NodeJS 플러그인을 검색하여 설치합니다.

#### EJS 플러그인 설치

EJS(Embedded JavaScript)는 HTML 내에서 JavaScript를 실행할 수 있게 해주는 템플릿 엔진입니다.

EJS를 사용하려면 EJS 플러그인도 설치합니다.


#### Node interpreter 설정

설치가 잘 되었다면 다시 세팅창으로가서 Languages & Frameworks > Node.js 탭을 선택합니다.

Node.js 설치 경로에서 node.exe 파일을 지정합니다. 기본 설치 경로는 "C:\Program Files\nodejs\node.exe"입니다.


![[Node.js 설치6.png]]

### 3. 프로젝트 생성

IntelliJ에서 새로운 Node.js 프로젝트를 생성하는 방법입니다.

새 프로젝트 생성: IntelliJ의 메뉴에서 File > New > Project를 선택합니다.

![[Node.js 설치7.png]]

왼쪽에 Express를 선택합니다.

![[Node.js 설치8.png]]


템플릿으로 EJS를 선택합니다.

EJS는 HTML 내에 JavaScript를 내장하여 동적 웹 페이지를 쉽게 만들 수 있게 해줍니다.

![[Node.js 설치9.png]]

## 2. 프로젝트 실행 방법

### IntelliJ에서 실행

프로젝트 실행: Shift + F10을 눌러 프로젝트를 실행합니다. IntelliJ는 프로젝트의 기본 설정에 따라 서버를 실행합니다.

기본 URL 확인: 브라우저에서 \http://localhost:3000/을 입력하여 기본 Index 페이지가 정상적으로 호출되는지 확인합니다. 기본 Index 페이지는 views > index.ejs 파일입니다.

![[Node.js 설치10.png]]





---
출처 - https://devit.koreacreatorfesta.com/entry/Nodejs-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0#Node.js%20%EC%84%A4%EC%B9%98%20%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-1


https://devit.koreacreatorfesta.com/entry/Nodejs-Intellij%EC%97%90%EC%84%9C-%EC%84%A4%EC%A0%95-%EB%B0%8F-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%83%9D%EC%84%B1-%EB%B0%A9%EB%B2%95
## Node.js 설치 프로세스
### Node.js 다운로드 및 설치

#### 1. 공식 웹사이트 접속

- [Node.js 공식 홈페이지에 방문](https://nodejs.org/)하여 다운로드 페이지로 이동합니다.

#### 2. 원하는 버전 선택

![[Pasted image 20240905140455.png]]

- LTS 버전
    - 장기 지원을 받는 안정적인 버전입니다.
    - 기업용 또는 중요 애플리케이션 개발에 권장됩니다.
- Current 버전
    - 최신 기능을 제공하며, 개인 프로젝트나 최신 기술을 실험하고 싶은 개발자에게 적합합니다.


![[Pasted image 20240905140525.png]]



## Node.js 환경 확인

![[Pasted image 20240905140843.jpg]]


- Node.js 버전 확인
    - 명령 프롬프트나 터미널을 열고 node -v를 입력합니다.
- NPM 버전 확인
    - npm -v 명령어로 NPM(노드 패키지 매니저)의 버전을 확인합니다.


## NPM 업데이트

Node.js를 사용하면서 NPM은 자주 업데이트되므로, 다음 명령어로 최신 상태를 유지하는 것이 좋습니다


```sql
npm install -g npm
```


![[Pasted image 20240905141007.png]]

## 1. IntelliJ에서 Node.js 개발 환경 설정하기 & 프로젝트 생성

### 1. Plug-in 설치

IntelliJ에서 Node.js 개발을 위해 필요한 플러그인을 설치해야 합니다.

플러그인은 IntelliJ에 특정 언어 또는 기능을 추가할 수 있는 확장 모듈이기 때문에 설치를 함으로써 Nodejs를 사용할 수 있습니다.

Setting 열기: Ctrl + Alt + S를 눌러 IntelliJ의 설정 창을 엽니다.


![[Pasted image 20240905141117.png]]

#### 플러그인 설치

설정 창에서 왼쪽 패널의 Plugins 탭을 선택한 후, 상단의 Marketplace 탭을 클릭합니다.

여기서 NodeJS 플러그인을 검색하여 설치합니다.

#### EJS 플러그인 설치

EJS(Embedded JavaScript)는 HTML 내에서 JavaScript를 실행할 수 있게 해주는 템플릿 엔진입니다.

EJS를 사용하려면 EJS 플러그인도 설치합니다.


#### Node interpreter 설정

설치가 잘 되었다면 다시 세팅창으로가서 Languages & Frameworks > Node.js 탭을 선택합니다.

Node.js 설치 경로에서 node.exe 파일을 지정합니다. 기본 설치 경로는 "C:\Program Files\nodejs\node.exe"입니다.


![[Pasted image 20240905141236.png]]







---
출처 - https://devit.koreacreatorfesta.com/entry/Nodejs-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0#Node.js%20%EC%84%A4%EC%B9%98%20%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-1
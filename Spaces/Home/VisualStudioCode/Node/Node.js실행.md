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


### **VS Code (Visual Studio Code) 설치**

다음으로는 Node.js의 가장 많이 사용되는 개발 툴인 VS Code를 설치하겠습니다. MS사에서 만든 오픈소스 IDE로 설치도 간편하고 가볍고 사용이 편리합니다. 아래의 공식 사이트에서 자신의 OS에 맞는 설치 파일을 다운로드합니다.

![[vscode1.png]]
다운로드한 설치 파일을 실행하여, 아래와 같이 라이선스 동의 후 설치 폴더 선택 후 다음 버튼을 계속 클릭하여 설치를 해줍니다. 설치가 완료되면 위와 같이 VS Code가 정상적으로 실행되는 것을 보실 수 있습니다.


![[vscode2.jpg]]

![[vscode3.jpg]]

![[vscode4.jpg]]

![[vscode5.jpg]]

![[vscode6.jpg]]

### **Node.js 프로젝트 생성 및 실행**

VS code에서 Node.js 프로젝트를 생성하고 실행하는 것은 매우 간단합니다.

먼저, 윈도 파일 탐색기에서 Node.js 프로젝트로 사용할 임의의 **폴더를 생성**해 줍니다.

![[vscode7.jpg]]

![[vscode8.jpg]]

아래와 같이 **Explorer 탭**이 오픈이 되고, 방금 내가 오픈한 폴더가 리스트에 있는 것을 확인하실 수 있습니다.

![[vscode9.jpg]]

다음으로 VS Code내에서 **Terminal 오픈**을 하도록 하겠습니다.

상단 메뉴바에서 **View > Terminal**을 눌러서 터미널을 오픈해줍니다.

그러면 아래와 같이 하단에 터미널이 오픈이 됩니다. 앞으로 진행될 명령은 CMD 창을 따로 오픈해서 입력하셔도 상관은 없지만, 이렇게 VS Code 내에서 진행을 하는 것이 창이동 없이 더 간편합니다.

![[vscode10.jpg]]

![[vscode11.png]]

이제 Node.js를 설치할 때 자동으로 설치되는 자바 패키지 관리 모듈인 **NPM**을 이용해서 프로젝트의 **package.json** 파일을 생성하도록 하겠습니다. package.json 파일은 Node.js의 프로젝트 기본 정보, 의존성 정보 등을 저장하는 json 파일로 JAVA Maven 프로젝트의 pom.xml과 유사한 역할이라고 생각하시면 됩니다.

하단 터미널 창에 **NPM init이라는** 명령을 치면, 프로젝트의 기본정보를 작성하도록 여러 항목들이 나옵니다. 실제로 작성을 하셔도 되고, 엔터를 계속 클릭하셔서 Default 값으로 세팅된 파일을 만들도록 해도 됩니다.

다 작성이 되면 프로젝트 폴더에 package.json 파일이 생성됩니다.

```
npm init
```


![[vscode12.png]]


_(Dark 테마가 보기가 편해서 급 Dark 테마로 변경했습니다. 테마 변경은 File > Prefernces > Color Theme..)_

프로젝트 최초 실행 자바스크립트 파일인 **index.js** 파일을 생성하도록 하겠습니다. 

(package.json 파일 main 항목에 설정되어 있습니다.)

우측 Explore 탭에서 **New file** 아이콘을 클릭하여, **index.js라는** 이름의 Javascript 파일을 생성합니다.

_(Dark 테마가 보기가 편해서 급 Dark 테마로 변경했습니다. 테마 변경은 File > Prefernces > Color Theme..)_

프로젝트 최초 실행 자바스크립트 파일인 **index.js** 파일을 생성하도록 하겠습니다. 

(package.json 파일 main 항목에 설정되어 있습니다.)

우측 Explore 탭에서 **New file** 아이콘을 클릭하여, **index.js라는** 이름의 Javascript 파일을 생성합니다.


![[vscode13.png]]

![[vscode14.png]]


그러면 아래와 같이 Launch Type을 선택하도록 되어 있는데, 당연히 **Node.js**를 선택합니다. 그러면 launch.json 파일이 생성되는 것을 확인할 수 있습니다.

![[vscode15.png]]

이제 VS code에서 프로젝트 실행은 Run and Debug 탭으로 이동하여 해당 Launch name을 선택 후 **F5** 버튼 혹은 아래 **화살표 버튼**을 클릭하면 Node.js 프로젝트가 정상적으로 실행이 되는 것을 확인할 수 있습니다.


![[vscode16.jpg]]



---
출처 - https://kim-oriental.tistory.com/14
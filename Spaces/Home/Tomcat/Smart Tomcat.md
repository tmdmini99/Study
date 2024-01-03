스프링 부트와 AWS로 혼자 구현하는 웹서비스"를 보면서 공부하고 있었는데, 톰캣 연결이 안되어있어서 테스트를 할 수가 없었다..

Intellij 무료 버전인 Intellij IDEA Community Edition에서는 Tomcat 서버를 제공하지 않는다. 그래서 "Smart Tomcat"이나 "Tomcat Runner" 플러그인을 설치해서 사용해야한다. community에서 톰캣을 연결하는 방법에 대해서 최신 버전의 정보가 많이 없어서 3일만에 드디어 연결을 성공했다...! 같은 어려움을 겪고 있는 사람들에게 도움이 되고자 연결 과정을 정리해본다.

사용한 Tomcat 버전: 9.0.5  
사용한 IntelliJ 버전: 2021.3.1  
사용한 기기: 맥북 m1

## 1. Tomcat 설치하기

[https://tomcat.apache.org/](https://tomcat.apache.org/)  
원하는 버전의 Tomcat을 선택하여 설치하고 압축을 푼다.

## 2. Smart Tomcat 플러그인 설치

1) IntelliJ IDEA > Preferences... 클릭  
(이전 버전의 intelliJ에서 settings 이었던 메뉴가 Preferences로 바뀐 것 같다.)  
![[Smart T.png]]
2) Plugins에 들어가서 `Smart Tomcat` 설치  
![](https://velog.velcdn.com/images%2Fyoujung%2Fpost%2Ff8299646-fd4b-4a3e-89bd-b646e7ea3bf3%2FScreen%20Shot%202022-01-31%20at%205.59.13%20PM.png)

## 3. Smart Tomcat 설정

기본적으로 Name, Tomcat Server, Deployment Directory, Before launch 총 3가지 옵션은 설정해주어야 한다.

#### 1) Run > Edit Configurations 클릭

![](https://velog.velcdn.com/images%2Fyoujung%2Fpost%2F455e8c75-3514-44fa-8624-f125d95666b8%2FScreen%20Shot%202022-01-31%20at%206.00.38%20PM.png)

#### 2) `+` 버튼 눌러서 Smart Tomcat configuration 추가하기

![](https://velog.velcdn.com/images%2Fyoujung%2Fpost%2F79c03fc8-ae03-41e5-b04b-cacd79046250%2FScreen%20Shot%202022-01-31%20at%206.02.05%20PM.png)

#### 3) Tomcat Server 오른쪽 Configuration 클릭

(아래 사진은 이미 설정이 완료된 사진입니다. 메뉴의 위치를 참고해주세요.)  
![](https://velog.velcdn.com/images%2Fyoujung%2Fpost%2Fb6b0c227-a15c-4044-a851-f69aea74c613%2FScreen%20Shot%202022-01-31%20at%206.03.20%20PM.png)

#### 4) Tomcat Server 추가하기

위에서 Configuration을 눌렀다면 아래와 같은 화면이 뜰 것이다.  
(만약 이미 존재하는 게 있다면, 혹시 모르니 먼저 지워주세요.)  
`+`버튼을 눌러서 설치되어있는 Tomcat 폴더를 선택한다.  
추가가 되었다면 `Apply`를 눌러 적용시킨 후에 `OK`를 눌러 닫는다.  
![](https://velog.velcdn.com/images%2Fyoujung%2Fpost%2F8711944e-1426-47b0-9525-67c1ba29217c%2FScreen%20Shot%202022-01-31%20at%206.05.40%20PM.png)

#### 5) Deployment Directory 설정

web project일 경우 → src/main안에 있는 webapp폴더를 선택  
gradle project일 경우 → 프로젝트 폴더 선택

#### 6) Before launch 설정

시작 전에 무엇을 할지 설정하는 것인데, Gradle 프로젝트라 `+`를 눌러 `Run Gradle task`를 추가해주었다.  
Gradle project에는 프로젝트 폴더를 선택해주고, Tasks에 bootRun을 써준 뒤 `OK`를 눌러 종료한다.  
![](https://velog.velcdn.com/images%2Fyoujung%2Fpost%2Fcfa041a2-eadb-4d08-8a03-991c027e5bf2%2FScreen%20Shot%202022-01-31%20at%206.23.06%20PM.png)

## 4. Tomcat 서버 실행

오른쪽 위의 망치 아이콘 오른쪽에 화살표를 클릭하면 아래처럼 목록이 나오는데, 여기서 아까 추가한 Tomcat server를 클릭한 뒤 `run`버튼을 누른다.  
![](https://velog.velcdn.com/images%2Fyoujung%2Fpost%2F336a816e-785e-4c0d-96bb-71caacd7f079%2FScreen%20Shot%202022-01-31%20at%206.26.09%20PM.png)  
아래처럼 나오면 성공이다.  
Tomcat started on port(s): 8080 ~~나오는데, 확인을 위해서 접속을 해보자.  
![](https://velog.velcdn.com/images%2Fyoujung%2Fpost%2Fca231802-c144-488d-9098-99432c424b69%2FScreen%20Shot%202022-01-31%20at%206.29.01%20PM.png)  
만약 lombok과 관련된 에러가 뜬다면, dependencies 메뉴로 들어가 lombok의 버전을 설정해주면 된다. 1.18.20으로 설정해주었더니 그 후로는 에러가 뜨지 않았다.  
![](https://velog.velcdn.com/images%2Fyoujung%2Fpost%2Fe6f3b16e-4bad-4f96-8007-c5fdca821e6a%2FScreen%20Shot%202022-01-31%20at%206.30.32%20PM.png)

## 5. 확인

아래와 같이 HelloController에 "/hello"로 요청이 들어오면 "hello world!"를 반환하게 해놨다.  
![](https://velog.velcdn.com/images%2Fyoujung%2Fpost%2Fb250d738-a1ff-4850-95b9-59d06b10d71b%2FScreen%20Shot%202022-01-31%20at%206.34.31%20PM.png)  
그냥 `localhost:8080`으로 가니 아래와 같이 에러페이지가 떠서 당황했는데,  
![](https://velog.velcdn.com/images%2Fyoujung%2Fpost%2F1088477c-c770-43af-9b1f-1afcc552253c%2FScreen%20Shot%202022-01-31%20at%206.37.14%20PM.png)  
`localhost:8080/hello`로 들어가니 아래와 같이 잘 출력되는 걸 확인할 수 있었다.






---
참조 - https://velog.io/@youjung/Intellij-IDEA-Community-Edition%EC%97%90%EC%84%9C-Tomcat-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0
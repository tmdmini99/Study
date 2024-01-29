




docekr desktop 설치





### 명령 프롬프트에서 

버전 확인
```
docker -v
```




1. Image 검색

a. Web의 DockerHub에서 원하는 이미지 검색

[https://www.docker.com/products/docker-hub/](https://www.docker.com/products/docker-hub/)


b. CMD에서 명령어로 이미지 검색

\# Docker search 검색어

docker search mariadb

![[Docker-Compose설치4.png]]



### 이미지 다운

![[Docker-Compose설치3.png]]


Docker Desktop에는 윈도우나 맥에서 개발 환경으로 많이 사용되는데, Docker Compose를 기본적으로 포함하고 있습니다.

### docker 이미지 보기
```
docker images
```



### 컨테이너 파일로 생성
```
 docker create -it --name ubuntu_server ubuntu
```

## Docker 명령어


## 1. 도커 이미지 검색

> # docker images

현재 Host에 다운받은 이미지들을 출력하는 명령어

![[Docker명령어1.png]]

## 1-1. 도커 단일 이미지 삭제

> # docker image rm \<image ID>


![[Docker명령어2.png]]

추가적으로 해당 이미지를 컨테이너에서 사용하고 있으면 이미지를 삭제할 수 없습니다.

## 1-2. 도커 모든 이미지 삭제

> # docker rmi $(docker images -q) -f

(docker image -q)라는 명령어는 이미지의 ID를 출력하는 명령어입니다.




## 2. 도커 컨테이너 생성

생성과 동시에 실행까지 할수 있다

> # docker run <옵션> --name <컨테이너이름:test> <이미지 Repository>

**옵션**

-i : 사용자가 입출력을 할 수 있는 상태

-t : 가상 터미널 환경을 에뮬레이션하겠다는 말

-d : 컨테이너를 일반 프로세스가 아닌 데몬프로세스(백그라운드) 형태로 실행해 프로세스가 끝나도 유지되도록 한다.


![[Docker명령어3.png]]


## [2-1. 도커 컨테이너 생성만](https://narup.tistory.com/198#----%--%EB%-F%--%EC%BB%A-%--%EC%BB%A-%ED%--%-C%EC%-D%B-%EB%--%--%--%EC%--%-D%EC%--%B-%EB%A-%-C)

> # docker create <옵션> --name <컨테이너이름:test> <이미지 Repository>




- docker ps  
    : 실행중인 컨테이너 목록 확인
    
- docker ps -a  
    : 전체 컨테이너 목록 확인
    
- docker container ls -a  
    : 전체 컨테이너 목록 확인
    
- docker start 컨테이너ID  
    : 컨테이너 시작
    
- docker attach 컨테이너ID  
    : 컨테이너 접속
    
- docker stop 컨테이너ID  
    : 컨테이너 멈춤
    
- docker run 컨테이너ID  
    : 컨테이너 생성 및 시작
    
- docker run -it 컨테이너ID  
    : 컨테이너 생성 및 시작 및 접속
    
- docker rm 컨테이너ID  
    : 컨테이너 삭제
    
- docker exec -it 컨테이너ID /bin/bash  
    : 실행되고 있던 컨테이너 접속
    
- exit  
    : 컨테이너 빠져나오기





## docker로만 container 만들시

도커 실행  컨테이너 이름은 tomcat-test로 하고 
-p는 포트 지정 8080:8080은 8080에서 8080으로
뒤에 tomcat:9.0.84는 버전을 의미
```
docker run -d --name="tomcat-test" -p 8080:8080 tomcat:9.0.84
```


war파일을 복사

```
docker cp C:\Project\CrawlerTest\target/CrawlerTest-1.0-SNAPSHOT.war tomcat-test:/usr/local/tomcat/webapps/ROOT.war
```


mysql 설치

\<password> 부분에 내가 원하는 값 입력
\--name에 컨테이너 이름 입력 mysql-container로 설정 해놈
```
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=<password> -d -p 3307:3306 mysql:latest
```


mysql
```
docker run --network=mynetwork --name likesql -e MYSQL_ROOT_PASSWORD=1234 -d -p 3307:3306 mysql:latest
```


tomcat
```
docker run --network=mynetwork --name tomcat-test -d -p 8080:8080 tomcat:9.0.84
```

network
```
docker network create mynetwork
```


컨테이너 각자 생성시 네트워크를 묶어줘야지만 사용 가능





---
참조 - https://firework-ham.tistory.com/62
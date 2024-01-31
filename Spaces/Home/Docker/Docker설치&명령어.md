




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


## 2-1. 도커 컨테이너 생성만

> # docker create <옵션> --name <컨테이너이름:test> <이미지 Repository>


![[Docker명령어4.png]]



## 3. 컨테이너 접속

> # docker exec -it <컨테이너이름> /bin/bash

![[Docker명령어5.png]]
`exec` 명령은 도커 컨테이너 내부에서 새로운 명령을 실행하는 데 사용됩니다. 
즉, 컨테이너 내에서 새로운 프로세스를 시작하는 것입니다. 
이 명령을 사용하면 컨테이너 내부에서 명령을 실행하고 결과를 확인할 수 있습니다. 
예를 들어, `docker exec`를 사용하여 컨테이너 내부의 셸에 접속하거나, 특정 명령을 실행할 수 있습니다.




> # docker attach 컨테이너ID  

컨테이너의 실행 중인 프로세스에 연결하는 데 사용됩니다. 
즉, 이미 실행 중인 프로세스에 터미널 입력과 출력을 연결하는 것입니다. 
이 명령을 사용하면 컨테이너의 표준 입력, 출력, 오류 스트림을 터미널에 연결하여 실시간으로 컨테이너의 출력을 확인할 수 있다
`docker attach` 명령은 이미 실행 중인 프로세스에 연결하기 때문에, 컨테이너를 종료하면 터미널 연결도 종료된다.


## 4. 컨테이너 빠져나오기

컨테이너에서 빠져나오는 방법은 두가지가 있습니다.

1) 컨테이너를 종료하면서 빠져나오기

> # exit 또는 ctrl+D


![[Docker명령어6.png]]




2) 컨테이너가 가동되는 상태를 유지하면서 접속만 종료하기

> # ctrl + P 입력 후 Q 입력


![[Docker명령어7.png]]



## 5. 컨테이너 실행/종료

1) 실행

> # docker start <컨테이너이름>

2) 종료

> # docker stop <컨테이너이름>

## 6. 컨테이너 조회

1) 실행중인 컨테이너 리스트 출력

> # docker ps

2) 전체 컨테이너 목록 확인

> # docker ps -a


## 7. 컨테이너 삭제

> # docker rm <컨테이너이름>


![[Docker명령어8.png]]










## docker로만 container 만들시

도커 실행  컨테이너 이름은 tomcat-test로 하고 
-p는 포트 지정 8080:8080은 8080에서 8080으로
뒤에 tomcat:9.0.84는 버전을 의미
```
docker run -d --name="tomcat-test" -p 8080:8080 tomcat:9.0.84
```


war파일을 복사 내가 저장해둔 위치를 지정

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

user와 database 등록하면서 생성
```
docker run -d  --name my-mysql  -e MYSQL_ROOT_PASSWORD=my-secret-pw  -e MYSQL_USER=myuser   -e MYSQL_PASSWORD=mypassword   -e MYSQL_DATABASE=mydatabase   mysql:latest
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
만약 이미 컨테이너를 만들었을 경우
```
docker network connect my-mynetwork tomcat
docker network connect my-mynetwork likesql

```

이런식으로 네트워크 설정 가능



database.xml로 변경 해줘야함
이런식으로 같은 네트워크에 있는 컨테이너명으로 변경
    <property name="jdbcUrl" value="jdbc:mysql://likesql/test"></property> 
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">  
  
  
    <!-- mybatis 사용하기 위해 객체 생성 -->  
  
    <!-- Connection -->    <bean class="com.zaxxer.hikari.HikariDataSource" id="dataSource">  
        <property name="username" value="root"></property>  
        <property name="password" value="1234"></property>  
        <property name="jdbcUrl" value="jdbc:mysql://likesql/test"></property>  
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />  
  
        <property name="maximumPoolSize" value="5" />  
                <property name="minimumIdle" value="5" />  
                <property name="connectionTimeout" value="30000" />  
                <property name="idleTimeout" value="600000" />  
                <property name="maxLifetime" value="1800000" />  
  
    </bean>  
  
    <bean class="org.mybatis.spring.SqlSessionFactoryBean" id="sqlSessionFactoryBean">  
  
        <property name="dataSource" ref="dataSource"></property>  
        <property name="configLocation" value="classpath:database/config/MybatisConfig.xml"></property>  
        <property name="mapperLocations" value="classpath:database/mappers/*Mapper.xml"></property>  
    </bean>  
  
    <bean class="org.mybatis.spring.SqlSessionTemplate" id="sqlSession">  
        <constructor-arg name="sqlSessionFactory" ref="sqlSessionFactoryBean"></constructor-arg>  
    </bean>  
  
</beans>
```



---
참조 - https://firework-ham.tistory.com/62
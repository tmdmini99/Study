
## 설치







### 구조

- **정의 파일(compose file)** : 컨테이너나 볼륨을 어떠한 설정으로 만들지에 대한 항목뿐만 아니라 시스템에 대한 모든 정보가 기재됨. 작성 내용은 도커 명령어와 비슷하지만 다름.
    - **up** 명령어
        - docker run 커맨드와 비슷
        - 정의 파일에 기재된 내용대로 이미지를 내려받고 컨테이너를 생성 및 실행.
        - 정의 파일에는 네트워크나 볼륨에 대한 정의도 기재할 수 있어서 주변 환경을 한꺼번에 생성 가능함.
    - **down** 명령어
        - 컨테이너와 네트워크를 정지 및 삭제.
        - 볼륨과 이미지는 삭제하지 않음
        - 컨테이너와 네트워크 삭제 없이 종료만 하고 싶다면 stop 커맨드를 사용.
- Docker compose vs Dockerfile script vs K8s
    - Docker compose는 docker run 명령어를 여러 개 모아놓은 것과 같으며, 컨테이너와 주변 환경(네트워크, 볼륨 등) 을 한번에 생성할 수 있다.
    - Dockerfile script는 컨테이너가 아닌 이미지를 만들기 위한 도구로, 네트워크나 볼륨같은 컨테이너 주변 환경들은 생성이 불가능하다.
    - k8s는 도커 컨테이너를 관리하는 도구로 도커 컴포즈와 혼동하기 쉬우나 사용 목적 자체가 다르다. 도커 컴포즈로 컨테이너 관리는 불가능하다.



## 도커 컴포즈의 설치와 사용법

- 도커 컴포즈는 도커 엔진과 별개의 소프트웨어이나 도커 컴포즈로 생성한 컨테이너를 도커 엔진으로 똑같이 관리할 수 있음.
- Window, MacOS의 도커 데스크톱은 도커 컴포즈가 함께 설치되지만, 리눅스에서는 도커 컴포즈와 python3 런타임 및 필요 도구(python3, python3-pip package)를 설치해야 함

```
# python 관련 package install (debian 계열 Linux 기준 명령어)
$ sudo apt install -y python3 python3-pip
$ sudo pip3 install docker-compose
```

- 도커 컴포즈 사용법
    1. 호스트 컴퓨터에 폴더를 만들고 이 폴더에 YAML 파일을 배치함
    2. 정의 파일의 이름은 미리 정해진 docker-compose.yml 이라는 이름을 사용해야 함 (다른 이름을 사용할 때는 인자로 이름을 지정해줘야 함)
    3. 파일은 호스트 컴퓨터에 배치되지만 명령어는 똑같이 도커 엔진에 전달됨. 즉, 도커 컴포즈가 명령어를 대신 입력해주는 구조.
    4. 만들어진 컨테이너도 동일하게 도커 엔진 위에서 동작함
    5. 정의 파일은 한 폴더에 하나만 있을 수 있음
        - 여러 개의 정의 파일을 사용하려면 그 개수만큼 폴더를 만들어야 함
        - 이미지 파일이나 HTML 파일도 컴포즈가 사용할 폴더에 함께 둠
- 도커 컴포즈에서는 컨테이너가 모인 것을 서비스라고 부르며, 보통 공식 문서에서는 컨테이너와 서비스 두 용어가 혼용되고 있음.

## 📍 도커 컴포즈 파일을 작성하는 법

- 도커 컴포즈는 정의 파일(compose file)을 그대로 실행하는 역할을 수행하므로 정의 파일이 반드시 필요하다.

> ex1) apa000ex2 컨테이너를 실행하는 명령어와 아파치 컨테이너의 컴포즈 파일

```
$ docker run —name apa000ex2 -d -p 8080:80 httpd
```

```
 version: "3"

 services:
    apa000ex2:
      image: httpd
      ports:
        -8080:80
      restart: always
```

> ex2) apa000ex2 컨테이너를 실행하는 명령어와 아파치 컨테이너의 컴포즈 파일

```
$ docker run --name wordpress000ex12 -dit --net=wordpress000net1 -p 8085:80 -e WORDPRESS_DB_HOST=mysql000ex11 -e WORDPRESS_DB_NAME=wordpress000db -e WORDPRESS_DB_USER=wordpress000kun -e WORDPRESS_DB_PASSWORD=wkunpass wordpress
```

```
 version: "3"

 services:
   wordpress000ex12:
     depends_on:
       - mysql000ex11
     image: wordpress
     networks:
       - wordpress000net1
     ports:
       - 8080:80
     restart: always
     environment:
       WORDPRESS_DB_HOST=mysql000ex11
       WORDPRESS_DB_NAME=wordpress000db
       WORDPRESS_DB_USER=wordpress000kun
       WORDPRESS_DB_PASSWORD=wkunpas
```

- 정의 파일은 YAML 형식을 따름 (파일 확장자는 .yml)
- 파일 이름은 기본적으로 docker-compose.yml로 지정 (다른 이름을 사용할 경우 -f 옵션을 사용해야 지정 가능)

### 컴포즈 파일을 작성하는 방법

- YAML 형식에서는 공백에 따라 의미가 달라지므로 탭은 의미가 없다.
- **‘주 항목 → 이름 추가 → 설정’** 순서로 작성
    - 주 항목
        - services, networks, volumes 등등
    - 주 항목 아래에 이름 추가 (공백으로 들여쓰기 필요)
        - 컨테이너 이름, 네트워크 이름, 볼륨 이름
        - 이름 뒤에는 반드시 **콜론(:)**을 붙여야 함
        - 해당 줄에 이어서 컨테이너 설정을 기재하려면 콜론 뒤로 공백이 하나 있어야 함
    - 설정
        - 기재할 내용이 한 가지라면 콜론 뒤에 이어서 작성 **(사이에 공백 필수)**
        - 내용이 여러 개라면 줄을 바꿔 하이픈(-)을 앞에 적고 들여쓰기를 맞춰야 함
        - 하이픈을 앞에 적은 행은 다시 공백을 넣어 들여 써줘야 함

### 컴포즈 파일 (YAML 형식)의 작성 요령

- 첫 줄에 도커 컴포즈 버전을 기재
- 주 항목 services, networks, volumes 아래에 설정 내용을 기재
- 항목 간의 상하 관계는 공백을 사용한 들여쓰기로 나타낸다.
- 들여쓰기는 같은 수의 배수만큼의 공백을 사용한다.
- 이름은 주 항목 아래에 들여쓰기한 다음 기재한다.
- 컨테이너 설정 내용은 이름 아래에 들여쓰기한 다음 기재한다.
- 여러 항목을 기재하려면 줄 앞에 ‘-’를 붙인다.
- 이름 뒤에는 콜론(:)을 붙인다.
- 콜론 뒤에는 반드시 공백이 와야 한다(바로 줄바꿈하는 경우는 예외)
- # 뒤의 내용은 주석으로 간주된다.
- 문자열은 작은따옴표(’) 또는 큰따옴표(”)로 감싸 작성한다.

### 컴포즈 파일의 항목


![[Docker-Compose명령어1.png]]

- depends_on은 다른 서비스에 대한 의존관계를 나타냄
    - 컨테이너를 생성하는 순서나 연동 여부를 정의
    - ex. penguin 컨테이너의 정의에 ‘depends_on: -namgeuk” 내용이 포함됐다면 namgeuk 컨테이너를 생성한 다음에 penguin 컨테이너를 만듦 : 워드프레스처럼 MySQL 컨테이너가 먼저 있어야 하는 경우에 컨테이너 생성 순서를 지정
- restart는 컨테이너 종료 시 재시작 여부를 설정함


![[Docker-Compose명령어2.png]]

### 실습 예제

1. docker-compose.yml 파일 생성 (com_folder 안에)  
2. 주 항목 작성  
3. 이름 작성  
4. MySQL 컨테이너의 정의 작성  
5. 워드 프레스 컨테이너의 정의 작성  
6. 파일 저장

![[Docker-Compose명령어3.png]]

## 📍 도커 컴포즈 실행

- 도커 컴포즈는 docker-compose 명령을 사용
- -f 옵션을 통해 compose file 경로 지정
- **docker-compose up** : 컴포즈 파일에 정의된 컨테이너 및 네트워크와 같은 주변 환경 생성
    - $ docker-compose -f [정의_파일_경로] up [옵션]


![[Docker-Compose명령어4.png]]

docker-compose down : 생성 된 컨테이너와 네트워크를 종료하고 삭제

- $ docker-compose -f [컴포즈_파일_경로] down [옵션]

![[Docker-Compose명령어5.png]]

- docker-compose stop : 컨테이너를 종료
    - $ docker-compose -f [컴포즈_파일_경로] stop [옵션]
- docker-compose [명령어] -d
    - -d 옵션을 붙이고 경로 정의 없이 도커 컴포즈 명령어를 실행하면 현재 작업 디렉터리를 컴포즈용 폴더로 사용할 수 있다.
- --scale 옵션
    - 같은 구성의 컨테이너를 여러 세트 만들고 싶은 경우 사용할 수 있는 옵션.


```yml
$ docker-compose up -d 
  Creating network "com_folder_wordpress000net1" with the default driver
  Creating volume "com_folder_mysql000vol11" with default driver
  Creating volume "com_folder_wordpress000vol12" with default driver
  Pulling mysql000ex11 (mysql:5.7)...
  5.7: Pulling from library/mysql
  66fb34780033: Pull complete
  4b7eaab7220f: Pull complete
  ...
  f8575f2324da: Pull complete
  b4c26cf54614: Pull complete
  Digest: sha256:4279d155f8cab19149c6078b20d53976f1498e31d6f848ac83e11323909b41f1
  Status: Downloaded newer image for mysql:5.7
  Pulling wordpress000ex12 (wordpress:)...
  latest: Pulling from library/wordpress
  b85a868b505f: Already exists
  78fdfd2598e0: Pull complete
  26769c8659f4: Pull complete
  0bd105fadbe3: Pull complete
  cec5cceb91d7: Pull complete
  ca31293bb368: Pull complete
  ...
  b6a172e68ef0: Pull complete
  d251d6673035: Pull complete
  62b30aea4447: Pull complete
  9b5a3cabe1fe: Pull complete
  df30812aac94: Pull complete
  Digest: sha256:1134d9db6eccc6fdca73176e5ffed7b5516638a9ed36169d21e2692495e8fe2f
  Status: Downloaded newer image for wordpress:latest
  Creating com_folder_mysql000ex11_1 ... done
  Creating com_folder_wordpress000ex12_1 ... done

$ docker ps
  CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS         PORTS                  NAMES
  477023ba29c6   wordpress   "docker-entrypoint.s…"   4 seconds ago    Up 3 seconds   0.0.0.0:8085->80/tcp   com_folder_wordpress000ex12_1
  3aaa8e52d2eb   mysql:5.7   "docker-entrypoint.s…"   11 seconds ago   Up 3 seconds   3306/tcp, 33060/tcp    com_folder_mysql000ex11_1

$ docker network ls
  NETWORK ID     NAME                          DRIVER    SCOPE
  19e014ef7475   bridge                        bridge    local
  c6c78f97b7fb   com_folder_wordpress000net1   bridge    local
  94cf1f2451f9   host                          host      local

$ docker volume ls
  DRIVER    VOLUME NAME
  local     com_folder_mysql000vol11
  local     com_folder_wordpress000vol12
  
$ docker-compose stop   
  [+] Running 2/2
   ⠿ Container com_folder_mysql000ex11_1      Stopped                                                                                                          1.8s
   ⠿ Container com_folder_wordpress000ex12_1  Stopped
   
$ docker ps -a
  CONTAINER ID   IMAGE            COMMAND                  CREATED         STATUS                       PORTS                  NAMES
  477023ba29c6   wordpress        "docker-entrypoint.s…"   6 minutes ago   Exited (0) 20 seconds ago                           com_folder_wordpress000ex12_1
  3aaa8e52d2eb   mysql:5.7        "docker-entrypoint.s…"   6 minutes ago   Exited (0) 19 seconds ago                           com_folder_mysql000ex11_1

$ docker-compose down
  Removing com_folder_wordpress000ex12_1 ... done
  Removing com_folder_mysql000ex11_1     ... done
  Removing network com_folder_wordpress000net1 ## 네트워크만 삭제됨. 볼륨과 이미지는 그대로.
  
$ docker ps -a
CONTAINER ID   IMAGE            COMMAND                  CREATED      STATUS                        PORTS                  NAMES
```



## 내가 만든 .yml

docker-compse.yml이 있는 디렉토리로 이동
이거 같은 경우에는 docker-compose.yml 파일이 저 위치에 있기 때문에 저기로 이동 
다른곳에 있으면 다른 곳으로 이동해야함
![[Docker-Compose명령어11.png]]
![[Docker-Compose명령어6.png]]



docker-compose up 실행(이미 만들어 놓은 yml파일이 있을경우)
![[Docker-Compose명령어7.png]]

로그와 함께 실행
이때 나오고 싶을 경우 ctrl + c 입력
![[Docker-Compose명령어8.png]]

도커 지우고 싶을 때
docker-compose down

![[Docker-Compose명령어9.png]]

docker-compose up -d  도커 컴포즈 백그라운드 실행

![[Docker-Compose명령어10.png]]


결과
도커 컴포즈 안에 3개의 컨테이너 생성
![[Docker-Compose명령어12.png]]

![[Docker-Compose명령어13.png]]




```yml
version: '3'
services:
  tomcat:
    image: tomcat:9.0.84
    container_name: myTomcat //컨테이너 명 지정
    ports:
      - 8080:8080
    volumes:
      - ./CrawlerTest-1.0-SNAPSHOT.war:/usr/local/tomcat/webapps/ROOT.war
    environment:
	  - JAVA_OPTS=-Djava.version=8 //내가 원하는 자바 버전 설정하고 싶을때
	  - CATALINA_CONNECTOR_CONNECTIONTIMEOUT=300000 // 프록시 설정 응답 시간 늘림
  mysql: //여기에 있는 이름으로 database-context.xml에서 url 이름 수정
  //<property name="jdbcUrl" value="jdbc:mysql://mysql:3306/test"></property> 여기서 localhost대신
    image: mysql:latest
    container_name: mySqls // 컨테이너 명 바뀌면
    //<property name="jdbcUrl" value="jdbc:mysql://mySqls:3306/test"></property> 여기 있는 mysql:3306에서 내가 지정한 컨테이너명으로 변경
    ports:
      - 3306:3306
    environment:
      - MYSQL_ROOT_PASSWORD=1234
      - MYSQL_DATABASE=test
      - MYSQL_USER=test
      - MYSQL_PASSWORD=1234
      - MYSQL_SSL_MODE=DISABLED
      - MYSQL_ALLOW_CLEARTEXT_PLUGIN=1
      - MYSQL_SSL=false

```



war 파일 생성
인텔리제이 터미널에서 
mvn clear package


database-context.xml 수정
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">  
  
  
    <!-- mybatis 사용하기 위해 객체 생성 -->  
  
    <!-- Connection -->    <bean class="com.zaxxer.hikari.HikariDataSource" id="dataSource">  
        <property name="username" value="test"></property>  
        <property name="password" value="1234"></property>  
        <property name="jdbcUrl" value="jdbc:mysql://mysql:3306/test"></property>//localhost에서 mysql로
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


컨테이너 이름 지정시
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">  
  
  
    <!-- mybatis 사용하기 위해 객체 생성 -->  
  
    <!-- Connection -->    <bean class="com.zaxxer.hikari.HikariDataSource" id="dataSource">  
        <property name="username" value="root"></property>  
        <property name="password" value="1234"></property>  
        <property name="jdbcUrl" value="jdbc:mysql://mySqls/test"></property>  
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




docker 실행 명령어
```
docker-compose up //도커 컴포즈 실행
docker-compose up -d //백그라운드 실행
docker-compose down // 도커 컴포즈 remove
docker-compose logs 컨테이너 이름 //특정 컨테이너 로그보기
docker-compose logs myTomcat
```


war 파일 내보내기
인텔리제이 터미널에서
```
mvn clean package
```


dockerfile 만들었을 시
```file
FROM tomcat:8.5-jdk8-openjdk

# WAR 파일을 ROOT.war로 이름 변경하여 Tomcat의 webapps 디렉토리에 배포
ADD target/CrawlingPractice-1.0-SNAPSHOT.war /usr/local/tomcat/webapps/ROOT.war 
```

dbeaver 연결 실패시
![[Docker-Compose설정1.png]]

연결 가서 Driver properties 들어가서 useSSL False로 변경  --원래 true 로 설정되어있음



![[Docker-Compose설정2.png]]

마찬가지로 allowPublicKeyRetrieval true로 변경 -- false로 설정되어있음




## Oracle

```yml
version: '3'
services:
  tomcat:
    image: tomcat:9.0.84
    container_name: myTomcat
    ports:
      - 8080:8080
    volumes:
      - ./DbConn-1.0-SNAPSHOT.war:/usr/local/tomcat/webapps/ROOT.war # ./DbConn-1.0-SNAPSHOT.war은 docker-compose.yml과 같은 위치에 있을경우 ./DbConn-1.0-SNAPSHOT.war으로 사용 가능
      - C:/Project/andDb/.smarttomcat/andDb/conf/server.xml:/usr/local/tomcat/conf/server.xml # 실제 위치가 다를경우 직접 위치를 지정해줘야함 
    environment:
      - CATALINA_CONNECTOR_CONNECTIONTIMEOUT=600000

  oracle:
    image: oracleinanutshell/oracle-xe-11g
    container_name: myOracle
    ports:
      - 1521:1521
    environment:
      - ORACLE_ALLOW_REMOTE=true
      - ORACLE_ROOT_PASSWORD=1234
      - ORACLE_DATABASE=testdb
      - ORACLE_USER=test
      - ORACLE_PASSWORD=test
      - ORACLE_SYSTEM_PASSWORD=1234

```


```yml
- ORACLE_ROOT_PASSWORD=1234
      - ORACLE_DATABASE=testdb
      - ORACLE_USER=test
      - ORACLE_PASSWORD=test
      - ORACLE_SYSTEM_PASSWORD=1234
```
얘네는 있어도 되고 없어도됨

cmd에서
```cmd
docker exec -it myOracle bash
su - oracle
sqlplus / as sysdba
ALTER USER sys IDENTIFIED BY newpassword;
```

차례로 입력 
```
ALTER USER sys IDENTIFIED BY 1234;
```
여기서  newpassword대신 내가 원하는 password 입력


![[Docker-Compose설정3.png]]

여기서 sys 입력후 role에 normal대신 다른것 입력


이전 캐쉬 삭제

```bash
docker-compose down -v
docker builder prune --force
```


---
참조 -  https://devzzi.tistory.com/76



https://hermeslog.tistory.com/498


https://jow1025.tistory.com/313

https://seosh817.tistory.com/387
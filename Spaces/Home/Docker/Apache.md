



`./apache/htdocs` 디렉토리는 Apache HTTP Server 컨테이너의 정적 웹 콘텐츠를 제공하기 위한 디렉토리입니다. 따라서, 해당 디렉토리에는 웹 서버를 통해 접근 가능한 파일들을 위치시켜야 합니다.

예를 들어, `index.html`이라는 파일을 `./apache/htdocs` 디렉토리에 추가하고 싶다면, 호스트의 `./apache/htdocs` 디렉토리에 `index.html` 파일을 생성하면 됩니다. Apache HTTP Server 컨테이너가 실행되면 해당 파일이 웹 서버를 통해 접근 가능해집니다.

만약 `./apache/htdocs` 디렉토리에 여러 파일을 추가하고 싶다면, 해당 디렉토리에 원하는 파일들을 생성하거나 복사하면 됩니다. 이렇게 함으로써 Apache HTTP Server 컨테이너는 해당 파일들을 제공할 수 있게 됩니다.

예를 들어, `index.html`, `styles.css`, `script.js`라는 파일을 `./apache/htdocs` 디렉토리에 추가하고 싶다면, 호스트의 `./apache/htdocs` 디렉토리에 해당 파일들을 생성하거나 복사하면 됩니다. 이후 Apache HTTP Server 컨테이너가 실행되면 이러한 파일들이 웹 서버를 통해 접근 가능해집니다.

따라서, `./apache/htdocs` 디렉토리에는 웹 서버를 통해 접근 가능한 파일들을 위치시키면 됩니다.

```
version: '3'
services:
  tomcat:
    image: tomcat:9.0.84
    container_name: myTomcat
    ports:
      - 8080:8080
    volumes:
      - ./CrawlerTest-1.0-SNAPSHOT.war:/usr/local/tomcat/webapps/ROOT.war
    # 톰캣 설정 등 추가 설정

  mysql:
    image: mysql:latest
    container_name: mySqls
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
    # MySQL 설정 등 추가 설정

  apache:
    image: httpd:latest
    container_name: myApache
    ports:
      - 80:80
    volumes:
      - ./apache/httpd.conf:/usr/local/apache2/conf/httpd.conf
      - ./apache/htdocs:/usr/local/apache2/htdocs
      - ./apache/workers.properties:/usr/local/apache2/conf/workers.properties
    depends_on:
      - tomcat

```










httpd.conf 안에 추가를 안해도 되는데 혹시 몰라서 저장
```
# mod_jk 추가
LoadModule jk_module modules/mod_jk.so
JkWorkersFile conf/workers.properties
JkLogFile logs/mod_jk.log
JkLogLevel info
JkLogStampFormat "[%a %b %d %H:%M:%S %Y]"
JkOptions +ForwardKeySize +ForwardURICompat -ForwardDirectories
```


workers.properties 두명일때 예시

```
worker.list=worker1, worker2

worker.worker1.type=ajp13
worker.worker1.host=tomcat1
worker.worker1.port=8009

worker.worker2.type=ajp13
worker.worker2.host=tomcat2
worker.worker2.port=8009

```






---
참조 - https://know-one-by-one.tistory.com/108 멀티 톰캣 아파치 설
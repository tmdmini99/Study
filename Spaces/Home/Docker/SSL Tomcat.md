

docker container만 실행

```
docker run -d \
    -v /path/to/keystore.jks:/usr/local/tomcat/conf/keystore.jks \
    -p 8080:8080 \
    -p 8443:8443 \
    your-image-name
```



server.xml 경로 수정시


```xml
<Connector
    port="8443"
    protocol="org.apache.coyote.http11.Http11NioProtocol"
    maxThreads="150"
    SSLEnabled="true">
    <SSLHostConfig>
        <Certificate
            certificateKeystoreFile="/app/keystore.jks"
            type="RSA"
            certificateKeystorePassword="yourKeystorePassword"/>
    </SSLHostConfig>
</Connector>

```


## Docker-compose

### Dockerfile

```
FROM tomcat:9.0

# server.xml 파일을 톰캣의 conf 디렉터리에 복사
COPY server.xml /usr/local/tomcat/conf/server.xml

# (선택사항) 다른 설정이나 애플리케이션 파일을 복사
# COPY your-app.war /usr/local/tomcat/webapps/

```


server.xml
```
<Connector
    port="8443"
    protocol="org.apache.coyote.http11.Http11NioProtocol"
    maxThreads="150"
    SSLEnabled="true">
    <SSLHostConfig>
        <Certificate
            certificateKeystoreFile="/app/keystore.jks"  <!-- 여기는 도커 내부의 경로 -->
            type="RSA"
            certificateKeystorePassword="yourKeystorePassword"/>
    </SSLHostConfig>
</Connector>
```


docker-compose.yml

```yml
version: '3'
services:
  web:
    build: .
    ports:
      - "8080:8080"
      - "8443:8443"
    volumes:
      - ./keystore.jks:/app/keystore.jks  # 호스트의 keystore.jks를 컨테이너 내부에 마운트
    environment:
      - JAVA_OPTS=-Djavax.net.ssl.trustStore=/app/keystore.jks

```




### docker-comsose.yml만 사용

```yml
version: '3.8'

services:
  tomcat:
    image: tomcat:9.0
    ports:
      - "8080:8080"    # HTTP 포트
      - "8443:8443"    # HTTPS 포트
    volumes:
      - C:/Program Files/Java/jdk-11/bin/key.jks:/usr/local/tomcat/conf/keystore.jks
      - C:/Project/andDb/.smarttomcat/andDb/conf/server.xml:/usr/local/tomcat/conf/server.xml
    environment:
      - JAVA_OPTS=-Djava.security.egd=file:/dev/./urandom


```


server.xml
```xml
<Connector
    port="8443"
    protocol="org.apache.coyote.http11.Http11NioProtocol"
    maxThreads="150"
    SSLEnabled="true">
    <SSLHostConfig>
        <Certificate
            certificateKeystoreFile="/usr/local/tomcat/conf/keystore.jks"
            type="RSA"
            certificateKeystorePassword="yourKeystorePassword"/>
    </SSLHostConfig>
</Connector>

```
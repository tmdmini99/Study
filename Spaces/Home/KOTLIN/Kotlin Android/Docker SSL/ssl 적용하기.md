코틀린 코드에서 도커로 올렸을때 ssl을 적용하는 방법


server.xml 예시
```xml
<Connector
    port="8443"
    protocol="org.apache.coyote.http11.Http11NioProtocol"
    maxThreads="150"
    scheme="https"
    secure="true"
    SSLEnabled="true"
    keystoreFile="${catalina.base}/conf/keystore.jks"
    keystorePass="yourKeystorePassword"
    clientAuth="false"
    sslProtocol="TLS"
/>

```

compose.xml 예시
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


## 실제 사용 docker run 명령어  && server.xml 추가

docker run
```
docker run --network=mynetwork --name tomcat-test -d \
  -p 8080:8080 \
  -p 8443:8443 \
  -v "C:/Program Files/Java/jdk-11/bin/key.jks:/usr/local/tomcat/conf/keystore.jks" \
  -v "C:/Project/andDb/.smarttomcat/andDb/conf/server.xml:/usr/local/tomcat/conf/server.xml" \
  -e JAVA_OPTS="-Djava.security.egd=file:/dev/./urandom -Duser.timezone=Asia/Seoul" \
  tomcat:9.0.84

```

한줄 
```
docker run --network=mynetwork --name tomcat-test -d -p 8080:8080 -p 8443:8443 -v "C:/Program Files/Java/jdk-11/bin/key.jks:/usr/local/tomcat/conf/keystore.jks" -v "C:/Project/andDb/.smarttomcat/andDb/conf/server.xml:/usr/local/tomcat/conf/server.xml" -e JAVA_OPTS="-Djava.security.egd=file:/dev/./urandom -Duser.timezone=Asia/Seoul" tomcat:9.0.84

```



```
docker cp C:\Project\andDb\target\andDb-1.0-SNAPSHOT.war tomcat-test:/usr/local/tomcat/webapps/ROOT.war

```



server.xml
```xml
<Connector
    port="8443"
    protocol="org.apache.coyote.http11.Http11NioProtocol"
    maxThreads="150"
    scheme="https"
    secure="true"
    SSLEnabled="true"
    keystoreFile="${catalina.base}/conf/keystore.jks"
    keystorePass="yourKeystorePassword"
    clientAuth="false"
    sslProtocol="TLS"
/>

```


## 안드로이드에 적용

cmd에서 SHA-256 지문을 확인

```
openssl x509 -in "C:\Users\tmdal\Downloads\openssl-1.0.2j-fips-x86_64\OpenSSL\bin\private.crt" -noout -fingerprint -sha256

```

만약 없을경우 key.jks에서 인증서 추출후 확인
```
// 인증서 추출
keytool -exportcert -alias tomcat -keystore key.jks -file mycert.crt

//SHA-256 핀 추출
openssl x509 -in mycert.crt -noout -fingerprint -sha256


//결과
SHA256 Fingerprint=12:34:56:78:90:AB:CD:EF:GH:IJ:KL:MN:OP:QR:ST:UV:WX:YZ:12:34:56:78:90:AB:CD


```

network_security_config.xml에 추가

```xml
<network-security-config>
    <domain-config cleartextTrafficPermitted="false">
        <domain includeSubdomains="true">example.com</domain>
        <pin-set>
            <pin digest="SHA-256">1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef</pin>
        </pin-set>
    </domain-config>
</network-security-config>

```



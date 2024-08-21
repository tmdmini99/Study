코틀린 코드에서 도커로 올렸을때 ssl을 적용하는 방법


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

compose.xml
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
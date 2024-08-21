

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


파일 복사

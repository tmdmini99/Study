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


p12 생성
```
openssl pkcs12 -export -in private.crt -inkey private.key -out key.p12 -name tomcat
```

p12 적용
```
docker run --network=mynetwork --name tomcat-test -d \
    -p 8080:8080 -p 8443:8443 \
    -v "C:/Users/tmdal/Downloads/openssl-1.0.2j-fips-x86_64/OpenSSL/bin/key.p12:/usr/local/tomcat/conf/key.p12" \
    -v "C:/Project/andDb/.smarttomcat/andDb/conf/server.xml:/usr/local/tomcat/conf/server.xml" \
    -e JAVA_OPTS="-Djava.security.egd=file:/dev/./urandom -Duser.timezone=Asia/Seoul" \
    tomcat:9.0.84

```

한줄
```
docker run --network=mynetwork --name tomcat-test -d -p 8080:8080 -p 8443:8443 -v "C:/Users/tmdal/Downloads/openssl-1.0.2j-fips-x86_64/OpenSSL/bin/key.p12:/usr/local/tomcat/conf/key.p12" -v "C:/Project/andDb/.smarttomcat/andDb/conf/server.xml:/usr/local/tomcat/conf/server.xml" -e JAVA_OPTS="-Djava.security.egd=file:/dev/./urandom -Duser.timezone=Asia/Seoul" tomcat:9.0.84

```




```
docker cp C:\Project\andDb\target\andDb-1.0-SNAPSHOT.war tomcat-test:/usr/local/tomcat/webapps/ROOT.war

```



dockerfile로 이미지 생성후 만들기

dockerfile
```
# Tomcat 9.0.84 이미지를 기반으로
FROM tomcat:9.0.84

# 필수 패키지 설치
RUN apt-get update && apt-get install -y \
    openssl \
    libssl-dev

# OpenSSL 버전 확인
RUN openssl version

# Tomcat 서버 포트 설정
EXPOSE 8080 8443

# Tomcat 서버 시작
CMD ["catalina.sh", "run"]

```

빌드 명령어
```
docker build -t my-tomcat-image .

```

컨테이너 실행 명령어
```
docker run --network=mynetwork --name tomcat-test -d -p 8080:8080 -p 8443:8443 -v "C:/Users/tmdal/Downloads/openssl-1.0.2j-fips-x86_64/OpenSSL/bin/key.p12:/usr/local/tomcat/conf/key.p12" -v "C:/Project/andDb/.smarttomcat/andDb/conf/server.xml:/usr/local/tomcat/conf/server.xml" -e JAVA_OPTS="-Djava.security.egd=file:/dev/./urandom -Duser.timezone=Asia/Seoul" my-tomcat-image
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
    keystoreFile="/usr/local/tomcat/conf/key.p12"  
    keystorePass= "123456"  
    clientAuth="false"  
    sslProtocol="TLS"  
    keystoreType="PKCS12"  
/>

```
keystorePass = '123456' 에서 123456 부분에는 내가 넣은 비밀번호 입력


```xml
<Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
           maxThreads="150" SSLEnabled="true">
    <SSLHostConfig>
        <Certificate certificateKeystoreFile="/usr/local/tomcat/conf/key.p12"
                     certificateKeystorePassword="1234"
                     type="RSA"
                     certificateKeystoreType="PKCS12"/>
        <Protocol>
            <Property name="sslEnabledProtocols" value="TLSv1.2,TLSv1.3"/>
        </Protocol>
    </SSLHostConfig>
</Connector>

```

**첫 번째 방법**은 간단한 설정에 적합하며, **두 번째 방법**은 세부적인 SSL/TLS 구성과 특정 요구사항에 맞는 설정을 필요로 하는 경우에 적합합니다





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

PowerShell 에서 base 64로 변경

```
# SHA-256 핀 값 (16진수 문자열로 입력)
$fingerprintHex = "13CF308E4539397914C7961F7ED908CE07136378BD1A878FF31B203E27675BC8"

# 16진수 문자열을 바이트 배열로 변환
$bytes = [System.Collections.Generic.List[byte]]@()
for ($i = 0; $i -lt $fingerprintHex.Length; $i += 2) {
    $bytes.Add([Convert]::ToByte($fingerprintHex.Substring($i, 2), 16))
}

# 바이트 배열을 Base64 문자열로 변환
$base64String = [Convert]::ToBase64String($bytes.ToArray())

# 결과 출력
Write-Output $base64String

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


그냥 추출하는 법


```
"C:\Users\tmdal\Downloads\openssl-1.0.2j-fips-x86_64\OpenSSL\bin\openssl.exe" x509 -in "C:\Users\tmdal\Downloads\openssl-1.0.2j-fips-x86_64\OpenSSL\bin\private.crt" -pubkey -noout > "C:\Users\tmdal\Downloads\openssl-1.0.2j-fips-x86_64\OpenSSL\bin\publickey.pem"
```
그냥 위치 지정후 public.key를 만듬

그후 

```
openssl rsa -pubin -in publickey.pem -outform DER | openssl dgst -sha256 -binary | openssl base64  
```
치면 변경 완료
## 자체 서명 인증서 신뢰



network_security_config.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <domain-config cleartextTrafficPermitted="false">
        <!-- 인증서 고정 설정 -->
        <domain includeSubdomains="true">example.com</domain>
        <pin-set>
            <!-- 공개 키 해시를 입력합니다. -->
            <pin digest="SHA-256">E88wjkU5OXkUx5YfftkIzgcTY3i9GoeP8xsgPidnW8g=</pin>
        </pin-set>
        <!-- 신뢰할 인증서 설정 -->
        <trust-anchors>
            <!-- 인증서 파일이 raw 디렉토리에 있어야 합니다. -->
            <certificates src="@raw/my_ca"/>
        </trust-anchors>
    </domain-config>
</network-security-config>

```


---

출처 - https://developer.android.com/privacy-and-security/security-config?hl=ko#CertificatePinning 인증서 관련

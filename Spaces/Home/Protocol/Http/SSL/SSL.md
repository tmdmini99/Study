## Secure Sockets Layer


![[SSL2.png]]

우선, HTTPS와 SSL은 다릅니다. SSL과 TLS는 '보안 계층'이라는 독립적인 프로토콜 계층을 만들어, 위 그림과 같이 응용 계층과 전송 계층 사이에 속하게 됩니다. HTTPS는 SSL 또는 TLS 위에 HTTP 프로토콜을 얹어 보안된 HTTP 통신을 하는 프로토콜입니다. 즉, SSL과 TLS는 HTTP뿐만이 아니라 FTP, SMTP와 같이 다른 프로토콜에도 적용할 수 있으며, HTTPS는 TLS와 HTTP가 조합된 프로토콜만을 가리키는 겁니다. SSL과 TLS는 같은 의미의 단어입니다. TLS가 SSL의 후속 버전이지만, SSL이 일반적으로 더 많이 사용되는 용어입니다.




> **“이제 SSL 암호화 통신에 대해 어느 정도 이해가 갑니다. 그럼 어떤 식으로 보안이 적용되는 건가요?”**

SSL 암호화 통신은 여러 가지 조건과 과정을 통해 설립됩니다. 앞에서 언급한 이야기는 아주 간단한 예시입니다. 먼저, 통신 과정들을 알아보겠습니다. SSL 암호화 통신은 총 세 단계로 나눌 수 있습니다.

![[SSL1.png]]

첫 번째로, SSL 핸드셰이크를 수행합니다. 이때 데이터를 주고받기 위해 어떤 방법을 사용해야 하는지 서로의 상태를 파악합니다. SSL은 80번 포트를 사용하는 http와 달리 443번 포트를 기본으로 사용하는 TCP 기반의 프로토콜입니다. TCP 기반이기 때문에 SSL 핸드셰이크 전에 TCP 3-way 핸드셰이크 또한 수행합니다. 서로 간 협상이 완료되면, SSL 세션이 생성되고 클라이언트와 서버는 원하는 데이터를 주고받게 됩니다. 그리고 데이터 전송의 끝을 서로에게 알리며 세션을 종료합니다.

**핸드셰이크 단계에선 서로 어떤 것들을 협상하게 되나요?**


![[SSL3.png]]


서로 간의 메시지와 데이터 통신은 사람과 사람이 대화하는 것과 유사합니다. TCP 통신의 바탕인 '신뢰성'을 생각하면 더욱 쉽게 이해할 수 있을 겁니다.

​
- Client hello : 클라이언트가 서버에게 연락합니다. 브라우저 검색창에 도메인을 입력하는 것으로 보면 됩니다. 이때 클라이언트는 자신의 브라우저가 지원할 수 있는 암호화 방식(Cipher Suite)을 먼저 제시합니다. 그리고 랜덤 데이터를 생성하여 추가로 전송합니다.

- Server hello : 서버가 클라이언트에게 연락합니다. 서버는 클라이언트가 제시한 암호화 방식 중 하나를 선정하여 알려줍니다. 또한, 서버 자신의 인증서를 전달합니다. 이 인증서에는 서버의 공개 키가 포함되어 있습니다. 클라이언트와 마찬가지로 서버 측에서 생성한 랜덤 데이터 또한 전달합니다.

- Client key exchange : 클라이언트는 미리 주고받은 자신과 서버의 랜덤 데이터를 참고하여 서버와 암호화 통신을 할 때 사용할 키를 생성한 후 서버에게 전달합니다. 이때 키는 서버로부터 받은 공개키로 암호화되어 보내집니다.

- Finished : 마지막으로 핸드셰이크 과정이 정상적으로 마무리되면, 클라이언트와 서버 모두 “finished” 메시지를 보냅니다. 그 후부턴 클라이언트가 생성한 키를 이용하여 암호화된 데이터를 주고받게 됩니다.



**3. SSL 인증서**

SSL 인증서는 클라이언트와 서버간의 통신을 제3자가 보증해주는 전자화된 문서입니다. 클라이언트가 서버에 접속한 직후에 서버는 클라이언트에게 이 인증서 정보를 전달하게 됩니다. 클라이언트는 이 인증서 정보가 신뢰할 수 있는 것인지를 검증 한 후에 다음 절차를 수행하게 됩니다.



![[SSL4.png]]




![[SSL20.png]]





**SSL과 SSL 디지털 인증서를 이용했을 때의 이점은 아래와 같습니다.**

 - 통신 내용이 공격자에게 노출되는 것을 막을 수 있습니다.

 - 클라이언트가 접속하려는 서버가 신뢰할 수 있는 서버인지를 판단할 수 있습니다.

 - 통신 내용의 악의적인 변경을 방지할 수 있습니다.

  

**SSL 인증서의 역할**

SSL 인증서의 역할은 다소 복잡하기 때문에 인증서의 매커니즘을 이해하기 위한 몇가지 지식들을 알고 있어야 합니다. 인증서의 기능은 크게 두가지가 있습니다. 아래 두가지를 이해하는 것이 인증서를 이해하는 핵심입니다.

  

> 1. 클라이언트가 접속한 서버가 신뢰할 수 있는 서버임을 보장합니다.
> 
> 2. SSL 통신에 사용할 공개키를 클라이언트에게 제공합니다.

  

**CA**

**인증서의 역할은 클라이언트가 접속한 서버가 클라이언트가 의도한 서버가 맞는지를 보장하는 역할**을 합니다. 이 역할을 하는 민간기업들이 있는데 이런 기업들을 CA(Certificate authority) 혹은 Root Certificate 라고 부릅니다. CA는 아무 기업이나 할 수 있는 것이 아니고 신뢰성이 엄격하게 공인된 기업들만 참여할 수 있습니다.




![[SSL5.jpg]]




#### 1. 서버는 공개키와 개인키를 만든다.

#### 2. 믿을만한 저장소(CA)와 계약해 두 키를 관리하도록 한다.

#### 3.  CA도 자신만의 공개키와 개인키를 가지고 자체 암호화를 하여 SSL 인증서를 발급한다.

#### 4. 클라이언트의 데이터 요청이 들어오면 서버는 CA에서 만든 SSL 인증서를 보내준다.

#### 5. 클라이언트는 CA의 공개키를 이용해 SSL 인증서를 복호화(암호 해독)한다.

#### 6. SSL 내부의 서버 공개키를 이용해서 암호화해서 인증서를 서버에게 보낸다.

#### 7. 서버는 개인키로 암호화된 인증서를 복호화(암호 해독)하고 다시 암호화 하여 클라이언트에게 보낸다.



## 실제로 적용시키는 방

윈도우 기준

[https://sourceforge.net/projects/openssl/files/latest/download?source=typ_redirect](https://sourceforge.net/projects/openssl/files/latest/download?source=typ_redirect)

사이트에 들어가서 .zip 파일을 다운받고 C드라이브 아래에 다운해준다. 물론 자기가 원하는 위치에 다운해도 된다! 대신 중간에 생기는 오류는 내가 해결해줄수 없다...ㅎ C드라이브 아래에 압축을 풀어주고 bin 폴더로 들어가자

window의 경우 openssl.exe가 있는데 이를 실행시켜준다!

여기서 가장 중요한 사항은 그냥 실행시키면 안된다! 무조건 관리자 권한으로 실행시켜주자





![[SSL6.png]]


이걸 몰라서 파일이 정상적으로 만들어지지 않아 오류가 생길수 있다!

이제 ssl 인증을 위한 파일들을 순서대로 만들어볼 차례다!

(파일들은 모두 openssl 실행한 폴더 내부에 생성되어집니다.)


### **1. 개인키, 공개키 발급**

실행된 cmd창에서

**genrsa -des3 -out private.pem 2048**

genrsa \[암호화 알고리즘] -out \[파일이름].pem 2048

명령어를 실행해준다.

[] 안에 내용은 사용자가 원하는대로 변경해서 사용해도 된다.

 다음 내용들도 알아서 맞게 변경


![[SSL7.png]]

실행하면 비밀번호를 입력하라고 한다. 자신이 기억할 수 있는 비밀번호를 사용하자 계속해서 물어볼거다.



![[SSL8.png]]



정상적으로 실행된 화면이다. 

하지만 이렇게 개인키만 만들면 이 비밀번호를 절대 까먹지 말아야하고 ssl을 적용시켜주는 곳마다 적용시켜줘야한다. 그게 귀찮으니 비밀번호가 없는 키를 하나 더 만들자


**genrsa -out private.key 2048**

genrsa -out \[키파일 이름].key 2048

명령어를 실행시키고


![[SSL9.png]]

이렇게 나와야 정상이다. 여기서 +++대신에 다른 말이 있다거나 error가 뜬다면 관리자 권한으로 실행시켰는지 꼭 확인하자!

**rsa -in private.key -pubout -out public.key**

rsa -in \[key파일] -pubout -out \[공개키 이름].key

![[SSL10.png]]

### **2. CSR 만들기 (인증요청서)**

CSR이란

SSL 인증의 정보(도메인을 사용하는 개인/기업에 대한 정보)를 암호화하여 인증기관에 보내 인증된 인증서를 발급받게 하는 신청서

req -new -key private.key -out private.csr

req -new -key \[key파일] -out \[csr파일이름].csr

명령어를 입력하면

![[SSL11.png]]



오류가 뜬다... 큰일이지만 사실 큰일이 아니다 내용을 읽어보면 C드라이브/OpenSSL/openssl.cnf의 파일을 읽으려고 하니 파일이 없다는거다! 이 파일은 bin 안에 들어있기때문이다... 기본경로가 이상하다. 우리는 이 파일을 저 경로에 맞게 변경하거나 자신이 C드라이브에 설치하지 않고 다른 드라이브나 경로가 다르다면 아래 명령어로 수정해서 입력하자

**req -new -key private.key -out private.csr -config C:/OpenSSL/bin/openssl.cnf**

req -new -key \[key파일] -out \[csr파일이름].csr -config \[openssl.cnf 파일 경로]




![[SSL12.png]]

여기서는 개인정보를 물어보는건데 어차피 테스트용이니 아무내용

![[SSL13.png]]

고민하지 말고 아무거나
밑에 2가지 내용은 빈칸으로 넣어도 된다
이렇게 모두 종료되면 csr파일이 생성되었다!


### **3. CRT 만들기 (인증서)**

CRT는 따로 그냥 만들수 있다고 하지만 난 방법을 모른다. 그래서 아는 방법인 인증서에 서명까지 해줄 사설 CA를 만들어보겠다.

> rootCA.key 생성 (rootCA의 이름이 아닌 다른 이름을 사용해도 무방하다.)

**genrsa -aes256 -out rootCA.key 2048**

genrsa [암호화 알고리즘] -out [키이름].key 2048




![[SSL14.png]]

key파일을 생성하고 이 키를 이용해서 10년 짜리 rootCA.pem을 만든다.

req -x509 -new -nodes -key rootCA.key -days 3650 -out rootCA.pem

를 입력해주면 우리가 보던 잘 알거같은 오류가 난다. 똑같이 수정해준다.

**req -x509 -new -nodes -key rootCA.key -days 3650 -out rootCA.pem -config C:/OpenSSL/bin/openssl.cnf**


![[SSL15.png]]


이제 CRT를 만들어줄건데 위에서 만들었던 나의 csr을 rootCA의 인증을 받아 crt를 생성하자

**x509 -req -in private.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out private.crt -days 3650**


![[SSL16.png]]

비밀번호를 물어보는데 이것도 우리가 안다. 입력


### **4. tomcat에 적용할 .keystore 파일 생성**

**pkcs12 -export -in private.crt -inkey private.key -out .keystore -name tomcat**


![[SSL17.png]]

tomcat에 적용할 파일을 만드는데 비밀번호를 또 입력하라고 한다




## SSL 환경변수 등록


![[SSL18.png]]




![[SSL19.png]]



JDK 내부에서 keytool 을 제공한다. JDK 디렉토리로 이동하여 아래 명령어를 입력해 keystore 를 생성하자.

> 여기서 JDK 내부 디렉토리란 cmd창에서 C:\Program Files\Java\jdk-11\bin와 같이 자바가 다운되어있는 위치
> cmd에서 이 위치로 이동후 사용해야함
> 만약 key.p12가 다른곳에 있다면  밑에와 같이 ke.p12가 있는 위치를 지정해줘야함

```
keytool -importkeystore -srckeystore C:\Users\tmdal\Downloads\openssl-1.0.2j-fips-x86_64\OpenSSL\bin\key.p12 -srcstoretype PKCS12 -destkeystore key.jks -deststoretype JKS
```





```applescript
keytool -genkey -alias tomcat -keyalg RSA -keystore d:\tomcat.keystore
```

```
keytool -genkey -alias tomcat -keyalg RSA -keystore d:\tomcat.keystore -storepass 비밀번호
```
비밀번호도 입력


window
```
del d:\tomcat.keystore

```

linux or macos

```
rm d:\tomcat.keystore
```

위의 명령어에서 `d:\tomcat.keystore`는 삭제하려는 키스토어 파일의 경로와 파일명을 나타냅니다. 정확한 경로와 파일명을 지정하여 해당 명령어를 실행하면 키스토어 파일이 삭제됩니다.
하지만, 키스토어 파일을 삭제하기 전에 해당 파일이 애플리케이션 또는 서버에서 사용 중인지 확인하고, 필요한 경우 백업을 수행하는 것이 좋습니다. 또한, 삭제 작업은 되돌릴 수 없으므로 신중하게 진행해야 합니다.


## Tomcat 에서 `server.xml` 파일 수정하기

```abnf
<Connector port="443" protocol="org.apache.coyote.http11.Http11NioProtocol"
    maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
    clientAuth="false" sslProtocol="TLS"
    keystoreFile="D:\tools\apache-tomcat-9.0.62\conf\tomcat.keystore" keystorePass="123456" />
```

위는 톰캣 9.xxx 버전 기준이다. 설정을 한 이후 서버 켜고 `https://localhost` 접속해보면 된다.









---
참조 - https://blog.naver.com/skinfosec2000/222135874222
https://12bme.tistory.com/80


https://namjackson.tistory.com/24


https://ililil9482.tistory.com/114

https://ililil9482.tistory.com/115

https://iotmaker.kr/2023/03/14/install-openssl-make-cert-windows/


https://qor3326.tistory.com/17


https://jake-seo-dev.tistory.com/85
https://hudi.blog/https/
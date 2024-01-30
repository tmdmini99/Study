
```
x509 -req -in private.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out private.crt -days 3650
```
이 명령은 `private.csr`인 Certificate Signing Request (CSR) 파일을 `rootCA.pem`인 CA(Certificate Authority) 인증서와 `rootCA.key`인 CA 개인 키를 사용하여 서명합니다. 결과적으로, `private.crt`라는 이름의 CA에 의해 서명된 인증서가 생성됩니다. 이 인증서는 3650일 동안 유효합니다.


![[Pasted image 20240130161405.png]]



![[Pasted image 20240130161728.png]]


자체 서명
```
x509 -req -days 365 -in localhost.csr -signkey localhost.key -out localhost.crt
```


openssl.exe에서 모듈이 동일한지 확인 하는 코드 동일해야 사용가능
```
openssl x509 -noout -modulus -in C:\Users\tmdal\Downloads\openssl-1.0.2j-fips-x86_64\OpenSSL\bin\private.crt > crt.modulus

openssl rsa -noout -modulus -in C:\Users\tmdal\Downloads\openssl-1.0.2j-fips-x86_64\OpenSSL\bin\private.key > key.modulus


```
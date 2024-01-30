
```
x509 -req -in private.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out private.crt -days 3650
```
이 명령은 `private.csr`인 Certificate Signing Request (CSR) 파일을 `rootCA.pem`인 CA(Certificate Authority) 인증서와 `rootCA.key`인 CA 개인 키를 사용하여 서명합니다. 결과적으로, `private.crt`라는 이름의 CA에 의해 서명된 인증서가 생성됩니다. 이 인증서는 3650일 동안 유효합니다.




자체 서명
```
x509 -req -days 365 -in localhost.csr -signkey localhost.key -out localhost.crt
```
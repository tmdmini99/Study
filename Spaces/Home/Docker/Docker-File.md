Dockerfile은 단순히 텍스트 파일 형식으로 만들어야 합니다. Dockerfile의 내용은 Docker 이미지를 정의하는 명령어들의 집합으로, 일반적으로 `.Dockerfile` 확장자를 사용하지 않고, 파일 이름을 그냥 `Dockerfile`로 설정합니다. 확장자는 필요 없습니다


Web 서비스용 Dockerfile (`Dockerfile`)

```file
# web/Dockerfile

# 기본 Apache HTTPD 이미지를 사용
FROM httpd:2.4

# Apache 설정 파일과 SSL 인증서 복사
COPY ./my-httpd.conf /usr/local/apache2/conf/httpd.conf
COPY ./ssl/certificate.crt /usr/local/apache2/conf/ssl.crt/certificate.crt
COPY ./ssl/private.key /usr/local/apache2/conf/ssl.key/private.key
COPY ./ssl/ca-bundle.crt /usr/local/apache2/conf/ssl.crt/ca-bundle.crt

# 포트 443을 외부로 노출
EXPOSE 443

```


App 서비스용 Dockerfile (`Dockerfile`)

```file
# app/Dockerfile

# 예시로 Node.js 애플리케이션을 사용
FROM node:18

# 애플리케이션 코드를 컨테이너로 복사
WORKDIR /usr/src/app
COPY . .

# 의존성 설치
RUN npm install

# 애플리케이션 실행
CMD ["npm", "start"]

# 포트 8080을 외부로 노출
EXPOSE 8080

```



Docker Compose 파일 작성 (`docker-compose.yml`)

```yml
version: '3'
services:
  web:
    build:
      context: ./web
      dockerfile: Dockerfile
    ports:
      - "443:443"
    volumes:
      - ./web/ssl:/usr/local/apache2/conf/ssl

  app:
    build:
      context: ./app
      dockerfile: Dockerfile
    ports:
      - "8080:8080"

```



Docker FIle과 Docker-compose,yml이 같은 위치에 있을경우

```
project-root/
├── Dockerfile
└── docker-compose.yml

```

`docker-compose.yml`

```yml
version: '3'
services:
  app:
    build: .
    ports:
      - "8080:8080"

```


**다른 위치에 둘 경우**:

```
project-root/
├── docker-compose.yml
├── app/
│   └── Dockerfile
└── mysql/
    └── Dockerfile

```


```yml
version: '3'
services:
  tomcat:
    build:
      context: ./app
      dockerfile: Dockerfile
    ports:
      - "8080:8080"
  
  mysql:
    build:
      context: ./mysql
      dockerfile: Dockerfile
    ports:
      - "3306:3306"

```

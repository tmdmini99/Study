.env 파일

루트 디렉토리에 

```bash
npm install @nestjs/config
```
설치후
.env 파일 내용

```env
# MongoDB 환경 변수

MONGO_INITDB_ROOT_USERNAME=root

MONGO_INITDB_ROOT_PASSWORD=rootpassword

  

# JWT 설정

JWT_SECRET_KEY=your-secret-key

JWT_EXPIRATION_TIME=3600 # 1 hour

  

# 포트 설정

PORT=3000

  

# MongoDB 연결 URI (서버 코드에서 사용)

MONGO_URI_AUTH=mongodb://root:rootpassword@mongo-auth:27017/auth-db?authSource=admin

MONGO_URI_EVENT=mongodb://root:rootpassword@mongo-event:27017/event-db?authSource=admin

  

# 기타 설정

SOME_OTHER_CONFIG=value
```

그 후
```yaml
version: '3.8'

  

services:

mongo-auth:

image: mongo:6

container_name: mongo-auth

restart: always

ports:

- "27017:27017"

environment:

MONGO_INITDB_ROOT_USERNAME: ${MONGO_INITDB_ROOT_USERNAME}

MONGO_INITDB_ROOT_PASSWORD: ${MONGO_INITDB_ROOT_PASSWORD}

volumes:

- ./mongo/init-auth.js:/docker-entrypoint-initdb.d/init-auth.js:ro

- mongo-auth-data:/data/db

  

mongo-event:

image: mongo:6

container_name: mongo-event

restart: always

ports:

- "27018:27017"

environment:

MONGO_INITDB_ROOT_USERNAME: ${MONGO_INITDB_ROOT_USERNAME}

MONGO_INITDB_ROOT_PASSWORD: ${MONGO_INITDB_ROOT_PASSWORD}

volumes:

- ./mongo/init-event.js:/docker-entrypoint-initdb.d/init-event.js:ro

- mongo-event-data:/data/db

  

volumes:

mongo-auth-data:

mongo-event-data:
```

이렇게 사용


```bash
npm install @nestjs/jwt @nestjs/passport passport passport-jwt bcryptjs
```

```bash
nest generate module auth
nest generate service auth
nest generate controller auth
```

#### 1.3. Auth 서비스 구현

`auth.service.ts` 파일을 열고 JWT 인증과 관련된 로직을 작성합니다.

```ts
// src/auth/auth.service.ts

import { Injectable } from '@nestjs/common';
import { JwtService } from '@nestjs/jwt';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import * as bcrypt from 'bcryptjs';
import { User } from '../users/schemas/user.schema';

@Injectable()
export class AuthService {
  constructor(
    @InjectModel(User.name) private userModel: Model<User>,
    private jwtService: JwtService,
  ) {}

  async validateUser(email: string, pass: string): Promise<any> {
    const user = await this.userModel.findOne({ email });
    if (user && bcrypt.compareSync(pass, user.password)) {
      const { password, ...result } = user.toObject();
      return result;
    }
    return null;
  }

  async login(user: any) {
    const payload = { email: user.email, sub: user._id };
    return {
      access_token: this.jwtService.sign(payload),
    };
  }
}
```



#### 1.4. JWT 모듈 설정

`auth.module.ts` 파일에 JWT 모듈을 설정합니다.

```ts
// src/auth/auth.module.ts

import { Module } from '@nestjs/common';
import { JwtModule } from '@nestjs/jwt';
import { AuthService } from './auth.service';
import { AuthController } from './auth.controller';
import { UsersModule } from '../users/users.module';
import { MongooseModule } from '@nestjs/mongoose';
import { User, UserSchema } from '../users/schemas/user.schema';

@Module({
  imports: [
    MongooseModule.forFeature([{ name: User.name, schema: UserSchema }]),
    JwtModule.register({
      secret: 'yourSecretKey', // 적절한 secret으로 변경
      signOptions: { expiresIn: '60s' },
    }),
    UsersModule,
  ],
  providers: [AuthService],
  controllers: [AuthController],
})
export class AuthModule {}
```
#### 1.5. Auth 컨트롤러 구현

`auth.controller.ts` 파일에서 로그인 엔드포인트를 설정합니다.

```ts
// src/auth/auth.controller.ts

import { Controller, Post, Body } from '@nestjs/common';
import { AuthService } from './auth.service';
import { LoginDto } from './dto/login.dto';

@Controller('auth')
export class AuthController {
  constructor(private authService: AuthService) {}

  @Post('login')
  async login(@Body() loginDto: LoginDto) {
    return this.authService.login(loginDto);
  }
}
```


#### 1.6. DTO 파일 생성

로그인할 때 사용할 DTO 파일을 생성합니다.

```ts
// src/auth/dto/login.dto.ts

export class LoginDto {
  email: string;
  password: string;
}
```

#### 1.7. User Schema 설정 (MongoDB 연결)

`users/schemas/user.schema.ts` 파일을 만들어서 MongoDB에서 사용자 데이터를 저장할 수 있도록 설정합니다.

```ts
// src/users/schemas/user.schema.ts

import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { Document } from 'mongoose';

@Schema()
export class User extends Document {
  @Prop()
  email: string;

  @Prop()
  password: string;
}

export const UserSchema = SchemaFactory.createForClass(User);
```

### 2. MongoDB 연결 설정

NestJS에서 MongoDB에 연결하려면 **MongooseModule**을 설정해야 합니다. `app.module.ts` 파일에 MongoDB 연결 정보를 추가합니다.

```ts
// src/app.module.ts

import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';
import { AuthModule } from './auth/auth.module';
import { UsersModule } from './users/users.module';

@Module({
  imports: [
    MongooseModule.forRoot('mongodb://localhost/nest'), // MongoDB URI로 변경
    AuthModule,
    UsersModule,
  ],
})
export class AppModule {}
```


### 3. Dockerfile 작성

NestJS 애플리케이션을 Dockerize 하기 위해 Dockerfile을 작성합니다.

```dockerfile
# Stage 1: Build
FROM node:16 AS build

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

# Stage 2: Production
FROM node:16 AS production

WORKDIR /app

COPY --from=build /app /app

RUN npm install --production

CMD ["npm", "run", "start:prod"]
```

### 4. Docker Compose 설정

`docker-compose.yml` 파일을 작성하여 MongoDB와 NestJS 애플리케이션을 함께 실행할 수 있도록 설정합니다.

```yaml
version: '3.7'
services:
  auth-service:
    build: .
    container_name: auth-service
    ports:
      - "3000:3000"
    depends_on:
      - mongo
    environment:
      - MONGO_URI=mongodb://mongo:27017/nest
    networks:
      - nest-network

  mongo:
    image: mongo:latest
    container_name: mongo
    volumes:
      - ./data:/data/db
    networks:
      - nest-network

networks:
  nest-network:
    driver: bridge
```

### 5. NestJS 애플리케이션 실행

위 모든 설정이 완료되면 Docker를 사용하여 애플리케이션을 실행할 수 있습니다.

```bash
docker-compose up --build
```


### 3. MongoDB 연결 정보 설정 (NestJS에서 사용)

MongoDB와 연결하는 코드를 `app.module.ts`에서 설정합니다. 이 때, `MongooseModule.forRoot()` 메서드를 사용하여 MongoDB 연결 URI를 환경 변수로 설정해줍니다.

```ts
// src/app.module.ts

import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';
import { AuthModule } from './auth/auth.module';
import { UsersModule } from './users/users.module';

@Module({
  imports: [
    MongooseModule.forRoot(process.env.MONGO_URI || 'mongodb://localhost/nest'),  // 환경 변수로 URI 설정
    AuthModule,
    UsersModule,
  ],
})
export class AppModule {}
```

### 4. MongoDB 초기화 (Optional)

MongoDB에서 초기 데이터 삽입을 원한다면 `mongo` 서비스에 초기화 스크립트를 추가할 수 있습니다. 예를 들어, MongoDB 초기 데이터를 추가하는 스크립트를 `./init-mongo.js`와 같은 파일로 만들어 `docker-compose.yml`에 추가할 수 있습니다.

```js
// ./init-mongo.js
db.createUser({
  user: 'admin',
  pwd: 'adminpassword',
  roles: [
    {
      role: 'readWrite',
      db: 'nest',
    },
  ],
});
```


그리고 `docker-compose.yml`에 `command` 옵션을 추가하여 MongoDB가 시작될 때 이 스크립트를 실행하게 할 수 있습니다.

```yaml
mongo:
  image: mongo:latest
  container_name: mongo
  volumes:
    - ./data:/data/db
    - ./init-mongo.js:/docker-entrypoint-initdb.d/init-mongo.js  # 초기화 스크립트 추가
  environment:
    MONGO_INITDB_ROOT_USERNAME: root
    MONGO_INITDB_ROOT_PASSWORD: rootpassword
  networks:
    - nest-network
```

```bash
event-task/
├── src/
├── test/
├── node_modules/
├── dist/
├── package.json
├── docker/
│   └── init-mongo.js
└── docker-compose.yml

```

docker 파일 생성 후 그 안에 js파일 생성
```yaml
version: '3.8'

services:
  mongodb:
    image: mongo:latest
    container_name: mongodb
    ports:
      - "27017:27017"
    volumes:
      - ./docker/init-mongo.js:/docker-entrypoint-initdb.d/init-mongo.js
    environment:
      MONGO_INITDB_ROOT_USERNAME: example
      MONGO_INITDB_ROOT_PASSWORD: example
      MONGO_INITDB_DATABASE: my_database
    networks:
      - event-task-network

  # 다른 서비스들 (예: NestJS 서버 등)
  app:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: event-task-app
    ports:
      - "3000:3000"
    depends_on:
      - mongodb
    networks:
      - event-task-network

networks:
  event-task-network:
    driver: bridge

```

### 5. Docker Compose 실행

이제 모든 설정이 완료되었습니다. 아래 명령어를 통해 Docker Compose를 실행하고 모든 서비스를 시작할 수 있습니다.

```bash
docker-compose up --build
```



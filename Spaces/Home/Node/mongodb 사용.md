


```bash
npm install @nestjs/mongoose mongoose
```

로 설치

```bash
npm list @nestjs/core @nestjs/mongoose mongoose
```



`MongooseCoreModule`이 `ModuleRef` 의존성을 못 찾는다는 뜻인데, 이건 보통 `@nestjs/mongoose`와 NestJS 코어 라이브러리 버전이 맞지 않을 때 많이 발생합니다.

이 경우가 발생 할수 있으니

최신 버전 다운
```bash
npm install @nestjs/mongoose@11 mongoose@7

```
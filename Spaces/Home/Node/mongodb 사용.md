


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

나중에  user_event_log에 넣을 데이터 테스트 코드에 넣을 예정
userId, eventId는 insert 된거에서 뽑아 올 예정

```json
{

"_id": {

"$oid": "682886e2266610acda6a4181"

},

"userId": "682884b2c2f72b019c65d0fc",

"eventId": "68288169ca911b184d65d0fb",

"action": "INVITE_FRIEND",

"timestamp": {

"$date": "2025-05-17T12:00:00.000Z"

}

}
```
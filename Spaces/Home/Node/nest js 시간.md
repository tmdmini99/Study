
nest js 시간을 맞추려면
여기를
```json
"start": "nest start",

"start:dev": "nest start --watch",

"start:debug": "nest start --debug --watch",

"start:prod": "node dist/main",
```

이렇게 수정
```json
"start": "TZ=Asia/Seoul nest start",

"start:dev": "TZ=Asia/Seoul nest start --watch",

"start:debug": "TZ=Asia/Seoul nest start --debug --watch",

"start:prod": "TZ=Asia/Seoul node dist/main",
```
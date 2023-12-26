
1. User 생성 
```sql
create user 유저명@host identified by 비밀번호; 
create user 'user01'@'%' identified by 'user01';
create user 'test'@host identified by '1234';
```


```sql
-- host 
localhost : localhost 에서만 접근 가능 
% : 모든 외부 ip에서 접근 가능 
211.211.211.211 : 211.211.211.211 에서만 접근 가능 
211.211.211.% : 211.211.211.xxx(/24) 대역대에서만 접근 가능
```

2. 권한 적용
```sql
grant all privileges on database명.* to 유저명@host; 
grant all privileges on user01.* to 'user01'@'%';
```



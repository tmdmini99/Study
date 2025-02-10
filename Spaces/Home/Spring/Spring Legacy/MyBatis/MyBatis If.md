
```xml
<if test="dateKey != null and !dateKey.equals('')">
```



```xml
<if test="!''.equals(dateKey)">
```

앞에 문자열.equlas()이렇게 쓰면 자동으로 null 체크도 해줌

















httpd.conf 안에 추가를 안해도 되는데 혹시 몰라서 저장
```
# mod_jk 추가
LoadModule jk_module modules/mod_jk.so
JkWorkersFile conf/workers.properties
JkLogFile logs/mod_jk.log
JkLogLevel info
JkLogStampFormat "[%a %b %d %H:%M:%S %Y]"
JkOptions +ForwardKeySize +ForwardURICompat -ForwardDirectories
```


workers.properties 두명일때 예시

```
worker.list=worker1, worker2

worker.worker1.type=ajp13
worker.worker1.host=tomcat1
worker.worker1.port=8009

worker.worker2.type=ajp13
worker.worker2.host=tomcat2
worker.worker2.port=8009

```






---
참조 - https://know-one-by-one.tistory.com/108 멀티 톰캣 아파치 설
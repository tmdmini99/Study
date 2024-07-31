인텔리제이 메모리 사이즈 변경
## **인텔리제이 메모리 사이즈 변경하기**

자바 프로그램 성능 테스트를 할때나 아니면 스펙을 올리고 싶을때 가끔 힙 메모리를 변경해야 할 경우가 생긴다.

인텔리제이에서는 정말 간단하게 메모리 설정을 할 수 있으며, 이외에도 JVM 옵션들을 한꺼번에 처리해주는 기능을 제공한다.

---

### **인텔리제이 전역 메모리 설정**

**1. Help > Change Memory Settings 메뉴 클릭**
![[IntelliJ JVM1.png]]


**2. Maximum Heap Size의 사이즈 값을 변경**

- 이때 반드시 Save and Restart를 해주어야 변경 사항이 적용이 된다.

![[IntelliJ JVM2.png]]



3. 인텔리제이 하단 표시줄을 우클릭 하여 Memory Indecator 체크한다.
![[IntelliJ JVM3.png]]


4. 그러면 하단 표시줄 맨 우측에 사용 메모리양이 표시된다.

![[IntelliJ JVM4.png]]


힙 메모리 뿐만 아니라 인텔리제이 자바 프로젝트에 적용할 전반적인 JVM 옵션들을 관리하고 추가하기 위해서는 idea64.exe.vmoptions 파일을 설정해주면 된다.

idea64.exe.vmoptions 파일은 인텔리제이 설치 폴더의 bin 폴더 안에 위치해 있으며, 인텔리제이에서는 다음 메뉴를 누르면 자동으로 열어준다.


![[IntelliJ JVM5.png]]


```bash
-Xms2g # 초기 Heap 사이즈
-Xmx2g # 최대 Heap 사이즈
-XX:ReservedCodeCacheSize=256m # 코드 캐쉬 사이즈 Heap 메모리 사이즈와 공유하지 않는다.
-XX:+UseG1GC # G1GC 가비지 컬랙션을 사용한다.
-XX:MetaspaceSize=768m # Java8 이상의 Permanent 영역 사이즈
-XX:MaxMetaspaceSize=768m # Java8 이상의 최대 Permanent 영역 사이즈
-XX:+UseCompressedOops # 64비트 JVM에서 압축 참조를 사용 가능
-XX:MaxGCPauseMillis=200 # GC로 인한 최대 중단시간을 명시
-XX:ParallelGCThreads=4 # 다중 GC를 위해 사용되어질 GC 스레드의 수
-XX:ConcGCThreads=1 # 동시적 CMS 단계가 동작할때에 사용할 쓰레드 개수를 정의
-XX:+HeapDumpOnOutOfMemoryError # OutOfMemoryError 발생 시 자동으로 heap dump를 생성
-XX:ErrorFile=$USER_HOME/java_error_in_idea_%p.log # 에러파일 생성 위치
-XX:HeapDumpPath=$USER_HOME/java_error_in_idea.hprof # HeapDump 파일 생성 위치
-ea # assertions을 사용한다.
-server # 자바 HotSpot Server VM
-Dsun.io.useCanonCaches=false # Java의 정규화 캐시 사용여부
-Djava.net.preferIPv4Stack=true # IP4를 사용여부
-Dfile.encoding=UTF-8 # Java 소스파일 인코딩

```



---
출처 - https://inpa.tistory.com/entry/IntelliJ-%F0%9F%92%BD-JVM-%ED%9E%99-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%82%AC%EC%9D%B4%EC%A6%88-%EB%B3%80%EA%B2%BD%ED%95%98%EA%B8%B0
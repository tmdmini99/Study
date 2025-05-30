

# 2. **소스 루트 설정 확인**

- `src/main/java` 폴더가 인텔리제이에서 **Sources Root**(파란색 폴더)로 설정되어 있어야 합니다.
    
- 프로젝트 창에서 `src/main/java` 폴더를 우클릭 → `Mark Directory as` → `Sources Root`로 지정해 주세요.

설정 해주고 


### 실행 확인 체크리스트

1. IntelliJ 메뉴 → `Run > Edit Configurations`
    
2. `+ > Application` 클릭
    
3. 아래처럼 정확히 입력했는지 다시 확인:

| 항목                          | 예시 입력                                                        |
| --------------------------- | ------------------------------------------------------------ |
| **Name**                    | `SpringBootRun` 등 자유롭게                                       |
| **Main class**              | `com.example.servingwebcontent.ServingWebContentApplication` |
| **Use classpath of module** | `serving-web-content-complete` (정확히 모듈 이름)                   |
| VM options                  | (비워도 됨)                                                      |
- Apply → OK
    
- 상단 ▶ 버튼으로 실행


---

## gs-serving-web-content

여기서 실행 시키려면 

gs-serving-web-content 폴더를 여는것이 아닌
그 안에 pom이 있는 파일 complete 파일을 열어야 함


## test 

test 코드 실행 시

- - `File > Settings (Preferences on Mac) > Build, Execution, Deployment > Build Tools > Gradle`
        
    - 여기서 `Gradle JVM`을 프로젝트 SDK(예: Java 17 또는 21)와 동일하게 설정해 주세요.
 이 부분이 

**프로젝트 SDK 설정:**

- `File > Project Structure > Project`
    
- Project SDK가 원하는 Java 버전으로 설정되어 있는지 확인.
랑 똑같이 설정 되어있는지 확인

윈도우에서 캐시가 꼬였을 경우

```bash
gradlew --stop
rd /s /q "%USERPROFILE%\.gradle\caches"
rd /s /q "%USERPROFILE%\.gradle\daemon"
gradlew clean build --refresh-dependencies

```

멈추고 삭제 후 다시 로드
멈추지 않으면 캐시를 삭제해도 적용이 안될수도 있음

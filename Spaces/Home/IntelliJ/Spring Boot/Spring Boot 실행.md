

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


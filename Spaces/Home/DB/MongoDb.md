

테이블 대신 컬렉션 개념

dbeaver에서 접속 방법

### 2. **DBeaver에서 MongoDB 연결 설정하기**

1. **DBeaver 실행**: DBeaver를 실행합니다.
    
2. **새 연결 추가**:
    
    - DBeaver가 실행되면 상단의 메뉴에서 **`Database`** → **`New Database Connection`**을 클릭합니다.
        
    - 연결할 데이터베이스 유형을 선택하는 창이 열리면, **`MongoDB`**를 선택하고 **`Next`**를 클릭합니다.
        
3. **MongoDB 접속 정보 입력**:
    
    - **Host**: MongoDB의 호스트 주소를 입력합니다. 로컬에서 실행 중이라면 `localhost` 또는 `127.0.0.1`을 입력합니다.
        
    - **Port**: 기본 MongoDB 포트는 `27017`입니다. Docker Compose에서 포트를 `27017`로 매핑했기 때문에 기본값을 그대로 사용하면 됩니다.
        
    - **Database**: 접속하려는 MongoDB 데이터베이스 이름을 입력합니다. 예를 들어, `my_database`와 같은 이름을 입력합니다.
        
    - **Authentication**:
        
        - **Username**: `docker-compose.yml`에 설정한 MongoDB의 사용자 이름을 입력합니다 (예: `example`).
            
        - **Password**: `docker-compose.yml`에 설정한 MongoDB의 비밀번호를 입력합니다 (예: `example`).
            
    
    예시로는 아래와 같이 설정할 수 있습니다:
    
    - **Host**: `localhost`
        
    - **Port**: `27017`
        
    - **Database**: `my_database` (혹은 사용자가 설정한 데이터베이스 이름)
        
    - **Username**: `example` (docker-compose에서 설정한 사용자 이름)
        
    - **Password**: `example` (docker-compose에서 설정한 비밀번호)
        
4. **연결 테스트**:
    
    - 위의 정보를 입력한 후, **`Test Connection`** 버튼을 클릭하여 연결이 제대로 되는지 확인합니다. 연결이 성공하면 **`Finish`** 버튼을 클릭합니다.
        
5. **연결 완료**:
    
    - 연결이 성공하면 DBeaver의 왼쪽 메뉴에서 MongoDB 연결이 나타납니다. 해당 연결을 클릭하면 데이터베이스 및 컬렉션을 탐색할 수 있습니다.
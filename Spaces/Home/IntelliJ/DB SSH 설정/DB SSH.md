

DBeaver에서 SSH 터널을 통해 DB에 연결하는 것과 유사하게, IntelliJ IDEA에서 데이터베이스에 연결할 때도 SSH 터널을 사용하여 설정할 수 있습니다. 그러나 IntelliJ에서의 설정 과정은 DBeaver와 약간 다를 수 있습니다. IntelliJ에서 SSH 터널을 사용하여 데이터베이스에 연결하려면 다음과 같이 설정을 진행하세요:

### 1. IntelliJ에서 데이터베이스 도구 창 열기

1. IntelliJ IDEA를 열고, 오른쪽에 있는 **"Database"** 탭을 클릭합니다.
2. 데이터베이스 창이 없으면 `View > Tool Windows > Database`를 통해 활성화할 수 있습니다.

![[Pasted image 20240904104546.png]]


### 2. 새 데이터베이스 연결 설정

1. 데이터베이스 창에서 `+` 버튼을 클릭하고, **"Data Source"**에서 연결하려는 데이터베이스 유형을 선택합니다. (예: MySQL, PostgreSQL 등)


### 3. SSH 터널 설정

1. **"General"** 탭에서 데이터베이스 이름, 사용자 이름, 비밀번호 등 기본 정보를 입력합니다.
2. **"SSH/SSL"** 탭으로 이동합니다.
3. **"Use SSH tunnel"** 체크박스를 선택합니다.
4. SSH 서버 정보를 입력합니다:
    - **Host:** SSH 서버 주소 (예: `ssh.example.com`)
    - **Port:** SSH 포트 (보통 `22`)
    - **User:** SSH 사용자 이름
    - **Authentication Type:** SSH 인증 방법 선택 (예: Password, Key pair)
    - **Private Key file (if needed):** SSH 키 파일 경로 입력

### 4. 로컬 포트 포워딩 확인

- IntelliJ에서는 SSH 터널 설정 시 로컬 포트 포워딩을 자동으로 설정합니다. DBeaver와 마찬가지로, 이 로컬 포트가 충돌되지 않도록 확인합니다.
- 데이터베이스 호스트 필드에 로컬호스트 (`localhost`)와 IntelliJ가 지정한 포트를 사용합니다.

### 5. 데이터베이스 연결 테스트

1. 모든 설정이 완료되었으면, **"Test Connection"** 버튼을 눌러 연결이 잘 되는지 확인합니다.
2. 연결이 성공하면 **"OK"**를 눌러 설정을 저장합니다.
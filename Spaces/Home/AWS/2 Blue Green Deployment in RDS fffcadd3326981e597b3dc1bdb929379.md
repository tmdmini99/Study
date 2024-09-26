# 2. Blue/Green Deployment in RDS

## [실습 목적]

- RDS 구성에서 사용할 수 있는 Blue/Green Deployment 기능에 대한 이해
    - Blue/Green Deployment로 가장 흔하게 시도하게 될 버전 업그레이드(5.7 > 8.0) 상황 실습
    - 버전 업그레이드 시 에러 확인 방법, 해결 방법 실습

## [실습 진행 전 주의 사항]

모든 실습은 RDS MySQL 기준으로 진행할 예정입니다.

실제 환경에서는 DB 엔진에 따라 사용에 차이가 생길 수 있으니 도입 전 충분한 테스트 및 검증을 거치는 것을 권장 드립니다.

## [실습 진행]

### 0. 실습 시작 전 구성도

![image.png](image.png)

### 1. 사전 구성 요소 확인

- 이전 실습에서 생성한 RDS
    - RDS 자동 백업 활성화 확인
        
        ![image.png](image%201.png)
        
        ![image.png](image%202.png)
        
    - Parameter Group의 binlog_format 확인
        
        ![image.png](image%203.png)
        
        ![image.png](image%204.png)
        
        ![image.png](image%205.png)
        

### 2. Blue/Green Deployment 생성

![image.png](image%206.png)

- 블루/그린 배포 식별자(`bg-deployment`) 입력
- 블루/그린 배포 설정
    - 그린 데이터베이스의 엔진 버전 : 현재 버전(`MySQL 5.7.44-rds.20240808`)과 동일한 버전
    - 그린 데이터베이스의 DB 파라미터 그룹 선택 : 동일 DB 파라미터 그룹(`pg-mysql-57`)
    
    ![image1.png](image1.png)
    
    - 다른 설정은 모두 기본값
    - ‘스테이징 환경 생성’ 클릭
    
    ![image1-1.png](image1-1.png)
    

### 3. Blue/Green Deployment 생성 완료 확인

![image3.png](image3.png)

### 4. 현재 구성도

![image.png](image%207.png)

### 5. Green DB 엔진 버전 업그레이드

- Green DB를 수정해 버전 업그레이드 진행
    
    ![image3.png](image3%201.png)
    
- DB 엔진 버전 : 8.0.39 선택
    
    ![image4.png](image4.png)
    
- 추가 구성에서 DB 파라미터 그룹/옵션 그룹 선택
    - DB 파라미터 그룹 : `pg-mysql-80`
    - 옵션 그룹 : `mysql80-option-group`
    
    ![image.png](image%208.png)
    

- 수정 내역 확인 및 즉시 적용
    
    ![image.png](image%209.png)
    

### 6. 버전 업그레이드 확인

![image7.png](image7.png)

- Green DB의 상태가 ‘사용 가능’으로 변경되면 업그레이드 로그 확인
    
    ![image8.png](image8.png)
    
    - ‘로그’ 항목에서 PrePatchCompatibility.log 확인
    
    ![image9.png](image9.png)
    
    ![image10.png](image10.png)
    

### 7. Blue/Green Deployment 전환

- 전환 도중 Downtime 확인을 위해 스크립트 실행
    
    ```bash
    # Python 스크립트 확인
    # simple_failover.py : RDS 연결 확인 스크립트
    cd ~/hands-on/
    ls -l
    ...
    -rw-rw-r-- 1 ubuntu ubuntu 3727 Aug 13 01:43 simple_failover.py
    
    # 전환 도중 다운타임 확인을 위한 스크립트 실행
    cd ~/hands-on
    python3 simple_failover.py -e$DBENDPOINT -u$DBUSER -p$DBPASS
    ```
    
    ![image17.png](image17.png)
    
- Blue/Green Deployment 전환 실행
    
    ![image13.png](image13.png)
    
    - Blue DB - Green DB 간 변경점 확인
    
    ![image14.png](image14.png)
    
    - 제한 시간 설정은 기본값(5분)
    - ‘전환’ 클릭
    
    ![image15.png](image15.png)
    
    ![image16.png](image16.png)
    
    - 전환 도중 Downtime 확인
        
        ![image18.png](image18.png)
        

### 8. 전환 결과 확인 및 리소스 삭제 진행

- 전환 완료 후 구성도
    
    ![image.png](image%2010.png)
    

- Blue/Green Deployment 삭제
    
    ![image20.png](image20.png)
    

![image21.png](image21.png)

- Old Blue DB 삭제
    
    ![image22.png](image22.png)
    
    ![image23.png](image23.png)
    

### 9. 실습 완료 후 구성도

![image.png](image.png)

[다음 실습 : DB Monitoring in RDS](https://www.notion.so/3-DB-Monitoring-in-RDS-fffcadd332698126ac6ce4173fa9e04c?pvs=21)
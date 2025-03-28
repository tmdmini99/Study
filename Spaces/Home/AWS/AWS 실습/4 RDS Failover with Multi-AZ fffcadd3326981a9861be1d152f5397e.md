# 4. RDS Failover with Multi-AZ

## [실습 목표]

- RDS의 Multi AZ 구성에서 Active > Standby 전환 확인
- 전환 중 발생하는 Downtime 확인

## [실습 진행 전 주의 사항]

모든 실습은 RDS MySQL 기준으로 진행할 예정입니다.

실제 환경에서는 DB 엔진에 따라 사용에 차이가 생길 수 있으니 도입 전 충분한 테스트 및 검증을 거치는 것을 권장 드립니다.

## [실습  진행]

### 0. 실습 시작 전 구성도

![RDS Failover1.png](RDS%20Failover1.png)

### 1. 사전 구성 요소 확인

- 실습에 사용할 RDS
- EC2(HandsOn-Client)
    - RDS Downtime을 확인할 Python 스크립트
        
        ```bash
        # Python 스크립트 확인
        # simple_failover.py : RDS 연결 확인 스크립트
        cd ~/hands-on/
        ls -l
        ...
        -rw-rw-r-- 1 ubuntu ubuntu 3727 Aug 13 01:43 simple_failover.py
        ```
        

### 2. RDS 연결 상태 확인

- 장애 조치를 실행하기 전에 RDS 연결 상태를 지속적으로 확인할 수 있도록 스크립트 실행
    
    ```bash
    # 장애 조치 도중 다운타임 확인을 위한 스크립트 실행
    python3 simple_failover.py -e$DBENDPOINT -u$DBUSER -p$DBPASS
    ```
    
    ![image.png](RDS%20Failover2.png)
    

### 3. RDS 장애 조치 발생

- RDS 콘솔에서 재부팅으로 장애 조치 실행
    
    ![화면 캡처 2024-09-06 172118.png](RDS%20Failover4.png)
    
- 장애 조치로 재부팅 진행
    
    ![화면 캡처 2024-09-06 172225.png](RDS%20Failover5.png)
    
    ![화면 캡처 2024-09-06 172446.png](RDS%20Failoverg5.png)
    
- 연결 상태 확인
    
    ![image.png](RDS%20Failover6.png)
    

![image.png](RDS%20Failover7.png)

### 4. 실습 완료 후 구성

![RDS Failover8.png](RDS%20Failover8.png)
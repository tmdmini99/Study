# 3. DB Monitoring in RDS

## [실습 목표]

RDS에서 사용 가능한 모니터링 기능을 확인하고, 각 기능의 역할과 차이점을 파악

## [실습 진행 전 주의 사항]

모든 실습은 RDS MySQL 기준으로 진행할 예정입니다.

실제 환경에서는 DB 엔진에 따라 사용에 차이가 생길 수 있으니 도입 전 충분한 테스트 및 검증을 거치는 것을 권장 드립니다.

## [실습 진행]

### 0. 실습 시작 전 구성도

![DB Monitoring1.png](DB%20Monitoring1.png)

### 1. 사전 구성 요소 확인

- 이전 실습에서 사용한 RDS
- Python 스크립트를 실행할 EC2(HandsOn-Client)
    
    ```bash
    # Python 스크립트 확인
    # loadtest.py : RDS 부하 발생 스크립트
    cd ~/hands-on/
    ls -l
    ...
    -rw-rw-r-- 1 ubuntu ubuntu 4713 Aug 19 07:35 loadtest.py
    ```
    

### 2. DB 부하 발생

- 부하 발생 작업
    
    ```bash
    # 부하 발생 스크립트의 대상이 될 테이블 생성 및 데이터 삽입
    sysbench oltp_write_only \
    --threads=1 \
    --mysql-host=$DBENDPOINT \
    --mysql-user=$DBUSER \
    --mysql-password="$DBPASS" \
    --mysql-port=3306 \
    --tables=1 \
    --mysql-db=$TARGETDB \
    --table-size=1000000 prepare
    
    # 부하 발생 스크립트 실행
    cd ~/hands-on
    python3 loadtest.py -e$DBENDPOINT -u$DBUSER -p$DBPASS -d$TARGETDB -t250
    ```
    

- (선택)사용 중인 RDS 인스턴스 유형의 최대 Connection 수 확인
    
    ```sql
    # DB 내부에서 최대 연결 수(max_connections) 확인
    # DB 내부에서 확인할 경우
    SHOW GLOBAL VARIABLES LIKE 'max_connections';
    # Client에서 확인할 경우
    mysql -h $DBENDPOINT -u $DBUSER -p $TARGETDB -e "SHOW GLOBAL VARIABLES LIKE 'max_connections';"
    ```
    
    - RDS Parameter Group에서 `max_connections` 확인
        
        ![image.png](DB%20Monitoring2.png)
        

### 3. CloudWatch Monitoring 확인

- RDS 상세 페이지에서 ‘모니터링’ 항목 클릭
    
    ![image.png](DB%20Monitoring3.png)
    

- CloudWatch에서 제공하는 기본적인 RDS 지표 확인 가능
    
    ![image.png](DB%20Monitoring4.png)
    

[CloudWatch에서 제공하는 지표](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/rds-metrics.html)

### 4. Enhanced Monitoring 확인

- 모니터링 페이지에서 향상된 모니터링 페이지로 이동
    
    ![화면 캡처 2024-09-06 153359.png](DB%20Monitoring5.png)
    

- 향상된 모니터링 기본 대시보드
    - ‘그래프 관리’를 통해 지표 추가 가능
    
    ![화면 캡처 2024-09-06 154943.png](DB%20Monitoring6.png)
    

- 그래프 관리를 통해 추가 가능한 지표
    
    ![image.png](DB%20Monitoring7.png)
    

[Enhanced Monitoring에서 제공하는 지표](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/USER_Monitoring-Available-OS-Metrics.html)

### 5. Performance Insight 확인

- 성능 개선 도우미 페이지로 이동
    
    ![화면 캡처 2024-09-06 160044.png](DB%20Monitoring8.png)
    

- 대상 RDS 선택 후 지표 대시보드 확인
    
    ![화면 캡처 2024-09-06 160326.png](DB%20Monitoring9.png)
    
- 성능 개선 도우미를 통해 제공되는 DB 내부 지표
    
    ![화면 캡처 2024-09-06 160705.png](DB%20Monitoring10.png)
    

[성능 개선 도우미 카운터 - Amazon Relational Database Service](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/USER_PerfInsights_Counters.html)

- 성능 개선 도우미에서 확인 가능한 쿼리 정보
    
    ![화면 캡처 2024-09-06 162209.png](DB%20Monitoring11.png)
    

- 쿼리 대기 시간 등 쿼리 관련 정보 확인 가능
    
    ![image.png](DB%20Monitoring12.png)
    


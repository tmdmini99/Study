# 1. RDS Provisioning

## [실습 목적]

RDS를 직접 생성하며 각 설정을 확인한다.

## [실습 진행 전 주의 사항]

모든 실습은 RDS MySQL 기준으로 진행할 예정입니다.

실제 환경에서는 DB 엔진에 따라 사용에 차이가 생길 수 있으니 도입 전 충분한 테스트 및 검증을 거치는 것을 권장 드립니다.

## [실습 진행]

### 0. 실습 시작 전 구성도



![[image.png]]


1. 실습 완료 후 목표 구성도


![[Pasted image 20240926094706.png]]




### 2. 사전 구성 요소 확인

- EC2(HandsOn-Client) 접속을 위한 KeyPair(`HandsOn-KeyPair.pem`) Download


![[Pasted image 20240926094721.png]]





EC2(HandsOn-Client) 접속

![[Pasted image 20240926094730.png]]


```bash
# EC2로 SSH 접속
ssh -i HandsOn-KeyPair.pem ubuntu@[Public IP Address]

# 디렉토리 이동
cd ~/hands-on/

# 파일 확인 
ls -l
```


![[Pasted image 20240926094749.png]]









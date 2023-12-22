현재 수많은 프로그램들이 인터넷으로 통신하는 데 있어 가장 기반이 되는 [[Protocol|프로토콜]]. 실제 대다수 프로그램은 TCP와 IP로 통신(정확히는 '네트워킹')하고 있다.


![[W3E1x5BqrRingHUDAX2LW8fHs7DHHGeGgaTDQIw5wdZhcF1kP5P73zprvMhTijQ61XSyhUzIZIqG14pvzGXNeDO-Jje0CeIbgcM6GBqbjZxT4NZBVs_lEalwvMrpsCp23Qg-YeudLDVYzX8Xr2T40w.webp]]


## *TCP*

TCP는 말 그대로 **전송 제어를 위한 작업을 해주는 역할**을 한다.


### TCP의 역할
- **받을 대상 노드(호스트)가 서비스 가능(연결 가능) 상태인지 확인 및 연결을 수립하는 역할**
- **전송을 제어해주는 정보를 패킷에 추가해주는 역할**

![[Pasted image 20231222111757.png]]

**SYN 단계**  
어플리케이션이 서버에 통신을 위한 연결을 요청하는 단계  
  
**SYN-ACK 단계**  
서버가 어플리케이션에 자신이 활성 상태임을 알리고 어플리케이션에서도 포트를 열어 연결을 활성화하라는 요청 메세지를 전송  
  
**ACK 단계**  
어플리케이션이 서버의 요청 메세지를 수락하여 연결이 수립





---
참조- https://kotlinworld.com/94
현재 수많은 프로그램들이 인터넷으로 통신하는 데 있어 가장 기반이 되는 [[Protocol|프로토콜]]. 실제 대다수 프로그램은 TCP와 IP로 통신(정확히는 '네트워킹')하고 있다.


![[W3E1x5BqrRingHUDAX2LW8fHs7DHHGeGgaTDQIw5wdZhcF1kP5P73zprvMhTijQ61XSyhUzIZIqG14pvzGXNeDO-Jje0CeIbgcM6GBqbjZxT4NZBVs_lEalwvMrpsCp23Qg-YeudLDVYzX8Xr2T40w.webp]]



![[27786B485715047219.gif]]


**1계층 네트워크 액세스 계층(Network Access Layer or Network Interface Layer)**

OSI 7계층의 물리계층과 데이터 링크 계층에 해당한다.

물리적인 주소로 MAC을 사용한다.

LAN, 패킷망, 등에 사용된다.


**2계층 인터넷 계층(Internet Layer)**

OSI 7계층의 네트워크 계층에 해당한다. 

통신 노드 간의 IP패킷을 전송하는 기능과 라우팅 기능을 담당한다.

프로토콜 – IP, ARP, RARP

**3계층 전송 계층(Transport Layer)**

OSI 7계층의 전송 계층에 해당한다.

통신 노드 간의 연결을 제어하고, 신뢰성 있는 데이터 전송을 담당한다.

프로토콜 – TCP, UDP

**4계층 응용 계층(Application Layer)**

OSI 7계층의 세션 계층, 표현 계층, 응용 계층에 해당한다.

TCP/UDP 기반의 응용 프로그램을 구현할 때 사용한다.

프로토콜 – FTP, HTTP, SSH


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

https://velog.io/@hidaehyunlee/%EB%8D%B0%EC%9D%B4%ED%84%B0%EA%B0%80-%EC%A0%84%EB%8B%AC%EB%90%98%EB%8A%94-%EC%9B%90%EB%A6%AC-OSI-7%EA%B3%84%EC%B8%B5-%EB%AA%A8%EB%8D%B8%EA%B3%BC-TCPIP-%EB%AA%A8%EB%8D%B8


https://backendcode.tistory.com/167
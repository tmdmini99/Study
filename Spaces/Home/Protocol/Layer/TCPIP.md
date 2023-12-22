현재 수많은 프로그램들이 인터넷으로 통신하는 데 있어 가장 기반이 되는 [[Protocol|프로토콜]]. 실제 대다수 프로그램은 TCP와 IP로 통신(정확히는 '네트워킹')하고 있다.


![[W3E1x5BqrRingHUDAX2LW8fHs7DHHGeGgaTDQIw5wdZhcF1kP5P73zprvMhTijQ61XSyhUzIZIqG14pvzGXNeDO-Jje0CeIbgcM6GBqbjZxT4NZBVs_lEalwvMrpsCp23Qg-YeudLDVYzX8Xr2T40w.webp]]



![[Pasted image 20231222124447.png]]



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





TCP/IP는 하나의 프로토콜이 아닌 TCP와 IP를 합쳐서 부르는 말입니다. 또한 TCP/IP를 사용하겠다는 것은 **IP** 주소 체계를 따르고 IP Routing을 이용해 목적지에 도달하며 **TCP**의 특성을 활용해 송신자와 수신자의 논리적 연결을 생성하고 신뢰성을 유지할 수 있도록 하겠다는 것을 의미합니다. 즉 **TCP/IP를 말한다는 것은 송신자가 수신자에게 IP 주소를 사용하여 데이터를 전달하고 그 데이터가 제대로 갔는지, 너무 빠르지는 않는지, 제대로 받았다고 연락은 오는지에 대한 이야기를 하는 것**


**Transport Layer(4 Layer)**  
송신자와 수신자의 논리적 연결(Connection)을 담당하는 부분으로, 신뢰성 있는 연결을 유지할 수 있도록 도와줍니다. 즉 Endpoint(사용자) 간의 연결을 생성하고 데이터를 얼마나 보냈는지 얼마나 받았는지, 제대로 받았는지 등을 확인합니다. TCP와 UDP가 대표적입니다.  
   
**Network Layer(3 Layer)**  
IP(Internet Protocol)이 활용되는 부분으로, 한 Endpoint가 다른 Endpoint로 가고자 할 경우, 경로와 목적지를 찾아줍니다. 이를 Routing이라고 하며 대역이 다른 IP들이 목적지를 향해 제대로 찾아갈 수 있도록 돕는 역할을 합니다.




---
참조- https://kotlinworld.com/94

https://velog.io/@hidaehyunlee/%EB%8D%B0%EC%9D%B4%ED%84%B0%EA%B0%80-%EC%A0%84%EB%8B%AC%EB%90%98%EB%8A%94-%EC%9B%90%EB%A6%AC-OSI-7%EA%B3%84%EC%B8%B5-%EB%AA%A8%EB%8D%B8%EA%B3%BC-TCPIP-%EB%AA%A8%EB%8D%B8


https://backendcode.tistory.com/167

https://aws-hyoh.tistory.com/entry/TCPIP-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0

OSI 7 계층은 네트워크 프로토콜이 통신하는 구조를 7개의 계층으로 분리하여 각 계층간 상호 작동하는 방식을 정해 놓은 것이다

![[OSI1.jpg]]


![[OSI2.png]]



![[OSI3.png]]

**물리 계층 : 하드웨어** /  **데이터링크 계층 : 하드웨어 + 소프트웨어** / **3 계층부터는 소프트웨어로 구성**
**1984년 국제표준화기구(ISO)에서 개발한 모델**로써, **네트워크 프로토콜 디자인과 통신 과정을 7개의 계층으로**

**구분하여 만든 "표준 규격**"이다. 초창기의 네트워크는 각 컴퓨터마다 시스템이 달랐기 때문에 하드웨어와

소프트웨어의 논리적인 변경 없이 통신할 수 있는 표준 모델이 나타나게 되었다.

- **통신이 일어나는 과정을 7단계로 크게 구분**하여, 단계별로 파악이 가능
- OSI 참조 모델 혹은 OSI 7 계층이라 불림
- OSI (Open System Interconnection) : 개방형 시스템 => 누구나 참조 및 부가적인 추가 가능
- 컴퓨팅 장치나 네트워킹 장치를 만들 때 이 모델을 참조해서 모든 통신 장치를 만든다.
- **각 계층은 독립적인 모듈**로 구성되어 있음
- **각 계층은 상하 계급 구조**를 가지고 있음
- **상위 계층의 프로토콜이 제대로 동작하기 위해서는 하위의 모든 계층에 문제가 없어야 한다.**

### 데이터 캡슐화

![[OSI4.jpg]]

**데이터 캡슐화**

데이터 캡슐화는 사용자 데이터가 각 계층을 지나면서 하위 계층은 상위 계층으로부터 온 정보를 데이터로 취급하며, 자신의 계층 특성을 담은 제어정보(주소, 에러 제어 등)를 헤더화 시켜 붙이는 일련의 과정을 말한다. 데이터를 보낼 때는 응용 계층에서 시작되어 OSI 계층을 차례로 내려오며 물리 계층으로 간다. 이 과정에서 캡슐화를 하게 되는데 각 계층은 다른 계층과 통신할 때 데이터에 특정 정보가 들어 있는 머리말(헤더)과 꼬리말(푸터)을 추가한 후 다른 계층으로 전달한다. PDU(Protocol Data Unit)은 프로토콜 데이터 단위이며 OSI 모델의 정보 처리 단위이다. 캡슐화 과정에서 만들어진다. 아래 계층으로 내려갈수록 PDU에는 다양한 프로토콜에 의해 헤더와 푸터가 더해진다. 마지막 물리 계층에서 PDU는 최종적인 모습으로 변하며, 데이터를 보내는 접점이 된다. 반대로 데이터를 받은 컴퓨터는 PDU로부터 프로토콜의 헤더와 푸터를 분석하며 올라가 마지막 응용 계층에 도달하면 원본 데이터만 남는다



## *응용 계층*

응용 계층(Application Layer, 7계층)에서는 OSI 7계층 모델에서 <span style="background:#fff88f">최상위 계층으로 사용자가 네트워크 자원에 접근하는 방법을 제공</span>한다. 그리고 계층 7은 최종적으로 사용자가 볼 수 있는 유일한 계층으로 모든 네트워크 활동의 기반이 되는 인터페이스를 제공하는데, 즉 사용자가 실행하는 응용 프로그램들이 계층 7에 속한다고 보면 된다. 예를 들면 가상 터미널인 텔넷(telnet), 구글의 크롬(chrome), 이메일(전자우편), 데이터베이스 관리 등의 서비스를 제공한다. 사용자와 가장 가까운 계층이다.


![[OSI4.png]]



**사용자와 바로 연결되어 있으며 응용 SW를 도와주는 계층**이다.  
**사용자로부터 정보를 입력받아 하위 계층으로 전달하고 하위 계층에서 전송한 데이터를 사용자에게 전달**한다.  
**파일 전송, DB, 메일 전송 등** 여러 가지 **응용 서비스를 네트워크에 연결해주는 역할**을 한다.


- **응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 네트워크에 연결 및 수행하는 역할**
- **사용자와 직접 접하는 유일한 계층**
    
    - **사용자로부터 정보를 입력받아 하위 계층으로 전달하고, 하위 계층에서 전송한 데이터를 사용자에게 전달**
    - UI 부분, I/O부분
    
- **대표적인 프로토콜 :** **HTTP,** DNS, Telnet, FTP 등


## *표현 계층*

표현 계층(Presentation Layer, 6계층)에서는 <span style="background:#fff88f">응용 계층으로부터 전달받은 데이터를 읽을 수 있는 형식으로 변환</span>하는데 표현 계층은 응용 계층의 부담을 덜어주는 역할이 되기도 한다. 응용 계층으로부터 전송받거나 응용 계층으로 전달해야 할 데이터의 인코딩과 디코딩이 이 계층에서 이루어진다. 그리고 표현 계층은 데이터를 안전하게 사용하기 위해서 <span style="background:#fff88f">암호화</span>와 <span style="background:#fff88f">복호화</span>를 하는데 이 작업도 표현 계층에서 이루어진다. 예를 들면 유니코드(UTF-8)로 인코딩 되어있는 문서를 ASCII로 인코딩 된 문서로 변환하려 할 때 이 계층에서 변환이 이루어진다.


![[OSI5.png]]




**송신 측과 수신 측 사이에서 데이터의 형식(png, jpg 등)을 정해**준다.   
**받은 데이터를 코드 변환, 구문 검색, 인코딩 - 디코딩 및 암호화, 압축의 과정**을 통해  
**올바른 표준 방식으로 변환**해준다.



- **응용 계층(7 Layer)으로부터 전달받거나 전송하는 데이터의 인코딩 - 디코딩 및 암호화 등이 이루어지는 계층**
- **코드 간의 번역을 담당**하여 데이터의 형식상 차이를 다루는 **부담을 응용 계층(7 Layer)으로부터 덜어준다.**
- **프로토콜(Protocol) : JPG, MPEG, SMB, AFP**





## *세션 계층*

세션 계층(Session Layer, 5계층)에서는 두 컴퓨터 간의 대화나 세션을 관리하며, <span style="background:#fff88f">포트</span>(Port)연결이라고도 한다. 모든 통신 장치 간에 연결을 설정하고 관리 및 종료하고 또한 연결이 전이중(Full duplex / 양방향)인지 반이중(half duplex / 단방향)인지 여부를 확인하고 체크 포인팅과 유휴, 재시작 과정 등을 수행하며 호스트가 갑자기 중지되지 않고 정상적으로 호스트를 연결하는 데 책임이 있다. 즉 이 계층에서는 TCP/IP 세션을 만들고 없애고 통신하는 사용자들을 동기화하고 오류 복구 명령들을 일괄적으로 다루며 통신을 하기 위한 세션을 확립, 유지, 중단하는 작업을 수행한다.




![[OSI6.png]]



> **통신 세션을 구성하는 계층으로, 포트(Port) 번호를 기반으로 연결**한다.  
> **통신 장치 간의 상호 작용을 설정하고 유지하며 동기화**한다. **동시 송수신(Duplex), 반이중(Half-Duplex),**  
> _반이중_(HDX) 전송에서는 한 시스템에서 데이터 패킷을 전송하고 다른 시스템이 이를 수신합니다. 수신 시스템이 발신 시스템에 응답을 보내지 않으면 다른 데이터 패킷을 전송할 수 없습니다.
> 
   _전이중_(FDX) 전송에서는 전송 및 수신 시스템이 동시에 서로 통신합니다. 즉, 두 대의 모뎀이 동시에 데이터를 전송 및 수신할 수 있습니다. 이는 한 모뎀이 패킷을 수신했다고 응답하는 중에도 데이터 패킷을 수신할 수 있음을 의미합니다. 방식의 통신과 함께 체크 포인팅과 종료, 다시 시작 과정 등을 수행한다.

- **Session(세션) :** **클라이언트와 웹 서버 간 네트워크 연결이 지속 유지되고 있는 상태**

        💡 사용자가 브라우저를 열어 서버에 접속한 뒤 접속을 종료할 시점까지를 의미

- **네트워크 상 양쪽 연결을 관리하고 연결을 지속**시켜주는 계층
- 세션 생성, 유지, 종료, 전송 중단 시 복구 기능 수행 (OS가 세션 계층으로 이 역할 수행)
- **TCP/IP 세션을 만들고 없애는 역할**
- 통신하는 사용자들을 동기화하고 오류 복구 명령들을 일괄적으로 다룬다.
- **프로토콜(Protocol) :** NetBIOS, SSH, TLS


## *전송 계층*

전송 계층(Transport Layer, 4계층)의 주목적은<span style="background:#fff88f"> 하위 계층에 신뢰할 수 있는 데이터 전송 서비스를 제공하는 것</span>이다. 컴퓨터와 컴퓨터 간에 신뢰성 있는 데이터를 서로 주고받을 수 있도록 해주어 상위 계층들이 데이터 전달의 유효성이나 효율성을 생각하지 않도록 부담을 덜어주는데, 이때 시퀀스 넘버 기반의 오류 제어 방식을 사용한다. 흐름 제어, 분할/분리 및 오류 제어를 통해 전송 계층은 데이터가 오류 없이 점-대-점으로 전달되게 하는데 신뢰할 수 있는 데이터 전송을 보장하는 것은 매우 번거롭기에 OSI 모델은 전체 계층을 사용한다. 전송 계층은 연결형 프로토콜과 비 연결형 프로토콜을 모두 사용한다. 전송 계층의 예로는 특정 방화벽이나 프록시 서버가 있다.


![[OSI7.png]]




> **발신지에서 목적지(End-toEnd) 간 제어와 에러를 관리**한다. 패킷(Packet)의 전송이 유효한지 확인하고  
> 전송에 실패된 패킷을 다시 보내는 것과 같은 **신뢰성 있는 통신을 보장**하며, **헤드에는**  
> **세그먼트(Segment)가 포함**된다. 주소 설정, 오류 및 흐름 제어, 다중화를 수행한다.

- **EndPoint의 사용자들이 신뢰성 있는 데이터를 주고받게 해주는 역할**
    
    - **오류 검출 및 복구, 흐름 제어와 중복 검사** 등을 수행
    
- **패킷 생성 및 전송**
    
    - 패킷들의 전송이 유요 한 지 확인하고 전송 실패한 패킷들을 다시 전송
    
- **헤더에 포트 번호가 포함되어 있음**
    
    - 포트 번호 : 디바이스에 있는 여러 프로세스 중 자기가 가야 할 프로세스를 구분하기 위해 필요한 번호
    
- **전송 단위(PDU) : 세그먼트(Segment)**
- **장비 : 게이트웨이(GateWay), L4 스위치**
- **프로토콜(Protocol) : TCP, UDP, ARP, RTP**  
    
    - **TCP**
        
        - 대부분 TCP 사용
        - **신뢰적인 전송 보장**(패킷 손실, 중복, 순서 바뀜 등이 없도록 보장) - ACK 사용
        - **IP가 처리할 수 있도록 데이터를 여러 개의 패킷으로 나누고, 도착지에서 완전한 데이터로 패킷을 재조립**
        - 데이터 전송 단위 : 세그먼트
        
    - **UDP**
        
        - **비연결성, 비신뢰성** 서비스
        - TCP와 다르게 **패킷을 나누고 재조립하는 과정 없이, 수신지에서 제대로 받든 말든 상관하지 않고, 데이터를 보내기만 한다**.  **=> 에러와 그에 따른 재전송, 대체는 애플리케이션에서 처리**해야 한다.
        - But **속도가 빠르다.** => Real Time 서비스에 사용하면 좋다
        - 데이터 전송 단위 : 블록 형태의 다이어그램



## *네트워크 계층*

네트워크 계층(Network Layer, 3계층)에서는 2홉 이상의 통신(멀티 홉 통신)을 담당한다. OSI 7 계층에서 가장 복잡한 계층 중 하나로서 <span style="background:#fff88f">실제 네트워크 간에 데이터 라우팅을 담당</span>한다. *이때 라우팅이란 어떤 네트워크 안에서 통신 데이터를 짜여진 알고리즘에 의해 최대한 빠르게 보낼 최적의 경로를 선택하는 과정을 라우팅이라고 한다.* 네트워크 계층은 네트워크 호스트의 논리 주소 지정(ex : ip 주소 사용)을 확인한다. 또한 데이터 스트림을 더 작은 단위로 분할하고 경우에 따라 오류를 감지해 처리한다. 그리고 여러 개의 노드를 거칠 때마다 경로를 찾아주는 역할을 하는 계층으로서 다양한 길이의 데이터를 네트워크들을 통해 전달하고 그 과정에서 전송 계층이 요구하는 서비스 품질을 제공하기 위한 기능적, 절차적 수단을 제공한다. 네트워크 계층은 라우팅, 흐름 제어, 세그멘테이션, 오류제어, 인터네트워킹 등을 수행한다. 라우터가 3계층에서 동작하고, 3계층에서 동작하는 스위치도 있다.

![[OSI8.png]]



MAC 주소를 IP주소로 변환하여 라우팅 하여 패킷단위로 분할하여 전송후 합쳐진다



> **기기에서 데이터그램(Datagram)이 가는 경로를 설정**해주는 역할을 한다.  
> **라우팅 알고리즘을 사용하여 최적의 경로를 선택**하고 **송신 측으로부터 수신 측으로 전송**한다.  
> 이때, **전송되는 데이터는 패킷(Packet) 단위로 분할하여 전송한 후 다시 합쳐**진다. 데이터 링크 계층(2 계층)이  
> 노드 대 노드 전달을 감독한다면, **네트워크 계층(3 계층)은 각 패킷이 목적지까지 성공적이고 효과적으로**  
> **전달**되도록 한다.

- **목적지 네트워크 주소(IP)를 정하고, 그에 따른 경로(Route)를 선택하고, 경로에 따라 패킷을 전달해 주는 역할**
- **데이터를 목적지까지 가장 안전하고 빠른 경로로 전달하는 기능(라우팅)이 가장 중요** - 프로토콜, 라우팅 기술 등
- 여러 개의 **노드(node)를 거칠 때마다 경로를 찾아주는 역할**을 하는 계층
- **다양한 길이의 데이터를 네트워크에 통해 전달**하고, 그 과정에서 전송 계층(4 Layer)이 요구하는 서비스 품질(QoS)을 제공하기 위한 기능적, 절차적 수단 제공
- **전송 단위(PDU) : 패킷(Packet)**
- **장비 : 라우터, L3 스위치**
- **프로토콜(Protocol) : IP, ICMP 등**



## *데이터링크 계층*

데이터링크 계층(DataLink Layer, 2계층)은 물리적인 네트워크를 통해 데이터를 전송하는 수단을 제공한다. 1홉 통신을 담당한다고도 말한다. 홉(hop)은 컴퓨터 네트워크에서 노드에서 다음 노드로 가는 경로를 말한다. 1홉 통신이면 한 라우터에서 그다음 라우터까지의 경로를 말한다. 주목적은 물리적인 장치를 식별하는 데 사용할 수 있는 주소 지정 체계를 제공하는 것이다. 데이터 링크 계층은 포인트 투 포인트 간의 신뢰성 있는 전송을 보장하기 위한 계층으로 CRC 기반의 오류 제어와 흐름 제어가 필요하다. 네트워크 위의 개체들 간 데이터를 전달하고 <span style="background:#fff88f">물리 계층에서 발생할 수 있는 오류를 찾아내고 수정하는 데 필요한 기능적, 절차적 수단을 제공한다</span>. 이 계층의 예시를 들자면 브리지 및 스위치 그리고 이더넷 등이 있다.



![[OSI9.png]]


> **네트워크 기기들 사이의 데이터 전송을 하는 역할**을 한다. 시스템 간의 오류 없는 데이터 전송을 위해  
> **패킷(Packet)을 프레임(Frame)으로 구성하여 물리 계층(1 계층)으로 전송**한다.   
> 네트워크 계층(3 계층)에서 정보를 받아 주소와 제어 정보를 헤더와 테일에 추가한다.

- **물리적인 네트워크 사이**에 **데이터 전송을 담당하는 계층**
- **Point to Point 간 신뢰성 있는(안전한) 전송을 보장**하기 위한 계층
- **물리 계층(1 layer)을 통해 송수신되는 데이터의 전송 오류를 감지하는 기능을 제공, 오류 감지 시 재전송**
- **MAC(맥) 주소를 가지고 통신**
- **전송 단위(PDU) : 프레임(Frame)**
- **장비 :** 브리지, 스위치 등
- **프로토콜(Protocol) :** 이더넷, MAC, PPP, ATM, LAN, Wifi

※ MAC Address : 컴퓨터 간 데이터를 전송하기 위한 컴퓨터의 물리적 주소

**※ MAC vs IP**

- **IP 주소 간의 통신은 각 라우터에서 일어나는 MAC 주소와 MAC 주소 통신의 연속적인 과정**이다.
- ex) 한국에 있는 주소로 편지를 보낼 때, **IP는 시작점과 끝점에 해당하는 주소**라면, **MAC 주소는 편지가 거쳐가는          중간 거점들**(즉, 바로 옆에 물리적으로 연결되어 있는 노드와 통신 시 사용되는 주소)

※ *이더넷*
이더넷은 여러 장치를 연결하여 LAN(근거리 통신망)을 구축할 목적으로 설계된 네트워킹 기술로서 소기업부터 대규모 데이터 센터에 이르기까지 다양한 환경에서 유선 네트워크를 연결하기 위한 안정적이고 효율적인 표준입니다.

CSMA/CD - https://m.blog.naver.com/mailkkw1/222113214067

## *물리 계층*

물리 계층(Physical Layer, 1계층)은 OSI 모델의 맨 밑에 있는 계층으로서, 네트워크 데이터가 전송되는 물리적인 매체이다. 데이터는 0과 1의 비트열로 ON, OFF의 전기적 신호 상태로 이루어져 있다. 이 계층은 전압, 허브, 네트워크 어댑터, 중계기 및 케이블 사양을 비롯해 사용된 모든 하드웨어의 물리적 및 전기적 특성을 정의한다. <span style="background:#fff88f">물리 계층은 연결을 설정 및 종료하고 통신 자원을 공유하는 수단을 제공하며</span> 디지털에서 아날로그로 또는 그 반대로 신호를 변환하는 역할을 한다. OSI 모델에서 가장 복잡한 계층으로 간주된다.

![[OSI10.png]]



>  OSI 모델의 **최하위** 계층에 속하며,   
> **상위 계층(데이터 링크)에서 전송된 데이터를 물리적인 전송 매체(허브, 라우터, 케이블 등)를 통해**  **다른 시스템에**  
> **전기적 신호를 전송하는 역할**을 한다. **즉, 기계어를 전기적 신호로 바꿔**서 와이어에 실어주는 것이다.

- **최하위 계층**
- 물리적인 전송 매체를 통하여 상위 계층인 데이터 링크 계층으로부터 전달된 비트 스트림을 상대측 물리 계층으로  전달하는 기능을 수행한다.
- 단지 데이터를 전달만 할 뿐, **전송받으려는 데이터가 무엇인지, 어떤 에러가 있는지는 전혀 신경 쓰지 않음**.
- **전송 단위(PDU) :** 비트(1 - 전기적으로 On, 0 - 전기적으로 Off) - **전기 신호의 흐름**
-  **장비 :** 통신 케이블. 허브, 리피터 등
- **프로토콜(Protocol) :** Modem, Cable, Fiber, RS-232C
- **[핵심] : 1 Layer. 물리 계층 : 데이터를 전기적인 신호로 변환해서 주고받는 기능만 수행**




## **7.OSI 모델_전체적인 통신 플로우**


![[OSI10.jpg]]


OSI 7 Layer 전체적인 통신 과정 (출처 : 하단에 기재)

1. 발신 측에서 응용 계층(7 Layer)부터 시작해 각 계층마다 헤더를 붙여 캡슐화를 진행
2. 수신 측에서는 물리 계층(1 Layer)부터 차례로 올라가면서 헤더를 떼 내는 디캡슐레이션을 진행하여 데이터 식별
    
    - ex) 데이터가 목적지로 이동할 때, 네트워크 계층(3 Layer)에서 IP 헤더에 있는 프로토콜 정보를 이용해 데이터가 TCP인지 UDP인지 식별한 후 그에 따른 처리를 전송 계층(4 Layer)에서 수행한다.
    
3. 목적지에 원하는 데이터가 전송된다.


PDU
---
정보를 추가한 계층에 따라 PDU를 지칭하는 이름과 포함하는 내용이 달라진다.

- 전송계층 : 세그먼트(segment)

1. 발신지 포트 : 발신하는 application의 포트
2. 목적지 포트 : 수신해야 할 application의 포트
3. 순서 번호 : 순차적 전송할 경우 순서를 붙이며, 순서가 어긋나면 목적지 프로토콜이 이를 바로 잡는다.
4. 오류검출코드 : 발신지와 목적지 프로토콜은 세그먼트를 연산하여 오류 검출 코드를 각각 만든다. 만약 발신지에서 전송한 세그먼트에 포함된 오류 검출 코드와 목적지에서 만든 오류 검출 코드가 다르다면 전송되는 과정에서 오류가 발생한 것이다. 이 경우, 수신측은 그 세그먼트를 폐기하고 복구 절차를 밟는다. 오류검출코드는 체크섬, 프레임 체크 시퀀스라고도 부른다.

- 네트워크 계층 :패킷(packet)

1. 발신지 컴퓨터 주소 : 패킷의 발신자 주소
2. 목적지 컴퓨터 주소 : 패킷의 수신자 주소
3. 서비스 요청 : 네트워크 접속 프로토콜은 우선 순위와 같은 서브 네트워크의 사용을 요청할 수 있다.







---
참고 - https://shlee0882.tistory.com/110

http://wiki.hash.kr/index.php/OSI_7_%EA%B3%84%EC%B8%B5

https://backendcode.tistory.com/167
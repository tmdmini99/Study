# 도커란?

Go언어로 작성된 리눅스 컨테이너 기반으로 하는 오픈소스 가상화 플랫폼이다.

다시 말해 특정한 서비스를 패키징하고 배포하는데 유용한 오픈소스 프로그램이다.

> 가상화란?
> 	가상화란 물리적 자원인 하드웨어를 효율적으로 활용하기 위해서 하드웨어 공간 위에 가상의 머신을 만드는 기술이고, 컨테이너란 컨테이너가 실행되고 있는 호스트 os의 기능을 그대로 사용하면서 프로세스를 격리해 독립된 환경을 만드는 기술을 뜻합니다.


 도커는 **독립된 환경을 만들어서 하드웨어를 효율적으로 활용하는 기술**
 
도커는 컨테이너 기반의 가상화 플랫폼입니다. 부두에서 컨테이너를 옮기고 관리하는 직업인 `docker`에서 따 온 이름에 걸맞게 컨테이너를 잘 다룰 수 있게 도와 주는 도구라고 할 수 있다.
도커를 이용하면 이미지를 실행시켜 컨테이너로 만들거나, 생성된 컨테이너를 관리하거나, 컨테이너를 다시 이미지로 만드는 작업을 쉽게 할 수 있습니다.

도커는 아주 강력한 도구.
개발 과정에서 다른 라이브러리와 충돌하는 것을 방지하기 위해 격리된 환경이 필요할 때, 완성된 서비스를 배포할 때, 혹은 배포 중인 서비스를 받아서 실행해 볼 때도 유용하다. 
특히 배포 과정에서 도커를 사용해 필요한 파일들만 예쁘게 포장해서 이미지로 만들면 지긋지긋한 종속성 이슈에서 벗어날 수 있다. 
서버 한 대에만 배포한다면 종속성은 큰 문제가 되지 않겠지만, 서버가 2대, 4대, 8대... 수백, 수천 대까지 늘어난다면  _파일 받고, 필요한 라이브러리 설치하고, 앗! 이거 먼저 설치해야 하는데..._ 도커를 사용하면 그냥 같은 이미지를 실행해서 컨테이너로 만들면 된다. 

도커는 일반적으로 도커 엔진(Docker Engine) 혹은 도커에 관련된 모든 프로젝트를 말합니다.

> 도커 엔진(Docker Engine)
> 컨테이너를 생성하고 관리하는 주체로서 이 자체로도 컨테이너를 제어할 수 있고 다양한 기능을 제공하는 도커의 프로젝트입니다. 
> 도커의 생태계에 있는 여러 프로젝트들은 도커 엔진을 좀 더 효율적으로 사용하기 위한 것에 불과하기 때문에 도커의 핵심은 도커 엔진이라고 할 수 있습니다.

> 도커
> docker는 기본적으로 linux 위에서만 돌아간다.
Winodws와 MacOS 용 docker를 설치하면 경량화된 linux 머신이 가상화되어 구동되고, 그 위에서 docker가 구동되는 것이다.
Kernel은 Host OS를 그대로 사용하되, Host OS와 Container의 OS의 다른 부분만 Container 내에 같이 Packing된다


![[Docker11.png]]






![[Docker1.png]]



컨테이너에는 라이브러리, 시스템 도구, 코드, 런타임 등 소프트웨어를 실행하는데 필요한 모든 것이 포함되어 있다.

가상 머신에 비해 꼭 필요한 것만 담겨서 구동되기 때문에 이미지를 만들 경우 용량이 대폭 줄어들게 된다.

>도커를 왜 사용할까?
>1. 애플리케이션 독립성을 가진다. 호스트 OS, 다른 컨테이너와도 독립된 공간을 보장받아 충돌이 발생하지 않는다.
>
>2. 컨테이너 내부에 작업 후 배포하려 한다면 도커 이미지로 만들어서 운영서버에 전달만 하면 된다.
>
>3. 마이크로 서비스 구조로 변화가 쉽다. 컨테이너 하나당 하나의 기능을 제공하는 모듈로 만드는 등 조정이 가능하다.


다시 말해, Docker을 사용하면 환경에 구애받지 않고 애플리케이션을 신속하게 배포, 확장할 수 있다.


### 가상화와 컨테이너


가상화와 컨테이너에 대해 조금 더 이야기해 볼까요? 앞서 가상화는 하나의 하드웨어를 여러 개의 가상 머신으로 분할해 효율적으로 사용할 수 있는 기술이라고 언급했는데요, 분할된 가상 머신들은 각각 독립적인 환경으로 구동됩니다. 이 때 베이스가 되는 기존의 환경을 `Host OS`, 그리고 가상 머신으로 분할된 각각의 환경을 `Guest OS`라고 부릅니다.
![[Docker3.png]]


![[Docker4.png]]


![[Docker6.png]]



![[Docker7.png]]




가상 머신을 생성하기 위해서는 하이퍼바이저 또는 가상 머신 모니터라고 불리는 소프트웨어를 이용합니다. 하이퍼바이저는 호스트 하드웨어에 설치되어 호스트와 게스트를 나누는 역할을 하고, 각각의 게스트는 하이퍼바이저에 의해 관리되며 시스템 자원을 할당받게 됩니다. 이 때 하이퍼바이저에 의해 생성된 게스트는 호스트나 다른 게스트와 상호 간섭하지 않고 완전히 분리된 환경에서 구동됩니다. 하이퍼바이저를 활용하면 마치 하드웨어가 여러 개인 것처럼 하나의 서버를 여러 명이 나눠 쓸 수도 있고, 컴퓨터 한 대에서 서로 다른 OS를 동시에 사용할 수도 있습니다.

하지만 가상 머신으로 무언가 하려면 반드시 하이퍼바이저를 거쳐야 하기 때문에 속도 저하가 필연적입니다. 또 가상 머신은 해당 환경을 구동하는 데 필요한 파일을 모두 포함하고 있기 때문에, 가상 머신을 배포할 때 만들어지는 이미지의 크기가 매우 커진다는 한계점이 있습니다.

  
하이퍼바이저와 달리 컨테이너는 가상의 OS를 만드는 것은 아닙니다. 컨테이너는 베이스 환경의 OS를 공유하면서 필요한 프로세스만 격리하는 방식으로, 커널을 공유하기 때문에 호스트 OS의 기능을 모두 사용할 수 있습니다. 그렇기 때문에 ==컨테이너 위에서는 호스트 OS와 다른 OS를 구동할 수 없습니다==. 대신 격리시킬 애플리케이션과 거기에 필요한 파일이나 특정 라이브러리 등 종속 항목만 포함하기 때문에 배포를 위해 생성되는 이미지의 용량이 작아진다는 장점이 있습니다. 운영체제가 아닌 프로세스이며, 하이퍼바이저를 거칠 필요가 없어 실행 속도가 빠르기도 하고요.

> 이미지는 가상 머신이나 컨테이너 또는 프로그램을 실행하는 데 필요한 파일과 라이브러리, 설정 등을 가지고 있는 파일입니다. 이미지는 레이어라는 계층 구조로 이루어져 있는데, 변경 사항이 생기면 새로운 레이어를 추가해서 기록합니다. 이미지 전체를 새로 받지 않고 해당 레이어만 받는 것으로 이미지를 업데이트할 수 있다는 장점이 있지요.  
>   
> 이미지를 실행하면 프로세스, 즉 컨테이너가 됩니다.



## 하이퍼바이저 가상화(Hypervisor Virtualization) == 가상화

하이퍼바이저 가상화 기술의 도입은 앞서 설명한 **`하나의 컴퓨터에서 하나의 OS만 운영`** 하는 방식의 비효율을 해결한다. 하나의 컴퓨터에서 다수의 독립적인 OS를 운영한다. **`하이퍼바이저`**는 하나의 컴퓨터에서 여러 OS를 동시 실행하기 위한 소프트웨어를 뜻한다. 하이퍼바이저 가상화는 아래와 같이 이루어져 있다.

1. 하나의 물리적 서버 위에 존재하는 Host OS
2. 그 위에 존재하는 다수의 독립적 OS들

이렇듯 하나의 물리적 서버 위 Host OS가 존재하고, 그 위엔 다수의 독립적 OS가 가상(virtually)으로 돌아갈 수 있도록 하는 것이 **`하이퍼바이저 가상화`** 기술이다. 각각의 OS는 서로에 대해 알지 못하며 Host OS조차 알지 못한다. 하나의 물리적 서버에서 실행되고는 있지만 완전히 독립적인 OS로 운영되는 것이다. 즉, 하나의 물리적 서버 리소스를 각각의 OS에 할당하여 효율적으로 사용할 수 있다.



**- 가상머신은 Hypervisor를 통해 여러개의 운영체제를 생성되고 관리됨. (Guest OS)**

**- 시스템 자원을 가상화하고 독립된 공간을 생성하는 작업은 HyperVisor를 거치므로** -> `성능 손실이 큼 ↑`

**- 가상머신은 Guest OS를 사용하기 위한 라이브러리, 커널 등을 포함하므로** -> `배포할 때 용량이 큼 ↑`

#### 가상화 기술의 특징 및 장점

위와 같이 가상화 기술은 자원을 처리하는 풀을 생성하여 **가상화된 CPU, 메모리, 스토리지, 네트워크 인터페이스 등의 할당된 자원을 각 가상 머신에 제공하고, 실제 자원에 대한 VM 자원의 일정을 관리**합니다. 호스트 시스템의 하드웨어는 요청을 받아서 실행 작업을 수행하는데, 하이퍼바이저가 VM 의 작업 일정을 관리하면서 VM 의 작업을 요청하면 CPU 가 VM 의 요청을 받아서 CPU 명령을 실행합니다.

하이퍼바이저를 사용하여 **가상 머신을 실행시키면 여러개의 다른 OS 를 독립적으로 하나의 호스트 OS 에서 실행**할 수 있고, **같은 가상화 하드웨어 자원과 하이퍼바이저를 공유하여 사용**할 수 있습니다. 이 것이 가상화의 가장 큰 장점입니다.

#### 가상화의 단점

하지만 **이렇게 많은 이점을 가진 '가상화' 기술도 단점**이 있습니다.

- 프로세스 단위로 격리하고 싶은 경우, OS level조차도 **가상화해야 할 파트가 너무 많아 cost가 크다**고 느껴질 때가 많습니다. 
- 또한 VM 마다 **고유한 OS(게스트 OS)가 필요**하기 때문에, **각각 CPU, RAM 및 기타 리소스 등 별도의 자원를 소비하며 보다 많은 시간과 자원을 낭비**하게 됩니다.
- 위와 연장선으로 **완전한 운영체제가 올라가기 때문에, 실행 자체만으로도 대량의 메모리가 필요**합니다. 프로세스의 경우 전환이 매우 빠르고 애플리케이션들이 대부분의 시간을 sleep상태로 있기에, 여러 개의 VM들이 CPU를 경쟁해도 크게 문제가 되지 않습니다. 하지만 메모리는 CPU 만큼 빠르게 전환(자원 회수) 할 수가 없기에 대량의 메모리를 가지고 있어야 합니다. **가상화를 할 때, 적정 메모리를 산정하는 것은 상당히 까다로운 문제**입니다.
- 추가로, **특정 OS는 라이센스가 필요한 경우**도 있습니다.







## **컨테이너 가상화(Container Virtualization)** == 컨테이너

기존의 가상화 방식은 위에서 설명했듯 OS를 가상화하는 방식이었다. VMware, VirtualBox와 같은 대표적인 가상화 서비스도 이와 마찬가지로 Host OS위 다수의 Guest OS를 가상화하여 사용하는 방식이다. 하지만 이렇게 여러 Guest OS를 가상화하여 사용하는 방식은 사용법이 간단하지만 기술적으로 무거워지는(heavy-weight) 단점이 있다. 각각의 독립적인 OS를 실행시켜야 하기 때문에 부팅 시간이 길며 리소스 또한 많이 차지할 수밖에 없다. 따라서 실제 운영 환경에서는 사용이 어렵다.

이렇듯 추가적인 Guest OS를 설치하여 가상화하는 방식은 성능면에서 이슈가 있다. 이를 개선하기 위해 `프로세스를 격리된 환경에서 실행`하는 기술, 즉 `컨테이너 가상화 기술` 이 등장했다.

리눅스에서는 이러한 방식을 `리눅스 컨테이너`라고 한다. OS를 가상화하는 것이 아닌, 운영체제 수준의 가상화 기술로 리눅스 커널을 공유함과 동시에 프로세스를 격리된 환경에서 실행한다. 때문에 `하이퍼바이저 가상화 방식보다 더욱 가볍고 빠르게 동작`한다.

`컨테이너 가상화`를 통해 하나의 서버에서 다수의 컨테이너를 실행하면 컨테이너끼리 서로 영향을 끼치지 않음과 동시에 독립적으로 실행된다. 실행중인 컨테이너에 접속하여 명령어 입력하거나 패키지를 설치하는 등 다양한 작업이 가능하다. 또한 CPU 혹은 메모리를 제한할 수 있으며 호스트 디렉토리에 마운트하여 내부 디렉토리로 사용할 수도 있습니다.

새로운 컨테이너를 만드는 데 들어가는 시간은 OS를 가상화하는 방식엔 비교할 수 없을 정도로 빠르다. 그리고 하나의 프로세스(컨테이너)를 실행하는데 필요한 모든 파일을 이미지로 만들어 제공한다. 때문에 컨테이너 가상화 방식을 사용하면 개발 단계부터 테스트 및 프로덕션에 이르기까지 일관된 환경을 유지할 수 있다.


#### 컨테이너 기술의 특징 및 장점

컨테이너 모델에서 **컨테이너**는 VM과 유사합니다. 주요 차이점은 컨테이너에 **자체적인 하나의 완전한 OS****full-blown OS** **가 필요하지 않다는 점**입니다. 실제로 컨테이너 모델에서는 단일 호스트의 모든 컨테이너는 **호스트의 OS를 공유**합니다. 이것은 **CPU, RAM 및 스토리지와 같은 막대한 양의 시스템 리소스가 확보**된다는 의미를 가집니다. 또한 **라이센스 비용을 절감**할 수 있고 **OS 패치 및 유지보수의 오버헤드를 줄여주며 VM의 문제를 해결**할 수 있습니다. 이로 인해 **VM에서 컨테이너를 사용한 후 시간이나 리소스 및 네트워크를 절감**할 수 있습니다.

추가로 가상 머신의 경우 부팅 준비하는 것만 해도 상당한 시간이 걸리지만, 컨테이너는 **애플리케이션 구동을 위한 패키지(이미지)만 있으면 구동할 수 있습니다**. 이로 인하여 **컨테이너 워크로드를 노트북에서 클라우드로, 혹은 베어메탈 서버로 쉽게 이동할 수 있습니다.** 또한 컨테이너 구동 자체도 일반 프로세스를 실행하는 것과 차이가 없으며, **ms 단위로 보다 가볍고 빠르게 실행**할 수 있습니다. 

마지막으로 가장 눈에 띄는 컨테이너의 장점은 **실행시 디스크 공간 점유도**입니다. 가상머신은 완전한 운영체제를 포함하기에 우분투 서버리눅스의 경우 설치만으로도 1G가 넘는 디스크 공간을 사용합니다. 컨테이너는 **이미지로 부터 Copy on Write(CoW) 방식으로 만들어지기에, 그 자체로 디스크 공간을 점유하지 않습니다.**


#### 컨테이너 기술의 단점

**이렇게 많은 이점을 가진 '컨테이너' 기술도 단점**이 있습니다.

- 자원의 격리와 제한이 어렵다. cgroup와 namespace와 같은 기술들이 발전 하고 있기는 하지만, 가상머신에 비해서는 부족한 점이 있다.
- 컨테이너는 시스템과 격리된 하나 이상의 프로세스 들의 집합입니다. 컨테이너는 프로세스들에 대해 이미 지정된 자원들에 대해서만 요청을 허용하는데, 이러한 자원 제한으로 해당 컨테이너가 충분한 용량을 가진 노드에서 실행 가능하다는 것을 보장합니다.
- 컨테이너는 하나의 운영 체제만 구동할 수 있습니다(리눅스 서버를 구동 중인 컨테이너는 리눅스 운영체제만 구동할 수 있습니다).
- 스토리지 성능이 좋지 않다. 가상머신은 블럭 디바이스 위에 ext3, ext4를 이용해서 스토리지를 사용한다. 컨테이너는 AUFS, Device mapper, Overlay Filesystem등과 같은 유니온 파일 시스템(Union filesystem)을 사용하는데, ext4와 같은 파일 시스템 위에 올라가는 데다가 변경된 내용을 기록하고, 기록된 내용으로 부터 원본 데이터를 복원해야 하기 때문에 느릴 수 밖에 없다. 데이터의 읽기와 쓰기를 일반 파일 시스템으로 분리하거나 Btrfs나 ZFS 같은 COW파일 시스템을 이용하는 등으로 문제를 해결 할 수 있기는 하지만, 가상머신에 비해서 신경써야 할게 많다.
- 네트워크 구성이 어렵다. 가상머신은 물리적인 컴퓨터 시스템과 차이가 없어서 네트워크 구성이 특별히 어렵지 않다. 그냥 가상머신 마다 IP 주소 붙여서 연결하면 된다. 하지만 컨테이너는 그렇게 하기 힘들다. 그런 방식을 그냥 사용하면 프로세스에 IP를 주는 것이 되며, 게다가 컨테이너는 대량으로 만들 수 있기 때문에 IP 주소를 붙이는 것 자체가 낭비다. 호스트 운영체제 레벨에서 네트워크를 한번 더 추상화 해야 해서 가상머신 보다 네트워크 구성에 신경을 써야 할게 많다.


그렇기 때문에 **단일 OS 커널에서 여러 애플리케이션을 실행**하려는 경우에는 **컨테이너가 유리**하고, **서로 다른 OS 환경에서 애플리케이션을 실행해야 하는 경우에는 VM 이 필요합니다**. 이렇게 **컨테이너 기술**을 구현한 것 중 가장 널리 쓰는 오픈소스는 '도커'이며, 이 기술의 토대가 되었던 Linux Container와 비교


 **도커 컨테이너는 가상화된 공간을 생성할 때 리눅스 자체 기능을 사용하여 프로세스 단위의 격리 환경을 만드므로** -> `성능 손실 없음 無`
프로세스를 격리된 환경에서 실행한다는 것은 각 프로세스가 독립적인 실행 환경을 가지고 다른 프로세스로부터 격리되어 동작한다는 의미입니다. 이는 보안, 안정성, 성능 등 다양한 측면에서 이점을 제공합니다.

**- 가상머신과 달리 커널을 공유해서 사용하므로, 컨테이너에는 라이브러리 및 실행파일만 있으므로** -> `용량이 작음 ↓`

**- 위의 이유로 -> 컨테이너를 이미지로 만들었을 때**

 `배포하는 시간이 가상 머신에 비해 빠르며 ↑, 사용할 때의 성능 손실 또한 거의 없음 ↓`



![[Docker5.png]]




## 도커 이미지, 컨테이너란?


_도커 이미지란? :_

**Docker Image**란 컨테이너를 실행할 수 있는 실행파일, 설정 값들을 가지고 있는 것으로,

더 이상 의존성 파일을 컴파일하거나 이것저것 설치할 필요가 없는 상태의 파일을 의미한다.

Image를 컨테이너에 담고 실행시키면 해당 프로세스가 동작한다.

도커 이미지는 소스 코드, 라이브러리, 종속성, 도구 및 응용 프로그램을 실행하는데 필요한 기타 파일을 포함하는 불변(변경 불가) 파일이다.

1) 도커 이미지의 용량은 보통 수백MB ~ 수GB가 넘는다. 하지만 가상머신의 이미지에 비하면 굉장히 적은 용량이다.
2) 이미지는 상태 값을 가지지 않고 변하지 않는다(Immutable).

3) 하나의 이미지는 여러 컨테이너를 생성할 수 있고, 컨테이너가 삭제되더라도 이미지는 변하지 않고 그대로 남아 있음.

4) 도커 이미지들은 github와 유사한 서비스인 DockerHub를 통해 버전 관리 및 배포(push&pull)가 가능하다.

5) 다양한 API가 제공되어 원하는 만큼 자동화가 가능하다.

6) 도커는 Dockerfile이라는 파일로 이미지를 만든다. Dockerfile에는 소스와 함께 의존성 패키지 등 사용했던 설정 파일을 버전 관리하기 쉽도록 명시되어진다.(그래서 누구나 이미지 생성과정을 확인할 수 있으며 수정도 할 수 있다)


> **이미지와 레이어(Layer)**  

레이어란 기존 이미지에 추가적인 파일이 필요할 때 다시 다운로드받는 방법이 아닌 해당 파일을 추가하기 위한 개념이다. 이미지는 여러 개의 읽기 전용(read only) layer로 구성되고, 파일이 추가되면 새로운 Layer가 생성됨. 그리고 도커는 여러 개의 Layer를 묶어서 하나의 파일시스템으로 사용할 수 있게 해준다. 그래서 이미지와 레이어는 같은 의미로도 사용된다. 추가적으로 DockerHub 및 개인 저장소에서 이미지를 공유할 때는 바뀐 부분(Layer = image)만 주고받기 가능하다.


_도커 이미지의 생성 방식:_

도커 이미지는 기존 이미지에 추가적인 구성이 필요할 때 다시 다운로드하는 방법이 아닌

기존이미지에 레이어를 추가하여 구성을 올려주는 방식으로 생성된다.

다시 말해, 이미지는 여러 개의 읽기 전용 레이어로 구성되고 파일 추가되면 새로운 레이어가 생성되어 추가된다.

도커는 여러 개의 레이어를 묶어 하나의 파일시스템으로 사용할 수 있게 해 준다.

_이미지와 컨테이너의 차이 :_


![[Docker2.png]]


간단하게 설명하면 도커 이미지는 설계서, 컨테이너는 설계서로 만들어진 상품이다.

게다가 이미지가 중간에 바꾸게 되더라도 기존 컨테이너는 더 이상 이미지에 영향을 받지 않는다.

_컨테이너의 정의 및 특징 :_

다시 말해 **Docker container**란 이미지를 실행한 상태로 응용프로그램의 종속성과 함께 응용프로그램 자체를 

패키징 Or 캡슐화하여 격리된 공간에서 프로세스를 동작시키는 기술이다.

  _특징_

- 컨테이너는 이미지 레이어에 읽기/쓰기 레이어를 추가하는 것으로 생성됨
- 종료되었다고 해도 삭제되지 않음 -> 읽기/쓰기 레이어 보존
- 컨테이너를 삭제한 것은 생성파일이 사라지는 것
- 한 서버는 여러 개의 컨테이너를 가져도 상관없으며 독립적으로 실행
- 컨테이너는 커널 공간과 호스트 os자원을 공유

>도커 엔진에서 사용하는 기본 단위는 이미지와 컨테이너이며 도커 엔진의 핵심입니다.
도커 이미지와 컨테이너는 `1:N` 관계입니다.
도커 이미지와 컨테이너의 관계는 운영체제에서의 프로그램 <-> 프로세스, 객체지향 프로그래밍에서의 
클래스 <-> 인스턴스의 관계와 비슷하다고 생각하면 이해가 더 편합니다.


`Docker File -> Docker Image`: Docker File은 도커 이미지를 만들때 사용하는 파일. **docker build 명령어를 실행**시키면 도커 이미지를 만들 수 있습니다. 

`Docker Image -> Docker Container`: Docker Image를 **docker run 명령어를 실행**시키면 Docker Container를 만들 수 있습니다.

### 도커 이미지

`도커 이미지(Docker Image)`는 컨테이너를 생성할 때 필요한 요소이며, 가상 머신을 생성할 때 사용하는 iso 파일과 비슷한 개념입니다.

이미지는 컨테이너를 생성하고 실행할 때 읽기 전용으로 사용되며 여러 계층으로 된 바이너리 파일로 존재합니다.

도커에서 사용하는 이미지의 이름은 기본적으로 아래의 형태로 구성됩니다.


```bash
[저장소 이름]/[이미지 이름]:[태그]
```

- `저장소 이름`: 이미지가 저장된 장소. 저장소 이름이 명시되지 않은 이미지는 도커 허브의 공식 이미지를 뜻함.

- `이미지 이름`: 해당 이미지가 어떤 역할을 하는지 나타내며 필수로 설정해야함. ex) ubuntu:latest -> 우분투 컨테이너를 생성하기 위한 이미지라는 것을 나타냄.

- `태그`: 이미지의 버전을 나타냄. 태그를 생략하면 도커 엔진은 latest로 인식함.

### 도커 컨테이너

`도커 컨테이너(Docker Container)`는 도커 이미지로 생성할 수 있으며, 컨테이너를 생성하면 해당 이미지의 목적에 맞는 파일이 들어 있는, 호스트와 다른 컨테이너로부터 격리된 시스템 자원 및 네트워크를 사용할 수 있는 독립된 공간(프로세스)이 생성됩니다.

대부분의 도커 컨테이너는 생성될 때 사용된 도커 이미지의 종류에 따라 알맞은 설정과 파일을 가지고 있기 때문에 도커 이미지의 목적에 맞도록 사용되는 것이 일반적입니다.

ex) 웹 서버 도커 이미지로부터 여러개의 도커 컨테이너를 생성하면 생성된 컨테이너의 개수만큼 웹 서버가 생성되고, 이 컨테이너들은 외부에 웹 서비스를 제공하는 데에 사용됩니다.

컨테이너는 이미지를 읽기 전용으로 사용하되 이미지에서 변경된 사항만 컨테이너 계층에 저장하므로 컨테이너에서 무엇을 하든지 원래 이미지는 영향을 받지 않습니다. 또한 생성된 각 컨테이너는 각기 독립된 파일시스템을 제공받으며 호스트와 분리돼 있으므로 특정 컨테이너에서 어떤 어플리케이션을 설치하거나 삭제해도 다른 컨테이너와 호스트는 변화가 없습니다.

ex) 같은 도커 이미지로 A, B 두 개의 컨테이너를 생성한 뒤에 A 컨테이너를 수정해도 B 컨테이너에는 영향을 주지 않습니


### Linux Container(이하 LXC)


![[Docker8.png]]

#### LXC의 특성

**LXC**는 단일 컨트롤 호스트 상에서 여러개의 고립된 리눅스 시스템(컨테이너)들을 실행하기 위한 **운영 시스템 레벨 가상화 방법**입니다. 이러한 컨테이너는 daemon에게 Linux kernel에 존재하는 컨테이너의 기본 building block에 대한 **namespaces**나 **cgroups**(control groups)와 같은 접근을 제공했습니다.

- namespace는 간단히 말하자면, 운영 시스템을 쪼개서 각각 고립된 상태로 운영이 되는 개념을 의미해요. 
- cgroups는 간단히 말하자면, namespaces으로 고립된 환경에서 사용할 자원을 제한하는 역할 등을 하는 개념입니다.

이 부분은 Building Block을 이해한 후 다시 보면 이해하는데 도움이 되실거에요.

어쨌든, 분리된 후 고립된 각각의 환경들이 만들어진다는 것을 확인할 수 있는데요. container의 개념을 알고 있다면, 바로 이 기술이 container 기술의 근간이라는 것을 예상할 수 있습니다.

VM처럼 OS level에서 격리하는 것이 아닌 프로세스 단위로 격리합니다. 이러한 컨테이너는 리눅스 커널 기술을 기반으로 구성되었습니다. 이에 대해 간단하게 말하자면 namespace을 통해 가상의 독립된 환경을 만들고 cgroups를 이용하여 각 프로세스와 쓰레드를 그룹으로 나누어 통제함으로써 이전보다 higher level에서 isolation을 하였습니다. 정리하자면 각 namespace가 그에 속한 process들은 하나의 machine처럼 작동합니다. 이 기술을 접목한 **LXC (linux containers)** 가 나타났으며, **초기 버전의 도커는 LXC를 그대로 사용**했습니다.

#### 2.1.2 LXC의 문제점

LXC는 **리눅스에 특화되어 있는데**, Docker가 Multi-Platform을 목표로 하는데 큰 리스크였습니다. 또한 시스템을 구성하는 핵심적인 요소가 외부 시스템에 의존한다는 문제도 있었습니다. 때문에 Docker사는 LXC를 대체하기 위해 libcontainer라는 독자적인 툴을 개발했으며, go 언어로 작성되어 platform-agnostic(불특정 플랫폼) 툴로 만들어주었습니다. 그래서 Docker 0.9부터 기본 실행 드라이버를 **LXC에서 libcontainer로 대체**했습니다.

#### 2.1.3 Libcontainer

![[Docker9.png]]


**Libcontainer**는 현재의 Docker Engine에서 사용하고 있는 주요 컴포넌트입니다. Go 언어로 만들어져서 Container를 생성 시 namespaces, cgroups, capabilities 를 제공하고, filesystem의 접근을 제한할 수 있습니다. 컨테이너가 생성된 후 작업을 수행할 수 있도록 컨테이너의 수명 주기를 관리할 수 있습니다.

위 그림을 보면 알 수 있듯이, Libcontainer는 LXC와는 다르게 Docker 내부에서 실행되고 있는 것을 확인할 수 있습니다. Docker에서는 자체적으로 제작한 libcontainer의 CLI wrapper인 **runc**를 사용합니다.

자, 그럼 이제 본격적으로 컨테이너의 구조를 알아볼게요. 도커 내부를 알아가기 전에 도커 용어를 확실히 하고 넘어가야 할 것 같아요. 도커의 구조를 살피며 자주 사용할 단어들을 한번씩 짚고 가겠습니다.

###  Docker(이하 도커)

**“도커는 리눅스 컨테이너 시스템인 runC를 이용해 하나의 운영체제 위에서 격리된 컴퓨팅 환경을 운영할 수 있도록 도와주는 애플리케이션 레벨 가상화 소프트웨어”** ...였지만 위에서 설명했듯이 Windows 또한 NT Kernel 상에서 isolation을 통해 windows(Host) → windows(Base) 컨테이너를 생성하므로 위는 outdated된 정의입니다.

- 컨테이너 가상화는 게스트 운영체제 없이 **호스트 운영체제 위에 격리된 환경에서 가상화를 구현**함
- 게스트 운영체제가 따로 존재하지 않으니 호스트 운영체제의 요소를 공유하며 그만큼 중복되는 요소가 줄어 성능적 이점을 누릴 수 있음
- 컨테이너는 운영 체제의 동작을 완전히 재현하지는 못하기 때문에, 좀 더 엄밀한 운영 체제의 동작이 요구되는 가상 환경을 구축해야 한다면 VMWare나 VirtualBox가 나음

###  도커가 기존 Linux container(이하 LXC)에 비해 가진 장점

- 호스트 운영 체제의 영향을 받지 않는 실행 환경
    - Docker Engine을 이용한 실행 환경 표준화
    - LXC를 사용할 땐 LXC의 설정 차이로 LXC에서 만든 application이 다른 LXC에서 안 돌아가는 문제가 발생하곤 했음
- DSL(Domain Specific Language)를 이용한 Dockerfile
- 이미지 버전 관리
- 레이어 구조를 갖는 이미지 포맷(차분 빌드가 가능함)
- 도커 레지스트리(이미지 저장 서버 역할)

###  (현재)도커의 한계: Kernel 및 Architecture의 차이
- 호스트형 가상화 기술처럼 하드웨어를 연산으로 에뮬레이션하는 기술과 달리, 도커에서 사용되는 컨테이너형 가상화 기술은 호스트 운영 체제와 커널 리소스를 공유한다. 이는 사실상 도커 컨테이너를 실행하려면 호스트가 특정 CPU 아키텍처 혹은 운영 체제를 사용해야 한다는 의미. 라즈베리 파이와 같은 ARM 계열의 arm7I 아키텍처를 채용한 플랫폼에서는 인텔의 x86_64 아키텍처에서 빌드한 도커 컨테이너를 실행할 수 없다.
- microsoft의 windowsservercore 이미지는 리눅스나 macOS등의 플랫폼에서는 실행할 수 없다.

### “Container Host”와 “Container OS”의 차이

#### Container Host (Host OS)

- Docker client와 Docker daemon이 실행되고 있는 곳의 OS
- linux와 non-Hyper-V container들은 Host OS가 Docker container에게 kernel을 나누어주지만, Hyper-V container는 각자의 Hyper-V kernel을 가진다.

#### Container OS (Base OS)

- Ubuntu, CentOS, windowsservercore와 같이 컨테이너의 OS
- Base OS를 담은 이미지 위로 쌓이는 이미지는 Base OS를 utilize함.

쭉 설명했듯이 도커는 기본적으로 Linux kernel을 이용하여 개발되었다. Docker for Windows/Mac이 linux container를 만들 때는 LinuxKit(linux vm)을 이용한다. → Linux containers are running on Linux, and Windows containers are running on Windows. → 그랬는데 이게 또 windows에선 LCOW가 기본이 되면서 바뀌었다.

- The setup for running Linux containers with LCOW is a lot simpler than the previous architecture where a Hyper-V Linux VM runs a Linux Docker daemon, along with all your containers. With LCOW, the Docker daemon runs as a Windows process (same as when running Docker Windows containers), and every time you start a Linux container Docker launches a minimal Hyper-V hypervisor running a VM with a Linux kernel, runc and the container processes running on top.
- **이렇게하면 Docker Daemon이 windows(Host OS) 위에서 돌아도 Linux Container를 만들 수 있어서 Windows와 Linux 컨테이너를 동시에 실행해줄 수 있다.**
- windows container가 windows에서 생성되는 과정이나, docker 자체 기술이나, 현재 진행형으로 새로 개발되는 것들이 있는 것 같다.


### 도커 동작원리

도커 컨테이너는 호스트 OS 의 커널을 공유한다. 각 컨테이너에 리눅스 커널을 할당하여 프로세스를 실행시키고 각 컨테이너 간의 격리를 구현한다. 이때 리눅스 커널의 Cgroup 과 네임스페이스 기능을 이용하여 독립된 공간을 구현하도록 한다. 이를 통해서 서로 다른 프로세스, 컨테이너 사이에 벽을 만든다.  
  
호스트 시스템과 컨테이너가 하나의 커널을 공유하기 떄문에 호스트 시스템에서 컨테이너 내부의 프로세스를 볼 수 있다. 컨테이너 내부에서 프로세스를 실행하고 호스트 시스템에서 ps 명령어를 통해 실행중인 프로세스를 조회하면 컨테이너 내부에서 실행 중인 프로세스를 확인할 수 있다.  
  
도커는 도커 엔진을 통해서 실행되고 관리된다. 도커는 리눅스 커널을 사용하기 때문에 호스트 시스템이 리눅스거나 리눅스 커널을 사용할 수 있는 OS 여야 한다. 도커 엔진은 또 하나의 VM 으로 리눅스를 게스트 OS 로 가지고 있다. 이 게스트 OS 인 리눅스의 커널을 컨테이너에 할당하여서 컨테이너가 실행되고, 이 커널을 통해서 각 컨테이너들이 격리된다. 그렇기 떄문에 호스트 OS 가 리눅스가 아니어도 도커를 사용할 수 있다.

### 3.2 도커 엔진(Docker Engine)

이제 도커 컨테이너를 구축하고 실행하는 핵심 SW인 도커 엔진에 대해 이해해보도록 하겠습니다.


![[Docker10.png]]


Docker Engine은 Docker Daemon, REST API, API를 통해 도커 데몬과 통신하는 CLI로 모듈식으로 구성되어 있습니다. 개발자들이 Docker라고 할 때, 주로 Docker engine을 의미합니다.

위 그림을 통해 구조를 살펴봅시다. 컨테이너를 빌드, 실행, 배포하는 등의 무거운 작업은 Docker Daemon이 하며, Docker Client는 이러한 로컬 혹은 원격의 Docker Daemon과 통신합니다. 통신을 할 때에는 UNIX socket(/var/run/docker.sock) 또는 네트워크 인터페이스를 통한 REST API를 사용합니다.

#### docker client (= Docker CLI): docker cli 명령을 dockerd에 요청

**docker cli 등에 입력한 명령어(e.g. docker run)를 적절한** **REST API payload로 변환해서 dockerd에 post 요청(e.g. POST /containers/create HTTP/1.1)**하게 됩니다. 또한 Docker Client는 다수의 데몬과 통신할 수도 있습니다.

이 때 **/var/run/docker.sock에 있는 유닉스 소켓을 통해 도커 데몬의 API를 호출**하게 되는데, 이 도커 데몬과 통신하기 위한 소켓에 연결할 수 없을 경우 'Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?'과 같은 에러가 발생하곤 합니다.

- Linux에서 socket은 /var/run/docker.sock 이고, Windows에서는 \pipe\docker_engine 입니다.

#### dockerd (docker deamon): 도커 서비스 관리
도커 데몬은 **Docker API 요청을 수신**하고, **다른 데몬과 통신하여 도커 서비스를 관리**할 수도 있습니다. dockerd의 역할로는 **도커 이미지의 관리, 이미지 빌드, REST API, 인증, 보안, 코어 네트워킹, 오케스트레이션 등 다양한 작업을 수행**합니다. 

또한 logging drivers, volume 및 volume drivers, network를 설정하는 등 컨테이너에 필요한 대부분의 설정을 지정합니다.

추가로 도커 프로젝트가 커지면서 이 dockerd는 더이상 직접 컨테이너를 실행시키지 않으며, **컨테이너의 실행과 런타임 코드를 모듈화하여 분리(containerd)**하였습니다. 대신에 만약 dockerd가 client로부터 ‘새로운 container를 생성하라’는 명령을 수신하면 containerd를 호출합니다. 이때, **dockerd는 CRUD 스타일 API를 통해 gRPC로 containerd와 통신**합니다.

#### containerd

현재는 docker에서 분리되어 오픈소스로 운영되고 있는 컨테이너 런타임 코드입니다. containerd는 실제로 containers를 생성하지 못하고 runc를 통해 생성하지만, Docker 이미지를 가져와서 컨테이너 구성을 적용하여 runc가 실행할 수 있는 OCI 번들로 변환합니다. containerd가 실질적으로 컨테이너를 관장하게 되는데, 쿠버네티스에서 만든 Container Runtime Interface(CRI)를 구현합니다. 여기서 컨테이너의 생명 주기를 관장하는데, 이 때 runC를 사용합니다.

원래 containerd는 작고 가벼운 Container lifecycle operations으로 설계되었는데, 시간이 지나면서 image pulls, volumes and networks와 같은 기능들이 확장되었습니다.

- [github] containerd: [https://github.com/containerd](https://github.com/containerd)

#### # High-Level Runtime

containerd는 High-Level Runtime이며, High-Level Runtime은 보통 이미지 관리, gRPC/Web API와 같이 컨테이너를 관리하는 것 이상의 높은 수준의 기능을 지원하는 런타임을 의미합니다. "high-level container tools", "high-level container runtimes" 으로 부르거나, 가끔은 그냥 "container runtimes”이라고도 부릅니다. 대조적으로, Low-Level Runtime에는 다음으로 볼 runC가 있습니다. 

Docker Client로부터 Container 관련 요청은 dockerd를 거쳐 gRPC 통신을 통해 containerd 로 전달됩니다. 그리고 나서, containerd는 컨테이너의 관리를 위해 runc를 사용하는데요. 그럼, runc가 무엇인지 알아볼게요. 

#### runC: Container 생성

**runC는 *libcontainer용 CLI Wrapper로, 독립된 container runtime입니다. Docker에서 runc는 목적은 단 하나인데요, 바로 Container 생성입니다.  
*** libcontainer : Docker사가 multi-platform 서비스를 만들기 위해 go 언어로 작성된 패키지.

docker에서 컨테이너 기술을 개발하게 되면서 **리눅스의 namespace, control group들과 같은 기술들을 많이 사용**하게 되었는데, OS 커널에 접속해서 컨테이너를 만드는 데 필요한 구성 요소(네임스페이스, cgroup 등)을 **하나의 low-level component**로 묶고 **runC**라고 이름붙였다고 합니다. 따라서 **이는** **실제로 컨테이너를 만드는 기술들의 집합으로 runc로 새로운 container를 생성합니다.**

현재는 오픈소스로 기부되어 하나의 standalone 프로젝트로서 **플랫폼에 관계 없이 container를 구현하는 기술**들을 가지고 있습니다.

- [github] opencontainers/runC: [https://github.com/opencontainers/runc](https://github.com/opencontainers/runc)

#### # OCI(Open Container Initiative)

runC는 OCI container-runtime-spec의 구현체입니다. OCI는 kernel의 container 관련 기술을 다루는 interface를 표준화시킨 기준입니다. 그래서 runc가 동작하는 계층을 OCI Layer라고 부르기도 합니다.

#### # Low-Level Runtimes

Low-Level Runtimes는 보통 컨테이너를 운영하는 것에 초점을 맞춘 실제 컨테이너 런타임을 의미합니다.

runC는 독립된 컨테이너 런타임이기 때문에 바이너리로 다운받고 빌드할 수 있습니다. runC(OCI) container를 빌드하고 실행시키는데 모든 것을 갖출 수 있다는 의미입니다.

하지만 이것은 골격(bare bones)일 뿐이며 매우 낮은 레벨(low-level)입니다.즉, 완전한 Docker 엔진의 특성(full-blown)을 가질 수는 없습니다. 

#### 3.2.5 containerd-shim: 생성된 컨테이너의 생명주기 관여
컨테이너 프로세스는 runc의 하위 프로세스로 시작되는데(생성되는 모든 container 당 runc의 새로운 인스턴스를 fork ), 컨테이너 프로세스가 실행하자마자 runc가 종료(exit)됩니다. 이 시점부터 **containered-shim이 컨테이너의 새로운 부모 프로세스가 되어 생성된 컨테이너의 생명주기에 관여**하게 됩니다. 이는 containerd에게 컨테이너의 ***file descriptor(e.g. stdin/out)**와 **종료 상태를 관리**하는 데 필요한 최소한의 코드를 메모리에 남깁니다.

* **file descriptor**

- 프로세스에서 열린 파일의 목록을 관리하는 파일 테이블의 인덱스입니다. 프로그램이 프로세스로 메모리에서 실행될 때, 기본적으로 할당되는 파일디스크립터는 표준입력(Standard Input), 표준 출력(Standard Output), 표준에러(Standard Error)이며 이들에게 각각 0, 1, 2라는 정수(file table의 인덱스)가 할당됩니다.
- daemon이 재시작될 때, 파이프가 닫히는 등의 이유 때문에 container가 종료되지 않도록 **STDIN과 STDOUT 스트림을 열린 상태로 유지**합니다



## 도커 파일

Doker File은 이미지 생성 출발점으로 이미지를 구성하기 위한 명령어들을 작성하여 이미지를 구성할 수 있다.

다시 말해, 만들 이미지에 대한 정보를 기술한 템플릿이라고 보면 된다.

이를 빌드하면 자동으로 이미지가 생성된다. 그러므로 도커 파일을 통해 애플리케이션 빌드 및 배포를 자동화할 수 있다.

```java
FROM python:3.8.3-alpine
ENV PYTHONUNBUFFERED 1

RUN mkdir /app
WORKDIR /app

# dependencies for psycopg2-binary
RUN apk add --no-cache mariadb-connector-c-dev
RUN apk update && apk add python3 python3-dev mariadb-dev build-base && pip3 install mysqlclient && apk del python3-dev mariadb-dev build-base

RUN apk add linux-headers libffi-dev musl-dev
RUN apk add --no-cache python3-dev libffi-dev gcc

# By copying over requirements first, we make sure that Docker will cache
# our installed requirements rather than reinstall them on every build
COPY requirements.txt /app/requirements.txt
RUN pip install -r requirements.txt

# Now copy in our code, and run it
COPY . /app/
```


## 도커 구성요소


`Docker Client` -> 도커를 설치하면 그것이 Client며 build, pull, run 등의 도커 명령어를 수행합니다.

`DOCKER_HOST` -> 도커가 띄워져있는 서버를 의미합니다. DOCKER_HOST에서 컨테이너와 이미지를 관리하게 됩니다.

`Docker daemon` -> 도커 엔진입니다.

`Registry` -> 외부(remote) 이미지 저장소입니다. 다른 사람들이 공유한 이미지를 내부(local) 도커 호스트에 pull할 수 있습니다. 이렇게 가져온 이미지를 run하면 컨테이너가 됩니다.

- **public 저장소:** Docker Hub, QUAY 
- **private 저장소:** AWS ECR 혹은 Docker Registry를 직접 띄워서 비공개로 사용하는 방법등이 존재합니다.










---
참조 -  https://squirmm.tistory.com/entry/Docker-%EB%8F%84%EC%BB%A4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80#--%--%EB%-F%--%EC%BB%A-%--%EC%-D%B-%EB%AF%B-%EC%A-%--%-C%--%EC%BB%A-%ED%--%-C%EC%-D%B-%EB%--%--%--%EB%-E%--%-F


https://hoon93.tistory.com/48



https://csj000714.tistory.com/641


https://mosei.tistory.com/entry/Docker-Container%EC%9D%98-OS-vs-VM%EC%9D%98-OS
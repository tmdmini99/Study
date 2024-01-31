## 도커 컴포즈란?

`도커 컴포즈`는 단일 서버에서 여러개의 컨테이너를 하나의 서비스로 정의해 컨테이너의 묶음으로 관리할 수 있는 작업 환경을 제공하는 관리 도구입니다.
Docker Compose란 여러 컨테이너를 가지는 애플리케이션을 통합적으로 만들고, 각각의 컨테이너를 시작 및 중지하는 작업을 더 쉽게 수행할 수 있도록 도와주는 도구입니다.

![[Docker-Compose1.png]]


![[Docker-Compose2.png]]


### **계층 구조(3 Tier- Architecture) 란?**

어떠한 플랫폼을 3 계층으로 나누어 별도의 논리적/물리적인 장치에 구축 및 운영하는 형태


![[Docker-Compose3.png]]

보통 서버 한대에 여러 기능을 구축하는 것이 아니라 계층으로 나눠서 관리하게 된다고 합니다.

여기서는 데이터 계층, 로직 계층, 클라이 언 계층으로 나누고, 각각의 기능으로 별도의 논리적/물리적인 장치로 운영하는 방식을 사용한다고 합니다.

**프레젠테이션 계층 (Presentation Tier)**

사용자가 직접 마주하게 되는 계층이다. 따라서 주로 사용자 인터페이스(인터넷 브라우저 등)를 지원하며 이 계층은 GUI 또는 프론트엔드(front-end) 라고도 부른다. 그러므로 이 계층에서는 사용자 인터페이스와 관계없는 데이터를 처리하는 로직은 포함하지 않는다. 주로 웹 서버를 예시로 들 수 있고, HTML, Javascript, CSS 등이 이 계층에 해당 된다.

**어플리케이션 계층 (Application Tier)**

이 계층에서는 (프레젠테이션 계층) 요청되는 정보를 어떠한 규칙을 바탕으로 처리하고 가공하는 것들을 담당한다. (동적인 데이터 제공!) 비즈니스 로직 계층 또는 트랜잭션 계층 이라고도 한다. 첫 번째 계층에서 이 계층을 바라볼 때에는 서버처럼 동작하고(응답), 세 번째 계층의 프로그램에 대해서는 마치 클라이언트처럼 행동한다.(요청)

따라서 이 계층은 미들웨어(Middleware) 또는 백엔드(back-end)라고도 불린다. 이 계층에서는 프레젠테이션코드 (예를 들면 HTML, CSS)나 데이터 관리를 위한 코드는 포함하지 않는다. 주로 PHP, Java 등이 이 계층에 해당한다.

**데이터 계층 (Data Tier)**

데이터 계층은 데이터베이스와 데이터베이스에 접근하여 데이터를 읽거나 쓰는 것을 관리하는 것을 포함한다.

주로 DBMS (Database Management System)이 이 계층에 해당된다. 데이터 계층 또한 백엔드(back-end)라고도 부른다. 주로 MySQL, MongoDB 등이 이 계층에 해당된다.



이러한 구조가 2층도 있고 4층도 있고 다층 구조라고 부릅니다.(Multi-tier Architecture)

이런 것을 각각 docker container를 개별로 띄울 수 있지만, 이러면 서로 연결관계를 다 고려해 줘서 해줘야 하지만,

docker-compose를 구성하면 하나의 network에서 쉽게 관리할 수 있게 합니다.

Docker Compose를 구성하기 위해서는 YAML 파일을 사용하여 구성할 수 있습니다.


#### 계층 종류

![[Docker-Compose4.png]]

**1계층 구조**

하나의 물리적인 컴퓨터 또는 서버에 3가지의 다른 기능으로 함께 구현한 방식이다.

따라서 물리적인 장비를 새로운 장비로 변경하고자 하는 경우에는 모든 구성을 함께 변경해야 한다.



![[Docker-Compose5.png]]

**2계층 구조**

클라이언트 계층과 데이터 계층의 물리적인 컴퓨터 또는 서버로 구분하여 클라이언트 계층에서의 변경이나 데이터베이스의 변경 시 서로 영향을 받지 않는다.


![[Docker-Compose6.png]]


**3계층 구조**

클라이언트 계층, 어플리케이션 계층, 데이터 계층으로 서버를 모두 물리적으로 나누어 구성하는 방식이다.

이에 각각의 계층에서 변화가 일어나더라도 서로 영향을 받지 않고 독립적으로 운영된다.


**3계층 구조의 장점 /  단점**

장점

각 계층이 분리되어 있어 업무 분담이 가능해지므로 업무 효율성이 증가할 수 있다. 또한 여러 대의 서버로 나누어 각 계층이 동작하므로 서버의 부하를 줄여줄 수도 있으며, 경우에 따라 합리적인 스케일업(서버의 성능 업그레이드)이 가능하다.

단점

1계층으로만 사용하는 것 대비 관리가 더 필요하고, 장애가 발생하는 포인트가 더 늘어날 수 있다는 점을 생각해두어야 한다.
따라서 비용이 그만큼 많이 발생하게 되므로 서비스 규모 및 사용자 증가에 따라 계층 구조를 설계 및 고려해야 한다.









## 도커 컴포즈를 사용하는 이유

여러 개의 컨테이너가 하나의 어플리케이션으로 동작할 때 `도커 컴포즈`를 사용하지 않는다면, 이를 테스트하려면 각 컨테이너를 하나씩 생성해야 합니다. 예를 들면, 웹 어플리케이션을 테스트하려면 웹 서버 컨테이너, 데이터베이스 컨테이너 두 개의 컨테이너를 각각 생성해야 합니다.


```
$ docker run --name wordpress_db -d mysql:8

$ docker run -d -p 8080:80 \
--link wordpress_db:mysql --name seunghwan_wordpress \
wordpress:latest
```


위의 예제를 실행하면 wordpress와 mysql 컨테이너를 생성합니다. 이처럼 여러 개의 컨테이너로 구성된 어플리케이션을 구축하기 위해 run 명령어를 여러 번 사용할 수 있지만 각 컨테이너가 제대로 동작하는지 확인하는 테스트 단계에서는 이런식으로 일일히 여러개의 컨테이너를 실행하기는 번거롭습니다. 매번 run 명령어에 옵션을 설정해 CLI로 컨테이너를 생성하기보다는 여러 개의 컨테이너를 하나의 서비스로 정리해 컨테이너 묶음으로 관리할 수 있다면 좀 더 편리할 것입니다. 이를 위해 도커 컴포즈는 컨테이너를 이용한 서비스의 개발과 CI를 위해 여러 개의 컨테이너를 하나의 프로젝트로서 다룰 수 있는 작업 환경을 제공합니다.

`도커 컴포즈`는 여러 개의 컨테이너의 옵션과 환경을 정의한 파일을 읽어 컨테이너를 순차적으로 생성하는 방식으로 동작합니다. 도커 컴포즈의 설정 파일은 도커 엔진의 run 명령어의 옵션을 그대로 사용할 수 있으며, 각 컨테이너의 의존성, 네트워크, 볼륨 등을 함께 정의할 수 있습니다. 또한 스웜모드의 서비스와 유사하게 설정 파일에 정의된 서비스의 컨테이너 수를 유동적으로 조절할 수 있으며 컨테이너의 서비스 디스커버리도 자동으로 이뤄집니다. 그래서 컨테이너의 수가 많아지고 정의해야 할 옵션이 많아지고 정의해야 할 옵션이 많아진다면 도커 컴포즈를 사용하는 것이 좋습니다.


## 도커 컴포즈 설치

리눅스에서는 아래 명령어로 도커 컴포즈의 깃허브 저장소에서 현재 날짜 기준으로 최신 버전인 2.16버전을 설치할 수 있습니다.

다른 OS에서의 설치방법은 도커 컴포즈 도큐먼트에서 확인하실 수 있습니다.

```
$ curl -SL https://github.com/docker/compose/releases/download/v2.16.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
```


설치가 완료되었는지 확인하기 위해 아래 명령어로 도커 컴포즈의 버전을 확인해줍니다.

```
$ docker-compose -v
```





##  YAML

Docker Compose 파일은 다음과 같은 구성 요소로 이루어져 있습니다.

- Services: Services는 Docker Compose에서 실행할 컨테이너를 정의합니다. 각각의 서비스는 이미지 이름, 컨테이너 이름, 사용할 포트, 환경 변수 등의 정보를 포함합니다.
- Networks: Networks는 각 서비스 간의 네트워크 연결을 정의합니다. 각각의 네트워크는 이름과 드라이버 등의 정보를 포함합니다.
- Volumes: Volumes는 애플리케이션에서 사용하는 데이터 볼륨을 정의합니다. 각각의 볼륨은 이름, 드라이버, 옵션 등의 정보를 포함합니다.
- Environments: Environments는 서비스에서 사용할 환경 변수를 정의합니다.
- Configuration : Configurations는 서비스에서 사용할 환경 설정 파일을 정의합니다.
- Deployments : Deployments는 서비스를 배포하는 데 사용할 설정을 정의합니다.

Docker Compose를 사용하면 Docker CLI 명령어를 사용하여 애플리케이션을 실행하는 대신, docker-compose 명령어를 사용하여 애플리케이션을 실행할 수 있습니다. Docker Compose는 YAML 파일을 읽어 애플리케이션을 실행하며, 컨테이너의 로그를 모니터링하고 상태를 관리할 수 있습니다.

### Services

services에는 자기가 원하는 여러 도커 컨테이너의 서비스들을 작성하는 곳입니다.

여기서 사용하고자 하는 컨테이너들의 정의를 하시면 됩니다.

- image
    - image는 해당 서비스에서 사용할 도커 이미지 이름을 정의합니다. 이 이미지는 Docker Hub, Private Registry 또는 로컬에 저장되어 있을 수 있습니다.
    - image를 사용하면 해당 서비스는 이미지를 이미 가지고 있으므로 Dockerfile을 사용하여 이미지를 빌드할 필요가 없습니다. 이미지가 로컬 또는 리모트 레지스트리에서 가져올 수 있습니다.
- continer_name
    - container_name은 컨테이너의 이름을 정의합니다. 이름을 지정하지 않으면 Docker Compose는 자동으로 이름을 생성합니다.
- ports
    - ports는 컨테이너가 사용할 포트와 호스트에서 사용할 포트를 지정합니다.
- environment
    - environment는 서비스에서 사용할 환경 변수를 정의합니다.
- volumes
    - volumes는 컨테이너와 호스트 간의 데이터 공유를 위한 볼륨을 정의합니다.
- depends_on
    - depends_on은 다른 서비스에 의존하는 서비스를 정의합니다.
- build
    - build는 Dockerfile을 사용하여 이미지를 빌드합니다.
    - Dockerfile을 사용하여 새로운 이미지를 빌드합니다. build는 image와 다르게 이미지를 이미 가지고 있지 않습니다. 따라서 이미지를 빌드하기 위해 Dockerfile을 사용하며, 이미지를 로컬 또는 리모트 레지스트리에 저장할 수 있습니다.

web이라는 서비스를 만들 때의 예시는 다음과 같습니다.

```yml
services:
  web:
    image: nginx:latest
    # build: . (image 또는 build 하나만 사용)
    container_name: my-nginx-container
    ports:
      - "8080:80"
  	environment:
      - ENV_VAR=value
    volumes:
      - "/path/to/local/folder:/path/to/container/folder"
    depends_on:
      - db
```


### Networks

Docker Compose YAML 파일에서 networks는 애플리케이션의 컨테이너들이 속할 네트워크를 정의합니다. 컨테이너는 동일한 네트워크에서 연결된 다른 컨테이너와 통신할 수 있습니다.

아무런 설정을 하지 않을 경우 자동으로 생성됩니다.

하지만 다른 컨테이너 잘 통신하는 것이 필요한 경우 Network를 설정해 줘야 합니다.

- driver
    - driver는 네트워크 드라이버를 지정합니다. 네트워크 드라이버는 Docker Engine에서 제공하는 기능으로, 컨테이너를 연결하기 위한 다양한 네트워크 타입을 지원합니다.
- driver_opts
    - driver_opts는 네트워크 드라이버에 대한 옵션을 정의합니다.
- ipam
    - IP 주소 관리 방법을 정의합니다. IPAM(IP Address Management)은 IP 주소 할당과 관리를 자동화하기 위한 도구입니다.
- attachable
    - attachable은 다른 서비스에서 해당 네트워크를 사용할 수 있는지 여부를 지정합니다.

아직 이쪽 분야에 모르는 게 많아서 하나씩 단어를 찾아보면 다음과 같다.

- IPAM  
    IPAM은 IP 주소를 할당하고 관리하며, IP 주소 충돌을 방지하고, IP 주소를 효율적으로 사용할 수 있도록 지원합니다. IPAM은 DHCP(Dynamic Host Configuration Protocol) 서버를 통해 IP 주소를 동적으로 할당할 수 있습니다.  
    IPAM은 대규모 네트워크에서 특히 유용합니다. 대규모 네트워크에서는 많은 수의 IP 주소를 관리해야 하며, 이를 수동으로 할당하고 관리하면 매우 복잡하고 시간이 많이 소요됩니다. IPAM을 사용하면 이러한 작업을 자동화하고, IP 주소 관리를 간소화할 수 있습니다.

위의 예시에서는 overlay 네트워크 드라이버가 사용되고, attachable 옵션이 true로 설정되어 다른 서비스에서 my-network 네트워크를 사용할 수 있습니다.  
  
networks는 여러 개의 네트워크를 정의할 수 있습니다. 각각의 서비스는 networks 구성 요소를 사용하여 하나 이상의 네트워크에 참여할 수 있습니다.

```yml
networks:
  my-network:
    driver: bridge
    driver_opts:
      com.docker.network.bridge.name: my-bridge
    labels:
      environment: development
  internal-network:
    internal: true
    ipam:
      driver: default
      config:
        - subnet: 10.1.1.0/24
```



### Volumes
![[Docker-Compose7.png]]

도커 컨테이너를 사용하여 서비스를 배포하다 보면 데이터를 저장해야 할 필요성이 있습니다. 이때 도커에서 제공하는 Volume을 사용하면 호스트 파일시스템과 컨테이너의 파일시스템을 마운트 하여 데이터를 관리합니다.

이때 각각의 컨테이너에 Volume을 일일이 생성하고 관리하는 것은 번거로울 수 있습니다.

docker-compose의 volume을 사용하면, 여러 개의 컨테이너에 같은 볼륨을 사용할 수 있게 됩니다.

- driver 
    - 볼륨 드라이버 지정하는 데 사용
    - 도커 Volume을 생성할 때 기본적으로 사용되는 드라이버는 local입니다. 이 local 드라이버 이외에도 다른 드라이버를 사용할 수 있으며, driver 옵션을 사용하여 지정할 수 있습니다
    - local을 기본적으로 사용하고, 호스트 머신의 파일 시스템에 디렉터를 생성하고, 이를 컨테이너와 공유하여 데이터를 저장하는 방식
- name
    - 기본적으로 도커는 무작위로 이름을 생성하지만, name 옵션을 사용하여 이름을 지정할 수 있습니다.
- external
    - external 옵션을 사용하면 이미 생성된 Volume을 참조할 수 있습니다. 이 옵션을 사용하면 docker-compose up 명령을 실행할 때 Volume을 새로 생성하지 않고 기존 Volume을 사용합니다.
- volumes_from
	- 다른 컨테이너의 볼륨을 공유하는 경우 사용됩니다.
- type
	- bind
		- 호스트 파일 시스템과 컨테이너를 직접 바인딩하는 타입입니다. 
		- 호스트의 특정 경로가 컨테이너의 경로와 일치하도록 설정됩니다.
		- 이 설정은 호스트 파일 시스템의 변경 사항이 컨테이너에 반영되는 실시간 공유를 제공합니다.
	- volume
		- 독립적인 Docker 볼륨을 사용하는 타입입니다. 
		- 볼륨은 컨테이너와 분리된 상태로 관리되며, 여러 컨테이너에서 공유할 수 있습니다. 
		- 볼륨은 Docker 엔진에 의해 생성 및 관리되며, 호스트 파일 시스템에 직접 접근하지 않습니다.
	- tmpfs
		- 컨테이너의 임시 파일 시스템을 사용하는 타입입니다. 
		- 호스트 파일 시스템이 아닌 컨테이너 내부에 생성되며, 컨테이너가 종료되면 삭제됩니다. 주로 임시 데이터를 저장하고 싶을 때 사용됩니다.
- source
	- source는 Docker volumes에서 bind 타입을 사용할 때 사용되는 옵션입니다
	- source는 호스트 파일 시스템의 경로를 지정하는 것으로, 컨테이너와 호스트 간의 경로 매핑을 설정할 때 사용됩니다.
```yml
volumes:
  - type: bind
    source: /호스트/경로
    target: /컨테이너/경로

```
	


- target
	- `target`은 Docker volumes에서 경로 매핑을 설정하는 데 사용되는 옵션입니다.
	- `target`은 컨테이너 내부의 경로를 지정하는 것으로, 호스트 파일 시스템의 경로와 매핑되어 컨테이너 내부에서 해당 경로를 사용할 수 있게 합니다.

```yml
volumes:
  - type: bind
    source: /호스트/경로
    target: /컨테이너/경로

```

	- 합쳐서 쓰면 volumes: /호스트/경로:/컨테이너/내부/경로 대신 type은 bind






```yml
version: '3.9'
services:
  web:
    image: nginx
    volumes:
      - type: volume
        source: mydata
        target: /usr/share/nginx/html
  db:
    image: mysql
    volumes:
      - type: volume
        source: mydata
        target: /var/lib/mysql
volumes:
  mydata:
```

위의 코드와 같이 web과 db 서비스에 공유하는 Volume을 만들 수 있습니다.

### Environments

 컨테이너에 전달할 환경 변수를 설정하는 데 사용됩니다

환경 변수는 컨테이너 내부에서 실행되는 애플리케이션에서 사용할 수 있는 변수로, 컨테이너의 동작을 조정하는 데 중요한 역할


```yml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "80:80"
    environment:
      - DB_HOST=db
      - DB_USER=user
      - DB_PASSWORD=password
  db:
    image: mysql
    environment:
      - MYSQL_ROOT_PASSWORD=password
      - MYSQL_DATABASE=myapp
      - MYSQL_USER=user
      - MYSQL_PASSWORD=password
```

### Configuration

 "configuration" 옵션은 Docker 스웜을 사용할 때 스웜 서비스에 대한 구성을 지정하는 데 사용됩니다.

"configuration" 옵션을 사용하면 Docker Compose 파일 내에서 서비스와 관련된 설정을 중앙 집중적으로 관리할 수 있습니다.

음 많이 쓰는 것은 나는 아직까지는 못 봤다.

### Deployments 

애플리케이션 모델의 배포를 지원하는 Compose 구현에는 Compose 애플리케이션 모델이 서비스당 실제 인프라 요구 사항 또는 수명 주기 제약 조건을 반영하기에는 너무 추상적이기 때문에 일부 추가 메타데이터가 필요할 수 있습니다(MAY).

Compose 사양 배포를 통해 사용자는 서비스에 대한 추가 메타데이터를 선언할 수 있으므로 Compose 구현은 관련 데이터를 가져와 플랫폼에 적절한 리소스를 할당하고 사용자의 요구에 맞게 구성할 수 있습니다.

- replicas: 서비스 인스턴스의 수를 지정합니다.
- placement: 서비스 인스턴스를 배치할 노드를 선택하는 규칙을 지정합니다.
- update_config: 서비스 업데이트를 제어하는 옵션을 지정합니다.
- resources: 서비스 인스턴스가 사용할 CPU 및 메모리 등의 자원을 지정합니다.
- networks: 서비스가 연결할 네트워크를 지정합니다.

```yml
version: '3.7'

services:
  web:
    image: nginx:latest
    deploy:
      replicas: 3
      placement:
        constraints:
          - node.role == manager
      update_config:
        parallelism: 2
        delay: 10s
      resources:
        limits:
          cpus: '0.5'
          memory: 512M
      networks:
        - frontend
        - backend

networks:
  frontend:
  backend:
```

위의 예시는 다음과 같다고 한다.

위의 예시에서는 "web" 서비스를 배포할 때, "deploy" 옵션을 사용하여 다양한 설정을 지정하고 있습니다. 

예를 들어, "replicas" 옵션을 사용하여 3개의 서비스 인스턴스를 생성하고, "placement" 옵션을 사용하여 매니저 노드에서만 서비스 인스턴스를 배치하도록 지정하고 있습니다.

또한 "update_config" 옵션을 사용하여 서비스 업데이트를 제어하고, "resources" 옵션을 사용하여 서비스 인스턴스가 사용할 자원을 제한하고 있습니다. 마지막으로 "networks" 옵션을 사용하여 서비스가 연결할 네트워크를 지정하고 있습니다.



```yml
version: '3.9'
services:
  web:
    build: ./presentation
    ports:
      - "8080:80"
    depends_on:
      - app
  app:
    build: ./application
    ports:
      - "5000:5000"
    environment:
      - DB_HOST=db
      - DB_PORT=3306
      - DB_USER=user
      - DB_PASSWORD=password
      - DB_NAME=myapp
    depends_on:
      - db
  db:
    image: mysql:5.7
    restart: always
    environment:
      - MYSQL_DATABASE=myapp
      - MYSQL_USER=user
      - MYSQL_PASSWORD=password
      - MYSQL_ROOT_PASSWORD=root_password
    volumes:
      - db_data:/var/lib/mysql
volumes:
  db_data:
```


위의 구성 파일은 3개의 서비스를 정의합니다.  
  
web: Nginx를 사용하여 Presentation Layer를 구성하는 서비스입니다../presentation 디렉터리에서 Dockerfile을 빌드하며, 호스트의 8080 포트를 컨테이너의 80 포트로 매핑합니다.  

app: Flask를 사용하여 Application Layer를 구성하는 서비스입니다. ./application 디렉터리에서 Dockerfile을 빌드하며, 호스트의 5000 포트를 컨테이너의 5000 포트로 매핑합니다. db 서비스에 대한 의존성을 가지며, MySQL 데이터베이스 연결 정보를 환경 변수로 설정합니다.  

db: MySQL을 사용하여 Data Layer를 구성하는 서비스입니다. mysql:5.7 이미지를 사용하며, 호스트의 3306 포트를 컨테이너의 3306 포트로 매핑합니다. MySQL 데이터베이스 및 사용자 정보를 환경 변수로 설정하며, 데이터베이스 파일을 볼륨으로 저장합니다.

  
이렇게 Docker Compose를 사용하여 3-Tier 아키텍처를 구성하면, 각각의 레이어를 독립적으로 구성할 수 있으며, 서로 다른 서버에서 실행될 때 발생할 수 있는 호환성 문제를 최소화할 수 있습니다. 또한, Docker Compose를 사용하여 레이어 간의 의존성 및 관계를 쉽게 관리할 수 있으며, 스케일링과 관리도 용이합니다.



---
참조 - https://seosh817.tistory.com/387
https://www.stevenjlee.net/2020/05/08/%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-3%EA%B3%84%EC%B8%B5-%EA%B5%AC%EC%A1%B0-3-tier-architecture/


https://jaws-coding.tistory.com/9



https://data-newbie.tistory.com/930
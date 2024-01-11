
#### **Pre-set 설정 모음**

maven을 사용한 Spring Legacy 프로젝트 새로 생성할때를 기억을 더듬어볼까요? DB연결이 들어간다면 pom.xml파일에 jdbc관련 의존성 라이브러리 추가를 하고 ORM을 mybatis를 쓴다면 함께 추가해야합니다. 그리고 lombok, junit 등 프로젝트에 추가적인 내용을 설정하려면 pom.xml에 추가를 하고 그에 따른 설정을 xml로 해줘야합니다.

처음부터 Legacy를 구성해본 분들이라면 Spring Legacy 프로젝트 설정으로 삽질 1회 이상은 경험해봤을 겁니다.

그러나 Spring Boot는 많은 사람들이 사용하던 설정 내용을 Pre-set으로 구성해서 제공되므로 [https://start.spring.io/](https://start.spring.io/) 에서 원하는 의존성 기능들을 추가한 뒤 다운로드 받고 IDE에 Import하면 빠르게 설정내용을 프로젝트에 적용할 수 있습니다.

JetBeans사의 Intellij를 사용해서 프로젝트를 생성할 때는 Spring Initializer탭을 이용해서 생성을 진행하면  [https://start.spring.io/](https://start.spring.io/)과 비슷한 방법으로 프로젝트를 설정할 수 있습니다.

---

#### **WAS**

Spring Legacy 프로젝트를 진행하게 되면 WAS를 설치하고, 설정 하는 등 초기에 적지 않은 시간을 투자해서 WAS를 구성해야 했습니다.

하지만 Spring Boot에서는 내장 WAS가 있기 때문에 별도의 WAS를 설정하지 않아도 JVM 옵션을 Command Line에 입력해서 사용이 가능하다고 합니다.

---

#### **Spring Legacy와 동일한 기능**

Spring Boot는 Spring Legacy와 동일한 기능을 제공한다고 합니다. 정확하게 알지는 못하지만(자세하게 아시는 분들은 댓글로 제게 지식을 전도해주세요.)경량화를 통해서 프레임웍의 퍼포먼스가 향상된 Spring Boot가 나왔고 이는 프레임워크의 기능을 해치지 않는 선에서 이루어진 것 같습니다.

그리고 Spring Legacy가 버전업이되면 Spring Boot도 버전업이 된다고 합니다. 따라서 Spring Legacy의 새 버전에 릴리즈되는 기능이 Spring Boot에도 동작을 한다는 의미로 받아드릴 수 있다고 판단됩니다.

---

#### **Finish**

Spring Boot와 Spring Legacy와 비교를 해봤습니다. 사실 차이점이나 특징을 염두해두지 않고 개발을 한다면 Spring Legacy나 Spring Boot를 이용해서 개발할 때 크게 다른점은 없다고 생각합니다.

프로젝트의 방향성 그리고 프레임워크의 이해가 충분하다면 Legacy나 Boot중 최적의 퍼포먼스를 발휘할 수 있는 프레임워크를 선택하신다면 만족할만한 결과를 얻을 수 있을 것 같습니다.

출처: [https://kimdonghyungsoo.tistory.com/5](https://kimdonghyungsoo.tistory.com/5) [김동형수 개발기:티스토리]




---
참조 - https://kimdonghyungsoo.tistory.com/5

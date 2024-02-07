## @Entity

**@Entity** 어노테이션은 데이타베이스의 **테이블**과 일대일로 매칭되는 객체 단위이며 Entity 객체의 인스턴스 하나가 테이블에서 하나의 레코드 값을 의미합니다. 그래서 객체의 인스턴스를 구분하기 위한 유일한 키값을 가지는데 이것은 테이블 상의 **Primary Key** 와 같은 의미를 가지며 **@Id** 어노테이션으로 표기 됩니다.

먼저 Spring Boot 를 설정할때 **spring.jpa.hibernate.ddl-auto** 설정이 **create** 혹은 **update** 로 되어 있을 경우 Spring 프로젝트가 시작될때 EntityManager 가 자동으로 DDL 을 수행해 테이블을 생성해 줍니다.

이때 명시적으로 **@Table** 의 **name** 속성을 이용해 데이타베이스상의 실제 테이블 명칭을 지정하지 않는다면 **Entity 클래스**의 이름 그대로 **CamelCase 를 유지한채** 테이블이 생성이 되기 때문에 테이블 이름을 명시적으로 작성하는 것이 관례입니다. 왜냐하면 데이타베이스상에서 보편적으로 사용 되는 명명법은 **UnderScore** 가 원칙이기 때문입니다.

```java
@Entity
@Table(name = "ORGANIZATION")
public class Organization { 
    ...
}

@Entity
@Table(name = "EMPLOYEE")
public class Employee { 
    ...
}

@Entity
@Table(name = "EMPLOYEE_ADDRESS")
public class EmployeeAddress { 
    ...
}
```

## @Column

**@Column** 어노테이션은 데이타베이스의 테이블에 있는 컬럼과 동일하게 1:1로 매칭되기 때문에 Entity 클래스안에 내부변수로 정의 됩니다. 만약 테이블에 a, b, c 컬럼이 있다면 각각 3개의 **@Column** 어노테이션을 작성 하게 됩니다. 다만 이때 의도적으로 필요없는 컬럼들은 작성하지 않아도 되는데 데이타베이스 테이블에 실제 a, b, c, d 총 4개의 컬럼이 있더라도 a,b,c 컬럼만 Entity 클래스에 작성해도 무방 하다는 이야기 입니다.

이때 **@Column** 어노테이션은 별다른 옵션을 설정하지 않는다면 생략이 가능합니다. 즉 Entity 클래스에 정의된 모든 내부변수는기본적으로 @Column 어노테이션을 포함한다고 볼 수 있습니다.

Spring Boot 의 **spring.jpa.hibernate.ddl-auto** 설정이 **create** 혹은 **update** 로 되어 있을 경우 **create** 일때는 최초에 한번 컬럼이 생성이 되고, **update** 일때는 Entity 클래스에 있지만 해당 테이블에 존재하지 않는 컬럼을 추가로 생성해 줍니다. 하지만 컬럼의 데이타 타입이 변경 되었거나 길이가 변경 되었을때 자동으로 데이타베이스에 반영을 해주지는 않기 때문에 속성이 변경되면 기존 테이블을 **drop** 후 새롭게 **create** 하던지 개별로 **alter table** 을 통해 직접 ddl 구문을 적용하는 것이 좋습니다.

**spring.jpa.hibernate.ddl-auto** 설정이 **create-drop** 로 되어 있으면 프로젝트가 시작될때 자동으로 기존 테이블을 **drop** 한 후 **create** 를 해줍니다. 하지만 기존 스키마가 전부 삭제 되기 때문에 시스템 설계와 개발 시점에만 사용해야 하며 운영 시점에 **create, update, create-drop** 을 사용 하지 않아야 합니다.

**@Column** 도 **@Entity** 어노테이션과 동일하게 name 속성을 명시하지 않으면 Entity 클래스에 정의한 컬럼 변수의 이름으로 생성이 됩니다. 그렇기 때문에 **CamelCase** 로 작성된 컬럼 변수가 있다면 **UnderScore** 형식으로 name 을 명시적으로 작성 합니다.

데이타베이스상에서 컬럼은 실제 데이타가 가질 수 있는 최대 길이를 가지게 되는데 이것은 데이타베이스에 데이타를 효율적으로 관리하기 위해서 입니다. **@Column** 에도 이처럼 length 속성으로 길이를 명시 할 수 있습니다. 만약 length 속성이 없다면 기본 길이인 255가 지정 됩니다. 이것은 문자열 형태인 데이타 속성에만 해당 되며 큰 숫자를 표현하는 BigDecimal 일 경우 precision, scale 로 최대 길이를 지정 할 수 있습니다.
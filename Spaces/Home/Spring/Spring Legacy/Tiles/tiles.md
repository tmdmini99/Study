**Tiles란?**

Tiles는 레이아웃 템플릿 엔진으로 중복되는 include 코드를 사용하지 않고도 지정된 페이지 레이아웃에 따라 페이지 타일을 조합하여 완전한 페이지로 만들어줍니다.

- 반복되는 부분들을 한 곳에서 관리할 수 있게 도와주는 템플릿 프레임워크입니다.
    
- JSP Include와 비슷하지만, Tiles는 레이아웃 템플릿 엔진으로 레이아웃을 구성하는데 좀 더 세분화되고 관리하기 쉬워 유지보수에 용이합니다.
    
- tiles에는 상속 기능이 있기때문에, 기존의 값을 참조하여 그대로 쓸 수 있습니다.
    
- 2021.03.02 기준으로 3.0.8버전까지 나왔으며 더 이상 지원하지 않습니다.

**iles 관련 용어**

Template는 페이지의 구조를 기술하고 Attribute는 구조내에서 실제 내용에 해당하며, definition은 Template(구조)에 Attribute(내용)을 연결하여 랜더링가능한 페이지를 기술합니다.

★ Template
페이지 레이아웃을 의미하며, jsp 파일로 페이지의 기본 골격을 구성하고 
각 페이지의 실제 구성 내용은 definition에서 설정되는 Attribute(실제 내용) 태그를 사용하여 런타임 시 뿌려줍니다.
 · string 타입 Attribute 추가: <titles:getAsString name="속성명"/>
 · template 및 definition Attribute 추가: <tiles:insertAttribute name="속성명" />

★ Attribute
Template의 빈 공간을 채우기 위하여 사용되는 정보로 3가지 타입으로 구성
 · string: 직접 출력 할 문자열. ex. title
 · template : 템플릿 내 또 일부의 레이아웃을 기술할 수도 있다.
 · definition : 전체 혹은 일부 Attribute 들이 실제 내용으로 채워진 페이지, 페이지 구조(Template)와 레이아웃 내부를 채울 정보(Attribute)가 같이 정의된 페이지를 의미

★ Definition
사용자에게 제공되기 위하여 랜더링되는 Template과 Attribute들을 연결

**Tiles 사용법**

**1. 의존성 추가**

- Spring Boot 사용시 tomcat-embed-jasper, jstl 의존성을 추가합니다.
    
- tomcat-embed-jasper: Spring Boot 내장 톰캣에는 jasper engine이 없기 때문에 JSP 엔진 역할을 하는 패키지
    
- jstl: Spring Boot에서 JSTL을 사용할 수 있게 도와주는 패키지
    

```xml
<!-- JSP -->
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
</dependency>
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
</dependency>

<!-- tiles -->
<dependency>
    <groupId>org.apache.tiles</groupId>
    <artifactId>tiles-core</artifactId>
    <version>3.0.8</version>
</dependency>
<dependency>
    <groupId>org.apache.tiles</groupId>
    <artifactId>tiles-jsp</artifactId>
    <version>3.0.8</version>
</dependency>
<dependency>
    <groupId>org.apache.tiles</groupId>
    <artifactId>tiles-servlet</artifactId>
    <version>3.0.8</version>
</dependency>
```

​

**2. Tiles 관련 Bean 등록**

- Tiles 관련 Bean을 등록하여, Tiles를 통해 ViewResolver가 동작하도록 설정합니다.
    
- xml을 통해 스프링 빈을 등록하는 방법과 자바 코드로 스프링 빈을 등록하는 방법 2가지가 존재합니다.
    

```xml
<!-- 
    Spring Legacy 환경인 경우
    root-context.xml에 Tiles 관련 Bean을 등록합니다.
 -->
<bean id="tilesConfigurer" class="org.springframework.web.servlet.view.tiles3.TilesConfigurer">
	<property name="completeAutoload" value="true" />
	<property name="definitions">
		<list>
			<value>/WEB-INF/tiles/tiles.xml</value>
		</list>
	</property>
</bean>
<bean id="tilesViewResolver" class="org.springframework.web.servlet.view.UrlBasedViewResolver">
	<property name="viewClass" value="org.springframework.web.servlet.view.tiles3.TilesView" />
	<property name="order" value="1" />
</bean>
```

또는

```xml
 <!-- Tiles -->
    <beans:bean id="tilesConfigurer" 
                class="org.springframework.web.servlet.view.tiles3.TilesConfigurer">
        <beans:property name="definitions">
            <beans:list>
                <beans:value>/WEB-INF/tiles/tiles.xml</beans:value>
            </beans:list>
        </beans:property>
    </beans:bean>        
    <beans:bean id="tilesViewResolver" 
                class="org.springframework.web.servlet.view.UrlBasedViewResolver">
        <beans:property name="viewClass" 
                        value="org.springframework.web.servlet.view.tiles3.TilesView" />
        <beans:property name="order" value="1" />
    </beans:bean>
    
    <!-- viewResolver 설정 (사용자 view의 위치, 확장명 설정) -->	
    <beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
      <beans:property name="prefix" value="/WEB-INF/views/" />
      <beans:property name="suffix" value=".jsp" />
      <beans:property name="order" value="2" />
    </beans:bean> 

```


Tiles 세팅이 우선순위에 있어서 1순위 이기 때문에 아래의 코드를 통해 1순위로 지정을 해주었고

```xml
<beans:property name="order" value="1" />
```

기존의 viewResolver는 2순위로 지정해 주었습니다.

```xml
<beans:property name="order" value="2" />
```

한마디로 view(JSP파일)를 불러올 때 tiles를 우선순위로 두겠다는 소리입니다.



```java
// Java Config 설정시
// @Configuration, @Bean 어노테이션으로 Tiles 관련 Bean을 등록합니다.
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.view.tiles3.TilesConfigurer;
import org.springframework.web.servlet.view.tiles3.TilesView;
import org.springframework.web.servlet.view.tiles3.TilesViewResolver;

@Configuration
public class TilesConfig {

	@Bean
	public TilesConfigurer tilesConfigurer() {
		final TilesConfigurer configurer = new TilesConfigurer();
		configurer.setDefinitions(new String[] {"/WEB-INF/tiles/tiles.xml"});
		configurer.setCheckRefresh(true);
		return configurer;
	}
	
	@Bean
	public TilesViewResolver tilesViewResolver() {
		final TilesViewResolver resolver = new TilesViewResolver();
		tilesViewResolver.setViewClass(TilesView.class);
		tilesViewResolver.setOrder(1);
		return tilesViewResolver;
	}
}
```



**3. Tiles 관련 설정**

- src/main/webapp/WEB-INF 폴더에 tiles 폴더 생성 후 tiles 관련 설정 파일인 tiles.xml을 생성합니다.
    
- tiles.xml: tiles를 통해 보여줄 뷰의 위치를 설정할 파일
    
- default.jsp: 요청된 화면만을 표시할 jsp
    
- layouts 폴더: 요청된 화면 외에 기본 화면(header, footer)를 표시할 화면(*.jsp)



![[tiles 1.png]]



```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE tiles-definitions PUBLIC "-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN" "http://tiles.apache.org/dtds/tiles-config_3_0.dtd">
<tiles-definitions>
 
	<!-- 기본 레이아웃 설정 -->
	<definition name="tiles-default" template="/WEB-INF/tiles/default.jsp">
		<put-attribute name="header" value="/WEB-INF/tiles/layouts/header.jsp" />
		<put-attribute name="footer" value="/WEB-INF/tiles/layouts/footer.jsp" />
	</definition>
	<!-- 팝업 레이아웃 설정 (빈 레이아웃) -->
	<definition name="tiles-default" template="/WEB-INF/tiles/popup.jsp">
		<put-attribute name="body" />
	</definition>

	<!-- popup으로 끝나는 값 반환시 *popup.jsp 화면 반환 -->
	<!-- 해당 결과로 /WEB-INF/tiles/popup.jsp 화면이 반환되며, 해당 화면에는 body만 존재 -->
	<definition name="*popup" extends="tilesNoStyle">
		<put-attribute name="body" value="/WEB-INF/jsp/main/{1}popup.jsp" />
	</definition>

	<!-- 모든 요청에 대해 매핑 화면 반환 -->
	<!-- 해당 결과로 /WEB-INF/tiles/default.jsp 화면이 반환되며, 해당 화면에는 header, body, footer가 존재 -->
	<definition name="*" extends="tiles-default">
		<put-attribute name="body" value="/WEB-INF/jsp/main/{1}.jsp" />
	</definition>
	<definition name="*/*" extends="tiles-default">
		<put-attribute name="body" value="/WEB-INF/jsp/main/{1}/{2}.jsp" />
	</definition>
	<definition name="*/*/*" extends="tiles-default">
		<put-attribute name="body" value="/WEB-INF/jsp/main/{1}/{2}/{3}.jsp" />
	</definition>
</tiles-definitions>
```


\<put-attribute name="body" value="/WEB-INF/jsp/views/{1}/{2}.jsp" />에서 `{1}`과 `{2}`는 요청 경로의 각 부분에 해당하는 동적 변수를 나타냅니다.

```jsp
http://localhost:8080/yourapp/login/user
```

여기서 `login`이 첫 번째 경로 파트, `user`가 두 번째 경로 파트가 됩니다.

- `{1}`: 요청 경로의 첫 번째 부분, 이 경우 `login`
- `{2}`: 요청 경로의 두 번째 부분, 이 경우 `user`

따라서 위의 설정을 통해 다음과 같은 경로의 JSP 파일이 선택됩니다:


```
/WEB-INF/jsp/views/login/user.jsp
```

이 방식은 URL 패턴을 동적으로 처리할 수 있어, 다양한 경로에 대한 JSP 파일을 하나의 패턴으로 렌더링할 수 있게 해줍니다.

```jsp
// default.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://tiles.apache.org/tags-tiles" prefix="tiles"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>사이트 제목</title>
    
<style>
    .header {
        background-color : red;
        height : 150px;
    }
    
    .contents {
        background-color : green;
        height : 500px;
    }
    
    .footer {
        background-color : blue;
        height : 100px;
    }
</style>
</head>
<body>
    <!-- Page Header -->
    <tiles:insertAttribute name="header" />
    
    <!-- Page Contents -->
    <tiles:insertAttribute name="body" />
    
    <!-- Page Footer -->
    <tiles:insertAttribute name="footer" />
    
</body>
</html>

// footer.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<div class="footer">Footer</div>

// header.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<div class="header">Header</div>
```

**Tiles 동적 메뉴 설정하기**

Tiles를 이용하는 경우 ViewPrepare 인터페이스를 구현하여 TilesRequestContext나 AttributeContext를 이용해서 미리 데이터를 설정하여 페이지에서 해당 데이터를 사용할 수 있습니다.

- 1. 기존 Tiles 관련 Bean에 ViewPrepare 기능을 사용하기 위한 옵션을 설정합니다.
    
- 2. ViewPrepare 인터페이스를 구현한 클래스를 작성합니다.
    
- 3. 이후 해당 클래스를 Bean으로 등록합니다.
    
- 4. tiles 레이아웃 화면에서 해당 ViewPrepare에서 미리 설정한 데이터를 사용하여 동적 화면을 제공합니다.
    

​

**1. 기존 Tiles 관련 Bean에 ViewPrepare 기능을 사용하기 위한 옵션 설정**

· SpringBeanPrepareFactory 옵션 등록

```xml
<bean id="tilesConfigurer" class="org.springframework.web.servlet.view.tiles3.TilesConfigurer">
	<property name="definitions">
		<list>
			<value>/WEB-INF/tiles/tiles.xml</value>
		</list>
	</property>
	<property name="preparerFactoryClass" value="org.springframework.web.servlet.view.tiles3.SpringBeanPreparerFactory"/>
</bean>
```

**2. ViewPrepare 인터페이스를 구현한 클래스 작성**


```java
// MenuPreparer
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

import org.apache.tiles.Attribute;
import org.apache.tiles.AttributeContext;
import org.apache.tiles.preparer.ViewPreparer;
import org.apache.tiles.request.Request;
import org.springframework.stereotype.Component;

@Component
public class MenuPreparer implements ViewPreparer {

	@Override
	public void execute(Request tilesContext, AttributeContext attributeContext) {

		// 메뉴(URL, 이름) 정보를 담은 List 설정
		List<MenuVO> menuList = new ArrayList<MenuVO>();
		String[][] menuArray = {
				{"Home","/home"},
				{"About","/about"},
				{"Contact", "/contact"}
		};
		for(String[] menuArr : menuArray) {
			MenuVO menu = new MenuVO(menuArr[0], menuArr[1]);
			menuList.add(menu);
		}

		attributeContext.putAttribute("menuList", new Attribute(menuList), true);
	}
}

// MenuVO
@Data
public class MenuVO {
	private String menuName;
	private String menuUrl;
}
```



**3. MenuPrepare Bean 등록**


**4. tiles 레이아웃 화면에서 동적 메뉴 설정**

- /WEB-INF/tiles/tiles.xml 수정 : menuPreparer 등록

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE tiles-definitions PUBLIC "-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN" "http://tiles.apache.org/dtds/tiles-config_3_0.dtd">
<tiles-definitions>
 
	<!-- 기본 레이아웃 설정 -->
	<definition name="tiles-default" template="/WEB-INF/tiles/default.jsp" preparer="menuPreparer">
		<put-attribute name="header" value="/WEB-INF/tiles/layouts/header.jsp" />
		<put-attribute name="footer" value="/WEB-INF/tiles/layouts/footer.jsp" />
	</definition>

	..
</tiles-definitions>
```

- /WEB-INF/tiles/layouts/header.jsp 수정



```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" isELIgnored="false"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="tiles" uri="http://tiles.apache.org/tags-tiles"%>
<c:set var="requestURI" value="${requestScope['javax.servlet.forward.request_uri']}"/>

<ul>
<c:forEach var="menu" items="${menuList}">
	<li>
		<a href="${menu.menuUrl}" title="${menu.menuName}">
			<c:out value="${menu.menuName}">
		</a>
	</li>
</c:forEach>
</ul>
```














---

출처 - https://m.blog.naver.com/hj_kim97/222935327085


https://admm.tistory.com/45
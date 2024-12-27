

pom.xml
```xml
<!-- Tiles -->
	<dependency>
            <groupId>org.apache.tiles</groupId>
            <artifactId>tiles-extras</artifactId>
            <version>3.0.8</version>
          </dependency>
        <dependency>
            <groupId>org.apache.tiles</groupId>
            <artifactId>tiles-servlet</artifactId>
            <version>3.0.8</version>
        </dependency>
        <dependency>
            <groupId>org.apache.tiles</groupId>
            <artifactId>tiles-jsp</artifactId>
            <version>3.0.8</version>
        </dependency>
```

servlet-context.xml
```xml
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
    
    <!-- viewResolver 설정 (사용자 view의 위치, 확장명 설정)   개인설정-->	
    <beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
      <beans:property name="prefix" value="/WEB-INF/views/" />
      <beans:property name="suffix" value=".jsp" />
      <beans:property name="order" value="2" />
    </beans:bean> 
```


```xml
<beans:property name="order" value="1" />


<beans:property name="order" value="2" />
```


```xml
<beans:value>/WEB-INF/tiles/tiles.xml</beans:value>
```
사용법


tile.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE tiles-definitions PUBLIC
  "-//Apache Software Foundation//DTD Tiles Configuration 2.1//EN"
  "http://tiles.apache.org/dtds/tiles-config_2_1.dtd">

<tiles-definitions>

	<!-- tiles 적용 -->
	<definition name="tilesLayout" template="/WEB-INF/jsp/layout/TilesLayout.jsp">
		<put-attribute name="siteTop" value="/WEB-INF/jsp/layout/SiteTop.jsp" />
		<put-attribute name="content" value="" />
		<put-attribute name="siteBottom" value="/WEB-INF/jsp/layout/SiteBottom.jsp" />
	</definition>
 
	<definition name="*.tiles" extends="tilesLayout">
		<put-attribute name="content" value="/WEB-INF/jsp/{1}.jsp" />
	</definition>
	
	<definition name="*/*.tiles" extends="tilesLayout">
		<put-attribute name="content" value="/WEB-INF/jsp/{1}/{2}.jsp" />
	</definition>
	
	
		
	<!-- 타일즈 미 적용 -->
	<definition name="normalLayout" template="/WEB-INF/jsp/layout/NormalLayout.jsp">
		<put-attribute name="NormalLayout" value="" />
	</definition>
	
	<definition name="*.jsp" extends="normalForm">
		<put-attribute name="NormalLayout" value="/WEB-INF/jsp/{1}.jsp" />
	</definition>
		
	<definition name="*/*.jsp" extends="normalForm">
		<put-attribute name="NormalLayout" value="/WEB-INF/jsp/{1}/{2}.jsp" />
	</definition>
	
</tiles-definitions>
```

tiles.xml에서는 컨트롤러에서 리턴한 String값에 따라 타일즈를 적용할지 말지를 결정하는 설정을 넣습니다. 위의 코드에서는 컨트롤러에서 리턴하는 String 값이 *.tiles일 경우 tiles적용을 *.jsp일 경우 적용을 안하도록 설정했으며 jsp마다 경로의 단계차이가 있을 수 있기 때문에 /*/*.tiles 처럼 몇가지 경로를 더 추가해줬습니다.










---
참조 - https://admm.tistory.com/45
---
title:  "[Spring] @Annotation(어노테이션)"

categories:
  - Spring
tags:
  - Spring
  - Annotation
last_modified_at: 2023-01-25T01:16:00-05:00

toc: true
toc_sticky: true
toc_label: "목차"
---

## 1. Spring @Annotation 개념

### 1.1 Spring @Annotation 이란?

@Annotation 이란 사전적 의미로는 '주석'이라는 뜻으로<br/>
작성해놓은 자바코드에 '@{Annotation명}'를 추가해서 의미를 부여하고,<br/>
각각의 Annotation 마다 특별한 의미, 기능을 수행하도록 하는 기술입니다.

Annotation은 클래스, 메서드, 파라미터 등 다양한 요소에 사용이 가능한데

```java
@SpringBootApplication
public class HelloSpringApplication {
	public static void main(String[] args) {
		SpringApplication.run(HelloSpringApplication.class, args);
	}
}
```
위에 코드에서 쓰인 Annotation은 <span style="color:green;font-weight:bold;">SpringBootApplication</span>으로,<br/> 
Spring Boot 프로젝트를 생성했을 때 기본적으로 가지고 있는 Annotation 입니다. 

@SpringBootApplication Annotation은 뒤에서 자세히 알아보겠습니다.

<hr/>

## 2. Spring @Annotation 종류

| Annotation명 | 설명 |
| :-- | :--: |
| @SpringBootApplication | Spring Boot Framework에서 Application 클래스에서 사용하는 Annotation |
| @Controller | Spring MVC Framework에서 JSP 같은 View, 즉 client에게 보여줄 페이지를 넘겨주는 Annotation |
| @RestController | XML 또는 JSON 형식으로 HTTP 응답을 통해 데이터 자체를 넘겨준다. |
| @RequestMapping | client가 보낸 요청(특정 URI)과 매핑하기 위한 Annotation  |
| @Repository | DB나 파일같은 외부 I/O 작업을 처리하기 위한 Annotation  |
| @Service | 비즈니스 로직을 수행하는 서비스 레이어임을 알려주는 Annotation  |

### 2.1 @SpringBootApplication

[@SpringBootApplication](https://docs.spring.io/spring-boot/docs/2.0.x/reference/html/using-boot-using-springbootapplication-annotation.html)은 Spring Boot 개발자들이 자동 구성을 할 수 있게 해주는 Annotation으로<br/>
@SpringBootAnnotation을 이용해서 다음 세 가지 기능을 활성화할 수 있습니다.
 1. [@EnableAutoConfiguration](#211-enableautoconfiguration)
 2. [@ComponentScan](#212-componentscan)
 3. [@Configuration](#213-configuration)

#### 2.1.1 @EnableAutoConfiguration
@EnableAutoconfiguration은 Spring Boot 환경에서 Application Context 설정을 자동으로 수행한다는 Annotation으로,<br/>
spring-boot-autoconfigure-[version].jar/META-INF/spring.factories에 정의되어 있는
configuration 대상 클래스들을 Bean으로 등록해줍니다.

```Java
// ...
# Auto Configuration Import Filters
org.springframework.boot.autoconfigure.AutoConfigurationImportFilter=\
org.springframework.boot.autoconfigure.condition.OnBeanCondition,\
org.springframework.boot.autoconfigure.condition.OnClassCondition,\
org.springframework.boot.autoconfigure.condition.OnWebApplicationCondition
// ...
```
spring.factories에는 엄청나게 많은 클래스들이 Auto Configuration 대상으로 작성되어 있는데, <br />
이 리스트에 있는 클래스들은 자동으로 Bean 등록이 됩니다.

Auto Configuration에 대한 내용은 추후에 자세히 다뤄보겠습니다.

#### 2.1.2 @ComponentScan
@ComponentScan Annotation은 Bean으로 등록할 클래스들의 스캔 위치를 설정하고,<br/>
어떤 Annotation을 스캔할지 결정하는 Filter 기능을 가지고 있습니다.

스캔 위치를 설정하는 방법으로는 BasePackage와 BasePackageClasses가 있는데, <br/>
BasePackage는 String으로 입력된 패키지의 경로를 스캔하는 방법이고,<br/>
BasePackageClasses는 전달된 클래스의 위치를 기준으로 스캔하는 방법입니다.

위에서 잠깐 다룬 [@SpringBootApplication](#21-springbootapplication) Annotation 내부로 들어가보면,<br/>

```Java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {

// ... 
```
@ComponentScan Annotation을 찾을 수 있는데, ComponentScan 관련 설정을 하지 않았다면<br/>
@SpringBootApplication Annotation이 정의된 클래스 위치가 BasePackage가 됩니다.

만약 @SpringBootApplication Annotation이 정의되지 않은 곳에서 Bean 등록을 하고 의존성 주입을 시도한다면 

```Java
The injection point has the following annotations:
	- @org.springframework.beans.factory.annotation.Autowired(required=true)
```

이처럼 Bean을 찾지 못했다는 에러를 만날 수 있습니다.

결론적으로 @ComponentScan Annotation은 BasePackage에서 <span style="font-weight:bold;">@Component</span> Annotation이 붙어 있는 클래스들을 Bean으로 등록해줍니다.

#### 2.1.3 @Configuration
@Configuration Annotation은 특정 클래스에서 수동으로 스프링 컨테이너에 Bean 등록을 하겠다고 명시해주는 Annotation으로<br/>
@Configuration Annotation도 내부적으로 @Component Annotation을 가지고 있습니다.

수동으로 Bean 등록을 할 때 메서드명이 Bean 이름으로 지정되며, 

임의 클래스에서 @Bean Annotation을 활용하여 Bean 등록이 가능하긴 하나,<br/>
<span style="color:blue;font-weight:bold">싱글톤</span>을 보장받기 위해서는 @Configuration Annotation과 함께 사용해야 합니다.

### 2.2 @Controller, @RestController

### 2.3 @RequestMapping

### 2.4 @Repostiory

### 2.5 @Service



## Reference
[devNote - [Spring Boot] 어노테이션 정리](https://www.genuitec.com/spring-frameworkrestcontroller-vs-controller/)<br/>
[jwkim.log - 스프링 부트 차곡차곡](https://velog.io/@jwkim/series/essentials)<br/>
[Mangkyu - [Spring]](https://mangkyu.tistory.com/category/Spring%20)
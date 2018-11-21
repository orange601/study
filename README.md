# study

출처 http://rebeccajo.tistory.com/5

web.xml이란?
- 서블릿 배포 서술자 라고 부른다
- WAS 구동 시 /WEB-INF 디렉토리에 존재하는 web.xml 파일을 읽어 들여 웹 어플리케이션 설정을 구성하기 위함이다.

# web.xml
```xml
<listener>
  <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

1. ContextLoaderListener
- 출처 http://memo.polypia.net/archives/2344
- Creates the spring Container shared by all Servlets and Filter.
  - 서블릿이 2개 이상일때 context-param으로 등록한 설정파일을 공유한다. 전역으로 사용한다.
  - DispatcherServlet을 2개 이상 생성시 각 별도의 WebApplicationContext를 생성하게 됩니다. 
  두개의 context는 독립적이므로 각각의 설정파일에서 생성한 빈을 사용할 수 없습니다.( 공유 X )
 출처 http://unabated.tistory.com/entry/%EC%8A%A4%ED%94%84%EB%A7%81-ContextLoaderListener-%EC%9D%98-%EC%97%AD%ED%95%A0

2. webAppRootKey 오류
- 출처 http://memo.polypia.net/archives/2344
- 한개의 컨테이너에 두개의 Webapp을 올릴 경우 나는 오류로 Log4j에서 라는 System.property(“webapp.root”)를 사용해서 발생하는 문제다.
하나의 인스턴스내에서 System.property는 공유되니까.. 한마디로 전역변수 문제라고 할 수 있다. 해결방법은 web.xml에서 앱마다 저 키값을 
고유하게 지정 해 주는 것이다. 
```xml
기본값 - 아무것도 설정하지 않았을 경우
<context-param>
	<param-name>webAppRootKey</param-name>
	<param-value>webapp.root</param-value>
</context-param>
 
앱마다 다르게 설정.
<context-param>
	<param-name>webAppRootKey</param-name>
	<param-value>app1webapp.root</param-value>
</context-param>
```

말 그대로 log4j에서 webapp의 root 디렉토리를 가져올 때 사용하는 값이다. {tomcat_home}/webpaps/{app war파일명?}

# servelet-context.xml
1. mvc:annotation-driven
- 출처 http://kdarkdev.tistory.com/103
- RequestMapping, ModelAttribute, SessionAttribute, RequestParam 등 Annotation 사용을 위해 mvc:annotation-driven을 사용.

2. MappingJackson2HttpMessageConverter
- 출처: http://victorydntmd.tistory.com/172 [victolee]
- 데이터 전송할때 xml 형태가 아닌 json 형태로 
- MessageConverter를 통해 java 객체를 JSON으로 응답하게 되면 Ajax 통신이 가능

3. default-servlet-handler
<mvc:default-servlet-handler />만 있으면 되는 줄 알았는데 <mvc:annotation-driven /> 코드도 있어야 정상적으로 동작이 되더라구요.

이 부분은 조금 더 알아봐야 할 것 같습니다.



출처: http://victorydntmd.tistory.com/165 [victolee]
- Handler Mapping에 해당하는 URL이 없으면 default-servlet으로 처리하겠다는 의미
- 출처: http://victorydntmd.tistory.com/165 [victolee]
- <mvc:default-servlet-handler />만 있으면 되는 줄 알았는데 <mvc:annotation-driven /> 코드도 있어야 정상적으로 동작



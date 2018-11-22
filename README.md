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
- Handler Mapping에 해당하는 URL이 없으면 default-servlet으로 처리하겠다는 의미
- 출처: http://victorydntmd.tistory.com/165 [victolee]
- <mvc:default-servlet-handler />만 있으면 되는 줄 알았는데 <mvc:annotation-driven /> 코드도 있어야 정상적으로 동작

- <mvc:resources>를 사용할 경우에는 필요없다. 
DispatcherServlet이 처리하지 못한 요청을 서블릿 컨테이너의 DefaultServlet에게 넘겨주는 역할을 하는 핸들러이다. [토스3] 스프링 3.0.4 <mvc:default-servlet-handler/>를 이용해서 UrlRewriteFilter없이 깔끔한 URL을 만들기 » Toby's Epril 참조.
일단 DispatcherServlet을 그냥 /에 매핑한다. jsp와 같은 특정 확장자를 가진 URL말고는 모두 DispatcherServlet이 다 받는다. 일단 스프링의 기본 등록된 핸들러 매핑 전략을 이용해서 컨트롤러를 매핑해본다. @Controller가 담당하는 URL이라면 그리로 넘어갈거고. 그런데 그러다보면 /js/jquery.js 처럼 컨트롤러에 매핑안되는 URL이 나올 것이다. 이런 나머지 모든 URL은 <mvc:default-servlet-handler/>이 내부적으로 등록해주는 DefaultServletHttpRequestHandler이 담당한다. 이 핸들러(컨트롤러)는 /**로 매핑되어있다. 대신 핸들러 매핑 우선순위가 가장 낮다. 따라서 애노테이션 매핑 등등을 거쳐서 다 실패한 URL만 넘어온다. 그리고 DefaultServletHttpRequestHandler는 이 요청을 자신이 직접 스태틱 리소스를 읽어서 처리하는 것이 아니라, 원래 서버가 제공하는 디폴트 서블릿으로 넘겨버린다. 그러면 서버의 기본 디폴트 서블릿이 동작해서 스태틱리소스를 처리해버리는 것이다. 일단 스프링이 다 받고 스프링이 처리 못하는 건 다시 서버의 디폴트 서블릿으로 넘긴다는 아이디어이다.

출처:http://kwonnam.pe.kr/wiki/springframework/mvc



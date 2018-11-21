# study

http://rebeccajo.tistory.com/5



# web.xml
```xml
<listener>
  <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

1. ContextLoaderListener
- Creates the spring Container shared by all Servlets and Filter.
  - 서블릿이 2개 이상일때 context-param으로 등록한 ContextConfig(설정파일)을 공유한다. 
  즉 cntextConfigLocation을 통해 등록한 설정파일을 전역으로 사용할 수 있게 한다.
  - DispatcherServlet을 2개 이상 생성시 각 별도의 WebApplicationContext를 생성하게 됩니다. 
  두개의 context는 독립적이므로 각각의 설정파일에서 생성한 빈을 사용할 수 없습니다( 공유 X )
  참조: http://unabated.tistory.com/entry/%EC%8A%A4%ED%94%84%EB%A7%81-ContextLoaderListener-%EC%9D%98-%EC%97%AD%ED%95%A0

2.

# study

http://rebeccajo.tistory.com/5

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
- Creates the spring Container shared by all Servlets and Filter.
  - 서블릿이 2개 이상일때 context-param으로 등록한 설정파일을 공유한다. 전역으로 사용한다.
  - DispatcherServlet을 2개 이상 생성시 각 별도의 WebApplicationContext를 생성하게 됩니다. 
  두개의 context는 독립적이므로 각각의 설정파일에서 생성한 빈을 사용할 수 없습니다.( 공유 X )
 http://unabated.tistory.com/entry/%EC%8A%A4%ED%94%84%EB%A7%81-ContextLoaderListener-%EC%9D%98-%EC%97%AD%ED%95%A0

2. 

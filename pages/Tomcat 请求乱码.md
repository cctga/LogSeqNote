- 解决 Post 乱码
  id:: 6187bea2-d10c-444c-a48a-92b38d066177
	- 为 tomcat 添加一个 filter 过滤器，使用 Springmvc
	- ```xml
	  <!--springmvc提供的针对post请求的编码过滤器-->
	  <filter>
	    <filter-name>encoding</filter-name>
	    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
	    <init-param>
	      <param-name>encoding</param-name>
	      <param-value>UTF-8</param-value>
	    </init-param>
	  </filter>
	  
	  <!-- 配置拦截器的拦截范围，相当于 AOP 的 pointcut -->
	  <filter-mapping>
	    <filter-name>encoding</filter-name>
	    <url-pattern>/*</url-pattern>
	  <filter-mapping>
	  ```
- 解决 Get 乱码
	- 在 tomcat 的配置文件 server.xml 中进行 Connector 标签的编码指定 URIEncoding
	- ```xml
	  <Connector URIEncoding="utf-8" port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" />
	  ```
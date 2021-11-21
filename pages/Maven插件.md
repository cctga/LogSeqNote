- maven-compiler-plugin
  id:: 61926a19-4993-4f9b-af42-c2aff841d20f
	- 指定编译级别
	- ```xml
	  <build>
	    <plugins>
	      <plugin>
	        <groupId>org.apache.maven.plugins</groupId>
	        <artifactId>maven-compiler-plugin</artifactId>
	        <version>3.1</version>
	        <configuration>
	          <!--指定源码 JDK 版本-->
	          <source>11</source>
	          <!--指定编译使用的 JDK 版本-->
	          <target>11</target>
	          <!--指定编译的编码-->
	          <encoding>UTF-8</encoding>
	          <!--编译参数，编译后保持形参名称，以让 spring 根据参数名称注入-->
	          <compilerArgs>
	            <arg>-parameters</arg>
	          </compilerArgs>
	        </configuration>
	      </plugin>
	    </plugins>
	  </build>
	  ```
-
- tomcat7-maven-plugin
	- tomcat7 服务插件，可以不需要部署到 [[tomcat]]，直接 maven 中启动
	- ```xml
	  <plugin>
	    <groupId>org.apache.tomcat.maven</groupId>
	    <artifactId>tomcat7-maven-plugin</artifactId>
	    <version>2.2</version>
	    <configuration>
	      <port>8080</port>
	      <path>/</path>
	    </configuration>
	  </plugin>
	  ```
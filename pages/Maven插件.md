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
	        </configuration>
	      </plugin>
	    </plugins>
	  </build>
	  ```
-
-
- 类似 [[Maven]] 的构建工具
- 安装
	- `brew install gradle `
- 配置远程仓库
	- 在 HOME 的 `.gradle/` 下创建文件 `init.gradle` 并写入以下内容
	- 配置成阿里云 maven 仓库
	- ```groovy
	  allprojects{
	      repositories {
	          def REPOSITORY_URL = 'http://maven.aliyun.com/nexus/content/groups/public/'
	          all { ArtifactRepository repo ->
	              if(repo instanceof MavenArtifactRepository){
	                  def url = repo.url.toString()
	                  if (url.startsWith('https://repo1.maven.org/maven2') || url.startsWith('https://jcenter.bintray.com/')) {
	                      project.logger.lifecycle "Repository ${repo.url} replaced by $REPOSITORY_URL."
	                      remove repo
	                  }
	              }
	          }
	          maven {
	              url REPOSITORY_URL
	          }
	      }
	  }
	  ```
	- 使用 `init.gradle` 使之生效
		- `gradle –init-script init.gradle `
- `build.gradle` 配置文件配置
	- 类似于 Maven 的 pom.xml 文件
	- 指定 Java 版本
		- `sourceCompatibility=8`
	- 添加依赖
		- 在 dependencies 下添加
		- ```groovy
		  dependencies {
		      testImplementation 'org.junit.jupiter:junit-jupiter-api:5.7.0'
		      testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.7.0'
		      compile(project(":spring-context"))
		  }
		  ```
		- ```groovy
		  /*第一种（只添加一种依赖，全部写全）：*/
		  compile "group":"org.springframework","name":"spring-beans","version":"5.2.5.RELEASE"
		  
		  /*第二种（只添加一种依赖，简写）：*/
		  compile "org.springframework:spring-core:5.2.5.RELEASE"
		  
		  /*第三种（添加多种依赖）：*/
		  compile(
		          "org.springframework:spring-core:5.2.5.RELEASE",
		          "org.springframework:spring-beans:5.2.5.RELEASE"
		  )
		  ```
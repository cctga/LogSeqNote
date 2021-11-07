- DI 是什么
	- [[Spring IOC]]
	- IOC 和 DI 描述的是同一件事情，但是出发点不一样，IOC 从对象角度出发，将对象的控制交给了容器，DI 从容器角度出发，为相互依赖的对象进行依赖的注入
- DI 的三种方式
	- 注入也提供了复杂数据类型的注入，如 List，Map 等，但是几乎不使用，所以这里不做陈述
	- 构造函数注入
		- 自动调用类的对应的构造函数来生成对象，相应的属性值在构造器中初始化
		- 如果类没有相应的构造器将会报错
		- ``` java
		  public class User {
		    private String name;
		    private int age;
		    public User(String name, int age){
		      this.name = name;
		      this.age = age;
		    }
		  }
		  ```
		- ```xml
		  <bean id="user" class="top.maoyilan.model.User">
		    <constructor-arg name="name" value="cctga"/>
		    <constructor-arg name="age" value="28"/>
		  </bean>
		  ```
	- Setter 注入
		- 自动调用类的相应属性的 setter 方法来进行属性值的注入
		- ```java
		  public class User {
		    private String name;
		    private int age;
		    private Role role;
		    public void setName(String name){
		      this.name = name;
		    }
		    public void setAge(int age){
		      this.age = age;
		    }
		    public void setRole(Role role){
		      this.role = role;
		    }
		  }
		  public class Role {
		    private String name;
		    private int id;
		  }
		  ``` #card #card
		- ```xml
		  <bean id="user" class="top.maoyilan.model.User">
		    <property name="name" value="cctga"/>
		    <property name="age" value="28"/>
		    <property name="role" ref="role"/>
		  </bean>
		  
		  <bena id="role" class="top.maoyilan.model.Role"/>
		  ```
	- 注解注入
		- ((16e999fd-8479-457f-86aa-d2b262180114))
		- ((ff88a13e-0451-4886-b74a-5ba170a9d133))
		- ((066a9d79-1292-42ed-b9d1-e2294764ca26))
		- ((46a6510a-00e5-413c-9e48-17ea3904e0ff))
		- ((47abb64b-9663-4043-bbe0-2ce9881a32e9))
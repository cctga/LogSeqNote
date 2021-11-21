- 基于 JPA 规范，简化 jdbc 的使用，作用和 [[MyBatis]] 一样
	- 使用方式和底层机制不同
	- 很多时候不需要使用者使用 SQL 语句
- 和 JPA 规范，Hibernate 的关系
	- 最初出现了 JDBC
	- 由 JDBC 出现了 ORM 的思想
	- 依据 ORM ，出现了 Hibernate 等完全 ORM 的框架
	- Sun 公司总结了 Hibernate 这些框架，抽取出了 JPA 规范
	- Spring Data JPA 封装了 JPA 规范，优化了操作，实现默认选择的是 Hibernate
- 基础使用
	- 配置 Maven 编译行为
		- ((61926a19-4993-4f9b-af42-c2aff841d20f))
	- 引入 Jar 包
		- spring-data-jpa 相关包
		- 由于使用的是 Hibernate 的JPA 实现，所以需要 Hibernate 相关包
			- hibernate-core 核心包
			- hibernate-entitymanager 相当于 mybatis 的 sqlsession
			- hibernate-validator 校验
		- 其他如 jdbc 包，线程池 druid 包等
	- 配置 applicationContext.xml
	  collapsed:: true
		- 数据库连接配置
			- ```xml
			  <!--引入外部资源文件-->
			  <context:property-placeholder location="classpath:jdbc.properties"/>
			  
			  <!--第三方jar中的bean定义在xml中-->
			  <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
			    <property name="driverClassName" value="${jdbc.driver}"/>
			    <property name="url" value="${jdbc.url}"/>
			    <property name="username" value="${jdbc.username}"/>
			    <property name="password" value="${jdbc.password}"/>
			  </bean>
			  ```
		- EntityManagerFactory 对象配置，类似于 SqlSessionFactory 配置
			- 具体类为 `LocalContainerEntityManagerFactoryBean`
			- 配置数据源，引用上一步的数据源
			- 配置包扫描，pojo 包
			- 配置 jpa 的实现类
			- 配置 jpa 方言，可以看成是对 jpa 实现的一些扩展
			- jpa 实现的适配器，用 spring 来配置实现类的配置
				- 数据库表是否自动创建，在发现 pojo 对应的数据表不存在时
				- 指定数据库类型
				- 配置数据库方言的实现类
				- 是否显示 Sql，在执行时是否打印执行的 Sql
			- ```xml
			  <bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
			    <!--配置一些细节.......-->
			  
			    <!--配置数据源-->
			    <property name="dataSource" ref="dataSource"/>
			    <!--配置包扫描（pojo实体类所在的包）-->
			    <property name="packagesToScan" value="com.lagou.edu.pojo"/>
			    <!--指定jpa的具体实现，也就是hibernate-->
			    <property name="persistenceProvider">
			      <bean class="org.hibernate.jpa.HibernatePersistenceProvider"/>
			    </property>
			    <!--jpa方言配置,不同的jpa实现对于类似于beginTransaction等细节实现起来是不一样的，
			        所以传入JpaDialect具体的实现类-->
			    <property name="jpaDialect">
			      <bean class="org.springframework.orm.jpa.vendor.HibernateJpaDialect"></bean>
			    </property>
			  
			  
			    <!--配置具体provider，hibearnte框架的执行细节-->
			    <property name="jpaVendorAdapter" >
			      <bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
			        <!--定义hibernate框架的一些细节-->
			  
			        <!--
			            配置数据表是否自动创建
			            因为我们会建立pojo和数据表之间的映射关系
			            程序启动时，如果数据表还没有创建，是否要程序给创建一下
			        -->
			        <property name="generateDdl" value="false"/>
			  
			  
			        <!--
			            指定数据库的类型
			            hibernate本身是个dao层框架，可以支持多种数据库类型的，这里就指定本次使用的什么数据库
			        -->
			        <property name="database" value="MYSQL"/>
			  
			  
			        <!--
			            配置数据库的方言
			            hiberante可以帮助我们拼装sql语句，但是不同的数据库sql语法是不同的，所以需要我们注入具体的数据库方言
			        -->
			        <property name="databasePlatform" value="org.hibernate.dialect.MySQLDialect"/>
			        <!--是否显示sql
			            操作数据库时，是否打印sql
			        -->
			        <property name="showSql" value="true"/>
			      </bean>
			    </property>
			  </bean>
			  ```
		- 配置 jpa 的 dao 层的实现，引用 EntityManagerFactory 对象，事务管理器等
			- 指定 dao 的包
			- 指定 entityManagerFactory
			- 指定 transaction-manager
			- ```xml
			  <!--
			       <jpa:repositories> 配置jpa的dao层细节
			       base-package:指定dao层接口所在包
			  -->
			  <jpa:repositories base-package="com.lagou.edu.dao" entity-manager-factory-ref="entityManagerFactory"
			                    transaction-manager-ref="transactionManager"/>
			  
			  ```
		- 事务管理器配置
			- ```xml
			  <!--4、事务管理器配置
			      jdbcTemplate/mybatis 使用的是DataSourceTransactionManager
			      jpa规范：JpaTransactionManager
			  -->
			  <bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
			    <property name="entityManagerFactory" ref="entityManagerFactory"/>
			  </bean>
			  ```
		- 声明式事务
		- 配置 spring 包扫描
			- ```xml
			  <context:component-scan base-package="com.lagou.edu"/>
			  ```
		-
	- 使用示例
	  collapsed:: true
		- 继承已有的查询接口
			- 主要有 JpaRepository，JpaSpecificationExecutor 两个接口
		- 使用 `jpql`(简化版 Sql)，进行注解查询
			- ```java
			  @Query("from Resume  where id=?1 and name=?2")
			  public List<Resume> findByJpql(Long id,String name);
			  ```
		- 使用 `Sql`，相比于使用 `jpql` 来说，要将参数 `nativeQuery=true`
			- ```java
			  
			  
			  @Query(value = "select * from tb_resume  where name like ?1 and address like ?2",
			         nativeQuery = true)
			  public List<Resume> findBySql(String name,String address);
			  ```
		- 方法命名规则查询
			- 方法名遵循一定的命名规则就能生成 Sql 语句
			- `findBy`，`deleteBy`，`countBy` 等
			- ```java
			  /**
			    * 方法命名规则查询
			    * 按照name模糊查询（like）
			    *  方法名以findBy开头
			    *          -属性名（首字母大写）
			    *                  -查询方式（模糊查询、等价查询），如果不写查询方式，默认等价查询
			    */
			  public List<Resume> findByNameLikeAndAddress(String name,String address);
			  ```
		- 动态拼接 Sql 查询
			- 在接口 `JpaSpecificationExecutor` 中的方法，可以创建出复杂的查询
			- 利用 `Specification` 接口来创建复杂的查询条件
				- ```java
				  /**
				   * 需求：
				   *   根据name（指定为"张三"）
				   *   并且，address 以"北"开头（模糊匹配），
				   *   查询简历
				   */
				  
				  Specification<Resume> specification = new Specification<Resume>() {
				    @Override
				    public Predicate toPredicate(Root<Resume> root, CriteriaQuery<?> criteriaQuery, CriteriaBuilder criteriaBuilder) {
				      // 获取到name属性
				      Path<Object> name = root.get("name");
				      Path<Object> address = root.get("address");
				      // 条件1：使用CriteriaBuilder针对name属性构建条件（精准查询）
				      Predicate predicate1 = criteriaBuilder.equal(name, "张三");
				      // 条件2：address 以"北"开头（模糊匹配）
				      Predicate predicate2 = criteriaBuilder.like(address.as(String.class), "北%");
				  
				      // 组合两个条件
				      Predicate and = criteriaBuilder.and(predicate1, predicate2);
				  
				      return and;
				    }
				  };
				  
				  Optional<Resume> optional = resumeDao.findOne(specification);
				  ```
		- 分页和排序
			- 利用 `Pageable` 接口下的 `PageRequest` 实现来进行拼接分页条件
				- ```java
				  
				  Sort sort = new Sort(Sort.Direction.DESC,"id");
				  List<Resume> list = resumeDao.findAll(sort);
				  ```
			- 利用 Sort 对象来进行排序
				- ```java
				  
				  /**
				   * 第一个参数：当前查询的页数，从0开始
				   * 第二个参数：每页查询的数量
				   */
				  Pageable pageable = PageRequest.of(0,2);
				  Page<Resume> all = resumeDao.findAll(pageable);
				  ```
- [[Spring Data JPA 源码分析]]
-
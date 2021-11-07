- JDBC 是什么
  collapsed:: true
	- Java语言访问数据库的一种规范,是一套API
	- Java Database Connectivity，Java数据库编程接口
	- 一组标准的Java语言中的接口和类，使用这些接口和类，Java客户端程序可以访问各种不同类型的数据库
- 两个包
  collapsed:: true
	- `java.sql`([[JavaSE]])
	- `javax.sql`([[JavaEE]])
	  collapsed:: true
		- `javax.sql.*` 提供了很多新特性, 是对 `java.sql` 的补充   
		  collapsed:: true
			- Datasource接口提供了一种可选择性的方式去建立连接
			- 提供了连接池的支持
			- 增加了分布式的事务处理机制
			- 增加了rowset
- 基本操作
  collapsed:: true
	- 加载数据库驱动
	- 获取数据库连接
	- 获取预处理对象
	- 设置参数
	- 发送 Sql，获取结果集
	- 从结果集中获取数据，封装数据对象
	- 释放资源
	  collapsed:: true
		- 关闭结果集
		- 关闭预处理对象
		- 关闭连接对象
- 例子
  collapsed:: true
	- collapsed:: true
	  ```java
	   
	  public static void main(String[] args) {
	    
	    Connection connection = null;
	    PreparedStatement preparedStatement = null;
	    ResultSet resultSet = null;
	    try {
	      // 加载驱动
	      Class.forName("com.mysql.jdbc.Driver"); 
	      // 获取连接
	      String url = "jdbc:mysql:///mybatis?characterEncoding=utf-8";
	      connection = DriverManager.getConnection(url, "root", "root");
	      // sql
	      String sql = "select * from user where username = ?";
	      // 获取预处理对象 statement
	      preparedStatement = connection.prepareStatement(sql);
	      // 设置参数
	      preparedStatement.setString(1, "tom");
	      // 发送执行 sql
	      resultSet = preparedStatement.executeQuery(); 
	      // 从结果集中获取数据并处理
	      while (resultSet.next()) {
	        int id = resultSet.getInt("id");
	    	  String username = resultSet.getString("username"); 
	        // 构建结果对象 User
	        user.setId(id);
	        user.setUsername(username);、
	        System.out.println(user);
	      }
	    } catch (Exception e) {
	      e.printStackTrace();
	    } finally { 
	      // 释放资源
	      if (resultSet != null) { 
	        try { 
	          resultSet.close();
	        } catch (SQLException e) { 
	          e.printStackTrace();
	        } 
	      }
	      if (preparedStatement != null) { 
	        try {
	          preparedStatement.close();
	        } catch (SQLException e) {
	          e.printStackTrace();
	        } 
	      }
	  
	      if (connection != null) {
	        try {
	          connection.close();
	        } catch (SQLException e) {}
	      } 
	    }
	  }
	  ```
- 存在的问题
  id:: 617b8df0-fdf4-4f56-8dff-f7c3618762f3
	- 数据库连接的频繁创建释放
	- sql 和代码混淆，硬编码
	- 设置参数不方便，硬编码
	- 数据集解析不方便，硬编码
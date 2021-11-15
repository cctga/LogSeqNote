- ```java
  
  // 第二个参数表示不自动提交
  // 不过单个参数就是默认不自动提交
  // 可以简写为 
  //    SqlSessionFactory.openSession(ExecutorType.BATCH);
  // 是一样的
  SqlSession sqlSession = 
    SqlSessionFactory.openSession(ExecutorType.BATCH, false);
  UserMapper mapper = sqlSession.getMapper(UserMapper.class);
  // 这里应该防止批处理过多，可以分批处理
  userList.forEach(item -> UserMapper::insertOne);
  sqlSession.commit();
  sqlSession.clearCache();
  
  ```
-
- ```java
  
  @Test
  public void batchTest() {
    SqlSession sqlSession = sqlSessionFactory.openSession(ExecutorType.BATCH);
    IUserMapper mapper = sqlSession.getMapper(IUserMapper.class);
    for (int i = 0; i < 200; i++) {
      mapper.addUser(new User(String.valueOf(i)));
    }
    sqlSession.commit();
    sqlSession.clearCache();
  }
  ```
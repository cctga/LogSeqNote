- #+BEGIN_TIP
  这是未经整理的知识
  #+END_TIP
- 问题
	- JDK 9 中 newInstance 显示过时的问题
		- 推荐使用 `clazz.getDeclaredConstructor().newInstance()`
-
-
- 数据库的事务归根结底是 jdbc connection 的事务
- 如果两个 connection ，那肯定不可能控制在同一个事务中，所以如果要给两个操作使用同一个事务，则必须使用同一个 connection，所以这通过同一个线程使用同一个 connection 来解决，也就是把 connection 放在 ThreadLocal 中来解决
-
- ![image.png](../assets/image_1635223989742_0.png){:height 424, :width 766}
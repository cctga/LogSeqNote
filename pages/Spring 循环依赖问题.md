- 无法解决的问题
	- 构造器注入
		- 三级缓存需要获取到构造完成的对象来暴露
	- 多例
		- Spring 只负责创建，不负责管理，所以不使用三级缓存解决
- 三级缓存
	- 一级缓存
		- 初始化全部完成的 Bean
	- 二级缓存
		- 没有进行过属性注入的 Bean
		- 进行过方法增强的代理对象
	- 三级缓存
		- 仅仅调用过构造方法，既没有注入属性 Bean，也没有进行过方法增强
- 三级缓存的过程
	- > A 和 B 循环依赖
	- 创建 A
	- 将创建完成的 A 放入 三级缓存
	- 如果 A 有方法增强则进行增强放入 二级缓存
	- 进行 A 的属性注入，发现 A 需要依赖 B
	- 在 一级缓存 中查找 B，没有
	- 在 二级缓存 中查找 B，没有
	- 在 三级缓存 中查找 B，没有
	- 创建 B
	- 将创建完成的 B 放入三级缓存
	- 如果 B 有方法增强这进行增强放入 二级缓存
	- 进行 B 的依赖注入，发现 B 需要依赖 A
	- 在 一级缓存 中查找 A，没有
	- 在 二级缓存 中查找 A，存在，在 B 中注入 A
	- B 创建完成，将 B 放入 一级缓存
	- 将 B 返回给 A，并完成 A 的依赖属性注入
	- A 创建完成，将 A 放入 一级缓存
- END 创建完成
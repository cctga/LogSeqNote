- 设计原则 7
	- 单一职责原则
	- 开闭原则
	- 依赖倒置原则（面向接口）
	  id:: 616ed63a-72d0-4438-9331-f237f8a228ca
	- 迪米特法则
	- 合成复用原则（组合复用原则）
	- 接口隔离原则
	- 里氏替换原则
- 创建模式 5
	- 单例模式
	- 原型模式
	- 工厂模式 alias:: Facotry
		- 解耦
		  collapsed:: true
			- 工厂可以自己选择一个实现类来实例化返回
			  ```java
			  public class Factory {
			    public static Service getService(){
			      return new ServiceImpl();
			    }
			  }
			  
			  // 使用
			  // 使用者不知道具体的实现类细节
			  // 依赖倒置，面向接口编程
			  // 在修改实现类的时候不影响使用者，只需要修改工厂就好
			  Service service = Factory.getService();
			  ```
	- 抽象工厂
	- 建造者模式
- 结构模式 7
	- 代理模式
		- 静态代理
		- 动态代理
			- ASM
			- CGLIB
			- JDK
			- Javassist
	- 装饰模式
	- 组合模式
	- 适配器模式
	- 桥接模式
	- 外观模式
	- 享元模式
- 行为模式 11
	- 观察者模式
	- 中介者模式
	- 状态模式
	- 命令模式
	- 模版方法模式
	- 策略模式
	- 责任链模式
	- 迭代器模式
	- 访问者模式
	- 备忘录模式
	- 解释器模式
	-
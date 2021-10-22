- 实现原理
	- 由于不同的CPU架构的缓存体系不一样，重排序的策略不一样，所提供的[[内存屏障]]指令也就有差异。
	  这里只探讨为了实现volatile关键字的语义的一种参考做法：
	  （1）在volatile写操作的前面插入一个StoreStore屏障。保证volatile写操作不会和之前的写操作重排序。
	  （2）在volatile写操作的后面插入一个StoreLoad屏障。保证volatile写操作不会和之后的读操作重排序。
	  （3）在volatile读操作的后面插入一个LoadLoad屏障+LoadStore屏障。保证volatile读操作不会和之后的读操作、写操作重排序。
	  具体到x86平台上，其实不会有LoadLoad、LoadStore和StoreStore重排序，只有StoreLoad一种重排序（内存屏障），也就是只需要在volatile写操作后面加上StoreLoad屏障。其他的CPU架构做法不一
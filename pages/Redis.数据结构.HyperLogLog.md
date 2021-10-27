title:: Redis/数据结构/HyperLogLog

-
  title:: Redis/API/数据结构/HyperLogLog
- 使用概率的方法统计一个集合的势，存在误差（很小，0.81%），但是性能很棒
- HyperLogLog 原理可参考 [走近源码：神奇的HyperLogLog](https://zhuanlan.zhihu.com/p/58519480)
- PFADD
	- 添加元素到集合中
	- `PFADD hyperloglog element [element ...]`
	- ```bash
	  PFADD hy 'a' 'b' 'c'
	  (integer) 1	# 新增三个元素的计数，如果产生了变化就返回 1，否则返回 0
	  
	  PFADD hy 'a' 
	  (integer) 0 # 'a' 已经参与了计数，再次计数不产生影响, 也可以看成 a 已经在集合中，添加失败了
	  ```
- PFCOUNT
	- 查看多个集合的 ((61760a24-5050-4747-8559-b5c53eea2680))
	- `PFCOUNT hyperloglog [hyperloglog ...]`
	- ```bash
	  pfcount hy
	  (integer) 3 # 有三个元素
	  
	  pfcount hy2
	  (integer) 3
	  
	  pfcount hy hy2
	  (integer) 5 # 效果为取两者的并集，数值比两者的和要小，说明存在相同元素
	  ```
- PFMERGE
	- 计算并集并存储，可参考 ((6176109b-a4cc-4d22-860f-dd7f22afbe9a))
	- `PFMERGE union_hy hy1 hy2 [hy ...]`
	- ```bash
	  
	  ```
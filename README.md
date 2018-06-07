# SecKillSystem
高并发系统设计思路与实现
## 思路
1. 一般都是读多(读库存)写少的操作，尽量将请求拦截在系统上游  
2. 对于查询库存操作通过缓存实现，减少数据库操作
3. 每次只放指定数量的请求到消息队列，等处理完毕再重新拉请求入队列。
4. 下订单 -> 锁库存 付款 -> 减库存
## 实现
1. 通过Redis实现缓存，每次查询库存都从Redis进行查找(注意缓存雪崩和缓存穿透)
2. 使用RocketMQ作为消息队列
3. SpringBoot(SSM)作为整个项目的框架，相比SpringDataJPA，MyBatis更灵活一些
4. MySQL存放数据，目前分别采用乐观锁、悲观锁进行并发控制。
5. Jmeter进行压力测试
## 代码
1.配置Cache
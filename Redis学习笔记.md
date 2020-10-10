# Redis学习笔记

### Redis特点：

1. Redis支持数据持久化，将数据保存在磁盘中，重启继续再次加载使用
2. Redis不仅仅支持简单的key-value类型的数据,同时提供list,set,zset,hash等数据结构的存储
3. Redis支持数据的备份，即master-slave的数据备份（主从）。

### Redis 优势:

1. 性能极高 读约11w次每秒 写8w次每秒
2. 丰富数据类型 Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作
3. 原子 Redis的所有操作都是原子性的，意思就是要么成功执行要么失败完全不执行。单个操作是原子性的。多个操作也支持事务，即原子性，通过MULTI和EXEC指令包起来。
4. 丰富特性  Redis还支持 publish/subscribe, 通知, key 过期等等特性

### Redis数据类型

- String(字符串)  最大能存储512MB SET GET命令
- Hash(哈希)  适合存储对象  HMSET HGET命令
- List(列表) 添加一个元素 lpush lrange命令
- Set(集合) string类型的无序集合 不允许重复 sadd smembers 命令
- zset(有序集合)  string类型的有序集合 每个元素关联一个double类型的分数，redis通过分数来为集合中的成员从小到大排序。成员唯一分数可以重复。

### Redis命令 


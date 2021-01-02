## Redis 事务

* 开启事务：multi 

* 命令入队 

* 执行事务：exec 

* 撤销事务：discard

> 单线程，没有隔离级别的问题
>
> key的粒度要小

Watch 实现乐观锁，watch 一个key，发生变化则事务失败



## Redis 管道技术

批处理，减少IO操作



## Redis 数据备份与恢复

### RDB

> 类似表的frm文件，全量备份

备份: 执行 `save` 即可在redis数据目录生成数据文件 `dump.rdb`, 也可以异步执行 `bgsave`

恢复：将备份文件 (`dump.rdb`) 移动到 `redis` 数据目录并启动服务即可

### AOF

> 类似MySQL的binlog，增量备份

备份：如果 `appendonly` 配置为 yes，则以 AOF 方式备份 Redis 数据，那么此时 Redis 会按照配置，在特定的时候执行追加命令，用以备份数据。 

```
appendfilename "appendonly.aof" 
appendfsync always 
appendfsync everysec
appendfsync no...... 
```

AOF 文件和 Redis 命令是同步频率的

假设配置为 always，其含义为当 Redis 执行命令的时候，则同时同步到 AOF 文件，这样会使得 Redis 同步刷新 AOF 文件，造成缓慢。而采用 evarysec 则代表每秒同步一次命令到 AOF 文件



## Redis 性能优化

1. 内存优化 ,建议内存在10-20G,再大就需要考虑拆分

https://redis.io/topics/memory-optimization 

hash-max-ziplist-value 64 

zset-max-ziplist-value 64 

2. CPU优化，单线程，卡住就完蛋了
   1. 不要阻塞，特别是Lua脚本
   2. 谨慎使用范围操作（keys在线上绝对不能用）
   3.  SLOWLOG get 10 // 慢查询，默认10毫秒，默认只保留最后的128条



## Redis 使用的一些经验



1. 性能： 
   1. client线程数(8-16)与连接数(默认10000,可以增加)；
   2. 监控系统读写比和缓存命中率

2. 容量：
   1. 做好容量评估，合理使用缓存资源； 
3. 资源管理和分配：
   1. 尽量每个业务集群单独使用自己的Redis，不混用； 
   2. 控制Redis资源的申请与使用，规范环境和Key的管理（以一线互联网为例）； 
   3. 监控CPU ，优化高延迟的操作














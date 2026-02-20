# Redis 增删改查与常用语法详解

## 📊 Redis 数据类型概览

| 数据类型   | 结构    | 特点          | 常用场景        |
| ------ | ----- | ----------- | ----------- |
| String | 字符串   | 最基本类型，二进制安全 | 缓存、计数器、分布式锁 |
| Hash   | 键值对集合 | 适合存储对象      | 用户信息、商品信息   |
| List   | 列表    | 有序、可重复      | 消息队列、最新列表   |
| Set    | 无序集合  | 无序、不可重复     | 标签、共同好友     |
| ZSet   | 有序集合  | 有序、不可重复     | 排行榜、延迟队列    |

---

## 🔤 通用命令

### 1. 键管理

```
# 查看所有键（生产环境慎用，数据量大时会阻塞）
KEYS *                    # 所有键
KEYS user:*               # 匹配模式
KEYS *name*               # 包含name的键（模糊查询）

# 判断键是否存在
EXISTS key1 key2          # 返回存在的键数量，没有返回0

# 删除键
DEL key1 key2             # 删除指定键,返回成功删除的键的数量
UNLINK key1              # 异步删除（Redis 4.0+）返回被成功解除链接的键的数量

# 设置过期时间
EXPIRE key 60            # 60秒后过期,查询和写入优先删除，查询返回nil
EXPIREAT key 1672531199  # 指定时间戳过期
TTL key                  # 查看剩余时间（秒）
PTTL key                 # 查看剩余时间（毫秒）

# 取消过期时间
PERSIST key              # 移除过期时间

# 重命名键
RENAME oldkey newkey
RENAMENX oldkey newkey   # 仅当新键不存在时重命名

# 移动键到其他数据库
MOVE key 1               # 移动到数据库1

# 随机返回一个键
RANDOMKEY

# 扫描键（替代KEYS，不会阻塞）
SCAN 0 MATCH user:* COUNT 10
```
异步删除:[redis中UNLINK key1             ...](obsidian://deepseek-ai-assistant?id=1769060793988 "Open plugin:deepseek-ai-assistant")
过期键:[redis中的键过期后会怎么样](obsidian://deepseek-ai-assistant?id=1769061741849 "Open plugin:deepseek-ai-assistant")
### 2. 数据库操作

```
# 选择数据库（0-15）
SELECT 1

# 清空当前数据库
FLUSHDB
FLUSHDB ASYNC          # 异步清空（Redis 4.0+）

# 清空所有数据库
FLUSHALL
FLUSHALL ASYNC         # 异步清空

# 查看数据库大小(键的个数)
DBSIZE

# 查看键类型
TYPE key
```

---

## 📝 String（字符串）

### 1. 基本操作

```
# 设置值
SET name "张三"          # 设置字符串
SET age 25             # 设置数字
SET counter 0
SETNX name "李四"       # 仅当键不存在时设置（原子性，实现分布式锁）
SETEX name 60 "张三"    # 设置值并指定过期时间（秒）
PSETEX name 60000 "张三" # 设置值并指定过期时间（毫秒）
SETNX  key  value  #(原子操作)只有不存在key时才会成功返回1，否则为0。


# 批量设置
MSET k1 v1 k2 v2 k3 v3   #覆盖已经存在的键
MSETNX k1 v1 k2 v2      # 原子操作，全部成功或全部失败,不覆盖已经存在的键

# 获取值
GET name               #单个获取
MGET k1 k2 k3          # 批量获取

# 获取并设置（原子操作）
GETSET counter 1       # 返回旧值，设置新值

# 获取字符串长度
STRLEN name

# 获取子字符串(左右都是闭区间)
GETRANGE name 0 3      # 获取索引0-3的字符

# 追加字符串
APPEND name "先生"      # 返回新长度


```

### 2. 数字操作

```
# 自增自减
INCR counter           # 自增1
INCRBY counter 5       # 自增5
DECR counter           # 自减1
DECRBY counter 3       # 自减3

# 浮点数操作
INCRBYFLOAT price 1.5  # 增加浮点数

# 位操作
SETBIT online_user:20240101 100 1  # 设置第100位为1
GETBIT online_user:20240101 100    # 获取第100位
BITCOUNT online_user:20240101      # 统计1的个数
BITOP AND result key1 key2         # 位与操作
```

---

## 🏷️ Hash（哈希表）

### 1. 基本操作

```
# 设置字段
HSET user:1 name "张三" age 25 city "北京"
HSETNX user:1 email "zhangsan@example.com"  # 字段不存在时设置

# 获取字段
HGET user:1 name
HMGET user:1 name age city        # 批量获取
HGETALL user:1                    # 获取所有字段和值
HKEYS user:1                      # 获取所有字段
HVALS user:1                      # 获取所有值

# 删除字段
HDEL user:1 city

# 判断字段是否存在
HEXISTS user:1 name

# 获取字段数量
HLEN user:1

# 递增数字字段
HINCRBY user:1 age 1
HINCRBYFLOAT user:1 score 0.5

# 字段值长度
HSTRLEN user:1 name
```

### 2. 高级操作

```
# 扫描哈希字段
HSCAN user:1 0 MATCH n* COUNT 10

# 原子操作
HSET user:1 name "张三" age 25  # 原子性设置多个字段
```

---

## 📋 List（列表）

### 1. 基本操作

```
# 左侧插入
LPUSH tasks "task1" "task2" "task3"  # 结果：task3 task2 task1
LPUSHX tasks "task4"                 # 仅当列表存在时插入

# 右侧插入
RPUSH tasks "task4" "task5"         # 结果：task1 task2 task3 task4 task5
RPUSHX tasks "task6"                # 仅当列表存在时插入

# 指定位置插入
LINSERT tasks BEFORE "task2" "newtask"  # 在task2前插入
LINSERT tasks AFTER "task2" "newtask"   # 在task2后插入

# 获取元素
LRANGE tasks 0 -1                    # 获取所有元素
LRANGE tasks 0 2                     # 获取前3个
LINDEX tasks 2                       # 获取索引2的元素

# 弹出元素
LPOP tasks                          # 左侧弹出
RPOP tasks                          # 右侧弹出
BLPOP tasks 30                      # 阻塞式左侧弹出，等待30秒
BRPOP tasks 30                      # 阻塞式右侧弹出

# 修改元素
LSET tasks 1 "updated_task"         # 设置索引1的元素

# 删除元素
LREM tasks 2 "task1"                # 删除2个值为task1的元素
LREM tasks 0 "task1"                # 删除所有值为task1的元素
LREM tasks -2 "task1"               # 从右边开始删除2个值为task1的元素

# 修剪列表
LTRIM tasks 1 3                     # 只保留索引1-3的元素

# 列表长度
LLEN tasks

# 移动元素
RPOPLPUSH source destination        # 从source弹出，推入destination
BRPOPLPUSH source destination 30    # 阻塞版本
```

---

## 🔄 Set（集合）

### 1. 基本操作

```
# 添加元素
SADD tags "java" "python" "redis" "mysql"
SADD tags "java"                     # 重复元素不添加

# 获取所有元素
SMEMBERS tags

# 判断元素是否存在
SISMEMBER tags "java"

# 删除元素
SREM tags "mysql"

# 随机获取元素
SPOP tags                           # 随机弹出一个元素
SPOP tags 2                         # 随机弹出2个元素
SRANDMEMBER tags                    # 随机获取但不弹出
SRANDMEMBER tags 3                  # 随机获取3个元素

# 集合大小
SCARD tags

# 移动元素
SMOVE source dest "element"         # 移动元素到另一个集合
```

### 2. 集合运算

```
# 差集
SDIFF set1 set2                     # set1 - set2
SDIFF set1 set2 set3                # set1 - (set2 ∪ set3)

# 交集
SINTER set1 set2
SINTER set1 set2 set3

# 并集
SUNION set1 set2
SUNION set1 set2 set3

# 存储运算结果
SDIFFSTORE result set1 set2         # 差集存储到result
SINTERSTORE result set1 set2        # 交集存储
SUNIONSTORE result set1 set2        # 并集存储
```

### 3. 扫描集合

```
SSCAN tags 0 MATCH j* COUNT 10
```

---

## 🏆 ZSet（有序集合）

### 1. 基本操作

```
# 添加元素
ZADD rank 100 "Alice" 200 "Bob" 150 "Charlie"
ZADD rank NX 120 "David"           # 仅当元素不存在时添加
ZADD rank XX CH 110 "Alice"        # 仅当元素存在时更新，返回修改数量
ZADD rank INCR 10 "Alice"          # 递增分数

# 获取元素
ZRANGE rank 0 -1                   # 按分数升序获取所有
ZRANGE rank 0 -1 WITHSCORES        # 带分数
ZREVRANGE rank 0 -1                # 按分数降序获取
ZRANGEBYSCORE rank 100 200         # 获取分数在100-200之间的元素
ZRANGEBYSCORE rank 100 200 WITHSCORES LIMIT 0 5

# 获取排名
ZRANK rank "Alice"                 # 升序排名（从0开始）
ZREVRANK rank "Alice"              # 降序排名

# 获取分数
ZSCORE rank "Alice"

# 获取指定分数范围内的元素数量
ZCOUNT rank 100 200

# 删除元素
ZREM rank "Bob"
ZREMRANGEBYRANK rank 0 2           # 删除排名0-2的元素
ZREMRANGEBYSCORE rank 100 200      # 删除分数在100-200之间的元素

# 集合大小
ZCARD rank
```

### 2. 集合运算

```
# 交集
ZINTERSTORE result 2 set1 set2 WEIGHTS 1 0.5 AGGREGATE SUM
# WEIGHTS: 权重
# AGGREGATE: SUM|MIN|MAX

# 并集
ZUNIONSTORE result 2 set1 set2 WEIGHTS 1 0.5 AGGREGATE SUM
```

---

## 📁 发布订阅

```
# 订阅频道
SUBSCRIBE news sports
PSUBSCRIBE news.*                 # 模式订阅

# 发布消息
PUBLISH news "今日新闻"
PUBLISH news.sports "体育新闻"

# 取消订阅
UNSUBSCRIBE news
PUNSUBSCRIBE news.*

# 查看订阅
PUBSUB CHANNELS                   # 查看活跃频道
PUBSUB NUMSUB news sports         # 查看频道订阅数
PUBSUB NUMPAT                     # 查看模式订阅数
```

---

## 🔄 事务操作

```
# 开启事务
MULTI

# 命令入队
SET name "张三"
INCR counter
HSET user:1 age 25

# 执行事务
EXEC

# 放弃事务
DISCARD

# 监视键（乐观锁）
WATCH key1 key2
MULTI
# ... 操作
EXEC  # 如果被监视的键在事务执行前被修改，事务失败

# 取消监视
UNWATCH
```

---

## 📡 管道与脚本

### 1. 管道（Pipeline）

```
# 一次发送多个命令，减少网络开销
# Redis客户端示例（Python）
import redis
r = redis.Redis()
pipe = r.pipeline()
pipe.set('key1', 'value1')
pipe.set('key2', 'value2')
pipe.get('key1')
results = pipe.execute()
```

### 2. Lua脚本

```
# 执行Lua脚本
EVAL "return redis.call('GET', KEYS[1])" 1 mykey
EVAL "return {KEYS[1],KEYS[2],ARGV[1],ARGV[2]}" 2 key1 key2 first second

# 加载脚本
SCRIPT LOAD "return redis.call('GET', KEYS[1])"
EVALSHA sha1 1 mykey

# 检查脚本
SCRIPT EXISTS sha1 sha2
SCRIPT FLUSH   # 清空脚本缓存
SCRIPT KILL    # 杀死正在运行的脚本
```

---

## 🎯 实用示例

### 示例1：用户会话管理

```
# 用户登录
SETEX session:user123 3600 '{"user_id":123,"username":"张三"}'

# 验证会话
GET session:user123

# 更新会话
EXPIRE session:user123 7200

# 用户登出
DEL session:user123
```

### 示例2：商品库存

```
# 设置库存
SET product:1001:stock 100

# 减少库存（原子操作）
DECR product:1001:stock

# 检查库存
GET product:1001:stock
```

### 示例3：最新文章列表

```
# 发布文章
LPUSH articles:latest "article:1001"
LTRIM articles:latest 0 9  # 只保留最近10条

# 获取最新文章
LRANGE articles:latest 0 9
```

### 示例4：用户标签

```
# 添加标签
SADD user:123:tags "技术" "编程" "Redis"

# 获取共同标签
SINTER user:123:tags user:456:tags

# 推荐相关标签
SUNIONSTORE recommendation 3 user:123:tags user:456:tags user:789:tags
SDIFF recommendation user:123:tags
```

### 示例5：文章排行榜

```
# 文章投票
ZINCRBY article:votes 1 "article:1001"

# 获取排行榜
ZREVRANGE article:votes 0 9 WITHSCORES

# 获取前10%的文章
ZCARD article:votes
ZREVRANGE article:votes 0 [计算前10%的数量] WITHSCORES
```

---

## 🚨 注意事项

### 1. 性能优化

```
# 使用批量操作
MGET key1 key2 key3
MSET key1 val1 key2 val2

# 使用管道
# 客户端实现

# 合理设置过期时间
SETEX key 60 value
```

### 2. 内存优化

```
# 使用合适的数据类型
# 小对象用String，大对象用Hash

# 使用压缩列表配置
# 在redis.conf中配置
hash-max-ziplist-entries 512
hash-max-ziplist-value 64
```

### 3. 安全考虑

```
# 设置密码
# redis.conf: requirepass yourpassword

# 重命名危险命令
# redis.conf: rename-command FLUSHALL ""

# 限制网络访问
# redis.conf: bind 127.0.0.1
```

### 4. 监控命令

```
# 查看慢查询
SLOWLOG GET 10
CONFIG SET slowlog-log-slower-than 10000  # 设置10ms为慢查询

# 查看客户端
CLIENT LIST
CLIENT SETNAME myclient
CLIENT GETNAME

# 查看内存
INFO memory
MEMORY USAGE key
```

---

## 📊 命令速查表

|操作|String|Hash|List|Set|ZSet|
|---|---|---|---|---|---|
|添加|SET|HSET|LPUSH/RPUSH|SADD|ZADD|
|获取|GET|HGET|LRANGE/LINDEX|SMEMBERS|ZRANGE|
|删除|DEL|HDEL|LPOP/RPOP/LREM|SREM|ZREM|
|修改|SET|HSET|LSET|无直接修改|ZADD|
|计数|STRLEN|HLEN|LLEN|SCARD|ZCARD|
|存在判断|EXISTS|HEXISTS|无|SISMEMBER|无|
|范围查询|GETRANGE|无|LRANGE|无|ZRANGEBYSCORE|
|递增|INCR|HINCRBY|无|无|ZINCRBY|

---

## 💡 最佳实践

1. **键名规范**：使用冒号分隔，如 `user:123:profile`
    
2. **设置过期时间**：避免内存泄漏
    
3. **使用合适的数据类型**：根据场景选择
    
4. **避免大键**：单个键不宜过大
    
5. **使用管道**：减少网络往返
    
6. **监控慢查询**：及时发现性能问题
    
7. **备份重要数据**：定期执行 `BGSAVE`
    
8. **使用连接池**：避免频繁创建连接
    

这个指南涵盖了Redis最常用的增删改查操作和语法。掌握这些命令，你就能应对大多数Redis使用场景。
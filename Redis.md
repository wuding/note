# Redis

https://redis.io/

~~http://www.redis.cn/~~



## 文档

- https://www.runoob.com/redis/redis-tutorial.html

- http://doc.redisfans.com/
- https://github.com/huangz1990/redis => http://redisdoc.com/
- [《Redis 设计与实现》](https://github.com/huangz1990/redisbook)



## 下载安装

### Windows

- https://github.com/tporadowski/redis
- https://github.com/microsoftarchive/redis 3.2.100
- https://github.com/ServiceStack/redis-windows 3.0.503

#### 目录文件说明

|                            |                |                                                              |
| -------------------------- | -------------- | ------------------------------------------------------------ |
| redis-server.exe           | 服务端程序     | 提供redis服务                                                |
| redis-cli.exe              | 客户端程序     | 通过它连接redis服务并进行操作                                |
| redis-check-dump.exe       | 本地数据库检查 |                                                              |
| redis-check-aof.exe        | 更新日志检查   | 用以模拟同时由N个客户端发送M个 SETs/GETs <br>查询 (类似于 Apache 的ab 工具) |
| redis-benchmark.exe        | 性能测试       |                                                              |
| redis.windows.conf         | 配置文件       | 将redis作为普通软件使用的配置，命令行关闭则redis关闭         |
| redis.windows-service.conf | 配置文件       | 将redis作为系统服务的配置，用以区别开两种不同的使用方式      |



#### 测试

启动 redis

```bash
redis-server.exe redis.windows.conf
```



#### 服务

- https://github.com/alfishe/redis-service

- https://github.com/kcherenkov/redis-windows-service

##### 注册服务

```bash
redis-server.exe --service-install redis.windows.conf --loglevel verbose
```

##### 删除服务

```bash
redis-server --service-uninstall
```

##### 开启服务

```bash
redis-server --service-start
```

##### 停止服务

```bash
redis-server --service-stop
```

##### 重命名服务

```bash
redis-server --service-name name

redis-server --service-install --service-name redisService3 --port 10003
redis-server --service-start --service-name redisService3
```

##### 可执行文件的路径

```bash
"C:\Program Files\Redis\redis-server.exe" --service-run "C:\Program Files\Redis\redis.windows-service.conf"
```



#### 工具

- [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager/releases/tag/0.8.8) 最后的免费版本 0.8.8
- https://github.com/cinience/RedisStudio
- https://github.com/qishibo/AnotherRedisDesktopManager
- https://github.com/lework/RedisDesktopManager-Windows
- https://github.com/kanyways/rdm
- https://github.com/FuckDoctors/rdm-builder
- [通过源码自己编译 Windows 版本](https://github.com/lloy1231/RedisDesktopManager-Windows)



### Windows Subsystem for Linux（WSL）

- [如何在Windows系统下优雅地运行Redis](https://zhuanlan.zhihu.com/p/56374534)



## Client

### redis-cli

cmd 运行

```bash
redis-cli.exe -h 127.0.0.1 -p 6379 -a test123
```

测试

```bash
set name value
get name
```



### PHP

- https://github.com/phpredis/phpredis => https://pecl.php.net/package/redis

php.ini
```ini
extension=redis
```

示例
```php
$redis = new Redis();
$redis->connect('127.0.0.1', 6379);
$redis->auth('redis3.2.100');
$redis->set('key', 'value');
echo $redis->get('key');
```

- https://github.com/nrk/predis



#### 工具

- https://github.com/erikdubbelboer/phpRedisAdmin



## 配置

### 密码

配置文件

```
requirepass foobared
```

命令行

```bash
config set requirepass test123
config get requirepass

# 密码验证
auth test123
```


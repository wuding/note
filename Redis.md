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

- https://github.com/microsoftarchive/redis
- https://github.com/ServiceStack/redis-windows

#### 服务

- https://github.com/alfishe/redis-service

- https://github.com/kcherenkov/redis-windows-service

#### 工具

- [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager/releases/tag/0.8.8) 最后的免费版本 0.8.8

- https://github.com/cinience/RedisStudio
- https://github.com/qishibo/AnotherRedisDesktopManager
- https://github.com/lework/RedisDesktopManager-Windows
- https://github.com/lloy1231/RedisDesktopManager-Windows

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



#### Windows Subsystem for Linux（WSL）

- [如何在Windows系统下优雅地运行Redis](https://zhuanlan.zhihu.com/p/56374534)



## Windows 服务

注册服务


redis-server.exe --service-install redis.windows.conf



删除服务


redis-server --service-uninstall



开启服务


redis-server --service-start



停止服务


redis-server --service-stop



## Client

### redis-cli

cmd 运行

```bash
redis-cli.exe -h 127.0.0.1 -p 6379
```

测试

```bash
set name value
get name
```



### PHP

- https://github.com/phpredis/phpredis
- https://github.com/nrk/predis

#### 工具

- https://github.com/erikdubbelboer/phpRedisAdmin
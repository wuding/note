# FastRoute 使用指南

https://github.com/nikic/FastRoute



## 安装 FastRoute

```sh
composer require nikic/fast-route
```



## 添加路由规则

```php
addRoute($httpMethod, $route, $handler)
```

#### $httpMethod

方法： `GET`, `POST`, `PUT`, `PATCH`, `DELETE` and `HEAD`

支持数组：['GET', 'POST']



#### $route

路由模式

1. 绝对完整：/users
2. 包含路径分隔符：/user/{name:.+}
3. 参数：/user/{id:\d+}
4. 可选：/articles/{id:\d+}[/{title}]




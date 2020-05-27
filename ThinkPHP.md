# ThinkPHP

[ThinkPHP入门系列](https://www.kancloud.cn/special/thinkphp5_quickstart)：

- [ThinkPHP6.0完全开发手册](https://www.kancloud.cn/manual/thinkphp6_0)

- [ThinkPHP6.0入门必读](https://www.kancloud.cn/thinkphp/thinkphp6-quickstart)

PDF 下载：

- [ThinkPHP5.0入门实例教程](https://pan.baidu.com/s/1Pa3OmJRqqQDi-qdK5povWQ) - 4rwq

- [ThinkPHP5.0快速入门](https://pan.baidu.com/s/1ndvKlkjCqigm6e33ryr5ng) - qpep

- [ThinkPHP完全开发手册](https://pan.baidu.com/s/1cRz4G09FhfgcenVVEaqcpw) - 7bae



## 基础

### 安装

#### 环境要求

PHP >= 7.1.0

#### 命令

```bash
# 第一次安装
composer create-project topthink/think tp
composer create-project topthink/think=6.0.x-dev tp

# 更新
composer update topthink/framework
```

#### 测试运行

```bash
# 显示版本
php think version

# 默认端口 8000
php think run

# 设置端口
php think run -p 80
```

http://localhost:8000/



### 配置

#### 环境变量文件 .env

```bash
copy .example.env .env
```
##### 获取环境变量

```php
use think\facade\Env;

Env::get('database.username', 'root');
```

#### 配置文件

```php
use think\facade\Config;

// 批量设置参数
Config::set(['name1' => 'value1', 'name2' => 'value2'], 'config');

// 获取配置
Config::get('config');
Config::get('config.name1');

// 判断是否存在
Config::has('template');
```



## 架构

### 多应用模式

#### 安装多应用模式扩展

```bash
composer require topthink/think-multi-app
```

#### 默认应用

app.php

```php
// 默认应用
'default_app' => 'home',

// 开启应用快速访问
'app_express' => true,
```

#### 增加应用入口

http://servername/test.php

```php
namespace think;

require __DIR__ . '/../vendor/autoload.php';

// 执行HTTP应用 admin 并响应
$http = (new  App())->http;
$response = $http->name('admin')->run();
$response = $http->path('path/to/app')->run(); // 对于非自动多应用部署的情况
$response->send();
$http->end($response);
```

#### 获取当前应用

```php
app('http')->getName();
```

#### 应用映射

```php
'app_map' => [
    'think' =>  'admin',
    'home'  =>  'index',
    '*'     =>  'index',
    'test' => function($app) {
        $app->http->path('path/to/composer/app'); // 使用composer加载应用
    },
],
```

#### 域名绑定应用

```php
'domain_bind' => [
    'blog'        =>  'blog',  // blog子域名绑定到blog应用
    'shop.tp.com' =>  'shop',  // 完整域名绑定
    '*'           =>  'home',  // 二级泛域名绑定到home应用
],
```

#### 禁止应用访问

```php
'deny_app_list' => ['common']
```





## 路由

http://servername/index.php/think

```php
Route::get('think', function () {
    return 'hello,ThinkPHP!';
});
```

### 路由定义

#### 注册路由

```php
// 注册路由到News控制器的read操作
Route::rule('new/:id','News/read');

Route::rule('new/:id', 'News/update', 'GET|POST');
```

http://servername/new/5 会自动路由到 http://servername/news/read/id/5

```php
// 快捷注册方法：get post put patch delete any
Route::any('new/:id','News/read');
```

#### 规则表达式

```php
Route::rule('/', 'index'); // 首页访问路由
Route::rule('my', 'Member/myinfo'); // 静态地址路由
Route::rule('blog/:id', 'Blog/read'); // 静态地址和动态地址结合
Route::rule(':user/:blog_id', 'Blog/read'); // 全动态地址

// 可选变量
Route::get('blog/:year/[:month]','Blog/archive');
// 或者
Route::get('blog/<year>/<month?>','Blog/archive');

// 完全匹配
Route::get('new/:cate$', 'News/category');
```

#### 路由标识

```php
// 指定生成标识
Route::rule('new/:id','News/read')
    ->name('new_read');

// 生成路由地址
url('new_read', ['id' => 10]);

// 默认使用路由地址作为路由标识
url('News/read', ['id' => 10]);
```



## 杂项

### 缓存

#### 设置缓存有效期

```php
cache('name', $value, 3600); // 助手函数
Cache::set('name', $value, 3600);
Cache::set('name', $value, new DateTime('2019-10-01 12:00:00'));
```

#### 不存在则写入缓存数据

```php
Cache::remember('start_time', time());
Cache::has('start_time'); // 仅判断
```

#### 缓存自增自减

```php
Cache::set('name', 1);
Cache::inc('name');
Cache::inc('name', 3);
Cache::dec('name');
Cache::get('name', ''); // 支持指定默认值
cache('name');
```

#### 追加一个缓存数据

```php
Cache::set('name', [1,2,3]);
Cache::push('name', 4);
```

#### 删除缓存

```php
cache('name', NULL);
Cache::delete('name');
Cache::pull('name'); // 获取并删除
Cache::clear(); // 清空缓存
```

#### 切换缓存类型

```php
Cache::store('redis')->set('name','value',3600);
Cache::store('default')->set('name','value',3600);
```



## 扩展库

### Workerman

https://github.com/top-think/think-worker

#### 安装

php.ini

```ini
extension=fileinfo
```

安装命令

```bash
composer require topthink/think-worker
```

#### HttpServer

```bash
# 启动服务端
php think worker

# linux下面可以支持下面指令
php think worker [start|stop|reload|restart|status]
```

http://localhost:2346/

#### SocketServer

```bash
php think worker:server
```

config/worker_server.php

```php
return [
	'socket' 	=>	'http://127.0.0.1:8000',
	'name'		=>	'thinkphp',
	'count'		=>	4,
	'onMessage'	=>	function($connection, $data) {
		$connection->send(json_encode($data));
	},
];
```

##### 自定义类

```php
<?php
namespace app\http;

use think\worker\Server;

class Worker extends Server
{
	protected $socket = 'http://0.0.0.0:2346';

	public function onMessage($connection,$data)
	{
		$connection->send(json_encode($data));
	}
}
```

config/worker_server.php

```php
return [
	'worker_class'	=>	'app\http\Worker',
];
```

http://localhost:2346/



## 附录

### 助手函数

#### 应用目录获取

| 获取方法       | 目录位置   | 目录说明       |
| -------------- | ---------- | -------------- |
| root_path()    | 根目录     | 项目所在的目录 |
| base_path()    | 基础目录   | app目录        |
| app_path()     | 应用目录   | app/应用子目录 |
| config_path()  | 配置目录   | config         |
| runtime_path() | 运行时目录 | runtime        |



## 参考

- [狂奔的蜗牛Snails 随笔分类 - Thinkphp6](https://www.cnblogs.com/wesky/category/1692269.html)
- [《Thinkphp5使用Socket服务》 入门篇](https://www.jianshu.com/p/5cfb386978f8)

  - https://www.cnblogs.com/guaiyouyisi/p/9530295.html
- [TP5 集成 WorkerMan 以及 GatewayWorker 做实时聊天](https://www.jianshu.com/p/84863b101e9c)
- [使用 workerMan 搭建一个简单的聊天室](https://gitee.com/goto8848/build_a_simple_chat_room_with_workerman)
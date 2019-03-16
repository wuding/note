Laravel 开发手册
=======

##### 参考：

- [2017 版 Laravel 系列入门教程](https://github.com/johnlui/Learn-Laravel-5)
- https://github.com/OMGZui/noteBook/tree/master/php/laravel
- [Laravel 完整开源项目大全](https://laravelacademy.org/laravel-project)
- https://github.com/summerblue/laravel5-cheatsheet | https://cs.laravel-china.org/
- https://github.com/summerblue/laravel-shop
- https://github.com/z-song/laravel-admin
- [Laravel 5.8 中文文档](https://learnku.com/docs/laravel/5.8)
- [上线清单 —— 20 个 Laravel 应用性能优化项](https://learnku.com/laravel/t/24559)



## 目录

[**安装**](#安装)

[**命令行**](#命令行)

make

db

migrate

key

vendor

[**配置**](#配置)

环境变量

数据库

应用配置

路由

[**控制器**](#控制器)

[**多模块开发**](#多模块开发)

[**数据库**](#数据库)

读写分离

多库连接

Migration

Seeder

[**模型**](#模型)

类定义

属性

方法

[**视图**](#视图)

布局

区块

分页

赋值

循环

条件

[**函数**](#函数)

[**异常**](#异常)

错误日志

[**常见问题**](#常见问题)



## 安装

```sh
composer create-project laravel/laravel learnlaravel5 ^5.5
```

设置文件夹权限
```
chmod 777 -R storage bootstrap/cache
```



## Artisan 命令行

##### 参考：
[Artisan 命令行](https://laravel-china.org/docs/laravel/5.7/artisan/2276)
[Laravel Artisan常用命令](https://www.jianshu.com/p/f6e1f9322e2d)

```sh
# 所有命令列表
php artisan list

# 命令帮助
php artisan help migrate
```



### make

#### middleware

```sh
php artisan make:auth
php artisan make:middleware XXX
```



#### model

```sh
php artisan make:model Article
php artisan make:model Comment -m
# linux or macOs 加上转义符
php artisan make:Model App\\Models\\User
# 创建表及其迁移
php artisan make:model --migration Post
```



#### migration

```sh
# 创建迁移
php artisan make:migration create_articles_table
# 指定路径
php artisan make:migration --path=app\providers create_users_table
```



#### seeder

```sh
# 创建要填充的数据类
php artisan make:seeder ArticleSeeder
```



#### controller

```sh
php artisan make:controller ArticleController
# REST 风格
php artisan make:controller CommentController --resource
php artisan make:controller Admin/HomeController
```



#### request

主要用于表单验证

app/Http/Requests/

```sh
php artisan make:request TagCreateRequest
```



#### console

app/Console/Commands/

```sh
php artisan make:console TopicMakeExcerptCommand --command=topics:excerpt123
php artisan make:command SendEmails
```
在 app/Console/Kernel.php 文件里面，添加以下
```php
protected $commands = [
    \App\Console\Commands\TopicMakeExcerptCommand::class,
];
```



### db

```sh
# 数据填充（全部表）
php artisan db:seed

# 指定要填充的表
php artisan db:seed --class=UsersTableSeeder
```



### migrate

```sh
# 数据迁移
php artisan migrate
```



### key

```sh
php artisan key:generate
```



### vendor

```sh
# 自定义分页视图
php artisan vendor:publish --tag=laravel-pagination
```



### route

```sh
# 查看所有路由
php artisan route:list
```



## 配置

### 环境变量

.env

```ini
# 应用
APP_NAME=Laravel
APP_ENV=local
APP_DEBUG=true
APP_URL=http://localhost
APP_KEY=base64:+US9WuCFNDh0EMF38Ud6bZDO3McW+58f5iipZvTZF/s=

# 数据库
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel5
DB_USERNAME=root
DB_PASSWORD=root
```



### 数据库

config/database.php

database/



### 应用配置

config\app.php

```php
return [
    'name' => env('APP_NAME', 'Laravel'),
    'env' => env('APP_ENV', 'production'),
    'debug' => env('APP_DEBUG', false),
    'url' => env('APP_URL', 'http://localhost'),
    'asset_url' => env('ASSET_URL', null),
    'timezone' => 'UTC',
    'locale' => 'en',
    'fallback_locale' => 'en',
    'faker_locale' => 'en_US',
    'key' => env('APP_KEY'),
    'chipher' => 'AES-256-CBC',
    'providers' => [
        /*
         * Laravel Framework Service Providers...
         */
        Illuminate\Auth\AuthServiceProvider::class,
        
        /*
         * Package Service Providers...
         */

        /*
         * Application Service Providers...
         */
        App\Providers\AppServiceProvider::class,
	],
    'aliases' => [
        'App' => Illuminate\Support\Facades\App::class,
    ],
 ];
```



### 路由

routes/web.php

```php
Route::group(['middleware' => 'auth', 'namespace' => 'Admin', 'prefix' => 'admin'], function() {
	Route::get('/', 'HomeController@index');
});

Route::has('login')
    
Auth::routes();

Route::get('/home', 'HomeController@index')->name('home');
```

##### 参考：

[路由](https://laravel-china.org/docs/laravel/5.7/routing/2253)



## 控制器

app/Http/Controllers/

#### 控制器原型类

```php
use Illuminate\Routing\Controller as BaseController; //abstract
use Illuminate\Foundation\Auth\Access\AuthorizesRequests;
use Illuminate\Foundation\Bus\DispatchesJobs;
use Illuminate\Foundation\Validation\ValidatesRequests;

class Controller extends BaseController
{
    use AuthorizesRequests, DispatchesJobs, ValidatesRequests; //trait
}
```

#### 控制器类

```php
use Illuminate\Http\Request; //HTTP请求

class HomeController extends Controller
{
    public function __construct()
    {
        $this->middleware('auth');
    }
    
    public function index(Request $request)
    {
        $data = [
            'input' => $request->input('i', 'in'),
            'query' => $request->query('q', 'qu'),
        ];
        return view('home.index', $data); //视图
    }
}
```

#### 请求原型类

```php
namespace Illuminate\Http;

use ArrayAccess;
use Illuminate\Contracts\Support\Arrayable;
use Symfony\Component\HttpFoundation\Request as SymfonyRequest;

class Request extends SymfonyRequest implements Arrayable, ArrayAccess
{
}
```

##### 参考：

[控制器](https://laravel-china.org/docs/laravel/5.7/controllers/2256)

[Laravel资源控制器处理的动作](https://blog.csdn.net/h330531987/article/details/78789588)



## 多模块开发

##### 参考：

[基于Laravel5.5的模块化开发](https://segmentfault.com/a/1190000011552359) => https://gitee.com/techlee/laravel5.5-modules-demo

[Laravel 模块化开发](https://laravel-china.org/articles/6153/laravel-modular-development) => https://github.com/nWidart/laravel-modules

[使用 Laravel-Modules 扩展包通过模块化开发大型 Laravel 应用](https://laravelacademy.org/post/7924.html)

[Laravel 怎么实现分模块，求大牛详解](https://laravel-china.org/topics/2687/laravel-how-to-achieve-the-sub-module-seeking-daniel-explain) => https://github.com/caffeinated/modules

[基于 Laravel 5 构建的、支持模块化和多语言的 CMS —— AsgardCMS](https://laravelacademy.org/post/6417.html) => https://github.com/AsgardCms







## 数据库

##### 参考：

[数据库：入门](https://laravel-china.org/docs/laravel/5.7/database/2288)



定义

```php
use Illuminate\Support\Facades\DB;
```
使用
```php
DB::select($sql, [$id]);
```



### 读写分离

```php
'mysql' => [
    'read' => [
        'host' => ['192.168.1.1'],
    ],
    'write' => [
        'host' => ['196.168.1.2'],
    ],
    'sticky'    => true, // 可选项 请求周期内执行过写操作，那么读操作将使用写连接
],
```

#### 参考：

[Laravel 5 配置读写分离和源码分析](https://laravel-china.org/topics/1879/laravel-5-configuration-read-and-write-separation-and-source-analysis)

[Laravel 数据库读写分离](https://www.cnblogs.com/yingnan/p/5884363.html)



### 多库连接

```php
// 连接方法
$users = DB::connection('foo')->select(...);

// 实例
$pdo = DB::connection()->getPdo();
```



### 数据表

```php
DB::table($table_name)
from($table_name)
```



### 表连接

```php
join($table, $column, '=', $field) //Inner Join
join('contacts', function ($join) {
    $join->on('users.id', '=', 'contacts.user_id')->orOn(...);
}) //高级 Join 语句
leftJoin()
crossJoin()
```



### 表联合

```php
$first = Db::table('users');
union($first)
unionAll()
```



### 原生表达式方法

```php
DB::raw($sql_str) //转换部分
selectRaw($sql_str, $arg_arr) //方法
whereRaw()
orWhereRaw()
havingRaw()
orHavingRaw()
orderByRaw()
```



### Migration



### Seeder



## 模型

### 类定义

```php
namespace App;

use Illuminate\Database\Eloquent\Model; //abstract

class DbTable extends Model
{
    protected $connection = 'mysql';
    protected $table = 'test';
    
    protected $fillable = [
        'name', 'email', 'password',
    ];

    protected $hidden = [
        'password', 'remember_token',
    ];
    
    public function __construct(array $attributes = [])
    {
        parent::__construct($attributes);
    }
}
```



### 属性

多库连接

```php
// 模型里添加属性
protected $connection = 'mysql';
```



### 方法

#### 指定

```php
select($col, $col2...) //字段
addSelect($column) //添加字段
distinct() //不重复的结果
```



#### 查询

```php
where($field, $value)
where($field, '>', $value)
where([
    [$field, '>', $value],
])
where('preferences->dining->meal', 'salad') //JSON 支持
    
orWhere() //同上
orWhere(function ($query) {
    $query->where()->where();
}) //参数分组
    
whereExists(function ($query) {
    $query->select(DB::raw(1))
          ->from('orders')
          ->whereRaw('orders.user_id = users.id');
})
    
whereBetween('created_at', [$start, $end])
whereNotBetween()
    
whereIn($column, $arr)
whereNotIn()

whereNull($field)
whereNotNull()
    
whereDate($field, $value)
whereMonth
whereDay
whereYear
whereTime($field, '=', $value)
    
whereColumn($col, $col2)
whereColumn($col, '>', $col2)
whereColumn([])
    
find($id)
```



#### 条件

```php
when($sortBy, function ($query) use ($sortBy) {
    return $query->orderBy($sortBy);
}, function ($query) {
    return $query->orderBy('name');
})
```



#### 分组

```php
groupBy($column)
having($field, '=', $value)
```



#### 排序

```php
orderBy(field, 'desc')

// 日期排序，默认 created_at 字段
latest()
oldest()
    
inRandomOrder() //随机
```

##### 参考：

[Laravel使用Eloquent ORM查询时多字段排序](https://www.jianshu.com/p/73a3c276d94c)

[Laravel ORM Model 的预定义属性](https://www.jianshu.com/p/68fefae9175b)



#### 偏移和数量

```php
offset(0)->limit(10)

// 等价于
skip(3)->take(3)
```



#### 分页

```php
paginate(40)
simplePaginate(15)
```



#### 获取

```php
get() //所有行
first() //单个行
value($column) //单个列
pluck($column_value, $key) //获取列
chunk($limit, function ($all) {}) //结果分块
```



#### 聚合

```
count()
max($column)
min($colmun)
avg($column)
sum($column)
```



#### 插入

```php
insert(
    ['email' => 'john@example.com', 'votes' => 0]
)

// 插入多条
insert([
    ['email' => 'taylor@example.com', 'votes' => 0],
    ['email' => 'dayle@example.com', 'votes' => 0]
])

// 自增 ID
insertGetId(
    ['email' => 'john@example.com', 'votes' => 0]
)
```



#### 更新

```php
update($data_arr)
update(['options->enabled' => true]) //更新 JSON 字段
```

自增 & 自减

```php
increment($field)
decrement('votes', 5, ['name' => 'John'])
```



#### 删除

```php
delete()
truncate() //清空
```



#### 悲观锁

```php
sharedLock() //共享锁
lockForUpdate() //更新锁
```




## 视图



### 布局

```php
@extends('layouts.app')
```



### 区块

```php
// 定义
@section('content')
// 内容
@endsection

// 引用
@yield('content')
```



### 分页

```php
withPath('custom/url')
appends(['sort' => 'votes'])
fragment('foo')
onEachSide(5)
toJson()
// 定义参见上面模型分页
// 视图中引用
links()
links('view.name', ['foo' => 'bar'])
```



### 赋值

```php
return view('home')->withArticles(\App\Article::all());
# 等价于
->with('articles', \App\Article::all())
    
->withFooBar(100)
# 等价于
->with('foo_bar', 100)
```



### 循环

```php
@foreach ($articles as $article)
	{{ url('article/'.$article->id) }}
@endforeach
```



### 条件

```php
@if (session('status'))
	{{ session('status') }}
@endif
```



## 函数

### app()

```php
app()->getLocale()
```



### session()



### url()



### config()



## 异常

### 错误日志

storage/logs/



## 常见问题

### No application encryption key has been specified.

.env 中 APP_KEY 为空，执行以下命令

```sh
php artisan key:generate
```

### 500 Whoops, something went wrong on our servers.
缺少 .env

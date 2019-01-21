Laravel 开发手册
=======

#### 参考：

[2017 版 Laravel 系列入门教程](https://github.com/johnlui/Learn-Laravel-5)

https://github.com/OMGZui/noteBook/tree/master/php/laravel



## 目录

[**安装**](#安装)

##### 命令行

make

db

migrate

key

vendor

##### 配置

环境变量

数据库

应用配置

路由

##### 控制器

##### 多模块开发

##### 数据库

读写分离

多库连接

Migration

Seeder

##### 模型

类定义

属性

方法

##### 视图

布局

区块

分页

赋值

循环

条件

##### 函数

##### 异常

错误日志

##### 常见问题



## 安装

```sh
composer create-project laravel/laravel learnlaravel5 ^5.5
```

设置文件夹权限
```
chmod 777 -R storage bootstrap/cache
```



## 命令行



### make

```sh
# 中间件
php artisan make:auth

# 模型
php artisan make:model Article
php artisan make:model Comment -m

# 数据库迁移
php artisan make:migration create_articles_table

# 数据库播种
php artisan make:seeder ArticleSeeder

# 控制器
php artisan make:controller Admin/HomeController
php artisan make:controller Admin/ArticleController
php artisan make:controller ArticleController
php artisan make:controller CommentController
```



### db

```sh
# 数据库
php artisan db:seed
```



### migrate

```sh
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



## 多模块开发

#### 参考：

[基于Laravel5.5的模块化开发](https://segmentfault.com/a/1190000011552359) => https://gitee.com/techlee/laravel5.5-modules-demo

[Laravel 模块化开发](https://laravel-china.org/articles/6153/laravel-modular-development) => https://github.com/nWidart/laravel-modules

[使用 Laravel-Modules 扩展包通过模块化开发大型 Laravel 应用](https://laravelacademy.org/post/7924.html)

[Laravel 怎么实现分模块，求大牛详解](https://laravel-china.org/topics/2687/laravel-how-to-achieve-the-sub-module-seeking-daniel-explain) => https://github.com/caffeinated/modules

[基于 Laravel 5 构建的、支持模块化和多语言的 CMS —— AsgardCMS](https://laravelacademy.org/post/6417.html) => https://github.com/AsgardCms







## 数据库

#### 参考：

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

#### 字段

```php
select()
```



#### 条件

```php
where(field, value)
where(field, '>', value)
whereBetween('created_at', [$start, $end]);

find(id)
```



#### 排序

```php
orderBy(field, 'desc')
```

##### 参考：

[Laravel使用Eloquent ORM查询时多字段排序](https://www.jianshu.com/p/73a3c276d94c)



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
first()
get()
all()
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


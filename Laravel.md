Laravel
=======

### 参考：

[2017 版 Laravel 系列入门教程](https://github.com/johnlui/Learn-Laravel-5)



# 安装

```sh
composer create-project laravel/laravel learnlaravel5 ^5.5
```



# 命令行



## make

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



## db

```sh
# 数据库
php artisan db:seed
```



## migrate

```sh
php artisan migrate
```



## key

```sh
php artisan key:generate
```



# 配置



## .env



## config\app.php



# 模型



多库连接

```php
// 模型里添加属性
protected $connection = 'mysql_classified';
```



字段

```php
select()
```

条件

```php
where(field, value)
where(field, '>', value)

find(id)
```

排序

```php
orderBy(field, 'desc')
```

偏移和数量

```php
offset(0)->limit(10)

// 等价于
skip(3)->take(3)
```

分页

```php
paginate(40)
```

获取

```php
first()
get()
all()
```



# Migration





# Seeder






# 路由



## routes/web.php

```php
Route::group(['middleware' => 'auth', 'namespace' => 'Admin', 'prefix' => 'admin'], function() {
	Route::get('/', 'HomeController@index');
});



Route::has('login')
```



# 视图



## 布局

```php
@extends('layouts.app')
```



## 区块

```php
// 定义
@section('content')
// 内容
@endsection

// 引用
@yield('content')
```



## 分页

```php
// 定义参见上面模型分页
// 视图中引用
links()
```



## 赋值

```php
return view('home')->withArticles(\App\Article::all());
# 等价于
->with('articles', \App\Article::all())
    
->withFooBar(100)
# 等价于
->with('foo_bar', 100)
```



## 循环

```php
@foreach ($articles as $article)
	{{ url('article/'.$article->id) }}
@endforeach
```



## 条件

```php
@if (session('status'))
	{{ session('status') }}
@endif
```







# app()

```php
app()->getLocale()
```



# session()



# url()



# config()





# 常见问题



## No application encryption key has been specified.

.env 中 APP_KEY 为空，执行以下命令

```sh
php artisan key:generate
```


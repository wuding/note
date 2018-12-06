https://github.com/johnlui/Learn-Laravel-5/issues/20

1.安装
composer create-project laravel/laravel learnlaravel5 ^5.5

2.命令行
php artisan make:auth
php artisan migrate
php artisan make:model Article
php artisan make:migration create_articles_table
php artisan make:seeder ArticleSeeder
php artisan db:seed
php artisan make:controller Admin/HomeController
php artisan make:controller Admin/ArticleController
php artisan make:model Comment -m
php artisan make:controller ArticleController
php artisan make:controller CommentController

3.配置
.env
config\app.php

4.Migration

5.Seeder

6.模型
find(id)
where(field, value)
where(field, '>', value)
orderBy(field, 'desc')
first()
get()
all()

select()

offset(0)->limit(10)

skip(3)->take(3)





7.路由
routes/web.php

Route::group(['middleware' => 'auth', 'namespace' => 'Admin', 'prefix' => 'admin'], function() {
​    Route::get('/', 'HomeController@index');
});



Route::has('login')



8.视图
return view('home')->withArticles(\App\Article::all());
->with('articles', \App\Article::all())
->withFooBar(100) 等价于 ->with('foo_bar', 100)



循环
@foreach ($articles as $article)
​    {{ url('article/'.$article->id) }}
@endforeach



布局

@extends('layouts.app')



区块

@section('content')

@endsection

@yield('content')



条件

 @if (session('status'))
​                        {{ session('status') }}
@endif



9.app()

app()->getLocale()



10.session()



11.url()



12.config()



# 常见问题

## No application encryption key has been specified.

.env 中 APP_KEY 为空，执行以下命令

```sh
php artisan key:generate
```


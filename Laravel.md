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

7.路由
routes/web.php

Route::group(['middleware' => 'auth', 'namespace' => 'Admin', 'prefix' => 'admin'], function() {
    Route::get('/', 'HomeController@index');
});

8.视图
return view('home')->withArticles(\App\Article::all());
->with('articles', \App\Article::all())
->withFooBar(100) 等价于 ->with('foo_bar', 100)

循环
@foreach ($articles as $article)
    {{ url('article/'.$article->id) }}
@endforeach
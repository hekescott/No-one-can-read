# Laravel 5.4 制作简书--笔记

> [慕课网链接](https://coding.imooc.com/class/111.html)
>
> 作者：轩脉刃
>
> 滴滴资深工程师



## 1.文章（blog）系统的建立

## ![img](file:///E:/Desktop/Write/Laravel5.5jianshu/4-2%E6%96%87%E7%AB%A0%E6%A8%A1%E5%9D%97%E7%AB%A0%E8%8A%82%E8%AF%B4%E6%98%8E.mp4_20180112_163348.434.jpg?lastModify=1515746276)

> 麻雀虽小，五脏俱全。数据库的增删改查，页面对数据库的操作
>
> * 业务要求
>
>
> 	* 文章列表展示 index
> 	* 添加文章 add
> 	* 编辑文章 edit
> 	* 删除文章 delete
> 	* 文章详情 show

* 为了完成以上功能，需要对数据库进行操作，同时还需要前端页面的跳转
  * 前端对页面进行操作（路由部分）
  * 后端对数据库进行操作 （表的设计，建立model模型，等等）



## 2.马上开始：路由

> 话不火说，我们这就开始，进入laravel5.4的世界。

###2.1 啥是路由

> 啥是路由啊，就是一个接线员。

ok，我现在要打一个电话，110.我和110之间不是直连，为啥不是直连，大家都懂，不是为我一个人服务的嘛，很多很多人。这时候就需要一个数据中心，进行转接。我打给数据中心，数据中心转播给110，我就可以和110进行通话了。

laravel的路由在routes/web.php，你看，他不叫路由，居然叫web.php，牛的不行了。说明路由是多么的重要，我就是web。web就是我。

默认是webcome界面

```php
Route::get('/',function(){
  return view('welcome');
});
```

PHP这个语言很直接啊，

###2.2 web.php

Route我是路由 （类）

::直接调用

get方法 http（get：获取资源 post：创建资源  put：更新资源  patch：增量更新（git） delete（删除资源） options（查询资源支持那些方法））

any，所有方法都支持。

match，进行匹配



> tips：html的form表单的method只支持，get和post
>
> 问题来了，我想让form支持put方法，怎么做呢，
>
> 笨方法：<input type="hidden",name="_method", value="PUT"/> 一个隐藏的输入框，强制指定了PUT方法
>
> Laravel优雅方法：{{ methed_field("PUT") }} 搞定，



网址是一个/ 这个就是一个主页面了。 然后就转给谁呢，转给return ，具体就是这里的view（‘welcome’）



```
这个意思就是，我拿到一个/网址，那么我就会给你一个welcome的view。没有弯弯绕
```

```
Route::get('/',控制Controller@行为（action）)
```

````
这个意思也很简单，当我拿到一个'/'的时候，我就让某某做某事，上面还是服务员呢，这会就有点领导的意思了。
````

没错，我就是web.php、



教程中是这样写的

```php
Route::get('/posts','\App\Http\Controllers\PostController@index');
```

但是在Jerrey Way的课程里

```php
Route::get('/posts','PostController@index');
```

Laravel是一门优雅的语言，所以我在看到作者的源代码之后，觉得太不科学了，翻了一下其他的教程，果然有精简版的。



###2.3 Route::group分组

比如，我的网址是\posts \posts\{id}  \posts\create

那么优雅的做法就是

```php
Route::group(['prefix'=>'posts'],function(){
  Route::put('/','PostController@index');
  Route::get('/{id}','PostController@show');
  Route::get('/create','PostCOntroller@create');
})
```

Route不传递id，传递文章模型进来，这个还需要细细的研究

```php
// post => 表：posts => 主键：id

Route::get('/posts/{post}','PostController@show');

function show(\App\Post $post){
  //...
}
```

###2.4 路由实战

事实证明，多学多看多练才管用啊

这一招叫`创建Rest风格资源控制器（带有index、create、store、edit、update、destroy、show方法）`

```bash
// 这个命令比不带--resource 的管用多了。
php artisan make:controller xxxx --resource
```

等下，还没完。还可以绑定model

```bash
php artisan make:controller xxx --resource --model=xxx
```

--resource 可以简写为-r 但是-model 不能简写为-m=xxx



具体可以参考[5.4控制器文档](https://d.laravel-china.org/docs/5.4/controllers)



###2.5 generator

[Laravel-5-Generators-Extended](https://github.com/laracasts/Laravel-5-Generators-Extended)

```5.5
composer require laracasts/generators --dev
```

```5.4
// 在app/Providers/AppServiceProvider.php添加

public function register()
{
	if ($this->app->environment() == 'local') {
		$this->app->register('Laracasts\Generators\GeneratorsServiceProvider');
	}
}
```

这样会多出3个选项

```
* Migrations With Schema
* Pivot Tables
* Database Seeders
```

```bash
php artisan make:migration:schema create_users_table --schema="username:string,email:string:unique"
```

```bash
username:string,body:text,age:integer,published_at:date,excerpt:text:nullable,email:string:unique:default('foo@example.com')
```

```txt
--schema= 可以简写为 -s+空格，是不是更方便了呢
```

```php
php artisan make:migration:schema 可以简写为genmm
```



运行这个命令之后，同样可以得到一个model，同时建立了数据表和model

数据表的连接foreign

```bash
php artisan make:migration:schema create_posts_table --schema="User_id:interget:foreign,title:string,text=body"
```

生成

```php
Schema::create('posts', function(Blueprint $table) {
	$table->increments('id');
	$table->integer('user_id');
	$table->foreign('user_id')->references('id')->on('users');
	$table->string('title');
	$table->text('body');
	$table->timestamps();
);
```

### 2.6 make:model

```php
php artisan make:model Demo -mrc
```

这是一个比较变态的命令，就目前来说，完全满足我的小需求。

注意Demo，而不是demo，一定要首字母大写哦
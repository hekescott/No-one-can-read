# Laravel5.4 Jshu项目

## 00_开始前的准备工作

### _01 修改env

```c
/*	创建新的项目:env文件数据库配置(mysql)
	
	修改主机地址->DB_HOST
	数据库名字->DB_DATABASE
	用户名->DB_USERNAME
	密码->DB_PASSWORD
	
	* */

/*	github上down的项目:
	
	* 修改env.example,为.env
	* 并按要求修改.env配置
	* 如上所示
	* 当然还会有其他设置
	
	* conposer install
*/

```

### _02 启动laragon

```c
/*	为啥是laragon?

	* 因为laragon够傻瓜啊,全都打包到一起了,不用太操心环境问题
	
	* Pretty url=>test.dev 炫酷不

	* 不过之前,要把win10的80端口解放出来,是一个World Wide Web 发布服务,直接停止+禁掉(可百度)
	
	* 至于项目部署的话,等做好了再说吧
**/

/*	主要是创建一个新的数据库
	
	* 可按.env进行数据库的创建
	
	* 也可使用laragon自动生成laravel工程,这样就会自动创建一个数据库啦
	
	* 只是laravel创建的工程是最新版的,不能选择其他版本
	
	**/
```



### _03 修改为中国时区

```c
/*  如何修改时区呢?

	* config/app--line67 'timezone'=> 'UTC'
	
	* 'timezone'=>'Asia/Shanghai' */
```

> 参考
>
> 1.[PHP手册:php.net/manual/zh](http://php.net/manual/zh/timezones.asia.php)
>
> ​	修改date_default_timezone_set 函数
>
> 2.UTC 世界统一时间:以本初子午线的平子夜起算的平太阳时。又称[格林尼治平时](https://baike.baidu.com/item/%E6%A0%BC%E6%9E%97%E5%B0%BC%E6%B2%BB%E5%B9%B3%E6%97%B6)或[格林尼治时间]

### _04 Generator(可选) 

```c
// cmder项目目录下执行
composer require laracasts/generators --dev

// 在app/providers/appserviceprovider 下替换或者添加如下代码
public function register()
    {
        if ($this->app->environment() == 'local') {
            $this->app->register('Laracasts\Generators\GeneratorsServiceProvider');
        }
    }

/*  为啥说可选呢
	* 比model XXX -mrc 功能比起来太弱了
	* 可model XXX -cr + mm:m create_xxxs_table 模式
	* 其实migration部分输入还是蛮少的,
	* 所以可选吧
```

> 参考
>
> 1.[Generators Github仓库](https://github.com/laracasts/Laravel-5-Generators-Extended)

### _05 Tinker使用

```c
/*	Tinker是一个命令行测试软件
	
	* 全称 php artisan tinker
	* cmder alias tinker
	
	* 在设置了 model,controller,等等之后
	* 就可以实时进行测试,实时把关代码
	
	* App\Post 可以被Tinker自动alias为 Post,人性化吧
	
	* 增
	* $post=new Post
	* $post->title = "This is a Test Title"
	* $post->content = "This is a Test Content"
	* $post->save()
	
	* 查
	
	* 查找所有
	* $post = Post::all()
	
	* 根据id查找
	* $post = Post::find($id)
	* $post = Post::where('id',2)
	
	* 根据id范围查找
	* $post = Post::where('id','>',2)
	
	* 精确匹配查找,
	* $post = Post::where('title',"This is a Test title")
	
	* 改
	* 查+重新设置
	
	* 删
	* 查+delete()
	
	* 
	**/
```

## 01_MMCr设定

> What is This?!
>
> M:model
>
> M:migration
>
> C:Controller
>
> Cr:restful Controller

```c
// 一个小命令
model Post -cr

// 使用Generator
composer require laracasts/generators --dev

// 在app/providers/appserviceprovider 下替换或者添加如下代码
public function register()
    {
        if ($this->app->environment() == 'local') {
            $this->app->register('Laracasts\Generators\GeneratorsServiceProvider');
        }
    }

// cmder alias
m:mm=php artisan make:migration:schema $*

// 输入
m:mm create_posts_table -s "title:string"
  
// 这里有个小的遗憾 在github上提了issue 不知道有没有好心的外国人
$table->string('title',100) 如何实现

/* OK 不管怎么样,我们建立了
	
	* Model -> Post.php
	
	* Controller -> PostController.php + restful
	
	* Migration -> create_posts_table -> migrate 



```


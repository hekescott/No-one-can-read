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

### _06 Github上传

```c
/*	Github Desktop

	* 安装 git
	
	* git init
	
	* 把文件夹拖入即可
	
	* 注意git的repositories的名字 不能有汉字哦
	
	**/
```

### _07 Tips

```c
/*	提升开发效率(笔记本 14寸,超级本篇)

	* 由于笔记本的屏幕偏小,性能较差
	
	* 所以选择IDE是不合算的,太卡,不流畅,这怎么能安心工作呢?
	
	* --->sublime Tex3 是最好的选择.
	
	* layout选择 横排*2,alt+shift+8,
	
	* 这是由于,很多代码需要各个文件进行关联对比
	
	* 比如web.php,和 controller的关联 

***/
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
	
	**/
```

## 02_V-blade

>V: view层 展示层
>
>blade:laravel blade模版

```c
/*	一个简单的视图页居然折腾半天,不应该啊 
	
	* 不过也是值了,很有收获,好不好
	
	* 如果有模版,进行header footer 肢解的时候
	
	* 一定要在chrome中用f12的方法,进行肢解,方便
	
	**/
```

## 03_URL和view的关联

```c
/*	用户接触的View,是通过URL进行定位的

	*  baidu.com 找的就是百度家,当然中间要经过域名定位等等工作
	
	*  baidu.com/accout 找的就是百度家的帐号,就是这样进行定位
	
	*  在后台,使用路由进行转接,为啥用路由呢,为了方便以后扩展
	
	*  url->路由->controller->view 最简单的了 四层
	
	**/


/*  create方法写在前面,恶心的来了
	
	* posts/{post}
	* posts/create
	
	* 如果是这样的顺序,优先识别create为{post}参数,进入post
	
	* 目前要这样写
	
	* posts/create
	* posts/{post}
	***/
```

## 04_index 页面输出

```c
/*	开胃菜:输出文章列表
	
	* 这个是比较简单一些的了,但是并不代表没有东西学
	* 
	* 首先引入 Use App\Post  引入和数据库关联的模型
	* $post = Post::orderBy('created_at','desc')->get();
	
	* $post = Post::latest()->get();
	
	* 都是按时间排序的哦****/


/*	carbon时间转化
	
	2017-05-10 18:22:32 这个是默认输出格式
	
	-> toFormattedDateString -> May-23-2017
	* **/

/*	数据填充
	
	* 数据库模式的填充,不用手动输入了,直接程序生成,节省开发时间
	
	*  ***/
```

> 参考
>
> 1. [carbon文档](http://carbon.nesbot.com/docs/)
> 2. [数据填充Faker](https://github.com/fzaninotto/Faker)


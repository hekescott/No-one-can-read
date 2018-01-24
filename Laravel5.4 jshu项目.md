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
	
	* database -> factories -> ModelFactory.php 
	
	* 格式***/
	
	$factory->define(App\Post::class,function(Faker\Generator $Faker){
      return [
        'title'=> $faker->sentence(6)
        ....
      ];
	})
      
    /* 注意: 1.这里面的属性一定是要定义好的哦,没有定义的会报错. **/
	

/*	分页
	
	* 这个是一个常见的功能了
	
	* 实现起来贼简单 ****/
      
    {{ $posts->links() }}

	/* 完事 */
```

> 参考
>
> 1. [carbon文档](http://carbon.nesbot.com/docs/)
> 2. [数据填充Faker](https://github.com/fzaninotto/Faker)

## 05_show 页面输出

```c
/* 这个就很简单了
	
 * 但是有更简单的方法一定要记住***/
 
 // web.php

    Route::get('/post/{post}',PostController@show);

 // PostController.php

	public function show(Post $post)
    {
      return view('posts.show',compact('post'));
    }
```

## 06_create 页面

```c
/*	Form属性 name设置

	* name 要和 数据库字段以及migration文件字段一致,方便管理 **/

/*	CSRF调用 	****/
	
	// 在form下加入 
	
	{{csrf_field()}}

	<input type="hidden" name="_token" value="{{csrf_token}}"/>
	
    // 原理
    
      
    /* 这是通过一个网址,来导入数据库的方式,那么就会产生一个问题,可以利用一些手段
    	
    	* 命令行,黑客等等方式,实现数据库的大量注入,而不是人为的输入
    	
    	* 这就很讨厌了,别管他是不是要干坏事,这至少不是我想看到的.
    	
    	* 怎么办呢,进入这个网址之后,我就会生成一个value值,代表这个网页生成了
    	
    	* 上传的时候,也必须带这个value值,才能代表你确定是打开这个页面并上传了
    	
    	* 这个value有个专业的名字叫_token
    	
    	* 不是很专业,大概就是这样.
    */

  /*  create 方法***/
      
      Post::create(request(['title','content']));
		
	  /* 注意create方法使用有个小前提,那就是在Post.php 写入 */
	  
	  Class Post extends Model
      {
      	protected $guarded=[];
      	protected $filled = ['title','content'];
      }
      
  /*  基类 不是鸡肋哦
	  	
	  	* app/Model.php
	  	*/

		<?php

		namespace App;

		use Illuminate\Database\Eloquent\Model as baseModel;

		class Model extends baseModel
		{
		    protected $guarded=[];
		}
		
	 	// Post.php
		use App\Model 
		
		class Post extends Model
        {
        
        }
        // 现在看是多此一举,以后看看
        
/*	表达验证
	
	* laravel内置一个超级好用的表单验证 validate */
	
		$this->validate(request(),[
		  'title'=>'required|string|max:100|min:2',
		  'content'=>'required|string|min:10',
		]);
	 * // 注意啦,这个validata,还有一个牛逼的地方,那就是自动传一个$errors到view中
       // 是的,就是这个牛逼的地方了
       // 等等还有更牛逼的,加入中文提示
       
       $this->validate(request(),[
		  'title'=>'required|string|max:100|min:2',
		  'content'=>'required|string|min:10',
       ],[
         'title.min'=>'标题太短了'
       ]);

	 /* 还要自己写,太烦了啊
	 
	 	* OK,有不烦的,
	 	
	 	* 添加zh文件夹
	 	* resources->lang->zh->validation.php
	 	
	 	* 修改config/app.php 配置
	 	* app.php->'locale' => 'zh',
	 **/

/*	总结
	
	* 1.验证 validate
	
	* 2.逻辑 获取请求+保存
	
	* 3.页面跳转(return redirect)  ***/
```

> 参考:
>
> 1.[5.4表单验证](https://d.laravel-china.org/docs/5.4/validation)
>
> 2.[5.4 validate中文提示](https://gist.github.com/linkdesu/994b59c8dc6217dd299a)

## 07_wangeditor

```c
/*	Wangeditor

	* 文本编辑器不够漂亮,问啥不来试试 "wang"呢
	
	* 支持国产,从我做起,aha~
	
	**/

/*	wangEditor 2.1.23

	* 先来测试 V2 版本

	* 复制dist->js&css&fonts->min.css 到 public->js & css& fonts
	
	* 新建一个js,命名为ylaravel.js,这个名字命的垃圾,wangeditorConfig.js 

	* 这些文件同时在main.blade.php中引入
	
	* 格式为/css/min.css  /js/min.js  需求jquery+wangeditor.js+ylaravel.js */

	// 这里需要注意的是 要把textarea标签的id 和这个 new wangEditor('id') 对应起来
		
		// create.blade.php 写法

		<textarea id="content" 
          		  style="height: 400px; max-height: 500px;" 
          		  name="content" 
          		  class="form-control" 
          		  placeholder="这里是内容"
        ></textarea>
        
          
		// wangeditorConfig.js 引入
		var editor = new wangEditor('content');

		editor.config.uploadImgUrl = '/posts/image/upload';

		// 设置 headers（举例）
		editor.config.uploadHeaders = {
    	  'X-CSRF-TOKEN' : $('meta[name="csrf-token"]').attr('content')
		};

		editor.create();
	

/*	测试
	
	* 完成上述步骤后,可以刷新页面了
	
	* 完了,你发现里面输出的都是html
	
	* 这是正常的,那么我应用肯定是把这些html格式化啊,肯定不能输出源码啊,
	
	* 看我的**/
	<p>{{ str_limit($post->content),100,'...' }}</p>
	
	->
      
    {!! str_limit($post->content),100,'...' !!}

	Bingo!
      
 /*	重头戏:图片上传
      
    * 完成这个才叫完美
    
    * 图片上传本质上也是一个表单提交 */
    
    // 在ylaravel.js中添加 
    
      editor.config.uploadImgUrl = '/posts/image/upload';

   /* 这个是用来设置,图片上传的地址的
	
    * 有了地址,我们就需要路由 */
     
    //  在web.php中 添加
    Route::post('/posts/image/upload','PostController@imgUpload');

	// post那就必须有个token,
	
    // main.blade.php 写入
	
	
	// ylaravel.js 设置 headers（举例）
	editor.config.uploadHeaders = {
    	'X-CSRF-TOKEN' : $('meta[name="csrf-token"]').attr('content')
	};

	// 对图片进行储存 储存地址
	storage->app->public
	
	// 修改配置,关联文件夹
	filesystems.php->'default' => env('FILESYSTEM_DRIVER', 'public'),

    // 加入公开磁盘
    php artisan storage:link
    
    // 对PostController进行修改
    public function imageUpload(Request $request)
    {
      $path = $request->file('wangEditorH5File')->storePublicly(md5(time()));
      return asset('storage/'. $path);
    }

	// 真是意想不到的错误,一个简单的post网址请求,我居然错了两次,不敢相信啊

```

> 参考
>
> 1. [wangeditor](www.wangeditor.com)
>
> 2. [X-XSRF-TOKEN](https://d.laravel-china.org/docs/5.4/csrf#csrf-x-xsrf-token)


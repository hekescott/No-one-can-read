# Laravel 5.4 From Scratch

> Jerrey Way.官方推荐教程！你说牛不牛，可惜咱英文实在不行，要不然肯定以这个为主了。现在综合学习，循环滚动，把laravel MVC掌握了再说。

## 06_QueryBuilder

> QueryBuilder:数据库请求构建器
>
> 好拗口的翻译，
>
> Laravel 的数据库查询构造器提供了一个方便的接口来创建及运行数据库查询语句。它能用来执行应用程序中的大部分数据库操作，且能在所有被支持的数据库系统中使用。

他就是提供了一个数据库`CRUD`的接口，对数据库进行操作。

### _1 创建一个Task数据表

要求

* 熟悉 php artisan make:migration create_tasks_table 命令

* ```bash
  rem 拓展插件命令  
  php artisan make:migration:schema create_tasks_table -s "body:string"
  m:mm create_tasks_table -s "body:string"
  ```

* ​

* ```php
  // 在app/providers/appserviceprovider 下替换或者添加如下代码
  public function register()
      {
          if ($this->app->environment() == 'local') {
              $this->app->register('Laracasts\Generators\GeneratorsServiceProvider');
          }
      }
  ```

* 但是大杀器居然是他

* ```bash
  rem 自动创建migration create_demos_table
  rem 自动创建Controller DemoController 还是restful风格的，而且自动绑定模型app/demo
  rem 就问你这个命令厉害不厉害，少输入多少字符啊。
  rem 美中不足的是，不能和 laracast的generator结合。 实现快速创建mig，要是在加上这个功能那岂不是完美了。
  rem generator 在输入mig迁移表的时候，确实很给力，但是也是有缺点的，要是能在程序里面体现就好了，就像emmet一样。

  model Demo -mrc
  php artisan make:model Demo -mrc
  ```

  ​

我的任务是`创建一个tasks的数据表`，但是不好意思，我一下子就创建了`model`，`关联model的而且具有restful风格的controller`。速度很快，有没有。继续坚持，看看能不能发现更给力的技巧来。

Task1达成！

### _2 获取这个Task表的数据，并输出显示

```php
// 把数据表中的数据 传值给tasks变量
$tasks = DB::table('tasks')->get();
return $tasks;
return view('tasks.index',compact('tasks'));
// 通过id获取单项
$task = DB::table('tasks')->find($id);
dd($task);
return view('tasks.show',compact('task'));
```

###_3 通过网址绑定show和index 

```html
<a href="/tasks/{{ $task->id }}"></a>
```

## 07_Eloquent 

###_1 简写DB

```php
$tasks=DB::table('tasks')->get();

// 建立Task模型之后
$tasks=App\Task::all();

// 在web.php中引入
use App\Task
  
//就可以写为
$tasks = Task:all();

//怎么样，够优雅吧
```

Task1 完成！

###_2 tinker 基础用法

```bash
rem cmder中user-alias.cmd中设置
tinker=php artisan make:tinker
```

```php
// 输出所有
App\Task::all()

// 输出第一项到变量$task
// 直接输入App\Task:first() 是不能执行的哦
$task = App\Task:first()
  
// App\Task::where('id','=',2)->get()  '>'可省略
// > 序号>2的 <序号小于
App\Task::where('id',2)->get()

// pluck 输出body的所有内容
App\Task::pluck('body') 
  
// first 输出第一项
App\Task::pluck('body')->first()
```

Task2 完成！！



###_3 非破坏性的增加和移除数据表column

系统自带命令:有个小缺点 posts table需要反复输入，优雅吗，一点都不优雅

```php
// 删除 remove from
php artisan make:migration remove_user_id_from_posts_table --table=posts
m:m remove_user_id_from_posts_table --table=posts

// 添加 add to
php artisan make:migration add_description_to_articles_table --table=articles
m:m add_description_to_articles_table --table=articles
  
// 改变 change on
php artisan make:migration change_description_on_articles_table --table=articles
m:m change_description_on_articles_table --table=articles
```

那么就来一个优雅的

```php
// 移除 简写之后是不是很优雅呢
// 注意对已经有model的 需要加上  --model=0
// 咳咳，上面的话当我没说，设置上已经很完美了，就是如果已经有了model那么不进行任何操作
// 那么你还担心什么呢，大胆的操作吧
php artisan make:migration:schema remove_user_id_from_posts_table --schema="user_id:integer"
m:mm remove_user_id_from_posts_table -s "user_id:integer"
 
  
// 暂时没看到 updata 只有add和remove，好像这两个远远不够啊，修改的时候会比较多
```

```php
// update

m:m update_completed_in_tasks_table --table=tasks

// 其他写法都一样，只不过在最后加入了change()

$table->boolean('completed')->default(false)->change();
 
```

#### _3_1 generator适用场景

```php
// 好吧 在经历了一段辛苦又蛋疼还没有成效的search之后 2018.01.15 进行盖棺定论

// generator适用场景，
// 大量的创建已有的或者是已设计好的（心里有谱的数据库，而且项目比较多的情况下，放心使用 generator的create项目，至于add remove，这些都是锦上添花，可有可无的角色）

// 1.大量的工作量
// 2.很熟悉其中的项目
// 3.仅限于create
// 4.在add move 或者想要的update功能上，还是复制粘贴，或者系统原生来的更方便些。
// 5.Thats All。还算优雅吧。

```

### _4 获取所有的completed数据

```php
// 01 Tinker命令
App\Task::where('completed',0)->get();

// 02 给Task添加方法
public static function incompleted(){
  return stati::where('completed',0)->get();
}

// 添加方法之后，Tinker中可以这样调用
App\Task::incompleted()

// 03 同样给Task添加方法
public function scopeIncomplete($query){
  return $query->where('completed',0);
}

// 邪门？放着好好的方法2不用，还要搞这么复杂的方法3
// too young!这个是为了代码的重用性，这个零件是多功能的，牛不牛

//添加方法后，Tinker中可以这样调用
App\Task::incomplete()
// 拿到数据之后，可以->get 不仅如此还可以->where('completed','>',3)->get()
App\Task::incomplete()->get()
App\Task::incomplete()->where('completed','>',3)->get()

// 写在最后，优雅的Tinker
// 每次都输入App\Task好烦，试试直接输入Task试试，是不是瞬间舒畅不少呢？
[!] Aliasing 'Task' to 'App\Task' for this Tinker session.
// 优雅、舒服！
```



> Eloquent 是 Laravel 的 'ORM'，即 'Object Relational Mapping'，对象关系映射。ORM 的出现是为了帮我们把对数据库的操作变得更加地方便。

## 08_Controller

### _1 路由做路由的事情

```php
// Controller的意义就是接受Route的分发

Route::get('/tasks',function(){
  $tasks = Task::all();
  return view('tasks.index',compact('task'));
})
  
// 引入Controller之后，就在TasksController文件中加入
  use App\Task;
  
  public function index(){
  $tasks = Task::all();
  return view('tasks.index',compact('tasks'));
}

// 原先的路由写为
Route::get('/tasks','TasksController@index');

// 注意，这里官方自动生成的Controller是不带复数的，比如TaskController
// 而JerreyWay教程中是生成带复数的Controller 还有一个5.5 step by step也是复数，但是

// 在细细考察了几个教程之后，这些作者的Controller写法，大部分都不带复数

// 我是这样理解的，这是一个类，比如Model:Task.php, Controller也属于类 所以应该是TaskController
// 复数代表一个集合，比如数据表 tasks，比如Tasks的view文件夹，在比如显示Task的index的网址，肯定也是/tasks，在mig中，创建数据库版本控制，是对tasks的控制，所以也是create_tasks_table

// 还有个大小写的问题
// 同理，类就要大写，比如Task.php, TaskController.php
// 其他都是小写
// 复数+小写，单数+大写，大致是这样，没有系统论证，咱们走着瞧
```

同理，把tasks.show转换为Controller控制。

// 别忘了分号，以及注意拼写，都错了好多次了



## 09_Route Model Binding

### _1 show($id)简写   

```php
public function show($id){
  $task = Task::find($id);
  return view('tasks.show',compact('task'));
}

简写为
  public function show(Task $task){
  return view('tasks.show',compact('task'));
}

// 干的漂亮，能简写就简写，能优雅就优雅，能优化就优化
// 注意function(a b c) 不带逗号 和 Javascript的不一样 function(a,b,c) 又简写了
// 还有一个需要明白，Task $task这个是固定组合，比如User $user,这是必须滴，
// 想省事，就得守规矩，明白了吗？

```

##10_Layout and Structure

### _1 创建模版 layout

主文件main

```html
// layout.blade.php 这个一般都是主文件模版

<!DOCTYPE html>
<html>
  <head>
    <title>My Application</title>
  </head>
  <body>
    <!--意思是content插入到这里-->
    <div class="container">
      @yield('content')
    </div>
    @yield('footer')
  </body> 
</html>
```

分文件

```blade.php
@extents('layout')

@section('content')
abcdefg
@endsection

@section('footer')
<script src="xxxx.js"></script>
@endsection
```

以上是简单的应用，随便测试一下

### _2 模版实战

[Bootstrap的album模版](http://v4-alpha.getbootstrap.com/examples/album/)

复制源代码到项目中，更换js 和css链接，JS使用官方cdn，

css则落实到本地的`public>css`中

```html
<!--Jeffrey Way的做法-->
<link href="/css/album.css" rel="stylesheet">

<!--我自己的做法-->
<link href-"css/album.css" rel="stylesheet">

<!--我自己的做法显然是愚蠢的，-->
在某些情况下会出bug，比如我刚才设置的blog，blog/create
blog可以完美继承css
但是blog/create 就不会继承css
所以，只有"/css/album.css"
这一种写法

<!--注意css要放在 public的css下 切记-->
```

```html
<!--主页引入部件-->
@include('layouts.nav')  
```

> 同样是引入 include 和 yield有啥区别呢？
>
> * include 是传统的调用，是Main-part，需要用到Main view（‘main’）
> * 而yield 则不是传统调用，是Part+通用，view（'part'）
> * include 单个调用，也yield是要和extends配合的



````php
// 假设我们现在有view A(通用/全局) 和 B（局部/local）
// 同时假设，我们还有Part C D E F

// 如果展示A，但是A一般不是单独展示的，A+B的形式 
@include C
@include D
@include E
@include F
  
// 如果展示B 默认是A+B

// 在A中要写
  @yield('content')
  @yield('footer')
// 在B中要写
  @extends(A)
  
  @section('content')
  @endsection

// 这次你明白了吗，知道怎么用了么 
入点是B，展示的是B+(A+C+D+E+F)
  
````

## 11_CSRF 

[5.3文档](https://d.laravel-china.org/docs/5.3/csrf)

### _01 CSRF是啥

```c
/* CSRF 是个啥?

   * cross-site request forgery ??? 我了个去
   
   * 跨站请求伪造
   
   * Laravel 提供简单的方法保护你的应用不受到 跨站请求伪造 (CSRF) 攻击。
   
   * 跨站请求伪造是一种恶意的攻击，它利用已通过身份验证的用户身份来运行未经授权的命令。
   
   * 是不是很高端,很黑客,不过sorry,laravel在等你

   * Laravel 会为每个活跃用户自动生成一个 CSRF "token" 。
   
   * 该 token 用来核实应用接收到的请求是通过身份验证的用户出于本意发送的。
   
/*  秘密武器:{{ csrf_field() }} 写法 */

	<form method="POST" action="/profile">
	    {{ csrf_field() }}
	    ...
	</form>
	
/*  如果忘记设置,就会出现TokenMismatchException

	* verifyCsrToken line68
	
	* 兄弟,我不想看到你第二次!
	

```

###_02 store 方法进行保存

```c
/*  在store方法中输出 */
	
    // web.php
    Route::post('/post/blog/create',BlogController@store);  

	// BlogController.php @store 输出所有all()
	dd(request()->all());
	// 输出数组中的项目
	dd(request(['title','body']));


// 代码如下

	// 利用post模型,保存title和body

	$post = new Post;
	
	$post->title = request('title');
	$post->body = request('body');
      
    // save
      
    $post->save();

	// 跳转
	
	return redirect('/');

	// 精简代码

		// post::create方法
	
		    Post::create([
			  'title'=>request('title'),
			  'body'=>request('body')
			]);

			// 继续精简
				// 一行代码搞定
				Post::create(request(['title','body']));

  	    // post.php
  
  			class Post extends Model
  			{
  	
  			// 可被填充
   			protected $filled = ['title','body'];
   
  			// 不可被填充, 为空的时候, 有时候不失为一种巧妙的方法
  			// 但是一般情况下,还是规规矩矩的来
  			protected $guarded = []
 		    }

/*  反思
    * 出现N个错误,终于上传了我的第一个数据,我是不是很菜啊
	
	* 匪夷所思的错误,title,我居然错写成titie,我是不是太有才了
	
	* 每次都提示我说,找不到title,我说title就在那里啊,怎么找不到.
	
	* 结果还真我我搞错了
	
	* 起初我判断错误出现在 Post.php, protected属性,发现没用,不过第二种方法可以哦
	
	* 犯了一个愚蠢的错误,命名不统一,一会blog,一会post,终于又搞错了
	
	* 主要是名字不统一的问题,这时候想念phpstorm了吧,因为phpstorm改名是可以自动更新的哦
	
	* 还是规范好,省的到时候出错,另外看看sublime有没有项目管理插件
```

### _03 复习

``` c
/*  还记得Tinker吗
	
	* 获取数据库数据
	* App\Post::all()
	* 可简写为 Post::all()

```



## 12_表单验证

```c
/*  为啥会有表单验证
	
	* 前端页面,就相当于一个前台服务员,酒店前台
	
	* 后端数据库,就相当于房客,符合条件才能住进来
	
	* 这就需要前台服务员对客户进行甄别了,
	
	* 没钱,不好意思,没身份证,不好意思, 
    
    * 前端页面就是干这个脏活的
    
    * 数据为空,不行,超出长度,不行,等等     *****/
```



### _01 最简单的表单验证(html)

```c
/*  最简单的表单验证
	
	* 很简单 
	<input required>
	<textarea required></textarea>
	
	* 输入不能为空
```



###_02 最简单的表单验证(store)

```c
/*	最简单的表单验证,不那么简单的实现方法
	
	* 不简单还实现他干什么啊.
	
	* 简单的功能少,不简单的功能多,当业务需求多时,请问我还有的选么? */
	
    $this->validate(request(),[
    	'title'=> 'required',
      	'body'=> 'required'
    ]);
	
```

### _03 Html提示信息展示

```php
// 如果有错误信息,则展示
@if(count($errors))
  
<div class="form-group">
  <div class="alert alert-danger">
  	<ul>
  	// 获取所有错误
  	@foreach ($errors->all() as $error)
  		<li>{{$error }}</li>
  	@endforeach
  	</ul>
  </div>
</div>

@endif
```

## 13_输出展示博文

```c
/*	终于到输出和渲染啦,完成一个闭环

	* 前面,做数据库的设置,
	* 数据库的导入
	* 现在该数据库输入啦
```

## 15_Eloquent Relationships and comments

> 关系模型绑定,以及评论系统

```c
/* 	Task1 生成Comments的Model Controller Migration */
	
	Model Comment -mrc
	
	* Comment 大写,单数,没忘记吧
	
/*  Task2 建立posts和comments的关系

	* 啥关系啊,就是一篇博文,对应N个评论 1对多的关系 是一个从属关系
	
	* 睁大眼睛看看,一对多的从属关系怎么写哦  */
	
	
	// in Post.php
	
	public function comments()
    {
      return $this->hasMany(Comment::class);
    }
		
	// in Comment.php

	public function post()
    {
      return $this->belongsTo(Post::class);
    }
	
    /* Tips
    
       * 切记，Eloquent 会自动判断 Comment 模型上正确的外键字段。按约定来说，Eloquent 会取用自身		 * 模型的名称的「Snake Case」，并在后方加上 _id。所以，以此例来说，Eloquent 会假设 Comment 		  * 模型的外键是 post_id
```


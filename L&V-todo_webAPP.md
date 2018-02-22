## 01_方法步骤整理(粗)

1. 生成lararvel5.5 项目,
2. cnpm i,注册开发依赖
3. model Task -m 生成模型+迁移文件
4. migration中写

```c
$table->string('name');
$table->boolean('completed')->default(false);
```

5. 配置.env 设置数据库
6. 数据库迁移 migrate
7. 写入api.php的路由

```c
Route::resource('tasks','TasksController',[
  'except'=>['create','edit','show']
]);
```

8. 创建TasksController

```c
ctrl TaskController --resource

// 并在生成的TaskController中加入 use App\Task

// 早知道这样,不如直接输入 Model Task -mcr 方便快捷
```

9. index 获取所有,按时间顺序

```c
// 这个就是api的设计
return Task::latest()->get();

// 如果是网址,那么就该是 这样就实现了在laravel中api和前端页面的结合
// 但是有时候这样并不自由,
// 两种方式任君选择,都还可以

$task = Task::latest()->get();

return view('tasks.index',compact('task'))
```

10. store 进行数据上传

```c
public function store(Request $request)
{
  $task = new Task();
  $task->name=$request->name;
  $task->save();
  
  return $task;
}

不是可以写为

return Task::create(request(['name']));

// 这个需要查一个官方文档,Create方法

// 这里留一个TODO::
// 就是看这个能不能用create进行简化
```

```c
// 这里store的写法就不一样了

// 验证
$this->validate(request(),[
  'name'=>"required"
]);

// 上传+保存
Task::create(request(['name']));

// 页面跳转
return redirect('/tasks');
```

11. update 数据更新

```c
public function update(Request $request,$id)
{
  $task = Task::findorFail($id);
  $task->completed = $request->completed;
  $task->update();
  
  return $task;
}
```

12. destroy 数据删除

```c
public function destroy($id)
{
  $task -> Task::findorFail($id);
  $task -> delete();
  
  return 204;
}
```

13. 安装Postman,这个是一个api测试神器

[postman地址](https://www.getpostman.com/)

用postman对刚刚建立的api 进行 CURD测试 OK

对于postman的操作 , postman已经做的很傻瓜化了,

get方法,直接操作就行

至于post put等,要选择 body->x-www-form-unlencoded 然后进行数据操作



重命名 ,保存快捷方式,这都不用说了吧,so easy 



14. 在测试api成功之后,或者出现问题,修复之后, 进行view页的改进

welcome的修改,

```c
// 在head中加入
<meta name="csrf_token" content="{{ csrf_token() }}">
  
```

> ## [X-CSRF-TOKEN](https://d.laravel-china.org/docs/5.5/csrf#csrf-x-csrf-token)[#](https://d.laravel-china.org/docs/5.5/csrf#X-CSRF-TOKEN):令牌
>
> 除了检查 POST 参数中的 CSRF 令牌外，`VerifyCsrfToken` 中间件还会检查 `X-CSRF-TOKEN` 请求头。你可以将令牌保存在 HTML `meta` 标签中：
>
> ```
> <meta name="csrf-token" content="{{ csrf_token() }}">
> ```
>
> 然后你就可以使用类似 jQuery 的库自动将令牌添加到所有请求的头信息中。这可以为基于 AJAX 的应用提供简单、方便的 CSRF 保护
>
> ```
> $.ajaxSetup({
>     headers: {
>         'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')
>     }
> });
> ```
>
> [参考资料X-csrf-token](https://d.laravel-china.org/docs/5.5/csrf#csrf-x-csrf-token)
>
> 这里有一个想法TIP,如果编辑器或者IDE中,能想chrome中,直接删除整个代码段,或者更进一步对整个代码段进行操作,那感觉就太棒了
>
> 至少sublime中对闭合标签代码段的操作还非常弱
>
> 以致于,我要学习pug,

15. js定位

```c
<script src="{{ asset('js/app.js') }}"</script>
```

resources -> assets -> js -> components

16. 开始vue的配置

vue目录resources -> assets -> js

```c
// 修改app.js

Vue.component('tasks',require('./components/Tasks.vue'));
```

吐槽TIP:vue为啥就没有一款好用的format呢,可能开发起来难度高吧,毕竟是综合 html+js+css 综合,算了,别浪费这个时间了,手工排版也挺好.

17. 使用emmet进行排版

 .input-group>input.form-control+span.input-group-btn>button.btn.btn-success



.tasks-list>ul.list-unstyled>li*3>.checkbox+label>input[type=checkbox]{task $}

```c
/*	$的用法{$@ $@-}
	
	* $ 有很多,但是在emmet中只表示序号
	* 比如 $*5 1,2,3,4,5
	
	* 比如 $@-*5 5,4,3,2,1
	* 比如 $@5*5  5,6,7,8,9
	
	* 但是$ 貌似不能加入－号,不知道是设置的,还是怎么回事,不过没关系,基本上没有碰到过符号的时候
```

18. 主文件中引入.css

```c
<link rel="stylesheet" href="{{ asset('css/app.css') }}"/>
```

19. 主文件引入taskscomponents

```c
<tasks></tasks>
```

Tip:在这里有一个sublime的坑,就是在atom和ps ws中都是tab可以使用emmet的扩展,但是sublime中确不可以,只能用ctrl+e,不知道怎么设置.

如果设置了tab取代ctrl+e 那么 tab的默认功能将消失,这可不是我想看到的

谁让我英文不好呢,谁让我自己不会编程呢,这就是一个悲剧啊



19.1 bug ,当然是我的问题咯,

我在写好vue的components的组件之后,引入就是编译不了

新建一个laravel工程之后,测试引入文件,可以

在按照新的文件去修改旧的,还是不可以

等了一会,我去居然可以了,

是不是很可怕

bug不可怕,找不到bug才可怕,这就是一颗定时炸弹啊



20. 先不管这个bug了,做不好就重新再来呗

修改body 和 tasks-list的padding-top :20px 这下看起来舒服多了



21. 查 : 在vue中开始使用api数据了哦 使用的axios

```c
// 代码如下
axios.get('/api/tasks').then((res)=>{
  this.tasks = res.data
})
  .catch((err)=>{
    console.log(err)
  })
  
// 这个是我们的老朋友了,当时我并不怎么懂这样写法的意思,现在看起来清晰多了
  
1.网址请求('api/tasks') axios 就会返回一个request 简写为res

2.把这个request赋值给我们的变量tasks

3.一旦发生错误,那么就通过console.log打印出来
```

22. 增:

```c
create(){
  axios.post('/api/tasks',this.task).then((res)=>{
    this.tasks.unshift(res.data)
      this.tasks.name=''
  })
    catch((err)=>{
      console.log(err)
    })
}

// unshift 是往数组头部增加一个元素

// 代码的意思是

// 1.axios.post提交,提交当前的task, 
// 2.请求tasks,并加入当前的tasks 数组
// 3.当前task清空
// 4.如果发生错误,则抛出错误.
```

23. 增:bug,

只能增加一次是什么鬼,邪门啊



24. 服务器设置

```c
// 原生

php artisan sever --host 0.0.0.0 --port:80
  
// 在laragon-nignx上配置
  
C:\laragon\etc\nginx\sites-enabled

找到该网站的配置,比如我的是todo

找到auto.Todo.dev.conf

添加局域网内容

复制上面一个server 修改

listen 8080
server_name 为 192.168.1.114:8080

其他不动即可

研究了一天啊,我勒个去

这还没完,又研究了一个晚上

手贱,升级了chrome浏览器,然后居然不支持.dev域名了,我勒个去

在62版本,可以很愉快的输入test.dev
在63版本,GG

在找个其他的名字吧
```


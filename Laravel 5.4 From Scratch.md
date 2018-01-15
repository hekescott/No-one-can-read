# Laravel 5.4 From Scratch

> Jerrey Way.官方推荐教程！你说牛不牛，可惜咱英文实在不行，要不然肯定以这个为主了。现在综合学习，循环滚动，把laravel MVC掌握了再说。

## 06.QueryBuilder

> QueryBuilder:数据库请求构建器
>
> 好拗口的翻译，
>
> Laravel 的数据库查询构造器提供了一个方便的接口来创建及运行数据库查询语句。它能用来执行应用程序中的大部分数据库操作，且能在所有被支持的数据库系统中使用。

他就是提供了一个数据库`CRUD`的接口，对数据库进行操作。

### 06.1 创建一个Task数据表

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

### 06.2 获取这个Task表的数据，并输出显示

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

###06.3 通过网址绑定show和index 

```html
<a href="/tasks/{{ $task->id }}"></a>
```

## 07.Eloquent 

###07.1 简写DB

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

###07.2 tinker 基础用法

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



###07.3 非破坏性的增加和移除数据表column

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

#### 07.3.1 generator适用场景

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



> Eloquent 是 Laravel 的 'ORM'，即 'Object Relational Mapping'，对象关系映射。ORM 的出现是为了帮我们把对数据库的操作变得更加地方便。


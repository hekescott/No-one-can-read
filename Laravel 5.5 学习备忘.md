# Laravel 5.5 学习备忘

## 1.环境

* laragon 环境配置  
* 需要全局安装composer 才能使用 laravel new 命令
* 至于网站简称，每次都要加上端口名称，也不知道为啥，反正就是app.io：81，先这样吧，回头在研究

## 2.启动laravel服务

* 推荐使用laragon 迅速创建一个laravel应用，这样可以使用composer的缓存。
* 如果用laravel new 则不能使用缓存，不光慢，而且会出现部分fail


* 可以直接使用laragon来进行启动
* 这样可以不用在输入 php artisan key:generate 
* 同样不需要输入 php artisan package:discover
* 在.env文件夹中直接配置 DB 数据库 名字 username 密码（测试用可以为空）
* 也可以直接进入项目文件夹，输入php artisan serve 就会启动一个带8000端口的服务 http://localhost:8000/
* php -S localhost:8888 -t public 同样也可以启动服务

## 3.migrations 迁移，就是把系统中的表单，直接生成到数据库中。

* 这个是一个非常重要的数据库版本控制工具
* 命令就是 php artisan migrate
* 注意这个命令的前提是，必须有.env 配置表中的数据库 “appdb”
* 然后就是提示成功，如果没有就是提示失败。
* 同时注意username和password是否匹配，这些都是很简单啦
* migrate:rollback :reset 常用命令.
* 创建一个新migration文件 php artisan make:migration create_articles_table --create=articles
* 难度升级，比如我这个网站上线以后，但是我又想加入一个新的数据列，但是这时候，我不能把原先的初始化，或者是rollback，只能自增一个，这时候怎么办呢，方法当然有了。
* 新建一个表 php artisan make:migration add_intro_column_to_articles --table=articles 
* 在这个表中进行 输入项目的编辑即可。正 add 反是dropcolumn
* 使用这个功能需要一个依赖包，composer require doctrine/dbal
* 官网doc laravel.com/docs/5.1/migrations
* 5.5 官网 https://laravel.com/docs/5.5/migrations
* 5.5 中文官网 https://d.laravel-china.org/docs/5.5/migrations
* 数据表的相互引用 怎么样建立不同数据表的联系呢
* $table->foreign('user_id')->reference('id')->on('users')
* 建立数据表之间的联系，而且名字可以不一样哦。至于spring-boot是怎么玩的，没看到这个联系啊，不过肯定是有，只是我忽略了罢了，回头有时间的时候再想想。
* 执行php artisan migrate 如果出现错误，就删除数据表重来呗，但是不是办法，rollback 和 reset居然都错，真是见了鬼。是要解决的问题。

## 4.模型关联 relations  MVC之Model

* $table->foreign('company_id')->references('id')->on('companies');
* 这个是不是模型关联呢，好像不是啊
* 这个才是 hasMany belongsTo
* public function user（）｛return $this->hasMany('App\user')｝
* php artisan make:model Project

## 5.Controllers  MVC 之 Controller

* php artisan make:controller CompaniesController --resouce --model=Company
* 这个命令实现MC的直接绑定。
* 参考网址 laravel.com/docs/5.5/controllers
* 中文网址 https://d.laravel-china.org/docs/5.5/controllers

## 6 Route

* 光是拼写都错了好多次
* 这里有变量 tasks task，这里有文件夹tasks，这里还有数据库tasks和task，还有网址的tasks，我去你说乱不乱。
* 变量还好说一点，加上$
* 文件夹的tasks  这个和view 在一起。一般是tasks.show tasks.index.
* 网址的tasks 这个也好说，是get('/tasks')
* 数据库的tasks 是 DB::table('tasks')
* 数据库的单项，一般在view中引用。$task->task
* 这里还有一个特例，compact 传值，这个变量是不加$,这是目前来说唯一的特例
* 摇摇晃晃的终于完成了，step6. 主要是命名的混乱。
* 还是参照jerrey的命名方式，如果有更好的在继续进化
* 大部分都是小写，文件夹s es表示负数结尾，数据库也是s es结尾，长的单词采用驼峰写法。注意引号的使用。
* 网址全部用小写，要不然麻烦。

## 7 .命令行简化

* 自动生成工具https://github.com/laracasts/Laravel-5-Generators-Extended

* `make:migration:schema`

* `make:migration:pivot`

* `make:seed`

* ```
  composer require laracasts/generators --dev
  ```

* https://github.com/cmderdev/cmder/issues/214

* ```
  art=php artisan $* 
  g:mig=php artisan generate:migration $*
  g:con=php artisan generate:controller $*
  g:mod=php artisan generate:model $*
  vssh=ssh vagrant@127.0.0.1 -p 2222
  ```

## Tip

* 命令行 start . 快速打开文件夹
* cmder 加入右键菜单，不是特别完美，但是还好了，cmdermini和laragon的cmder不怎么兼容。


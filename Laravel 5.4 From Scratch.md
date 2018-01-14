# Laravel 5.4 From Scratch

> Jerrey Way.官方推荐教程！你说牛不牛，可惜咱英文实在不行，要不然肯定以这个为主了。现在综合学习，循环滚动，把laravel MVC掌握了再说。

## 06.QueryBuilder

> QueryBuilder:数据库请求构建器
>
> 好拗口的翻译，
>
> Laravel 的数据库查询构造器提供了一个方便的接口来创建及运行数据库查询语句。它能用来执行应用程序中的大部分数据库操作，且能在所有被支持的数据库系统中使用。

他就是提供了一个数据库`CRUD`的接口，对数据库进行操作。

### 06.1 创建一个Task数据库

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

* ​
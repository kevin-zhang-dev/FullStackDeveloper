# Laravel

> 在laravel中如果想友好输出打印内容，可以使用dd方法。

### 模块开启：

LoadModule deflate_module modules/mod_deflate.so

LoadModule rewrite_module modules/mod_rewrite.so

| 包/扩展名称                                   | 命令                                       |
| ---------------------------------------- | ---------------------------------------- |
| [overtrue/*laravel-lang*](https://packagist.org/packages/overtrue/laravel-lang) | composer require "overtrue/laravel-lang:~3.0" |
| php artisan serve                        | 测试项目的安装                                  |
| [barryvdh/](https://packagist.org/packages/barryvdh/)laravel-debugbar | composer require barryvdh/laravel-debugbar --dev |
| [mews/captcha](https://github.com/mewebstudio/captcha) |                                          |
| [Intervention/image](https://github.com/Intervention/image) | 扩展包来处理图片裁切的逻辑                            |
| [hieu-le/](https://packagist.org/packages/hieu-le/)active | 导航栏添加 `active` 类                         |
| [mews/](https://packagist.org/packages/mews/)purifier | 防止 XSS 攻击                                |
| [Guzzle](https://github.com/guzzle/guzzle) | PHP HTTP 请求套件                            |
| [summerblue/](https://packagist.org/packages/summerblue/)generator | 代码生成器                                    |



npm install --save-dev laravel-mix			// npm 安装依赖

npm install --no-bin-links					// npm 解决依赖

yarn add [package]						// 添加指定的包

### artisan

| artisan 命令                               | 说明           |
| ---------------------------------------- | ------------ |
| php artisan key:generate                 | 生成 App Key   |
| php artisan make:controller              | 生成控制器        |
| php artisan make:model Models/Article -m | 生成模型 + 迁移文件  |
| php artisan make:policy                  | 生成授权策略       |
| php artisan make:seeder UsersTableSeeder | 生成 Seeder 文件 |
| php artisan migrate                      | 执行迁移         |
| php artisan migrate:rollback             | 回滚迁移         |
| php artisan migrate:refresh              | 重置数据库        |
| php artisan db:seed --class=UsersTableSeeder | 填充数据库        |
| php artisan tinker                       | 进入 tinker 环境 |
| php artisan route:list                   | 查看路由列表       |
| php artisan serve                        | 启动Laravel    |
| php artisan make:migration add_is_admin_to_users_table --table=users |              |
| php artisan make:factory StatusFactory   |              |
| php artisan make:auth                    | 生成认证脚手架      |
| php artisan vendor:publish               | 生成配置文件       |
| php artisan make:request UserRequest     | 表单请求验证       |
|                                          |              |
|                                          |              |

### HTTP操作

| 操作     | 介绍   |
| ------ | ---- |
| GET    | 页面读取 |
| POST   | 数据提交 |
| PATCH  | 数据更新 |
| DELETE | 数据删除 |

### yarn 和 npm

| yarn 命令                                  | 解释                 | npm 命令             |
| ---------------------------------------- | ------------------ | ------------------ |
| yarn install --no-bin-links              | 安装扩展包              |                    |
|                                          | 将 .scss 文件编译为 .css | npm run dev        |
|                                          |                    | npm run watch-poll |
| yarn add [package]                       |                    |                    |
| yarn config set registry https://registry.npm.taobao.org |                    |                    |
|                                          |                    |                    |
|                                          |                    |                    |
|                                          |                    |                    |
|                                          |                    |                    |



### Composer

composer是PHP中用来管理依赖（dependency）关系的工具。

迁移的好处：

- 多人并行开发；
- 代码版本管理；
- 数据库版本控制 —— 如：回滚/重置/更新等；
- 兼容多种数据库系统；
- 部署方便。

### RESTful 架构

​	Laravel 遵从 RESTful 架构的设计原则，将数据看做一个资源，由 URI 来指定资源。对资源进行的获取、创建、修改和删除操作，分别对应 HTTP 协议提供的 GET、POST、PATCH 和 DELETE 方法。当我们要查看一个 id 为 1 的用户时，需要向 `/users/1` 地址发送一个 GET 请求，当 Laravel 的路由接收到该请求时，默认会把该请求传给控制器的 `show` 方法进行处理。

```
Route::resource('users', 'UsersController');
```

上面代码将等同于：

```
Route::get('/users', 'UsersController@index')->name('users.index');
Route::get('/users/{user}', 'UsersController@show')->name('users.show');
Route::get('/users/create', 'UsersController@create')->name('users.create');
Route::post('/users', 'UsersController@store')->name('users.store');
Route::get('/users/{user}/edit', 'UsersController@edit')->name('users.edit');
Route::patch('/users/{user}', 'UsersController@update')->name('users.update');
Route::delete('/users/{user}', 'UsersController@destroy')->name('users.destroy');
```

可以看到使用 `resource` 方法让我们少写了很多代码，且严格按照了 RESTful 架构对路由进行设计。

生成的资源路由列表信息如下所示：

| HTTP 请求 | URL                | 动作                      | 作用          |
| ------- | ------------------ | ----------------------- | ----------- |
| GET     | /users             | UsersController@index   | 显示所有用户列表的页面 |
| GET     | /users/{user}      | UsersController@show    | 显示用户个人信息的页面 |
| GET     | /users/create      | UsersController@create  | 创建用户的页面     |
| POST    | /users             | UsersController@store   | 创建用户        |
| GET     | /users/{user}/edit | UsersController@edit    | 编辑用户个人资料的页面 |
| PATCH   | /users/{user}      | UsersController@update  | 更新用户        |
| DELETE  | /users/{user}      | UsersController@destroy | 删除用户        |

### 文件冲突

这时我们运行 Git 的状态检查命令，会发现新增了一个 `public/css` 文件夹。这是为什么呢？

```
$ git status
git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   public/css/app.css

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    public/mix-manifest.json

no changes added to commit (use "git add" and/or "git commit -a")
```

原因是我们在前面使用了 `npm run watch-poll` 来监听文件的更改并持续编译，因此当我们切回到主分支上时，`app.scss` 文件会回到一开始在主分支上未经过任何修改的状态，这时 `app.scss` 文件被 watch-poll 任务检测到发生更改，便会对该文件进行编译，最终产生一个 `public/css/app.css` 文件。但这个文件对我们来说是个累赘，没有一点作用，原因如下：

1. 新编译的 `app.css` 文件没有包含我们本章新添加的样式代码，因为它是由初始状态的 `app.scss` 文件编译成的。
2. 在 Git 分支 `filling-layout-style` 上已新增了 `app.css`，若将该分支与主分支进行合并，将导致文件冲突的情况发生。

解决冲突的方法很简单，先检出 Master，放弃所有文件修改，再进行分支合并即可。

```
$ git checkout .
$ rm -f public/mix-manifest.json
$ git merge filling-layout-style
```

### Facades

门面的思想。门面是介于一个类的实例化与没有实例化中间的一个状态。其实是类的一个接口实现。在这个状态下可以不实例化类但是可以调用类中的方法。

在laravel中不仅仅是Input门面可以获取用户的输入，Request门面也可以获取用户输入的，其语法和Input一样，也存在get、all、only等方法。

### 会话 Session

由于 HTTP 协议是无状态的，我们无法在两个页面之间保证用户身份的同步，因此我们需要借助会话在浏览器中临时存储用户的身份信息，进而保证在同一浏览器中，用户在不同页面具有相同的登录状态。

### 中间件 Middleware

Laravel 中间件 (Middleware) 是 HTTP 中间件，这是一种程序设计模式，为我们提供了一种非常棒的过滤机制来过滤进入应用的 HTTP 请求，例如，当我们使用 Auth 中间件来验证用户的身份时，如果用户未通过身份验证，则 Auth 中间件会把用户重定向到登录页面。

### CSRF 攻击

在表单项中添加隐藏域，name值为_token，其value值是{{csrf_token}}。当然也可以简化写整个input隐藏域都直接交由laravel生成：使用{{csrf_field()}}。

Csrf_token：返回的是csrf的token值。【主要一般用于js/ajax等位置】

Csrf_field：则表示生成整个隐藏域。【一般用在form标签中】

### AR 模式

Active Record 是一种领域模型模式，其特点是一个模型类对应关系型数据库中的一个表，模型类的一个实例对应表中的一行记录。Active Record 最大优点是允许我们简单, 直观地操作数据层。

**AR模式三个核心**（映射）：

​	每个数据表                         与数据表进行交互的Model模型映射（实例化模型）

​	记录中的字段                     与模型类的属性映射（给属性赋值）

​	表中的每个记录                 与一个完整的请求实例映射（具体的CURD操作）

> 第一：定义一个$table属性，值是不要前缀的表名，如果不指定则使用类名的复数形式作为表名。
>
> 第二：定义$primaryKey属性，值是主键名称，如果需要使AR模式的find方法，则需要指定主键（Model::find(n)）。
>
> 第三： 定义$timestamps属性，值是false,如果不设置为false，则默认会操作表中的created_at和updated_at字段,我们表中一般没有这两个字段，所以设置为false,表示不要操作这两个字段。
>
> 第四：定义$fillable属性，表示使用模型插入数据时，允许插入到数据库的字段信息。

注意：使用模型中create插入数据时，要设置$fillable允许入库的字段，使用$guarded是设置排除入库的字段。

toArray()方法，作用针对对象，如果对象的打印结果看的不是很明显，则可以使用toArray方法，将对象转化成数组，以便观察。

注意：在laravel里面如果要删除数据，必须先根据主键id查询对应的记录，返回一个对象，然后调用对象的delete方法即可。

### 自动验证

一般一个框架都会提供自动验证的机制，在TP里面的验证的规则是写在模型里面进行验证的，但是自laravel里面的思想有些不一样，它的验证规则可以在控制器里面，也可以单独的写一个专门的验证文件。并且laravel里面的验证不通过情况下的提示信息和表单数据是保存在session里面的，并且验证不通过的情况下会跳到上一个页面。

confirmed:验证两个字段是否相同，如果验证的字段是password,则必须输入一个与之匹配的password_confirmation字段

### Eloquent ORM

Eloquent 提供了简洁优雅的 ActiveRecord 实现来跟数据库进行交互。Active Record 是一种领域模型模式，其特点是一个模型类对应关系型数据库中的一个表，模型类的一个实例对应表中的一行记录。Active Record 最大优点是允许我们简单, 直观地操作数据层。

### migration 迁移

迁移的好处：

- 多人并行开发；
- 代码版本管理；
- 数据库版本控制 —— 如：回滚/重置/更新等；
- 兼容多种数据库系统；
- 部署方便。

### 文件上传

enctype="multipart/form-data"

在 Laravel 中，我们可直接通过 [请求对象（Request）](https://d.laravel-china.org/docs/5.5/requests#retrieving-uploaded-files) 来获取用户上传的文件，如以下两种方法：

```
// 第一种方法
$file = $request->file('avatar');

// 第二种方法，可读性更高
$file = $request->avatar;
```

### Errors

`MassAssignmentException` - 批量赋值异常，这是因为我们未在微博模型中定义 `fillable` 属性，来指定在微博模型中可以进行正常更新的字段，Laravel 在尝试保护。

1. 编码错误

```
$ php artisan migrate
Migration table created successfully.
In Connection.php line 664:
  SQLSTATE[42000]: Syntax error or access violation: 1071 Specified key was t
  oo long; max key length is 1000 bytes (SQL: alter table users add unique
  users_email_unique(email))
In Connection.php line 458:
  SQLSTATE[42000]: Syntax error or access violation: 1071 Specified key was t
  oo long; max key length is 1000 bytes
```

手动配置迁移命令`migrate`生成的默认字符串长度，在`AppServiceProvider`中调用`Schema::defaultStringLength`方法来实现配置：

```
    use Illuminate\Support\Facades\Schema;

    /**
* Bootstrap any application services.
*
* @return void
*/
public function boot()
{
   Schema::defaultStringLength(191);
}
```
# PHP

> 类的三大特性：封装性、继承性、多态性、抽象性。

密码加密：md5、sha1

### 1. 面向对象

类是由相同属性和方法构成。对象也是由属性和方法构成的。

类中只有两个内容：成员属性和成员方法。

类几乎不占内存，但每个对象都要占用内存空间。

PHP中$this变量代表当前对象，用来调用对象的属性和方法。

$this只能在成员方法中存在，其它地方都不能使用。

类常量，就是类的常量，与对象无关。

静态属性，就是类的属性，与类相关，与对象无关；

静态方法，就是类的方法，与类相关，与对象无关；

静态属性和静态方法，在内存中只有一份，不会随着对象的增加而增加。

好处：节省内存。可以被所有对象去共享。

self用来调用类的东西：类常量、静态属性、静态方法。

构造方法一定没有返回值，不要使用return语句。

析构方法的作用：垃圾回收。

### 2. 访问修饰符

作用：主要用来保护数据的安全。

​	public(公共权限)：在任何地方都可以访问，主要指类内部、类外部、子类中都可以访问。

​	protected(受保护的权限)：只能在本类中、子类中被访问，在类外不能访问。

​	private(私有的权限)：只能在本类中被访问，在类外、子类中都无权访问。

### 3. 值传递

PHP有8种数据类型：

​	标量数据类型：字符串型、整型、浮点型、布尔型；

​	复合数据类型：数组、对象

​	特殊数据类型：资源、NULL

其中，字符串型、整型、浮点型、布尔型、数组，默认都是“值传递”。

其中，对象、资源，默认都是“引用传递”。

在PHP中，使用“&”符号，可以将标量数据类型和数组，变成“引用传地址”。

### 4. 封装性

类的封装性：将敏感数据保护起来，不被外界访问。

类的封装性实现，就是通过权限控制符来实现。

### 5. 函数

**date('N')**	ISO-8601 格式数字表示的星期中的第几天(PHP 5.1.0 新加)，*1*(表示星期一)到 *7*(表示星期天)。

**str_replace**    ( [mixed](mk:@MSITStore:C:\Users\KpZhang\Desktop\php_enhanced_zh.chm::/res/language.pseudo-types.html#language.types.mixed) `$search`   , [mixed](mk:@MSITStore:C:\Users\KpZhang\Desktop\php_enhanced_zh.chm::/res/language.pseudo-types.html#language.types.mixed) `$replace`   , [mixed](mk:@MSITStore:C:\Users\KpZhang\Desktop\php_enhanced_zh.chm::/res/language.pseudo-types.html#language.types.mixed) `$subject`   [, int `&$count`  ] )

该函数返回一个字符串或者数组。该字符串或数组是将 `subject` 中全部的 `search` 都被 `replace` 替换之后的结果。 

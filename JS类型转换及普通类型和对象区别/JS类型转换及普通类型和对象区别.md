# 类型转换及普通类型和对象的区别

## 类型转换

总结一下这节课讲的数据类型的转换：

### 1.转换成String

+ String()

+ toString()    每个对象都会有这个方法

注意：object.toString()   打印出  [object Object]

+ console.log()   会隐式调用toString()

+ 对象的属性访问也会隐式地调用toString()

+ 老司机用 `+ ''`的方式    这个甚至可以转换null和undefined



### 2.boolean

+ Boolean()

+ 老司机方法：

  ！！ 可以快速变成boolean 比如  `!! 2` //true

总结一下5个falsy值：

+ 0

+ NaN是false

+ ‘’
+ null
+ undefined

其他都是true



### 3.Number

+ Number()

+ parseInt('1',10)     parse的意思是解析    这里后面的是进制，这里会有一个坑   parseInt('011') =>11   parseInt('011',2) => 3            //整数，后面指定进制

+ parseFloat('1.23')             //浮点数

+ 比较骚的方法：

1. `- 0`    减0
2. `+ '1'`   前面加正号  





## 普通类型和对象的区别

普通类型包括：

+ Number
+ String
+ Boolean
+ null
+ undefined
+ symbol

复杂类型：

+ 对象

### 区别

1. 简单数据类型的值只是上述6种的一个，而复杂数据类型，它键值对的值可以是上述7种的一个（包括对象）

2. 普通类型存储在栈内存中，复杂类型（对象）存储在堆内存中

3. 声明一个普通变量，就是将值赋给该变量；声明一个对象变量，实际上是将对象地址的引用赋值给变量

4. 使用typeof xxx  ，普通类型会显示该类型，比如Number，String等，复杂数据类型则会显示Object

   例外：  

   + typeof null  // object

   + typeof fn（函数）   // function

5. Number，String，Boolean属于基本包装类型，再使用`.`运算符访问属性的时候会隐式转换成对象

   
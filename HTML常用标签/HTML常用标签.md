# HTML常用标签

[HTML5 标签列表](<https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5/HTML5_element_list>)。MDN里面已经分类并且介绍得很好了，就不再当搬运工了。下面讲一下我的理解。



## 1.整体结构

HTML的整体结构,通常由 html, head, meta, title, link( style ), script, body构成

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <link rel="stylesheet" href="style.css">
    <script src="script.js"></script>
</head>
<body>
    ...
</body>
</html>
```

### 

## 2.内容与语义化

html的语义化可以体现在以下三方面

### 2.1自然语言表达能力的补充

举例em和strong     贴一段w3s的介绍：

如果常识告诉我们应该较少使用 `<em> `标签的话，那么 `<strong>` 标签出现的次数应该更少。如果说用` <em>` 标

签修饰的文本好像是在大声呼喊，那么用 `<strong>` 标签修饰的文本就无异于尖叫了。沉默寡言的人说出的话总

是一诺千金，与此相同，限制` <strong>` 的使用可以令应该更加引人注意，而且更加有效。

举一个例子，经常访问 W3school 的用户可以注意到了，许多教程页面的第一句摘要都是以粗体显示的，而实际

上，我们对这一句摘要使用了 `<strong>` 标签。使用这个标签的理由是，我们认为教程摘要不仅概括了其所在页

面的内容，而且位于页面的最重要的位置，其内容自然是非常重要的且值得强调的。

个人理解对于平时可以多留意一下，是否能够使用em和strong这样的语义化标签



### 2.2作为标题摘要

h1-h6是最基本的标题，一篇文档会有一个**树形的目录结构**，它由各个级别的标题组成。这个树形结构可能不会跟HTML元素的嵌套关系一致。因此正确运用标题标签还是重要的。
注意2点：

 ① 要产生副标题的时候用`<hgroup></hgroup>`包围 

```html
<hgrounp>
<h1>JavaScript对象</h1>
<h2>我们需要模拟类吗？</h2>
<p>balah balah</p>
</hgrounp>
```

会生成以下标题结构：
● JavaScript 对象----我们需要模拟类吗？
     ○  ...

 ② section标签包裹的标题会出现降级，比如

```html
<section>
  <h1>HTML语义</h1>
  <p>balah balah balah balah</p>
    <section>
        <h1>弱语义</h1>
        <p>balah balah</p>
    </section>
    <section>
        <h1>结构性元素</h1>
        <p>balah balah</p>
    </section>
</section>
```

会出现这样的结构：
●  HTML 语义
  ○  弱语义
  ○  结构性元素

### 2.3作为整体结构

```html

<body>
    <header>
        <nav>
            ...
        </nav>
    </header>
    <aside>
        <nav>
            ...
        </nav>
    </aside>
    <section>...</section>
    <section>...</section>
    <section>...</section>
    <footer>
        <address>...</address>
    </footer>
</body>
```

介绍一些这些新的语义化标签：

●  header，通常出现在前部，表示导航或者介绍性的内容

●  footer，通常出现在尾部，包含一些作者的信息，相关连接，版权信息

上面2者一般放在atricle，或者放在body中

●  aside，表示根文章主题不那么相关的部分，他可能包含导航，广告等工具性质的内容

aside很容易理解为侧边栏，实际上它也可以不是侧边栏

注意：header和footer都可能出现导航(nav 标签)，二者的区别是，header中的导航多数是到文章自己的目录，

而aside中的导航多数是到关联页面或者是整站地图。

最后，footer中包含address，这是个非常容易被误用的标签。它表示的是联系方式，只关联到body和article标

签。



## 3.几个需要关注的标签

**iframe**

- 用得少    也是可替换元素
- 第一种用法是直接src连接到某个页面
- 第二种用法是和a标签一起使用，a使用target，值和iframe的name一样，这样a里面的src就会在iframe里面打开

**a**

- target    blank， self ，parent ，top

- download属性    设置了之后会下载该页面

- 不写download，也要它下载的时候，后台在响应的时候设置content-type为application/octet-stream

- href属性

  - href="qq.com"    相对路径，会找qq.com这个文件

  - href="http://qq.com"   或者   href=”https://qq.com“    href="//qq.com"（现在是什么协议就用什么协议打开，文件协议，http协议--用http-server打开的时候）

  - href="xxx.html"    相对，以目录为参考

  - href=“#aaa”   写一个锚点，页面内跳转，体现为加到index.html后面

  - href=“?name=holiday”    加到路径后面，会发请求，事实上，除了#aaa之外的都会发请求

  - href="javascript:;"   伪协议，历史遗留，以前用它点击触发js； 现在用它写一个链接，但是不触发任何行为的需求

    

**form**

1. form

   `form action="xxx" method='POST'`

   - 和a一样，都是跳转页面，不过a是get请求，form是post请求

   - form里面没有提交按钮，那么你将无法提交这个form

   - form里面的表单元素必须要有name属性才会被提交

   - 使用 x-www-form-urlencoded    编码方式，除了英文会有一个特殊的编码，比如中文

   - method可以是get，此时如果提交，没有请求正文，但是有查询参数

   - action里面user？name=holiday

   - file协议不支持post方法

   - 他也有target属性，跳转到一个页面

     



**input和button**

- input里面不能有子元素

- 如果form标签里面没有input的submit类型，但是有button标签，这个button会升级为提交按钮

- form 的input的类型是button时，他不是提交按钮

- type

  - text       加一个label，label和for和input的id绑定起来     老司机直接用label包起来
  - password
  - checkbox       value设置选中时候的值
  - radio        value值设置选中时候的值，name相同的时候，只能有一个被选中 

  textarea

  - 推荐使用css控制resize：none；width：100px；height：100px

  

**table**

- colgroup
  - col  width='100'   第一列
  - col width='200'    第二列
- thead
  - th
- tbody
  - th
  - td
- tfoot
  - th
  - td

合并表格边框：

border-collapse：collapse


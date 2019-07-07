# css布局之左右，左中右，水平居中，垂直居中

下面介绍如何用css做出：

1. 左右布局

2. 左中右布局
3. 水平居中
4. 垂直居中
5. 其他



## 1.左右布局

### flex  

HTML

```html
<div class='parent'>
<div class='children1'></div>
<div> class='children2'</div>
</div>
```

CSS

```css
.parent{
	dispaly:flex;
	justify-content:space-between
}
.parent > div{
    width:100px;
    height:100px;
    background-color:red;
}
```

### float

html

```html
<div>
	<div class='left'>left</div>
	<div class='right'>right</div>
</div>
```

css

```css
.left{
    float:left;
}
.right{
    float:right;
}
```



## 2.左中右布局(两侧固定，中间自适应)

### float + margin

html

```html
<!-- 注意中间的部分写在下面，不然浮动会顶着div，在他下面 -->
<div id='content clearfix'>
	<div class='left'></div>
	<div class='right'></div>
	<div class='middle'></div>    
</div>
```

css

```css
/* 关键是利用了浮动和盒子模型，左右元素浮动，加上给中间的元素一个margin的left 和 right */     
#content{
            height:300px;
        }
        .left{
            width: 200px;
            height:100%;
            float: left;
            background-color: #00a0dc;
        }
        .middle{
            height:100%;
            margin-left:200px;
            margin-right: 300px;
            background-color: red;
        }
        .right{
            height:100%;
            width: 300px;
            float: right;
            background-color: #00a0dc;
        }
```



### float + absolute

html

```html
<div id="content clearfix">
    <div class="left"></div>
    <div class="middle"></div>
    <div class="right"></div>
</div>
```

css

```css
/* 关键点是利用float，然后结合absolute，设置top：0，bottom：0，left：200px，right：300px .这里利用了absolute设置上下0，0相当于100%父级元素高度，左右同理 */  
#content{
            position: relative;
            width:100%;
            height:300px;
        }
        .left{
            float: left;
            width: 200px;
            height:100%;
            background-color: #00a0dc;
        }
        .middle{
            position: absolute;
            top:0;
            bottom:0;
            left:200px;
            right: 300px;
            background-color: red;
        }
        .right{
            float: right;
            height:100%;
            width: 300px;
            background-color: #00a0dc;
        }
```



### flex

html

```html
<div id="content">
    <div id="left"> </div>
    <div id="middle"></div>
    <div id="right"></div>
</div>
```

css

```css
/* 容器属性：display:flex */
/* 项目属性：flex（grow，shrink，basis） 这里在中间的项设置了flex：1，即存在多余空间，则全部分给他（因为其他item默认是0）*/
#content {
            display: flex;
            width: 100%;
            height: 200px;
        }
        #left {
           flex:0 0 200px;
            height: 100%;
            background-color: #f04d0d;
        }
        #middle{
            flex: 1;
            background-color: blue;
        }
        #right {
            flex:0 0 200px;
            height: 100%;
            background-color: #f04d0d;
        }
```



另外还有什么[圣杯布局，双飞翼布局](https://www.cnblogs.com/imwtr/p/4441741.html)，也是两侧固定，中间自适应。圣杯布局原理其实就是float + 负margin + padding + relative设置移动2边，双飞翼则是float + 负margin + middl外围增加标签，然后设置margin，详情看链接。



## 3.水平居中

### flex

父元素设置：

```css
.parent{
 	display:flex;
    justify-content:center
}
```

### 块级元素

margin：0 auto

### 行内元素

text-align：center



## 4.垂直居中

### flex

父元素

```css
.parent{
    display:flex;
    align-items:center
}
```

### table

html

```css
<div class='container'>
  <div class='wrapper'>
    <div class='headline'>
     <span>这是标题</span>
    </div>
    <div class='content'>
      <span>这是内容,这段文字在当前页面垂直水平       居中</span>
    </div>
  </div>
</div>
```

css

```css
.container{
  text-align:center;
  width:500px;
  border:3px solid yellow;
  height:500px;
  display:table;
}

.wrapper{
  display:table-cell;
  vertical-align:middle
}
.headline{
  border:3px solid red;

}
.content{
  border:3px solid green
}
```



### 块级元素

通过计算

### 行内元素

**文字**：

1.设置line-height = height

2.下面也是一种方法

html

```html
<div class="d">
<p>内联元素的居中实现</p>
</div>
```

css

```css
.d {
    height: 600px; 
    width: 600px; 
    text-align: center; 
    border: 1px solid #ccc;
}	   
.d:before{ 
    content: "."; 
    height: 100%; 
    display: inline-block; 
    vertical-align: middle; 
    visibility: hidden; 
}
.d p { 
    background:#000; 
    color:#fff; 
    vertical-align:middle; 
    display:inline-block;
}
```

**图片**：

```css
img{
	dispaly:inline-block;
	vertical-align: middle;
}
```



## 5.其他

布局的传统解决方案：基于盒模型，通过`display + position + float`实现

但是，flex和grid布局的出现使得布局会变得更简单：

可以考虑多使用flex布局

可以学习新的布局--grid布局






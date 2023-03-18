# iframe微前端

最近入职了一家新的公司，然后接触了新的项目，里面涉及到了不同部门和业务侧共同构建一个系统的问题，为了不同团队能够相互配合并独立开发和部署各自的模块，自然而然就使用了所谓的微前端的方案去解决这个问题



## 什么是微前端

微前端是一种前端架构模式，旨在将大型Web应用程序分解为更小、可管理和可维护的部分。 它可以帮助团队以更加松散耦合的方式构建和交付不同的功能模块，每个模块都是独立开发、测试、运行和部署的。

微前端将前端应用程序划分为独立的模块，每个模块都是一个自包含的应用程序，包括所有需要运行的HTML、CSS、JavaScript和其他资源。然后，这些模块可以组装成完整的应用程序，并交互地工作。

微前端的主要优点是：

1. 技术栈无关性： 每个微前端模块都可以使用**不同的技术栈**，因此团队可以选择最适合自己的技术，而不必担心影响整个应用程序。
2. 更好的可维护性：通过将应用程序拆分为**独立的部分**，可以减少代码库的复杂性，从而使其更易于理解、测试和维护。
3. 更快的交付：**不同的团队**可以**独立开发和部署**各自的模块，从而实现**并行开发，加快交付速度**。

微前端是一种有利于团队协作、提高开发效率和维护性的前端架构模式

而目前东家实现微前端的方式是iframe



## 什么是`iframe`微前端

这是实现微前端的其中一种方式。简单来说就是在A网站中通过各个`iframe`元素嵌入不同的网站，这些不同的网站往往是庞大系统中不同的模块，最后去构成一个完整的，规模较大的系统。而这些模块可能是不同的团队去构建，也可能使用了不同的技术栈，可以独立开发和部署



## 介绍一下`iframe`元素

当网页中包含一个iframe元素时，实际上是在当前页面中嵌入了一个独立的HTML文档。这种技术称为内嵌框架，可以用于在一个网页中同时显示多个网页或者在同一个页面内放置广告。

iframe标签使用起来非常简单，只需要在HTML文档中使用`<iframe>`标签，并将src属性设置为要嵌入的网页的URL即可。例如：

```jsx
<iframe src="<http://example.com>"></iframe>
```

除了src属性，iframe标签还支持其他的一些属性，如width、height、frameborder、scrolling等，可以用来控制框架的大小、边框、滚动条等外观和行为特性。

当iframe元素被嵌入到页面中之后，可以通过JavaScript来操作它。比如，可以用以下代码获取iframe元素的引用：

```jsx
var iframe = document.getElementById('myFrame');
```

然后就可以通过iframe对象来访问嵌入的文档中的各种元素和数据了，例如：

```jsx
var el = iframe.contentDocument.getElementById('some-element');
```

需要注意的是，如果iframe元素中嵌入的文档和当前页面属于不同的域，那么它们之间的通信将会受到同源策略的限制。只有在同一域名下的文档才能相互访问和操作，如果需要在跨域的情况下进行通信，可以使用一些其他的技术手段，如postMessage API、跨域资源共享(CORS)等



## 引申：同一域名下的文档如何相互访问和操作

同一域名下的文档相互访问和操作是指在两个或多个文档的URL协议、主机名和端口号都相同时，它们之间可以通过JavaScript进行相互访问和操作。

1.父窗口访问和操作`iframe`窗口

```jsx
// 获取 iframe 元素
const iframe = document.getElementById('my-iframe');

// 获取 iframe 内的某个元素
const elementInIframe = iframe.contentWindow.document.getElementById('element-in-iframe');

// 修改 iframe 内的某个元素的属性
elementInIframe.style.color = 'red';
```

2.iframe访问和操作父窗口

在同源的情况下，iframe内部的文档可以通过 `window.parent`属性来获取其父窗口对象，并进行一些简单的操作，比如修改父窗口中某个元素的样式（包括DOM元素的增删查改）等。例如：

```jsx
// 获取父窗口中的元素
const parentElement = window.parent.document.getElementById('parent-element');

// 修改父窗口中某个元素的样式
parentElement.style.backgroundColor = 'red';
```



## iframe窗口和父窗口是如何通信的

这里将涉及到一个关键的API—`window.postMessage`

### 作用

可以实现在2个建立了联系（使用iframe或者window.open的方式）的窗口中进行通信（双方互传数据）

### 前提

在发送 `postMessage`消息之前，发送者和接收者需要先**建立联系**，也就是通过 `window.open()`或者 `iframe`这两种方法方式打开两个窗口

### 安全实践（必须）

1. 在发送 `postMessage` 消息时需要指定目标窗口的 origin，以保证只有指定来源的窗口才能接收到消息。
2. 在接收 `postMessage` 消息时需要进行身份验证，避免受到来自不安全源的攻击。

### 例子

1.`window.open()` 方式

在父窗口（A.html）中：

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Parent Window</title>
</head>
<body>
    <h1>Parent Window</h1>
    <button onclick="sendPostMessage()">Send Message to Child Window</button>
    <script>
        function sendPostMessage() {
            const childWindow = window.open('<http://localhost:8080/B.html>', 'childWindow');
            childWindow.postMessage('Hello from parent window!', '<http://localhost:8080>');
        }
    </script>
</body>
</html>
```

在子窗口（B.html）中：

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Child Window</title>
</head>
<body>
    <h1>Child Window</h1>
    <script>
        window.addEventListener('message', function(event) {
            if (event.origin !== '<http://localhost:8080>') {
                return;
            }

            console.log('Receive message:', event.data);

						// 回复消息
					  var replyData = { message: 'Hi, example.com!' };
					  event.source.postMessage(replyData, event.origin);
        }, false);
    </script>
</body>
</html>
```

2.`iframe`方式

前提：页面A和页面B在同一域名

```jsx
html
<!-- Parent Window A.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Parent Window A</title>
</head>
<body>
    <h1>Parent Window A</h1>
    <button onclick="sendPostMessage()">Send Message to Child Window</button>
    <iframe src="B.html" id="childIframe"></iframe>
    <script>
        function sendPostMessage() {
            const childIframe = document.getElementById('childIframe');
            childIframe.contentWindow.postMessage('Hello from parent window!', window.origin);
        }
    </script>
</body>
</html>
<!-- Child Window B.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Child Window B</title>
</head>
<body>
    <h1>Child Window B</h1>
    <script>
        window.addEventListener('message', function(event) {
						if (event.origin !== "<https://example.com>")
					    return;

            console.log('Receive message:', event.data);

						// 回复消息
					  var replyData = { message: "Hi, example.com!" };
					  event.source.postMessage(replyData, event.origin);
        }, false);
    </script>
</body>
</html>
```

这个示例代码中，当点击按钮后，父窗口会向嵌入的 `iframe` 发送一个消息，并指定了消息的目标窗口为 `childIframe.contentWindow`，同时指定了 `window.origin` 为消息的来源。在嵌入的 `iframe` 中监听 `message` 事件，将接收到的消息打印出来。

需要注意的是，`iframe` 的源必须与父窗口的源相同，否则 `postMessage` 将无法传递消息。另外，在进行跨域消息传递时，应该对消息来源进行验证，避免接收到来自不安全源的恶意消息。

## iframe父子窗口如何获取对方的窗口对象

### 1.获取iframe窗口对象

在网站 B 中，如果它被嵌入到网站 A 的 iframe 中，可以通过 `window.parent`属性来获取包含它的窗口对象（即网站 A 的窗口对象）

```jsx
var parentWindow = window.parent;
```

### 2.iframe获取父窗口对象

```jsx
var iframeWindow = window.frames['iframeId'].contentWindow;
```

其中，`iframeId`是 `iframe`元素的 ID 属性

### 拓展：通过window.open打开的网站如何获得来源

返回打开当前窗口的那个窗口的引用，例如：在 window A 中打开了 window B，B.opener 返回 A



## 拓展阅读

### 实现微前端的几种方式

微前端是一种将复杂的单体应用分解成多个小型、独立运行的应用，并通过组合方式形成完整应用的架构模式。下面是实现微前端的一些方法：

1. 使用 Web Components：使用 Web Components 技术，将应用拆分成多个组件，这些组件可以独立开发、测试和部署，然后再通过组合的方式将它们组装成一个完整的前端应用。
2. 使用框架集成：在微前端架构中，使用不同的框架来构建各个子应用，每个框架可以被看作一个独立的前端应用。将不同的前端应用通过协议集成到一个整体，形成了一个完整的前端应用。
3. 使用 Iframe：使用 Iframe 在一个父级页面中加载各个子应用，这些应用互相独立，能够独立开发、测试和部署。由于每个子应用都在独立的 Iframe 中运行，因此它们的样式和 JavaScript 等资源之间不会相互干扰，能够避免版本冲突的问题。
4. 使用服务端的聚合：将微前端视为多个独立的前端应用，通过服务端聚合实现请求路由和响应组装，这样每个子应用可以独立开发、测试和部署，而整个应用则可以通过服务端聚合来组合显示。

需要注意的是，微前端架构是一种折衷的方式，需要在开发团队和项目的具体情况下选择最适合自己的方案。

目前公司系统使用的是第三种方式，而笔者在前司的时候，他们搭建一个多业务侧共同构建的系统，使用的是第四种方式

但是事实上，第二种框架集成的方式会更加方便和可控，比如使用微前端框架qiankun，很多东西，比如共享代码，路由方式切换子应用，子应用间的通信，整个app的监控方面都做了比较的集成。下面也简单介绍微前端框架qiankun

### 关于微前端框架qiankun

qiankun是一个基于微前端架构的JavaScript框架，它由蚂蚁金服前端团队开发，旨在解决多个独立应用之间的集成问题。qiankun提供了一种简单、高效、稳定的前端微服务化方案，可以将多个独立的前端应用程序组合成一个整体，并且能够保持彼此独立

qiankun框架具有以下特点：

1. 模块化打包：每个子应用程序可以独立地进行打包和部署，因此可以使用不同的技术栈和工具链。同时，qiankun提供了自动化构建和动态加载等功能，可以实现更灵活的模块化管理。
2. 共享框架：qiankun框架提供公共的框架代码，可以减少每个子应用程序的重复代码，同时使得整个应用程序更加稳定和可靠。
3. 动态路由：qiankun支持动态路由，可以根据需要动态加载和卸载子应用程序，因此具有更好的扩展性和灵活性。
4. 数据通信：qiankun提供了数据通信机制，可以在不同的子应用程序之间进行数据通信，从而实现更加复杂的交互功能。
5. 前端监控：qiankun框架内置了前端监控功能，可以对整个应用程序进行实时监控和性能分析，从而帮助开发者及时发现和解决问题。

qiankun是一个优秀的微前端框架，具有很强的扩展性和适用性，能够帮助开发者更好地实现前端微服务化
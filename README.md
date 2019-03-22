# 前端面试总结
jquery    
源码博客文章，核心架构，事件委托，插件机制   

vue   
react   
angular   
取一个，原理差不多。细节。vue源码分析。遇到什么问题，怎么解决。   

node.js   
(学好，或者不学)   

sass，less   

gulp,grunt    
通gulp，理解两者区别    

npm   
常见命令，怎么用的   

webpack    
必备，打包，内容比较全   

技术面：    
页面布局，css盒模型，dom事件，http协议，面向对象，原型链（必问），算法()，安全（XSS,CSRF），通信（跨域通信，普通前后端通信）    

## 一、CSS

表格布局    
display:table=>相当于“table”标签；    
display:table-row=>相当于“tr”标签；     
display:table-cell=>相当于“td”   
flex布局    
网格布局display:grid    
浮动布局，绝对定位   

盒模型：    
![images](https://github.com/yusisi/codeStudy/blob/master/images/1.png)

/* 标准模型 */    
box-sizing:content-box;   

/*IE模型*/    
box-sizing:border-box;    

BFC原理：    
　BFC(Block formatting context)直译为"块级格式化上下文"。它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。    

内部的box会在垂直方向，一个接一个的放置   
每个元素的margin box的左边，与包含块border box的左边相接触（对于从做往右的格式化，否则相反）    
box垂直方向的距离由margin决定，属于同一个bfc的两个相邻box的margin会发生重叠    
bfc的区域不会与浮动区域的box重叠   
bfc是一个页面上的独立的容器，外面的元素不会影响bfc里的元素，反过来，里面的也不会影响外面的    
计算bfc高度的时候，浮动元素也会参与计算   

应用场景:   
自适应两栏布局   
清除内部浮动   
防止垂直margin重叠    

生成BFC的元素：   
根元素   
float属性不为none   
position为absolute或fixed   
display为inline-block, table-cell, table-caption, flex, inline-flex    
overflow不为visible   
{
左边元素float:left,右边元素比左边高的时候要加个：overflow:auto,（bfc不与float重叠）
overflow：hidden,子元素不重叠也要加个overflow:hidden
}

## 二、DOM事件类

dom0：直接通过 onclick写在html里面的事件    
dom2：addEventListener绑定的事件, 还有IE下的DOM2事件通过attachEvent绑定;    

DOM2级的事件规定了事件流包含三个阶段包括：     
1：事件捕获，window->document->html->body   
2：处于目标阶段   
3：事件冒泡阶段（IE8以及更早版本不支持DOM事件流）;   

dom3：是一些新的事件, 区别DOM3和DOM2的方法我感觉是DOM3事件有分大小写的    


事件对象event下的属性和方法：   

bubble ： 表明事件是否冒泡   
cancelable ： 表明是否可以取消冒泡   
**currentTarget ： 当前时间程序正在处理的元素, 和this一样的;**    
defaultPrevented： false ，如果调用了preventDefualt这个就为真了;   
detail： 与事件有关的信息(滚动事件等等)    
eventPhase： 如果值为1表示处于捕获阶段， 值为2表示处于目标阶段，值为三表示在冒泡阶段   
**target || srcElement： 事件的目标**   
trusted： 为ture是浏览器生成的，为false是开发人员创建的（DOM3）    
type ： 事件的类型view ： 与元素关联的window， 我们可能跨iframe；   
**preventDefault() 取消默认事件；**    
**stopPropagation() 取消冒泡或者捕获；**     
stopImmediatePropagation() (DOM3)阻止任何事件的运行；   
**stopImmediatePropagation阻止 绑定在事件触发元素的 其他同类事件的callback的运行**    
IE下的事件对象是在window下的，而标准应该作为一个参数, 传为函数第一个参数;    
IE的事件对象定义的属性跟标准的不同，如：cancelBubble 默认为false, 如果为true就是取消事件冒泡;    
returnValue 默认是true，如果为false就取消默认事件;    
srcElement, 这个指的是target, Firefox下的也是srcElement;     

## 三、http协议   
http报文组成部分：请求行，请求头，空行，请求体，    
http方法：get,post,put,delete,head(获得报文首部)   
get、post区别：   
GET在浏览器回退时是无害的，而POST会再次提交请求。    
GET产生的URL地址可以被Bookmark，而POST不可以。    
GET请求会被浏览器主动cache，而POST不会，除非手动设置。   
GET请求只能进行url编码，而POST支持多种编码方式。   
GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。    
GET请求在URL中传送的参数是有长度限制的，而POST么有。   
对参数的数据类型，GET只接受ASCII字符，而POST没有限制。   
GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。    
GET参数通过URL传递，POST放在Request body中。   

对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；    
而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。    
也就是说，GET只需要汽车跑一趟就把货送到了，而POST得跑两趟，第一趟，先去和服务器打个招呼“嗨，我等下要送一批货来，你们打开门迎接我”，然后再回头把货送过去。   

http状态码：    200 返回正常；304 服务端资源无变化，可使用缓存资源；400 请求参数不合法；401 未认证；403 服务端禁止访问该资源；404 服务端未找到该资源；500 服务端异常，503 请求未完成，服务器过载或当机   

## 四、原型链
创建对象方法：原型，构造函数，实例，原型链   
instanceof原理    
new运算符    

![images](https://github.com/yusisi/codeStudy/blob/master/images/2.png)

![images](https://github.com/yusisi/codeStudy/blob/master/images/3.png)

## 五、面向对象**（重要）
#### 1.构造函数继承：call apply 实现父类的继承（父级的构造函数所有属性指向子类的构造函数，缺点：不能实现继承原型对象的方法）    
#### 3.原型链继承：Child.prototype=new Parent(); new Child().__proto__===Child.prototype   
(缺点：原型链中的原型对象是共用的，一个原型对象变其它也跟着变)    
#### 3.组合继承**(原型链的掌握)    
先子级引入，Parent.call(this);    
//再原型继承，Child.prototype=Parent.prototype(不可用)   
Child.prototype=Object.creat(Parent.prototype)    
Child.prototype.constructor=Child //再给自己写一个constructor    
Object.creat创建的原型对象就是参数，达到父类和子类对象的分离    

## 六、通信类
#### 1.同源策略以及限制    
限制从一个源加载的文档或脚本如何与另一个源的资源进行交互。（用于隔离潜在恶意文件的关键的安全机制）   
 cookie、localstorage和indexDB无法读取  
Dom无法获得  
AJAX请求不能发送  

#### 2.前后端如何通信  
 ajax(同源通信)  
websoket（不受同源限制）  
cors（支持同源和非同源策略）  

#### 3.如何创建ajax
 XMLHttpRequest对象的工作流程  
 
![images](https://github.com/yusisi/codeStudy/blob/master/images/4.png)

![images](https://github.com/yusisi/codeStudy/blob/master/images/5.png)

兼容性处理  
事件的触发条件  
事件的触发顺序  

#### 4.跨域通信的几种方式
http://blog.damonare.cn/2016/12/01/%E5%89%8D%E7%AB%AF%E8%B7%A8%E5%9F%9F%E6%95%B4%E7%90%86/
JSONP（利用script标签的异步加载实现）    

![images](https://github.com/yusisi/codeStudy/blob/master/images/6.png)

Hash    

![images](https://github.com/yusisi/codeStudy/blob/master/images/7.png)

postMessage   

![images](https://github.com/yusisi/codeStudy/blob/master/images/8.png)

WebSocket   

![images](https://github.com/yusisi/codeStudy/blob/master/images/9.png)

CORS    

![images](https://github.com/yusisi/codeStudy/blob/master/images/10.png)

## 七、安全类
#### 1.CSRF跨站请求伪造    
攻击原理：用户在注册网站登录过，从另一个网站进去    
防御措施：token验证（服务器存在本地的），referer验证（页面来源属于自己的站点），隐藏令牌（和token有点像，使用方式有点差别，放在http头中）   
#### 2.XSS跨域脚本攻击   
攻击原理：向页面注入脚本    
防御措施：imooc.com/learn/812    
对输入(和URL参数)进行过滤，对输出进行编码。将容易导致XSS攻击的边角字符替换成全角字符    
https://www.cnblogs.com/digdeep/p/4695348.html    

## 八、算法类
排序（快速排序 ，选择排序，希尔排序）   
https://segmentfault.com/a/1190000009426421   
https://segmentfault.com/a/1190000009366805   
https://segmentfault.com/a/1190000009461832   
堆栈**、队列、链表(juejin.im/entry/58759e....)    
http://blog.damonare.cn/2017/01/16/%E5%AD%A6%E4%B9%A0javascript%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%EF%BC%88%E4%B8%80%EF%BC%89%E2%80%94%E2%80%94%E6%A0%88%E5%92%8C%E9%98%9F%E5%88%97/#more    

http://blog.damonare.cn/2017/01/16/%E5%AD%A6%E4%B9%A0javascript%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%EF%BC%88%E4%B8%89%EF%BC%89%E2%80%94%E2%80%94%E9%9B%86%E5%90%88/   

http://blog.damonare.cn/2017/01/16/%E5%AD%A6%E4%B9%A0javascript%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%EF%BC%88%E5%9B%9B%EF%BC%89%E2%80%94%E2%80%94%E6%A0%91/#more   
递归**(segmentfalt.com/a/119000...)   
波兰式和逆波兰式(理论，源码)   

## 九、渲染机制
渲染机制：     
1.doctype(声明DTD文档类型，有三种HTML5，HTML4.0严格模式：不包括展示性的弃用元素和传统模式)    
2.浏览器渲染过程(DOMTree+styleRules->layout,rendertree:dom位置->painting->display)   
3.reflow重排(更改dom，更改css，更改网站默认字体)      
4.repaint重绘(dom改动，css改动)    

js运行机制    
js单线程   
优先执行同步任务，再执行异步任务(setTimeout)    
任务和队列 Event Loop    

## 十、页面性能
1.资源压缩合并，减少http请求   
**2.非核心代码异步加载（异步加载的方式，和区别）**    
方式：动态脚本加载，defer，async   
区别：defer在html解析之后才执行，async在加载完成后立刻执行    

**3.利用浏览器缓存（缓存的分类，原理）**
分类：强缓存（http协议头：Express，Cache-Control），协商缓存（http协议头：Last-Modifide，If-Modefide-Since，Etag，If-None-Match）    
原理：   
4.使用CDN   
5.预解析DNS    
(强制打开a标签的dns预解析)    
<meta http-equiv="x-dns-prefetch-control" content="on">   
<link rel="dns-prefetch" href="//host_name_to_profetch.com">    

## 十一、错误监控  
1.代码错误捕获    
try.cach  winwod.onerror    
2.资源加载错误捕获    
object.onerror  performance.getEntries()  error事件捕获   
跨域js运行错误捕获：1.在script标签添加crossorigin属性  2.设置js资源响应头Access-Control-Allow-Origin:*   

上报错误的基本原理：1.采用ajax通信方式上报  2.利用image对象上报   

## 十二、职业规划
1.目标：在业务上成为专家，技术上成为大牛  
2.近阶段的目标：不断学习积累各方面的经验，以学习为主  
3.长期目标：做几件有价值的事情，如开源作品，技术框架等  
4.方式方法：先完成业务上的主要问题，做到极致，然后逐步向目标靠拢  


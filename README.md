本文主要是巩固对前端知识点的理解，资料来源于网络和书本，由本人整理

1.position的值，relative和absolute分别是相对于谁进行定位的？<br/>
 ·absolute：绝对定位，以使用position定位的上一层组件（不是static定位的父组件）的左上角点为原点进行<br/>
 定位，如无position定位的上一层组件，则以body左上角点为原点来定位<br/>
 ·relative：相对定位，以组件本身的左上角点（普通流）为原点来定位<br/>
 ·fixed（老的IE不支持）：绝对定位，相对于浏览器窗口或frame进行定位<br/>
 ·static：默认值。无定位，元素出现在正常流中<br/>
 ·sticky：生成粘性定位的元素，容器的位置根据正常文档流计算得出<br/>
 ·图层定位：z-index必须与position属性一起使用，同值按照书写顺序重叠上去<br/>
2.overflow的值：<br/>
 ·hidden：超出长宽的内容不显示<br/>
 ·scroll：无论长款超不超出，都加入滚动条<br/>
 ·auto：视情况决定是否加入滚动条<br/>
3.如何解决跨域问题？<br/>
 在js中，直接使用XMLHttpRequest请求不同域上的数据是不可以的，但是在页面上引入不同域上<br/>
 的js文件却可以，jsonp就是利用这个特性来实现的<br/>
 
 如有个a.html页面，需要通过ajax获取一个不同域上的json数据，假设这个json数据地址是http://example.com/<br/>
 data.php，那么a.html中的代码可以这样写：<br/>
 <script>
 function dosomthing(jsondata){
   ／／处理获得的数据
   }
 </script>
 <script src="http://example.com/data.php?callback=dosomthing"></script><br/>
 被访问网址返回的必须是一个能执行的js文件，所以：<br/>
 <?php
 $callback = $GET['callback'];
 $data = array('a,b,c');
 echo $callback.'('.json_encode($data).'$data).')';
 ?><br/>
 最后该网址输出：<br/>
 dosomthing(['a','b','c'])<br/>
 所以通过http://example.com/data.php?callback=dosomthing得到的js文件，就是之前定义的js文件，<br/>
 就是我们之前定义的dosomthing函数，它的参数就是我们需要的数据，这样就跨域得到了我们想要的数据<br/>
 
 ·JSONP原理：动态插入script标签，通过其引入一个JS文件，这个文件载入成功后会执行我们在url参数中指定的函<br/>
 数，并把我们需要的json数据作为参数传入。<br/>
 由于同源策略限制，xmlHttpRequest只允许请求当前源（域名、协议、端口）的资源，为了实现跨域请求，可以通过<br/>
 script标签实现跨域请求，然后再服务器端输出JSON数据并执行回掉函数，优点是兼容性好，简单易用，支持浏览器<br/>
 和服务器双向通信，缺点是只支持GET请求。<br/>
 <script>
     function createJS(sUrl){
        var oScript = document.createElement('script');
        oScript.type = 'text/javascript';
        oScript.src = sUrl;
        document.getElementsByTagName('head')[0].appendChild(oScript);
     }
     
     createJS('jsonp.js');
     box({
         'name':'test'
     });
     
     function box(json){
         alert(json.name);
     }
	 </script><br/>
     
 .CORS：服务器端对CORS的支持，主要是通过设置Access-Control-Allow-Oringin来进行的，如果浏览器检测到<br/>
 相应的设置，就可以允许Ajax进行跨域访问。<br/>
 ·通过修改document.domain，将子域和主域的document.domain设为同一个主域。前提条件是域名必须属于同一个<br/>
 基础域名，而且所用协议端口要一致，否则无效。<br/>
 ·window.name,window有个name属性，特征是：在一个窗口的生命周期内，窗口载入的所以页面都是共享一个window.<br/>
 name的，每个页面对window.name都有读写权限，window.name持久存在一个窗口载入过的所以页面中<br/>
 ·使用HTML5中引进的window.postMessage方法来跨域传送数据，还有flash、在页面上设置代理页面等。<br/>
 目前来看，window.name的方法既不复杂也能兼容到几乎所有浏览器。<br/>
4、XLM和JSON的区别？<br/>
	1）JSON相对XML来说，数据体积小，传递速度更快点<br/>
	2）JSON与JavaScript交互更方便，更容易解析处理，更好的数据交互<br/>
	3）JSON对数据的描述性比XML差<br/>
	4）JSON速度远快于XML<br/>
5、谈谈你对webpack的看法<br/>
	webpack是一个模块打包工具，可以利用它管理模块依赖，并编译输出模块们所需的静态文件，它能够很好的管理、<br/>
	打包web开发中所用到的HTML、JavaScript、CSS以及各种静态文件（图片、字体），让开发过程更高效，对不同<br/>
	类型的资源，webpack有对应的模块加载器。webpack模块打包器会分析模块间的依赖关系，最后生成了优化且合并<br/>
	后的静态资源。<br/>
	webpack：1）code splitting；2）loader 可以处理各种类型的静态文件，并且支持串联操作<br/>
	webpack是以commonJS形式书写，对AMD/CMD的支持很全面，方便旧项目的代码迁移<br/>
	webpack具有requireJs和browserify功能，有许多自身新特性：<br/>
		1）对CommonJS、AMD、ES语法做了兼容<br/>
		2）对js\css、图片文件等都支持打包<br/>
		3）串联式模块加载器以及插件机制，<br/>
		4）独立的配置文件<br/>
		5）将代码切割成不同的chunk，实现按需加载，降低了初始化时间<br/>
		6）支持SourceUrls和SourceMaps，易于调试<br/>
		7）具有强大的Plugin接口，大多是内部插件，使用灵活方便<br/>
		8）webpack使用异步IO并具有多级缓存<br/>
6、TCP/IP传输的第三次握手和四次挥手策略<br/>
	·三次握手：发送端首先发送一个带SYN标志的数据包给对方，接收端接受后，回传一个带有SYN/ACK标志的数据包传达确认信息。<br/>
	最后，发送端再回传一个ACK标志数据包，代表握手结束。若握手过程中某个阶段莫名中断，TCP协议会再次重复同样步骤。<br/>
	
	·四次挥手：主动关闭方发送一个FIN，用来主动关闭数据传送；被动方收到FIN包后，发送一个ACK给对方，确认序号+1<br/>
	被动关闭方发送一个FIN；主动方收到后发送一个ACK，序号+1，完成四次挥手。<br/>
7、TCP和UDP的区别<br/>
	·TCP（transmission control protocol,传输控制协议）：在正式发送数据之前，必须建立可靠连接。<br/>
	·UDP（user data protocol,用户数据报协议）：面向非连接的协议，不与对方建立连接，直接发送数据，适用于少量数据传输。<br/>
8、对作用域链的理解<br/>
	作用域链的作用是保证执行环境里有权访问的变量和函数是有序的，作用域链的变量只能向上访问，变量访问到<br/>
	window对象即被终止，作用域向下访问变量是不被允许的。<br/>
9、创建AJAX过程<br/>
	1）创建XMLHttpRequest对象，即创建一个异步调用对象<br/>
	2）创建一个新的HTTP请求，并指定该请求的方法、URL及验证信息<br/>
	3）设置响应HTTP请求状态变化的函数<br/>
	4）发送HTTP请求<br/>
	5）获取异步调用返回的数据<br/>
	6）使用JavaScript和DOM实现局部刷新<br/>
10、渐进增强和优雅降级<br/>
	·渐进增强：针对低版本的浏览器进行构建页面，保证最基本功能，然后针对高级浏览器进行效果、交互等改进和追加<br/>
	功能达到更好的用户体验<br/>
	·优雅降级：一开始构建完整功能，再针对低版本浏览器进行兼容<br/>
11、常见的web安全及防护原理<br/>
	·sql注入原理：把SQL命令插入到web表单递交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意<br/>
	SQL命令，如下：<br/>
		1）永远不要信任用户的输入，要对用户进行校验，可以通过正则表达式或限制长度，对单双引号进行转换<br/>
		2）永远不要使用动态拼接SQL，可以使用参数化的SQL或直接使用存储过程进行数据查询存取<br/>
		3）永远不要使用管理员权限的数据库连接，为每个应用使用单独的权限有限的数据库连接<br/>
		4）不要把机密信息明文放入，请加密或hash掉密码和敏感信息<br/>
	·XSS原理及防范：<br/>
		·原理：Xss（cross-site scripting）攻击指的是攻击者往web页面里插入恶意的html标签或者<br/>
		javascript代码，例如，在一个论坛里放入一个看似安全的链接，骗取用户点击，窃取cookie中的用户私密<br/>
		信息，或者在论坛中加入一个恶意表单，当用户提交表单，却把信息传入到攻击者的服务器中，而不是用户原本<br/>
		信任的站点。<br/>
		·防范：首先对用户输入的地方和变量都需要仔细检查长度和对“<","<",";","'"等字符做过滤；任何内容写<br/>
		到页面之前都必须加以encode，避免不小心把html tag弄出来，至少可以堵住一半XSS攻击。<br/>
		避免直接在cookie中泄漏用户隐私，使cookie和系统IP绑定来降低cookie泄漏后的风险<br/>
		如果网站不需要在浏览器端对cookie进行操作，可以在Set-cookie末尾加上HttpOnly来防止javascript<br/>
		代码直接获取cookie。<br/>
		尽量采用post提交表单。<br/>
	  ·XSS和CSRF的区别：<br/>
	  		·XSS：获取信息，不需要提前知道其他用户页面的代码和数据包<br/>
			·CSRF：1）登陆受信任网站A，并在本地生成cookie;2)在不登出A的情况下，访问危险网站<br/>
	  ·CSRF的防御：在客户端页面加入伪随机数，通过验证码的方法<br/>
12、web worker 和websocket<br>
		·web workers规范：为了解决长时间运行javascript进程导致的浏览器冻结用户界面的情况。webworker<br>
		通过让使用线程、后台进程、或者运行在其他处理器核心上的进程；<br/>
			1）通过worker = new Worker（URL）加载一个JS文件来创建一个worker，同时返回一个worker实例<br/>
			2）通过worker。postmessage（data)方法来向worker发送数据<br/>
			3）绑定worker.onmessage方法来接收worker发送过来的数据
			4）使用worker.terminate()来终止一个worker的执行
		·websocket：是web应用程序的传输协议，提供了双向的按序到达的数据流，是HTML5的协议，连接是持久的，<br/>
		通过在客户端和服务器端保持双工连接，服务器的更新可以及时被推送到客户端，不需要客户端以一定时间去轮询<br/>
13、HTTP和HTTPS<br/>
		<br/>HTTP协议在TCP协议之上，在二者之间添加一个安全协议层（SSL或TSL），就是HTTPS
		<br/>HTTP默认端口号80，HTTPS的端口号443<br/>
14、为什么HTTPS安全
	<br/>因为网络请求需要中间有很多服务器路由器的转发，中间的节点都有可能被篡改，
	<br/>如果使用HTTPS，密钥在你和终点站才有，https利用ssl/tls协议传输，它
	<br/>包含证书，卸载，流量转发，负载均衡，页面适配，浏览器适配，refer传递等，保证传输过程的安全性
<br/>15、对前端模块化的认识
	<br/>AMD（CMD）是RequireJS（seajs）在推广过程中定义的规范化产出
	<br/>AMD是提前执行，CMD是延迟执行
	<br/>AMD推荐的风格通过返回一个对象作为模块对象，CommonJS的风格通过对module.exports或exports的属性
	<br/>赋值来达到暴露模块对象的目的
<br/>16、javascript垃圾回收方法
	<br/>标记清除：当变量进入执行环境的时候，比如函数中声明一个变量，垃圾回收器将其标记为“进入环境”，当变
	<br/>量离开环境的时候将其标记为“离开环境”。
	<br/>垃圾回收器会在运行的时候给存储在内存中的所有变量加上标记，然后去掉环境中的变量以及被环境中变量所引用的变量（闭包），在这些完成之后仍存在	 <br/>标记的就是要删除的变量了
	<br/>
	<br/>·引用计数（reference counting）：在低版本IE中经常会出现内存泄漏，很多时候就是因为其采用
	<br/>引用计数方式进行垃圾回收。引用计数的策略是跟踪记录每个值被使用的次数，当声明了一个变量并将一
	<br/>个引用类型赋值给该变量的时候这个值的引用次数就+1，如果该变量的值变成了另一个，则这个值引用次数-1
	<br/>当这个值的引用次数变为0的时候，说明没有变量在使用，这个值没发被访问了，可以回收其占用的空间。
	<br/>在IE中虽然javascript对象通过标记清除方式进行垃圾回收，但BOM和DOM对象却是通过引用计数回收垃圾
	<br/>
<br/>17、你觉得前端工程的价值体现在哪里
	<br/>1）为简化用户使用提供支持（交互）
	<br/>2）为多个浏览器兼容性提供支持
	<br/>3）为提高用户浏览速度（浏览器性能）提供支持
	<br/>4）为跨平台或其他基于webkit或其他渲染引擎的应用提供支持
	<br/>5）为展示数据提供支持（数据接口）
<br/>18、谈谈性能优化问题
	<br/>代码层面：避免使用css表达式，避免使用高级选择器，通配选择器。
	<br/>	用hash-table来优化查找

少用全局变量

用innerHTML代替DOM操作，减少DOM操作次数，优化javascript性能

用setTimeout来避免页面失去响应

缓存DOM节点查找的结果

避免使用CSS Expression

避免全局查询

避免使用with(with会创建自己的作用域，会增加作用域链长度)

多个变量声明合并

避免图片和iFrame等的空Src。空Src会重新加载当前页面，影响速度和效率

尽量避免写在HTML标签中写Style属性<br/>
	<br/>缓存利用：缓存AJAX，使用CDN，使用外部JS和外部CSS文件以便缓存，添加Expire头，减少DNS查找等
	<br/>请求数量：合并样式和脚本，使用CSS精灵图片，初始首屏之外的图片资源按需加载，静态资源延迟加载
	<br/>请求带宽：压缩文件，开启GZIP。
<br/>18、移动端性能优化
	<br/>尽量使用css3动画，开启硬件加速。
<br/>适当使用touch事件代替click事件。
<br/>避免使用css3渐变阴影效果。
<br/>可以用transform: translateZ(0)来开启硬件加速。
<br/>不滥用Float。Float在渲染时计算量比较大，尽量减少使用
<br/>不滥用Web字体。Web字体需要下载，解析，重绘当前页面，尽量减少使用。
<br/>合理使用requestAnimationFrame动画代替setTimeout
<br/>CSS中的属性（CSS3 transitions、CSS3 3D transforms、Opacity、Canvas、WebGL、Video）
<br/>会触发GPU渲染，请合理使用。过渡使用会引发手机过耗电增加
<br/>PC端的在移动端同样适用
<br/>19、什么是Etag
	<br/>当发送一个服务器请求时，浏览器首先会进行缓存过期判断，浏览器根据缓存过期时间判断缓存文件是否过期
	<br/>未过期，直接使用缓存结果，在console中可以看到200OK，此时完全使用内存，浏览器和服务器没有任何交互
	<br/>已过期，发送请求，并带上（1）修改时间和Etag，
	<br/>服务器根据浏览器传过来的文件修改时间，判断自上一次请求之后，文件是不是没有被修改过；
	<br/>根据E tag判断文件内容自上一次请求之后，有没有发生变化
	<br/><br/>若都没有修改过，直接告诉浏览器，文件未被修改-304 Not Moditied,此时叫协议缓存，浏览器和服务器之间有一次交互
	<br/><br/>若有任意一个条件改动，受理请求，操作同（1）
	<br/>（1）只有GET请求会被缓存，post请求不会
<br/>20、Expires和Cache-Control
	<br/>Expires要求客户端和服务端的时钟严格同步。HTTP1.1引入Cache-Control来克服Expires头的限制。如果max-age和Expires同时出现，则max-<br/>age有更高的优先级。

    <br/>Cache-Control: no-cache, private, max-age=0

    <br/>ETag: abcde

    <br/>Expires: Thu, 15 Apr 2014 20:00:00 GMT

    <br/>Pragma: private

   <br/> Last-Modified: $now // RFC1123 format
<br/>21、E tag的应用
	<br/> Etag由服务器端生成，客户端通过If-Match或者说If-None-Match这个条件判断请求来验证资源是否修改。常见的是使用If-None-Match。请求一<br/> 个文件的流程可能如下：

<br/> ====第一次请求===

<br/> 1.客户端发起 HTTP GET 请求一个文件；

<br/> 2.服务器处理请求，返回文件内容和一堆Header，当然包括Etag(例如"2e681a-6-5d044840")(假设服务器支持Etag生成和已经开启了Etag).状态码200
<br/> ====第二次请求===

<br/> 客户端发起 HTTP GET 请求一个文件，注意这个时候客户端同时发送一个If-None-Match头，这个头的内容就是第一次请求时服务器返回的Etag：2e681a-6-<br/> 5d0448402.服务器判断发送过来的Etag和计算出来的Etag匹配，因此If-None-Match为False，不返回200，返回304，客户端继续使用本地缓存；流程很简<br/> 单，问题是，如果服务器又设置了Cache-Control:max-age和Expires呢，怎么办
<br/> 答案是同时使用，也就是说在完全匹配If-Modified-Since和If-None-Match即检查完修改时间和Etag之后，

<br/> 服务器才能返回304.(不要陷入到底使用谁的问题怪圈)

<br/> 为什么使用Etag请求头?

<br/> Etag 主要为了解决 Last-Modified 无法解决的一些问题。
<br/> 22、栈和队列的区别?

<br/> 栈的插入和删除操作都是在一端进行的，而队列的操作却是在两端进行的。

<br/> 队列先进先出，栈先进后出。

<br/> 栈只允许在表尾一端进行插入和删除，而队列只允许在表尾一端进行插入，在表头一端进行删除

<br/>23、栈和堆的区别？

<br/>栈区（stack）—   由编译器自动分配释放   ，存放函数的参数值，局部变量的值等。

<br/>堆区（heap）   —   一般由程序员分配释放，   若程序员不释放，程序结束时可能由OS回收。

<br/>堆（数据结构）：堆可以被看成是一棵树，如：堆排序；

<br/>栈（数据结构）：一种先进后出的数据结构
<br/>24、快速排序思想及一个实现
	<br/>1）在数据集中找一个基准点
	<br/>2）建立两个数组，分别存储左边和右边数组
	<br/>3）利用递归进行下次比较
	<br/><script>
	<br/>function quicksort(arr){
	<br/>	if(arr.length<=1){
		<br/>return arr;
		<br/>}
	<br/>var  num = Math.floor(arr.length/2);
	<br/>var numvalue = arr.splice(num,1);
	<br/>var left = [];
	<br/>var right = [];
	<br/>for(var i=0; i<arr.length;i++){
	<br/>if(arr[i]<numvalue){
	<br/>left.push(arr[i]);
	<br/>}else{
		<br/>right.push(arr[i]);
	<br/>}
	<br/>return quicksort(left).concat([numvalue],quicksort(right));
	<br/>}
	<br/></scirpt>
<br/>25、你觉得jQuery或zepto源码有哪些写的好的地方
<br>query源码封装在一个匿名函数的自执行环境中，有助于防止变量的全局污染，然后通过传入window对象参数，可以使window对象作为局部变量使用，好处是当<br>jquery中访问window对象的时候，就不用将作用域链退回到顶层作用域了，从而可以更快的访问window对象。同样，传入undefined参数，可以缩短查找<br>undefined时的作用域链。

 <br>   (function( window, undefined ) {

  <br>       //用一个函数域包起来，就是所谓的沙箱

  <br>       //在这里边var定义的变量，属于这个函数域内的局部变量，避免污染全局

  <br>       //把当前沙箱需要的外部变量通过函数参数引入进来

   <br>      //只要保证参数对内提供的接口的一致性，你还可以随意替换传进来的这个参数

   <br>     window.jQuery = window.$ = jQuery;

  <br>  })( window );
<br>jquery将一些原型属性和方法封装在了jquery.prototype中，为了缩短名称，又赋值给了jquery.fn，这是很形象的写法。

<br>有一些数组或对象的方法经常能使用到，jQuery将其保存为局部变量以提高访问速度。

<br>jquery实现的链式调用可以节约代码，所返回的都是同一个对象，可以提高代码效率。
<br>26、ES6的了解
<br>新增模板字符串（为JavaScript提供了简单的字符串插值功能）、箭头函数（操作符左边为输入的参数，而右边则是进行的操作以及返回的值<br>Inputs=>outputs。）、for-of（用来遍历数据—例如数组中的值。）arguments对象可被不定参数和默认参数完美代替。ES6将promise对象纳入规范，提供了原<br>生的Promise对象。增加了let和const命令，用来声明变量。增加了块级作用域。let命令实际上就增加了块级作用域。ES6规定，var命令和function命令声明的<br>全局变量，属于全局对象的属性；let命令、const命令、class命令声明的全局变量，不属于全局对象的属性。。还有就是引入module模块的概念

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
	
	

     

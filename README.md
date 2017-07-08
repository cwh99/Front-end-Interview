本文主要是巩固对前端知识点的理解，资料来源于网络和书本，由本人整理

1.position的值，relative和absolute分别是相对于谁进行定位的？
 ·absolute：绝对定位，以使用position定位的上一层组件（不是static定位的父组件）的左上角点为原点进行定位，如无position定位的上一层组件，则以body左上角点为原点来定位
 ·relative：相对定位，以组件本身的左上角点（普通流）为原点来定位
 ·fixed（老的IE不支持）：绝对定位，相对于浏览器窗口或frame进行定位
 ·static：默认值。无定位，元素出现在正常流中
 ·sticky：生成粘性定位的元素，容器的位置根据正常文档流计算得出
 ·图层定位：z-index必须与position属性一起使用，同值按照书写顺序重叠上去
2.overflow的值：
 ·hidden：超出长宽的内容不显示
 ·scroll：无论长款超不超出，都加入滚动条
 ·auto：视情况决定是否加入滚动条
3.如何解决跨域问题？
 在js中，直接使用XMLHttpRequest请求不同域上的数据是不可以的，但是在页面上引入不同域上
 的js文件却可以，jsonp就是利用这个特性来实现的
 
 如有个a.html页面，需要通过ajax获取一个不同域上的json数据，假设这个json数据地址是http://example.com/
 data.php，那么a.html中的代码可以这样写：
 <script>
 function dosomthing(jsondata){
   ／／处理获得的数据
   }
 </script>
 <script src="http://example.com/data.php?callback=dosomthing"></script>
 被访问网址返回的必须是一个能执行的js文件，所以：
 <?php
 $callback = $GET['callback'];
 $data = array('a,b,c');
 echo $callback.'('.json_encode($data).'$data).')';
 ?>
 最后该网址输出：
 dosomthing(['a','b','c'])
 所以通过http://example.com/data.php?callback=dosomthing得到的js文件，就是之前定义的js文件，
 就是我们之前定义的dosomthing函数，它的参数就是我们需要的数据，这样就跨域得到了我们想要的数据
 
 ·JSONP原理：动态插入script标签，通过其引入一个JS文件，这个文件载入成功后会执行我们在url参数中指定的函
 数，并把我们需要的json数据作为参数传入。
 由于同源策略限制，xmlHttpRequest只允许请求当前源（域名、协议、端口）的资源，为了实现跨域请求，可以通过
 script标签实现跨域请求，然后再服务器端输出JSON数据并执行回掉函数，优点是兼容性好，简单易用，支持浏览器
 和服务器双向通信，缺点是只支持GET请求。

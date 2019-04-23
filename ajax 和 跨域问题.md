## ajax 和 跨域问题

## ajax

**Ajax工作原理：**
相当于在客户端与服务端之间加了一个抽象层(Ajax引擎)，使用户请求和服务器响应异步化，并不是所有的请求都提交给服务器，像一些数据验证和数据处理

都交给Ajax引擎来完成，只有确认需要向服务器读取新数据时才右Ajax引擎向服务器提交请求。

Ajax优缺点：**

**优点：**

1.无刷新更新数据

Ajax最大的优点就是能在不刷新整个页面的情况下维持与服务器通信

2.异步与服务器通信

使用异步的方式与服务器通信，不打断用户的操作

3.前端与后端负载均衡

将一些后端的工作移到前端，减少服务器与带宽的负担

4.基于规范被广泛支持

不需要下载浏览器插件或者小程序，但需要客户允许JavaScript在浏览器上执行。

5.界面与应用分离

Ajax使得界面与应用分离，也就是数据与呈现分离

 

**缺点：**

1.Ajax干掉了Back与History功能，即对浏览器机制的破坏

在动态更新页面的情况下，用户无法回到前一页的页面状态，因为浏览器仅能记忆历史纪录中的静态页面

2.安全问题

AJAX技术给用户带来很好的用户体验的同时也对IT企业带来了新的安全威胁，Ajax技术就如同对企业数据建立了一个直接通道。这使得开发者在不经意间会暴露比以前更多的数据和服务器逻辑。

3.对搜索引擎支持较弱

4.破坏程序的异常处理机制

5.违背URL与资源定位的初衷

6.不能很好地支持移动设备

7.客户端肥大，太多客户段代码造成开发上的成本



异步请求 局部刷新 (提高用户体验)    异步请求的数据但是不能被爬虫获取 

具体步骤

```
异步对象的应用
在事件中
	1.获取用户数据
		var name = this.value；
	2.1创建异步对象
     	var xhr  =new XMLHttpRequest();
     2.2设置请求行 open(请求方式,请求url[,true])
true是异步的false是同步 默认为true 而且现在基本用的都是异步，不然也不会创建这个请求
		get请求如果有参数就直接在url后面添加
		post有参数需要在请求体中传递
		xhr.open("get","validate.php?username="+name)
	2.3设置请求头 setRequestHeader("key","value")键值对形式存在
		get不需要设置请求头
		post需要设置Content-Type:application/x-www-form-urlencoded
	2.4 设置请求体：发送请求send(参数，key=value&key=value)
		post应该在这里传递
语法.send(null) 这是get请求
	
```

服务器返回数据 判断数据正确传输回来

```
	报文行200 ok是正常的
	报文体  responseText:普通字符创 
	responseXML：符合xml格式的字符串
		因为异步是同时进行的所以得判断请求是否完成
	1.xhr.status;可以获取当前服务器的响应200 成功
	2.一个真正成功的不仅在服务器端响应了，还要在客户端接收到了并且可以使用了
	3.监听异步对象的响应状态 方法是readystate (有五个状态)
		0;创建了异步对象但没发送请求
		1;调用了send方法正在发送
		2;send方法执行完毕了 已经收到服务器的响应内容--但还不可以使用
		3;正在解析 返回的数据
		4;响应内容解析完毕，可以使用了
语法:xhr.onreadystatechange = function(){
	xhr.state==200是当服务器响应了
	xhr.readyState==4 客户端解析成功了 可以运用了
	if(xhr.state==200&&xhr.readyState==4) {
		输出返回的值
		console.log(xhr.responseText);
		输出到页面
语法document.querySelector("显示数据的标签").innerHTML=xhr.responseText;
}
```

#### ajax 原理代码

```
	var name = this.value
	var xhr  =new XMLHttpRequest();
	xhr.open("get","validate.php?username="+name)
	send(key=value&key=value)
	xhr.onreadystatechange = function(){
		if(xhr.state==200&&xhr.readyState==4) {
		console.log(xhr.responseText);
		语法document.querySelector("显示数据的标签").innerHTML=xhr.responseText;
		} }
```

#### php数据操作

```
php当被请求数据时
	看有没有传递数据 当有传递数据时 看其请求方式 来就收数据 $_POST['key']  $_GET[]
	链接数据库
	$connect = mysqli_connect("主机域名或者id","用户名","用户密码","需要链接的数据库"); (必须以分号结尾)
	$sql = ""; 这里书写 增删改查 事件 当查询时
	$result = mysqli_query($connect，$sql);当查询时返回的是一个结果集或者false($connect或$sql语句有错误时) 增删改返回的都是一个布尔类型 执行正确时(如果有数据变化时 现在数据库中的数据就已经变化了)返回true， 当错误时返回false 数据库也不会改变
	$response = [ "code" => 0, "msg" => "失败"];
	if($result) {	判断执行数据库中的代码是否正确  
	$response["code"] = 1; 这是前端判断数据库中代码是否正确执行依据
	$response["msg"] = '添加成功';	这个就是给你看看的 然并卵
	[$response["data"] = $result ;]  当查询时返回在数据库中查询到的数据 
}
	echo json_encode($response); 将json格式转化为字符串 因为数据传输 用的是字符串 (实际上传输的是二进制)
```

#### jQ中的ajax请求

```
一个请求天气的案例
$.ajax({
            url:"http://api.map.baidu.com/telematics/v3/weather",
            data:{
                "ak":"zVo5SStav7IUiVON0kuCogecm87lonOj",
                "location":"北京",
                "output":'json'
            },
            dataType:'jsonp',
            // jsonpCallback:function(){}
            success:function(result){
                console.log(result);
                var html = template("weatherTemp",{"items":result.results[0].weather_data});
                document.querySelector("tbody").innerHTML = html;
            }
        });
```

## 跨域

#### CORS跨域

```
header("Access-Control-Allow-Origin:*");
	*
header("Access-Control-Allow-Origin:http//www.xixi.com");
	这是指定的域可以访问
```

#### JSONP跨域

```
【重点】JSONP跨域 利用的是src的天然的跨域特性
判断dataType是jsonp
	1.创建script标签 这是一个异步请求
语法 varscript = document.createElement("script")
	2.为script标签设置一个src属性，同时在src中传入之后需要的使用的函数名
语法 script.src = url+"?callback="+callback;
	3.将生成的script标签添加到页面中
	document.body.appendChild(script);
	
	
	jq里面的 写 dataType:jsonp 就可以了
```


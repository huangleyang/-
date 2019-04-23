# token的使用

浏览器发送ajax给服务器

然后服务器加密之后 res.send返回这个token

当浏览器再次请求时。浏览器会将浏览起中的token取出被将去携带给服务器

然后服务器拿到token 返回其对应的数据

token就是安全令牌

```
登录成功后，获token值，保存到localstorage中 
localstorage.setItem('token',"获取到的token值")
设置请求头
headers: { 属性名(后台设置的): localstorage.getItem('token') }

请求拦截:在所有请求发送之前，将请求拦截下来，对请求的配置参数进行处理
axios.interceptors.request.use(function(config){
config.headers.属性名(后台设置的) =localstorage.getItem('token') }
	return config
}, function(error){
}
```


# fecth请求

fetch的使用

这是一个类似于ajax的求情

```js
//这个get请求的方法
fecth('url')
  .then(response => response.json(
	//第一个.then的数据不是一个json数据 而是一个二进制数据需要调用方法将其转化为json数据
	) )
  .then(data => {console.log(data)})
//post请求
// application/x-www-form-urlencoded // name=zs&age=18
// application/json // {name:'zs',age:18}

fetch('http://www.lovegf.cn:8899/api/addproduct',{
    method:"POST",
    body:JSON.stringify({content:'zs'})
}).then(res=>res.json())
    .then(data=>{
    console.log(data)
})
```
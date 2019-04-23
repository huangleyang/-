# axios请求拦截和响应拦截

```js
//导入axios包
import axios from 'axios'
//设置axios的全局请求头
axios.defaults.baseURL = "http://www.lovegf.cn:8888/api/private/v1"
//需要设置token值 前提是以将token获取到了 并存储到了localstorage中
//请求拦截request 在发送请求之前执行的函数 这里的作用就是将token设置到header中以便后台获取身份信息
axios.interceptors.request.use(function(config) {
  	//获取token并将其设置到了请求头中
    config.headers.Authorization = localStorage.getItem("token")
  	//注意这里必须得返回config *******
    return config
})
//响应拦截response 数据请求成功，在返回数据之前，提前设置返回数据 可以再次进行判断(即是全局判断)
axios.interceptors.response.use(function(response) {
    if (response.data.meta.status !== 200) {
        console.log('错误')
    } else {
        return response.data.data
    }
})
```


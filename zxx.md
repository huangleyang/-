## Vue扩展

### v-model双向数据绑定原理

> 双向数据绑定: 模型中的数据发生了变化,会自动更新视图;视图中的数据被更新了,模型中的数据也会发生变化。
>
> 在vue中,所有data中的属性都是通过Object.defineProperty()这个函数添加到vue实例上面的
>
> 只要修改data中的数据,必然会执行对应的set函数，在set函数中就可以进行视图更新
>
> 视图数据发生变化影响模型,只需要监听表单元素value的change事件
>
> Vue2.0使用的是Object.defineProperty()，这种方式称作数据劫持
>
> Vue3.0使用Proxy这种方式实现数据劫持



> Angular双向数据绑定原理:脏数据检查.对数据进行轮询，即不断的对数据进行遍历,检查数据是否已经发生了变化,如果发生了变化就更新视图,如果没有变化,继续下一次轮询。

```javascript
var vm = new Vue({
    data:{
        msg:'hello'
    },
    methods:{
        change(){
            this.msg = '123'
        }
    }
})

// 模型 影响 视图
var msg = 'hello'
Object.defineProperty(vm,'msg',{
  // msg和get/set不能同时存在
  // msg:'hello',
  // 获取属性的值
  get(){
    console.log('msg属性正在被获取...')
    return msg
  },
  // 设置属性的值
  set(arg){
    console.log('msg属性正在被赋值...')
    // 监听到了数据正在发生变化,在这里进行UI界面的更新
    msg = arg
  }
})

// 视图 影响 模型
// v-model内部会监听表单的change事件,在这个事件处理函数内部会拿到用户输入的最新值,然后将新值赋值给data中定义的属性
<input v-model='msg' />

```

### vue-router路由模式

> vue-router常用的两种模式:hash和history
>
> hash特点:在浏览器地址栏,**URL地址中会带#**,可以兼容低版本浏览器
>
> history特点:在浏览器地址栏,**URL地址中没有#**,由于使用的而是HTML5新属性history实现,导致不能兼容低版本浏览器，*如果复制地址在新标签页或者新浏览器都无法打开页面*（需要后端配合:任何情况下都返回index.html）
>
> hash模式实现原理:监听hash的改变事件(onhashchange),获取到#后面的hash值,然后和定义的路由规则进行匹配，匹配到之后马上将对于的组件渲染到页面,不会造成页面的刷新
>
> history模式实现原理:history对象有两个方法pushState、replaceState,者两个方法可以实现页面跳转但是不刷新页面这种效果



### event-bus(事件总线)兄弟组件传值

> 发布订阅者模式

> event-bus传值是使用事件传递，依靠一个中介(空的vue实例)进行事件的传递

```javascript
// 1. 创建一个空的vue实例,这个实例就是bus对象
const bus = new Vue({})

// 2. 在需要传递数据的组件中调用bus.$emit('事件名','数据')进行事件的触发
// 事件的发布
bus.$emit('getData',123456)

// 3. 在需要接收数据的组件中调用bus.$on('事件名',data => {})
// 事件的订阅
bus.$on('getData',data => {
    console.log(data) // 123456
})

```

### 组件传值方式有哪些

```javascript
// 父向子传值：
// 1. 属性绑定(props)
// 2. 子组件中通过this.$parent.xxx

// 子向父传值
// 1. 事件绑定($emit)
// 2. 父组件中通过this.$refs.sonname 获取到子组件对象
// 3. slot插槽

// 兄弟组件传值(非父子)
// 1. event-bus(事件总线)
// 2. vuex
```



### slot插槽

> 插槽的使用: 在组件内部定义slot标签,在使用组件时书写到组件内容区域的元素就能够出现在slot部分

```javascript
// 普通插槽
// 1. 父组件中使用子组件时,直接在子组件内容区域书写任意元素和内容
// 2. 在子组件内部template模板部分定义<slot></slot>
// 3. 父组件中书写的额内容会直接替换slot标签

// 具名插槽
// 1. 在父组件中使用子组件时,在子组件内容区域先书写一个template标签,该标签上面有一个slot属性，这个属性的值就是插槽的名字(slotname)
// 2. 在子组件内部template模板部分定义<slot name = 'slotname'></slot>
// 3. 这样书写以后,父组件中书写在子组件内容的区域的元素只会显示到具有name名字的slot部分

// 作用域插槽(实现父组件中使用子组件的数据)
// 1. 在父组件中使用子组件时,在子组件内容区域先书写一个template标签，然后定义一个slot-scope属性,属性值任意(scope)
// 2. 在子组件内部template模板部分定义slot标签,添加一个自定义属性,属性值就是需要传到父组件中使用的数据<slot data='123'></slot>
// 3. 这样书写以后,父组件中书写在子组件内容的区域的结构里面就可以使用scope.data获取到传过来的数据123
```



### keep-alive组件缓存

> `keep-alive`可以对组件进行缓存,主要用于`component`和`router-view`实现的动态组件切换效果中,即`keep-alive`只能用来包裹使用`component`和`router-view`渲染的组件.使用`keep-alive`缓存的组件再次进入时不会重新走生命周期函数,可以提升组件的渲染效率，另外由于缓存了组件,再次进入不会重新创建这个组件,提升性能。缺点:1.组件常驻内存,会耗费一定的内存空间2.对于需要及时更新数据的组件,使用缓存会出现数据不能及时获取的问题

```javascript
// 1. 使用方式:在router-view或者component组件外面使用keep-alive进行包裹
<keep-alive>
    <router-view />
</keep-alive>

// 2. 包裹之后,使用router-view或者component渲染的组件都将被缓存

// 3. 如果希望只缓存一部分组件,可以使用include指定组件的名字
// 如下代码表示只缓存home和search组件
<keep-alive :include=“['home','search']”>
    <router-view />
</keep-alive>

// 4. 如果希望缓存大部分组件,只有一部分组件不被缓存，可以使用exclude排除掉部分组件不被缓存
// 如下代码表示只有newslist和photolist这两个组件不会被缓存
<keep-alive :exclude=“['newslist','photolist']”>
    <router-view />
</keep-alive>
```



### Vue.mixin 全局混入

> Vue.mixin可以为组件混入一个新的组件对象，混入之后,每个组件实例在调用时都会先执行被混入的这个对象中的代码

```javascript
Vue.mixin({
    created(){
        console.log('我是被混入的组件实例')
    },
    methods:{
        
    },
    data(){
        return {}
    }
})
```

### vm.mixins局部混入

> mixins属性时vm实例的属性,其作用是为当前组件实例混入一个组件实例
>
> 主要使用场景:对复杂组件的业务逻辑进行抽离,可以让单文件组件中的代码量减少

```javascript
// 1. 封装一个单独的组件混入实例
const mixin = {
    created(){
   		console.log('我是被混入的组件实例')
	}
}
export default mixin

// 2. 在需要混入的组件中导入混入实例
import mixin from './mixin.js'
export default {
    mixins:[ mixin ],
    created(){
        console.log('我是组件自己的created函数')
    }
}

// 3. 运行结果
// 先打印 '我是被混入的组件实例'
// 再打印 '我是组件自己的created函数'
```



### Vue.use()插件机制

> 凡是使用了Vue.use()进行安装的第三方组件或库都是Vue的插件
>
> **插件内部必然会有一个install函数,使用Vue.use安装插件其实就是在调用这个install函数。**
>
> 制作插件的步骤:
>
> 
>
> 1. 新建一个单独的js文件,暴露一个对象,对象必须包含一个install方法，install方法有两个参数,第一个是Vue,第二个是options;Vue这个参数就是将来使用Vue.use(plugin)传入的构造函数Vue
>
> ```javascript
> const plugin = {}
>
> plugin.install = function(Vue,options){
>     
> }
>
> export default plugin
> ```
>
> 2. 在install函数内部处理四种不同的业务逻辑（注意：平时自己封装插件用的最多的就是注册全局组件和在原型中挂载实例方法）
>
> ```javascript
> plugin.install = function (Vue, options) {
>   // 1. 添加全局方法或属性
>   Vue.myGlobalMethod = function () {
>     // 逻辑...
>   }
>   // 2. 添加全局资源(指令、过滤器、组件)
>   Vue.directive('my-directive', {
>     bind (el, binding, vnode, oldVnode) {
>       // 逻辑...
>     }
>   })
>   // 注册过滤器
>   Vue.filter('datefmt',function(){
>       return '2018-06-12'
>   })
>   // 注册组件
>   Vue.component('com',{})
>
>   // 3. 混入组件(将一个组件的功能混入到另一个组件中)
>   Vue.mixin({
>     created: function () {
>       // 逻辑...
>     }
>   })
>   // 4. 添加实例方法
>   Vue.prototype.$myMethod = function (methodOptions) {
>     // 逻辑...
>   }
> }
>
> ```
>
> 3. 使用插件
>
> ```javascript
> import plugin from './plugin.js'
>  // 调用Vue.use时 会自动执行plugin插件内部的install函数
>
> Vue.use(plugin)
> ```



## cmder使用

```JavaScript
// 1. 将cmder.exe加入环境变量
1. window + R 输入 sysdm.cpl
2. 选中 高级 - 环境变量 - 系统变量 - Path
3. 将cmder.exe所在目录添加到Path中

// 2. 添加成为右键菜单
在cmder.exe所在目录打开命令行窗口 输入 ./Cmder.exe /REGISTER ALL
```

## yarn使用

> npm: node package manager,是node官方提供的用于管理包的工具
>
> yarn:是Facebook公司提供的用于管理node包的工具(丢包率比较低,下载速度比较快，提供的所有命令都正常可用)

```js
// 安装yarn
cnpm i yarn -g
// 安装完毕之后需要把yarn的bin目录配置到环境变量的path下面

// 初始化package.json文件
npm init -y
yarn init -y
// 安装开发阶段的依赖包
npm install webpack -D/--save-dev
yarn add webpack --dev
// 安装发布阶段的依赖包
npm i vue -S/--save(npm5.x以后可用省略-S)
yarn add vue
// 卸载包
npm uninstall webpack -D
yarn remove webpack(不需要加参数就可以移除掉package.json中包名的记录)

// 安装全局包
npm i webpack -g
yarn global add webpack
// 卸载全局包
npm uninstall webpack -g
yarn global remove webapck

// 运行package.json中的脚本
npm run dev
yarn run dev

```

## 跨域

> **同源策略**:浏览器规定只有当协议、域名、端口号完全一致才称作是同源服务
>
> 注意:**跨域只存在浏览器上面**

### JSONP

> 特点: 前后端需要协作才能实现

>JSONP能够跨域的原因: **script 标签通过src发送请求不受同源策略限制。**

1. 当前端程序员通过JSONP发送请求时,一般封装过的请求库内部都会离开在页面中创建一个script标签,同时将请求的url路径作为src属性传入

```html
<script src="http://127.0.0.1:8888/list"></script>
```

2. 在请求路径后面会拼接上一个参数，以查询字符串的方式拼接,`?cb=getlist`

```html
// cb这个名字程序员可以自己定义，getlist这个函数名是请求库内部随机生成的
<script src="http://127.0.0.1:8888/list?cb=getlist"></script>
```

3. 后端监听到这个jsonp请求,会解析携带的参数,拿到函数名getlist，然后拼接上需要发送给前端的数据,作为一个字符串返回给前端

```js
// 后端返回给前端的实际上时一个函数调用字符串
res.send('getlist(123)')
```

4. 浏览器接收到响应数据之后会直接将函数调用字符串当做函数进行调用,函数在调用时就可以获取到传递的实参
5. 请求库内部处理完JSONP的数据之后,会将script标签进行删除。

### CORS

> 特点:**只需要后端添加允许跨域的请求头就能实现跨域**
>
> CORS能够解决100%跨域问题

```js
app.all('*', function(req, res, next) {  
  res.header("Access-Control-Allow-Origin", "*");  
  res.header("Access-Control-Allow-Headers", "X-Requested-With");  
  res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS");  
  res.header("Content-Type", "application/json;charset=utf-8");  
  next();  
});
```

### proxy(代理)

> 代理大多数时候只解决开发阶段跨域问题
>
> **服务器对服务器发送请求是没有跨域问题的。**
>
> 所有后端语言都可以通过内置模块向服务端发送请求
>
> 代理服务器原理: 
>
> 1. 客户端C向服务器S发送数据请求,存在跨域问题
> 2. 可以在C所在的计算机上面搭建一个代理服务器P，客户端C访问P,P再去访问S
> 3. 由于服务器向服务器发送请求是不存在跨域的,所以数据能够直接到达P服务器里面
> 4. 只要C请求P能够实现解决跨域问题,最终就能拿到S中的数据了
>
> **C请求P解决跨域的方案**：
>
> 1. P设置CORS
> 2. C和P保证在同一个域名端口号下面

```javascript
// http模块发送请求
const http = require('http')
const req = http.request({
  hostname:'www.lovegf.cn',
  port:'8899',
  path:'/api/getlunbo'
},function(res){
  let info = '';
    // 监听数据
    res.on('data',(chunk)=>{
        info += chunk;
    });
    // 数据发送完毕
    res.on('end',()=>{
        console.log(info)
    });
})

req.end()
```

### webpack-dev-server解决跨域问题

> 通过设置webpack-dev-server的代理解决开发阶段跨域问题

```js
// webpack.config.js配置文件导出对象中加入
 devServer:{
    // 代理跨域
    proxy: {
      '/api': {
        target: 'http://vue.studyit.io/api', // 需要被代理的api根域名
        changeOrigin: true, // 是否跨域
        pathRewrite: {
          '^/api': '' // 重写(target中只有根域名时不需要配置此选项)
        }
      }
    }
  }
---------------------------
// vue-cli中
config/index.js中
proxyTable: {
    '/api': {
        target: 'http://vue.studyit.io/api',
        changeOrigin: true,
        pathRewrite: {
          '^/api': ''
        }
    }
}
```





## React扩展

### creat-react-app

> 快速搭建react项目的脚手架工具

```js
// 1. 全局安装creat-react-app
yarn global add create-react-app 或者 cnpm i creat-react-app -g

// 2. 初始化项目模板
create-react-app my-react-app

// 3. 进入项目目录运行项目
yarn start 或者 npm start

// 4. 如果要更改配置,需要手动执行命令下载配置文件
yarn eject

// 5. 项目开发完毕打包
yarn build
```



### context属性传递

>  如果组件嵌套太深，`props`的传递方式会增加组件的依赖，每个组件之间最好是低耦合的，所以我们可以使用`context`属性实现组件之间数据的传递。


1. 父组件中通过`getChildContext`设置需要传递的属性,同时需要使用`childContextTypes`设置传递的属性校验

```js
// 1. 定义需要传递给子孙组件的数据
getChildContext(){
    return {
      msg:'我是需要传给孙子的数据'
    }
}

// 2. 对该数据进行属性校验
static childContextTypes = {
	msg: PropTypes.string,
}
```

2. 子组件接收传递过来的属性必须使用`contextTypes`接收，然后通过`this.context.[prop]`即可使用传递过来的属性

```js
// 3. 子组件中对父组件传过来的数据进行属性校验
static contextTypes = {	
	msg: PropTypes.string
}

// 4. 使用 this.context.msg 访问父组件传过来的数据
console.log(this.context.msg)
```

  

###  Redux的使用

> Redux的概念: Redux是React项目中的一个状态管理库,专门用来管理公共状态,其具备三个核心概念,分别为Store,Reucer,Action

> `state`：定义公共数据
>
> `action`: 触发更改数据的业务逻辑，同时传入需要修改的数据以及怎么修改数据(增删改查)
>
> `reucer`:接收action传递过来的信息(需要执行哪种业务逻辑、数据)

```js
// 1. 安装redux
cnpm i redux -S

// 2. 在index.js中进行集成

// 2.1 导入创建store的方法

import { createStore } from "redux";

// 2.2 定义用于执行具体操作状态的reducer

// reducer是一个函数,该函数接收两个参数,一个是state一个是action

// state就是公共状态,action是用于定义要执行的操作和被改变的状态

// action是一个对象,必须有一个type属性,type用于描述进行的操作,通常是个字符串常量

const reducer = (state = 0, action) => {
  switch (action.type) {
      case 'ADD':
          return state + action.payload;
      case 'SUB':
          return state - action.payload;
      default: 
          return state;
  }
}

// 3. 使用createStore创建store实例,一个项目只存在一个store
// 为方便其他组件使用store,可以将store挂载到React组件的原型中
let store = createStore(reducer);

React.Component.prototype.$store = store

// 4. 使用store中的数据
this.$store.getState()

// 5. 更新数据
add = ()=>{
    this.$store.dispatch({
        type: 'ADD',
        payload:1
    })
}

// 6. 监听数据的变化(subscribe订阅数据)
this.$store.subscribe(() =>{
    console.log(this.$store.getState())
    this.setState({
        count:this.$store.getState()
    })
})
```



### react-redux的使用

> react-redux类似vuex中的辅助函数，可以将state中的数据映射成为组件的props属性，可以将action映射成为组件的props属性(该属性是一个方法)

```js
// 1. 安装react-redux
yarn add react-redux

// 2. 导入组件Provider，将项目的根组件包裹,将store作为属性进行传递
import { Provider } from 'react-redux'

ReactDOM.render(
  // Provider身上绑定store属性之后，项目中所有子组件都可以访问到公共的state数据
  // 利用的是context特性传值
  <Provider store={store}>
    <TodoApp />
  </Provider>, document.getElementById('root'));

// 3. 将action映射成为组件的props属性,同时会映射成为一个函数
import { connect } from "react-redux";
// 3.1 将action映射成为方法(提交action使用this.props.addTodo())
export default connect(null,{
  addTodo:(todo)=>{
    return {
      type:'add',
      payload:todo
    }
  }
})(AddTodo)

```

## 小程序框架 mpvue

> 使用vue开发小程序的框架

```js
// 1. 先全局安装vue-cli
yarn global add vue-cli

// 2. 使用vue-cli提供的命令安装小程序项目模板
vue init mpvue/mpvue-quickstart my-pmvue-app

// 3. 进入项目目录安装依赖
yarn 

// 4. 运行项目
yarn run dev或者npm run dev

// 5. 使用微信开发者工具打开项目,修改src目录的vue组件,可以看到模拟器中界面发生变化

// Tip:
// 1. 在.vue的组件中可以写任何vue语法进行组件的开发
// 2. 在.vue的组件中可以书写小程序的页面生命周期(一般不用)
// 3. 在.vue的组件中可以调用小程序内置的API
// 4. 在.vue的组件中不能使用小程序的组件,只能使用HTML和CSS
```


## Mock.js

> 生成随机数据，拦截 Ajax 请求

> 后端定义的接口路径、每个接口返回的数据格式、字段都应该提前做好约定


```js
// 1. 安装mock.js
yarn add mockjs

// 2. 引入mockjs
const Mock = require('mockjs');
// 2.1 获取 mock.Random 对象
const Random = Mock.Random;
// 2.2 mock一组数据
const produceNewsData = function() {
    let articles = [];
    for (let i = 0; i < 100; i++) {
        let newArticleObject = {
            title: Random.csentence(5, 30), //  Random.csentence( min, max )
            img: Random.dataImage('300x250', 'mock的图片'), // Random.dataImage( size, text ) 生成一段随机的 Base64 图片编码
            author_name: Random.cname(), // Random.cname() 随机生成一个常见的中文姓名
            date: Random.date() + ' ' + Random.time() // Random.date()指示生成的日期字符串的格式,默认为yyyy-MM-dd；Random.time() 返回一个随机的时间字符串
        }
        articles.push(newArticleObject)
    }
    return {
        articles: articles
    }
}
 
// 3. 拦截请求 Mock.mock( url, post/get , 返回的数据)；
Mock.mock('/news/index', 'post', produceNewsData);

axios.post('/news/index').then(res=>{
    console.log(res)
})
```



## CMD/AMD

```js
AMD/CMD可以理解为是commonjs在浏览器端的解决方案，AMD/CMD下，模块都是异步加载的；

1. AMD模块化规范代表：RequireJS(异步模块化规范)
   + 主要特性1：对于依赖的模块，AMD 是提前执行；
   + 主要特性2：推崇依赖前置；
2. CMD模块化规范代表：SeaJS(通用模块化规范)
   + 主要特性1：对于依赖的模块，CMD 是延迟执行；CMD 推崇 as lazy as possible.
   + 主要特性2：推崇依赖就近；
```



## 百度地图



```js
1. 在百度地图开发者网站申请开发者key(选择浏览器端应用)
http://lbsyun.baidu.com/apiconsole/key?application=key

2. 在项目中引入百度地图库文件(由于异步加载的问题,在组件中引入方式比较特殊,这里直接在模板文件中引入)
<script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak=填写秘钥key"></script>

3. 在vue文件中实例化地图(地图需要显示在一个容器div中,实例化地图的方法要写在生命周期mounted中)
mounted() {
    // 创建地图
    var map = new BMap.Map("allmap");
    // 初始化地理位置
    var point = new BMap.Point(116.331398, 39.897445);
    // 缩放中心和比例
    map.centerAndZoom(point, 12);

    // 定位
    var geolocation = new BMap.Geolocation();
    geolocation.getCurrentPosition(function (r) {
    if (this.getStatus() == BMAP_STATUS_SUCCESS) {
            // 定位成功 标记地理位置
            var mk = new BMap.Marker(r.point);
            map.addOverlay(mk);
            map.panTo(r.point);
            console.log('您的位置：' + r.point.lng + ',' + r.point.lat);
    }
    else {
            alert('failed' + this.getStatus());
    }
    }, { enableHighAccuracy: true })
}

```



## HTTP协议

> http0.9  只支持get请求
>
> http1.0 支持get、post
>
> http1.1 支持get、post、put、delete、**options**、head、patch



> TCP/IP三个层次：网络层、传输层和应用层。
> **网络层**有**IP协议**、ICMP协议、ARP协议、RARP协议和BOOTP协议。
> **传输层**有**TCP协议与UDP协议**。
> **应用层**有**FTP**、**HTTP**、TELNET、SMTP、**DNS**等协议。
>
> TCP 是基于 TCP 协议实现的网络文本协议,属于传输层。
> UDP 是和TCP 对等的，属于传输层，UDP 和 TCP 有着很大的区别和不同的应用场景。

> ```
> HTTP/TCP（Transmission Control Protocol，传输控制协议）是基于连接的协议.传输大量数据，速度慢，可靠。存在三次握手
> UDP（User Data Protocol，用户数据报协议）是与TCP相对应的协议。它是面向非连接的协议，它不与对方建立连接，而是直接就把数据包发送过去。传输少量数据、速度快、不可靠。ping命令。
> ```



![img](https://images2015.cnblogs.com/blog/705728/201604/705728-20160424234826351-1957282396.png)

![img](https://images2015.cnblogs.com/blog/705728/201604/705728-20160424234827195-1493107425.png)

### 三次握手

> 是在TCP建立连接中需要执行的一个过程
>
> 客户端向服务端建立连接的过程

```js
第一次握手： A给B打电话说，你可以听到我说话吗？
第二次握手： B收到了A的信息，然后对A说： 我可以听得到你说话啊，你能听得到我说话吗？  (A可以发送消息、B可以接收消息、【不能保证A可以接收消息、B能发送消息】)
第三次握手： A收到了B的信息，然后说可以的，我要给你发信息啦！【A可以接收消息、B可以发送消息】

经过三次握手才能保证A、B既可以接收也可以发送消息。

```

### 四次挥手

> socket
>
> 客户端和服务端都可以直接发送数据才会有四次挥手的过程
>
> 客户端和服务端都有权利和对方断开连接

```js
A:“喂，我不说了 

B:“我知道了。等下，上一句还没说完。

// 先传输完数据
// 发送消息给A确认可以断开连接

B:”好了，说完了，我也不说了 (B已经断开连接)

// 必须让A知道B的数据发生完毕

A:”我知道了。(A断开连接)

```



### 状态码

```js
// 1xx	表示HTTP请求已经接受，继续处理请求
// 2xx	表示HTTP请求已经处理完成(200)
// 3xx	表示把请求访问的URL重定向到其他目录(304资源没有发生变化，会重定向到本地资源)
// 4xx	表示客户端出现错误(403禁止访问、404资源不存在)
// 5xx	表示服务端出现错误
```



### get/post区别

![1547434609579](C:\Users\LEO\AppData\Roaming\Typora\typora-user-images\1547434609579.png)

## fiddler(抓包工具)

> 可以拦截到当前环境中的所有网络请求
>
> 可以通过这个工具获取到网络请求中的所有信息
>
> 调试移动端App的接口
>
> 可以利用fiddler抓取手机上安装的所有应用程序的接口数据

> https://www.cnblogs.com/meitian/p/4997310.html

---

## 技术栈的选型

> PC端:vue-cli +  vue-router +vuex + element-ui/iview + axios(vue-resource/fetch) + less/scss + i18n(语言国际化插件)
>
> 移动端:vue-cli + mint-ui/vant/vux/cube-ui + axios(vue-resource/fetch) + less/scss + flexible.js(rem布局)/vw、vh + i18n(语言国际化插件) 



> 老板/项目经理提供项目需求-需求文档
>
> 产品经理提供原型图(原型图主要展示业务逻辑以及提供给UI进行设计)
>
> UI设计人员提供UI设计稿(设计稿会包含当前项目所有页面以及所有需要用到的图片、图标)
>
> 后端会根据原型图和UI图设计数据接口(mock)
>
> 前端开发根据UI开发界面(mock) 、再根据后端提供的接口接入真实数据
>
> 非专业测试(UI、产品经理、老板):主要测需求是否完全实现、流程是否能够走通、是否有明显bug
>
> 较专业测试: 文档记录、bug追踪系统记录(禅道):使用文字描述、截图等方式记录bug，然后指派给具体人员进行修复，还会标记优先级。开发人员根据指派给自己的bug进行修复。
>
> 专业测试: 使用代码测试代码，需要写测试脚本、测试用例。还分有黑盒测试、白盒测试。
>
> 上线:
>
> - 如果有专业的运维,或者后端、项目经理负责上线.前端只需要将静态文件给对方即可。
> - 如果前端自己负责上线，上级会给你一个ftp地址。前端只需要通过ftp的软件将静态文件传到这个地址中即可。



## xss & csrf攻击

>  XSS：跨站脚本（Cross-site scripting，通常简称为XSS）**这类攻击通常包含了HTML以及js脚本语言**。
>
>  > 防御措施: 永远不要相信用户输入的内容，要对内容做校验，使用正则匹配是否包含HTML标签,如果包含使用转义字符代替。
>
>  CSRF:跨站请求伪造（英语：Cross-site request forgery），缩写为 CSRF 或者 XSRF，冒充用户发起请求（在用户不知情的情况下）,完成一些违背用户意愿的请求（如恶意发帖，删帖，改密码，发邮件等）

> 1. 攻击者发送xss攻击脚本(获取token)
> 2. A访问网站时会被攻击脚本获取到token
> 3. 攻击者拿到token之后就可以使用token调用被攻击网站的所有接口
> 4. 攻击者做一个钓鱼网站,其他所有用于访问钓鱼网站时都可以触发攻击的请求
>
> > 防御措施: 
> >
> > 1. 保证用户信息不回被泄露，敏感信息不要存储到本地。防御掉xss攻击。
> > 2. 服务端接收请求需要验证请求头是否包含自己规定的内容，要求服务端需要自己做一套请求的校验规则。
> >
> >
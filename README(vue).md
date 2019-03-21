<!-- 
   TodoMVC作业:
1. 数据的基本展示
2. 数据的添加
3. 数据的删除
4. 样式的切换
5. 本地存储
6. 点击标签进行数据的切换(计算属性实现)
7. 点击clear删除所有已完成
8. 双击选项可以进行数据的修改
   -->
# Vue

## Vue基础

> Vue是基于M(数据)V(视图)VM(调度者)这种设计模式开发出来的一个框架

> MVC和MVVM之间的区别: MVC是单向通信 MVVM是双向数据绑定

## Vue基本的编码步骤

1. 引入Vue.js框架
```javascript
<script src='./src/vue.js'></script>
```
2. 在script标签部分创建一个Vue实例
```javascript
var vm = new Vue({
    el:'#app', // 通过选择器找到一个让vm实例监管的区域
    data:{
        // 在data中定义的数据就是整个Vue实例可以用到的数据
        msg:'hello'
    }
})
```
3. 在HTML结构部分书写Vue代码
```html
<div id = 'app'>
    {{msg}}
</div>
```

## Vue指令

- `{{}}`:插值表达式,用于显示`data`中定义的数据

- `v-text`:用于显示`data`中定义的普通文本数据

```javascript
new Vue({
    el:'#app',
    data:{
        msg:'普通文本数据'
    }
})
<p v-text = 'msg'></p>
```

- `v-html`:用于显示`data`中定义的富文本数据(带有HTML标签的文本)

```javascript
new Vue({
    el:'#app',
    data:{
        msg:'<p>富文本数据<p/>'
    }
})

<div v-html = 'msg'></div>
```

- `v-bind`:动态的给元素绑定属性

```javascript
new Vue({
    el:'#app',
    data:{
        url:'http://www.baidu.com/'
    }
})

<a v-bind:href = 'url'>百度</a>
```

- `v-for`:用于循环渲染元素进行显示

```html
<div v-for = '(item,index) in list' :key = 'index'>
    <!-- 数组中的每一项 -->
    {{item}} 
    <!--  当前项的索引 -->
    {{index}}
</div>
```

- `v-model`:用于对表单元素实现双向数据绑定,即模型中数据发生变化可以直接更新视图,视图中数据发生变化也可以影响到模型

```html
<!-- v-model 指令只能用于表单元素和组件 常见于input/textarea/select标签上 -->
<input v-model = 'name'></input>
```

- `v-on`:绑定事件

```html
<button v-on:click = 'clickHandle'></button>
```

```JavaScript
new Vue({
    el:'#app',
    // methods中专门用来定义方法
    methods:{
        clickHandle:function(){
            console.log('点击事件被触发了')
        }
    }
})
```
- `v-if`:控制元素的显示隐藏

> 通过添加和移除元素控制元素显示隐藏

> 如果元素初始状态是隐藏的,而且不涉及频繁显示隐藏切换,推荐使用v-if

```html
<div v-if = 'isshow'></div>
```

```javascript
new Vue({
    el:'#app',
    data:{
        isshow:true
    }
})
```

- `v-show`:控制元素的显示隐藏

> 通过给元素设置`display:none`的样式控制元素显示隐藏

> 如果元素涉及频繁显示隐藏切换,推荐使用v-show,性能会比v-if好

```html
<div v-show = 'isshow'></div>
```

```javascript
new Vue({
    el:'#app',
    data:{
        isshow:true
    }
})
```

## ref获取DOM

> 通过给元素设置ref属性,然后使用this.$refs.属性值得方式可以获取到DOM

```html
<div ref = 'dv'></div>
<button @click = 'getdv'>获取div元素</button>
```

```JavaScript
new Vue({
    el:'#app',
    methods:{
        getdv(){
            console.log(this.$refs.dv)
        }
    }
})
```

## 自定义指令

> 指令能够帮助开发者操作DOM,当系统指令不能够满足需求时,就可以使用自定义指令进行DOM操作;将操作DOM的方式封装成指令后首先可以重复使用,其次封装成指令的过程是vue内部提供的方法,这些方法在性能上比直接操作DOM元素要高。

```javascript
Vue.directive('mycolor',{
    // 指令的钩子函数
    inserted(el,binding){
        // el 是指令所在的元素对象
        // bingding是一个对象
        // binding.value // red 指令等号后面表达式的值 也就是变量的值
        // binding.expression // 指令等号后面的表达式  本质就是一个变量
    }
})

new Vue({
    el:'#app',
    data:{
        color:'red'
    }
})
```

```html
<div v-mycolor = "color"></div>
```

## 自定义过滤器

> 在数据展示之前,对原始数据进行处理并返回处理后的数据进行展示,不改变原始数据

> 过滤器只能用在插值表达式和属性绑定(v-bind)中

```javascript
Vue.filter('过滤器名称',function(data,arg){
    // data是原始数据,即管道符前面的数据
    // arg 是使用过滤器时传递的参数
    // 1. 对原始数据进行逻辑处理
    // 2. 处理完成后进行return
    return `处理完的数据`
})
```

```html
<div>{{原始数据 | 过滤器名称(参数)}}</div>
```

## 计算属性computed

> 用于定义一种可以随着所依赖数据发生变化的属性

```javascript
new Vue({
    el:'#app',
    // data中定义普通属性
    data:{
        msg:''
    },
    // compluted中定义的函数名就是计算属性 属性的值是该函数的返回值
    // 计算属性的值会随着依赖数据的变化而变化
    // 例如this.msg被修改 则comMsg的值也会重新计算
    compluted:{
        comMsg(){
            return this.msg
        }
    }
})
```

## 监听器 watch

> 监听data中定义的数据的变化

```javascript
new Vue({
    el:'#app',
    data:{
        msg:'被监听数据',
        obj:{
            name:'zxx'
        }
    },
    watch:{
        msg(newVal,oldVal){
            // 根据数据变化做相关逻辑处理
            // newVal 变化后的值
            // oldVal 变化前的值
        },
        // 当被监听的数据是复杂数据类型时,一般需要用深度监听才可以监听到复杂数据类型内部的数据变化
        obj:{
            handler(newVal,oldVal){
                
            },
            deep:true
        }
    }
})
```

## axios发送请求

> axios是一个基于promise的请求库,可以在node环境和浏览器环境中使用。

1. 引入axios

```js
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

2. get请求

```js
axios.get('url').then(function(res){
    // res.data即后台返回的数据
    console.log(res.data)
})
```

3. post请求
```js
axios.post('url',{
    username:'zxx',
    password:'123456'
}).then(function(res){
    // res.data即后台返回的数据
    console.log(res.data)
})
```

## vue过渡动画

> vue中的过渡动画只能作用于使用`v-if`、`v-show`控制的元素已经动态组件中

### 使用css类实现动画

1.使用`transition`标签将需要实现动画的元素包裹起来
```html
<button @click = 'isshow = !isshow'></button>
<transition>
    <div v-if = 'isshow'>御剑乘风来</div>
</transition>
```
2. 使用css类控制动画效果

```css
/* 进入动画就是一个元素从隐藏到显示这个过程需要执行的动画 */

/* 离开动画是一个元素从显示到隐藏这个过程需要执行的动画 */
/* 元素执行进入动画之前的状态 */
.fade-enter {
    transform: translateX(100px)
}
/* 元素执行进入动画的过程中持续添加到元素的状态 */
/* 在这个类中通常都是书写元素需要执行的动画效果 */
.fade-enter-active {
    transition: all 0.2s ease
}
/* 元素执行进入动画结束状态 */
/* 在这个类中通常是用来设置元素执行动画完毕之后的样式 */
.fade-enter-to {
    transform: translateX(0)
}
/* 元素执行离开动画之前需要设定的样式 */
.fade-leave {
    transform: translateX(0)
}
/* 元素执行离开动画的过程中持续添加到元素身上 */
.fade-leave-active {
    transition: all 0.5s ease
}
/* 元素中心离开动画过程中持续添加到元素身上 */
.fade-leave-to {
    transform: translateX(100px)
}
```

### 使用animate.css实现动画


1.使用`transition`标签将需要实现动画的元素包裹起来

```html
<button @click = 'isshow = !isshow'></button>
<transition>
    <div v-if = 'isshow'>御剑乘风来</div>
</transition>
```
2. 使用animate.css提供的类名设置动画

```html
<transition 
enter-active-class = 'animate fadeIn'
leave-active-class = 'animate fadeOut'
>
    <div v-if = 'isshow'>御剑乘风来</div>
</transition>
```

### 使用动画的钩子函数实现
```js
// 1. 将需要实现动画的元素使用transition标签包裹起来
<transition>
    <div>我是需要过渡动画的元素</div>
</transition>

// 2. 在transition上注册实现动画的钩子函数,入场动画三个,出场动画三个
<transition 
@before-enter = 'beforeEnter'
@enter = 'enter'
@after-enter = 'afterEnter'
@before-leave = 'beforeLeave'
@leave = 'leave'
@after-leave = 'afterLeave'
>
    <div>我是需要过渡动画的元素</div>
</transition>

// 3. 在vue实例的methods中定义钩子函数
new Vue({
    el:'#app',
    data:{},
    methods:{
        // 入场动画初始状态设置
        beforeEnter(){
            // 一般使用JS设置执行动画元素的初始位置和其他样式属性
        },
        // 入场动画持续过程
        enter(el,done){
            // 1. 固定写法 迫使浏览器重绘 刷新动画
            el.offsetWidth
            // 2. 一般设置执行动画元素最终的位置和其他样式属性

            // 3. 固定写法 调用afterEnter函数
            done()
        },
        // 入场动画结束状态
        afterEnter(){
            // 设置动画元素回到初始状态
        },
        // 出场动画初始状态
        beforeLeave(){

        },
        // 出场动画持续状态
        leave(){

        },
        // 出场动画结束状态
        afterLeave(){

        }
    }
})
```

## 组件

> 组件就是对视图的封装,方便重复使用

> 模块是对功能逻辑的封装

> **注意**:
>
> 1. 定义组件时如果使用的是驼峰命名,那么使用组件时需要将驼峰的大写字母转成小写,并且用-连接两个单词 
```javascript
Vue.component('myCom',{
    template:'<div>我是一个驼峰命名的组件</div>'
})
// 使用
<my-com></my-com>
```
> 2. 组件的`template`属性中的模版内部如果有多个元素,必须被包含在唯一的一个根元素中
```javascript
<template>
    <div>
        <p>我是p元素</p>
        <span>我是span元素</span>
    </div>
</template>
```
> 3. 子组件使用`components`属性进行定义,定义好的子组件只能在父组件的模板中使用
```javascript
<template id='father'>
    <div>
        <son></son>
    </div>
</template>
// 父组件
Vue.component('father',{
    template:'#father',
    components:{
        // 子组件
        son:{
            template:'<div>我是son组件</div>'
        }
    }
})
```

### 组件的三种定义方式

-  `vue.extend()`定义

```javascript
// 1. 使用vue.extend()定义组件模板
var dv = Vue.extend({
    template:'<div>我是一个组件</div>'
})

// 2. 使用Vue.component()注册组件
Vue.component('com',dv)

// 3. 在Vue托管区域像使用普通HTML标签一样使用组件
<com></com>

```
-  `vue.component()`定义

```javascript
// 1. 使用Vue.component定义组件
Vue.component('com',{
    template:'<div><p>我是一个组件中的元素</p><span>我也是组件中的元素</span></div>'
})

// 2. 使用组件
<com></com>
```

- 使用`template`标签定义模板

```javascript
// 1. 使用Vue.component定义组件，并且使用选择器选择模板
Vue.component('com',{
    template:'#temp'
}) 

// 2. 使用template标签定义模板,并且给template标签添加id
<template id='tmpl'>
    <div>
        <p>我是p元素</p>
        <span>我是span元素</span>
    </div>
</template>

// 3. 使用组件
<com></com>
```

### is属性和component实现组件切换

```
// comname 是哪个组件名,则component就会被渲染成哪个组件
// component 就是一个占位标签 
<component :is='comname'></component>

new Vue({
    el:'#app',
    comname:'login'
})
```

### 父子组件传值

#### 父向子传值

```javascript
// 1. 先在父组件中定义好要传递的数据
new Vue({
    el:'#app'
    data:{
        msg:'我是要传递给子组件的数据'
    },
    components:{
        son:{
            template:'<div>我是子组件{{message}}</div>',
            props:['message']
        }
    }
})

// 2. 在父组件中使用子组件的时候,用绑定属性的方式将父组件中的数据传递给子组件
<div id='app'>
    <son v-bind:message = 'msg'></son>
</div>

// 3. 在子组件中定义一个props属性,该属性是一个数组,数组中定义用于接收传过来的变量。这个变量
// 和第二步绑定的这个属性同名
son:{
    template:'<div>我是子组件</div>',
    props:['message']
}

```

#### 子向父传值

> 发布订阅者 设计模式

```javascript
// 1. 定义好父子组件,定义好子组件需要传递给父组件的数据
new Vue({
    el:'#app'
    components:{
        son:{
            template:'<div>我是子组件{{message}}</div>',
            data:function(){
                return {
                    msg:'传给父组件的数据'
                }
            }
        }
    }
})

// 2. 在子组件中使用this.$emit触发一个自定义的方法名,然后传递数据
// 第一个参数就是自定义的方法名
// 第二个参数就是需要传递给父组件的数据
this.$emit('func',this.msg)

// 3. 在父组件中使用子组件时,绑定一个事件,事件名称和子组件触发的方法名同名
<div id='app'>
    <son @func = 'getmsg'></son>
</div>

// 4. 在父组件的methods中定义一个事件处理函数
methods:{
    getmsg:function(data){
        // data就是子组件传过来的数据
    }
}
```

## 生命周期钩子函数

> 回调函数:一个函数被当做参数进行传递的时候,称作这个函数为回调函数

> 构造函数:一个函数被new 关键字引导执行的时候,称作这个函数为构造函数

> 钩子函数: 一个应用程序或者框架内部提前定义好的一批函数,这些函数会在特定的时间段自动执行
>
> 生命周期: 一个程序会存在初始化 - 运行 - 销毁等阶段,这些阶段统称为该程序的生命周期

```
new Vue({
    el:'#app',
    data:{},
    methods:{},
    beforeCreated(){},
    // data中的数据和methods中的方法已经初始化完毕会去自动执行created方法
    created(){
        // 用于发生数据请求,也可以初始化一些数据
    },
    beforeMount(){},
    // 真实DOM已经渲染完毕会执行mounted函数
    mounted(){
        // 操作真实DOM
    }
    beforeUpdate(){},
    // data中的发生了变化而且被重新渲染到了界面上时才会执行
    updated(){
        // 数据更新后重新操作DOM
    },
    // 实例销毁之前,实例上面的各种属性和方法都还可以正常访问,通常可以在这里手动回收一些页面没有被释放的变量,比如清楚定时器的操作。
    beforeDestroy(){},
    // 实例已经从内存中被销毁
    destroyed(){}
})
```

## 路由

> 后端路由:监听不同的URI(地址),做不同的请求处理

> 前端路由:是专门为SPA(单页应用程序)服务,也是监听不同的地址,做页面的切换


### 基本使用步骤
```javascript
1. 创建路由对象并配置路由规则
// 1.1 创建组件模板对象
var login = {
    template:'<div>登录</div>'
}
var register = {
    template:'<div>注册</div>'
}
// 1.2 创建路由对象并配置路由规则
var router1 = new VueRouter({
    // routes用来存放路由规则,每个路由规则都是一个对象
    routes:[
        // path: 页面#后面的地址
        // component 需要显示的组件对象
        {path:'/',redirect:'/login'},
        {path:'/login',component:login},
        {path:'/register',component:register}
    ]
})

// 1.3 将创建处理的路由对象和Vue进行绑定
new Vue({
    el:'#app',
    // 将vuerouter对象注册到Vue内部,以保证在托管区域中可以使用VueRouter内部提供的组件以及其他属性和方法
    router:router1
})

2. 使用<router-view>进行占位
<div id='app'>  
    <router-view></router-view>
<div>

3. 使用<router-link>设置跳转路径
<router-link to='/login'>登录</router-link>
```


### 路由传参
#### query传参

```JavaScript
// 1. 在跳转路径后面使用查询字符串拼接参数
<router-link to = '/login?name=zs$age=18'>登录</router-link>

// 2. 在路由对应的组件内部使用`this.$route.query`获取参数
this.$route.query.name // zs
this.$route.query.id // 18
```
#### params传参

```JavaScript
// 1. 更改路由规则用于匹配参数
new VueRouter({
    routes:[
        // :id用于匹配/login/13后面的参数13
        {path:'/login/:id',component:login}
    ]
})
// 2. 在跳转路径后面使用/拼接参数
<router-link to = '/login/13'>登录</router-link>

// 3. 在路由对应组件内部使用`this.$route.params`获取参数
var login = {
    template:'<div>登录组件{{this.$route.params.id}}</div>',
    created(){
        console.log(this.$route.params.id) // 获取到的就是13
    }
}
```

### 嵌套路由
```JavaScript
// 1. 在父路由内部使用children属性配置子路由规则,子路由规则对象中的path属性不要 '/' 
// 2. 在父路由规则对应的组件模板中添加router-view用于显示子路由规则对应的组件
// 3. 在<router-link>中设置跳转路径(将父路由和子路由的path拼接)
```

## webpack的学习

> 什么是webpack? 一个基于Node的前端构建工具,可以实现对项目的打包(构建),主要解决文件(模块)之间的依赖,高级代码的转译,文件(模块)的合并和压缩等问题。

### 基本使用

- `webpack3.~`版本安装使用

```JavaScript
// 1. 全局安装webpack
npm i webpack@3.10.0 -g

// 2. 使用
webpack 入口文件 输出文件
webpack ./src/main.js ./dist/build.js
```

- `webpack4.~`版本安装使用

```JavaScript
// 1. 全局安装webpack
npm i webpack  -g
npm i webpack-cli -g

// 2. 使用
webpack 入口文件 -o 输出文件 --mode development/production
webpack ./src/main.js -o ./dist/build.js --mode development
```

### webpack配置文件使用

```javascript
// 1. 在项目根目录新建webpack.config.js,并设置 打包的入口文件 和 输出文件
module.exports = {
    // 入口文件(绝对路径)
    entry: path.join(__dirname,'./src/main.js'),
    // 输出文件
    output: {
        path: path.join(__dirname,'./dist'), // 输出文件目录
        filename: 'build.js' // 输出文件名称
    }
}

// 2. 执行打包命令
webpack
```

### webpack-dev-server的使用

> 帮助在项目根目录搭建一个服务器,该服务器可以帮我们托管代码,同时监听所有文件的变化然后自动执行webpack编译命令以及自动刷新浏览器

#### 命令行方式

> `--hot`: 热更新,可以局部更新文件
> `--open`: 自动打开浏览器
> `--port`: 配置端口号
> `--inline`: 刷新浏览器

```javascript
// 1. 局部安装webpack-dev-server和webpack
npm i webpack-dev-server webpack --save-dev

// 2. 在package.json的scripts节点中添加启动命令
"scripts":{
    "dev":"webpack-dev-server --inline --open --hot--port 8888"
}

// 3. 在项目根目录运行项目
npm run dev

```

#### 配置文件方式
```javascript
// 1. 在webpack.config.js配置文件中添加如下配置
devServer:{
    open: true,
    port: 8888,
    hot: true
}

// 2. 如果设置了 hot:true 这个属性,则需要配置热更新插件
// 2.0 引入webpack模块
const webpack = require('webpack')
// 2.1 配置热更新插件
plugins: [
    new webpack.HotModuleReplacementPlugin()
]

// 3. package.json中配置启动命令
"scripts":{
    "dev":"webpack-dev-server"
}
```

### `html-webpack-plugin`的使用
> 可以根据指定的HTML模板文件生成一个HTML页面,并且在HTML页面中自动引入打包好的js文件

```javascript
// 1. 安装html-webpack-plugin
npm i html-webpack-plugin --save-dev

// 2. 在webpack.config.js中的plugins节点中配置插件
// 2.1 导入html-webpack-plugin
const htmlWebpackPlugin = require('html-webpack-plugin')
// 2.2 配置插件
plugins:[
    new htmlWebpackPlugin({
        template:path.join(__dirname,'./src/index.html'),// 需要生成的HTML的模板文件
        filename:'index.html' // 生成的HTML文件的名称
    })
]
```

### `css-loader`处理CSS文件
> 解析处理文件后缀名为.css的文件

```javascript
// 1. 安装style-loader css-loader
npm i style-loader css-loader --save-dev

// 2. 在webpack.config.js中的moudle节点中配置解析规则
module:{
    rules:[
        {
            test:/\.css$/,
            use:['style-loader','css-loader']
        }
    ]
}
```
### `less-loader`处理less文件
> 解析处理文件后缀名为.less的文件

```javascript
// 1. 安装less less-loader
npm i less less-loader --save-dev

// 2. 在webpack.config.js中的moudle节点中配置解析规则
module:{
    rules:[
        {
            test:/\.less$/,
            use:['style-loader','css-loader','less-loader']
        }
    ]
}
```
### `scss-loader`处理scss文件
> 解析处理文件后缀名为.scss的文件

```javascript
// 1. 安装node-sass sass-loader
npm i node-sass sass-loader --save-dev

// 2. 在webpack.config.js中的moudle节点中配置解析规则
module:{
    rules:[
        {
            test:/\.scss$/,
            use:['style-loader','css-loader','sass-loader']
        }
    ]
}
```
### `url-loader`处理图片等资源文件
> 解析处理项目中引入的图片、字体图标、音视频等资源文件

```javascript
// 1. 安装file-loader ulr-loader
npm i file-loader ulr-loader --save-dev

// 2. 在webpack.config.js中的moudle节点中配置解析规则
module:{
    rules:[
        {
            test:/\.(png|jpg|gif)/,
            use:'url-loader?limit=20000&name=[hash:8]-[name].[ext]'
            // 当需要打包的文件小于limit的值,会将图片转换成base64字符串在build.js中进行引用
            // 当需要打包的文件大于limit的值,会将图片打包到输出目录中
            //一般比较小的图片才需要被转成base64,所以该值不宜太大

            // name=[hash:8]-[name].[ext]表示将来打包出来的文件名称以8位hash值和原来的名称进行拼接,文件扩展名不变
        }
    ]
}
```

### `babel`的使用

> 解析webpack默认不能处理的JS

```JavaScript
// 1. 安装babel-core babel-loader babel-plugin-transform-runtime
npm i babel-core babel-loader babel-plugin-transform-runtime --save-dev

// 2. 安装babel-preset-env babel-preset-stage-0
npm i babel-preset-env babel-preset-stage-0 --save-dev

// 3. 在webpack.config.js中的moudle节点中配置解析规则
module:{
    rules:[
        {
            test:/\.js$/,
            use:'babel-loader',
            exclude:/node_modules/ //排除掉node_modules文件夹,让babel-loader不去解析node_modules文件夹中的js文件
        }
    ]
}

// 4. 在项目根目录新建.babelrc文件,书写如下配置
{
    "presets":["env","stage-0"],
    "plugins":["transform-runtime"]
}

```

## webpack中使用Vue

```JavaScript
// 1. 安装vue vue-loader vue-template-compiler
npm i vue --save
npm i vue-loader vue-template-compiler --save-dev

// 2. webpack.config.js中配置解析规则
module: {
    rules: [
        {
            test:/\.vue$/,use:'vue-loader'
        }
    ]
}

// 3. 在index.html中加入一个需要Vue托管的区域
<div id="app"></div>

// 4. 新建App.vue组件
<template>
    <div>vue组件</div>
</template>
<script>
export default {

}
</script>
<style>
</style>

// 5. main.js中导入vue的包,渲染App.vue组件
import Vue from 'vue'
import App from './App.vue'

new Vue({
    el:'#app',
    render:c=>c(App) //将App组件渲染到id为app的区域中
})

```

## webpack 中集成vue-router

```JavaScript
// 1. 安装vue-router
npm i vue-router --save

// 2. 在main.js中导入vue-router并实例化路由添加路由规则
import VueRouter from 'vue-router'
Vue.use(VueRouter) // 保证在vue的组件中可以使用vue-router提供的组件 如<router-view> <router-link>

const router = new VueRouter({
    routes: [
        {
            path: '/login', component: login
        }
    ]
})

// 3. 将实例化的router绑定到Vue
new Vue({
    el: '#app',
    render: c => c(App),
    router // 将router实例绑定到Vue上 以保证vue实例中可以访问$route $router等属性
})

// 4. 在App.vue中添加路由占位router-view标签
<router-link to = "/login">登录<router-link>
<router-view></router-view>
```
---

## --save和--save-dev
```js
// 项目开发期间需要安装的依赖
// --save-dev 会将安装的包名以及版本号记录到package.json文件中的devDependencies节点中
npm install webpack --save-dev 等价于 npm install webpack -D

// 项目上线后需要安装的依赖
// --save 会将安装的包名以及版本号记录到package.json文件中的dependencies节点中
npm i webpack --save 等价于 npm i webpack -S

npm i 会将所有在package.json文件中的devDependencies节点和dependencies节点内所有的依赖包全部安装一遍
npm i --save-dev 只会下载devDependencies中的所有依赖包
npm i --save 只会下载dependencies中的所有依赖包

// 为什么安装包的时候要区分 --save 和 --save-dev
// 1. 使项目的阅读更加友好，便于理解项目结构
// 2. 安装对应阶段的包时更加方便
```
## vue-cli脚手架的使用

```js
// 1. 安装vue-cli脚手架
npm i vue-cli -g

// 2. 初始化项目模板
vue init webpack 项目名称
eslint(语法规范化插件) 不要安装  当安装之后只能按照ESLint中规定的语法格式去书写代码
e2e(测试框架) 不要安装
unit test(单元测试框架) 不要安装

// 3. 进入项目安装所有依赖
npm i

// 4. 运行
npm run dev

config/index.js中 17/18行改端口号和自动打开浏览器
```
## VSCode用户代码片段
```json
"Print to vue": {
		"prefix": "vuec",
		"body": [
			"<template>",
			"    <div>",
			"      御剑乘风来,除魔天地间!$1",
			"    </div>",
			"</template>",
			"           ",
			"<script>",
			"export default{",
			"    $2         ",
			"}",
			"</script>",
			"<style scoped>",
			"$3            ",
			"</style>"
		],
		"description": "create a vue template"
	}
```

## 将项目提交到码云

```
// 1. 在项目文件夹初始化git
git init

// 2. 将代码提交到暂存区
git add .

// 3. 提交代码
git commit -m '描述信息'

// 4. 关联远程分支
git remote add origin https://gitee.com/UniverseKing/tes.git

// 5. 推送到远程分支
git push -u origin master

// 6. 查看文件信息
git status

// 7. 查看log
git log --oenline

// 8. 切换版本记录
git reset --hard 版本号
```

## 设置npm镜像源为淘宝镜像
```js
 npm config set registry https://registry.npm.taobao.org
```

## Vuex的使用

> Vuex是一个状态管理库,或者说是专为Vue应用程序开发设计的状态管理模式,它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

> 注:所谓状态,可以理解成项目中各个组件需要用到的数据。

> Demo:https://gitee.com/UniverseKing/vuex-study

### 初始化公共状态

``` JavaScript
1. 安装vuex
npm i vuex --save

2. 入口文件中实例化Store
import Vuex from 'vuex'
Vue.use(Vuex)
var store = new Vuex.Store({
  // 1. 用于定义状态(公共数据),类似于Vue实例中的data方法
  state:{
      msg:'初始化的数据'
  },
  // 2. 用于修改状态,类似于Vue实例中methods
  mutations:{
      change(state,arg){
          // 更改状态
          state.msg = arg
      }
  },
  // 3. 用于获取数据(获取数据之前可以进行一些操作),类似于Vue实例中的过滤器和计算属性
  // getters 主要会用在跨组件传值
  // getters 中定义的方法内部依赖的数据发生变化会自动重新调用函数计算返回值
  getters:{
      fixmsg(state){
          return `${state.msg}----处理后的数据`
      }
  },
  // 4. actions和mutations都是定义对数据进行操作的方法,mutations中都是同步方法,mutations中定义异步方法
  // Action 提交的是 mutation，而不是直接变更状态。所以需要修改状态还是需要使用mutations中定义的方法

  // 从网络请求回来的数据需要保存到store
  // 发送网络请求的方法可以定义到actions中
  // actions主要用于处理异步方法
  actions:{
      asyncchange(context,arg){
          // 异步方法
          setTimeout(() => {
              context.commit('change',arg)
          }, 3000)
      }
  }
})

3. 注入到Vue实例中
new Vue({
    el:'#app',
    store
})
```

### 使用状态

``` JavaScript
1. 使用state中的数据
JavaScript: this.$store.state.msg
HTML: $store.state.msg

2. 使用getters中的数据
JavaScript: this.$store.getters.fixmsg
HTML: $store.getters.msg
```

### 变更状态(修改数据)

> 状态的变更必须使用mutations中提供的方法进行修改

``` JavaScript
1. 提交mutations中的变更方法
this.$store.commit('change','我是被修改的数据')

2. 异步提交actions中的变更方法
this.$store.dispatch('asyncchange','我是被异步修改的数据')
```

### 使用辅助函数

> 辅助函数可以直接将`state`,`getters`中的数据映射到Vue组件中的计算属性上,可以将`mutations`,`actions`中的方法映射到组件中的`methods`中,然后在组件中就可以直接以属性和方法的方式去使用数据和方法。

``` JavaScript
import { mapState } from 'vuex'
import { mapGetters } from 'vuex' 
import { mapMutations } from 'vuex'
import { mapActions } from 'vuex'

new Vue({
    computed:mapGetters([
        'count'
    ])
})
// ==>等价于
new Vue({
    computed:{
        count(){
            return this.$store.state['count']
        }
})
```

 https://github.com/UniverseKing/awesome-github-vue#UI%E7%BB%84%E4%BB%B6

----
## Vue面试题

### v-on事件修饰符和键盘修饰符的作用?

> 事件修饰符主要是用于处理浏览器上的默认行为,比如阻止事件的冒泡,阻止a标签、form表单的默认事件

> 键盘修饰符主要针对keyup、keydown,如果希望事件由指定的键进行触发,可以用键盘修饰符进行指定

- 阻止默认行为
```html
<a v-on:click.prevent = 'jump' href = '#'></a>
<form @click.prevent = 'jump'></form>
```
- `once`:只触发一次事件
- `self`:保证事件由自己触发,不会经过冒泡或者捕获进行触发
```html
<div @click = 'divClick'>
    <p @click.self = 'pClick'></p>
</div>
```

```html
<input @keyup.f1 = 'inpKey'></input>
```
```js
// 自定义键盘修饰符
// Vue.config.keycodes.f2 = 112
```

- `stop`:阻止冒泡


### v-if和v-show的区别,使用场景?

> v-if和v-show都是用来控制元素的显示和隐藏,当值是true，元素显示，值为false,元素隐藏

> 区别: v-if当切换布尔值时,会创建/删除元素；v-show当切换布尔值时,会改变元素的样式,`display:block`

> 使用场景:当元素显示隐藏切换频繁时使用v-show,反之使用v-if; 例如:页面加载数据时的loading动画可以使用v-if;

> 页面中某个元素需要使用动画效果,这个动画效果需要人为进行操作控制,那么最好使用v-show,加入购物车时的小球飞入动画

### v-cloak解决插值表达式闪烁的原理?

> 为什么出现闪烁问题?

> 由于网速等原因导致vue.js没有被加载回来,此时页面中的插值表达式不会被vue实例解析,浏览器进行解析的时候回直接当做字符串;然后vue.js加载回来之后,vue实例又能够进行解析,那么插值表达式就会被解析成具体的值,这个过程会出现闪烁现象

> v-cloak原理

> 使用v-cloak指令绑定到元素上面之后,再配合`[v-cloak]{display:none}`这个样式进行控制;

> 在浏览器进行解析时,浏览器会将属性选择器的样式作用于元素身上,该元素会被隐藏;原后vue进行解析时,会将`v-cloak`从元素身上删除,样式也随之失效,插值表达式中的值也能够显示出来。

```js

<style>
    [v-cloak]{
        display:none
    }
</style>


<div v-cloak>{{msg}}</div>

new Vue({
    data:{
        msg:'hello word'
    }
})

<script src = './vue.js'></script>
```

### 什么是Vue双向数据绑定?原理?

> v-model指令、数据视图同步更新、使用的是ES5提供的`Object.defineProperty()`这个方法实现数据劫持

> 数据劫持(Object.defineProperty())

> 数据如何从模型同步到视图?当模型中数据发生变化时会触发Object.defineProperty的set方法,在这个方法内部能够劫持到数据的改变,然后就可以在该方法内部通知视图更新

> 视图中的数据如何同步到模型中?(v-model指令是怎么实现改变了元素中的数据同步到模型中)监听表单元素的change事件,在change事件中可以拿到用户输入的数据,然后给模型中的数据赋值

```js
var obj = {}
// 给对象设置属性
obj.name = 'zs'
// 获取对象的某个属性
console.log(obj.name)


var value = 'zs'
Object.defineProperty(obj,'name',{
    get(){
        return value
    },
    set(v){
        // 设置值的时候就是数据劫持
        // 只要走这个方法表示模型中的数据在发生变化
        // 调用视图更新的函数
        update()
        value = v
    }
})

obj.name = 'ls'
console.log(obj.name) // ls

function update(){
    // 专门用来更新视图
}
```


### Angular双向数据绑定?

> 脏数据检测: 会使用定时器进行轮询,并不是将定时器一直开着,只有触发了指定的一些方法时才会进行轮询,$apply,发送异步请求,触发了定时器时也会轮询

```js
<div>{{msg}}</div>
<input @change = 'inpChange($event)'></input>
<button @click = 'change'></button>

new Vue({
    data:{
        msg:'hello',
        name:'zs'
    },
    methods:{
        change(){
            this.msg = 'word'
        },
        inpChange(v){
            this.msg = v.targe.value
        }
    }
})
```

### scoped作用原理?怎么解决加了scoped后动态渲染的HTML标签样式不能修改的问题?

> scoped可以隔离组件之间的样式,避免样式污染

> 作用原理:当一个组件的style加了`scoped`后,首先vue解析当前组件时,会给当前组件中所有元素加上一个随机的属性,然后style中书写的样式会全部变成属性选择器,那么即使其他组件有相同的样式或者相同的标签，由于随机添加的属性是不一样的,组件之间的样式也不会相互影响

> 当在父组件中取修改子组件中标签样式,可以使用[vue-loader深度作用选择器](https://vue-loader.vuejs.org/zh/guide/scoped-css.html#%E6%B7%B1%E5%BA%A6%E4%BD%9C%E7%94%A8%E9%80%89%E6%8B%A9%E5%99%A8),在父组件的样式名和子组件的样式名之间添加`>>>`或者`/deep/`

### Vue生命周期函数的理解?

> 生命周期就是在vue实例执行过程中会触发的一批函数,这些函数可以帮助我们处理不同时间段的业务逻辑

> 生命周期的四个阶段:

> 实例创建阶段:

- `beforeCreate`:vue实例被创建时触发,此时仅仅是分配了内存,vue实例上的属性和方法都还没被绑定
- `created`:vue实例被创建完毕,data中的属性和methods中的方法都已经被挂载到了vue实例上

> DOM渲染阶段:

- `beforeMount`:vue实例中的数据已经被解析渲染到了内存中的虚拟DOM上,但真实DOM中指令还没有被解析
- `mounted`:vue实例中的数据已经完全被渲染到了真实DOM中

> 数据更新阶段:vue实例上的数据发生变化时触发

- `beforeUpdate`:模型中数据已经发生变化,但还没有同步更新到视图中
- `updated`:模型中数据发生了变化,而且已经同步到了视图中

> 实例销毁阶段:

- `beforeDestroy`:vue中的数据和方法还能继续使用,但是指令不能再被vue解析
- `destroyed`:vue中的数据和方法都已经被销毁

### vue-router路由模式有几种,原理分别是什么?

> vue-router路由库是用哪种技术实现的,总共有两种,分别叫hash模式和history模式,默认是hash模式

> hash模式:地址上带有#号;url地址可以放在任意标签中打开;可以兼容低版本的浏览器

> hash模式原理:监听`hashchange`事件,可以调用`window.location.hash`获取到锚点值,和路由规则进行匹配,匹配到之后将路由规则中定义的组件渲染到页面

> history模式:地址上没有#号,更加符合URL形式;url地址不能重复打开;

> history模式原理:利用HTML5新提供的`history.pushState` API 来完成 URL 跳转而无须重新加载页面

> history模式需要后台进行相关配置:要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面，这个页面就是你 app 依赖的页面

### vue-router导航钩子函数使用场景?

> `beforeEach`:全局守卫,在路由跳转时会对路由进行拦截,只有调用了`next`函数才会释放路由,使用场景:通常在后台管理系统中,页面是需要登录之后才能访问,那么对于所有的页面跳转都需要使用`beforeEach`进行拦截判断是否登录

> `beforeEnter`:路由独享守卫,只会拦截加了独享守卫的路由跳转。使用场景:如果整个项目中只有某一部分页面是需要登录之后才能访问,此时只能针对这一部分页面的路由规则加上独享守卫进行拦截。

### vue-router路由懒加载怎么使用?(单页应用程序的性能优化)

> 路由懒加载:使用懒加载可以在跳转到具体路由时才去加载对应的组件代码,没有访问的路由的组件代码永远不会加载回来。

> 用法:将导入组件的方式换成:`const Foo = () => import('./Foo.vue')`

### Vue.use()方法作用?Vue插件实现?

> Vue.use是用来安装Vue的插件

> 插件可以实现的功能:

```js
MyPlugin.install = function (Vue, options) {
  // 1. 添加全局方法或属性
  Vue.myGlobalMethod = function () {
    // 逻辑...
  }
  // 2. 添加全局资源(指令、过滤器、组件)
  Vue.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // 逻辑...
    }
  })
  // 注册过滤器
  Vue.filter('datefmt',function(){
      return '2018-06-12'
  })
  // 注册组件
  Vue.component('com',{})

  // 3. 混入组件(将一个组件的功能混入到另一个组件中)
  Vue.mixin({
    created: function () {
      // 逻辑...
    }
  })
  // 4. 添加实例方法
  Vue.prototype.$myMethod = function (methodOptions) {
    // 逻辑...
  }
}
```

> 如何实现插件:插件必须实现一个`install`方法,这个方法接收一个参数是Vue,这个方法内部就可以去添加全局的组件以及实例的属性。然后在使用插件时,调用Vue.use(插件对象),则install方法中的形参Vue就会接收到实参Vue。

### axios拦截函数的使用场景?请求拦截/响应拦截

> 请求拦截:在发送请求之前,对请求对象做相关配置,给请求头添加验证属性(登录校验)、设置参数数据格式(Content-Type)

> 响应拦截:当接口返回的信息是统一的类型时,如果都在具体的请求函数里面去做处理,比较繁琐,可以在响应拦截器中先做统一处理,在分发到具体的函数中。例如当后台的token过期时,如果在每一个请求函数中都做判断,然后跳转页面比较麻烦,由于token过期后台会返回规定的统一状态码,那么就可以使用响应拦截在拦截器中进行判断然后跳转

```js
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    if(response.code = 400){
        this.$router.push('/login')
    }
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```

## JSONP跨域原理?其他解决跨域方式?

> 跨域一般情况只存在于开发阶段
> 前端项目运行在192.168.1.100
> 后端服务器运行在192.168.1.200

> 项目上线:固定的服务器IP地址172.223.89.21 
> 前端项目和后端服务器代码都会运行到这个线上服务器,那么就保证了前后端的域名、端口号可以一致了，就不会存在跨域问题

- **JSONP**:JSON数据格式的一种使用方式,用于解决跨域问题
- **跨域**: 由于浏览器同源策略的限制,不同源的URL不能互相通信,这种现象叫做跨域
- **同源策略**: 两个域名只有 **协议** **主机地址** **端口号**完全一致时才能相互通信访问
- **JSONP实现原理**：利用script标签的src属性发送请求不受同源策略的限制这个机制去实现跨域资源共享(script标签能够跨域访问是历史遗留问题,并不安全) 

    1. 动态创建一个script标签,并且将需要请求的URL地址设置给script标签的src属性,同时在URL地址后面拼接上一个回调函数名称

```js
function getJson(data){
    console.log(data)
}
// getJson就是在前端本地定义好的一个方法的方法名
<script src = 'http://lovegf.cn/jsonp?callback=getJson'>
```
    2. 后台接收到该请求后,先根据参数判断是否是JSONP请求,如果是,则将客户端请求的数据整理好,和前端发送过来的方法名一起拼接成一个函数调用的字符串,客户端需要的数据就是这个函数调用时传的参数
```js
// 假设要发送给前端的数据是 'ok'
'getJson('ok')'
// 响应给前端
res.end('getJson('ok')')
```
    3. 由于后台响应回来的数据是一个字符串,而且是函数调用,所以前端拿到响应回来的数据相当于调用了该方法,那么前端定义好的方法会自动执行,而且方法内的形参可以接收到实参值,也就是后台拼接的数据
```js
function getJson(data){
    console.log(data) // ok
}
```
    4. 数据拿到之后,一般这个动态创建的script标签会被删除掉


### CORS

```js
res.header("Access-Control-Allow-Origin", "*");
res.header("Access-Control-Allow-Headers", "X-Requested-With");
res.header("Access-Control-Allow-Methods","*");
```

### 代理(解决的是开发阶阶段跨域问题,项目上线的时候代理服务器其实是不会运行的,所有如果存在跨域是不能解决的)

> 代理是通过服务器向服务器发送数据请求不存在跨域问题的机制解决浏览器向存在跨域问题的服务器发请求的技术

> C -> S

C运行在A电脑上 S运行在B电脑上
> 在A电脑上搭建一个服务器P P去请求S
> C再去请求P,这时由于C和P在同一台电脑,那么可以保存不存在跨域问题了

> C -> P -> S


// 跨域通常存在于开发阶段
http://192.168.0.111/api/getlist

http://192.168.0.112/index.html


http://172.16.273.73/api/getlist
http://172.16.273.73/index.html


## vue-cli配置代理解决开发阶段跨域问题

```JavaScript
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
},
```


## 什么是深拷贝?深拷贝浅拷贝的区别?如何实现?

> 深拷贝讨论的对象 主要是指定js中的复杂数据类型

> 最简单的实现方式:JSON.parse(JSON.stringify(obj)),这种方式的弊端不能实现对象中函数的拷贝

> 比较常见的实现方式: for-in循环加上递归

> for-in 循环需要判断不同的数据类型,如果是对象使用递归克隆,如果是数组,可以直接调用concat方法或者splice方法进行深拷贝,如果是函数/Date,使用constructor重新实例化函数和时间类型

```js
function deepClone(obj) {
    if (obj === null || typeof obj !== 'object') {
        return obj;
    }
    if (obj instanceof Array) {
        var copy = [];
        for (var i = 0; i < obj.length; i++) {
            copy.push(obj[i]);
        }
        return copy;
    }
    if (obj instanceof Date) {
        var copy = new Date();
        copy.setTime(obj.getTime());
        return copy;
    }
    if (obj instanceof Object) {
        var copy = {};
        for (var key in obj) { //递归 
            if (obj.hasOwnProperty(key)) {
                copy[key] = deepClone(obj[key]);
            }
        }
        return copy;
    }
}
```

> https://juejin.im/entry/57bd3817a34131005b21cbdb


## Restful API理解

> REST是所有Web应用都应该遵守的架构设计指导原则。

> Restful API是基于REST原则设计开发出来的接口，本质上只是一种规则

> 面向资源是REST最明显的特征，对于同一个资源的一组不同的操作。资源是服务器上一个可命名的抽象概念，资源是以名词为核心来组织的，首先关注的是名词。REST要求，必须通过统一的接口来对资源执行各种操作。对于每个资源只能执行一组有限的操作。（7个HTTP方法：GET/POST/PUT/DELETE/PATCH/HEAD/OPTIONS）


> 传统接口开发风格:请求方法 + 动词 + 名词

获取用户列表: http://www.lovegf.cn:8899/api/getusers  get方法
获取用户详情: http://www.lovegf.cn:8899/api/getuserinfo get方法
新增用户: http://www.lovegf.cn:8899/api/adduser       post方法
编辑用户: http://www.lovegf.cn:8899/api/edituser      post方法
删除用户: http://www.lovegf.cn:8899/api/deleteuser    get方法

> Restful API风格: 请求方法 + 名词单复数(通过明确的请求方法和名词来操作同一个资源)

获取用户列表: http://www.lovegf.cn:8899/api/users     get方法
获取用户详情: http://www.lovegf.cn:8899/api/user/:id  get方法
新增用户: http://www.lovegf.cn:8899/api/user          post方法
编辑用户: http://www.lovegf.cn:8899/api/user/:id      put方法
删除用户: http://www.lovegf.cn:8899/api/user/:id      delete方法

## [图片懒加载原理](https://www.cnblogs.com/flyromance/p/5042187.html)

> 1. 当数据请求回来之后,有多少图片数据就创建多少个img标签,先给img标签的src属性设置一张占位图,然后给img标签设置一个自定义属性,属性值为真实的图片地址,例如:`<img data-src='http://xxxxxx.png' src='placeholder.jpg'/>`

> 2. 当页面进行滚动时,监听滚动的偏移和img元素的位置,当img元素出现在可视范围之内时,将当前img的src属性值设置为data-src属性的值,只要img标签src属性为真实图片地址,自然会去请求图片回来,对于还没有出现在可视范围之内的img,由于src属性没有设置网络地址,则不会去请求,这样图片的懒加载就可以实现了。



## 到公司后需要做的准备工作 

- 配置开发环境(安装Node、git(svn)、编辑器)

    + sourcetree(git版本管理工具)
    + svn(TortoiseSVN)
    + vscode自带git管理
    + 公司内部会有自己的git服务器,所有的代码会放到自己的git服务器,在进入公司后,一般组长会给你一个项目的git地址和用户名、密码.

- 获取开发需要的文档或者代码

    + 原型图、UI图
    + 项目说明文档(逻辑交互文档)
    + 接口文档

- 二次开发

    + 找到之前开发者留下的文档信息
    + 阅读别人的代码
      (先看整体架构，总结出别人的技术栈:vue-cli+iview+fetch+vuex;找到自己要实现的功能，先看这个功能别人在项目中有没有类似的，如果有找到代码进行复制以及修正)
    + 注意查看别人书写的注释

- 新项目

    + 确定技术栈:vue-cli + element-ui/iview(mint-ui/vux) + axios + vuex
    + 确定需要实现的第一个模块是什么
    + 开发中一定是按照一个模块一个模块的进行开发
    + 遇到bug,如果半个小时解决不了,先做其他的。提高自己的开发效率

## 谈谈Promise的理解?怎么实现?

> Promise其实内部也有一个defers队列存放事件，.then的事件就在里面,程序开始执行的时候，.then就已经放入下一个事件，然后后面当异步操作完成时，resolve触发事件队列中的事件，便完成了一个.then操作，每次异步操作完成通过resolve触发事件并将事件从事件队列中移除，通过事件队列中的事件的resolve使事件的触发持续下去

## MVC、MVVM理解?

## 虚拟DOM的理解?实现?

## 工作中处理过的兼容性问题?

> [兼容性问题处理](https://juejin.im/entry/5b22686be51d4558af4015a1)

> [移动端开发的兼容问题](https://juejin.im/entry/5a17ad3af265da432240e1f2)

> 移动端滚动穿透

> 移动端输入框被键盘挡住问题

> IOS滚动不平滑的问题

> click 300ms 延时响应 

## 处理过的性能优化问题?

[性能优化](https://juejin.im/post/5b0bff30f265da08f76cc6f0)
[前端性能优化--从 10 多秒到 1.05 秒](https://juejin.im/post/5b0bff30f265da08f76cc6f0)

### webpack里面

1. 路由懒加载(组件按需加载)
2. 分离第三方包(抽取第三方模块安装的包的代码,较少build.js的体积)
3. 分离CSS

### JS
1. 对高频触发的事件进行节流或消抖(touchmove)
2. 尽量减少 HTTP 请求个数——须权衡(精灵图是合并请求,分离第三方包/css是拆分请求)
3. 避免空的 src 和 href
4. 减少 DOM 访问,减少DOM访问层级(迫不得已需要访问DOM,可以将DOM进行缓存)

5. 使用 CDN（内容分发网络）(静态资源使用CDN加速,用户访问时从CDN获取资源,CDN根据IP地址直接返回当前城市服务器中的资源)


## 工作中遇到的难解决的问题?后来是如何解决的?

- 组件绑定原生事件,无法绑定时使用native修饰符

- 后台系统中根据权限动态添加路由规则
```js
// 根据后台返回的权限列表,动态的向路由规则对象的routes数组中添加路由规则
	router.addRoutes([
	   {
		path:'/',
		component:user	
	])
```

- scoped问题(父组件无法直接修改子组件以及v-html渲染出来标签的样式,vue-loader深度作用选择器)

```js
// css中使用 >>> 作为操作符
.father >>> .son {

}
// css或者less中使用 /deep/ 作为操作符
.father /deep/ .son {

}
```

- mui.css引入到vue脚手架,打包时会报SVG图片属性符号错误(需要将mui.css文件中引入SVG图片的单引号改成双引号)

- mui严格模式(当webpack项目中引入mui.js,会报caller,callee，arguments在严格模式下无法使用的错误)

```js

// 真正问题产生是由于babel-loader在编译代码时会加严格模式限制
// 方法一: .babelrc文件中忽略不需要使用严格模式转换的文件路径
 "ignore": [
    "./src/js/mui/mui.min.js"
  ]

// 方法二: babel-loader配置中排除掉不需要严格模式转换的文件
{ 
    test: /\.js$/,
    use: 'babel-loader',
    // exclude: /mui\.min\.js$/
    exclude:["./src/js/mui/mui.min.js",'./src/js/mui/mui.js']
}

// 方法三: babel-plugin-transform-remove-strict-mode 移除整个项目打包编译时的严格模式
// https://www.npmjs.com/package/babel-plugin-transform-remove-strict-mode
1. 安装babel-plugin-transform-remove-strict-mode
npm install babel-plugin-transform-remove-strict-mode --save-dev

2. babelrc中添加
{
  "plugins": ["transform-remove-strict-mode"]
}
```
- mui中的滚动会在浏览器控制中报警告,需要添加一举样式代码

```css
* { touch-action: pan-y; }
```

- mui.js导入之后,滚动效果的js代码和tabbar产生了冲突,导致路由跳转失效

> **原因**: js中有操作类名的代码,这些代码中的类名和tabbar上的类名一模一样
> **解决**: 需要自己手动的更改tabbar上的类名为自定义类名,然后将系统默认类名中样式拷贝过来


## SPA单页应用程序性能优化

> 问题:首页加载速度过慢,首页白屏

> 1. 分离第三方包:将自己写的代码和引用的第三方模块分开打包,一个js文件变成多个js文件之后,由于浏览器多线程的原因,当加载完第一个js的时候,可以使用一个线程解析渲染第一个js的内容,同时又可以用另外一个线程下载其他的js

> 2. 分离css文件:将css从js中分离出来后,在解析js的时候,又可以去下载对应的css

> 3. 路由懒加载:当用户访问到某个路由时,才会去加载该路由对应的组件代码,对于没有访问的页面,这些组件的代码一直不会被加载回来,可以提高首页的访问速度

> [路由懒加载](https://router.vuejs.org/zh/guide/advanced/lazy-loading.html#%E6%8A%8A%E7%BB%84%E4%BB%B6%E6%8C%89%E7%BB%84%E5%88%86%E5%9D%97)

```js
// 路由懒加载需要将导入组件的方式由 import 组件名 from '路径'改成以下代码
const 组件名 = () => import('路径')
```

## 其他
```
webpack打包:是将项目开发阶段的代码进过编译整合到一起,输出静态文件,包含html、css、js
App打包: 是将前端开发好的移动web打包成一个可以安装到手机上的App
- HBuilder打包:在线打包,不需要依赖任何环境,是将我们的代码上传到hbuilder所在公司的服务器里面进行打包,打包完成后将包在发送回来
- cordova
- ionic
- AppCan

移动web:在手机浏览器上运行的网站
移动App:安装到手机上运行的应用程序
android包文件名称:xxxx.apk
iOS包文件名称:xxxx.ipa

所有第三方模块:第三方登录、分享、推送、支付、地图这些功能如果要实现,都需要到对应的开发者网站注册账号,
在网站里面新建一个应用程序,会生成一个appid和一个key
```

## 项目中用到的技术

> webpack + axios + ES6 + mui 是所有框架都可以使用的技术

```js
// 技术栈:
(vue + vue-router + webpack)vue-cli + vuex + element-ui + axios + ES6

// 技术:
vue: 系统指令 + 生命周期函数(created、mounted) + 组件传值(父子组件通信) + 自定义过滤器  + ref获取DOM
     + watch + 过渡动画(css原生类实现路由切换动画/动画钩子函数实现购物车小球动画)

vue-router: 基本的路由配置 + 嵌套路由 + 路由导航钩子函数(beforeEach、beforeEnter) + 编程式导航(this.$router.push('/home')) + 路由高亮(linkActiveClass) + 路由传参(params/query) + 重定向(redirect) + 路由跳转(this.$router.go(-1))

webpack: 加载器 + 插件

vuex: state(定义公共数据) + mutations(定义修改数据的方法) + getters(定义随着依赖属性变化的数据)

element-ui: el-from + el-table + 弹窗 + 分页 + 面包屑 + 导航栏

mint-ui/mui: 轮播图 + 提示框(Toast) + 上拉加载更多/下拉刷新 + tabbar + 横向滚动条

axios:GET/POST/PUT/DELETE + 拦截器(请求拦截/响应拦截) + ResfulAPI
vue-resource:是vue的一个插件,需要使用Vue.use()进行绑定

ES6:箭头函数 + 模板字符串 + 函数的简写 + Promise

深度作用选择器: css中用 >>> scss和less中用/deep/

百度的echarts图表库集成

动态路由:通常后台会根据当前用户的权限返回该权限应该能够访问的路由规则,前端使用路由的router.addRoutes()方法添加路由规则

本地存储:localStorage
.
```
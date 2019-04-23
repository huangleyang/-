## Vue学习

### 框架和库的区别

> 框架:提供了一整套解决方案。大而全。
> 库:只专注某一项功能。小而精

### MVC和MVVM的异同

- 相同点
> 都是一种设计模式(规范、原则)
> 都是将一个软件、框架的设计分成三个角色
> MVC: Model(模型)->数据  View(视图)->界面 Contoller(控制器) -> 调度者
> MVVM: Model(模型)->数据  View(视图)->界面 ViewModel(视图模型) -> 调度者

- 区别

> MVC:通常是后端进行软件开发的设计模式(特殊的前端框架:angular1.x使用的是MVC设计模式)
> MVVM:通常用于前端框架或者项目开发的设计模式(C#最早提出MVVM其实用于后端开发)

> MVC控制器(C)中的代码就是所有的业务逻辑,负责M和V之间的数据通信，所有代码都需要程序员自己手动实现
> MVVM视图模型(VM)是框架内部提供好的机制，也可以负责M和V之间的数据通信(数据通信的业务逻辑不需要程序员自己去写)

> MVC是单向通信，MVVM是双向数据绑定: MVC数据在通信过程中只能从M到V或者从V到M,在数据传递的过程中,必须经由C进行数据的处理；MVVM数据通信过程是双向的，M和V的数据绑定之后,可以互相影响。

> 双向数据绑定的概念:模型中数据发生变化可以立即更新到视图展示,视图中数据发生变化可以立即被更新到模型。

### Vue指令学习

```js
// 1. 导入vue.js 核心包
<script src="./lib/vue-2.4.0.js"></script>
// 2. 创建Vue实例
<script>
var vm = new Vue({
  el:'#app',
  data:{
    msg:'前端24期Vue学习'
  }
})
</script>
// 3. html部分定义管理区域
<div id='app'>
  {{ msg }}
</div>
```

- `v-cloak`:用于解决`{{}}`闪烁的问题

```js

<style>
  [v-cloak]{
    display:none
  }
</style>

<div id='app'>
 <div v-cloak>
   <h3>{{ msg }}</h3>
   <p>{{ msg }}</p>
 </div>
</div>
// 1. 导入vue.js 核心包
<script src="./lib/vue-2.4.0.js"></script>
// 2. 创建Vue实例
<script>
var vm = new Vue({
  el:'#app',
  data:{
    msg:'前端24期Vue学习'
  }
})
</script>
```

- `v-text`:用于展示纯文本数据

```html
<div v-text="msg"></div>
<div v-text="'hello'"></div>
```

- `v-html`:用于展示富文本数据(带有HTML标签格式的文本)

```html
<div v-text="'<p>富文本数据</p>'"></div>
```

- `v-bind`: 用于动态绑定属性

```html
<script>
var vm = new Vue({
  el:'#app',
  data:{
    img:'http://www.lovegf.cn:8889/1.jpg'
  }
})
</script>

<img v-bind:src='img' />
<img :src='img' />
```

- `v-on`: 用于绑定事件

```html
<button v-on:click='btnClick'>按钮</button>
<button @click='btnClick'>按钮</button>
<script>
var vm = new Vue({
  el:'#app',
  data:{},
  methods:{
    btnClick(e){
      console.log(e)
    }
  }
})
</script>
```

- `v-model`: 用于表单元素的双向数据绑定

```html
<script>
var vm = new Vue({
  el:'#app',
  data:{
    msg:'123456'
  }
})
</script>
<!-- <input type='text' value='123456' /> -->
<!-- v-model指令会将指令值和表单元素的value属性进行关联 -->
<input type='text' v-model='msg' />
```

- `v-for`：用于循环渲染DOM结构

> v-for循环的key属性的作用:
> 1. key属性可以将循环生成的DOM结构和数据进行关联绑定,可以保证后期数据有添加时,结构和数据也不会错乱
> 2. key属性可以提升渲染的性能(虚拟DOM)

```html
<div v-for='(item,index) in list' :key='index'></div>
```

- `v-if`: 使用bool值控制元素的显示和隐藏

> 利用添加和删除DOM元素实现元素的显示和隐藏
> 不适用频繁切换元素显示隐藏的场景,如果元素初始状态为不显示可以考虑使用v-if

- `v-show`：使用bool值控制元素的显示和隐藏

> 利用样式`display:none`控制元素的显示和隐藏
> 适用频繁切换显示隐藏元素的场景

### 事件修饰符

```html
<input type='text' v-model='msg' @keyup.enter='fn' />
```

- `stop`:阻止事件的冒泡
- `prevent`:阻止默认行为
- `capture`:实现事件的捕获机制
- `self`: 保证事件时由自己触发(不能通过冒泡、捕获等机制触发事件)
- `once`: 可以保证事件只被触发一次
- `enter`: 可以保证键盘事件只能在按回车时触发


### 样式绑定

#### class类名绑定

```html
<!-- <div class="box mydiv"></div> -->
<!-- 1. 使用数组给元素绑定多个类名 -->
<div :class="['box','mydiv']"></div>
<!-- 2. 使用三元表达式或者对象的方式控制某个类名是否需要添加 -->
<!-- 2.1 三元表达式 -->
<div :class="['box',flag===true?'mydiv':'']"></div>
<!-- 2.2 对象-->
<div :class="['box',{'mydiv':false}]"></div>
<!-- 3. 使用对象的方式绑定类名,可以利用对象的值控制类名是否需要添加 -->
<div :class="{box:false,mydiv:true}"></div>
<!-- 3.1 将对象抽离到vue实例的data中 -->
<div :class="cssobj"></div>
```

#### style行内样式绑定

```html
<!-- 绑定style样式 -->
<!-- <h1 style="color: red;font-size: 30px"></h1> -->
<!-- 1. 给style绑定对象 -->
<h1 :style="{color:'red','font-size': '30px'}"></h1>

<!-- 2. 给style绑定数组 -->
<h1 :style="[{color:'red','font-size': '30px'},{"background-color:'blue'"}]"></h1>
```

### 过滤器

> 对数据进行处理，处理完毕之后再展示到页面中,不会修改原数据
> 注意：过滤器只能用在插值表达式和v-bind指令中

#### 全局过滤器

> 全局过滤器可以被任何Vue实例的管理区域使用

```js
// 静态函数
Vue.filter('过滤器名称',function(value){
  // value是需要处理的原始数据
  // 对原始数据进行处理
  return 处理完毕的数据
})

<!-- 过滤器的定义 -->
Vue.filter('fmtphone',function(value){
  return value.substr(0,3) + '****' + value.substr(7,4)
})

<!-- 过滤器的使用 -->
<p>{{phone | fmtphone}}</p>
<input type='text' :value='phone | fmtphone' />

```

#### 私有过滤器

> 只能在定义的这个Vue实例管理的区域中使用

```html
<p>{{phone | fmtphone('※※※※')}}</p>
```
```js
  new Vue({
    el:'#app',
    data:{},
    methods:{},
    // 私有过滤器
    filters:{
      fmtphone:function(value,arg){
        return value.substr(0,3) + arg + value.substr(7,4)
      }
    }
  })
```

#### 键盘修饰符

> 使用键盘修饰符可以指定某些建触发事件

```html
<!-- 
  键盘修饰符:
  enter
  tab
  esc
  del
  left
  right
  up
  down
  space
 -->
<input type='text' @keyup.13='fn' />
<input type='text' @keyup.esc='fn' />
<input type='text' @keyup.f1='fn' />


<!-- 自定义键盘修饰符 -->
Vue.config.keyCodes.f1 = 112
```

### 数组方法扩展

- `Array.prototype.forEach`:替代for循环对数组进行循环遍历

```JavaScript
var list  = [1,2,3,4,5]
list.forEach(function(v,i){
    // v是当前遍历的这一个元素 i是这个元素的索引
    console.log('当前元素是' + v,'当前元素的索引是' + i)
})
```

- `Array.prototype.map`:对数组进行遍历,同时会返回一个新数组,新数组中的每一项都是回调函数中的返回值

```JavaScript
var list = [1,2,3,4,5]
var newArr = list.map(function(v,i){
    // v是当前遍历的这一个元素 i是这个元素的索引
    console.log('当前元素是' + v,'当前元素的索引是' + i)
    return v * 2
})
console.log(newArr) // [2,4,6,8,10]
```

- `Array.prototype.findIndex`:对数组进行遍历,会返回一个满足回调函数条件的元素的索引

```JavaScript
var list  = [1,2,3,4,5]
var index = list.findIndex(function(v,i){
    // v是当前遍历的这一个元素 i是这个元素的索引
    console.log('当前元素是' + v,'当前元素的索引是' + i)
    if(v == 4){
        // 当return true时终止循环 同时将该元素的索引返回给findIndex函数
        return true
    }

    // 简写成
    // return v == 4
})
console.log(index) // 3
```

- `Array.prototype.filter`:对数组进行遍历,返回一个新数组,新数组中的每一项都是满足回调函数中条件的元素,当满足条件时需要`return true`

```JavaScript
var list = [1,2,3,4,5]
var newArr = list.filter(function(v,i){
    // v是当前遍历的这一个元素 i是这个元素的索引
    console.log('当前元素是' + v,'当前元素的索引是' + i)
    if(v%2 == 0){
        return true
    }

    // 简写成
    // return v%2 == 0
})
console.log(newArr) // [2,4] 
```

- `Array.prototype.some`:对数组进行遍历,返回一个布尔值,判断当前数组中是否有一个元素符合回调函数中的条件(特点:只要一个元素满足条件,最终返回结果就是true)

```JavaScript
var list  = [1,2,3,4,5]
var flag = list.some(function(v,i){
    // v是当前遍历的这一个元素 i是这个元素的索引
    console.log('当前元素是' + v,'当前元素的索引是' + i)
    if(v % 2 == 0){
        // return true时则终止继续循环
        return true
    }
})
console.log(flag) // true

// 注意: 由于遍历的数组第二个元素就满足了条件 所以循环到第二个元素时 就停止循环了
```

- `Array.prototype.every`:对数组进行遍历,返回一个布尔值,判断当前数组中是否每一个元素都符合回调函数中的条件(特点:只要一个元素不满足条件,最终返回结果就是false)

```JavaScript
var list  = [1,2,4,6,8]
var flag = list.every(function(v,i){
    // v是当前遍历的这一个元素 i是这个元素的索引
    console.log('当前元素是' + v,'当前元素的索引是' + i)
    if(v % 2 == 0){
        // return true时则终止继续循环
        return true
    }
})
console.log(flag) // false

// 注意: 由于遍历的数组第一个元素就不满足条件 实际上循环一次就停止了
```



### 自定义指令

> 自定义指令其实就是对DOM操作的封装,便于复用、提升操作DOM的性能

####  全局指令

```js
Vue.directive('focus',{
    // 指令的生命周期函数(钩子函数)
    
    // 执行时机:指令在内存中绑定到DOM对象身上
    // 应用场景:凡是给DOM添加相关属性都可以在这个函数书写代码
    bind(el,binding){
        // el: 使用当前指令的DOM对象
        // binding.value 指令等号后面变量的值
        // binding.expression 指令等号后面的变量
    },
    
    // 执行时机:当前指令所在DOM对象被渲染到了界面上
    // 应用场景:凡是需要调用DOM对象的方法,需要使用JS控制DOM元素的动态效果都在这个函数中书写代码
    inserted(el,binding){},
    
    // 执行时机:指令值发生变化(同时使DOM重新渲染)时触发该函数
    // 应用场景:需要根据指令值的不同重新渲染指令效果在这个函数中书写代码
    update(el,binding){}
})
```



#### 私有指令

```js
<input type='text' v-color='color' />
new Vue({
    el:'#app',
    data:{
        color:'red'
    },
    methods:{},
    filters:{},
    directives:{
        'color':{
            bind(el,binding){
                el.style.color = binding.value
            },
            inserted(){
                 el.style.color = binding.value
            },
            updated(){
                el.style.color = binding.value
            }
        }
    }
})
```



#### 指令的简写

> 如果自定义指令的业务逻辑代码只需要书写到bind和updated函数中,和其他指令钩子函数无关，则指令名后面的对象可以简写为函数

```js
new Vue({
    el:'#app',
    data:{
        color:'red'
    },
    methods:{},
    filters:{},
    directives:{
        // 指令的简写
        color(el,binding){
            el.style.color = binding.value
        }
    }
})
```



### Vue实例生命周期函数

```js
new Vue({
    el:'#app',
    methods:{},
    
    // 创建阶段
    
    // vue实例被创建，此时实例上面没有data中的属性和methods中的额方法,但是有生命周期函数   
    beforeCreate(){},
    // 执行时机:vue实例创建完毕后,methods中的方法和data中的属性被挂载完毕会执行created
    // 应用场景:可以在这个函数中做数据的初始化,发送异步请求
    created(){},
    
    // 数据已经全部初始化完毕,Vue的指令在内存中已经挂载好，但是还没有被解析渲染到界面中(模板还没有被编译解析)
    beforeMount(){},
    // 执行时机:数据和模板都已经被解析渲染到界面中,真实DOM和内存中的虚拟DOM保持一致
	// 应用场景:如果需要操作真实DOM，必须在这个函数中写代码(百度地图、MUI这些插件的初始化必须在这里写)
    mounted(){},
    
    // 运行阶段
    
    // data中的数据被改变时会触发beforeUpdate(前提是这个数据在页面中有被使用)
    // data里面的数据被改变,但是页面中还是以前的旧数据(模型数据被改变,视图还没有被同步更新)
	beforeUpdate(){},
    // 执行时机:data里面的数据被改变,页面中也被同步更新渲染
    // 应用场景:可以监听到data中的数据变化,然后再处理其他业务
    updated(){},
    
    // 销毁阶段
    // 执行时机: 接收销毁的命令，在data里面的属性和methods中的方法被回收之前触发
    // 应用场景: 为了防止内存泄露,可以做 类似于清除定时器、释放全局变量、释放闭包缓存数据这样的一些工作
    beforeDestroy(){},
    // vue实例身上除开生命周期之外的所有属性都已经被销毁执行destroyed
    destroyed(){}
})
```



### axios发送异步请求

#### axios发送get请求

```js
// 查询字符串传参 ?id=123    http://www.lovegf.cn:8899/api/getlunbo?id=123
// params传参 /:id  http://www.lovegf.cn:8899/api/getlunbo/123

axios.get('http://www.lovegf.cn:8899/api/getlunbo').then(res=>{
    // res.data就是后端返回的数据
})
```

#### axios发送post请求

```js
axios.post('http://www.lovegf.cn:8899/api/addproduct',{
    name:'宝马'
}).then(res=>{
    // res.data是后端返回的响应数据
})
```



#### axios配置全局根域名

```js
axios.defaults.baseURL = 'http://www.lovegf.cn:8899/api'

axios.post('/addproduct',{
    name:'宝马'
}).then(res=>{
    // res.data是后端返回的响应数据
})
```



### 过渡动画

> 1. 过渡动画只能使用在`v-if/v-show/v-for/动态组件(component)/router-view`控制的元素上面
>
> 2. 只有元素能从显示到隐藏或者从隐藏到显示这样的状态切换,过渡动画才能生效。
>
> 3. 过渡动画有三种状态:动画开始之前、动画持续中、动画结束

> 只要需要某个元素实现过渡效果,则必须使用`transition`标签包裹这个元素。
>
> 默认动画前缀是`v`，可以通过在`transition`标签上面添加`name='my'`进行修改。

```html
<transition name='my'>
      <h3 v-if="flag">这是一个H3</h3>
</transition>
```



#### 动画类名实现

```js
// 元素从隐藏到显示

// 控制元素显示之前的状态(样式、位移信息)
.v-enter {
    
}
// 控制元素显示过程中实现的具体动画效果
.v-enter-active {
    transition: all 1.5s ease;
}
// 控制元素完全显示之后的状态(样式、位移信息)
.v-enter-to {
    
}

// 元素从显示到隐藏

// 控制元素隐藏之前的状态(样式、位移信息)
.v-leave {
    
}
// 控制元素隐藏过程中实现的具体动画效果
.v-leave-active {
    
}
// 控制元素完全隐藏之后的状态(样式、位移信息)
.v-leave-to {
    
}
```



#### 结合animate.css实现过渡动画

```html
<link rel="stylesheet" href="./lib/animate.css">

/* enter-active-class：控制元素隐藏到显示的动画效果*/
/* leave-active-class：控制元素显示到隐藏的动画效果*/
<transtion enter-active-class='animated bounceIn' leave-active-class='animated bounceOut' >
    <div></div>
</transtion>
```



#### 动画钩子函数

```html
<transtion
   @before-enter='beforeEnter'
   @enter='enter'
   @after-enter='afterEnter'
   
   @before-leave='beforeLeave'
   @leave='leave'
   @after-leave='afterLeave'
>
    <div></div>
</transtion>
```

```js
new Vue({
    el:'#app',
    methods:{
        
        // 进入动画 开始执行之前会执行beforeEnter
        // 通常用于设置元素的初始状态(样式、位移信息)
        beforeEnter(){},
        
        // 进入动画 执行过程中会执行enter
        // 通常用于设置元素的动画效果以及动画结束时的状态
        enter(el,done){
            // 1. 调用el.offsetWidth刷新动画效果的显示(触发浏览器的重绘)
            el.offsetWidth
            
            // 2. 设置动画元素的结束状态
            
            // 3. 设置元素的动画效果
            
            // 4. 调用done函数立即结束动画(done就是afterEnter的引用)
            done()
        },
        // 进入动画 执行完毕会执行afterEnter
        // 如果希望实现的是半场动画,在这个函数中从新将元素隐藏
        // 如果实现的是全场动画,这个函数可以不写或者也可以添加一些进入动画结束状态
        afterEnter(){},
        
        // 设置离开动画的初始状态
        beforeLeave(){}，
        
        // 用于设置元素的动画效果以及动画结束时的状态
        leave(){},
        
        // 设置离开动画结束之后的一些状态
        // 如果实现半场动画,可以在这个函数中重新将元素显示出来
        afterLeave(){}
 
    }
})
```



#### 列表动画

> 列表动画可以使用过度类名、animate.css、钩子函数去实现。唯一需要改动的就是包裹元素的标签换成`transition-group`
>
> `appear`：用于设置默认数据展示动画效果
>
> `tag`：`tag`属性用于设置`transition-group`被渲染成什么元素，不设置默认被渲染成`span`

```html
<transition-group tag='ul' appear>
    <li v-for="(item, i) in list" :key="item.id" @click="del(i)">
      {{item.id}} --- {{item.name}}
    </li>
</transition-group>
```

```css
/*实现元素在交替切换时的渐变效果:一个元素消失时,其他元素顶替其位置时的动画效果*/
.v-move {
  transition: all 0.6s ease;
}
.v-leave-active{
  position: absolute;
}
```



### 组件

> 模块化:从**业务逻辑**的角度对代码进行封装,方便业务逻辑的重用。
>
> 组件化:从**UI视图**的角度对代码进行封装,方便界面的重用。



> 组件创建时注意点:
>
> 1. 创建组件的代码必须写在vue实例创建之前(Vue构造函数调用的静态方法都必须写在vue实例创建之前)
> 2. 如果组件名采用的是驼峰命名，在使用组件时,需要将大写字母改成小写字母,然后在驼峰之间加上连接符`-`
> 3. 组件模板 `template`标签内部必须使用唯一的一个根元素包裹结构代码。 `Component template should contain exactly one root element`
> 4. `template`标签需要写到vue管理区域外面



#### 创建组件的三种方式

- `Vue.extend()`创建

```js
// 1. 创建组件的模板对象
var login = Vue.extend({
	template:'<h2>登录</h2>'
})
// 2. 将组件模板对象注册成为组件
Vue.component('mylogin',login)
```

- `Vue.component()`对象字面量创建

```js
Vue.component('mylogin',{
    template:'<h2>登录</h2>'
})
```

- `template`标签定义模板结构

```js
// 1. 注册组件
Vue.component('mylogin',{
    template:'#mylogin'
})

// 2. 在HTML结构部分(Vue管理区域之外)定义组件模板
<template>
	<div>
   		<h2>登录</h2> 	
    </div>    
</template>
```

#### 组件中的data写法

```js
Vue.component('home',{
    template:'#home',
    data(){
        return {
            msg:'hello'
        }
    }
})
```



> 组件中的data如果是对象,则会出现 某一处修改了组件的数据,其他使用该组件的地方数据也会被修改,其原因在于对象是引用数据类型
>
> 组件data写成函数中return {},作用就是每个使用组件的地方都会调用该函数返回一个全新的对象,每个对象互不相关,修改数据不会互相影响



#### component结合is属性实现组件切换

> 组件切换使用场景:同一个区域需要展示不同的组件(例如选项卡的切换)

```js
// 1. 在页面中需要展示内容的区域书写component组件，绑定is属性,is属性的值为组件的名称
<component :is='comName'></component>

// 2. 在vue实例内部定comName,只要切换comName的值，则component位置就会显示对应的组件

// 本质上 component 就是一个占位符,使用is属性规定该占位符需要展示什么内容
```



#### 组件传值

##### 父组件向子组件传值

> 1. 在父组件中使用子组件时,绑定一个自定义的属性`:sonmsg='msg'`,属性值就是需要传递的数据
> 2. 在子组件的props属性内部,定义一个用于接收父组件传过来值的变量，这个变量名字应该和绑定到组件身上的自定义属性同名
> 3. 在子组件模板区域使用props里面定义的属性展示父组件传过来的数据

> props传值时单向数据流动，父组件传过来的值在子组件中不能被修改。

```js
Vue.component('home',{  
    template:'#home',
    // 2. 子组件内部定义接收传参的属性
    props:['sonmsg']
})

let vm = new Vue({
    el:'#app',
    data:{
      name:'zxx'
    }
})
```

```html
<template id="home">
    <div>
      <h3>首页</h3>
      <!-- 3. 在子组件模板区域使用父组件传过来的数据 -->
      <p>{{this.sonmsg}}</p>
    </div>
</template>

<div id="app">
    <!-- 1. 在父组件中使用子组件时,绑定一个自定义的属性,属性值就是需要传递的数据 -->
    <home :sonmsg='name'></home>
</div>
```

- 可以通过`this.$parent`获取到父组件实例,通过实例对象访问父组件中数据

#### 子组件向父组件传值

> **子向父传值的本质:子组件调用父组件中的函数,进行数据的传递。**

> 1. 在父组件中使用子组件时，绑定一个自定义事件`@fn='getsonmsg'`,`getsonmsg`作为事件处理函数需要在父组件的`methods`中进行定义
> 2. 在子组件中通过`this.$emit('fn')`的方式调用父组件中的`getsonmsg`,在调用过程中，`this.$emit('fn')`第二个参数开始,就可以向父组件传递数据。例如`this.$emit('fn',123)`
> 3. 父组件中的`getsonmsg`里面定义形参接收数据

```js
Vue.component('home',{
    template:'#home',
    data(){
      return {
        sonmsg:'子组件的数据'
      }
    },
    created() {
      // @fn='getsonmsg' 事件名fn 事件处理函数getsonmsg
      // 参数一: 自定义的事件名称
      // 参数二: 需要传递到事件处理函数里面的参数
      this.$emit('fn',this.sonmsg)
    },
  })
  let vm = new Vue({
    el:'#app',
    methods: {
      getsonmsg:function(data){
          console.log('getsonmsg',data)
      }
    },
  })
```

- 可以在子组件中直接调用`this.$parent.getsonmsg(123)`将数据传递到父组件

### 路由

> 后端路由:监听网络请求,根据请求处理相关业务逻辑，将数据响应到客户端
>
> 前端路由:监听浏览器hash值的变化，根据hash值的切换展示不同的组件。使用前端路由做的项目都是单页应用程序(SPA)

#### 路由基本使用

```js
// 1. 定义需要通过路由展示的组件
var login = {
    template:'#login'
}
var register = {
    template:'#register'
}

// 2. 创建路由实例对象
var router = new VueRouter({
    // 2.1 定义路由规则数组
    routes:[
        {
            path:'/login',
            component:login
        },
        {
            path:'/register',
            component:register
        }
    ]
})

new Vue({
  el:'#app',
  // 3. 将创建的路由实例挂载到vue实例上面(可以保证在所有的vue实例内部可以访问this.$route和this.$router属性)
  // router:router
  router
})
```

```html
/* 1. 创建路由的跳转链接 */
<router-link to='/login'>登录</router-link>
<router-link to='/register'>注册</router-link>

/* 2. 指定组件需要展示的区域 */
<router-view></router-view>
```



#### 路由传参

```js
// query传参(查询字符串)
// 1. 在router-link跳转时直接使用查询字符串的方式拼接参数 /login?username=admin&password=123456
// 2. 在跳转到的组件中使用this.$route.query获取参数

// params传参(restful API)
// 1. 将路由规则修改为带有参数的格式，参数以:id这种形式进行修饰 /register/:username/:password
// 2. 在跳转时使用/register/admin/123456的方式传参
// 3. 在组件中使用this.$route.params获取参数
```



#### 嵌套路由

> 在一个页面使用路由切换之后，内部有公共区域不再需要被路由替换，那么当前这个页面需要被路由展示内容就必须使用嵌套路由进行展示
>
> **注意:子路由对应的组件必须展示在父路由对应组件的模板中，父路由对应组件模板里面必须放一个`router-view`进行占位**

```js
var router = new VueRouter({
    // 2.1 定义路由规则数组
    routes:[
        {
            path:'/login',
            componnet:login
        },
        {
            path:'/index',
            component:index,
            // index路由的子路由规则
            children:[
                {
                    path:'users', // 访问子路由需要使用 /index/users 这个地址访问
                    component:users
                },
                {
                    path:'/news', // 访问子路由需要使用 /news
                    component:news
                }
            ]
        }
    ]
})

new Vue({
    el:'#app'
})
```



```html
<template id='index'>
    <div>
        <!--这里的router-view用于展示子路由对应的组件-->
        <router-view></router-view>
    </div>
</template>
<div id='app'>
    <!--这里的router-view用于展示一级路由(父路由)对应的组件-->
    <router-view></router-view>
</div>
```



#### 路由重定向&路由高亮&命名视图

```js
var router = new VueRouter({
    linkActiveClass:'myactive',// 默认值是 router-link-active,该属性用于覆盖默认值
    routes:[
        {
            path:'/',
            redirect:'/index' // 路由重定向
        },
        {
            path:'/login',
            component:login
        },
        {
            path:'/index',
            component:index
        },
        // 命名视图
        {
            path:'/home',
            components:{
                'default':welcome,
         		'menu':menu,
                'content':content
            }
        }
    ]
})
```



```html
<div id='app'>
    <router-view></router-view> <!--这个占位会显示welcome组件-->
    <router-view name='menu'></router-view>  <!--这个占位会显示menu组件-->
    <router-view name='content'></router-view>  <!--这个占位会显示content组件-->
</div>
```



### watch

> **侦听器**:可以监听vue实例身上的所有属性,根据属性变化做出相应的逻辑处理

```js
new Vue({
    el:'#app',
    data:{
        msg:'hello',
        user:{
            name:'zxx',
            age:18
        }
    },
    watch:{
        // 监听简单数据类型
        msg(newvalue,oldvalue){
           // newvalue 新值
           // oldvalue 旧值
        },
        
        // 监听复杂数据类型(深度监听)
        user:{
            handler(value){
                // value就是变化之后的新对象
            },
            deep:true
        }
    }
})
```

### computed

> **计算属性**:用于定义可以依赖其他数据(data、props中的数据)变化而变化的数据
>
> 特点:
>
> 1. 计算属性依赖的数据发生变化,计算属性会重新求值
> 2. 计算属性的值会缓存,如果多个地方使用计算属性,那么优先从缓存中取值。

```js
new Vue({
    el:'#app',
    data:{
        msg:'hello'
    },
    // 计算属性:commsg会随着msg值的改变而发生变化
    computed:{
        commsg:function(){
            return this.msg + 'word'
        }
    }
})
```





### webpack

> webpack是什么:webpack是一个基于Node的前端项目构建工具。
>
> **构建**：将高级语法转换成低级语法,将浏览器不能识别的模块转换成浏览器可以直接使用的模块，最后将所有的处理好的资源进行合并压缩。
>
> webpack有什么用处:
>
> > 1. 将所有静态资源进行合并压缩。
> > 2. 使用前端模块化处理项目中的文件依赖关系。
> > 3. 将高级JS语法转换成为低级JS语法。



#### webpack的安装

- cnpm 安装

```js
// 1.安装cnpm
npm install -g cnpm --registry=https://registry.npm.taobao.org

// 2. 使用cnpm安装webpack/webpack-cli
cnpm i webpack webpack-cli -g

// 3. 检查版本号
webpack -v
```

- yarn 安装

```js
// 1. 使用cnpm安装yarn
cnpm i yarn -g

// 2. 安装全局包webpack/webpack-cli
yarn global add webpack webpack-cli

/*
	将yarn安装包的路径配置到环境变量
	将这个路径拷贝C:\Users\LEO\AppData\Local\Yarn\bin添加到环境变量的path中
	
	1. 按win+r输入sysdm.cpl
	2. 选择高级-环境变量
	3. 将yarn的bin目录添加到系统变量的path中
*/

// 3. 检查版本号
webpack -v

```

#### webpack的基本使用

```js
// 1. 新建文件夹，在文件夹中初始化package.json文件
yarn init -y

// 2. 在项目文件夹中新建src、dist子文件夹、webpack.config.js文件

// 3. 进入到src目录,新建index.html文件、index.js文件

// 4. 在webpack.config.js中写入以下内容
module.exports = {
    mode:'development'
}

// 5. 在index.js中书写代码,通过执行webpack这个命令进行编译,会输出到dist下的main.js里面
// index.html 中引入main.js
// 编译命令:webpack(必须保证在当前项目根目录下面)
```

#### webpack-dev-server的使用

> webpack-dev-server可以开启一个基于express的服务器
>
> webpack-dev-server可以将当前项目进行托管,托管的根目录就是项目的根目。托管之后可以监听每个文件的变化,当文件发生变化,会自动执行打包编译的命令。

```js
// 1. 在项目中安装webpack-dev-serv webpack webpack-cli
yarn add webpack-dev-server webapck webpack-cli --dev或者cnpm i webpack-dev-server webpack webpack-cli -D

// 2. 在package.json文件中书写以下配置
"scripts": {
	"dev":"webpack-dev-server --open --port 8888 --hot --inline --contentBase src"
}
// open 自动打开浏览器
// port 指定端口号
// hot 热重载
// inline 自动刷新浏览器
// contentBase 指定静态资源托管目录


// 3. 在项目根目录运行npm相关命令执行打包
npm run dev

注意:需要修改index.html文件中js文件的导入
<script src="/main.js"></script>
```

#### html-webpack-plugin

> 1. 根据指定的模板文件生成一个内存中的html文件
> 2. 在生成好的html文件里面自动引入打包好的main.js文件

```js
// 1. 安装html-webpack-plugin
yarn add html-webpack-plugin --dev

// 2. 在webpack.config.js中书写插件配置
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  mode:'development',
  plugins:[
    new HtmlWebpackPlugin({
      // 模板文件
      template:path.join(__dirname,'./src/index.html'),
      // 生成在内存中的文件名称,一般都必须写index.html
      filename:'index.html'
    })
  ]
}

注意: 使用html-webpack-plugin这个插件之后,需要删除调用模板index.html里面的js文件导入
```

#### style-loader&css-loader的使用

> 解析后缀为.css的文件,将css样式以内联样式的方式插入到html页面中

```js
// 1. 安装style-loader css-loader
yarn add style-loader css-loader --dev

// 2. 在webpack.config.js中书写加载器配置
module.exports = {
    module:{
        rules:[
            {
                test:/\.css$/,
                use:['style-loader','css-loader']
            }
        ]
    }
}
```

#### less-loader的使用

> 解析后缀为.less文件

```js
// 1. 安装解析less文件的加载器
yarn add less-loader less --dev

// 2. 在webpack.config.js中书写加载器配置
rules:[
        {
            test:/\.less$/,
            use:['style-loader','css-loader','less-loader']
        }
]
```



#### sass-loader的使用

> 解析后缀为.scss的文件

```js
// 1. 安装解析less文件的加载器
yarn add sass-loader node-sass --dev

// 如果node-sass下载不了,使用cnpm尝试下载 cnpm i node-sass -D

// 2. 在webpack.config.js中书写加载器配置
rules:[
        {
            test:/\.scss$/,
            use:['style-loader','css-loader','sass-loader']
        }
]
```



#### url-loader的使用

> 解析图片、字体图标、音视频等静态文件

```js
// 1. 安装解析文件的加载器
yarn add url-loader file-loader --dev

// 2. 在webpack.config.js中书写配置规则
rules:[
    	// 解析图片
        {
            test:/\.(jpg|png|gif|bmp|jpeg)$/,
            use:'url-loader?limit=5000&name=[hash:8]-[name].[ext]'
        },
   		// 解析字体图标
        {
			test:/\.(ttf|eot|woff|woff2|svg)$/,
            use:'url-loader?limit=2048&name=[hash:8]-[name].[ext]'
        }
]

// 如果资源被打包成路径 内部使用的是url-loader
// 如果资源被打包成base64字符串 内部会使用file-loader

// limit单位是字节 1kb = 1024字节
// 被打包的图片小于limit值,会被转换成base64字符串内嵌到main.js中
// 被打包的图片大于等于limit值,会被打包成一个路径,在访问图片时会单独发送一个请求获取
```

#### babel-loader的使用

> 解析高级JS语法

```js
// 1. 安装相关的包-直接装以下包
yarn add babel-loader --dev
yarn add @babel/core --dev
yarn add @babel/plugin-transform-runtime --dev
yarn add @babel/plugin-proposal-class-properties --dev
yarn add @babel/preset-env --dev
yarn add @babel/runtime --dev

react中需要多安装 yarn add @babel/preset-react --dev
// 2. 在.babelrc文件中书写以下代码
{
  "plugins": [
    "@babel/plugin-transform-runtime",
    "@babel/plugin-proposal-class-properties"
  ],
  "presets": [
    "@babel/preset-env"
	react中多配置 "@babel/react"
    
  ]
}

// 3. 在webpack配置中书写以下代码
{
  test:/\.js$/,
  use:'babel-loader',
  exclude:/node_modules/ // 排除掉node_modules不需要被babel解析
}
```



#### Vue-loader的使用

```js
// 1. 安装解析vue相关的包
yarn add vue
yarn add vue-loader vue-template-compiler --dev

// 2. 在webpack.config.js中书写配置
rules:[
        {
            test:/\.vue$/,
            use:'vue-loader'
        }
]

// 3. 导入vue-loader中的插件VueLoaderPlugin
const { VueLoaderPlugin } = require('vue-loader')
plugins:[
    new VueLoaderPlugin()
]
```



#### import&export的使用

```js
// CommonJS规范
// module.exports === exports === {}

// 完整导出
module.exports = {}

const obj = require('./file.js')

// 按需导出
exports.fn = function(){}

const obj = require('./file.js')
const { fn } = require('./file.js')
fn()


// ES6标准模块化规范
// 完整导出
export default {}
import obj from './file.js'

// 按需导出
export const username = 'zxx'
export const fn = function(){}

import { username as name, fn } from './file.js'
console.log(name)
fn()
```



#### vue-cli2.0

```js
// 1. 全局安装vue-cli
yarn global add vue-cli

// 2. 初始化基于webpack的项目
vue init webpack vue-project

// 3. 进入项目运行
npm run dev

/*
注意:
1. 修改端口号和自动打开浏览器:/config/index.js 16、17行
2. 如果项目中需要使用less、scss语法,需要自己单独装包,但是不需要写配置
3. 删除默认的路由配置、删除HelloWord组件
*/
```

![1551782300754](C:\Users\LEO\AppData\Roaming\Typora\typora-user-images\1551782300754.png)



#### vue-cli3.0

```js
// 安装
yarn global add @vue/cli

// 创建项目
vue create vue-project
vue ui 可视化界面配置项目

eslint相关的内容将来在实际项目中建议启用
```



#### 将项目推送到码云

```js
// 1. 在码云上创建一个空项目(黑色的窗口中有些命令)

// 2. 在本地使用vue-cli创建的项目目录中提交代码
git add .
git commit -m '项目初始化'

// 3. 将本地仓库和远程仓库关联
git remote add origin https://gitee.com/UniverseKing/my-vue-project.git

// 4. 将本地仓库中的代码提交到远程仓库
git push -u origin master

```



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



### 常用命令

```js
/*
cd 切换目录
ls 查看当前目录文件
mkdir 新建文件夹
touch 新建文件
*/
```



---


## Vue面试题

### v-if 和 v-show的区别?

> v-if和v-show都是用于控制元素的显示和隐藏
> v-if是使用新增或删除DOM元素实现元素的显示和隐藏效果(重排)
> v-if使用场景:默认不显示，后期跟进条件显示，而且不涉及频繁切换隐藏显示
> v-show是使用样式(display:block/none)实现元素的显示和隐藏效果(重绘)
> v-show使用场景:频繁切换显示隐藏效果

###  v-cloak的作用?

> 解决插值表达式的闪烁问题
> Vue没有加载时，浏览器解析DOM,此时不能识别v-cloak指令，只会当做一个普通的属性解析，属性选择器就能生效，元素会被隐藏。
> Vue被加载完毕，此时是Vue实例解析DOM，会识别到v-cloak指令，同时将识别的v-cloak指令移除掉，此时属性选择器的样式就不在生效，插值表达式的内容也就显示出来了。

```html
[v-cloak]{
  display:none
}

<p v-cloak>{{msg}}</p>
```

### Vue生命周期函数的理解

> 1. 解释什么是生命周期函数：Vue框架内部提前内置的一批函数,这些函数会在特定时机自动执行。
> 2. 有哪些生命周期函数：创建阶段4个、运行阶段2个、销毁阶段2个
> 3. 生命周期函数的用途：created、mounted、updated、beforeDestroy的执行时机和应用场景



### 描述vue中的单向数据流

> vue中的单向数据流指的是父组件向子组件传递数据，只能单向绑定。
>
> 子组件需要使用props属性进行接收传过来的数据，父组件传递过来的数据在子组件中是只读的，不能对其进行修改。
>
> 如果修改了父组件传过来的数据，会报错。
>
> **这种自上向下的数据传输方式叫做单向数据流。**


### Vue中组件传值方式

```js
// 组件传值
// 父向子传值
// 1. 属性绑定:props接收数据(单向数据流动思想)
// 2. this.$parent获取父组件实例对象,通过实例访问属性

// 子向父传值
// 1. 事件绑定: @fn='func' this.$emit('fn',123)
// 2. 子组件中调用this.$parent获取父组件实例对象,通过调用实例上的方法进行数据传递
// 3. 父组件中调用this.$refs.xxx获取到的是子组件实例对象，通过子组件实例对象获取子组件的数据

// 兄弟组件传值(event bus事件总线)
```
### computed/data/props的区别?
> 数据的来源不同。
>
> computed数据的来源是依赖函数的返回值。(computed定义计算属性的函数中一般会依赖data或者props中的数据，被依赖的数据发生变化，该计算属性的函数会被重新调用,返回新值)
>
> data数据的来源是定义时赋值或者在生命周期函数中赋初始值。
>
> props数据的来源是通过父组件传递过来的。


### 双向数据绑定实现原理



##  跨域

#### JSONP

#### CORS

#### 代理

## 反馈问题

- [ ] 安装包的时候哪些安装在开发环境,哪些安装在线上环境

```js
-D/--save-dev 开发环境  会将包名版本号记录到devDependencies节点
-S/--save 线上环境  会将包名版本号记录到dependencies节点

凡是组件库、插件这些肯定都是-S
和webpack相关的一般都是-D

不用区分,对于项目运行不管使用-S还是-D都没有任何影响
```

- [ ] Vue数据污染,浅复制Vue.prototype.$pureDate
- [ ] 正则

```js
/abc/.test('abcd')
```

- [ ] this指向

```js
上下文: 环境
this所处的环境不一样,代表的内容不一样

函数4中调用模式:
1. 函数调用

function fn(){
    this.name = 'zs'
    console.log(this) // window
}

fn()

2. 方法调用(指向调用者)

var obj = {
    fn:fn
}

obj.fn() // 点语法
obj['fn']() // 关联数组语法

var list = [1,2,3,fn]
list[3]()

3. 构造器调用(指向创建的实例对象)

new fn()

4. 上下文调用
// call apply bind
fn.call(window) === fn()
fn.call(obj) === obj.fn()
fn.call(123) === Number(123).fn()
// call、apply第一个参数如果传入基本数据类型,会将这个数据转换成包装类型
// (基本数据类似在调用方法时也会被自动转换成包装类型)
str.split()


// 大多数时候回调函数最终都会被使用 函数调用
axios.get().then(function(){
    console.log(this) // window或者undefined
})

this.$http.get().then(function(){
    console.log(this)
})

// callback&&callback() 正常情况回调函数调用方式
// callback&&callback.call() // 特殊情况回调函数调用方式
```



- [ ] .then中为什么不能使用return返回数据

  ```js
  this.$http.get().then(function(){
      console.log(this)
      return 123
  }).then(function(res){
      res ==== 123
  })
  ```

- [ ] js操作数组的常用方法

- [ ] 复习的方式

```js
JS高级面向对象
移动端的布局(rem/媒体查询/flex/h5c3)
服务器的相关概念(打开一个网页会经历的过程) http协议/DNS/三次握手/四次挥手
Vue相关(指令、传值、生命周期、路由、vuex)

```

- [ ] promise
- [ ] gulp、grunt
- [ ] 离线存储、缓存
- [ ] 地图定位、第三方支付、第三方登录、文件上传

```js
<form action='' method='post'>
    <input type='file' />
</form>
```

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



## rem布局

```js
css单位全部使用rem

UI图纸: 750/640
图片: 300px * 200px

代码里面: 1rem = 75px
width:300px;// 300/75rem 
height:200px; // 200/75rem
```

#### vue-cli3.0 flexible配置

```js
// 1. 安装lib-flexible
yarn add lib-flexible
// 2. main.js中导入
import 'lib-flexible/flexible.js'
// 3. 安装自动将px转换为rem的插件
yarn add postcss-pxtorem
// 4. 配置pxtorem的基准单位(在package.json或者postcss.config.js中书写配置信息)
plugins: {
    autoprefixer: {},
    "postcss-pxtorem": {
      "rootValue": 75, // 设计稿宽度的1/10
      "propList": ["*"] // 需要做转化处理的属性，如`hight`、`width`、`margin`等，`*`表示全部
    }
}
// 5. 根据UI设计图纸上面标注的宽高大小进布局
// 例如标注 某张图片 宽300px,高200px
img {
    width:150px;
    height:150px;
}

// pxtorem会自动转换
img {
    width: 2rem;
    height: 2rem
}

// 注意事项: 引入flexible.js后需要删除 默认带有viewport 的 meta 标签。
// 如果不删除,所有设备的dpr都会是1.

```



## vue后台管理系统

> vue-cli3.0 + vue-router + axios +  element-ui

```js
vue ui
vue create vue-oa
```















​                                 


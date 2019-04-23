# vue的路由守卫

```js
//映入vue中的路由包
import VueRouter from 'vue-router'
//为下面的引入路由添加一个路径
import login from '../views/login.vue'
//挂在路由路径
const router = new VueRouter({ 
  routes: [{
     path: '/login',  name: 'Login',  component: login
  }]
})
//全局前置路由守卫
router.beforeEach((to, from, next) => {
  	//当使用了全局前置路由守卫的时候不写next(), 请求是不会发送的 所有当书写了全局前置守卫的时候当注意next的使用
 	 //to: ROute: 即将要进入的目标 路由对象
	//from： Route： 当前要离开的路由
	//next： Function: 一定要调用该方法来selove这个钩子，执行效果依赖next方法的调用函数
  	//next(): 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 confirmed （确认的）。
  	//next(false): 中断当前的导航。如果浏览器的 URL 改变了（可能是用户手动或者浏览器后退按钮），那么 URL 地址会重置到 from 路由对应的地址。
  	//next('/') 或者 next({ path: '/' }): 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。
})
  //全局后置守卫 
router.afterEach((to,from)=>{
  //o：进入到哪个路由去，from：从哪个路由离。
})

//组件内的守卫
//进入组件是
beforeRouteEnter:(to,from,next)=>{
  //注意这里的next和全局的使用方法不同 
  //这里的next()会给一个对应的回调 如下
  next(vm=>{
           return vm.数据名
        })
}
//离开组件，
beforeRouteLeave:(to,from,next)=>{
  //这里的next就是确定的时候调用
}
//路由独享的守卫 
beforeEnter:(to,from,next)=>{
  
}
```


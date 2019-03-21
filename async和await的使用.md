# async和await的使用

这是基于promise再次封装的语法

async用来表示函数是异步的顶一个函数会返回一个promis对象，可以使用.then方法

await必须出现在函数内部，不能单独使用

官方解释：await 后面可以跟任何的JS 表达式。虽然说 await 可以等很多类型的东西，**但是它最主要的意图是用来等待 Promise 对象的状态被 resolved**。如果await的是 promise对象会**造成异步函数停止执行并且等待 promise 的解决**,如果等的是正常的表达式则立即执行

简单来书就是等待异步操作执行完毕，异步为完成不能执行await一下的代码

```
// 使用 async he await 写出一个延时器
// 手写一个定时器,使用异步，显示await的作用
const sleep = (time) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(3)
        },time)
    })
}
async function xixi() {
    console.log(1)
    let xixi =  await sleep(2000)
    console.log(xixi)
    
}
xixi()
```


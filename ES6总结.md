# ES6总结

## 1.变量

var 重复声明、函数级

let  不能重复声明、块级、变量

const  不能重复声明、块级、变量

## 2.箭头函数

a.方便

​	i.如果只有一个参数，()可以省

​	ii如果只有一个return ，{}也可以省略

b.修正this指向

## 3.参数扩展

收集

```
        function show(a, b, ...arg) {
            console.log(a)
            //1
            console.log(b)
            //2
            console.log(arg)
            //3,4
        }
        show(1,2,3,4)

```

扩展

```
let arr = [1,2,3]
let arr1 = [...arr,...arr]
console.log(arr1)
//[1,2,3,1,2,3]
```

默认参数

## 4.数组方法

map 映射

reduce 汇总 一堆       -->        一个

filter   过滤：  一堆     -->    剩下

forEach   循环

## 5.字符串

字符串模板 ``

## 6.Promise

分装异步操作

Promise.all([])

## 7.generator

function *show() {

yield

}

## 8.JSON

JSON.stringify() json转化为字符

JSON.parse() 字符转换为json

## 9.解构赋值

let [a,b,c] = [12,3,4]

左右结构一样

右边是个合法的东西

声明，赋值一次解决

## 10.面向对象

```
class Text {
	constructor() {
	this.xxx =
}
方法1(){
}
方法2(){
}
}
继承
class Cls2 extends Cls1 {
	constructor() {
	super(...arg)
}
}

```


# class面向对象

class关键字，是ES6中提供的新语法，是用来 实现 ES6中面向对象编程的方式

```
可以使用Point直接创建构造函数
class Point {
  constructor(x, y) {
  },
}
定义在point中的方法其实都是定义在Point.prototype上的
class Point {
  constructor() {
  }
  toString() { 
  }
}
等价于 Point.prototype = {
  constructor() {}
  toString() {}
}
在类的实例上调用的方法，其实就是在原型上调用方法
b.constructor === B.prototype.constructor
```



	
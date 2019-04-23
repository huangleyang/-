## ES6语法

### let和const

- let不存在预解析,必须先定义后使用

  ```javascript
  console.log(a); // ReferenceError: a is not defined. 引用错误: a 未定义
  
  let a = 10;
  ```

- let在同一个代码块中,申明的变量不允许重复

  ```js
  let a = 10;
  
  let a = 20; // SyntaxError: Identifier 'a' has already been declared. 语法错误: 标识符 a 已经被申明
  ```

- let在代码块内部申明的变量,代码块外部不允许访问(存在块级作用域)

  ```javascript
  - if(true){
  
      let a = 10;
  
    }
  
    console.log(a); // ReferenceError: a is not defined. 引用错误: a 未定义
  
    {
  
       let a = 10;
  
    }
  
    console.log(a); // ReferenceError: a is not defined. 引用错误: a 未定义
  
  - const用于申明常量,申明过后不允许再次赋值
  
    const a = 10;
  
    a = 20; // TypeError: Assignment to constant variable. 类型错误:给常量进行分配
  
  - const申明时就必须赋值
  
    const a;// SyntaxError: Missing initializer in const declaration. 语法错误: 定义常量时没有进行初始化
  ```


### 解构赋值

> ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解(Destructuring)

#### 数组的结构赋值

```javascript
// 解构

let [a,b,c] = [1,2,3];

console.log(a,b,c); // 1 2 3

// 解构时设置默认值

let [a = 1,b,c] = [,5,6];

console.log(a,b,c); // 1 5 6

// 解构时没有的数据为undefined

let [a = 1,b,c] = [4,5,];

console.log(a,b,c); // 4 5 undefined
```



#### 对象的解构赋值

```javascript
let {name,age} = {name:'zxx',age:18}

console.log(name,age); // zxx 18


// 顺序无关

let {name,age} = {age:18,name:'zxx'}

console.log(name,age); // zxx 18


// 属性别名,一旦设置了别名,原来的名字就无效了

let {name:tag,age} = {age:18,name:'zxx'}

console.log(name,age); // ReferenceError: name is not defined

console.log(tag,age); // zxx 18


// 解构赋值时设置默认值

let {name:tag='zxx',age} = {age:18}

console.log(tag,age); // zxx 18
```



#### 字符串的解构赋值

```javascript
let [a,b,c,d] = 'zxx';

console.log(a,b,c,d); // z x x undefined

let [a,b,c,d] = 'zxx';

console.log(a,b); z x

// 得到字符串长度

let { length } = 'zxx';

console.log(length); // 3
```



### 字符串扩展

- `includes(substring [, position])`:判断字符串中是否包含子字符串,第一个参数表示要测试的子字符串,第二个参数表示从那个位置开始查找,不传默认从索引0开始。

  ```javascript
  let str = 'my name is zxx';
  
  console.log(str.includes('zxx')); // true
  
  let str = 'my name is zxx';
  
  console.log(str.includes('zxx',12)); // false
  ```

- `startsWith()`: 判断字符串是否以特定的字符串开始

  ```javascript
  let url = 'http://www.baidu.com](http://www.baidu.com/)';
  
  console.log(url.startsWith('http://')); // true
  
  //  /^http:\/\//.test(url)
  ```

- `endsWith()`: 判断字符串是否以特定的字符串结束

  ```javascript
  let url = '[http://www.baidu.com](http://www.baidu.com/)';
  
  console.log(url.endsWith('.com')); // true
  // /\.com$/.test(url)
  ```

- 字符串填充:`padStart()`和`padEnd()`可以对字符串进行填充

  ```javascript
  let str = 'abc'
  
  str.padStart(5,d) // ddabc
  
  str.padEnd(5,d) // abcdd
  ```

- `repeat()`:复制字符串

  ```javascript
  'x'.repeat(3) // "xxx"
  ```

- `at()`:获取索引位置的字符

  ```javascript
  // 参数传入索引，返回值为角标对应的字符
  
  'abc'.at(0) // 'a'
  
  '曾小仙'.at(0)  // '曾'
  
  // 与ES5中charAt()不同之处，汉字的话charAt会返回对应Unicode编码，js内部用UTF-16
  ```

- 模板字符串:`${变量}`

  ```javascript
  let o = {name:'zxx',age:18};
  
  let str = `我的名字是${o.name},今年${o.age}岁`;
  
  console.log(str); // 我的名字是zxx,今年18岁
  ```

### 函数扩展

#### 参数默认值

```javascript
function func(arg = 10){
    console.log(arg);
}

func(); // 10
func(20); // 20
```

#### 参数解构赋值

```javascript
function func({name,age}){
    console.log(name,age);
}
func(); // ypeError: Cannot match against 'undefined' or 'null'.
func({}); // undefined undefined
func({name:'zxx',age:18}) // zxx 18
```

#### rest 剩余参数

> rest剩余参数只能作为最后一个形参

```javascript
function func(a,...args){
    console.log(a,args);
}
func(1,2,3); // 1 [2,3]
```

#### 扩展运算符

```javascript
function func(a,b){
    console.log(a + b);
}
func(...[2,3]); // 5
```

##### 合并数组

```javascript
let arr1 = [1,2,3];
let arr2 = [4,5,6];
let arr3 = [...arr1,...arr2];
console.log(arr3); // [1,2,3,4,5,6]
```

##### 字符串转数组

```javascript
[...'hello']
// [ "h", "e", "l", "l", "o" ]
```

#### 箭头函数

> 1、函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
>
> 2、不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
>
> 3、不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

- 使用箭头函数简写函数的定义

  ```javascript
  let f = function(v) {
      return v;
  };
  let f = v => v;
  ```

- 如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。

  ```javascript
  let f = () => 5;
  // 等同于
  let f = function () { return 5 };
  
  let sum = (num1, num2) => num1 + num2;
  // 等同于
  let sum = function(num1, num2) {
      return num1 + num2;
  };
  ```

- 如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回。

  ```javascript
  let sum = (num1, num2) => {let num3 = 3; return num1 + num2 + num3;}
  // 等同于
  let sum = function(num1, num2) {
      let num3 = 3;
      return num1 + num2 + num3;
  };
  ```

#### Array.from()

> 将伪数组或者类数组转换成为数组,比如NodeList集合,Set、Map数据结构转换为真正的数组

```javascript
// 将NodeList转换为数组
Array.from(document.getElementsByTagName('div'))

// 将Set集合转换为数组
Array.from(new Set([1,2,3,4,5,6,4,3,2,1,0]))

```

#### Array.of()

> 用于将一组值，转换为数组.

```javascript
Array.of(1,2,3,4,5) // [1,2,3,4,5]
```

#### Array.prototype.fill()

> 用于替换数组的内容，三个参数，后面两个可以省略，第一个参数为替换成什么内容，第二个为替换的起始位置，第三个为替换的终止位置

```javascript
['a', 'b', 'c'].fill(5)
// [5,5,5]

['a', 'b', 'c'].fill(5, 1, 2)
// ['a', 5, 'c']
```

#### entries()，keys() 和 values()

> 迭代器Iterator。`for-of`是一种新的对数组或对象进行遍历的方法，类似for-in.

> 三个方法都是遍历数组，都可以用for...of...唯一的区别是keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历

```javascript
for (let index of ['a', 'b'].keys()) {
    console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
```



### Set和Map结构

#### Set(集合)

> Set结构类似于数组，但是成员的值都是唯一的，没有重复的值。Set 本身是一个构造函数，用来生成Set数据结构。此结构不会添加重复的值。

```javascript
// 例一
const set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]

// 例二
const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
items.size // 5

// 例三 数组去重
// 方法一
[...new Set(array)] // 使用展开运算符
// 方法二
Array.from(new Set(array)) // 使用Array.from将Set转换为数组

```

#### Map(映射)

> Map结构类似于对象，是键值对的集合。
>
> 和Object的区别:Object的键只能是字符串或数值类型,Map数据结构的键可以是JS中的任何数据类型。

```javascript
let map = new Map([['a',123],['b',456]]);   console.log('map args',map);

console.log('size',map.size);

console.log('delete',map.delete('a'),map);

console.log('clear',map.clear(),map);
```

### Proxy(代理)拦截器

> Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。

> `Proxy`类似于`ES5`中 的`Object.define.property()`

```javascript
var obj = new Proxy({}, {

    get: function (target, key, receiver) {
        console.log(`getting ${key}!`);
        return Reflect.get(target, key, receiver);
    },

	set: function (target, key, value, receiver) {
        console.log(`setting ${key}!`);
        return Reflect.set(target, key, value, receiver);
	}
});
```

#### get()方法

> get方法用于拦截某个属性的读取操作，可以接受三个参数，依次为目标对象、属性名和 proxy 实例本身（即this关键字指向的那个对象），其中最后一个参数可选。

```javascript
var person = {
  name: "张三"
};
var proxy = new Proxy(person, {
  get: function(target, property) {
	if (property in target) {
		return target[property];
	} else {
		throw new ReferenceError("Property \"" + property + "\" does not exist.");
	}
  }
});

[proxy.name](http://proxy.name/) // "张三"

proxy.age // 抛出一个错误

```

#### set()方法

> set方法用来拦截某个属性的赋值操作，可以接受四个参数，依次为目标对象、属性名、属性值和 Proxy 实例本身，其中最后一个参数可选。

```javascript
let validator = {
  set: function(obj, prop, value) {
		if (prop === 'age') {
            if (!Number.isInteger(value)) {
                throw new TypeError('The age is not an integer');
            }
		if (value > 200) {
			throw new RangeError('The age seems invalid');
		}
	}
	// 对于满足条件的 age 属性以及其他属性，直接保存
	obj[prop] = value;
  }
};

let person = new Proxy({}, validator);

person.age = 100;
person.age // 100
person.age = 'young' // 报错
person.age = 300 // 报错
```



### Symbol类型

> ES6 引入新的原始数据类型Symbol，表示独一无二的值
>
> JS有多少种数据类型:String、Number、Boolean、Object、Array、null、undefined、Symbol

```javascript
//定义一个symbol

let str1 = Symbol();

let str2 = Symbol();

console.log(str1 === str2); //false

console.log(typeof(str1)); //Symbol


//symbol主要用于对象属性的定义，且定义的属性是唯一的

const obj = {};

obj[Symbol("name")]="zhangsan";

obj[Symbol("name")]="lisi"

console.log(obj);

//{Symbol(name): "zhangsan", Symbol(name): "lisi"}
```



### class类和extends继承

#### class定义构造函数

> 使用class关键字定义一个类

```javascript
class Animal{

     // 构造函数

    constructor(name){

          // 实例属性

        this.name = name;

    }

     // 实例方法

    sayName(){

        console.log(this.name);

    }

    // 静态方法(只能用类名调用)

    static say(){

            console.log('hello')

    }

}

let a = new Animal('二狗子');

a.sayName(); // 二狗子

Animal.say(); // hello
```



#### extends实现继承

> 使用extends关键字实现类对类的继承

```javascript
class Dog extends Animal{

    constructor(name,color){

        // super 调用父类的constructor 新建父类的this对象,相当于去执行了Animal类的constructor, 此时Dog类中的this其实就是Animal中的this,然后子类在对其进行加工修改

        super(name);

        this.color = color;

    }



    sayColor(){

        console.log(this.color);

    }

}


var d = new Dog('二狗子','白色');

// 继承父类实例方法

d.sayName(); // 二狗子

// 子类扩展方法

d.sayColor(); // 白色

// 继承父类静态方法

Dog.say(); // hello
```



### Promise

> Promise 是异步编程的一种解决方案.简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，可以从改对象获取异步操作的消息。

#### 基本用法

```javascript
// 1. 创建promise实例,在实例中执行异步操作(比如发送网络请求)

// 2. 异步操作成功时,调用reslove函数传递数据

// 3. 异步操作失败时,调用reject函数传递错误信息

const promise = new Promise(function(resolve, reject) {

    // 异步操作

    // ...

    if (/* 异步操作成功 */){

        resolve(value);

    } else {

        reject(error);

    }

});



// 4. 使用promise实例then方法接收reslove或reject返回的数据

promise.then(function(value) {

    // 此处数据即为reslove回来的数据

    // success

}, function(error) {

    // 此处数据即为reject回来的数据

    // failure

});
```



#### 网络请求案例

```javascript
// 1. 定义一个使用promise封装的请求函数,函数内部返回一个promise实例

function fetch(){

    // 函数内部返回一个promise实例

    return new Promise(function(reslove,reject){

        // 发送异步请求

        axios.get('<http://www.lovegf.cn:8090/api/getlunbo>').then(function(res){

            // 请求正常

            if(res.status == 1){

                reslove(res.data)

            }else{

                reject(res.error)

            }

        })

    })

}



// 2. 调用函数发送请求,通过Promise.prototype.then方法获取resolve或reject出来的数据

fetch().then(function(res){

    // res为reslove函数传出的数据

},function(err){

    // err为reject函数传出的错误

})
```



#### 解决回调地狱

> 假设有三个请求A、B、C,B请求需要依赖A请求的数据,C请求需要依赖B请求的数据.
>
> 传统回调函数式写法如下:

```javascript
function dependices_fetch(){

    // A请求

    axios.get('A').then(function(res){

        if(res.status == 1){

            // B请求

            axios.get('B').then(function(res){

                if(res.status == 1){

                    // C请求

                    axios.get('C').then(function(res){

                        // 请求完毕,执行后续逻辑

                    })

                }

            })

        }

    })

}
```



> 这种代码虽然能够满足业务,但是代码组织结构非常不便于阅读
>
> 通过Promise可以封装代码,使用链式方式解决这种多个异步依赖的回调
>
> 如下:

```javascript
function fetch(url){

    return new Promise(function(reslove,reject){

        axios.get(url).then(function(res){

            if(res.status == 1){

                reslove(res.data)

            }else{

                reject(res.error)

            }

        })

    })

}



//then方法内部返回的promise实例reslove或reject出来的对象会在下一个then方法内部得到

fetch('A').then(function(res){

    // A 请求正常

    return fetch('B')   // 这里返回一个新的promise实例,在后面的then中可以得到该实例reslove或reject出来的对象

}).then(function(res){

    // B 请求正常

    return fetch('C')

}).then(function(res){

    // C 请求正常

    // 请求完毕

})
```



#### 多个异步请求结果组合问题

> 假设有A、B、C三个异步请求,需要三个请求的数据都回来之后,将数据整合后再渲染页面,这种需求可以使用Promise.all()
>
> Promise.all方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。

```:american_samoa:
function fetch(url){

        return new Promise(function(reslove,reject){

            axios.get(url).then(function(res){

                if(res.status == 1){

                    reslove(res.data)

                }else{

                    reject(res.error)

                }

            })

        })

}



const p1 = fetch('A')

const p2 = fetch('B')

const p3 = fetch('C')

const p = Promise.all([p1, p2, p3]);

p.then(function(res){

    // res是一个数组,存放着p1,p2,p3的返回值

})
```


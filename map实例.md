# map实例

构建实例

const map = new Map()

Map结构的实例有以下属性和操作方法

#### 1.size属性

size属性返回结构的成员总数 

```
const map = new Map();
map.set('foo', true); 
map.set('bar', false);
map.size // 2

```

#### 2.set（key，value）

`set`方法设置键名`key`对应的键值为`value`，然后返回整个 Map 结构。如果`key`已经有值，则键值会被更新，否则就新生成该键。

```
const map = new Map();
m.set('edition', 6) 
可使用链式写法添加键值 
.set(1, 'a').set(2, 'b').set(3, 'c')
但是允许 一次写多个键值
```

#### 3.get(key)

`get`方法读取`key`对应的键值，如果找不到`key`，返回`undefined`。

```
const m = new Map()
const hello =function () {console.log('hello')}
m.set(hello,'hello yuan')
m.get(hello) //hello yu
```

#### 4.has(key)

`has`方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。





#### 5.delete(key)

`delete`方法删除某个键，返回`true`。如果删除失败，返回`false`。





#### 6.clear()方法没有返回值



#### 拓展

```
let map = new Map([
[1, 'one'],
[2, 'two'],
[3, 'three'],
]);

let arr = [...map.keys()]; // [1, 2, 3]
let arr = [...map.values()]; // ['one', 'two','three']
console.log(arr)
```


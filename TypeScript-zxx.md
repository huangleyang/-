# TypeScript

> `TypeScript` 是 `JavaScript` 的超集，一切`js`中可以使用的语法都可以在`ts`中使用
>
> `.js` 文件可以直接重命名为 `.ts` ,目前浏览器还不支持`.ts`文件,所以需要编译成`js`才能使用
>
> **主要特点**:新增 **类型推断**、**接口（Interfaces）**、**泛型（Generics）**、**类（Class）**、**枚举类型（Enums）**等前端工程师可能不是很熟悉的概念,使`JavaScript`弱类型语言变成强类型语言。
>
> https://ts.xcatliu.com/introduction/what-is-typescript.html



## 安装编译器

```js
// 1. 全局安装typescript
npm install -g typescript

// 2. 新建index.ts文件书写typescript代码，然后执行以下命令即可将ts代码转换为js代码
tsc index.ts

// 3. 自动转换(借助vscode等编译器可以实现自动转换)
// 3.1 先新建一个写代码的文件夹，在文件夹中执行以下命令
tsc --init 创建配置文件

// 3.2 创建ts文件,书写ts代码，在vscode中使用 CTRL + SHIFT + B 选择监视，以后每次改了ts文件都会自动生成js代码 
```



## 基本数据类型

### 布尔值

```typescript
let isDone: boolean = false;
```

### 数字

```typescript
let amount: number = 6;
```

### 字符串

- 类型
- 模板字符串
  - 支持换行
  - 支持内嵌表达式
- 和 JavaScript 一样，可以使用双引号，也可以使用单引号，推荐单引号

```typescript
let nickname: string = '张三';
```

还可以使用模板字符串（换行 + 嵌入表达式）：

```typescript
let nickname: string = `Gene`;
let age: number = 37;
let sentence: string = `Hello, my nickname is ${ nickname }.

I'll be ${ age + 1 } years old next month.`;
```

### 数组

TypeScript像JavaScript一样可以操作数组元素。 有两种方式可以定义数组。 第一种，可以在元素类型后面接上`[]`，表示由此类型元素组成的一个数组：

```typescript
let list: number[] = [1, 2, 3];
```

第二种方式是使用数组泛型，`Array<元素类型>`：

```typescript
let list: Array<number> = [1, 2, 3];
```

### 元组

元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 比如，你可以定义一对值分别为`string`和`number`类型的元组。

```typescript
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
```

### Object

- 允许赋任意值
- 但是不能调用任意方法，即便它真的有

```typescript
let foo: Object = {
  name: 'Jack',
  age: 18
}
```

> 知道即可，用的很少，没有类型校验和语法提示

### Any

有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 `any`类型来标记这些变量：

```typescript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

### Void

`void`类型像是与`any`类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是 `void`：

```typescript
function warnUser(): void {
  alert("This is my warning message");
}
```

声明一个`void`类型的变量没有什么大用，因为你只能为它赋予`undefined`和`null`：

```typescript
let unusable: void = undefined;
```

### Null 和 Undefined

和 `void`相似，它们的本身的类型用处不是很大：

```typescript
// Not much else we can assign to these variables!
let u: undefined = undefined;
let n: null = null;
```

默认情况下`null`和`undefined`是所有类型的子类型。 就是说你可以把 `null`和`undefined`赋值给`number`类型的变量。

然而，当你指定了`--strictNullChecks` 标记，`null` 和 `undefined` 只能赋值给 `void` 和它们各自。 这能避免 *很多*常见的问题。许在某处你想传入一个 `string`或`null`或`undefined`，你可以使用联合类型`string | null | undefined`。

> 注意：我们推荐尽可能地使用`--strictNullChecks` ，因为它使你的代码更严谨，可以极大的减少出错的几率。

### 类型推断

有时候你会遇到这样的情况，你会比TypeScript更了解某个值的详细信息。 通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。

通过*类型断言*这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript会假设你，程序员，已经进行了必须的检查。

类型断言有两种形式。 其一是“尖括号”语法：

```typescript
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```

另一个为`as`语法：

```typescript
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```

两种形式是等价的。 至于使用哪个大多数情况下是凭个人喜好；然而，当你在TypeScript里使用JSX时，只有 `as`语法断言是被允许的。

### 其它

- `ReadonlyArray<T>` 去除了数组的所有可变方法，确保数组创建后再也不能被修改

## 接口

TypeScript的核心原则之一是对值所具有的*结构*进行类型检查。 它有时被称做“鸭式辨型法”或“结构性子类型化”。 在TypeScript里，接口的作用就是为这些类型命名和为你的代码或第三方代码定义契约。

### 基本示例

```typescript
function printLabel(labelledObj: { label: string }) {
  console.log(labelledObj.label);
}

let myObj = { size: 10, label: "Size 10 Object" };
printLabel(myObj);
```

类型检查器会查看`printLabel`的调用。 `printLabel`有一个参数，并要求这个对象参数有一个名为`label`类型为`string`的属性。 需要注意的是，我们传入的对象参数实际上会包含很多属性，但是编译器只会检查那些必需的属性是否存在，并且其类型是否匹配。 然而，有些时候TypeScript却并不会这么宽松，我们下面会稍做讲解。

下面我们重写上面的例子，这次使用接口来描述：必须包含一个`label`属性且类型为`string`：

```typescript
interface LabelledValue {
  label: string;
}

function printLabel(labelledObj: LabelledValue) {
  console.log(labelledObj.label);
}

let myObj = {size: 10, label: "Size 10 Object"};
printLabel(myObj);
```

### 可选属性

接口里的属性不全都是必需的。 有些是只在某些条件下存在，或者根本不存在。 可选属性在应用“option bags”模式时很常用，即给函数传入的参数对象中只有部分属性赋值了。

```typescript
interface SquareConfig {
  color?: string;
  width?: number;
}

function createSquare(config: SquareConfig): {color: string; area: number} {
  let newSquare = {color: "white", area: 100};
  if (config.color) {
    newSquare.color = config.color;
  }
  if (config.width) {
    newSquare.area = config.width * config.width;
  }
  return newSquare;
}

let mySquare = createSquare({color: "black"});
```

### 只读属性

一些对象属性只能在对象刚刚创建的时候修改其值。 你可以在属性名前用 `readonly`来指定只读属性:

```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}
```

你可以通过赋值一个对象字面量来构造一个`Point`。 赋值后， `x`和`y`再也不能被改变了。

```typescript
let p1: Point = { x: 10, y: 20 };
p1.x = 5; // error!
```

`readonly` vs `const`

- 常量使用 const
- 对象属性使用 readonly

## 解构赋值

### 数组解构

```typescript
let input = [1, 2];
let [first, second] = input;
console.log(first); // outputs 1
console.log(second); // outputs 2
```

上面的写法等价于：

```typescript
first = input[0];
second = input[1];
```

利用解构赋值交换变量：

```typescript
[first, second] = [second, first];
```

函数参数解构：

```typescript
function f ([first, second]: [number, number]) [
  console.log(first)
  console.log(second)
]

f(1,2)
```

解构剩余参数：

```typescript
let [first, ...rest] = [1, 2, 3, 4]
console.log(first) // 1
console.log(rest) // [2, 3, 4]
```

也可以忽略其它参数：

```typescript
let [first] = [1, 2, 3, 4];
console.log(first); // outputs 1
```

或者跳过解构：

```typescript
let [, second, , fourth] = [1, 2, 3, 4]
```

### 对象解构

示例一：

```typescript
let o = {
    a: "foo",
    b: 12,
    c: "bar"
};
let { a, b } = o;
```

就像数组解构，你可以用没有声明的赋值：

```typescript
let a: number,
  b: number;

({a, b} = {a: 123, b: 456})

console.log(a, b) // 123 456
```

你可以在对象里使用 `...` 语法创建剩余变量：

```typescript
let { a, ...passthrough } = o;
let total = passthrough.b + passthrough.c.length;
```

#### 属性解构重命名

你也可以给属性以不同的名字：

```typescript
let { a: newName1, b: newName2 } = o;
```

注意，这里的冒号*不是*指示类型的。 如果你想指定它的类型， 仍然需要在其后写上完整的模式。

```typescript
let {a, b}: {a: string, b: number} = o;
```



#### 默认值

```typescript
function keepWholeObject(wholeObject: { a: string, b?: number }) {
    let { a, b = 1001 } = wholeObject;
}
```

### 展开操作符

- 展开数组
- 展开对象
  - 不会展开方法

### 解构赋值用于函数声明

```typescript
type C = {a: string, b?: number}

function f ({a, b}: C): void {
  // ...
}
```

### 解构赋值用于加载指定模块成员



## 类

### 基本示例

```typescript
class Person {
    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }

    sayHello() {
        console.log(this.name);
    }
}

let zs: Person = new Person('张三', 18);

```



### 构造函数

### 继承

```typescript
class Animal {
    move(distanceInMeters: number = 0) {
        console.log(`Animal moved ${distanceInMeters}m.`);
    }
}

class Dog extends Animal {
    bark() {
        console.log('Woof! Woof!');
    }
}

const dog = new Dog();
dog.bark();
dog.move(10);
dog.bark();
```

这个例子展示了最基本的继承：类从基类中继承了属性和方法。 这里， `Dog`是一个 *派生类*，它派生自 `Animal` *基类*，通过 `extends`关键字。 派生类通常被称作 *子类*，基类通常被称作 *超类*。

因为 `Dog`继承了 `Animal`的功能，因此我们可以创建一个 `Dog`的实例，它能够 `bark()`和 `move()`。

下面是一个更复杂的例子：

```typescript
class Animal {
    name: string;
    constructor(theName: string) { this.name = theName; }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}

class Snake extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 5) {
        console.log("Slithering...");
        super.move(distanceInMeters);
    }
}

class Horse extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 45) {
        console.log("Galloping...");
        super.move(distanceInMeters);
    }
}

let sam = new Snake("Sammy the Python");
let tom: Animal = new Horse("Tommy the Palomino");

sam.move();
tom.move(34);
```

与前一个例子的不同点是，派生类包含了一个构造函数，它 *必须*调用 `super()`，它会执行基类的构造函数。 而且，在构造函数里访问 `this`的属性之前，我们 *一定*要调用 `super()`。 这个是TypeScript强制执行的一条重要规则。

这个例子演示了如何在子类里可以重写父类的方法。 `Snake`类和 `Horse`类都创建了 `move`方法，它们重写了从`Animal`继承来的 `move`方法，使得 `move`方法根据不同的类而具有不同的功能。 注意，即使 `tom`被声明为`Animal`类型，但因为它的值是 `Horse`，调用 `tom.move(34)`时，它会调用 `Horse`里重写的方法：

```
Slithering...
Sammy the Python moved 5m.
Galloping...
Tommy the Palomino moved 34m.
```

### 实例成员访问修饰符

#### `public` 开放的

- 默认为 `public`

```typescript
class Animal {
    public name: string;
    public constructor(theName: string) { this.name = theName; }
    public move(distanceInMeters: number) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}
```

#### `private` 私有的

- 不能被外部访问，只能在类的内部访问使用
- 私有成员不会被继承

```typescript
class Person {
  public name: string;
  public age: number = 18;
  private type: string = 'human'
  public constructor (name, age) {
    this.name = name
    this.age = age
  }
}
```

#### `protected` 受保护的

- 和 `private` 类似，但是可以被继承

```typescript
class Person {
    protected name: string;
    constructor(name: string) { this.name = name; }
}

class Employee extends Person {
    private department: string;

    constructor(name: string, department: string) {
        super(name)
        this.department = department;
    }

    public getElevatorPitch() {
        return `Hello, my name is ${this.name} and I work in ${this.department}.`;
    }
}

let howard = new Employee("Howard", "Sales");
console.log(howard.getElevatorPitch());
console.log(howard.name); // 错误
```

注意，我们不能在 `Person`类外使用 `name`，但是我们仍然可以通过 `Employee`类的实例方法访问，因为`Employee`是由 `Person`派生而来的。

#### `readonly` 只读的

#### 在参数中使用修饰符

在上面的例子中，我们不得不定义一个受保护的成员 `name`和一个构造函数参数 `theName`在 `Person`类里，并且立刻给 `name`和 `theName`赋值。 这种情况经常会遇到。 *参数属性*可以方便地让我们在一个地方定义并初始化一个成员。 

```typescript
class Person {
  	name: string;
  	age: number;
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
}
```

可以简写为：

```typescript
class Person {
    constructor(public name: string, public age: number) {
    }
}
```



### 属性的存（get）取（set）器

```typescript
let passcode = "secret passcode";

class Employee {
  	// 私有成员，外部无法访问
    private _fullName: string;

  	// 当访问 实例.fullName 的时候会调用 get 方法
    get fullName(): string {
        return this._fullName;
    }

  	// 当对 实例.fullName = xxx 赋值的时候会调用 set 方法
    set fullName(newName: string) {
        if (passcode && passcode == "secret passcode") {
            this._fullName = newName;
        }
        else {
            console.log("Error: Unauthorized update of employee!");
        }
    }
}

let employee = new Employee();
employee.fullName = "Bob Smith";
if (employee.fullName) {
    alert(employee.fullName);
}
```



### 静态成员

- 不需要实例化访问的成员称之为静态成员，即只能被类访问的成员
- `static` 关键字

```typescript
class Grid {
    static origin = {x: 0, y: 0};
    calculateDistanceFromOrigin(point: {x: number; y: number;}) {
        let xDist = (point.x - Grid.origin.x);
        let yDist = (point.y - Grid.origin.y);
        return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
    }
    constructor (public scale: number) { }
}

let grid1 = new Grid(1.0);  // 1x scale
let grid2 = new Grid(5.0);  // 5x scale

console.log(grid1.calculateDistanceFromOrigin({x: 10, y: 10}));
console.log(grid2.calculateDistanceFromOrigin({x: 10, y: 10}));
```



## 函数

### 函数参数

- 参数及返回值类型

```typescript
function add(x: number, y: number): number {
    return x + y
}
```



- 可选参数

```typescript
function add(x: number, y?: number): number {
    return x + 10
}
```



- 默认参数

```typescript
function add(x: number, y: number = 20): number {
    return x + y
}
```



- 剩余参数

```typescript
function sum(...args: number[]): number {
    let ret: number = 0
    args.forEach((item: number): void => {
        ret += item
    })
    return ret
}

sum(1, 2, 3)
```



### 箭头函数

- 基本示例

```typescript
let add = (x: number, y: number): number => x + y
```




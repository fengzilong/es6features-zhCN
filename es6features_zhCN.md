# [ECMAScript 6 特性](https://github.com/lukehoban/es6features) / 翻译项目
## 介绍
ECMAScript 6，也被称做ECMAScript 2015，是ECMAScript标准的下一个版本。这个标准预计将在2015年6月被正式批准(然而现在已经是6月份，所以...)。ES6是这门语言的一次重大更新，自ES5以来该语言的首次更新是在2009年。主流Javascript引擎对ES6相关特性的实现[正在进行中](http://kangax.github.io/es5-compat-table/es6/)。

前往[ES6标准草案](https://people.mozilla.org/~jorendorff/es6-draft.html)查看ECMAScript 6的所有细节

ES6包括以下特性
- [arrows 箭头函数](#arrows)
- [class 类](#classes)
- [enhanced object literals 增强的对象字面量](#enhanced-object-literals)
- [template string 模板字符串](#template-string)
- [destructuring 解构](#destructuring)
- [default + rest + spread 关键字](#default--rest--spread)
- [let + const 关键字](#let--const)
- [iterators + for...of 遍历](#iterators--forof)
- [generators 生成器](#generators)
- [unicode 统一码](#unicode)
- [modules 模块](#modules)
- [module loaders 模块加载器](#module-loaders)
- [map + set + weakmap + weakset 新的数据类型](#map--set--weakmap--weakset)
- [proxies 代理](#proxies)
- [symbols 符号](#symbols)
- [subclassable built-ins 内置的继承](#subclassable-built-ins)
- [promises 承诺](#promises)
- [math + number + string + array + object APIs 新增的一些数据类型方法](#math--number--string--array--object-apis)
- [binary and octal literals 二进制和八进制字面量](#binary-and-octal-literals)
- [reflect api 反射](#reflect-api)
- [tail calls 尾调用](#tail-calls)

## ECMAScript 6 特性

###Arrows 箭头函数
箭头函数是使用 => 语法简写的函数。他们在语法上类似C#、Java 8和CoffeeScript中的相关特性。他们同时支持表达式和语句块。和普通函数不同，箭头函数和上下文代码共享同一个this

```Javascipt
// Expression bodies 表达式
var odds = evens.map(v => v + 1);
var nums = evens.map((v, i) => v + i);
var pairs = evens.map(v => ({even: v, odd: v + 1}));

// Statement bodies 语句块
nums.forEach(v => {
  if (v % 5 === 0)
    fives.push(v);
});

// Lexical this
var bob = {
  _name: "Bob",
  _friends: [],
  printFriends() {
    this._friends.forEach(f =>
      console.log(this._name + " knows " + f));
  }
}
```

### Class 类
ES6中提供了一个基于原型的面向对象模式的类声明的语法糖。这种简单的声明方式使得类模式变得更加容易使用，也促进了类之间的协作关系。类支持原型继承、调用父方法、实例方法、静态方法和构造函数。

```JavaScript
class SkinnedMesh extends THREE.Mesh {
  constructor(geometry, materials) {
    super(geometry, materials);

    this.idMatrix = SkinnedMesh.defaultMatrix();
    this.bones = [];
    this.boneMatrices = [];
    //...
  }
  update(camera) {
    //...
    super.update();
  }
  get boneCount() {
    return this.bones.length;
  }
  set matrixType(matrixType) {
    this.idMatrix = SkinnedMesh[matrixType]();
  }
  static defaultMatrix() {
    return new THREE.Matrix4();
  }
}
```

### Enhanced Object Literals 增强的对象字面量
对象字面量支持在构造函数中设置原型，`foo: foo`赋值的简写，定义方法，调用父方法，使用表达式计算属性名。同时这些也使得对象字面量和类声明更接近，也使得基于对象的设计从中受益。

```JavaScript
var obj = {
    // __proto__
    __proto__: theProtoObj,
    // Shorthand for ‘handler: handler’
    handler,
    // Methods
    toString() {
     // Super calls
     return "d " + super.toString();
    },
    // Computed (dynamic) property names
    [ 'prop_' + (() => 42)() ]: 42
};
```

### Template Strings 模板字符串
模板字符串提供了构建字符串的语法糖。这类似于Perl、Python和其他语言中的字符串插值特性。此外，作为可选项，标签能被用来添加进去使得字符串构建能自定义，避免注入攻击，或者基于字符串构建的高阶数据结构。

```JavaScript
// 基础的字面字符串创建
`In JavaScript '\n' is a line-feed.`

// 多行文本
`In JavaScript this is
 not legal.`

// 字符串插值
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`

//构造一个HTTP请求前缀，用来替换和生成
GET`http://foo.org/bar?a=${a}&b=${b}
    Content-Type: application/json
    X-Credentials: ${credentials}
    { "foo": ${foo},
      "bar": ${bar}}`(myOnReadyStateChangeHandler);
```

### Destructuring 解构
解构允许使用模式匹配的绑定，支持匹配数组和对象。解构是Fail-soft的，类似于对象查找`foo["bar"]`，未找到则会对应undefined

```JavaScript
// 匹配数组
var [a, , b] = [1,2,3];

// 匹配对象
var { op: a, lhs: { op: b }, rhs: c }
       = getASTNode()

// 匹配对象的简写方式
// 绑定作用域中的`op`，`lhs`，`rhs`
var {op, lhs, rhs} = getASTNode()

// 能被用在函数形参中
function g({name: x}) {
  console.log(x);
}
g({name: 5})

// Fail-soft的解构
var [a] = [];
a === undefined;

// 带有默认值的Fail-soft解构
var [a = 1] = [];
a === 1;
```

### Default + Rest + Spread 默认参数 + 不定参数 + 参数展开
支持被调用函数设置参数的默认值。在函数调用时使用`...`可以将一个数组展开后作为参数传入。在定义函数时使用`...`可以将传入的剩余参数转成一个数组。不定参数在很多情形下可以取代arguments的使用。

```JavaScript
function f(x, y=12) {
  // y is 12 if not passed (or passed as undefined)
  return x + y;
}
f(3) == 15
```
```JavaScript
function f(x, ...y) {
  // y is an Array
  return x * y.length;
}
f(3, "hello", true) == 6
```
```JavaScript
function f(x, y, z) {
  return x + y + z;
}
// Pass each elem of array as argument
f(...[1,2,3]) == 6
```

### Let + Const let和const关键字
let和const都是块级作用域的绑定构造。`let`是新的`var`。`const`是单次赋值的。`let`和`const`都有静态限制，可以避免在声明前使用

```JavaScript
function f() {
  {
    let x;
    {
      // okay, block scoped name
      const x = "sneaky";
      // error, const
      x = "foo";
    }
    // error, already declared in block
    let x = "inner";
  }
}
```

### Iterators + For..Of
TODO



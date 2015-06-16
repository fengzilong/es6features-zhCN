# [ECMAScript 6 特性](https://github.com/lukehoban/es6features) / 翻译项目
## 介绍
ECMAScript 6，也被称做ECMAScript 2015，是ECMAScript标准的下一个版本。这个标准预计将在2015年6月被正式批准(然而现在已经是6月份，所以...)。ES6是这门语言的一次重大更新，自ES5以来该语言的首次更新是在2009年。主流Javascript引擎对ES6相关特性的实现[正在进行中](http://kangax.github.io/es5-compat-table/es6/)。

前往[ES6标准草案](https://people.mozilla.org/~jorendorff/es6-draft.html)查看ECMAScript 6的所有细节

ES6包括以下特性
- [arrows 箭头函数](#arrows)
- [class 类](#classes)
- [enhanced object literals 增强的对象字面量](#enhanced-object-literals)
- [template string 模板字符串](#template-string)
- [destructuring 析构](#destructuring)
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






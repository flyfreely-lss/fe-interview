JavaScript 不知道原型链可以凑活，但不知道数据类型可是万万不能哒[天啊]

## 数据类型
引用数据类型：统称为 Object 类型，包括：Object、Array、Date等。

7种基础类型看下MDN这张表：
| 类型      | typeof返回值 | 对象包装器 |
| --------- | ------------ | ---------- |
| Null      | "object"     | N/A        |
| Undefined | "undefined"  | N/A        |
| Boolean   | "boolean"    | Boolean    |
| Number    | "number"     | Number     |
| BigInt    | "bigint"     | BigInt     |
| String    | "string"     | String     |
| Symbol    | "symbol"     | Symbol     |

从表格可以看出：
1. 除了`null`，所有的基础类型都可以用`typeof`运算符测试。因此必须使用 === null 来测试 null。
2. 除了 null 和 undefined，所有原始类型都有它们相应的对象包装类型。

除此外，有两个比较面生的数据类型：BigInt、Symbol，下面简单列举下如何使用。

**BigInt**
```javascript
const bi = BigInt(Number.MAX_SAFE_INTEGER); 
console.log(bi); // 9007199254740991n
```
上面输出的数字后面的 n 是因为：BigInt 是通过将 n 附加到整数末尾或调用 BigInt() 函数来创建的。

**Symbol**

也被称作“原子类型”（atom）。symbol 的目的是去创建一个唯一属性键，保证不会与其他代码中的键产生冲突。
```javascript
let s = Symbol();

let obj = {
  [s]: function (arg) { ... }
};

obj[s](123);
```

## 类型检测
涉及两个关键字：`typeof`、`instanceof`。

**typeof**

`typeof` 主要用来检测基础数据类型，在上述表格中有体现。

**instanceof**

`instanceof `主要用来检测引用数据类型。**实现原理就是检测关键字右边构造函数的 prototype 属性是否在左边实例对象的原型链上**。（对于原型链不清晰的可以参考上篇文章[JavaScript原型及原型链](./2-JavaScript原型及原型链.md)）

所以 instanceof 也可能判断不准确，比如一个数组，他可以被 instanceof 判断为 Object。所以我们要想比较准确的判断对象实例的类型时，可以采取 `Object.prototype.toString.call` 方法。
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

值得注意的是，`typeof null` 为 `object`。

**instanceof**

`instanceof `主要用来检测引用数据类型。**实现原理就是检测关键字右边构造函数的 prototype 属性是否在左边实例对象的原型链上**。（对于原型链不清晰的可以参考上篇文章[JavaScript原型及原型链](./2-JavaScript原型及原型链.md)）

所以 instanceof 也可能判断不准确，比如一个数组，他可以被 instanceof 判断为 Object。所以我们要想比较准确的判断对象实例的类型时，可以采取 `Object.prototype.toString.call` 方法。

**Object.prototype.toString.call**

该方法返回对象的类型字符串，因此可以用来判断一个值的类型。

不同数据类型的`Object.prototype.toString`方法返回值如下:

```javascript
console.log(Object.prototype.toString.call(2)); > "[object Number]"
console.log(Object.prototype.toString.call('str')); > "[object String]"
console.log(Object.prototype.toString.call(true)); > "[object Boolean]"
console.log(Object.prototype.toString.call(undefined)); > "[object Undefined]"
console.log(Object.prototype.toString.call(null)); > "[object Null]"
console.log(Object.prototype.toString.call([])); > "[object Array]"
console.log(Object.prototype.toString.call(arguments)); > "[object Arguments]"
console.log(Object.prototype.toString.call(function(){})); > "[object Function]"
console.log(Object.prototype.toString.call(new Error())); > "[object Error]"
console.log(Object.prototype.toString.call(new Date())); > "[object Date]"
console.log(Object.prototype.toString.call(new RegExp())); > "[object RegExp]"
其他对象：返回[object Object]。
```

### 关联面试题

1. JavaScript中Number类型的范围是多少？

   `Number` 类型是一个[双精度 64 位二进制格式 IEEE 754](https://zh.wikipedia.org/wiki/雙精度浮點數) 值，类似于 Java 中的 `double`。64位由3部分组成：

   - 1 位符号，正数或负数
   - 11 位表示指数，其中1个符号位
   - 52 位小数位

     所以，一个number值可容纳的最大数值为 2^1024^ - 1（基于指数位得出），对应的十进制范围为：-2^53^ + 1（`Number.MIN_SAFE_INTEGER`） 到 2^53^ - 1（`Number.MAX_SAFE_INTEGER`，16位）

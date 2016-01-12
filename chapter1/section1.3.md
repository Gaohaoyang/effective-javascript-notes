# 第3条 当心隐式的强制转换

* 类型错误可能被隐式的强制转换所隐藏。
* 重载的运算符 + 是进行加法运算还是字符串链接操作取决于其参数类型。
* 对象通过`valueOf()`方法强制转化为数字，通过`toString()`方法强制转化为字符串。
* 具有`valueOf()`方法的对象应该实现`toString()`方法，返回一个`valueOf()`方法产生的数字的字符串表示。
* 测试一个值是否为未定义的值，应该使用`typeof`或者与`undefined`进行比较而不是使用真值运算。

---

强制转换会隐藏错误。结果为`null`的变量在算术运算中不会导致失败，而是被隐式转换为0；未定义的变量会被转换为特殊的浮点数值`NaN`。不会抛出异常，而是继续运算。

监测`NaN`也会让人头疼。因为

```js
var x = NaN;
x === NaN;//false
```

标准库函数`isNaN`也不可靠。因为对于类型已知是数字的，没有问题。但是对于其他绝对不是`NaN`，但会被强制转换为`NaN`的值，使用此方法无效。例如：

```js
isNaN(NaN);//true
isNaN('aaaaString');//true
isNaN(undefined);//true
isNaN({});//true
isNaN({a:'a'});//true
```

由于`NaN`是唯一一个不等于自身的值，所以可以用是否与自身相等的方法检测。

```js
var a = NaN;
a !== a;//true
var b = 10;
b !== b;//false
```

可以将这个特性抽象为一个实用工具函数

```js
function isReallyNaN(a) {
    return x !== x;
}
```

对象也会被强制转换为原始值。最常见为字符串：

```js
'the Math Object:' + Math;//"the Math Object:[object Math]"
```

对象隐式调用了自身的`toString`方法转换为字符串。

类似的，对象也可以通过其`valueOf`方法转换为数字。

```js
var object = { valueOf:function () {
    return 3;
}};
2 * object;//6
```

即：
* **对象通过`valueOf()`方法强制转化为数字，通过`toString()`方法强制转化为字符串。**

但是，一个对象同时有`toString`和`valueOf`时：

```js
var obj = {
    toString:function () {
        return '[object MyObject]';
    },
    valueOf:function () {
        return 17;
    }
}
'abc' + obj;//"abc17"
```

这个例子说明，`valueOf`才是真正为哪些代表数值的对象而设计的。所以引出了我们开篇说到的第4条：
* **具有`valueOf()`方法的对象应该实现`toString()`方法，返回一个`valueOf()`方法产生的数字的字符串表示。**

这样就不会紊乱了。并且如果对象不是数字的抽象的话，应该尽量避免使用`valueOf`方法。

最后说说真值运算。

大多数的 JavaScript 值都为真值，也就是能隐式的转换为 true。

JavaScript 中有7个假值：false, 0, -0, "", NaN, null, undefined.

```js
function point(x,y) {
    if (!x) {
        x = 320;
    }
    if (!y) {
        y=240;
    }
    return{
        x:x,
        y:y
    }
}
```

此函数会忽略任何为假值的参数，包括0。应该使用如下代码。

```js
function point(x,y) {
    if (typeof x ==='undefined') {
        x = 320;
    }
    if (typeof y ==='undefined') {
        y = 240;
    }
    return{
        x:x,
        y:y
    }
}
```

或

```js
if (x === undefined) {
    ......
}
```

即：
* **测试一个值是否为未定义的值，应该使用`typeof`或者与`undefined`进行比较而不是使用真值运算。**

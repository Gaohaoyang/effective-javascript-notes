# 第10条 避免使用 with

* 避免使用 `with` 语句。
* 使用简短的变量名代替重复访问的对象。
* 显示地绑定局部变量到对象属性上，而不要使用 `with` 语句隐式地绑定它们。

---

它提供的任何“便利”，都更让其变得不可靠和低效率。

`with` 语句的动机是可以理解的。程序经常需要对单个对象依次调用一系列方法。使用 `with` 语句可以很方便地避免对对象的重复引用。

```js
function status(info) {
    var widget = new Widget();
    with (widget){
        setBackground('blue');
        setText('aaa');
        show();
    }
}
```

使用 `with` 语句从模块对象中“导入”变量也是很有诱惑力。

```js
function f(x, y) {
    with (Math){
        return min(round(x), sqrt(y));
    }
}
```

实际上并没有做应该做的事。JavaScript 对待所有变量都是相同的。从内层作用域开始向外查找变量。

最好将上述代码改为如下：

```js
function status(info) {
    var w = new Widget();
    w.setBackground('blue');
    w.setText('aaa');
    w.show();
}

function f(x, y) {
    var min = Math.min, round = Math.round, sqrt = Math.sqrt;
    return min(round(x), sqrt(y));
}
```

# 第11条 熟练掌握闭包

* 函数可以引用定义在其外部作用域的变量。
* 闭包比创建他们的函数有更长的声明周期。
* 闭包在内部存储其外部变量的引用，并能读写这些变量。

---

首先，JavaScript 允许你引用当前函数以外定义的变量。

```js
function makeSandwich() {
    var magicIngredient = "peanut butter";
    function make(filling) {
        return magicIngredient + " and " + filling;
    }
    return make("jelly");
}
makeSandwich(); // "peanut butter and jelly"
```

第二，即使外部函数已经返回，当前函数仍然可以引用在外部函数所定义的变量。

```js
function sandwichMaker() {
    var magicIngredient = "peanut butter";
    function make(filling) {
        return magicIngredient + " and " + filling;
    }
    return make;
}
var f = sandwichMaker();
f("jelly"); // "peanut butter and jelly"
f("bananas"); // "peanut butter and bananas"
f("marshmallows"); // "peanut butter and marshmallows"
```

JavaScript 的函数值包含了比调用它们时执行所需要的代码还要多的信息。而且，JavaScript 函数值还在内部存储它们可能会引用的定义在其封闭作用于的变量。那些在其所涵盖的作用域内跟踪变量的函数成为 **闭包**。`make` 函数就是一个闭包，其引用了两个外部变量 `magicIngredient` 和 `filling`。当 `make` 函数被调用时，其代码都能引用到这两个变量，因为该闭包存储了这两个变量。

可以根据这个编写更通用的 `sandwichMaker` 函数：

```js
function sandwichMaker(magicIngredient) {
    function make(filling) {
        return magicIngredient + " and " + filling;
    }
    return make;
}
var hamAnd = sandwichMaker("ham");
hamAnd("cheese"); // "ham and cheese"
hamAnd("mustard"); // "ham and mustard"
var turkeyAnd = sandwichMaker("turkey");
turkeyAnd("Swiss"); // "turkey and Swiss"
turkeyAnd("Provolone"); // "turkey and Provolone"
```

闭包的最后一个特点：闭包可以更新外部变量的值。

```js
function box() {
    var val = undefined;
    return {
        set: function(newVal) { val = newVal; },
        get: function() { return val; },
        type: function() { return typeof val; }
    };
}
var b = box();
b.type(); // "undefined"
b.set(98.6);
b.get(); // 98.6
b.type(); // "number"
```

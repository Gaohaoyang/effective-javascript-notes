# 第5条 避免对混合类型使用 == 运算符

* 当参数类型不同时，== 运算符应用了一套难以理解的隐式强制转换规则。
* 使用 === 运算符，使读者不需要涉及任何的隐式强制转换就能明白你的比较运算。
* 当比较不同类型的值时，使用你自己的显示强制转换使程序的行为更清晰。

---

当两个参数属于同一类型时，`==`和`===`运算符的行为是没有区别的。

**== 运算符的强制转换规则**

参数类型1  | 参数类型2 | 强制转换
------------- | ------- | ------
null  | undefined | 不转换，总是返回 true
null 或 undefined | 其他任何非 null 或 undefined 的类型 | 不转换，总是返回 false
原始类型：string、number 或 boolean | Date 对象 | 将原始类型转换为数字；将 Date 对象转换为原始类型（优先尝试 toString 方法，再尝试 valueOf 方法）
原始类型：string、number 或 boolean | 非 Date 对象 | 将原始类型转换为数字；将 非 Date 对象转换为原始类型（优先尝试 valueOf 方法，再尝试 toString 方法）
原始类型：string、number 或 boolean | 原始类型：string、number 或 boolean | 将原始类型转换为数字

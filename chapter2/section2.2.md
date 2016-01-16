# 第9条 始终声明局部变量
- 始终使用 `var` 声明新的局部变量。
- 考虑使用 `lint` 工具帮助检查未绑定的变量。

要注意意外的全局变量。

如果忘记将变量声明为局部变量，那么该变量将会被隐式的转换为全局变量。

```js
function swap(a,i,j) {
    temp = a[i];//global
    a[i] = a[j];
    a[j] = temp;
}
```

应使用 `var` 声明 `temp`。

```js
function swap(a,i,j) {
    var temp = a[i];
    a[i] = a[j];
    a[j] = temp;
}
```

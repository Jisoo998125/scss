## 语法嵌套规则

> 语法嵌套规则

`css`中重写选择器是非常恼人的。尤其是`html`结构嵌套非常深的时候，`scss`的选择器嵌套可以避免重复输入父选择器，可以有效的提高工作效率，减少样式覆盖可能造成的异常问题。这也是我们最常用的功能。

```scss
.container {
    width: 1200px;
    margin: 0 auto;
    .header {
        .img {
            width: 100px;
            height: 60px;
        }
    }
}
```

编译为

```css
.container {
    width: 1200px;
    margin: 0 auto;
}
.container .header .img {
    width: 100px;
    height: 60px;
}
```

> 属性嵌套

有些 `css`属性遵循相同的命名空间(相同的开头)，比如`font-family`,`font-size`,`font-weight`都是以`font`作为属性的命名空间。为了方便管理这样的属性，也为了避免重复输入。
```scss
.container {
  font: {
    family: fantasy;
    size: 30em;
    weight: bold;
  }
}
```

编译为

```css
.container {
  font-family: fantasy;
  font-size: 30em;
  font-weight: bold;
}
```

> 父选择器 `&`

在嵌套 `css` 规则时，有时候需要使用嵌套外层的父选择器，例如，当给某个元素设定 `hover` 样式的时候，可以使用 `&` 代表嵌套规则外层的父选择器， `scss` 在编译是会把 `&` 替换为父选择器名

```scss
.container {
  a {
    color: #333;
    &:hover {
      color: #f00;
    }
  }
}
```

编译后

```css
.container {
    color: #333;
}
.container a:hover {
    color: #f00;
}
```

换个思路，也可以使用 `&` 进行选择器名称拼接
```scss
.main {
  color: red;
  &--active {
    border: 1px solid red;
  }
}
```

编译后

```css
.main {
    color: red;
}
.main--active {
    border: 1px solid red;
}
```

## `scss`的两种注释

> 多行注释 `/*.....*/`

多行注释会编译到 `.css` 文件中，`compressed`(压缩)模式下除外，将`!`作为多行注释的第一个字符表示压缩输出模式下也保留这个信息。

```scss
/*！
* 我是
* 多行
* 注释
 */
body {
  color: red;
}
```

编译为

```css
/*!
* 我是
* 多行
* 注释 
*/body{color:red}
```

> 单行注释

单行注释不会编译到 `.css` 文件

```scss
// 我是单行注释
body {
  color: black;
}
```

//编译成 `css`

```css
body {
    color: black;
}
```

## 变量

> 使用

原生 `css` 中的变量，使用`--变量名:变量值`定义，`var(--变量名)`进行使用。

```css
:root {
    --color: #F00;
}
```

`scss`中的变量，以美元符号 `$` 开头，赋值方法与 `css` 属性的写法一样

```scss
$color: #F00;
p {
  color: $color;
}
```

编译为

```css
p {
    color: #F00;
}
```

> 5条变量规则

下文的 `mixin` 和 `function` 命名也遵循`1234`条规范：

1. 变量以美元符号 `$` 开头，后面跟变量名；
2. 变量名是不以数字开头的可包含字母、数字、下划线、横线(连接符);
3. 通过连接符 `-` 与下划线 `_` 定义的同名变量为同一变量;
4. 变量一定要先定义，后使用;
5. 写法同 `css`， 即变量和值之间用冒号 `:` 分隔;

> 2中变量作用域

1. 变量作用域分为 `全局作用域` 和 `局部作用域`：
    - 全局变量： 声明在最外层的变量，可在任何地方使用；
    - 局部变量： 嵌套规则内定义的变量只能在嵌套规则内使用。
2. 将局部变量转换为全局变量可以添加 `!global` 声明。
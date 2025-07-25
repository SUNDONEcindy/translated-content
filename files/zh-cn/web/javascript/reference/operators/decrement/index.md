---
title: 自减（--）
slug: Web/JavaScript/Reference/Operators/Decrement
---

**自减**（**`--`**）运算符对其操作数进行自减（减一），并根据运算符的位置返回自减之前或之后的值。

{{InteractiveExample("JavaScript Demo: Expressions - Decrement operator")}}

```js interactive-example
let x = 3;
const y = x--;

console.log(`x:${x}, y:${y}`);
// Expected output: "x:2, y:3"

let a = 3;
const b = --a;

console.log(`a:${a}, b:${b}`);
// Expected output: "a:2, b:2"
```

## 语法

```js-nolint
x--
--x
```

## 描述

如果使用后缀式，即将运算符放在操作数的后面（如，`x--`），操作数会减一，然后返回减一之前的值。

如果使用前缀式，即将运算符放在操作数的前面（如，`--x`），操作数会减一，然后返回减一之后的值。

自减运算符只能应用于引用的操作数（变量和对象属性，即有效的[赋值目标](/zh-CN/docs/Web/JavaScript/Reference/Operators/Assignment)）。`--x` 本身的计算结果是一个值，而不是一个引用，因此不能将多个自减运算符链接在一起。

```js example-bad
--(--x); // SyntaxError: Invalid left-hand side expression in prefix operation
```

## 示例

### 后缀式

```js
let x = 3;
const y = x--;

// x = 2
// y = 3
```

### 前缀式

```js
let x = 3;
const y = --x;

// x = 2
// y = 2
```

## 规范

{{Specifications}}

## 浏览器兼容性

{{Compat}}

## 参见

- [加法运算符](/zh-CN/docs/Web/JavaScript/Reference/Operators/Addition)
- [减法运算符](/zh-CN/docs/Web/JavaScript/Reference/Operators/Subtraction)
- [除法运算符](/zh-CN/docs/Web/JavaScript/Reference/Operators/Division)
- [乘法运算符](/zh-CN/docs/Web/JavaScript/Reference/Operators/Multiplication)
- [取余运算符](/zh-CN/docs/Web/JavaScript/Reference/Operators/Remainder)
- [幂运算符](/zh-CN/docs/Web/JavaScript/Reference/Operators/Exponentiation)
- [自增运算符](/zh-CN/docs/Web/JavaScript/Reference/Operators/Increment)
- [一元减运算符](/zh-CN/docs/Web/JavaScript/Reference/Operators/Unary_negation)
- [一元加运算符](/zh-CN/docs/Web/JavaScript/Reference/Operators/Unary_plus)

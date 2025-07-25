---
title: 符号なし右シフト (>>>)
slug: Web/JavaScript/Reference/Operators/Unsigned_right_shift
---

{{jsSidebar("Operators")}}

**符号なし右シフト演算子 (`>>>`)** (ゼロ埋め右シフト) は、1つ目のオペランドを指定されたビット数だけ右にずらします。右にずらしてあふれたビットは廃棄されます。0 のビットが左からずれて入ります。符号ビットは `0` になりますので、結果は負の数にはなりません。他のビット毎演算子とは異なり、ゼロ埋め右シフトは符号なし 32 ビット整数を返します。

{{InteractiveExample("JavaScript デモ: Expressions - Unsigned right shift operator")}}

```js interactive-example
const a = 5; //  00000000000000000000000000000101
const b = 2; //  00000000000000000000000000000010
const c = -5; //  11111111111111111111111111111011

console.log(a >>> b); //  00000000000000000000000000000001
// Expected output: 1

console.log(c >>> b); //  00111111111111111111111111111110
// Expected output: 1073741822
```

## 構文

```js
a >>> b;
```

## 解説

この演算子は、1 つ目のオペランドを指定されたビット数だけ右にずらします。右にずらしてあふれたビットは廃棄されます。0 のビットが左からずれて入ります。符号ビットは `0` になりますので、結果は負の数にはなりません。他のビット毎演算子とは異なり、ゼロ埋め右シフトは符号なし 32 ビット整数を返します。

負の数ではない場合、ゼロ埋め右シフトと符号保存右シフトは同じ結果をになります。例えば、 `9 >>> 2` は 2 となり、 `9 >> 2` と同じになります。

```js
.     9 (10 進数): 00000000000000000000000000001001 (2 進数)
                   --------------------------------
9 >>> 2 (10 進数): 00000000000000000000000000000010 (2 進数) = 2 (10 進数)
```

しかし、これは負の数の場合は当てはまりません。例えば、 `-9 >>> 2` は 1073741821 になり、 `-9 >> 2` とは異なります (`-3` になる)。

```js
.     -9 (10 進数): 11111111111111111111111111110111 (2 進数)
                    --------------------------------
-9 >>> 2 (10 進数): 00111111111111111111111111111101 (2 進数) = 1073741821 (10 進数)
```

## 例

### 符号なし右シフトの使用

```js
9 >>> 2; // 2
-9 >>> 2; // 1073741821
```

## 仕様書

{{Specifications}}

## ブラウザーの互換性

{{Compat}}

## 関連情報

- [ビット演算子 (JavaScript ガイド)](/ja/docs/Web/JavaScript/Guide/Expressions_and_operators#ビット演算子)
- [符号なし右シフト代入演算子](/ja/docs/Web/JavaScript/Reference/Operators/Unsigned_right_shift_assignment)

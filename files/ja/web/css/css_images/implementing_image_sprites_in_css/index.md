---
title: CSS での画像スプライトの実装
slug: Web/CSS/CSS_images/Implementing_image_sprites_in_CSS
l10n:
  sourceCommit: 50c8e290f11b061bbf2267e1a3279f28180a5fcb
---

**画像スプライト**は、複数の画像を使用する多くのウェブアプリで使用されています。それぞれの画像を個別の画像ファイルとして含めるのではなく、1 枚の画像として送信した方が、メモリーや帯域幅の面ではるかに有利です。同じ画像ファイル内の個々の画像を区別する方法として背景の位置を使用することで、 HTTP リクエストの数を減らすことができます。

> [!NOTE]
> HTTP/2 を使用する場合は、複数の小さなリクエストを使用する方が、実際には帯域幅に負荷を掛けない可能性があります。

## 実装

例えば、 `tool-btn` クラスを持つすべてのアイテムに画像が与えられたとします。

```css
.tool-btn {
  background: url(myfile.png);
  display: inline-block;
  height: 20px;
  width: 20px;
}
```

背景の位置は、 background の中で {{cssxref("url_value", "&lt;url&gt;")}} の後に x と y の 2 つの値として、または {{cssxref("background-position")}} として追加できます。

```css
#btn1 {
  background-position: -20px 0px;
}

#btn2 {
  background-position: -40px 0px;
}
```

これは ID が `btn1` の要素を左に 20 ピクセル、 ID が `btn2` の要素を左に 40 ピクセル移動します（これらの要素には `toolbtn` クラスが割り当てられており、上記の画像ルールの影響を受けているとします）。

同様に、ホバー状態にすることもできます。

```css
#btn:hover {
  background-position: <右方向へずらすピクセル数>px <下方向へずらすピクセル数>px;
}
```

## 関連記事

- [Full working demo at CSS Tricks](https://css-tricks.com/snippets/css/perfect-css-sprite-sliding-doors-button/)

---
title: WAI-ARIA の基本
short-title: WAI-ARIA
slug: Learn_web_development/Core/Accessibility/WAI-ARIA_basics
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/CSS_and_JavaScript","Learn_web_development/Core/Accessibility/Multimedia", "Learn_web_development/Core/Accessibility")}}

前回の記事に続いて言えることですが、意味論的ではない HTML や JavaScript によって更新される動的なコンテンツを含むような、複雑な UI コントロールの作成は難しくなることがあります。 WAI-ARIA は、ブラウザーや支援技術が認識できるさらなる意味論を追加することによってそのような問題に対処し、ユーザーの理解を助ける技術です。 ここでは、アクセシビリティを向上させるための基本的な使い方を説明します。

<table>
  <tbody>
    <tr>
      <th scope="row">前提知識:</th>
      <td><a href="/ja/docs/Learn_web_development/Core/Structuring_content">HTML</a>、<a href="/ja/docs/Learn_web_development/Core/Styling_basics">CSS</a> の知識、<a href="/ja/docs/Learn_web_development/Core/Accessibility/What_is_accessibility">基本的なアクセシビリティの概念の理解</a>。</td>
    </tr>
    <tr>
      <th scope="row">学習成果:</th>
      <td>
        <ul>
          <li>WAI-ARIA の目的は、意味づけされていない HTML に意味づけを提供し、 AT のユーザーが存在するインターフェイスを理解できるようにすることです。</li>
          <li>基本構文 — ロール、プロパティ、状態。</li>
          <li>ランドマークと標識。</li>
          <li>キーボードのアクセシビリティを向上させること</li>
          <li>ライブリージョンによる動的コンテンツの更新のアナウンス。</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## WAI-ARIA って何？

まずは、WAI-ARIA とは何か、そして何ができるのかという点から始めましょう。

### 全く新しい問題

ウェブアプリがより複雑で動的なものになると、新しいアクセシビリティの機能と問題が明らかになってきます。

例えば、 HTML は普遍的なページの機能を定義するためにいくつもの意味論的要素を取り入れました（{{htmlelement("nav")}}、{{htmlelement("footer")}} 等）。 これらが利用可能になる以前は、開発者は `<div class="nav">` のように単に {{htmlelement("div")}} を ID や class と共に使っていましたが、メインナビゲーションのようなページ内の機能をプログラムで識別する簡単な方法が無いために問題がありました。

初期の解決方法は、ナビゲーションへとリンクさせるため、次のようにページ上部に 1 つ以上の隠しリンク（もしくは他の何か）を追加することでした。

```html
<a href="#hidden" class="hidden">ナビゲーションへスキップ</a>
```

しかしこれは簡潔な方法ではない上に、スクリーンリーダーがページの先頭から読み込む場合にのみ利用できるものでした。

他の例としては、アプリが日付選択のための日付ピッカーや値選択のためのスライダーなどの複雑なコントロールを使いだしたケースがあります。 HTML は、そのようなコントロールを表す特別な入力型を提供しています。

```html
<input type="date" /> <input type="range" />
```

これらはブラウザー間の対応が十分でなく、またスタイル付けすることがとても困難であるため、ウェブサイトのデザインに合わせる上で不便となります。 結果として、開発者は多くの場合 JavaScript ライブラリーを利用して複数のネストされた {{htmlelement("div")}} でそのようなコントロールを生成したり、クラス名を持った表要素に対して CSS によるスタイル付けと JavaScript による制御を行ったりします。

この場合の問題は、見た目上は動作するものの、スクリーンリーダーはそれらが何なのか全く理解できず、ユーザーには意味を成さないごちゃごちゃの要素であるとしか教えられないという点にあります。

### WAI-ARIA の導入

[WAI-ARIA](https://www.w3.org/TR/wai-aria/) は W3C によって定められた仕様で、要素に適用できる追加の意味論を提供する一連の HTML 属性を定義しており、それが欠けているどのような場所でもアクセシビリティを向上させます。 この仕様では、主に次の 3 つの機能があります。

- [ロール](/ja/docs/Web/Accessibility/ARIA/Reference/Roles) (Role)
  - : これは要素が何か、もしくは何をするかを定義します。 多くの場合はいわゆるランドマークロール (landmark role) であり、主に HTML 構造要素の意味づけを複製します。 例えば、 `role="navigation"` ({{htmlelement("nav")}})、`role="banner"` (document {{htmlelement("header")}})、`role="complementary"` ({{htmlelement("aside")}})、`role="search"` ({{htmlelement("search")}}) などです。 しかし、それ以外にも別のページ構造を定義するロールもあります。 例えば、 `role="tablist"`、`role="tabpanel"` 等で、 UI に多く見られます。
- プロパティ (Property)
  - : これは要素の性質を定義するものであり、追加の意図や意味論を与えるために使用することができます。 例えば、 `aria-required="true"` はフォーム入力が有効となるために値を入力しなければならないことを定義し、 `aria-labelledby="label"` は、要素に ID を追加することで、複数の場合も含めてページ内の他のどの要素からもラベルとして参照することを可能にします。 これは `<label for="input">` ではできません。 別の例として、 `aria-labelledby` を使って主な説明を含む 1 つの {{htmlelement("div")}} が表の複数セルのラベルであると指定することができますし、画像の代替テキストの代わりとして使用することもできます。 これは、 `alt` 属性で繰り返すのではなく、ページの既存の情報を画像の代替テキストとして指定します。 [代替テキスト](/ja/docs/Learn_web_development/Core/Accessibility/HTML#代替テキスト) で例を確認することができます。
- 状態 (State)
  - : 要素の現在の状態を定義する特別なプロパティです。 例えば、 `aria-disabled="true"` は、フォーム入力が現在 disabled であることをスクリーンリーダーに対して伝えます。状態はプロパティとは異なり、プロパティはアプリのライフサイクルを通して変化しないのに対して、状態は主に JavaScript によってプログラムから変更されます。

WAI-ARIA 属性の重要な点は、ブラウザーのアクセシビリティ API（スクリーンリーダーはここから情報を取得する）によって提供される情報を除いて、それらはウェブページに何の影響も与えないという点です。 WAI-ARIA はウェブページの構造や DOM に影響を与えませんが、 CSS の要素選択で利用することが可能です。

> [!NOTE]
> ARIA ロールの使用方法と追加情報へのリンクを含む便利なリストを確認することができます。WAI-ARIA の仕様書では[ロールの定義](https://www.w3.org/TR/wai-aria-1.1/#role_definitions)（英語）を、このサイトでは [ARIA ロール](/ja/docs/Web/Accessibility/ARIA/Reference/Roles)を参照してください。
>
> この仕様では、プロパティと状態の追加情報を含んだリストも確認することができます。 [状態とプロパティの定義（すべての `aria-*` 属性）](https://www.w3.org/TR/wai-aria-1.1/#state_prop_def)（英語）を見てください。

### WAI-ARIA はどこで対応されているか

この質問に答えるのは簡単ではありません。 次の理由より、どこで、WAI-ARIA のどの機能が対応されているのかを記述する決定的なリソースを見つけることが難しいためです。

1. WAI-ARIA には大量の機能がある。
2. 検討しなければいけないオペレーティングシステム、ブラウザー、スクリーンリーダーの組み合わせが大量にある。

最後の点は重要です。 そもそもスクリーンリーダーを使用するためには、オペレーティングシステムが所定のアクセシビリティ API を持つブラウザーを動作させる必要があり、それはスクリーンリーダーが動作するために必要となる情報を提供しなければいけません。 ほとんどの人気の OS は、スクリーンリーダーが動作可能である 1 つか 2 つの所定のブラウザーを持っています。 Paciello Group は、この件に関してほぼ最新のデータを投稿しています — [ラフガイド: ブラウザー、オペレーティングシステム、スクリーンリーダーのサポート](https://www.tpgi.com/rough-guide-browsers-operating-systems-and-screen-reader-support-updated/)（英語）を見てください。

次に、ブラウザーが問題となっている ARIA の機能に対応しているのか、および API を通してそれらを公開しているのかという点を気にする必要があります。 しかし、スクリーンリーダーがそれらの情報を認識し、ユーザーに有益なやり方で伝えているのかという点もまた気にしなければいけません。

1. ブラウザーはほぼすべてが対応しています。
2. スクリーンリーダーの ARIA の対応状況はそこまでではありませんが、多くの一般的なスクリーンリーダーはそれに近いものになってきています。 Powermapper による [WAI-ARIA のスクリーンリーダーの互換性](http://www.powermapper.com/tests/screen-readers/aria/)（英語）の記事で、サポート状況を確認することができます。

この記事では、全ての WAI-ARIA の機能と詳細についてカバーするわけではありません。 代わりに、あなたが知るべき最も重要な WAI-ARIA の機能についてカバーします。 もしサポートの詳細について何も記述してしない場合は、その機能が十分にサポートされていると想定してください。 この例外がある場合は、明確に記述します。

> [!NOTE]
> JavaScript ライブラリーには WAI-ARIA をサポートしているものがありますが、それはライブラリーが複雑なフォームコントロールのような UI を生成した場合に、アクセシビリティを向上させるための ARIA 属性を追加することを意味します。 迅速な UI 開発のためにサードパーティーの JavaScript ライブラリーを探しているのであれば、その決断を下す際、UI のアクセシビリティのサポートを重要な要素として必ず考慮すべきです。 良い例としては、 jQuery UI（[jQuery UI について: ディープアクセシビリティサポート](https://jqueryui.com/about/#deep-accessibility-support)（英語）を見てください）、 [ExtJS](https://www.sencha.com/products/extjs/) 、 [Dojo/Dijit](https://dojotoolkit.org/reference-guide/1.10/dijit/a11y/statement.html) があります。

### いつ WAI-ARIA を使うべきか

私達は WAI-ARIA が作られるに至ったいくつかの問題について最初の方で話しましたが、基本的には WAI-ARIA が有用となる 4 つの主な場面があります。

- 道しるべ/ランドマーク (Signpost/Landmark)
  - : ARIA の `role` 属性の値は、HTML 要素の意味論（例えば {{htmlelement("nav")}}）を再現するランドマークとして振る舞ったり、 `search`、`tablist`、`tab`、`listbox` のように HTML の意味論の範囲外となる道しるべ (signpost) を異なる機能領域に提供することができます。
- 動的なコンテンツの更新
  - : スクリーンリーダーは、絶えず更新されるコンテンツが得意ではない傾向があります。例えば、ARIA の `aria-live` を使うことで、コンテンツが更新された場合、例えば、ページの JavaScript で [サーバーから新しいコンテンツを取得して DOM を更新](/ja/docs/Learn_web_development/Core/Scripting/Network_requests)した場合に、スクリーンリーダーのユーザーに対してそれを伝えることができます。
- キーボードのアクセシビリティの向上
  - : キーボードのアクセシビリティを最初から持つ HTML 要素がありますが、 JavaScript を使ってそれ以外の要素に同じようなインタラクションをさせる場合、スクリーンリーダーにとって困難が生じます。 こうしなければならない場合、 WAI-ARIA は他の要素に対してフォーカスを得る手段を提供しています（`tabindex` の使用）。
- 意味論的ではないコントロールのアクセシビリティ
  - : ネストした一連の `<div>` が CSS/JavaScript と共に複雑な UI 機能を構成していたり、ネイティブのコントロールが JavaScript によって大きく強化/変更されている場合、アクセシビリティの提供は困難になります。 そこに意味論や手がかりが無ければ、スクリーンリーダーのユーザーはその機能が何をするのか判断するのが難しくなるでしょう。 このような状況では、 `button`、`listbox`、`tablist` といったロールの組み合わせ、もしくは `aria-required` や `aria-posinset` などのプロパティにより機能の手がかりを提供することで、 ARIA は足りないものを補うことができます。

#### WAI-ARIA は必要な場合のみ使用する

正しい HTML 要素を使用することで、必要なロールが暗黙的に与えられますので、スクリーンリーダーのユーザーの理解に必要となる意味論の提供は、常に [ネイティブの HTML 機能](/ja/docs/Learn_web_development/Core/Accessibility/HTML) を使用して行うべきです。 しかし、コードの制御が限定されていたり、 HTML 要素への実装が容易ではない複雑なものを作っているなどの理由で、これが困難となるケースがあります。 そのような場合、 WAI-ARIA はアクセシビリティを向上させる上で価値のあるツールとなります。

もう一度言いますが、必要な時だけ使ってください。

> [!NOTE]
> 実際のさまざまなユーザーによってあなたのサイトをテストすることも忘れないでください — 障害のないユーザー、スクリーンリーダーを使用するユーザー、キーボードナビゲーションを使用するユーザーなどです。 どれだけうまく動作するかという点で、彼らはあなたよりもうまく観察してくれるでしょう。

## 実践的な WAI-ARIA の実装

次の節では、実際の例と共に、より詳細な 4 つの領域を見てみます。 続行する前に、これから見る例をテストできるように、スクリーンリーダーのテスト環境を用意してください。

詳細は[スクリーンリーダーのテスト](/ja/docs/Learn_web_development/Core/Accessibility/Tooling#スクリーンリーダー)を確認してください。

### 道しるべ/ランドマーク

WAI-ARIA は [`role` 属性](https://www.w3.org/TR/wai-aria-1.1/#role_definitions)（英語）をブラウザーに追加することで、必要に応じてあなたのサイトに付加的な意味論的価値を追加することができます。 これが有用となる最初の領域は、スクリーンリーダーのユーザーが共通のページ要素を見つけることができるようになる情報の提供です。この例は次のような構造です。

```html live-sample___aria-website-no-roles
<header>
  <h1>ヘッダー</h1>

  <!-- 必須ではありませんが、メインヘッダー内にメインナビゲーションメニューを配置するのが一般的です。 -->

  <nav>
    <ul>
      <li><a href="#">Home</a></li>
      <li><a href="#">Team</a></li>
      <li><a href="#">Projects</a></li>
      <li><a href="#">Contact</a></li>
    </ul>

    <!-- 検索フォームは、ウェブサイトをナビゲートする非線形の一般的な方法の 1 つです。 -->

    <form>
      <input type="search" name="q" placeholder="Search query" />
      <input type="submit" value="Go!" />
    </form>
  </nav>
</header>

<!-- こちらがページのメインコンテンツです。 -->
<main>
  <!-- この中には記事があります。 -->
  <article>
    <h2>記事の見出し</h2>

    <p>
      Lorem ipsum dolor sit amet, consectetur adipisicing elit. Donec a diam
      lectus. Set sit amet ipsum mauris. Maecenas congue ligula as quam viverra
      nec consectetur ant hendrerit. Donec et mollis dolor. Praesent et diam
      eget libero egestas mattis sit amet vitae augue. Nam tincidunt congue
      enim, ut porta lorem lacinia consectetur.
    </p>

    <h3>副見出し</h3>

    <p>
      Donec ut librero sed accu vehicula ultricies a non tortor. Lorem ipsum
      dolor sit amet, consectetur adipisicing elit. Aenean ut gravida lorem. Ut
      turpis felis, pulvinar a semper sed, adipiscing id dolor.
    </p>
  </article>

  <!-- 補助的なコンテンツは、本文コンテンツ内にさらに入れ子にすることもできます。 -->
  <aside>
    <h2>関連情報</h2>

    <ul>
      <li><a href="#">Oh I do like to be beside the seaside</a></li>
      <li><a href="#">Oh I do like to be beside the sea</a></li>
      <li><a href="#">Although in the North of England</a></li>
      <li><a href="#">It never stops raining</a></li>
      <li><a href="#">Oh well...</a></li>
    </ul>
  </aside>
</main>

<!-- そして、これがこのウェブサイトのすべてのページで使用されているメインフッターです。 -->

<footer>
  <p>©Copyright 2050 by nobody. All rights reversed.</p>
</footer>
```

```css hidden live-sample___aria-website-no-roles
/* || 全般的なセットアップ */

html,
body {
  margin: 0;
  padding: 0;
}

html {
  font-size: 10px;
  background-color: #a9a9a9;
}

body {
  width: max(70vw, 90%);
  margin: 0 auto;
  padding: 0 10px;
  display: flex;
  flex-direction: column;
}

/* || 書体 */

h1,
h2,
h3 {
  font-family: "Sonsie One", cursive;
  color: #2a2a2a;
}

p,
input,
li {
  font-family: "Open Sans Condensed", sans-serif;
  color: #2a2a2a;
}

h1 {
  font-size: 4rem;
  text-align: center;
  color: white;
  text-shadow: 2px 2px 10px black;
}

h2 {
  font-size: 3rem;
  text-align: center;
}

h3 {
  font-size: 2.2rem;
}

p,
li {
  font-size: 1.6rem;
  line-height: 1.5;
}

/* || ヘッダーのレイアウト */

header {
  margin-bottom: 10px;
}

nav,
article,
aside,
footer {
  background-color: white;
  padding: 1%;
}

nav {
  background-color: ff80ff;
  display: flex;
  gap: 2vw;
  @media (width <= 650px) {
    flex-direction: column;
  }
}

nav ul {
  padding: 0;
  list-style-type: none;
  flex: 2;
  display: flex;
  gap: 2vw;
}

nav li {
  display: inline;
  text-align: center;
}

nav a {
  display: inline-block;
  font-size: 2rem;
  text-transform: uppercase;
  text-decoration: none;
  color: black;
}

nav form {
  flex: 1;
  display: flex;
  align-items: center;
  height: 100%;
}

input {
  font-size: 1.6rem;
  height: 32px;
}

input[type="search"] {
  flex: 3;
}

input[type="submit"] {
  flex: 1;
  margin-left: 1rem;
  background: #333;
  border: 0;
  color: white;
}

/* || メインレイアウト */

main {
  display: flex;
  gap: 2vw;
  @media (width <= 650px) {
    flex-direction: column;
  }
}

article {
  flex: 4;
}

aside {
  flex: 1;
  background-color: #ff80ff;
}

aside li {
  padding-bottom: 10px;
}

footer {
  margin-top: 10px;
}
```

{{EmbedLiveSample("aria-website-no-roles", "100", "850")}}

もしあなたが現代のブラウザーでスクリーンリーダーのテストをした場合、いくつかの有用な情報が手に入るでしょう。 例えば、 VoiceOver は次の内容を読み上げます。

- `<header>` 要素の上 — "banner, 2 items"（見出しと `<nav>` を含んでいる）
- `<nav>` 要素の上 — "navigation 2 items"（リストとフォームを含む）
- `<main>` 要素の上 — "main 2 items"（記事（article）と余談（aside）を含む）
- `<aside>` 要素の上 — "complementary 2 items"（見出しとリストを含む）
- 検索フォーム入力の上 — "Search query, insertion at beginning of text"
- `<footer>` 要素の上 — "footer 1 item"

VoiceOver の Landmarks メニューを見ると（VoiceOver キー + U でアクセスし、矢印キーでメニューを選択する）、多くの要素が素早くアクセスできるように綺麗に並んでいることが確認できます。

![Mac の VoiceOver によるクイックアクセシビリティのためのメニュー。バナー、ナビゲーション、メイン、補完を含むランドマークヘッダーとランドマークリスト。](landmarks-list.png)

しかし、これは改善することができます。 検索フォームはユーザーが見つけたいと考える重要なランドマークですが、ここでは Landmarks メニューの中に列挙されておらず、検索入力（`<input type="search">`）であるということ以上に目立つランドマークとしても扱われていません。

ARIA の `role="search"` を使用することで改善することができますが、 {{htmlelement("search")}} 要素を使用すると、そのロールが暗黙的にフォームに与えられてしまいます。

```html live-sample___aria-website-roles
<header>
  <h1>ヘッダー</h1>

  <!-- 必須ではありませんが、メインヘッダー内にメインナビゲーションメニューを配置するのが一般的です。 -->

  <nav>
    <ul>
      <li><a href="#">Home</a></li>
      <li><a href="#">Our team</a></li>
      <li><a href="#">Projects</a></li>
      <li><a href="#">Contact</a></li>
    </ul>

    <!-- 検索フォームは、ウェブサイトをナビゲートする非線形の一般的な方法の 1 つです。 -->

    <search>
      <form>
        <input
          type="search"
          name="q"
          placeholder="Search query"
          aria-label="Search through site content" />
        <input type="submit" value="Go!" />
      </form>
    </search>
  </nav>
</header>

<!-- こちらがページのメインコンテンツです。 -->
<main>
  <!-- この中には記事があります。 -->
  <article>
    <h2>記事の見出し</h2>

    <p>
      Lorem ipsum dolor sit amet, consectetur adipisicing elit. Donec a diam
      lectus. Set sit amet ipsum mauris. Maecenas congue ligula as quam viverra
      nec consectetur ant hendrerit. Donec et mollis dolor. Praesent et diam
      eget libero egestas mattis sit amet vitae augue. Nam tincidunt congue
      enim, ut porta lorem lacinia consectetur.
    </p>

    <h3>副見出し</h3>

    <p>
      Donec ut librero sed accu vehicula ultricies a non tortor. Lorem ipsum
      dolor sit amet, consectetur adipisicing elit. Aenean ut gravida lorem. Ut
      turpis felis, pulvinar a semper sed, adipiscing id dolor.
    </p>

    <p>
      Pelientesque auctor nisi id magna consequat sagittis. Curabitur dapibus,
      enim sit amet elit pharetra tincidunt feugiat nist imperdiet. Ut convallis
      libero in urna ultrices accumsan. Donec sed odio eros.
    </p>
  </article>

  <!-- 補助的なコンテンツは、本文コンテンツ内にさらに入れ子にすることもできます。 -->
  <aside>
    <h2>関連情報</h2>
    <ul>
      <li><a href="#">Oh I do like to be beside the seaside</a></li>
      <li><a href="#">Oh I do like to be beside the sea</a></li>
      <li><a href="#">Although in the North of England</a></li>
      <li><a href="#">It never stops raining</a></li>
      <li><a href="#">Oh well...</a></li>
    </ul>
  </aside>
</main>

<!-- そして、これがこのウェブサイトのすべてのページで使用されているメインフッターです。 -->

<footer>
  <p>©Copyright 2050 by nobody. All rights reversed.</p>
</footer>
```

```css hidden live-sample___aria-website-roles
/* || 全般的なセットアップ */

html,
body {
  margin: 0;
  padding: 0;
}

html {
  font-size: 10px;
  background-color: #a9a9a9;
}

body {
  width: max(70vw, 90%);
  margin: 0 auto;
  padding: 0 10px;
  display: flex;
  flex-direction: column;
}

/* || 書体 */

h1,
h2,
h3 {
  font-family: "Sonsie One", cursive;
  color: #2a2a2a;
}

p,
input,
li {
  font-family: "Open Sans Condensed", sans-serif;
  color: #2a2a2a;
}

h1 {
  font-size: 4rem;
  text-align: center;
  color: white;
  text-shadow: 2px 2px 10px black;
}

h2 {
  font-size: 3rem;
  text-align: center;
}

h3 {
  font-size: 2.2rem;
}

p,
li {
  font-size: 1.6rem;
  line-height: 1.5;
}

/* || ヘッダーのレイアウト */

header {
  margin-bottom: 10px;
}

nav,
article,
aside,
footer {
  background-color: white;
  padding: 1%;
}

nav {
  background-color: ff80ff;
  display: flex;
  gap: 2vw;
  @media (width <= 650px) {
    flex-direction: column;
  }
}

nav ul {
  padding: 0;
  list-style-type: none;
  flex: 2;
  display: flex;
  gap: 2vw;
}

nav li {
  display: inline;
  text-align: center;
}

nav a {
  display: inline-block;
  font-size: 2rem;
  text-transform: uppercase;
  text-decoration: none;
  color: black;
}

nav form {
  flex: 1;
  display: flex;
  align-items: center;
  height: 100%;
}

input {
  font-size: 1.6rem;
  height: 32px;
}

input[type="search"] {
  flex: 3;
}

input[type="submit"] {
  flex: 1;
  margin-left: 1rem;
  background: #333;
  border: 0;
  color: white;
}

/* || メインレイアウト */

main {
  display: flex;
  gap: 2vw;
  @media (width <= 650px) {
    flex-direction: column;
  }
}

article {
  flex: 4;
}

aside {
  flex: 1;
  background-color: #ff80ff;
}

aside li {
  padding-bottom: 10px;
}

footer {
  margin-top: 10px;
}
```

{{EmbedLiveSample("aria-website-roles", "100", "850")}}

最も重要なのは、ページの構造に意味と役割を与える意味づけ HTML を使用しているということです。 HTML 構造に不必要な [`role`](/ja/docs/Web/Accessibility/ARIA/Reference/Roles) 属性を追加することなく、次のような構造になります。

```html
<header>
  <h1>…</h1>
  <nav>
    <ul>
      …
    </ul>
    <search>
      <form>
        <!-- 検索フォーム -->
      </form>
    </search>
  </nav>
</header>

<main>
  <article>…</article>
  <aside>…</aside>
</main>

<footer>…</footer>
```

この例では、ボーナス機能も提供しています — {{htmlelement("input")}} は [`aria-label`](/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) 属性が設定されていますが、これは {{htmlelement("label")}} 要素が含まれていない場合でもスクリーンリーダーによって読み上げられる説明ラベルを設定します。 こういうケースでは、この機能はとても便利です。このような検索フォームはよくあるものであり、簡単に識別できるので、見えるラベルを設定するとデザインを台無しにしてしまうのです。

```html
<input
  type="search"
  name="q"
  placeholder="Search query"
  aria-label="Search through site content" />
```

さて、この例を VoiceOver で見た時、次の改善を確認することができます。

- ページを閲覧した時と、 Landmarks メニューで見た時の両方において、検索フォームが分離したアイテムとして認識される。
- フォーム入力がハイライトされた時、 `aria-label` に含まれているテキストが読み上げられる。

IE8 などの古いブラウザーに対応する必要がある場合は、その目的のためだけにARIAロールを記載する価値があります。そして、もしあなたのサイトが何らかの理由により `<div>` のみで構成されているなら、必要な意味論を提供するために確実に ARIA ロールを含めるべきです。

以下の記事、特に [意味論的でないコントロールのアクセシビリティ](#意味論的でないコントロールのアクセシビリティ) の節で、より多くの意味論や ARIA のプロパティ/属性が持つ力を見ることができます。 まずは、 ARIA が動的コンテンツの更新をどのように助けるかを見てみましょう。

### 動的なコンテンツの更新

DOM に読み込まれたコンテンツは、テキストコンテンツから画像に添付された代替テキストまで、スクリーンリーダーを使用して簡単にアクセスすることができます。そのため、テキストコンテンツが大部分を占める従来の静的ウェブサイトは、視覚障碍者のアクセシビリティを確保しやすくなっています。

問題は、現行のウェブアプリケーションは静的なテキストだけでないことがよくあります。サーバーから新しいコンテンツを取得し（この例では静的な引用文の配列を使用しています）、 DOM を更新することで、ページの一部を頻繁に更新することがよくあります。これらは**ライブリージョン** (live region) と呼ばれることがあります。

```html live-sample___aria-no-live
<section>
  <h1>ランダムな引用</h1>
  <blockquote>
    <p></p>
  </blockquote>
</section>
```

```css live-sample___aria-no-live
html {
  font-family: sans-serif;
}

h1 {
  letter-spacing: 2px;
}

p {
  line-height: 1.6;
}

section {
  padding: 10px;
  width: calc(100% - 20px);
  background: #666;
  text-shadow: 1px 1px 1px black;
  color: white;
  min-height: 160px;
}
```

```js live-sample___aria-no-live
let quotes = [
  {
    quote:
      "Every child is an artist. The problem is how to remain an artist once he grows up.",
    author: "Pablo Picasso",
  },
  {
    quote:
      "You can never cross the ocean until you have the courage to lose sight of the shore.",
    author: "Christopher Columbus",
  },
  {
    quote:
      "I love deadlines. I love the whooshing noise they make as they go by.",
    author: "Douglas Adams",
  },
];
```

```js live-sample___aria-no-live
const quotePara = document.querySelector("section p");

window.setInterval(showQuote, 10000);

function showQuote() {
  let random = Math.floor(Math.random() * quotes.length);
  quotePara.textContent = `${quotes[random].quote} -- ${quotes[random].author}`;
}
```

{{EmbedLiveSample("aria-no-live", "100", "180")}}

これは正しく動作しますが、アクセシビリティとしてはよくありません。 コンテンツの更新がスクリーンリーダーに検知されないため、ユーザーは何が起こっているかを知ることができないからです。 これはつまらない例ですが、更新され続けるコンテンツを多く含む複雑な UI をあなたが作ることを想像してください（チャットルームや戦略ゲームの UI、動的に更新されるショッピングカートの表示など）。 ユーザーに更新を知らせる何かしらの手段がない限り、それがどんなに実用的なアプリであっても使いこなすことはできないでしょう。

幸いなことに WAI-ARIA はそのような通知を行う便利な機構を提供しています。 それが [`aria-live`](/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-live) プロパティです。 このプロパティを要素に適用すると、スクリーンリーダーが更新されたコンテンツを読み上げてくれるようになります。 どのような緊急性をもってコンテンツを読み上げてくれるかは、次のように属性値によって変わります。

- `off`
  - : 既定値。更新は通知されるべきではない。
- `polite`
  - : 更新はユーザーが暇になったときのみ通知されるべきである。
- `assertive`
  - : 更新は可能な限り早くユーザーに通知されるべきである。

ここで、 `<section>` 開始タグの内容を次のように更新します。

```html
<section aria-live="assertive">…</section>
```

これにより、コンテンツの更新があった際にスクリーンリーダーがその内容を読み上げてくれるようになります。

加えて、考慮すべきことがあります。 テキストの更新された部分だけを読み上げるべきかどうかです。 常に見出し全体を読み上げる方が、何を読み上げられているかをユーザーが認識できるという点で望ましいかもしれません。 その対象に [`aria-atomic`](/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-atomic) プロパティを追加することで、このような動作を得ることができます。 手元の `<section>` の開始タグを再度更新して、次のようにしてください。

```html
<section aria-live="assertive" aria-atomic="true">…</section>
```

この `aria-atomic="true"` 属性が、更新された一部分だけではなく、要素全体のコンテンツを 1 つのまとまりとして読み上げるようスクリーンリーダーに伝えます。

```html live-sample___aria-live
<section aria-live="assertive" aria-atomic="true">
  <h1>ランダムな引用</h1>
  <blockquote>
    <p></p>
  </blockquote>
</section>
```

```css live-sample___aria-live
html {
  font-family: sans-serif;
}

h1 {
  letter-spacing: 2px;
}

p {
  line-height: 1.6;
}

section {
  padding: 10px;
  width: calc(100% - 20px);
  background: #666;
  text-shadow: 1px 1px 1px black;
  color: white;
  min-height: 160px;
}
```

```js live-sample___aria-live
let quotes = [
  {
    quote:
      "Every child is an artist. The problem is how to remain an artist once he grows up.",
    author: "Pablo Picasso",
  },
  {
    quote:
      "You can never cross the ocean until you have the courage to lose sight of the shore.",
    author: "Christopher Columbus",
  },
  {
    quote:
      "I love deadlines. I love the whooshing noise they make as they go by.",
    author: "Douglas Adams",
  },
];
```

```js live-sample___aria-live
const quotePara = document.querySelector("section p");

window.setInterval(showQuote, 10000);

function showQuote() {
  let random = Math.floor(Math.random() * quotes.length);
  quotePara.textContent = `${quotes[random].quote} -- ${quotes[random].author}`;
}
```

{{EmbedLiveSample("aria-live", "100", "180")}}

> [!NOTE]
> [`aria-relevant`](/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-relevant) プロパティは、ライブリージョンが更新された際に何が読み上げられるかを制御するのにとても役に立ちます。 例えば、追加や削除をされた内容だけを読み上げさせることもできます。

### キーボードでのアクセシビリティの拡張

このモジュールの他の箇所でも何度か言及したように、アクセシビリティという観点での HTML がもつ重要な能力の 1 つが、キーボードでのアクセシビリティが組み込まれていることです。 キーボードから、ボタンやフォームコントロール、リンクなどの機能にアクセスできます。 一般的に、タブキーでコントロール間を移動したり、エンター/リターンキーでコントロールの選択や活性化をしたり、必要に応じたその他の制御（例えば、上下矢印キーでの `<select>` ボックス内のオプション間の移動）ができます。

しかし、時には（ボタンや他のタイプのコントロールとして）意味論的でない要素や正しい用途ではないフォーカス可能な要素を利用するコードを書かざるをえないこともあるでしょう。 あなたが引き継いだ作りの悪いコードを修正したり、そのようなコードを必要とする複雑なウィジェットを作ったりする場合があるかもしれません。

フォーカスできないコードをフォーカス可能にするために、WAI-ARIA では `tabindex` 属性を拡張して次のようにいくつかの値を加えています。

- `tabindex="0"` — 上で示したように、この値は通常タブキーでの移動対象とならない要素をタブ移動可能にします。 この値は `tabindex` の値の中で最も便利なものです。
- `tabindex="-1"` — この値は通常タブキーでの移動対象とならない要素がプログラム的にフォーカスを受け付けられるようにします。 例えば、JavaScript を利用したりリンクのターゲットとしてフォーカスするケースです。

より詳細な議論や典型的な実装例については、HTML のアクセシビリティに関する記事 — [キーボードアクセシビリティを呼び戻すように盛り込む](/ja/docs/Learn_web_development/Core/Accessibility/HTML#キーボードアクセシビリティを呼び戻すように盛り込む) をご覧ください。

### 意味論的でないコントロールのアクセシビリティ

この節の内容は前節の続きです。 多くの入れ子になった `<div>` 要素と CSS/JavaScript を利用して複雑な UI 機能を構築した場合、また、JavaScript で本来のコントロールの機能を拡張/改変した場合を考えてみましょう。 そのようなときには、キーボードでのアクセシビリティが損なわれるだけでなく、スクリーンリーダーのユーザーが各機能のふるまいを理解することは何らかの意味論や手がかりがない限り困難となってしまう。 そのような状況においても、ARIA はそこにあるべき意味論を補足する手助けができます。

#### フォーム検査とエラー警告

まず、CSS と JavaScript のアクセシビリティ の記事で最初に見たフォームの例を再検討しましょう（完全なまとめを再び見るには、[ひかえめに保つこと](/ja/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#ひかえめに保つこと)をお読みください）。 フォームを送信しようとした際に検査エラーがあると現れるエラーメッセージボックスにいくつかの ARIA 属性を含めたことを、そのセクションの末尾で示しておきました。

```html
<div class="errors" role="alert" aria-relevant="all">
  <ul></ul>
</div>
```

- [`role="alert"`](/ja/docs/Web/Accessibility/ARIA/Reference/Roles/alert_role) は、適用先の要素を自動的にライブリージョンにします。 すると、その要素に対する変更は読み上げられます。 また、その要素が警告メッセージ（重要であり、時間 / コンテキストの影響を受ける情報）なのだ、と意味論的に特定しています。 かつ、ユーザーに警告を伝える、よりアクセシビリティに優れたより良い方法も表現しています（[`alert()`](/ja/docs/Web/API/Window/alert) の呼び出しのようなモーダルダイアログには、いくつものアクセシビリティの問題があります。 WebAIM による [ポップアップウィンドウ](https://webaim.org/techniques/javascript/other#popups)（英語）を参照）。
- [`aria-relevant`](/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-relevant) の `all` という値は、エラーリストに対して何らかの変更がなされた際には（つまり、エラーが追加または削除された際には）エラーリストの中身を読み上げるよう、スクリーンリーダーに命令するものです。これは有用です。 なぜなら、ユーザーは、リストに何が追加され、リストから何が削除されたのかを知りたいだけでなく、何のエラーが残っているのかを知りたいでしょうから。

ARIA を使用して、更に先へ踏み込むこともできるでしょうし、なんらかの検査の手助けを更に提供することもできるでしょう。 そもそもフィールドが必須かどうかを示すことや、年齢がどの範囲にあるべきかを示すこと、などはいかがでしょうか？

1. いまの時点で、[`form-validation.html`](https://github.com/mdn/learning-area/blob/main/accessibility/css/form-validation.html) と [`validation.js`](https://github.com/mdn/learning-area/blob/main/accessibility/css/validation.js) のファイルのコピーをとり、それをローカル・ディレクトリーに保存してください。
2. 両ファイルをテキストエディターで開き、コードがどのように動くのかを見てください。
3. まず始めに、`<form>` 開始タグのすぐ上に次のような段落を加えるとともに、フォームの `<label>` には、両方ともアスタリスクの印をつけてください。 これは、晴眼者ユーザー用に必須フィールドに印をつける通常の方法です。

   ```html
   <p>アスタリスク（*）が付いているフィールドは必須です。</p>
   ```

4. これは視覚的に意味をなしますが、スクリーンリーダーのユーザーにとっては、理解するのがそれほど容易ではありません。 さいわい、WAI-ARIA には、フォーム入力欄を埋める必要があることをユーザーに伝えるべきだとスクリーンリーダーにヒントを与えるための、[`aria-required`](/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-required) 属性があります。 `<input>` 要素を次のように更新してください。

   ```html
   <input type="text" name="name" id="name" aria-required="true" />

   <input type="number" name="age" id="age" aria-required="true" />
   ```

5. この例をここで保存してスクリーンリーダーでテストしてみれば、「Enter your name star, required, edit text（名前を入れてください　星、必須、テキストを編集）」のようなものを聞くことになるはずです。
6. 年齢の値がどうあるべきかについて、スクリーンリーダーのユーザーと晴眼者のユーザーに知らせるのも、有用かもしれません。 これはツールチップとして提示されることがよくあり、あるいは、フォームのフィールド内部のプレースホルダーとして提示されることも、多分あります。 最小値と最大値を指定するための [`aria-valuemin`](/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-valuemin) プロパティと [`aria-valuemax`](/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-valuemax) プロパティを、WAI-ARIA は確かに含んではいますが、これらのプロパティは、今のところそれほどちゃんとサポートされてはいないようです。 よりちゃんとサポートされている機能は、HTML の `placeholder` 属性です。 これは、何の値も入力されていないときに入力欄の中に表示されるメッセージを含むことができ、多くのスクリーンリーダーにより読み上げられます。 数値入力欄を次のように更新してください。

   ```html
   <label for="age">Your age:</label>
   <input
     type="number"
     name="age"
     id="age"
     placeholder="Enter 1 to 150"
     required
     aria-required="true" />
   ```

すべての入力要素に、必ず {{HTMLelement('label')}} を付けてください。スクリーンリーダーの中には、プレースホルダーテキストをアナウンスするものもありますが、ほとんどはそうではありません。フォームコントロールに [`aria-label`](/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) および [`aria-labelledby`](/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby) によってアクセス可能な名前を提供するという代替手段もあります。しかし、`for` 属性を持つ `<label>` 要素は、マウスユーザーを含むすべてのユーザーに対してユーザビリティを提供するため、好ましい方法です。

> [!NOTE]
> この完成した例を、[`form-validation-updated.html`](https://mdn.github.io/learning-area/accessibility/aria/form-validation-updated.html) においてライブ版で見られます。

また、古典的な {{htmlelement("label")}} 要素以上の、ある種の先進的なフォームのラベルづけ技法も、WAI-ARIA によって可能になります。 晴眼者のユーザーに対してラベルを可視にしたくない箇所にラベルを設けるために [`aria-label`](/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) プロパティを使うことについては、すでに述べました（上記の [道しるべ/ランドマーク](#道しるべランドマーク) の節を参照）。 別のプロパティを使う別のラベルづけ技法も、いくつかあります。 例えば、`<label>` 以外の要素をラベルとして指定したいとき、または、同じラベルで複数のフォーム入力欄にラベルづけをしたいときに [`aria-labelledby`](/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby) を使うとか、別の情報をフォーム入力欄に関連づけてその情報も同様に読み上げさせたいときに [`aria-describedby`](/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-describedby) を使うとかいったものです。 より詳しくは、WebAIM の [高度なフォームのラベル付け](https://webaim.org/techniques/forms/advanced)（英語）の記事を参照してください。

その他にも、フォーム要素の状態を示すのに便利なプロパティや状態がたくさんあります。例えば、 `aria-disabled="true"` は、フォームフィールドが無効であることを示すために使用することができます。多くのブラウザーは、無効化されたフォームフィールドをスキップしてしまうため、スクリーンリーダーで読み上げることができません。場合によっては、無効化された要素が知覚されることもあるので、無効化されたフォームコントロールが実際に無効であることをスクリーンリーダーに知らせるために、この属性を含めるのは良い考えです。

もし入力欄の無効化状態が変化する可能性が高いなら、その変化が起きた時点と、その結果どうなったのかを示すことも、良い考えです。 例えば、[`form-validation-checkbox-disabled.html`](https://mdn.github.io/learning-area/accessibility/aria/form-validation-checkbox-disabled.html) のデモには、チェックされると他のフォーム入力欄への更なる情報の入力を可能とするようなチェックボックスがあります。 次のように隠しライブリージョンを設定してあります。

```html
<p class="hidden-alert" aria-live="assertive"></p>
```

これは、絶対的位置指定を使って、視界からは隠してあります。 これがチェックされたり、これのチェックが外されたりすると、このチェックボックスのチェックの結果がどうなったのかをスクリーンリーダーのユーザーに伝えるために、`aria-disabled` 状態やいくつかの視覚的表示を更新するだけでなく、この隠しライブリージョン内部のテキストも更新します。

```js
function toggleMusician(bool) {
  const instrument = formItems[formItems.length - 1];
  if (bool) {
    instrument.input.disabled = false;
    instrument.label.style.color = "#000";
    instrument.input.setAttribute("aria-disabled", "false");
    hiddenAlert.textContent =
      "Instruments played field now enabled; use it to tell us what you play.";
  } else {
    instrument.input.disabled = true;
    instrument.label.style.color = "#999";
    instrument.input.setAttribute("aria-disabled", "true");
    instrument.input.removeAttribute("aria-label");
    hiddenAlert.textContent = "Instruments played field now disabled.";
  }
}
```

#### 意味論的でないボタンをボタンとして説明

この課程の中で既に 2、3 回、ボタンやリンクやフォーム要素に本来備わったアクセシビリティ（および、ボタンやリンクやフォーム要素の外見を模倣するために他の要素を使うことの背後に隠れた、アクセシビリティの問題）について述べました（HTML アクセシビリティの記事の [UI コントロール](/ja/docs/Learn_web_development/Core/Accessibility/HTML#可能であれば意味のある_ui_コントロールを使う) と、上記の[キーボードでのアクセシビリティの拡張](#キーボードでのアクセシビリティの拡張)を参照）。
基本的には、多くの場合、`tabindex` とほんの少しの JavaScript を使えば、それほど問題を生じずにキーボードアクセシビリティを追加して呼び戻せます。

しかし、スクリーンリーダーについてはどうでしょうか？ スクリーンリーダーはそれでもまだ、そうした要素をボタンとは見なさないことでしょう。 もし [`fake-div-buttons.html`](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html) の例をスクリーンリーダーで試してみれば、見せかけのボタンは「Click me!, group（クリックしてください！、グループ）」のような語句を使って報告されるでしょうし、それは明らかに混乱を招くものです。

WAI-ARIA ロールを用いてこれを修正できます。 [`fake-div-buttons.html`](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html) のローカルコピーを作って、ボタンとしての `<div>` の各々に [`role="button"`](/ja/docs/Web/Accessibility/ARIA/Reference/Roles/button_role) と追加してください。 例えば次のようにします。

```html
<div data-message="This is from the first button" tabindex="0" role="button">
  Click me!
</div>
```

スクリーンリーダーを使ってこれを試すと、「クリックしてください！ボタン」のようなフレーズでボタンが報告されるようになります。これはずっと良いことですが、[`button` ロールのドキュメント](/ja/docs/Web/Accessibility/ARIA/Reference/Roles/button_role)で説明されているように、<kbd>Enter</kbd> やクリックイベントの処理など、ユーザーが期待するネイティブなボタン機能をすべて追加する必要があります。

> [!NOTE]
> とはいえ、可能な箇所では正しい意味的要素を使うことの方が常に良いのだ、ということを忘れないようにしてください。 もしボタンを作りたいなら、そして {{htmlelement("button")}} 要素が使えるなら、{{htmlelement("button")}} 要素を使うべきです。

#### 複雑なウィジェットを通じてユーザーを案内する

他にも、標準的な HTML で利用可能なものを超える一般的な UI 機能として、意味的でない要素構造を特定できる [ロール](/ja/docs/Web/Accessibility/ARIA/Reference/Roles) が多数存在します。例えば [`combobox`](/ja/docs/Web/Accessibility/ARIA/Reference/Roles/combobox_role)、[`slider`](/ja/docs/Web/Accessibility/ARIA/Reference/Roles/slider_role)、[`tabpanel`](/ja/docs/Web/Accessibility/ARIA/Reference/Roles/tabpanel_role)、[`tree`](/ja/docs/Web/Accessibility/ARIA/Reference/Roles/tree_role) などです。[Deque 大学コードライブラリー](https://dequeuniversity.com/library/)（英語）では、このようなコントロールをどのようにアクセシブルにできるかを示す有用な例を見ることができます。

わたしたち自身の事例を検討しましょう。 単純で、絶対的位置指定をした、タブ付きのインターフェイス（CSS と JavaScript のアクセシビリティの記事の、[ものごとを隠す](/ja/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#ものごとを隠す) を参照）へと戻りましょう。 これは、[タブ付きの情報ボックスの例](/ja/docs/Learn_web_development/Core/CSS_layout/Practical_positioning_examples#タブ付き情報ボックス)で見つかります。

```html live-sample___aria-tabbed-info-box
<section class="info-box">
  <div role="tablist" class="manual">
    <button
      id="tab-1"
      type="button"
      role="tab"
      aria-selected="true"
      aria-controls="tabpanel-1">
      <span>タブ 1</span>
    </button>
    <button
      id="tab-2"
      type="button"
      role="tab"
      aria-selected="false"
      aria-controls="tabpanel-2"
      tabindex="-1">
      <span>タブ 2</span>
    </button>
    <button
      id="tab-3"
      type="button"
      role="tab"
      aria-selected="false"
      aria-controls="tabpanel-3"
      tabindex="-1">
      <span>タブ 3</span>
    </button>
  </div>
  <div class="panels">
    <article id="tabpanel-1" role="tabpanel" aria-labelledby="tab-1">
      <h2>1 つ目のタブ</h2>
      <p>これはタブ 1 の内容で、段落のみです。</p>
    </article>
    <article
      id="tabpanel-2"
      role="tabpanel"
      aria-labelledby="tab-2"
      class="is-hidden">
      <h2>2 つ目のタブ</h2>
      <p>これはタブ 2 の内容で、段落のみです。</p>
    </article>
    <article
      id="tabpanel-3"
      role="tabpanel"
      aria-labelledby="tab-3"
      class="is-hidden">
      <h2>3 つ目のタブ</h2>
      <p>これはタブ 3 の内容で、段落とリストがあります。</p>
      <ul>
        <li>Cat</li>
        <li>Dog</li>
        <li>Horse</li>
      </ul>
    </article>
  </div>
</section>
```

```css live-sample___aria-tabbed-info-box
/* 全般的な設定 */

html {
  font-family: sans-serif;
}

* {
  box-sizing: border-box;
}

body {
  margin: 0;
}

/* info-box の設定 */

.info-box {
  width: 452px;
  height: 250px;
  margin: 1.25rem auto 0;
}

/* info-box のタブのスタイル設定 */

.info-box [role="tablist"] {
  min-width: 100%;
  display: flex;
}

.info-box [role="tab"] {
  border: none;
  background: white;
  padding: 0 1rem 0 1rem;
  line-height: 3rem;
  color: #b60000;
  font-weight: bold;
  outline: none;
}

.info-box [role="tab"]:focus span,
.info-box [role="tab"]:hover span {
  outline: 1px solid blue;
  outline-offset: 6px;
  border-radius: 4px;
}

.info-box [role="tab"][aria-selected="true"] {
  background-color: #b60000;
  color: white;
}

/* info-box パネルのスタイル設定 */

.info-box .panels {
  height: 200px;
  clear: both;
  position: relative;
}

.info-box [role="tabpanel"] {
  color: white;
  position: absolute;
  padding: 0.8rem 1.2rem;
  height: 200px;
  width: 100%;
  top: 0;
  background-color: #b60000;
  left: 0;
}

.info-box [role="tabpanel"].is-hidden {
  display: none;
}
```

```js live-sample___aria-tabbed-info-box
class TabsManual {
  constructor(groupNode) {
    this.tablistNode = groupNode;

    this.tabs = [];

    this.firstTab = null;
    this.lastTab = null;

    this.tabs = Array.from(this.tablistNode.querySelectorAll("[role=tab]"));
    this.tabpanels = [];

    for (let i = 0; i < this.tabs.length; i += 1) {
      const tab = this.tabs[i];
      const tabpanel = document.getElementById(
        tab.getAttribute("aria-controls"),
      );

      tab.tabIndex = -1;
      tab.setAttribute("aria-selected", "false");
      this.tabpanels.push(tabpanel);

      tab.addEventListener("keydown", this.onKeydown.bind(this));
      tab.addEventListener("click", this.onClick.bind(this));

      if (!this.firstTab) {
        this.firstTab = tab;
      }
      this.lastTab = tab;
    }

    this.setSelectedTab(this.firstTab);
  }

  setSelectedTab(currentTab) {
    for (let i = 0; i < this.tabs.length; i += 1) {
      const tab = this.tabs[i];
      if (currentTab === tab) {
        tab.setAttribute("aria-selected", "true");
        tab.removeAttribute("tabindex");
        this.tabpanels[i].classList.remove("is-hidden");
      } else {
        tab.setAttribute("aria-selected", "false");
        tab.tabIndex = -1;
        this.tabpanels[i].classList.add("is-hidden");
      }
    }
  }

  moveFocusToTab(currentTab) {
    currentTab.focus();
  }

  moveFocusToPreviousTab(currentTab) {
    let index;

    if (currentTab === this.firstTab) {
      this.moveFocusToTab(this.lastTab);
    } else {
      index = this.tabs.indexOf(currentTab);
      this.moveFocusToTab(this.tabs[index - 1]);
    }
  }

  moveFocusToNextTab(currentTab) {
    let index;

    if (currentTab === this.lastTab) {
      this.moveFocusToTab(this.firstTab);
    } else {
      index = this.tabs.indexOf(currentTab);
      this.moveFocusToTab(this.tabs[index + 1]);
    }
  }

  /* イベントハンドラー */

  onKeydown(event) {
    const tgt = event.currentTarget;
    let flag = false;

    switch (event.key) {
      case "ArrowLeft":
        this.moveFocusToPreviousTab(tgt);
        flag = true;
        break;

      case "ArrowRight":
        this.moveFocusToNextTab(tgt);
        flag = true;
        break;

      case "Home":
        this.moveFocusToTab(this.firstTab);
        flag = true;
        break;

      case "End":
        this.moveFocusToTab(this.lastTab);
        flag = true;
        break;

      default:
        break;
    }

    if (flag) {
      event.stopPropagation();
      event.preventDefault();
    }
  }

  // この例ではタブにボタンを使用しているため、クリックだけでなく
  // スペースキーとエンターキーでも有効になります。
  onClick(event) {
    this.setSelectedTab(event.currentTarget);
  }
}

// tablist の初期化

window.addEventListener("load", function () {
  const tablists = document.querySelectorAll("[role=tablist].manual");
  for (let i = 0; i < tablists.length; i++) {
    new TabsManual(tablists[i]);
  }
});
```

{{EmbedLiveSample("aria-tabbed-info-box", "100", "270")}}

この例では、意味的要素、aria ロール、aria 属性を組み合わせて使用しています。まず、タブとして {{htmlelement("button")}} 要素を使用しています。これにより、タブはマウスクリックまたはスペースキーやエンターキーなどのキーボード操作で選択できるようになります。

ARIA 機能は次のように使われます。

- 新たなロール — [`tablist`](/ja/docs/Web/Accessibility/ARIA/Reference/Roles/tablist_role)、[`tab`](/ja/docs/Web/Accessibility/ARIA/Reference/Roles/tab_role)、[`tabpanel`](/ja/docs/Web/Accessibility/ARIA/Reference/Roles/tabpanel_role)
  - : これらは、タブのコンテナー、タブ自体、対応するタブパネルなど、タブ付きインターフェイスの重要な領域を識別します。
- [`aria-selected`](/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-selected)
  - : 今どのタブが選択されているのかを定めます。 別のタブがユーザーにより選択されると、その別のタブ上のこの属性の値が、 JavaScript を介して更新されます。
- `tabindex="-1"`
  - : `tabindex="-1"` は、その要素をタブ順序から除外します。 ユーザーがキーボードやマウスでタブを制御できるようにするために JavaScript を使用しているため、ユーザーがタブキーを使用してボタンに移動できないようにする必要があります。
- [`aria-labelledby`](/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby)
  - : この属性は、要素（その `id` によって識別される）をラベル付けし、この例では、対応するタブまたは `<button>` によって `<article>` がラベル付けされます。
- [`aria-controls`](/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-controls)
  - : この属性は、要素を制御する要素（その `id` によって識別）を特定します。この例では、`<article>` は対応するタブまたは `<button>` によって制御されます。

`aria-hidden` を使用してタブパネルの内容を支援技術から隠すことも可能ですが、その内容にフォーカス可能なコンテンツ（リンクなど）が含まれている場合、非アクティブなパネルに aria-hidden=true が設定されていても、ユーザーはそのコンテンツにタブ移動することができます。この例では、 `aria-selected="false"` のタブに対応するタブパネルに `class="is-hidden"` を適用し、CSS を使用して `display: none;` と指定することで、非表示のコンテンツがタブで移動されないようにしています。

わたしたちの検査では、この新たな構造は、物事を全体的に改善するのに確かに役立ちました。 今や、タブはタブとして認識されます（例えば、スクリーンリーダーが「タブ」と話します）し、選択されたタブは、そのタブ名で読み上げられて「選択中」と示されますし。ユーザーは、キーボードまたはマウスでタブを移動することもできます。

## スキルテスト

この記事の終わりまで来ましたが、最も重要な情報を覚えていますか？先に進む前に、この情報が記憶されているかどうかを確認するために、さらにいくつかのテストが用意されています。 [スキルテスト: WAI-ARIA](/ja/docs/Learn_web_development/Core/Accessibility/Test_your_skills/WAI-ARIA) を参照してください。

## まとめ

本記事は、WAI-ARIA で利用可能なすべてのものを取り扱ったとは、到底、言えません。 でも、WAI-ARIA の使い方を理解するのに十分な情報をお伝えしたはずです。 また、これから出会うであろう、WAI-ARIA を必要とするような最もよくあるパターンのうちのいくつかを知るのに十分な情報も、お伝えしたはずです。

## 関連情報

- [ARIA の状態とプロパティ](/ja/docs/Web/Accessibility/ARIA/Reference/Attributes): すべての `aria-*` 属性
- [WAI-ARIA ロール](/ja/docs/Web/Accessibility/ARIA/Reference/Roles): ARIA のロールと MDN で扱うロールの分類
- [ARIA in HTML](https://www.w3.org/TR/html-aria/) (W3C): HTML の部品の各々について、どのアクセシビリティ（ARIA）の意味論がその部品に対してブラウザーにより暗黙裡に設定されるのか、そして、追加の意味論が必要な場合にはどの WAI-ARIA 機能をその部品に設定しうるのか、ということを定義するものです。
- [Deque 大学のコードライブラリー](https://dequeuniversity.com/library/) — WAI-ARIA 機能を使ってアクセス可能にしてある複雑な UI コントロールを見せてくれる、実に有用かつ実践的な例からなるライブラリーです。（英語）
- [WAI-ARIA のオーサリングプラクティス](https://www.w3.org/WAI/ARIA/apg/) — W3C によるとても詳細なデザインパターンです。異なる種類の複雑な UI コントロールを、WAI-ARIA 機能を用いてアクセス可能にしつつ、実装する方法を説明しています。（英語）

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/CSS_and_JavaScript","Learn_web_development/Core/Accessibility/Multimedia", "Learn_web_development/Core/Accessibility")}}

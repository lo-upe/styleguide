# HTML/CSS Coding Guide
for Rails project of LOUPE

## 参考
[「Google HTML/CSS Style Guide」 適当に和訳してみた](http://re-dzine.net/2012/05/google-htmlcss-style-guide/)  
[GitHubのHTML/CSS Styleguideを適当に和訳してみた](http://re-dzine.net/2012/09/github-htmlcss-styleguide/)  
[CodeGrid : SMACSSによるCSSの設計](https://app.codegrid.net/entry/smacss-1)  
[CyberAgent : メンテナブルCSS](https://www.cyberagent.co.jp/recruit/techreport/report/id=7926)  
[ちゃんとCSSを書くために - CSS/Sass設計の話](http://www.slideshare.net/hiloki/a-good-css-and-sass-architecture)  
[BEMという命名規則とSass3.3の新しい記法](http://blog.ruedap.com/2013/10/29/block-element-modifier)

## 基本
LOUPEの[Ruby Coding Guide / 基本](https://github.com/lo-upe/styleguide/blob/master/ruby.ja.md)の項目に準じる。

## コミット前に気をつけること
scssファイルを修正/追加したら、scss-lint を実行するように心がける。  
設定ファイルscss_lint.ymlは各プロジェクトに保持させたものを使う。  
scss-lintさんに理不尽な怒られ方をした場合は、scss_lint.ymlの問題かもしれないので、相談する。  

## ファイル配置
Ruby on Rails Guides に従う。
参考：[#2.2 Asset Organization](http://guides.rubyonrails.org/asset_pipeline.html#asset-organization)
* app/assets は対象アプリケーションの独自作成ファイル置き場
* lib/assets は複数アプリケーションにまたがって利用されるライブラリファイル置き場
* vendor/assets はVendor(サードパーティ)製ライブラリファイル置き場

※ app/assets 内のファイル内でさらにどのようにディレクトリ構成を行うかはCSS設計方針に準じる。

## 命名について
class名、id名、Sassの変数名等に利用する変数名はBEMをカスタマイズしたものを意識して検討する。
* blockName_elementName-modifierName
  * blockName → 大きな塊の定義名（メインページ、フォームなど）
  * _elementName → 要素名（ボタン、タイトルなど）
  * -modifierName → 状態（押した、オンマウスなど）
* 英単語は極端に省略しすぎない。第三者が閲覧した場合に理解できるかどうかを念頭において決定する。
  * 分かりやすい省略は推奨する。例：button → btn
* ただし既存定義に明確なルールがある場合は既存のものを優先する。
* 上記に捕われすぎないこと。
* 参考：[BEMを参考にしたCSS設計](http://qiita.com/mrd-takahashi/items/07dc3b4bad027daa2884)
* 参考：[実践 めんどうくさくない BEM](http://tsmd.hateblo.jp/entry/2013/12/12/004059)


## CSS設計方針
* SMACSSを意識して作成する。
  * 参考：[今必要なCSSアーキテクチャ](http://www.slideshare.net/MayuKimura/css-25547100)
  * 参考：[悩まないコーディングをしよう！ OOCSS,SMACSSを用いた、読みやすくてメンテナブルなCSS設計（Sass対応）](http://www.slideshare.net/horiguchiseito/master-architecture-forcss)
  * 参考：[SMACSS and Rails A Styleguide for the Asset Pipeline](http://blog.55minutes.com/2013/01/smacss-and-rails/)
  * また、以下に記載するコーディングルールに縛られすぎないこと。
  * 柔軟に対応することを意識する。
* app/assets/stylesheets 内は以下のようにcssファイルを配置する。
```
  app/assets/stylesheets
    |- application.css
    |- base.css.scss //量が多ければ分割を検討
    |- /globals
    |   |- _all.css.scss
    |   |- _functions.css.scss
    |   |- _mixins.css.scss
    |   |- _variables.css.scss
    |
    |- /layouts
    |   |- header.css.scss
    |   |- footer.css.scss
    |   |- ...
    |
    |- /modules
        |- button.css.scss
        |- heading.css.scss
        |- ...
```


## HTML（slim）記法について
* マルチメディアの要素には代替コンテンツを提供する(alt="説明"を書く)こと。
  * 装飾的な要素の画像にはalt=""としておくこと。

```slim
  img src="cat.png" alt="かわいい猫"
```

* 他、随時気づいた点から追記

## CSS（Sass）記法について
* プロパティ宣言の```:```の後ろにはスペースを入れる。
* ルール宣言の```{```の前にはスペースを入れる。
* すべてのプロパティの終端にはセミコロンを書く。
* 階層がわかるようにブロック単位でコードをインデントする。（SCSS）

```sass
// good
body {
  font-size: 10px;
  @media(max-width: 767px) {
    padding-left: 0;
  }
}
```

* 別々のセレクタとプロパティは改行して書く。
* 別々のCSSルールは改行して一行間を空けて書く。
* 1つのみしかプロパティを持っていないものは一行にまとめてよい。

```sass
// good
h1,
h2,
h3 {
  font-weight: normal;
  line-height: 1.2;
}

p {
  font-weight: bold;
}

hr { color: #e761a4; }

// bad
h1,h2,h3 {
  font-weight: normal; line-height: 1.2;
}
p {
  font-size: 10px;
}
```

* rgbaを使用しない限り、16進数のカラーコードを使う。
* HEX形式のカラーコードで3文字で表現できるものは3文字にする。

```sass
// good
color: #fff;
color: #e761a4;
color: rgba(0, 0, 0, 0.8);

//bad（省略できるものは省略するよう心がける）
color: #ffffff;
```

* プロパティはアルファベットの順に記述する。
* アルファベット順の際、ベンダープレフィックスは無視するが、接頭辞の中での順序は保つこと。
* ```font-size```プロパティには、```px```を使うこと。
* ```line-height```は単位なしが良い。
  * 親要素のパーセンテージ値を継承しないため。
  * 単位なしの場合、値は```font-size```の乗数に基づく。

```sass
// good
.menu {
  border: 1px solid;
  -moz-border-radius: 4px;
  -webkit-border-radius: 4px;
  display: inline-block;
  font-size: 10px;
  font-weight: normal;
  margin: 1px 0;
  padding: 5px 2px 5px 3px;
  vertical-align: middle;
  width 100%;
}
```

* コメントにTODOを記載しておくときは、TODOキーワードを用いる。

```sass
/* TODO: 中央寄せにする */
.menu {
  font-size: 10px;
}
```


## Sassの特徴的な利用方法について
* 参考：[CSS-TRICKS / Sass Style Guide](http://css-tricks.com/sass-style-guide/)

* @extend(s), @include(s), 他通常スタイルの表記順は scss-lint が通るようにする。（DeclarationOrder)

* ネストは３つまでに抑える。
  * 3つ以上になるときはやり過ぎで修正する必要があると考える。
  * SASSなので必要に応じて継承(@extend)を用いて解決する。
* ネスト表記は50行以内に抑えるようにする。
  * ネストの利点は1画面でみやすくなること、それを傷つけないようにする。

```sass
// good
.weather {
  > h3 {
    @extend %line-under;
  }
}
```

* @importの表記順は以下のようにする。
  1. Vendor製依存ライブラリ
  2. 自作依存ライブラリ
  3. 自作各Patternsの外部ファイル
  4. 自作各Sectionsの外部ファイル

```sass
/* Vendor Dependencies */
@import "compass";

/* Authored Dependencies */
@import "global/colors";
@import "global/mixins";

/* Patterns */
@import "global/tabs";
@import "global/modals";

/* Sections */
@import "global/header";
@import "global/footer";
```

* 積極的に変数を用いて定義すること。
  * 0 もしくは 100% 以外の数値を繰り返して使用する場合は変数として定義する。
  * 白もしくは黒以外の色の定義も可能な限り変数を用いる。
  * 変数の命名については上記のBEMについてを参考にする。

```sass
//数値
$zIndex-top: 5050;
$zIndex-middle: 5000;
$zIndex-bottom: 4090;
//色
$red: #d83d36;
$blue: #00859b;
$color_btn: $red;
$color_btn-pushed: darken($color_btn, 10%);

.header {
  z-index: $zIndex-bottom;
}
.overlay {
  z-index: $zIndex-top;
}
.message_btn {
  z-index: $zIndex-middle;
  background-color: $color_btn;
  &:hover {
    background-color: $color_btn-pushed;
  }
}
```

* MediaQueryについて、必要に応じて以下のような@mixinの活用を心がける。
  * 参考：[CSS-TRICKS / Naming Media Queries](http://css-tricks.com/naming-media-queries/)
  * ```$point```には抽象的な名前をつけるべきと上記参考にあるので注意。

```sass
@mixin breakpoint($point) {
  @if $point == large {
    @media (max-width: 1600px) { @content; }
  }
  @else if $point == middle {
    @media (max-width: 1250px) { @content; }
  }
  @else if $point == small {
    @media (max-width: 650px) { @content; }
  }
}

.module {
  width: 100%;
  @include breakpoint(middle) {
    width: 50%;
  }
}
```

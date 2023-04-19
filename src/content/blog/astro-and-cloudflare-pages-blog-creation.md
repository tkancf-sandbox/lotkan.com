---
title: "AstroとCloudflare Pagesでブログを作成しました"
description: "AstroとCloudflare Pagesでブログを作成しました。選定理由と公開までに実施したことを解説します。"
pubDate: "2023/04/17"
heroImage: "/placeholder-hero.webp"
---

[Astro](https://astro.build/) を利用して[ブログ](https://lotkan.com)を作成しました。ホスト先には Cloudflare Pages を利用しています。

## 選定理由

### Astro

Astro はコンテンツが豊富な Web サイトを構築するためのフレームワークです。

> **Astro was designed for building content-rich websites.** This includes most marketing sites, publishing sites, documentation sites, blogs, portfolios, and some ecommerce sites. [^1]

[^1]: Why Astro?: https://docs.astro.build/en/concepts/why-astro/#content-focused

今回はブログを作りたかったので、静的サイトジェネレータをいくつか調べていました。
Hugo は以前に利用したことがありそれ以外を検討していたのですが、Astro は以下の点に魅力を感じたので選択しました。

- デフォルトで高速
  - 実際にブログテンプレートで生成されるデフォルトのブログを Cloudflare Pages へデプロイすると、 [PageSpeed Insights](pagespeed.web.dev)で 100 点になります。
- React、Svelte などの UI フレームワークとインテグレーションが可能
- [example projects](https://astro.new/) に参考になりそうな例が豊富

### Cloudflare Pages

Astro では静的サイトが生成されるので、ホスト先の選択肢は豊富です。
GitHub Pages、Cloudflare Pages、Netlify、Vercel などがよく選ばれているようなので、その中で検討しました。

- GitHub Pages
- Netlify
  - 上記 2 つは過去に利用したことがあるので保留
  - 両サービス共、特に不満はないです
- Vercel
  - 無料プランだと[商用利用(アドセンス含む)は不可](https://vercel.com/docs/concepts/limits/fair-use-policy#commercial-usage) なので NG
    - 今後広告入れるかは未定ですが念のため
- Cloudflare Pages
  - 利用するドメインも Cloudflare で購入・管理しているので選定

## 導入から公開までやったこと

サイトは [GitHub へ公開](https://github.com/lotkan3/lotkan.com)しているのでそのコミットを貼りつつ、どんなことをしたか書いていきます。

### ブログテンプレートを生成

- コミット: https://github.com/lotkan3/lotkan.com/commit/dbe77beafc338d572998a571c78b776d1f736c63
  - 実行コマンド例

```bash
❯ npm create astro@latest -- --template blog

╭─────╮  Houston:
│ ◠ ◡ ◠  Let's make the web weird!
╰─────╯

 astro   v2.3.0 Launch sequence initiated.

   dir   Where should we create your new project?
         ./blog
      ◼  tmpl Using blog as project template
      ✔  Template copied

  deps   Install dependencies?
         Yes
      ✔  Dependencies installed

    ts   Do you plan to write TypeScript?
         Yes

   use   How strict should TypeScript be?
         Strict
      ✔  TypeScript customized

   git   Initialize a new git repository?
         Yes
      ✔  Git initialized

  next   Liftoff confirmed. Explore your project!

         Enter your project directory using cd ./blog
         Run npm run dev to start the dev server. CTRL+C to stop.
         Add frameworks like react or tailwind using astro add.

         Stuck? Join us at https://astro.build/chat

╭─────╮  Houston:
│ ◠ ◡ ◠  Good luck out there, astronaut! 🚀
╰─────╯
```

### 基本的な情報の更新

#### URL、サイトタイトルの変更

https://github.com/lotkan3/lotkan.com/commit/cd3a93f48237b2549ee7312815e4dc40c2e58913

#### コピーライト部分の変更 Twitter、GitHub アカウントの URL を自分のものに変更

Prettier のフォーマットが働いてしまって diff がでかくなってます...
https://github.com/lotkan3/lotkan.com/commit/bdef656331ff05aefab87895ce430727686fa090

#### アバウトページの更新

https://github.com/lotkan3/lotkan.com/commit/752e37368bd85536ced494f3bc682c90d06628d1

#### トップページの更新

https://github.com/lotkan3/lotkan.com/commit/2c82c6bb9b2fc213d1f4c48403f9903eae8c8739

#### 画像ファイルの更新

https://github.com/lotkan3/lotkan.com/commit/8ab7442ced4b0adaf5128839498bbe57e7e0e132

### .node-version ファイルを追加

この指定がない場合、デフォルトでは `12.18.0` が利用されるのですが、これだとビルドが失敗してしまうので
`.node-version` ファイルに Node.js のバージョンを指定しておきます。

https://github.com/lotkan3/lotkan.com/commit/5dd0af9d26fa76ee28d2fa72520434dad250da3e

### Cloudflare Pages で公開

Pages の画面から
Create a project => Connect to Git => repository の選択
などを進めていくだけのぽちぽち作業。簡単です。

### robots.txt の追加

テンプレートには robots.txt が存在していなかったので、本当に最低限のものを用意しました。
https://github.com/lotkan3/lotkan.com/commit/4554f520d66ceb9078d130e65810dd4309d5b1be

```
User-agent: *

Allow: /

Sitemap: https://example.com/sitemap-index.xml
```

### jpeg 画像を webp へ変換

jpeg 画像が重く PageSpeed Insights のパフォーマンスが 93 点になっていたため、webp へ変換しました。
https://github.com/lotkan3/lotkan.com/commit/3bb386ecca96bad3c2fd1ef0a8a18a7a601f9b4e

変換前の PageSpeed Insights の点数
![PageSpeed Insightsで93点の画像](/astro-and-cloudflare-pages-blog-creation/PageSpeed-Insights-93.webp)

変換後の PageSpeed Insights の点数

![PageSpeed Insightsで93点の画像](/astro-and-cloudflare-pages-blog-creation/PageSpeed-Insights-100.webp)

変換後は無事 100 点になりました。

## 終わりに

こんな感じで簡単に公開することが出来ました。今後デザインの変更などもやっていこうかと思っています。
ブログ以外にも Astro を利用して作ってみたいコンテンツがあるので、色々遊びたいです。
また、Cloudflare Pages を調べる中で Cloudflare Workers というのがあるのを知れたので、これも試してみようと思います。

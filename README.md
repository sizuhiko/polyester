# polyester

[![Join the chat at https://gitter.im/sizuhiko_polyester/Lobby](https://badges.gitter.im/sizuhiko_polyester/Lobby.svg)](https://gitter.im/sizuhiko_polyester/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Static Site Generator made by Polymer

とりあえず案をまとめていく

## 解決したい課題

- `middleman` や `Jekyll` などの静的サイトジェネレータでブログをやっていると、記事が追加されるたびのビルドと、どんどん遅くなるビルドがつらい
- GitHub上でマークダウンを直接編集したらサイトに反映されてほしい（どこかでビルドするとか面倒）

なので、フロントエンドでマークダウンを解釈してブログを表示できれば良いのではないだろうか。

## コンテンツの一覧

`sitemap.xml` を使って、コンテンツの一覧を生成する。
`markdown` ファイルから記事の日時の順にxml化していく。

[Google Newsサイトマップ](https://support.google.com/news/publisher/answer/74288?hl=ja)みたいに、カスタムタグで情報を増やしておきたい。
（タイトル、タグあたりが一覧表示時に有用になる）

`sitemap.xml` の最大URL数に達することはないと思うけど、年毎に分割すると良いかなーと思っている。

### 一覧表示

`iron-scroll-threshold` を使って、スクロール時の自動読み込みに対応する予定（10件ずつぐらい）

一覧の形式

- 全記事（新しい順）
- 年単位
- 月単位
- タグ単位

## マークダウンの表示

[embed.js](http://riteshkr.com/embed.js/)を使ったカスタムエレメントを作って、記事をマークダウンから表示できるようにしたい

Polymer標準の[molecules](https://elements.polymer-project.org/browse?package=molecules)は、
絵文字対応なんかが微妙なのと、埋め込みの対応が完璧とは言えないので使わない予定。

`embed.js` のオプションはタグの属性で指定できるようにしたい。

記事のヘッダー部にはYaml形式のFrontMatterを使う。
[js-yaml-front-matter](https://github.com/dworthen/js-yaml-front-matter)を使えば、フロントエンド側でコンテンツとFrontMatterをうまく解釈できそう。

## タグクラウド

[poly-cloud](https://customelements.io/RenatoUtsch/poly-cloud/)というのが見た目がかっこよくていい。
カスタムエレメント化して、sitemapのタグ情報とうまく連携できるようにしたい

## プロフィール表示

gravatarのカスタムエレメントがある

- https://customelements.io/vision89/lazy-gravatar/
- https://customelements.io/fakiolinho/gravatar-component/

githubのプロフィール表示タグがいい感じ

- https://customelements.io/kcmr/github-profile/

## SEO対策

- http://www.captaincodeman.com/app-metadata/components/app-metadata/

## Polymerのバージョン

これから作るので 2.x 系でやるつもり

## CLIツール

- markdownファイルの雛形生成
    - `middleman` や `wordpress` なんかと互換性があるフォーマットにしたい
- `sitemap.xml` の生成/更新
    - インデックスファイル
    - 実際のサイトマップ（年毎）
- `robots.txt` の生成/更新

CLIツールはPolymerとの親和性を考えると Node.js で作るのが良いだろうか？

Node.jsなら[Clite](https://github.com/remy/clite)というCLIフレームワークが良さそう

## URLルーティングとコンテンツの読み込み

### マークダウンファイル

`source/年-月-日-xxxxxxxxx.html.markdown` のような `middleman` などで用いられているファイル名規約にしたい

### URL

`http://host.domain/年/月/日/xxxxxxxxx.html` として、マークダウンを動的にロードし、`embed.js` のカスタムエレメントに流し込む

ルーティング用のカスタムエレメントが必要になると思っている。

## レイアウトやスタイル

基本的にはPolymerのテーマを[material palette](https://www.materialpalette.com/)で作って色を適用するのと、
自分で `app-layout` なんかをコーディングして勝手にできるようにしたい。

記事一覧、記事詳細、タグクラウドなんかのカスタムエレメントやCLIがジェネレータとして用意する肝な部分。

### テンプレートシステム

ただしGitHubなどでテンプレートを作って共有する仕組みは入れたい。

`polyester init` みたいなコマンドを実行したときに、スケルトンからページテンプレートを取得してくるようなイメージがいいなーと。
テンプレート指定する場合は `polyester init --template https://github.com/xxxx/polyester-template.git` みたいな

GitHubと連携するNode.jsのライブラリ
http://www.nodegit.org/

## そのほか

- カスタムドメインとかHTTPSとかは、CloudFlareとかGitHub Pagesにおまかせしたい
- ローカルでの確認：Polymer CLIでサーバー起動できる

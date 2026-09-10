# sidenote-pdf-web（サイドノート作成ツール ウェブ用Markdown作成版）

🔗 https://sidenote-pdf-web.pages.dev/

## このツールについて

- 文章に右側のサイドノート（注釈）を付け、その状態のまま **MkDocs・Docsify 等の静的サイトジェネレーター用の Markdown として書き出す** ツールです。
- 左＝本文（テキスト・画像）、右＝サイドノート、の2カラム構成です。書式は太字・下線・見出し（H1〜H3）だけを持ちます（配置・インデント・ぶら下げ・傍点は [書面レイアウト版](https://sidenote-pdf-doc.pages.dev/) の担当です）。
- 使い方（ヘルプ）は [サイドノート作成ツール](https://sidenote-pdf.pages.dev/help.html#web) に統合しています。このツール側にヘルプボタンはありません。
- **処理はすべてブラウザ内で完結し、内容はサーバーに送信されません。**
- 公開は Cloudflare Pages を利用しています。

## 姉妹ツール

- [sidenote-pdf](https://github.com/minnanosaiban/sidenote-pdf)（サイドノート作成ツール）＝注釈を付けるだけのシンプルな版
- [sidenote-pdf-doc](https://github.com/minnanosaiban/sidenote-pdf-doc)（書面レイアウト版）＝インデント・ぶら下げ・下線・傍点で書面の体裁を整える版
- [sidenote-pdf-md](https://github.com/minnanosaiban/sidenote-pdf-md)（Markdown読み込み版）＝AIが書いたMarkdownを読み込んで注釈を付ける版

## 使い方

1. 左側に本文となる文章を貼り付ける（スクリーンショットは Ctrl+V での貼り付け、または段落にカーソルを乗せて出る「＋画像」から挿入）
2. 注釈したい一文・画像を選択すると入力欄が出るので、コメントを書いて色（自分／共有相手／重要）を選ぶ
3. 書式ツールバーで太字・下線・見出し（H1〜H3）を付ける
4. 「書き出し」を押してパネルを開き、形式を選んで「フォルダへ書き出す」（サイトのフォルダを選ぶと「タイトル.md」として直接書き込みます）
5. 作業の続き・共有用には「保存」（.json）、印刷用には「PDF化」

## 書き出しの形式

- **ウェブ用（MkDocs / Docsify など）**：段落は素のMarkdownの段落、見出しは Markdown の `##`〜`####`、サイドノートは段落の直後の `<aside class="sn-note">`。書面レイアウト版で作った .json を開いた場合だけ、インデント・ぶら下げ・配置を持つ段落を `<p class="sn-p" style="…">` として出します。
- **hotline用（`:N X:` マーカー）**：自作サイト [hotline](https://github.com/minnanosaiban/hotline) の `overrides/hooks/doc_indent.py` がそのまま展開できる形式。`:1h#p1:` のようなマーカーと `<aside class="sidenote">` で出します。
- どちらも画像は書き出しに含まれません（.json保存・PDF化には含まれます）。

## サイドノート用のCSS

「ウェブ用」で書き出すとき、`sidenote.css`（サイドノートを右余白へ float させる Tufte CSS 方式・狭い画面では本文の下に流す）も一緒に書き出せます。ページ側で1回読み込んでください。

```yaml
# MkDocs（mkdocs.yml）
extra_css:
  - sidenote.css
```

```html
<!-- Docsify（index.html） -->
<link rel="stylesheet" href="sidenote.css">
```

## 保存先フォルダについて

- 初回だけフォルダ選択が出て、以後はそのフォルダへ同名で上書きします（フォルダのハンドルは IndexedDB に記憶します）。
- 「保存先を選び直す」で別のフォルダへ切り替えられます。
- File System Access API 非対応のブラウザ（Firefox 等）では、自動的にダウンロードへ切り替わります。

## 免責事項

- 本ツールは無保証で提供されます（現状有姿）。なお、コードは GitHub に公開しています。
- 処理はお使いのブラウザ内で完結し内容は外部送信されませんが、ご利用環境に起因する不具合について作成者は責任を負いません。
- 本ツールの利用により生じたいかなる損害についても作成者は責任を負いません。

## ローカルで動かす

```bash
npm install          # wrangler を入れる
npm run dev          # http://localhost:8788 で確認
```
`python -m http.server -d public 8000` でも構いません。

## 公開（Cloudflare Pages）

```bash
npm run deploy       # wrangler pages deploy
```

## 運用上の注意

- 保存した `.json`（画像を内包）には、注釈対象の資料そのものが含まれます。**個人情報・機微情報を含むファイルはコミット・pushしないでください。**
- アイコンは Bootstrap Icons（MIT License）から使う分だけSVGをそのまま埋め込んでいます。CDN・Webフォントは不使用です。詳細は [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) を参照してください。

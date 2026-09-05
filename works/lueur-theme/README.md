# Lueur — 喫茶店のための WordPress テーマ

静的HTMLで作った [Cafe Lueur のLP](https://tahara-asuka.github.io/rakulabo/works/lueur/) を、
**お店の方が自分で更新できる WordPress テーマ**に作り直したものです。

見た目は元のLPと同じまま、「文章や価格を直すのに制作者を呼ばなくていい」状態にしています。

---

## ファイルの構成と、それぞれの役割

```
lueur/
├── style.css              テーマ情報（先頭のコメント）＋ 全スタイル
├── functions.php          このテーマが何をできるかを WordPress に伝える
├── header.php             全ページ共通のヘッダー（get_header() が読む）
├── footer.php             全ページ共通のフッター（get_footer() が読む）
├── front-page.php         トップページ。各セクションを組み立てるだけ
├── index.php              最後の受け皿。テーマに必須のファイル
├── page.php               固定ページ
├── single.php             記事の個別ページ
├── 404.php                ページが見つからないとき
├── inc/
│   ├── menu-cpt.php       お品書きを管理画面から編集できるようにする
│   └── customizer.php     営業時間・住所などを編集できるようにする
├── template-parts/        トップページのセクション（6つ）
│   ├── hero.php  about.php  philosophy.php
│   └── menu.php  gallery.php  access.php
├── js/site.js             メニューの開閉・ヘッダーの切り替え
└── screenshot.jpg         管理画面のテーマ一覧に出る画像
```

WordPress は「決められた名前のファイルを、決められた順番で探して使う」仕組みです。
たとえばトップページなら `front-page.php` → 無ければ `home.php` → 無ければ `index.php`、という順に探します。
このテーマはその順番に沿ってファイルを置いています。

---

## お店の方が管理画面から直せるところ

| 直せるもの | 場所 | 仕組み |
|---|---|---|
| お品書き（品名・価格・説明・写真） | 管理画面「お品書き」 | カスタム投稿タイプ `lueur_menu` |
| Coffee / Pastry などの分類 | 「メニューの分類」 | カスタムタクソノミー `lueur_group` |
| 営業時間・住所・電話・SNSのURL | 外観 > カスタマイズ > 店舗情報 | カスタマイザー |
| ヘッダーのメニュー項目 | 外観 > メニュー | `register_nav_menus()` |
| 店名・キャッチコピー | 設定 > 一般 | `bloginfo()` |

**お品書きが1件も登録されていないときは、初期メニューをそのまま表示します。**
納品した直後にページが空白になってお客様を不安にさせない、という意図です。

---

## 作るときに気をつけたこと

- **CSSとJSは直接書かず、`wp_enqueue_style()` / `wp_enqueue_script()` で登録**
  他のプラグインと読み込み順で衝突しないようにするためです。
- **画面に出す値はすべてエスケープ**（`esc_html()` / `esc_url()` / `esc_attr()`）
  入力された文字がそのままHTMLとして動いてしまうのを防ぎます。
- **保存時は nonce と権限を確認**（`wp_verify_nonce()` / `current_user_can()`）
  外から勝手に書き換えられないようにするためです。
- **文字列は翻訳できる形（`__()` / `esc_html_e()`）で書く**
- **`ABSPATH` が定義されていなければ即終了**
  PHPファイルに直接アクセスされても動かないようにしています。

---

## 使い方

1. この `lueur` フォルダを zip に固める（または配布されている `lueur.zip` を使う）
2. WordPress 管理画面 > 外観 > テーマ > 新規追加 > テーマのアップロード
3. zip を選んでインストール → 有効化

インストールせずに試すこともできます（ブラウザだけで動く WordPress Playground）:
https://playground.wordpress.net/?blueprint-url=https://tahara-asuka.github.io/rakulabo/works/lueur-theme/blueprint.json

動作要件: WordPress 6.0 以上 / PHP 7.4 以上

---

## 写真について

初期表示の写真は [Unsplash](https://unsplash.com/) の素材を読み込んでいます。
実際のお店で使うときは、`template-parts/` 内の画像URLをお店の写真に差し替えるか、
メディアライブラリにアップロードしてアイキャッチ画像として設定してください。

---

制作: 田原 明日翔（ラクラボ） / MIT・GPL v2 or later

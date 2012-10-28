CAUTION: this project is open for only #isucon administrators

/webapp
/webapp/perl
/webapp/ruby
/webapp/nodejs

/tools
/tools/benchmark
/tools/domchecker
/tools/scoreboard

## Webアプリの基本方針
- 処理はすべてリクエストを受け取ってから実施する
 - DBへのクエリ
 - テンプレートからのレンダリング
- 全てのコンテンツをアプリケーションから渡す
 - js/css/画像も含めて
- キャッシュ等はとりあえず全て無し

## 実装するリクエストハンドラ
-  /
 -  GET
 -  articleのリスト(投稿順(id順) 最新10個)
 - - SELECT id,title,body,created_at FROM article ORDER BY id DESC LIMIT 10

-  /article/:articleid
 -  GET
 -  articleページの表示 (article + comments)
 - - SELECT id,title,body,created_at FROM article WHERE id=?
 - - SELECT name,body,created_at FROM comment WHERE article=? ORDER BY id

なお全ページ、左側のサイドバーに「新しいコメントがついた順に記事10件」を表示
 - - SELECT article FROM comment GROUP BY article ORDER BY created_at DESC LIMIT 10

-  /post
 -  GET
 -  記事投稿用HTML ただしベンチ対象外のURLとする

-  /post
 -  POST
 -  記事投稿
 -  パラメータはform形式で title, body
 - - INSERT INTO article SET ...
 -  レスポンスは / へのリダイレクト(成功)、もしくは適当なエラー用のHTTPステータス

-  /comment/:articleid
 -  POST
 -  コメント投稿 (投稿フォームは /article/:articleid の末尾)
 -  パラメータはform形式で name, body
 - - INSERT INTO comment SET ...
 -  レスポンスは /article/:articleid へのリダイレクト(成功)、もしくは適当なエラー用のHTTPステータス

## 添付するstaticファイル

-  画像
 -  isuconロゴを適当なサイズにして流用、ページトップのバナー画像にする

-  js
 -  jquery 最新版の minify 済みファイル
 -  jquery-ui 最新版の minify 済みファイル
 -  isucon.js デザイン調整用

-  css
 -  jquery-ui の適当なスタイル用のもの一式
 -  isucon.css デザイン調整用


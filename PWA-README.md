# TODO PWA — セットアップ手順

## ファイル構成
```
todo.html              アプリ本体
manifest.json          PWA マニフェスト
sw.js                  Service Worker（オフラインキャッシュ）
icon-192.png           アイコン
icon-512.png
icon-maskable-192.png  マスカブルアイコン（Android 適応アイコン用）
icon-maskable-512.png
apple-touch-icon.png   iOS ホーム画面アイコン
```
これら全部を **同じディレクトリ** に置いてください。

## 重要：HTTPS が必須
PWA（Service Worker）は `https://` または `http://localhost` でしか動きません。
`file://` で開くと Service Worker は登録されず、通常の HTML として動作します（PWA 機能は無効）。

## デプロイ方法（無料・おすすめ順）

### 1. Cloudflare Pages
1. このフォルダごと GitHub にプッシュ
2. Cloudflare Pages でリポジトリを接続
3. ビルドコマンドなし、出力ディレクトリ = ルート
4. 発行された `https://xxx.pages.dev/todo.html` を開く

### 2. GitHub Pages
1. リポジトリにファイルをプッシュ
2. Settings → Pages → Branch を main / root に設定
3. `https://<user>.github.io/<repo>/todo.html` を開く

### 3. ローカル確認（開発用）
```bash
cd このフォルダ
python3 -m http.server 8000
# ブラウザで http://localhost:8000/todo.html
```

## インストール方法
- **iOS (Safari)**: 共有ボタン → 「ホーム画面に追加」
- **Android (Chrome)**: メニュー → 「アプリをインストール」or 自動バナー
- **PC (Chrome/Edge)**: アドレスバー右のインストールアイコン

## オフライン動作
初回アクセス後、Service Worker が `todo.html` とアイコンをキャッシュします。
以降はオフラインでも起動・利用できます（データは localStorage に保存）。

## 更新の反映
`todo.html` などを変更したら、`sw.js` の `CACHE_VERSION`（例: `todo-v1` → `todo-v2`）
を変更してデプロイしてください。これで全クライアントが新しいキャッシュを取得します。

## 注意
- データ（タスク内容）は各端末の localStorage に保存されるため、端末間では同期しません。
  同期したい場合は以前検討した Cloudflare Workers KV などの仕組みが別途必要です。

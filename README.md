# foodsnap
「foodsnap」は直接的、視覚的に料理の魅力を伝えることを目的とした飲食店検索サイトです

## URL
### ユーザー画面
<a href="https://www.foodsnap.org">こちらをクリック</a>

### 店舗管理者画面
<a href="https://www.foodsnap.org/admin/dashboard">こちらをクリック</a>

## 技術スタック
### フロントエンド
<p>
  <img src="https://img.shields.io/badge/-TypeScript-1a1a1a?logo=typescript&logoColor=3178C6&style=flat" />
  <img src="https://img.shields.io/badge/-Next.js-1a1a1a?logo=next.js&logoColor=white&style=flat" />
  <img src="https://img.shields.io/badge/-Tailwind_CSS-1a1a1a?logo=tailwindcss&logoColor=06B6D4&style=flat" />
</p>

### バックエンド
<p>
  <img src="https://img.shields.io/badge/-TypeScript-1a1a1a?logo=typescript&logoColor=3178C6&style=flat" />
  <img src="https://img.shields.io/badge/-Supabase-1a1a1a?logo=supabase&logoColor=3ECF8E&style=flat" />
  <img src="https://img.shields.io/badge/-Hono-1a1a1a?logo=hono&logoColor=E36002&style=flat" />
  <img src="https://img.shields.io/badge/-Prisma-1a1a1a?logo=prisma&logoColor=2D3748&style=flat" />
</p>

### 外部API
<p>
  <img src="https://img.shields.io/badge/-Mapbox-1a1a1a?logo=mapbox&logoColor=4264FB&style=flat" />
</p>

### その他ライブラリ、ツール
<p>
  <img src="https://img.shields.io/badge/-Swagger-1a1a1a?logo=swagger&logoColor=85EA2D&style=flat" />
  <img src="https://img.shields.io/badge/-Zod-1a1a1a?logo=zod&logoColor=3E67B1&style=flat" />
  <img src="https://img.shields.io/badge/-Zustand-1a1a1a?logo=react&logoColor=FF6B6B&style=flat" />
  <img src="https://img.shields.io/badge/-React_Hook_Form-1a1a1a?logo=react&logoColor=EC5990&style=flat" />
</p>

## 要件定義
<a href="docs/src/要件定義.md">こちらのファイルで確認</a>

## 設計(アーキテクチャ)
<a href="docs/src/アーキテクト.md">こちらのファイルで確認</a>

## ソースコード
<a href="https://github.com/Takuya0202/foodsnap-app">こちらで確認</a>

## アプリ画面
### トップページ
<p float="left">
  <img src="readme-images/top1.png" width="45%"></img>
  <img src="readme-images/top2.png" width="45%"></img>
</p>

トップページではおすすめ店舗を紹介しています。<br>
左右方向をスライドすることで投稿を切り替えることができ、メニューについて知ることができます。<br>
また、上下方向をスワイプすることで、店舗の切り替えができます。<br>
店舗の情報は一度に20件まで取得しており、位置情報を許可することで、<br>
半径3km以内に登録されている店舗を優先的に取得することができます。<br>
店舗名をクリックすることでその店舗の詳細ページへと飛ぶことができます。<br>


### 検索ページ
<p float="left">
  <img src="readme-images/search1.png" width="45%"></img>
  <img src="readme-images/search2.png" width="45%"></img>
</p>

ヘッダーの右側ボタンから飲食店を絞り込むことができます。<br>
絞り込みは以下から対応しています。<br>
- ジャンル(一つのみ選択可能。例：焼き肉、寿司、天ぷらなど)
- エリア(複数検索可能。47都道府県での絞り込み)
- タグ(複数検索可能。例：24時間営業、English OK など)
絞り込まれた内容をもとに一覧検索ページへと遷移します。<br>

### 一覧ページ
<p float="left">
  <img src="readme-images/index1.png" width="45%"></img>
  <img src="readme-images/index2.png" width="45%"></img>
</p>

一覧検索ページでは絞り込まれた店舗の表示や全体の店舗を表示します。<br>
店舗名や写真をクリックすることで、モーダルが開かれます。<br>

### 一覧ページモーダル
<p float="left">
  <img src="readme-images/modal1.png" width="45%"></img>
  <img src="readme-images/modal2.png" width="45%"></img>
</p>
クリックされた店舗からモーダルが開かれます。<br>
モーダルはトップページと同じような使用感となっており、店舗の投稿が閲覧できます。<br>
これにより、従来の一覧ページの使用感は保ちながら、モーダル化することで、<br>
ページ遷移なしで店舗の詳細を確認でき、スムーズな閲覧体験を実現しています。<br>

### ユーザーページ(ログインページ)
<p float="left">
  <img src="readme-images/login1.png" width="45%"></img>
  <img src="readme-images/login2.png" width="45%"></img>
</p>
ユーザーログインはGoogleまたはメールアドレスでログイン、登録できます。<br>

### ユーザーページ
<p float="left">
  <img src="readme-images/user1.png" width="45%"></img>
  <img src="readme-images/user2.png" width="45%"></img>
</p>
ユーザーログインをすることで、いいねした投稿を振り返ることができます。<br>
一覧ページと同様、店舗名をクリックすることでモーダルが開かれます。<br>

### コメント機能
<p float="left">
  <img src="readme-images/comment1.png" width="45%"></img>
  <img src="readme-images/comment2.png" width="45%"></img>
</p>
ログイン済みユーザーは店舗にコメントすることができます。<br>
トップページやモーダルのコメントボタンをタップすることで開かれます。<br>

### 詳細ページ
<p float="left">
  <img src="readme-images/detail1.png" width="45%"></img>
  <img src="readme-images/detail2.png" width="45%"></img>
</p>
モーダルやトップページの店舗名をクリックすることで、詳細ページへと遷移します。<br>
店舗の詳細情報(電話番号、営業時間、公式リンクなど)の表示や<br>
Mapboxでの位置情報を閲覧することができます。<br>

### 管理者ページ(ログイン)
<p float="left">
  <img src="readme-images/ad-login1.png" width="45%"></img>
  <img src="readme-images/ad-login2.png" width="45%"></img>
</p>
店舗の登録・ログインはメールアドレスで行うことができます。<br>

### 管理者ページ(ダッシュボード)
<p float="left">
  <img src="readme-images/admin1.png" width="45%"></img>
  <img src="readme-images/admin2.png" width="45%"></img>
</p>
管理者ダッシュボードではユーザーからのいいね数、コメントを確認できます。<br>
また、料理の投稿もすることができます。<br>

### 管理者投稿ページ
<p float="left">
  <img src="readme-images/post1.png" width="45%"></img>
  <img src="readme-images/post2.png" width="45%"></img>
</p>
メニューの投稿には料理の写真、料理名、価格を設定することで投稿できます。<br>
また、任意で料理の説明も補足することができます。<br>

## 現在の取り組み
- GitHub Actionsでの自動デプロイ
- sp => pc , pc => spへのレスポンシブ対応化

## 可能であれば実装したいこと
- 詳細ページでの飲食店予約機能
- 実際の飲食店経営者に使ってもらい、フィードバックを得る。
- 店舗ダッシュボードの充実化


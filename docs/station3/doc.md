# システム設計
## 技術スタック
### next.js(front)
- vercelを使って簡単にプロジェクトをデプロイできる。
- CSR, SSR, SSGが使えるため、パフォーマンスとSEOの両立が可能
- 豊富なコミュニティ

### Hono.js,Prisma(backend)
- 軽量かつ高速なフレームワーク
- cloudflare workersにデプロイすることができ、エッジコンピューティングによる低遅延を実現できる
- Prismaを使ってORMによるCRUD操作

### supabase(DB、認証)
- PostgreSqlのリレーショナルデータベースに対応
- メール認証やOAuth認証などを簡単に使える
- 高度なDBのセキュリティ

### mapbox GL JS(地図API)
- 地図で店舗登録、表示が可能。
- モダンなデザイン。
- 料金体制がマッチ(月間リクエストが50000以下なら無料)

## 画面設計図
figma 
https://www.figma.com/design/6EykxcbLh6zb7I9fAZMVXd/%E7%84%A1%E9%A1%8C?node-id=2-2&t=ANLkUqDYXZkGkP2M-1

## 画面遷移図
miro
https://miro.com/welcomeonboard/clhJVG9xYWtQVmdZc0dnaGwzODQxeTYyeXZKdGJHYjlnWkZ0OHNxNHFhZ1UwT2hUVFlqdVl5R3RUV3paV2tUbUdoZS81NndxZjk3VGVyMHpHcFFlS1pJMU1lTUMvNE9vT3FVSnA5QzkvZTZpVG5hKy83bVRlV2RvTk9ORVVpMnFzVXVvMm53MW9OWFg5bkJoVXZxdFhRPT0hdjE=?share_link_id=150226919505

## ユーザーフロー図
miro
https://miro.com/welcomeonboard/Zmh5WDJkYXpHRUgrR0JycDI2VkcxZ3BHWm9oc3NBMFVrZ29zVWVsR0hEdTlCMDNRS1M4VWhRR0RjRXVHRkZyTU85ZDYxcUJHT3pqRFA0cnBzSFlDbnBJMU1lTUMvNE9vT3FVSnA5QzkvZTRqbnFENVBTMUM3ZFJJdjFuK3o3T3ZQdGo1ZEV3bUdPQWRZUHQzSGl6V2NBPT0hdjE=?share_link_id=75773470844

## ER図
draw.io
https://drive.google.com/file/d/1qV1Dv1KH6ZRUzyeQ4BoQLkxpWG9-iuh7/view?usp=drive_link

## API仕様書
`api.yaml`を参照

## ユーザー認証、apiに関するアーキテクチャ(シーケンス図)
### ユーザー登録フロー
```mermaid
sequenceDiagram
  participant U as User
  participant F as Frontend(Next.js)
  participant B as Backend(Hono)
  participant DB as Supabase

  alt メール認証での登録
    U->>F: 登録フォーム送信
    F->>F: バリデーションチェック
    F->>B: POST /api/users/register
    Note over F,B: { name, email, password }をjsonで送信
    
    B->>B: バリデーションチェック
    B->>DB: auth.signUp()でユーザー登録<br/>emailRedirectTo="http://localhost:3000/auth/callback"

    DB->>U: 確認メール送信（Supabase認証エンドポイント経由）
    U->>DB: メールリンククリック
    DB->>F: リダイレクト<br/>/auth/callback&#35;access_token=xxx
    Note left of F: URLからaccess_tokenを取得
    F->>B: POST /api/auth/callback
    Note over F,B: { accessToken } をjsonで送信
    B->>DB: auth.getUser(accessToken)でユーザー情報確認
    B->>DB: profilesテーブルにユーザー情報登録
    B->>F: 登録完了 + setCookie(accessToken)
    
  else Google OAuthでの登録
    U->>F: 「Googleで登録」ボタンクリック
    F->>B: GET /api/auth/google
    B->>DB: supabase.auth.signInWithOAuthで登録<br/>redirectTo="http://localhost:3000/auth/callback"
    DB->>U: Googleログイン画面へリダイレクト
    U->>DB: アカウントを選択
    DB->>F: リダイレクト<br/>/auth/callback&#35;access_token=xxx
    Note left of F: URLからaccess_tokenを取得
    F->>B: POST /api/auth/callback
    Note over F,B: { accessToken } をjsonで送信
    B->>DB: auth.getUser(accessToken)でユーザー情報確認
    B->>DB: profilesテーブルにユーザー情報登録<br/>（Googleから取得したname）
    B->>F: 登録完了 + setCookie(accessToken)
  end
```

### ログインフロー
```mermaid
sequenceDiagram
  participant U as User
  participant F as Frontend(Next.js)
  participant B as Backend(Hono)
  participant DB as Supabase

  alt メール認証でのログイン
    U->>F: ログインフォーム送信
    F->>F: バリデーションチェック
    F->>B: POST /api/auth/login
    Note over F,B: { email, password }をjsonで送信
    
    B->>B: バリデーションチェック
    B->>DB: auth.signInWithPassword()でログイン
    DB->>B: { accessToken, user }
    B->>F: ログイン成功 + setCookie(accessToken)
    
  else Google OAuthでのログイン
    U->>F: 「Googleでログイン」ボタンクリック
    F->>B: GET /api/auth/google
    B->>DB: supabase.auth.signInWithOAuthでログイン<br/>redirectTo="http://localhost:3000/auth/callback"
    DB->>U: Googleログイン画面へリダイレクト
    U->>DB: アカウントを選択
    DB->>F: リダイレクト<br/>/auth/callback&#35;access_token=xxx
    Note left of F: URLからaccess_tokenを取得
    F->>B: POST /api/auth/callback
    Note over F,B: { accessToken } をjsonで送信
    B->>DB: auth.getUser(accessToken)でユーザー情報確認
    B->>F: ログイン成功 + setCookie(accessToken)
  end
```

### 認証が必要なAPIアクセス（ミドルウェア経由させる）
以下はログインしているユーザー情報を取得する時のAPI
```mermaid
sequenceDiagram
  participant U as User
  participant F as Frontend(Next.js)
  participant B as Backend(Hono)
  participant M as Middleware
  participant DB as Supabase

  U->>F: 認証が必要なページアクセス
  F->>B: GET /api/auth/check
  Note over F,B: CookieからaccessToken自動送信
  
  B->>M: リクエスト受信
  M->>M: getCookie(accessToken)
  M->>DB: auth.getUser(accessToken)でトークン検証
  DB->>M: ユーザー情報返却
  
  alt トークン有効
    M->>B: ユーザー情報をコンテキストに追加
  else トークン無効
    M->>F: 401 Unauthorized
  end
```

### ログアウトフロー
```mermaid
sequenceDiagram
  participant U as User
  participant F as Frontend(Next.js)
  participant B as Backend(Hono)

  U->>F: ログアウトボタンクリック
  F->>B: POST /api/auth/logout
  B->>B: Cookieを削除（maxAge: 0）
  B->>F: ログアウト完了
  F->>F: ログインページにリダイレクト
```

### パスワードリセットフロー
```mermaid
sequenceDiagram
  participant U as User
  participant F as Frontend(Next.js)
  participant B as Backend(Hono)
  participant DB as Supabase

  U->>F: パスワードリセット
  F->>F : バリデーションチェック
  F->>B : POST /api/user/reset-password
  Note over F,B: { email } をjsonで送信
  B->>B : バリデーションチェック
  B->>DB : auth.resetPasswordForEmail()を実行
  DB->>U : パスワードリセットメールを送信
  U->>F : メールを開く
  Note left of F : メールurlのaccess_tokenを取得 
  F->>F : バリデーションチェック
  Note over F,B : { accessToken , Password } をjsonで送信
  F->>B : POST /api/user/reset-password/callback
  B->>B : バリデーションチェック
  B->>DB : auth.getUser(accessToken)でユーザー情報を取得させる
  DB->>DB :  admin.updateUserByIdでpasswordを更新
  DB->>B : 更新完了
  B->>F : ログインページにリダイレクトさせる
```
## このフローのメリット
- backendに認証を任せることでフロントのsupabase依存をなくせる
- localstorageではなく、httpOnly cookieを使うことでxss対策に繋がる
## テーブル定義図
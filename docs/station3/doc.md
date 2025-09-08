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

## ユーザー認証、apiに関するアーキテクチャ
### 1. ユーザー登録フロー
```mermaid
sequenceDiagram
  participant U as User
  participant F as Frontend(Next.js)
  participant B as Backend(Hono)
  participant S as Supabase
  participant E as Email

  U->>F: 登録フォーム送信
  F->>F: バリデーション
  F->>B: POST /users/register
  Note over F,B: {name, email, password}を入力
  
  B->>B: 入力値検証
  B->>S: auth.signUp({email, password, metadata: {name}})

  S->>E: 確認メール送信
  S->>B: {user: null, session: null}
  B->>F: 200 OK {message: "確認メールを送信しました"}
  F->>U: 確認メール送信完了メッセージ表示

  Note over U,E: ユーザーがメールを確認
  U->>E: 確認リンクをクリック
  E->>F: auth/callback?token=xxx にリダイレクト
  
  F->>S: auth.exchangeCodeForSession(code)
  S->>F: {user, session, access_token, refresh_token}
  F->>F: セッション情報をローカルストレージに保存
  F->>U: 登録完了画面表示
```

### 2. ログインフロー
```mermaid
sequenceDiagram
  participant U as User
  participant F as Frontend(Next.js)
  participant B as Backend(Hono)
  participant S as Supabase

  U->>F: ログインフォーム送信
  F->>F: バリデーション
  F->>S: auth.signInWithPassword({email, password})
  
  alt ログイン成功
    S->>F: {user, session, access_token, refresh_token}
    F->>F: セッション情報をローカルストレージに保存
    F->>U: ダッシュボードにリダイレクト
  else ログイン失敗
    S->>F: エラー情報
    F->>U: エラーメッセージ表示
  end
```

### 3. API認証フロー
```mermaid
sequenceDiagram
  participant F as Frontend(Next.js)
  participant B as Backend(Hono)
  participant S as Supabase

  F->>F: セッションからaccess_tokenを取得
  F->>B: API Request + Authorization: Bearer {token}
  
  B->>B: ミドルウェア: トークン抽出
  B->>S: auth.getUser(token)
  
  alt トークン有効
    S->>B: {user: {...}, error: null}
    B->>B: ユーザー情報をコンテキストに保存
    B->>B: ビジネスロジック実行
    B->>F: 200 OK + レスポンスデータ
  else トークン無効/期限切れ
    S->>B: {user: null, error: "Invalid token"}
    B->>F: 401 Unauthorized {error: "認証が必要です"}
    F->>F: ログイン画面にリダイレクト
  end
```

### 4. トークンリフレッシュフロー
```mermaid
sequenceDiagram
  participant F as Frontend(Next.js)
  participant B as Backend(Hono)
  participant S as Supabase

  F->>B: API Request + Authorization: Bearer {expired_token}
  B->>S: auth.getUser(expired_token)
  S->>B: {user: null, error: "Token expired"}
  B->>F: 401 Unauthorized
  
  F->>F: トークンリフレッシュ処理
  F->>S: auth.refreshSession({refresh_token})
  
  alt リフレッシュ成功
    S->>F: {session: {access_token, refresh_token}}
    F->>F: 新しいトークンを保存
    F->>B: 元のリクエストを再実行 + 新しいトークン
    B->>S: auth.getUser(new_token)
    S->>B: {user: {...}, error: null}
    B->>F: 200 OK + レスポンスデータ
  else リフレッシュ失敗
    S->>F: エラー
    F->>F: ログイン画面にリダイレクト
  end
```

## テーブル定義図
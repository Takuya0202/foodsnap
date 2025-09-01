# システム設計
## 技術スタック
### next.js(front)
選定理由
- vercelを使って簡単にプロジェクトをデプロイできる。
- CSR, SSR, SSGが使えるため、パフォーマンスとSEOの両立が可能
- 豊富なコミュニティ

### Hono.js,Prisma(backend)
選定理由
- 軽量かつ高速なフレームワーク
- cloudflare workersにデプロイすることができ、エッジコンピューティングによる低遅延を実現できる
- Prismaを使ってORMによるCRUD操作

### supabase(DB、認証)
選定理由
- PostgreSqlのリレーショナルデータベースに対応
- メール認証やOAuth認証などを簡単に使える
- 高度なDBのセキュリティ

### mapbox(地図API)
- モダンなデザイン。
- 料金体制がマッチ(月間リクエストが50000以下なら無料)

## 画面設計図
figma 
https://www.figma.com/design/6EykxcbLh6zb7I9fAZMVXd/%E7%84%A1%E9%A1%8C?node-id=2-2&t=ANLkUqDYXZkGkP2M-1

## 画面遷移図

## ユーザーフロー図

## ER図

## API仕様書

## テーブル定義図
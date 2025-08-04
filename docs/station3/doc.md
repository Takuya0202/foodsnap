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

## 画面設計図
figma 


## 画面遷移図

## ユーザーフロー図

## ER図

## API仕様書

## テーブル定義図
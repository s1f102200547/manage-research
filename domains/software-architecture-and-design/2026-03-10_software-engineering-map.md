# Web App 開発 × ソフトウェアエンジニアリング 領域マップ
> 学習メモ | 作成日: 2026-03-10  
> 対象: Web Application Development に軸を置いたSE全領域の俯瞰図

---

## 目次

1. [フロントエンドエンジニアリング](#1-フロントエンドエンジニアリング)
2. [バックエンドエンジニアリング](#2-バックエンドエンジニアリング)
3. [API設計・通信プロトコル](#3-api設計通信プロトコル)
4. [データベース・ストレージ設計](#4-データベースストレージ設計)
5. [認証・認可・セキュリティ](#5-認証認可セキュリティ)
6. [インフラ・クラウド・DevOps](#6-インフラクラウドdevops)
7. [テスト・品質保証](#7-テスト品質保証)
8. [パフォーマンス・可観測性](#8-パフォーマンス可観測性)
9. [ソフトウェアアーキテクチャ](#9-ソフトウェアアーキテクチャ)
10. [AIエンジニアリング（Web統合）](#10-aiエンジニアリングweb統合)
11. [リアルタイム・非同期通信](#11-リアルタイム非同期通信)
12. [開発プロセス・チーム工学](#12-開発プロセスチーム工学)
13. [領域マトリクス：習熟度 × 優先度](#13-領域マトリクス習熟度--優先度)

---

## 1. フロントエンドエンジニアリング

### 1-1. レンダリング戦略
| 方式 | 概要 | ユースケース |
|------|------|------------|
| CSR | クライアントでJS実行、APIからデータ取得 | ダッシュボード、管理画面 |
| SSR | リクエストごとサーバーでHTML生成 | SEO重要ページ、認証後コンテンツ |
| SSG | ビルド時にHTML生成 | ブログ、マーケティングLP |
| ISR | SSGをインクリメンタルに再生成 | 準静的コンテンツ（Next.js固有） |
| Streaming SSR | コンポーネント単位でHTMLをストリーム送信 | 大型ページのTTFB改善（React 18+） |
| PPR | 静的シェル＋動的部分を分離（Next.js 15） | ハイブリッド最適化 |

### 1-2. コンポーネント設計
- **Atomic Design**: atoms → molecules → organisms → templates → pages
- **Compound Components**: 関連コンポーネントを組み合わせる（例: `<Select><Option>`）
- **Render Props / HOC**: ロジック再利用パターン（現代はカスタムHooksで代替多い）
- **コロケーション原則**: コンポーネント・スタイル・テスト・型を同一ディレクトリに配置
- **Server Components vs Client Components**（React 18+）: データフェッチをサーバーに寄せてバンドルサイズ削減

### 1-3. 状態管理
```
ローカル状態           → useState / useReducer
サーバー状態（キャッシュ）→ TanStack Query / SWR
グローバルUI状態        → Zustand / Jotai / Redux Toolkit
フォーム状態           → React Hook Form + Zod
URL状態               → nuqs / useSearchParams
```

**状態のスコープ設計が最重要**: どの状態をどの層に置くか判断できることが設計力の核心

### 1-4. スタイリング
| アプローチ | 代表技術 | 特徴 |
|-----------|---------|------|
| Utility-first CSS | Tailwind CSS | クラス名で完結、JIT |
| CSS-in-JS | styled-components, Emotion | 動的スタイル、SSRに注意 |
| Zero-runtime CSS-in-JS | Linaria, vanilla-extract | パフォーマンス改善版 |
| CSS Modules | 標準 | スコープ化、シンプル |
| コンポーネントライブラリ | shadcn/ui, Radix UI | アクセシビリティ込み |

### 1-5. パフォーマンス最適化
- **Core Web Vitals**: LCP, FID/INP, CLS の3指標
- **Code Splitting**: `dynamic()` / `React.lazy` でチャンク分割
- **Image Optimization**: `next/image`、WebP/AVIF、lazy loading、blur placeholder
- **Font Optimization**: `next/font`、FOUT/FOIT防止
- **Bundle Analysis**: `@next/bundle-analyzer`、tree-shaking確認
- **React再レンダリング制御**: `memo`, `useMemo`, `useCallback` の適切な使用（過剰な使用はアンチパターン）

### 1-6. アクセシビリティ (a11y)
- WAI-ARIA roles, states, properties
- キーボードナビゲーション（focus management）
- スクリーンリーダー対応
- カラーコントラスト比（WCAG 2.1 AA準拠）
- ツール: axe-core, Lighthouse, NVDA/VoiceOver

---

## 2. バックエンドエンジニアリング

### 2-1. Webサーバーの仕組み
```
リクエストライフサイクル:
Client → DNS → TCP Handshake → TLS Handshake → HTTP Request
→ [Load Balancer] → [Reverse Proxy: nginx/Caddy]
→ [App Server: Node.js/Python/Go]
→ [Middleware: 認証/ロギング/CORS/Rate Limiting]
→ [Route Handler] → [Business Logic] → [DB/Cache/External API]
→ Response
```

### 2-2. Webフレームワーク設計パターン
- **MVC**: Model（データ）+ View（表示）+ Controller（制御）
- **Layered Architecture**: Presentation → Application → Domain → Infrastructure
- **ミドルウェアチェーン**: Express/Koa的なパイプライン処理（関心の分離）
- **依存性注入 (DI)**: NestJS等。テスタビリティと疎結合を実現

### 2-3. Node.js バックエンド
- **イベントループ理解**: Call Stack → Web APIs → Callback Queue → Microtask Queue
- **非同期パターン**: async/await, Promise chaining, Promise.all/allSettled/race
- **ストリーム処理**: Readable/Writable/Transform Stream（大量データのメモリ効率化）
- **Worker Threads**: CPUバウンドタスクの並列処理（デフォルトはシングルスレッド）
- **クラスタリング**: CPUコア数に応じたプロセスフォーク

### 2-4. サーバーレス・エッジ
| モデル | 特徴 | 課題 |
|--------|------|------|
| FaaS (Cloud Functions/Lambda) | イベント駆動、自動スケール | コールドスタート、ステートレス |
| Edge Functions (Vercel/CF Workers) | ユーザー近接実行、超低レイテンシ | 制限されたランタイム、Node API使用不可の場合あり |
| Container (Cloud Run) | フルコントロール、コールドスタート制御可能 | 最小限のインスタンス維持コスト |

---

## 3. API設計・通信プロトコル

### 3-1. REST設計原則
```
リソース指向:
GET    /reservations          → 一覧取得
GET    /reservations/:id      → 詳細取得
POST   /reservations          → 新規作成
PUT    /reservations/:id      → 全体更新
PATCH  /reservations/:id      → 部分更新
DELETE /reservations/:id      → 削除

ネスト:
GET    /reservations/:id/guests

冪等性:
GET, PUT, DELETE → 冪等（何度呼んでも同じ結果）
POST → 非冪等
```

**ベストプラクティス**:
- バージョニング: `/v1/`, `/v2/` or Headerベース
- ページネーション: cursor-based（大規模）vs offset-based（シンプル）
- エラーレスポンス: RFC 7807 (Problem Details) 形式
- HATEOAS: リンク情報をレスポンスに含める（実装コスト高め）

### 3-2. GraphQL
- **Schema First**: SDL でスキーマ定義 → コード生成
- **N+1問題**: DataLoader でバッチ・キャッシュ
- **Subscription**: WebSocketによるリアルタイム
- **利点**: クライアントが必要なフィールドを指定可 / 型安全
- **欠点**: キャッシュ複雑、ファイルアップロード扱いにくい、学習コスト

### 3-3. gRPC
- Protocol Buffers（バイナリシリアライズ）で高速
- HTTP/2 ベース、双方向ストリーミング対応
- マイクロサービス間通信に最適
- Webブラウザから直接は困難（grpc-web が必要）

### 3-4. tRPC
- TypeScript フルスタック型安全 RPC
- スキーマ定義不要、サーバーの型がクライアントに自動伝播
- Next.js との相性◎
- マイクロサービス・複数チームには向かない

### 3-5. Webhook設計
```
設計要素:
- Payload 署名検証（HMAC-SHA256）
- 冪等キー（重複配信対策）
- 再試行ポリシー（指数バックオフ）
- 配信ログ・デッドレターキュー
- タイムアウト（受信側は即200を返す）
```

### 3-6. HTTP深掘り
- **HTTP/1.1**: Keep-Alive、パイプライン（Head-of-Line blocking問題）
- **HTTP/2**: 多重化、ヘッダー圧縮(HPACK)、Server Push
- **HTTP/3 (QUIC)**: UDPベース、接続確立高速化、パケットロス耐性
- **キャッシュ制御**: `Cache-Control`, `ETag`, `Last-Modified`, `Vary`
- **CORS**: Preflight (OPTIONS), Simple Request, credentialsの扱い

---

## 4. データベース・ストレージ設計

### 4-1. RDB設計
```
正規化:
1NF: 繰り返し項目排除、原子的値
2NF: 部分関数従属排除
3NF: 推移関数従属排除
BCNF: すべての決定子が候補キー

実務では意図的な非正規化（パフォーマンス）も行う
```

**インデックス戦略**:
- B-tree インデックス: 等値検索・範囲検索・ソートに強い
- Hash インデックス: 等値検索のみ（範囲検索不可）
- 複合インデックス: カラム順序が重要（左端プレフィックスルール）
- カバリングインデックス: クエリがインデックスだけで完結（テーブルアクセス不要）
- インデックスの過剰付与は書き込み性能劣化を招く

**トランザクション分離レベル**:
| レベル | Dirty Read | Non-Repeatable Read | Phantom Read |
|--------|-----------|---------------------|-------------|
| READ UNCOMMITTED | 発生 | 発生 | 発生 |
| READ COMMITTED | なし | 発生 | 発生 |
| REPEATABLE READ | なし | なし | 発生（MySQLは防ぐ） |
| SERIALIZABLE | なし | なし | なし |

### 4-2. NoSQL設計
**Firestore / DynamoDB（Document/KV型）**:
- データをクエリパターンに合わせてモデリング（RDBと逆の発想）
- Denormalization が前提（JOIN不可のため）
- Composite index設計がパフォーマンスに直結
- トランザクション境界がドキュメント or バッチ単位

**設計パターン**:
```
One-to-Many:
  - 子をサブコレクションに格納（深いネスト）
  - 子のIDを配列で親に持つ（浅いアクセス）

Many-to-Many:
  - 中間コレクションを作成

Denormalization:
  - 参照先データを埋め込み（読み取り高速化、書き込み時の整合性保持が課題）
```

### 4-3. キャッシュ戦略
```
階層:
Browser Cache → CDN Cache → Reverse Proxy Cache → Application Cache (Redis) → DB Query Cache

パターン:
Cache-Aside (Lazy Loading): アプリがキャッシュを管理、ミス時にDBから取得
Write-Through: 書き込み時にキャッシュとDBを同時更新
Write-Behind (Write-Back): キャッシュに書き込み後、非同期でDBに反映
Read-Through: キャッシュレイヤーが自動でDBから補充

キャッシュ無効化:
- TTL（時間で失効）
- イベントベース（データ変更時に明示的に削除）
- タグベース（関連するキャッシュをグループで無効化）
```

**Redis活用**:
- セッションストア
- 分散ロック（SETNX / SET NX PX）
- Pub/Sub
- Rate limiting（Sliding Window / Token Bucket）
- リーダーボード（Sorted Sets）

### 4-4. データマイグレーション
- **Expand-Contract パターン**: 無停止でのスキーマ変更
  1. Expand: 新カラム追加（nullable）
  2. Dual-write: 新旧両方に書く
  3. Backfill: 既存データを新カラムに移行
  4. Switch: 読み取りを新カラムに切り替え
  5. Contract: 旧カラム削除
- ツール: Flyway, Liquibase, Prisma Migrate

### 4-5. Vector DB（AI時代の新標準）
- 埋め込みベクトルの近似最近傍探索（ANN）
- HNSW, IVF などのインデックスアルゴリズム
- 代表: Pinecone, Weaviate, pgvector（PostgreSQL拡張）, Chroma
- RAGシステムの基盤コンポーネント

---

## 5. 認証・認可・セキュリティ

### 5-1. 認証方式
```
セッションベース認証:
Client → Server（認証）→ Session ID発行 → Cookie保存
以降: Cookie送信 → Sessionストアで検証
利点: サーバー側でセッション無効化可能
欠点: スケールアウト時にセッションストア共有が必要

JWTベース認証:
Client → Server（認証）→ JWT発行
以降: Authorizationヘッダーに付与
利点: ステートレス（スケールしやすい）
欠点: 発行済みトークンの無効化が難しい（ブラックリスト必要）

推奨:
- Access Token: 短命（15分〜1時間）
- Refresh Token: 長命（7〜30日）、HttpOnly Cookie保存
- Token Rotation: Refresh Token使用時に新しいものを発行
```

### 5-2. OAuth 2.0 / OIDC
```
Authorization Code Flow（Webアプリ標準）:
1. Client → Authorization Serverへリダイレクト
2. ユーザーが同意
3. Authorization Code を受け取る
4. Code + Client Secret でToken交換（サーバーサイド）
5. Access Token / Refresh Token / ID Token 取得

PKCE (Proof Key for Code Exchange):
- Code Verifier（ランダム文字列）を生成
- Code Challenge（SHA-256ハッシュ）をリクエストに含める
- Token交換時にCode Verifierを送り検証
- SPAやモバイルなどClient Secretを安全に保持できない場合に必須
```

### 5-3. 主要セキュリティ脆弱性
| 脆弱性 | 概要 | 対策 |
|--------|------|------|
| XSS (Cross-Site Scripting) | 悪意あるJSをページに注入 | エスケープ、CSP, HttpOnly Cookie |
| CSRF | 正規ユーザーに意図しないリクエストを実行 | CSRF Token, SameSite Cookie, Double Submit Cookie |
| SQL Injection | 悪意あるSQLを注入 | プリペアドステートメント、ORM使用 |
| SSRF | サーバーに内部リソースへのリクエストを強制 | 許可リストによる宛先制限 |
| Path Traversal | `../`で意図しないファイルアクセス | パス正規化・検証 |
| Broken Authentication | セッション固定攻撃、ブルートフォース | ログイン後にセッション再生成、Rate Limiting |

### 5-4. HTTPS・TLS
- TLS 1.3: 1-RTTハンドシェイク（1.2は2-RTT）
- 証明書: DV / OV / EV の違い
- HSTS: HTTPSを強制（`Strict-Transport-Security`ヘッダー）
- Certificate Pinning: 特定証明書のみ信頼（モバイル向け）

### 5-5. 認可設計
```
RBAC (Role-Based Access Control):
  User → Roles → Permissions
  シンプル、一般業務システムに適合

ABAC (Attribute-Based Access Control):
  Subject属性 + Resource属性 + Environment属性 でポリシー評価
  柔軟だが複雑

ReBAC (Relationship-Based Access Control):
  オブジェクト間の関係でアクセス制御（Google Zanzibar）
  Google Drive的な「共有」機能の実装に適合
```

---

## 6. インフラ・クラウド・DevOps

### 6-1. コンテナ・オーケストレーション
**Docker基礎**:
```dockerfile
# ベストプラクティス
FROM node:20-alpine          # 軽量ベースイメージ
WORKDIR /app
COPY package*.json ./        # レイヤーキャッシュ活用
RUN npm ci --only=production # 本番依存のみ
COPY . .
RUN npm run build
USER node                    # 非rootユーザーで実行
EXPOSE 3000
CMD ["node", "server.js"]
```

**マルチステージビルド**:
```dockerfile
FROM node:20 AS builder
# ビルド処理...

FROM node:20-alpine AS runner
COPY --from=builder /app/.next ./.next
# 軽量な実行イメージのみ
```

**Kubernetes基礎概念**:
- Pod: 最小デプロイ単位（1〜複数コンテナ）
- Deployment: Podの宣言的管理、ローリングアップデート
- Service: Pod群へのネットワーキング（ClusterIP/NodePort/LoadBalancer）
- Ingress: L7ルーティング、TLS終端
- ConfigMap / Secret: 設定・機密情報の分離
- HPA (Horizontal Pod Autoscaler): メトリクスベースの自動スケール

### 6-2. CI/CD
```
典型的なパイプライン:
Commit → Lint → Type Check → Unit Test → Build
→ Integration Test → Security Scan (SAST/SCA)
→ Build & Push Container Image
→ Deploy to Staging → E2E Test
→ Deploy to Production → Smoke Test

デプロイ戦略:
Rolling Update: 旧Podを徐々に新Podに置き換え
Blue-Green: 新旧環境を並行維持、瞬時に切り替え
Canary: トラフィックの一部を新バージョンに流す
Feature Flag: コードはデプロイ済み、機能を動的にON/OFF
```

### 6-3. IaC (Infrastructure as Code)
- **Terraform**: HCLでクラウドリソースを宣言的定義、plan/apply ワークフロー
- **Pulumi**: TypeScript/Python等でIaC（プログラマブル）
- **Bicep / CDK**: Azure / AWS ネイティブIaC

**Firestore indexesのIaC的管理** (Haruが実践中):
```json
// firestore.indexes.json
{
  "indexes": [
    {
      "collectionGroup": "reservations",
      "queryScope": "COLLECTION",
      "fields": [
        {"fieldPath": "propertyId", "order": "ASCENDING"},
        {"fieldPath": "checkIn", "order": "ASCENDING"}
      ]
    }
  ]
}
```

### 6-4. クラウドコスト設計（FinOps）
- **リザーブドインスタンス vs オンデマンド**: 予測可能なワークロードはRI
- **Spot/Preemptible インスタンス**: バッチ処理・CI向け（最大80%削減）
- **右サイジング**: 実際のリソース使用量に合わせてインスタンスサイズを最適化
- **コールドスタート対策（Cloud Run）**: 最小インスタンス数設定とコスト増のトレードオフ
- タグ管理による環境・機能別コスト可視化

---

## 7. テスト・品質保証

### 7-1. テスト戦略
```
テストピラミッド（上ほど高コスト・低速・高確信度）:
         /E2E\           → 少数（ハッピーパスのみ）
        /統合テスト\      → 中程度
       /ユニットテスト\   → 多数

テストトロフィー（Kent C. Dodds提唱、Web向き）:
      /E2E\
     /統合テスト\        → 最多（ユーザー操作に近い粒度）
    /ユニットテスト\
   /静的解析（型・Lint）\
```

### 7-2. テストの種類
| テスト種別 | 目的 | ツール |
|-----------|------|--------|
| Unit Test | 関数・クラス単体の振る舞い検証 | Vitest, Jest |
| Integration Test | 複数モジュール連携、DB・外部サービスとの統合 | Vitest, Supertest |
| E2E Test | ブラウザ操作のシナリオ全体 | Playwright, Cypress |
| Contract Test | API契約（スキーマ）の一致確認 | Pact |
| Visual Regression | UIのビジュアル差分検出 | Chromatic, Percy |
| Load Test | 負荷下でのパフォーマンス検証 | k6, Artillery |
| Chaos Engineering | 障害注入によるレジリエンス検証 | Chaos Monkey |

### 7-3. テスタビリティの設計
- **依存性逆転**: インターフェースに依存、実装に依存しない（モック可能にする）
- **Pure Function**: 副作用なし → テストが最も容易
- **テストダブル**:
  - Stub: 固定値を返す
  - Mock: 呼ばれたかどうかを検証
  - Spy: 実際の実装を使いつつ呼び出しを記録
  - Fake: 簡易実装（In-memory DB等）

### 7-4. コード品質指標
- **カバレッジ**: Line / Branch / Function（100%を目指すのは誤り、重要なロジックを優先）
- **循環的複雑度 (Cyclomatic Complexity)**: 分岐の数。高いほどテストケースが増える
- **認知的複雑度**: 人間がコードを理解する難しさ（SonarQubeで計測）
- **技術的負債比率**: コードの問題修正にかかる時間 / 開発時間

---

## 8. パフォーマンス・可観測性

### 8-1. Webパフォーマンス指標
```
Core Web Vitals:
  LCP (Largest Contentful Paint)  → 知覚読み込み速度。目標: < 2.5秒
  INP (Interaction to Next Paint) → 応答性。目標: < 200ms（FIDの後継）
  CLS (Cumulative Layout Shift)   → 視覚的安定性。目標: < 0.1

その他:
  TTFB (Time to First Byte)       → サーバー応答速度
  FCP (First Contentful Paint)    → 初回コンテンツ表示
  TTI (Time to Interactive)       → インタラクション可能になるまでの時間
```

### 8-2. バックエンドパフォーマンス
- **N+1クエリ問題**: ORMのeager loading / DataLoaderで対処
- **接続プーリング**: DB接続確立コストを削減（pg-pool, PrismaのconnectionLimit）
- **クエリ最適化**: EXPLAIN ANALYZE でクエリプラン確認
- **非同期化**: 重い処理をバックグラウンドジョブ（Bull, Cloud Tasks）に移譲
- **ページネーション**: OFFSET問題（大きいOFFSETは遅い）→ Cursor-based推奨

### 8-3. 可観測性の3本柱 (OpenTelemetry)
```
Metrics（メトリクス）:
  - カウンター（リクエスト数、エラー数）
  - ゲージ（CPU使用率、メモリ）
  - ヒストグラム（レイテンシ分布）

Logs（ログ）:
  - 構造化ログ（JSON形式 → 検索・集計可能）
  - ログレベル: DEBUG / INFO / WARN / ERROR / FATAL
  - 相関ID（Correlation ID）でリクエスト追跡

Traces（トレース）:
  - 分散トレーシング（マイクロサービス間のリクエスト追跡）
  - Span: 処理の単位（開始・終了時刻、属性）
  - Trace ID で一連の処理を紐付け
```

### 8-4. SLO/SLA設計
```
SLI (Service Level Indicator): 測定する指標（可用性、レイテンシ等）
SLO (Service Level Objective): SLIの目標値（例: 99.9%の可用性）
SLA (Service Level Agreement): ユーザーとの合意（SLOより緩め）
Error Budget: SLO達成のために「使える」障害時間

例: 99.9% SLO の場合
  月次 Error Budget = 43.2分
  このバジェット内であれば積極的なリリースが可能
  超えたら凍結してレジリエンス改善に集中
```

---

## 9. ソフトウェアアーキテクチャ

### 9-1. アーキテクチャパターン
```
モノリシックアーキテクチャ:
  ┌─────────────────────────┐
  │ UI + API + Business + DB│
  └─────────────────────────┘
  利点: シンプル、デバッグ容易、トランザクション管理簡単
  欠点: スケール困難、デプロイ全体影響、技術スタック固定

マイクロサービスアーキテクチャ:
  ┌──────┐ ┌──────┐ ┌──────┐
  │予約MS│ │顧客MS│ │決済MS│
  └──────┘ └──────┘ └──────┘
  利点: 独立デプロイ、独立スケール、技術選択自由
  欠点: 分散システムの複雑性、ネットワーク障害、分散トランザクション

モジュラーモノリス（現代の推奨）:
  単一デプロイ単位だが、内部はモジュール境界で厳密に分離
  将来的なマイクロサービス分割の準備ができている
```

### 9-2. イベント駆動アーキテクチャ (EDA)
```
パターン:
Event Notification: 「何かが起きた」をブロードキャスト（軽量）
Event-Carried State Transfer: イベントにデータを乗せて伝播
Event Sourcing: 状態変化をイベントとして永続化（イミュータブル）
CQRS: 読み取りモデルと書き込みモデルを分離

メリット: 疎結合、スケーラビリティ、監査ログ自動生成
デメリット: Eventual Consistency、デバッグの難しさ、複雑性
```

### 9-3. ドメイン駆動設計 (DDD)
```
戦略的設計:
  Bounded Context: 用語・モデルが一貫して通用する境界
  Context Map: 複数のBounded Context間の関係（ACL, Partnership等）
  Ubiquitous Language: ビジネスとエンジニアが共通して使う語彙

戦術的設計:
  Entity: 識別子を持つオブジェクト（予約ID 001の予約）
  Value Object: 識別子なし、値で等値判断（金額、住所）
  Aggregate: 整合性境界（予約 = 予約詳細 + ゲスト情報のまとまり）
  Repository: Aggregateの永続化抽象
  Domain Service: どのEntityにも属さないビジネスロジック
  Domain Event: ドメイン内で起きた出来事
```

### 9-4. 分散システムの課題
- **CAP定理**: Consistency, Availability, Partition Tolerance の3つを同時に完全満足は不可能
- **Eventual Consistency**: 最終的には整合する（Firestoreのリアルタイム同期もこれ）
- **冪等性設計**: 同じリクエストを複数回受けても同じ結果になる設計
- **Distributed Locking**: Redis SETNX, Zookeeper等
- **Saga Pattern**: マイクロサービス間の分散トランザクション（補償トランザクション）
- **Outbox Pattern**: DBとメッセージキューへの二重書き込みの整合性保証

---

## 10. AIエンジニアリング（Web統合）

### 10-1. LLM API統合
```typescript
// Streaming Response（ユーザー体験向上）
const stream = await anthropic.messages.stream({
  model: 'claude-opus-4-5',
  max_tokens: 1024,
  messages: [{ role: 'user', content: prompt }]
});

for await (const chunk of stream) {
  process.stdout.write(chunk.delta?.text ?? '');
}
```

**プロンプトエンジニアリング**:
- Zero-shot / Few-shot / Chain-of-Thought (CoT)
- System Prompt の役割設計
- Output Format の強制（JSON mode, XML tags）
- プロンプトのバージョン管理（Langsmith, Promptfoo）

### 10-2. RAG (Retrieval-Augmented Generation)
```
アーキテクチャ:
Document → Chunking → Embedding → Vector DB に格納

クエリ時:
Query → Embedding → Vector DB検索（ANN）→ 関連チャンク取得
→ プロンプトに注入 → LLM生成

最適化ポイント:
- Chunking戦略: 固定長 vs 意味単位（Semantic Chunking）
- Embedding model選択（多言語対応、次元数）
- Reranking: BM25 + ベクトル検索のハイブリッド
- HyDE: 仮説文書生成で検索精度向上
```

### 10-3. AIエージェント設計
```
ReAct パターン:
Thought → Action → Observation → Thought → ...

Tool Use:
- ツール定義（JSON Schema）
- LLMがツールを選択・呼び出し
- 結果をコンテキストに追加

マルチエージェント:
- Orchestrator: タスク分解・各エージェントに指示
- Subagent: 専門タスク実行
- Human-in-the-Loop: 重要判断での人間介入

評価指標:
- Task Completion Rate（タスク完了率）
- Tool Call Accuracy（ツール選択の正確さ）
- Step Efficiency（最小ステップで完了できるか）
- Hallucination Rate（幻覚の発生率）
```

### 10-4. GUIエージェント（卒研の核心領域）
```
認識層:
  DOM/アクセシビリティツリー解析
  Network traffic インターセプト
  スクリーンショット + Vision model
  OCR（Tesseract等）

操作層:
  Playwright / Puppeteer による操作
  Self-healing locator（UI変更への適応）
  Human-in-the-loop（不確実時の確認）

標準化層:
  クロスSaaS データスキーマ
  操作ログのイベントソーシング
  異常検知・リトライ

研究空白:
  永続的クロスSaaS状態同期
  日本語UI対応のlocator生成
  API-less SaaS の変更検知戦略
```

### 10-5. LLMOps
- **評価パイプライン**: LLM-as-Judge, Human Eval, ベンチマーク
- **プロンプトバージョン管理**: A/Bテスト、回帰テスト
- **コスト管理**: トークン数モニタリング、キャッシング（Prompt Caching）
- **Fine-tuning vs RAG**: データ量・更新頻度・コストのトレードオフ
- **ガードレール**: 入力検証、出力フィルタリング（Guardrails AI等）

---

## 11. リアルタイム・非同期通信

### 11-1. WebSocket
```
HTTP: Client → Request → Server → Response（都度接続）
WebSocket: Client ↔ Server（持続的双方向接続）

ユースケース: チャット、通知、コラボレーションツール、ゲーム

実装注意点:
- 接続管理（再接続ロジック）
- スケールアウト時のブロードキャスト（Redis Pub/Sub）
- Heartbeat / Ping-Pong でゾンビ接続を検出
- Sticky Session or 共有状態ストア
```

### 11-2. Server-Sent Events (SSE)
- サーバー→クライアントの一方向ストリーム
- HTTP上で動作（WebSocketより軽量・シンプル）
- LLMのストリーミング出力に最適
- 自動再接続機能内蔵
- HTTP/2 で多重化可能

### 11-3. メッセージキュー・バックグラウンドジョブ
```
用途: 重い処理の非同期化、ピーク負荷の吸収、サービス間疎結合

技術選択:
  Bull/BullMQ (Redis-backed): Node.js向け、豊富なジョブ管理機能
  Cloud Tasks (GCP): HTTP経由のタスクキュー、Cloud Run相性◎
  Cloud Pub/Sub (GCP): メッセージングサービス、Fanout対応
  Kafka: 大規模ストリーム処理、ログ収集

設計ポイント:
  - 冪等性（同じジョブを複数回実行しても安全か）
  - Dead Letter Queue（失敗ジョブの保管・再試行）
  - ジョブの優先度設定
  - タイムアウト・リトライポリシー
```

### 11-4. Polling vs Webhook vs Streaming
| 方式 | レイテンシ | サーバー負荷 | 実装難度 |
|------|-----------|------------|---------|
| Short Polling | 高（ポーリング間隔依存） | 高 | 低 |
| Long Polling | 中 | 中 | 中 |
| SSE | 低 | 低 | 低 |
| WebSocket | 最低 | 低（接続確立後） | 高 |
| Webhook | 低 | 最低 | 中（受信側の実装要） |

---

## 12. 開発プロセス・チーム工学

### 12-1. バージョン管理・Git戦略
```
ブランチ戦略:
  Git Flow: main/develop/feature/release/hotfix（複雑、リリース明確な場合）
  GitHub Flow: main + feature branch（シンプル、継続的デプロイ向き）
  Trunk-Based Development: main直コミット + Feature Flag（高頻度デプロイ向き）

コミット規約:
  Conventional Commits: feat/fix/docs/chore/refactor/perf/test
  → CHANGELOGの自動生成、セマンティックバージョニングと連動
```

### 12-2. コードレビュー
- **レビューの目的**: バグ発見よりも知識共有・設計議論が主目的
- **Author の責務**: 差分を小さく保つ（PR size < 400行推奨）
- **Reviewer の責務**: 24時間以内のレスポンス、建設的フィードバック
- **Conventional Comments**: `nit:`, `q:`, `suggestion:`, `issue:` のプレフィックス
- **LGTM文化の危険性**: ゴム印押しにならないよう設計判断を議論

### 12-3. ドキュメント
```
種類と役割:
  ADR (Architecture Decision Record): 設計決定の記録（なぜこの選択をしたか）
  README: 設定・起動方法（onboarding向け）
  API仕様書: OpenAPI / Swagger（自動生成推奨）
  Runbook: 障害対応手順
  Postmortem: 障害振り返り（責任追及でなく改善のため）

良いドキュメントの原則:
  - コード変更と同期するDocs-as-Code
  - 「なぜ」を書く（「何を」はコードが示す）
  - 読者を明示する（初心者向け vs 経験者向け）
```

### 12-4. 技術選定の判断軸
```
評価フレームワーク:
1. 解決したい問題の明確化
2. 代替案の洗い出し（最低3案）
3. 評価軸の設定（パフォーマンス/DX/コスト/コミュニティ/学習コスト）
4. 各案のPros/Cons
5. 採用基準の明示
6. ADRとして記録

OSS採用時の注意:
  - ライセンス（MIT/Apache/GPL/AGPL）
  - メンテナンス状況（最終コミット・Issue対応速度）
  - 依存パッケージ数（サプライチェーン攻撃リスク）
  - BreakingChangeの頻度
```

---

## 13. 領域マトリクス：習熟度 × 優先度

> Haruの現在地を踏まえた自己評価（2026年3月時点）

| 領域 | 習熟度 | 卒研優先度 | 実務優先度 | コメント |
|------|--------|-----------|-----------|---------|
| 1. フロントエンド | ◎ | 低 | 中 | Next.js App Router実践済み |
| 2. バックエンド | ○ | 低 | 中 | Cloud Run/Node実践済み |
| 3. API設計 | ○ | 中 | 高 | REST実践済み、gRPC/tRPC未 |
| 4. DB設計 | ○ | 低 | 高 | Firestore高度。RDB理論補強余地 |
| 5. 認証・セキュリティ | ○ | 低 | 高 | Firebase Auth実践。深掘り余地大 |
| 6. インフラ・DevOps | ○ | 低 | 中 | CI/CD実践済み。K8s・IaC補強余地 |
| 7. テスト・QA | ○ | 中 | 高 | E2E実践済み。統合テスト強化余地 |
| 8. パフォーマンス・可観測性 | △ | 低 | 高 | OTelはほぼ未着手。重要 |
| 9. アーキテクチャ | ◎ | 高 | 高 | 旅館DXで実践中。DDDで言語化を |
| 10. AIエンジニアリング | ○→◎ | **最高** | 高 | 卒研の主戦場 |
| 11. リアルタイム通信 | △ | 低 | 中 | Firestore RT利用済み。SSE/WS未 |
| 12. 開発プロセス | ○ | 低 | 中 | 個人開発が多いため実践経験は限定的 |

### 短期アクション（卒研フェーズ）
1. **領域10（AIエンジニアリング）の10-4 GUIエージェント** → 文献調査・実装の深化
2. **領域9（アーキテクチャ）** → 旅館DX設計をDDD語彙で論文化
3. **領域7（テスト）** → エージェント評価指標の設計・実装

### 中期アクション（就職・大学院進学後）
1. **領域8（可観測性）** → OTel + Cloud Monitoring の実践
2. **領域5（セキュリティ）** → Web Security 体系学習（OWASP Top 10）
3. **領域4（DB）** → PostgreSQL + pgvector でRDB+Vector統合を実践

---

*このマップは生きたドキュメントとして定期更新すること*
*最終更新: 2026-03-10*
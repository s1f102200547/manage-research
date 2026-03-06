# AIエージェント包括調査レポート
## ─ 理論・実装・ビジネス・教育の4層から理解する ─

**作成日**: 2026年3月4日
**対象読者**: INIAD 3年生 / SaaS統合レイヤー開発を志向するエンジニア

---

## 1. あなたの調査の再構成と整理

あなたの初期調査は良い切り口を持っているが、分類軸が混在している。以下に「誰/何が主語か」と「抽象度」の2軸で再構成する。

### レイヤー1: パラダイムシフト（AIが労働者になる）

AIが「ツール」から「行為者（Agent）」に変わるという根本的な転換。

| 概念 | 意味 | 具体例 |
|------|------|--------|
| Agentic AI | 自律的に意思決定し、ツールを使い、フィードバックから学習するAI | Claude Code, OpenAI Operator |
| Digital Workers / AI Employees | 人間の代替として業務を遂行するAI実体 | 予約管理Bot, カスタマーサポートAgent |
| AI Workforce | 複数AIが組織内で分業・協調する概念 | マルチエージェントチーム |

### レイヤー2: ソフトウェアアーキテクチャ（AIが使う前提で作る）

| 概念 | 意味 | なぜ重要か |
|------|------|------------|
| AI-First Software | AIが第一のユーザーであることを前提に設計 | UI不要、API/構造化データが主体 |
| Headless SaaS | UIを持たず、APIのみで提供されるSaaS | エージェントが直接操作可能 |
| Agent-First Architecture | エージェントの利用を前提とした設計思想 | MCP/A2Aなどプロトコル対応 |
| Machine-to-Machine SaaS | AI同士がAPIで連携するSaaS | 人間の介在なしにデータ連携 |

### レイヤー3: オーケストレーション（AIが複数ツールを使う）

| 概念 | 意味 | 関連技術 |
|------|------|----------|
| Agent Orchestration | 複数ツール/エージェントの組み合わせと制御 | LangGraph, CrewAI, AutoGen |
| Agentic Workflows | AIが自律的にワークフローを構築・実行 | ReActループ, Plan-and-Execute |
| Goal-Driven Automation | 「目標」を与えると手段をAIが自律選択 | HuggingGPT, TaskWeaver |
| Intent-Based Automation | ユーザーの「意図」から行動を推論 | 自然言語→タスク分解 |

### レイヤー4: エージェント内部の役割分化

| 役割 | 機能 | 認知科学との対応 |
|------|------|-----------------|
| Planner Agent | タスクを分解し実行計画を策定 | 前頭前野（計画・抑制） |
| Executor Agent | 計画に従いツールを呼び出し実行 | 運動野（行動実行） |
| Critic Agent | 結果を評価しフィードバック | メタ認知（自己監視） |
| Supervisor Agent | 全体の進行を管理・調整 | 注意制御系 |

---

## 2. アカデミック研究レベルで抜け落ちている要素

### 2-1. 認知アーキテクチャの理論的基盤

あなたの調査には「LLMベースのエージェントが、認知科学的にどう位置づけられるか」が欠落している。

**古典的認知アーキテクチャとの接続**

LLMエージェントの設計は、50年以上の認知科学研究に基づいている。

- **SOAR（State, Operator, And Result）**: 1983年にNewell が提案。目標スタック、プロダクションルール、チャンキングによる学習。現代のPlan-and-Execute型エージェントの祖先。
- **ACT-R（Adaptive Control of Thought—Rational）**: Anderson が開発。宣言的記憶と手続き的記憶の分離、活性化に基づく記憶検索。LLMの「コンテキストウィンドウ」と「long-term memory（RAG/vector DB）」の分離に類似。
- **BDI（Belief-Desire-Intention）**: Rao & Georgeff (1995)。エージェントの信念・欲求・意図を分離してモデル化。ただし、最近の研究ではLLMベースのエージェントにBDIフレームワークを適用することは「概念の誤用（conceptual retrofitting）」とも指摘されている。

**重要な理論的区別**: シンボリック（古典的AI）パラダイムとニューラル（LLM）パラダイムは根本的に異なるアーキテクチャ基盤を持つ。前者は決定論的ルールベース、後者は確率的生成ベース。

### 2-2. 計画と推論の理論

| 概念 | 説明 | 代表手法 |
|------|------|----------|
| Chain-of-Thought（CoT） | 推論過程を言語化し段階的に回答 | Wei et al., 2022 |
| Tree-of-Thought（ToT） | 分岐する推論パスを木構造で探索 | Yao et al., 2023 |
| Graph-of-Thought（GoT） | 思考ノード間を任意にリンク・統合 | 2024年以降の発展 |
| ReAct | Thought→Action→Observationのループ | Yao et al., ICLR 2023 |
| Reflexion | 失敗からの自己反省と学習 | Shinn et al., 2023 |

### 2-3. 形式的な意思決定モデル

| モデル | 特徴 |
|--------|------|
| MDP（マルコフ決定過程） | 状態が完全観測可能。(S, A, P, R)のタプル |
| POMDP（部分観測MDP） | 状態が部分的にしか観測できない。信念状態による推論が必要。LLMエージェントの現実世界操作はほぼこれ |
| 強化学習（RL） | 報酬最大化のための探索と搾取。Model-native agentへの進化を支える |

### 2-4. マルチエージェントシステム（MAS）の理論

- **協調**: 共通目標のための分業・情報共有
- **競争**: 限られたリソースの獲得
- **通信プロトコル**: エージェント間のメッセージパッシング
- **創発的振る舞い**: 個々のルールから集団的な知的行動が出現

### 2-5. エラー伝搬と信頼性

AgentBench（Liu et al., ICLR 2024）の結果：GPT-4ですら複雑なマルチステップタスクでの成功率は約35%。各ステップでの失敗確率が連鎖的に蓄積する。10ステップで1ステップ95%の精度でも、全体の成功率は60%程度に崩壊する。

---

## 3. エンジニアリング（実装）レベルで抜け落ちている要素

### 3-1. プロトコル層

あなたの調査で最も大きな欠落は「エージェントが外部とどう接続するか」の標準化動向。

#### MCP（Model Context Protocol）

- **発表**: Anthropicが2024年11月にオープンソースとして公開
- **目的**: LLMアプリケーションと外部データソース・ツールの統合を標準化
- **アーキテクチャ**: JSON-RPC 2.0ベース。Host（LLMアプリ）→Client→Server構成
- **採用**: OpenAI（2025年3月正式採用）、Google DeepMind、Microsoft、各種IDE
- **意義**: N×M問題（N個のAIツール × M個のデータソース）をN+M問題に変換
- **2025年12月**: AnthropicがLinux Foundation傘下のAgentic AI Foundation（AAIF）に寄贈
- **セキュリティ課題**: ツールポイズニング、プロンプトインジェクション、クロスサーバーシャドウイングなど

#### A2A（Agent2Agent Protocol）

- **発表**: Google Cloud が2025年4月に発表
- **目的**: エージェント同士の通信と協調を標準化（MCPがツール接続なら、A2Aはエージェント間接続）
- **構成要素**: Agent Card（エージェント名刺）、Task管理、モダリティ交渉
- **採用**: 150以上の組織が参加。Linux Foundationに移管（2025年6月）
- **現状**: MCPに比べて採用速度は遅い。開発者体験の複雑さが課題

**MCPとA2Aの関係**: 補完関係にある。MCPは「エージェントがツール/データに接続する」ためのプロトコル、A2Aは「エージェント同士が対話・協調する」ためのプロトコル。

### 3-2. エージェントフレームワーク

| フレームワーク | 特徴 | ユースケース |
|----------------|------|--------------|
| LangChain / LangGraph | 最も広く使われるOSS。グラフベースのワークフロー定義 | 汎用エージェント開発 |
| CrewAI | ロールベースのマルチエージェント協調 | チーム型タスク分担 |
| AutoGen (Microsoft) | 会話型マルチエージェント。コード生成に強い | 開発・分析タスク |
| Google ADK | A2Aプロトコルのネイティブサポート | Googleエコシステム統合 |
| OpenAI Agents SDK | MCPサポート込み。Responses APIベース | OpenAIエコシステム |

### 3-3. ブラウザ/コンピュータ操作エージェント（あなたのドメインに直結）

**APIがないSaaSを操作する唯一の手段**。あなたの旅館DXケースに最も関係が深い。

| ツール | 特徴 | ベンチマーク |
|--------|------|-------------|
| Browser Use（OSS） | Playwright/Puppeteer統合、最も活発なOSS | WebVoyager 89.1% |
| Skyvern | LLM+CV、SOP文書からのワークフロー生成 | WebVoyager 85.8% |
| OpenAI Operator / CUA | GPT-4oベース、2025年1月ローンチ | WebVoyager 87%, WebArena 58.1% |
| Anthropic Computer Use | Claude 3.5ベース、スクリーンショット→操作 | - |
| Google Gemini Computer Use | 2025年10月発表、70%+精度、225秒レイテンシ | - |
| Amazon Nova Act | ブラウザ操作特化の基盤モデル | - |

これらのツールの本質は、「Web UIしか持たないSaaSに対して、AIがスクリーンを見て人間のように操作する」こと。あなたの制約（公開APIなし、Webhookなし、Web UIのみ）に対する直接的解答となる。

### 3-4. メモリアーキテクチャ

| メモリタイプ | 実装 | 対応 |
|-------------|------|------|
| Short-term Memory | コンテキストウィンドウ（In-context learning） | 作業記憶 |
| Long-term Memory | Vector DB（ChromaDB, pgvector）+ RAG | 長期記憶 |
| Episodic Memory | 過去のインタラクション記録の保存・検索 | エピソード記憶 |
| Procedural Memory | ファインチューニング、RL | 手続き記憶 |

Stack Overflowの2025年調査によると、開発者がエージェントのメモリ/データ管理に使うツールは：Redis（43%）、ChromaDB（20%）、pgvector（18%）。

### 3-5. 評価とベンチマーク

| ベンチマーク | 対象 |
|-------------|------|
| AgentBench | 8環境での汎用エージェント評価 |
| WebArena | Webブラウジングタスク |
| WebVoyager | クロスサイトWeb操作 |
| SWE-bench | ソフトウェアエンジニアリングタスク |
| MINT | マルチターンツール使用 |

---

## 4. ビジネスレベルで抜け落ちている要素

### 4-1. 市場規模と採用率

- AIエージェント市場：2024年に54億ドル → 2025年に76億ドル（YoY 40%成長）
- 2030年までに503億ドル規模へ（CAGR 45.8%）
- PwC調査（2025年5月）：79%の企業がAIエージェントを採用済み、うち66%が生産性向上を実測
- 88%の経営幹部が今後12ヶ月でAI関連予算を増額予定

### 4-2. 現実的な導入パターン

LangChainの2024年調査（1,300名以上）による：

1. **リサーチ・要約**（58%）- 最も多い用途
2. **個人の生産性向上**（53.5%）
3. **カスタマーサービス**（45.8%）
4. 本番環境でエージェントを使用中：全体の51%（中堅企業では63%）

### 4-3. あなたのドメイン（旅館DX）への示唆

**問題の本質**：小規模旅館のSaaSスタックは「API非公開のレガシーSaaS群」で構成されている。

| 課題 | エージェント的解法 |
|------|-------------------|
| OTA管理画面にAPIがない | Browser Use / Skyvern でWeb UIを直接操作 |
| 予約通知がHTMLメールのみ | Gmail API でインジェスト → LLMで構造化データ抽出 |
| CSVが手動エクスポート | ブラウザエージェントが定期的にログイン→DL→パース |
| シフト管理SaaSとの連携 | MCP Serverとして旅館の統合データを公開 |
| 複数システム間のデータ一貫性 | Supervisor Agentが横断的に整合性チェック |

**あなたの立ち位置の強み**: 「APIがないSaaS同士を統合する」ニーズは非常に大きいが、大手テック企業はAPI-first/Headless SaaSの世界に投資している。あなたが狙う「レガシーSaaS × ブラウザエージェント × ドメイン特化」のニッチは、実はブルーオーシャンに近い。

### 4-4. ビジネスモデルの視点

| モデル | 説明 | リスク |
|--------|------|--------|
| Per-execution課金 | タスク実行回数に応じた従量制 | LLM APIコストの不確実性 |
| Seat-based | 月額定額でユーザー数ベース | 採用ハードルが上がる |
| Outcome-based | 成果（予約数、工数削減等）に連動 | 測定・帰属の難しさ |
| Integration-as-a-Service | 統合自体をサービスとして提供 | 対象SaaSの変更リスク |

---

## 5. 学部生・高校生レベルで抜け落ちている基礎概念

### 5-1. 計算機科学の基礎

| 概念 | なぜ必要か |
|------|-----------|
| 有限状態機械（FSM） | エージェントの行動状態遷移の基礎。あなたは既に得意 |
| オートマトン理論 | FSMの数学的基盤。形式言語と受理能力 |
| グラフ理論 | ワークフローの表現、依存関係の解析 |
| 計算量理論（P, NP） | タスクの計算可能性と効率の限界を理解 |
| 並行性・並列性 | マルチエージェントの同時実行と競合 |

### 5-2. 確率・統計の基礎（LLMの根幹）

| 概念 | LLMエージェントとの関係 |
|------|----------------------|
| ベイズ推論 | 信念の更新、不確実性下の意思決定 |
| 条件付き確率 | 次トークン予測の数学的基盤 |
| 情報理論（エントロピー） | LLMの損失関数の基盤 |
| マルコフ連鎖 | MDPの基盤、状態遷移モデル |

### 5-3. 機械学習の基礎

| 概念 | 説明 |
|------|------|
| 教師あり学習 vs 教師なし学習 | 基本的な学習パラダイム |
| 強化学習（RL） | 報酬最大化。エージェントの学習メカニズムの根幹 |
| Transformerアーキテクチャ | LLMの基盤。Self-Attention機構 |
| Fine-tuning vs Prompting | モデルの適応方法の違い |
| RLHF | 人間のフィードバックからの強化学習。ChatGPT等の基盤 |

### 5-4. ソフトウェアエンジニアリングの基礎

| 概念 | エージェント開発での意味 |
|------|------------------------|
| デザインパターン（GoF） | Observer、Strategy、State等がエージェントに直結 |
| イベント駆動アーキテクチャ | エージェントのトリガーとリアクション |
| マイクロサービス | エージェントの分散配置 |
| API設計（REST, gRPC） | MCP/A2Aの通信基盤 |
| 冪等性（Idempotency） | エージェントのリトライ安全性 |

---

## 6. 読むべき論文リスト

### Tier 1: 必読（エージェントの根幹）

| # | 論文 | 会議 | 概要 |
|---|------|------|------|
| 1 | **ReAct: Synergizing Reasoning and Acting in Language Models** (Yao et al.) | ICLR 2023 | Thought→Action→Observationループの定式化。現代エージェントの最も基本的なアーキテクチャパターン |
| 2 | **Chain-of-Thought Prompting Elicits Reasoning in Large Language Models** (Wei et al.) | NeurIPS 2022 | 推論過程を言語化することでLLMの問題解決能力が向上することを示した。CoTの原点 |
| 3 | **Toolformer: Language Models Can Teach Themselves to Use Tools** (Schick et al.) | NeurIPS 2023 | LLMが自ら道具の使い方を学習できることを示した。ツール使用の理論的基盤 |
| 4 | **A Survey on Large Language Model based Autonomous Agents** (Wang et al.) | Frontiers of Computer Science 2024 | 最も包括的なサーベイ。Planning, Memory, Tool Useの3柱を定義 |
| 5 | **Cognitive Architectures for Language Agents** (Sumers et al.) | TMLR 2024 | LLMエージェントを古典的認知アーキテクチャ（SOAR, ACT-R）と接続。理論的深みを与える |

### Tier 2: 重要（アーキテクチャと拡張）

| # | 論文 | 会議 | 概要 |
|---|------|------|------|
| 6 | **Tree of Thoughts: Deliberate Problem Solving with LLMs** (Yao et al.) | NeurIPS 2023 | 線形なCoTを木構造に拡張。探索と評価によるより深い推論 |
| 7 | **Reflexion: Language Agents with Verbal Reinforcement Learning** (Shinn et al.) | NeurIPS 2023 | 失敗からの自己反省メカニズム。エージェントの自己改善の基盤 |
| 8 | **HuggingGPT: Solving AI Tasks with ChatGPT and its Friends** | NeurIPS 2023 | LLMがHugging Face上のAIモデルをオーケストレーション。タスク計画→モデル選択→実行→要約 |
| 9 | **AgentBench: Evaluating LLMs as Agents** (Liu et al.) | ICLR 2024 | 8環境でのエージェント性能ベンチマーク。GPT-4でも成功率35%という現実を示す |
| 10 | **Language Agent Tree Search Unifies Reasoning, Acting, and Planning** | ICML 2024 | ReActとToTを統合。MCTSによるエージェント探索 |

### Tier 3: 応用（ブラウザ/Web/実世界エージェント）

| # | 論文 | 概要 |
|---|------|------|
| 11 | **WebArena: A Realistic Web Environment for Building Autonomous Agents** (Zhou et al., ICLR 2024) | リアルなWebサイト群でのエージェント評価環境 |
| 12 | **AutoWebGLM: A Large Language Model-based Web Navigating Agent** (Lai et al.) | Web操作エージェントの最前線 |
| 13 | **Executable Code Actions Elicit Better LLM Agents** (Wang et al., ICML 2024) | CodeActパターン。Pythonコードをアクション空間として使用 |
| 14 | **ChemCrow: Augmenting LLMs with Chemistry Tools** (Bran et al., Nature Machine Intelligence 2024) | ドメイン特化エージェントの成功例。汎用より専門が勝つ |

### Tier 4: マルチエージェントとプロトコル

| # | 論文/文献 | 概要 |
|---|-----------|------|
| 15 | **Large Language Model based Multi-Agents: A Survey of Progress and Challenges** | マルチエージェントシステムの包括サーベイ |
| 16 | **The Rise and Potential of Large Language Model Based Agents: A Survey** | エージェント全体の大規模サーベイ |
| 17 | **MCP仕様書** (modelcontextprotocol.io/specification) | 実装の公式リファレンス |
| 18 | **A2A仕様書** (github.com/a2aproject/A2A) | エージェント間通信の公式リファレンス |

### Tier 5: パラダイムシフト（最新動向）

| # | 論文 | 概要 |
|---|------|------|
| 19 | **Model-native Agentic AI Survey** (ADaM-BJTU, 2025) | パイプライン型→モデルネイティブ型へのパラダイムシフト。RL+LLMの統合 |
| 20 | **Agentic AI: A Comprehensive Survey of Architectures, Applications, and Future Directions** (2025) | シンボリック vs ニューラルの二重パラダイム分類。概念の誤用（conceptual retrofitting）問題を指摘 |

---

## 7. 深掘り調査：あなたのドメインに直結する知見

### 7-1. API非公開SaaSへのエージェント統合戦略

あなたの最大の課題「APIがないSaaS同士の統合」に対して、2025年時点で利用可能な技術スタックは以下の通り：

**アプローチA: ブラウザエージェント（Web UI操作）**

仕組み：LLM + Computer Vision がスクリーンショットを解析 → マウス/キーボード操作を生成 → ブラウザで実行

メリット：
- 任意のWebアプリに対応（API不要）
- UIが変わってもLLMが「Submit ボタン」を認識して適応
- XPathベースの従来型RPAより頑健

課題：
- レイテンシ（1操作あたり数秒〜十数秒）
- ボット検知対策
- 認証情報の安全な管理
- エラーリカバリの難しさ

**アプローチB: HTMLメールインジェスト（あなたが既に着手）**

仕組み：Gmail API → HTMLパース → LLMで構造化データ抽出 → Firestoreに格納

メリット：
- リアルタイム性が高い（メール受信がトリガー）
- 予約確認、変更、キャンセル通知をキャプチャ可能

発展形：
- LLMによるメール分類（予約確認 / 変更 / キャンセル / プロモーション）
- 構造化スキーマへの自動マッピング
- 異常検知（二重予約、日付矛盾等）

**アプローチC: CSV定期エクスポート自動化**

仕組み：ブラウザエージェントが定期的にSaaSにログイン → CSV DL → Pandasパイプラインで正規化

メリット：
- バッチ処理に適する
- データ品質が比較的安定

課題：
- リアルタイム性がない
- エクスポート形式の変更に弱い

**推奨ハイブリッド構成**

```
[HTMLメール] ──→ Gmail API ──→ LLM構造化 ──→ Firestore
                                                ↑
[OTA管理画面] ──→ ブラウザAgent ──→ データ抽出 ─┘
                                                ↑
[シフトSaaS]  ──→ ブラウザAgent ──→ データ抽出 ─┘
                                                ↑
[POS]        ──→ CSV自動DL ──→ Pandas ETL ─────┘
                                                │
                              Firestore ←───────┘
                                │
                         MCP Server として公開
                                │
                    Claude / GPT 等のAIアシスタントが利用
```

### 7-2. あなたの技術スタックとの適合度

| あなたの技術 | エージェント開発での活用 |
|-------------|------------------------|
| Next.js + TypeScript | エージェント管理ダッシュボード、MCP Client UI |
| Firestore | エージェントの状態管理、タスクキュー、データストア |
| Cloud Run + Docker | エージェントプロセスのホスティング、MCP Serverのデプロイ |
| FSM設計 | エージェントのタスクライフサイクル管理に直結（状態遷移はエージェントの核心） |
| Playwright (E2E) | ブラウザエージェントの操作基盤そのもの |
| Gmail APIインジェスト | すでに確立済み。メールトリガーのエージェントワークフロー |
| Pandas ETL | CSV正規化パイプライン。バッチ処理エージェントのデータ処理層 |

あなたの既存スタックは、エージェント開発に驚くほどフィットしている。特にPlaywright + FSM + Gmail APIの組み合わせは、ブラウザエージェントのコア技術に直結する。

---

## 8. 自由調査：あなたへの提言

### 8-1. 研究テーマの方向性

あなたのビジョン「APIを持たないSaaS同士をAIエージェントで統合する」は、以下の研究領域に位置づけられる：

**学術的フレーミング案**: 「Web UIのみを持つレガシーSaaSに対するLLMベースのブラウザエージェントによるオペレーション統合」

関連するサブテーマ：
1. **頑健性**: WebサイトのDOM変更に対するエージェントの適応能力
2. **信頼性**: マルチステップ操作のエラー率低減手法
3. **効率性**: 操作あたりのLLM APIコストとレイテンシの最適化
4. **安全性**: 認証情報管理、意図しない操作の防止
5. **評価**: ドメイン特化（ホスピタリティ）でのベンチマーク構築

### 8-2. 実践的ロードマップ

**Phase 1（3月）**: 基盤理解
- ReAct論文を精読し、自分で簡易実装
- Browser Use（OSS）をローカルで動かし、OTA管理画面の操作を試す
- MCP仕様書を読み、簡単なMCP Serverを実装

**Phase 2（4月）**: プロトタイプ
- 旅館の実データ（メール、CSV）でパイプライン構築
- ブラウザエージェントでOTA操作の自動化PoC
- Firestoreを中心としたデータ統合層の設計

**Phase 3（5月〜）**: 研究と発展
- エラーパターンの体系的収集と分類
- 操作成功率の計測と改善手法の検討
- 論文化の方向性決定

### 8-3. 競合環境の理解

あなたと同じ領域（レガシーSaaS × AIエージェント統合）のプレイヤー：

| プレイヤー | アプローチ | 弱点 |
|-----------|-----------|------|
| Skyvern | 汎用Web操作。SOP文書入力 | ドメイン非特化、高コスト |
| Airtop | 自然言語→ブラウザ操作。SOC2準拠 | 汎用すぎる |
| 従来型RPA（UiPath等） | ルールベース自動化 | UI変更に弱い、AI非統合 |
| 各OTAの公式API | Booking.com, Expedia等のAPI | 小規模旅館には複雑すぎ / 非対応 |

**あなたの差別化ポイント**: ドメイン特化（旅館）+ 統合レイヤー（単なる操作自動化ではなく、データの正規化と横断的整合性）+ 日本市場特化（日本語SaaS、日本の旅館業界の商慣行）

### 8-4. 最も重要な気づき

2025年のAIエージェント分野における最も重要なパラダイムシフトは、**パイプライン型からモデルネイティブ型への移行**である。

- **パイプライン型**: Planning, Tool Use, Memoryを外部ロジックで制御（LangChain的）
- **モデルネイティブ型**: これらの能力をモデルのパラメータ内に内在化（RL + LLMの統合）

現時点ではパイプライン型が主流だが、OpenAI o1/o3やDeepSeek-R1に見られるように、推論・計画能力がモデル内部に取り込まれつつある。あなたの実装はパイプライン型から始めるのが現実的だが、この長期トレンドを意識しておくと良い。

---

## 参考URL

- MCP仕様: https://modelcontextprotocol.io/specification
- A2Aリポジトリ: https://github.com/a2aproject/A2A
- Browser Use: https://github.com/browser-use/browser-use
- LLM-Agent-Survey (CoLing 2025): https://github.com/xinzhel/LLM-Agent-Survey
- Model-native Agentic AI Survey: https://github.com/ADaM-BJTU/model-native-agentic-ai
- LangChain State of AI Agents: https://www.langchain.com/stateofaiagents
- Stack Overflow 2025 AI Survey: https://survey.stackoverflow.co/2025/ai

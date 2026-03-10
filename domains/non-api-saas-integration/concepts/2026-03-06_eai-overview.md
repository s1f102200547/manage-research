いい質問。EAIと同レベルの抽象度で、隣接・重複・上位にある概念を探す。十分揃った。EAIと同じ抽象度の概念群を整理する。

---

その通り。EAIは「エンタープライズのアプリケーションを統合する」という問題領域全体の総称であり、その中にESBやCDMなどの要素技術がある。EAIと同じ粒度で並ぶ概念は、大きく分けて3つのグループに整理できる。

## 1. EAIの「入れ子構造」：包含関係にあるもの

この3つの関係はロシアのマトリョーシカ人形に例えられる。EAIが最も外側、SOAが中間、ESBが最も内側。つまり：

**EAI（Enterprise Application Integration）** — 最も広い問題定義。「企業内の複数アプリケーションのデータとプロセスを統合する」という目標そのもの。

**SOA（Service-Oriented Architecture）** — EAIを達成するための設計原則。ビジネス機能を再利用可能なサービスとして公開し、標準プロトコルで通信させるアーキテクチャスタイル。EAIが「何を」なら、SOAは「どういう思想で」。

**ESB（Enterprise Service Bus）** — SOAを実装するための具体的なインフラ。メッセージルーティング、プロトコル変換、オーケストレーションを担うミドルウェア。Apache Camel、MuleSoft、WSO2が代表。SOAが「思想」なら、ESBは「道具」。

## 2. EAIと同レベルで並ぶ隣接概念

**EII（Enterprise Information Integration）**
EAIがアプリケーション間のプロセスとデータの統合を扱うのに対し、EIIはデータの統合だけにフォーカスする。複数のデータソースを仮想的に一つのビューとして見せる技術で、Data Virtualizationが代表的な実装。物理的にデータを移動させずにクエリ時に結合する。

**ETL / ELT（Extract, Transform, Load）**
データ統合の古典的手法。ソースからデータを抽出し、変換し、ターゲット（DWH等）にロードする。EAIとの違いは、ETLがバッチ処理・分析目的のデータパイプラインなのに対し、EAIはリアルタイムのプロセス連携を含む。ただし現代のELTはリアルタイムに近づいている。

**iPaaS（Integration Platform as a Service）**
iPaaSはEAIの傘の下にある、より新しいクラウドベースのモデル。Zapier、Make、Workato、MuleSoft Anypoint、Boomiが代表。プリビルトコネクタとローコードでAPI統合を提供する。大企業ではEAIとiPaaSが異なるオーケストレーション層で併用されることが多い。

**EDI（Electronic Data Interchange）**
EAIが主に企業内部のデータ共有を助けるのに対し、EDIは組織間の情報転送を標準化し促進する。請求書、出荷通知などの構造化文書を、X12やEDIFACTといった標準プロトコルで企業間交換する。1960年代から存在する最古の統合手法の一つ。

**RPA（Robotic Process Automation）**
GUI操作を記録・再生して業務を自動化する技術。EAIがシステム層の統合なのに対し、RPAはUI層の自動化。APIがないシステムへの接続手段として使われることがあるが、中央DBや横断的な状態管理を持たない点がHaruのアーキテクチャと根本的に違う。

**MOM（Message-Oriented Middleware）**
非同期メッセージングでシステムを疎結合にするミドルウェア。RabbitMQ、Apache Kafkaが代表。EAIやESBの内部で使われる通信基盤。EAIが「統合の枠組み全体」なら、MOMは「その中の通信レイヤー」。

**API Management**
APIのライフサイクル（設計・公開・セキュリティ・モニタリング・バージョニング）を管理するプラットフォーム。Kong、Apigee、AWS API Gateway。EAIの中の「接続手段の管理」に特化した概念。

**MDM（Master Data Management）**
顧客・商品・従業員などのマスターデータの一貫性を企業横断で保つための手法。「CDMはメッセージ形式の統一」、「MDMはビジネスエンティティの統一」という住み分け。Haruのプロジェクトでは、Central DBのスキーマ設計にMDM的な発想が必要になる（「ゲスト」を各SaaSでどう一意に特定するか等）。

## 3. 時代的に進化した概念

**Unified API**
同カテゴリの複数SaaS（CRM 10社分など）を1つのAPIで抽象化するアプローチ。Merge.dev、Apideck、Finch。iPaaSの次の進化形で、開発者は各SaaSのAPI仕様を知る必要がない。ただし対象SaaSがAPIを公開していることが大前提。

**Event-Driven Architecture (EDA)**
イベント（状態変化の通知）を中心にシステムを設計するアーキテクチャスタイル。SOAがリクエスト/レスポンス中心なのに対し、EDAはPublish/Subscribe中心。Apache Kafka、AWS EventBridge。Haruのシステムの「DB変更をトリガーにSaaSを更新する」部分はEDA的。

**Data Mesh**
ドメインごとにデータオーナーシップを分散させるアーキテクチャ思想。中央集権的なデータチーム（Data Warehouse/Data Lake）の反動として2019年にZhamak Dehghaniが提唱。Haruのプロジェクトは逆にCentral DBで集権化する構造なので、Data Meshとは対極にある。ただし「なぜ集権化が必要か」を説明する際にData Meshとの対比が使える。

**Composable Architecture / MACH Architecture**
Microservices・API-first・Cloud-native・Headlessの頭文字。SaaSをレゴブロックのように組み替えられる設計思想。これも「APIが前提」の世界の話。

---

## Haruの研究にとっての意味

これらの概念マップをHaruのプロジェクトに重ねると、こうなる：

EAI/SOA/ESB/iPaaS/Unified APIはすべて**「システムがAPIまたはサービスインターフェースを公開している」**ことを前提にしている。RPAだけがGUI層にアクセスするが、中央の状態管理（CDM相当）を持たない。ETL/ELTはデータ統合をするが双方向write-backがない。

Haruのプロジェクトは「EAIの問題設定＋RPAの接続手段＋ETLのデータ正規化＋EDAのイベント駆動＋AI Agentの判断層」を一つのアーキテクチャに組み合わせたもので、既存のどの単一カテゴリにも入らない。論文のRelated Workでは、これらの概念を系譜として引いた上で「それぞれの前提が崩れる条件」を示し、その交差点に自分の研究を置く、という書き方が最も強くなる。
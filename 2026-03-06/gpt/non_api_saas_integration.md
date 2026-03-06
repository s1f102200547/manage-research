チーム全員が読みやすいように、**特定サービス名を使わない抽象化版**に整理します。
（SquareのようにAPIがある例は今回は外します）

---

# APIがないSaaSを統合する仕組み（チーム向け概要）

## 1. 解決したい問題

多くの業務SaaSは

* APIがない
* APIが不完全
* Webhookがない

そのため、通常の方法では

```text
SaaS ↔ SaaS
```

の直接連携ができません。

結果として

* 同じデータを複数システムに入力
* データがバラバラ
* 自動化できない

という問題が起きます。

---

# 2. 基本アイデア

すべてのSaaSのデータを

**一度中央データベースに集約する**

```text
SaaS A
SaaS B
SaaS C
   ↓
Polling
   ↓
Central Database
```

Pollingとは

**一定時間ごとにデータを取得する処理**

です。

例

```
5分ごとに予約取得
10分ごとに顧客データ取得
```

---

# 3. データ取得（Read）

各SaaSからデータを定期取得します。

```
SaaS A ─┐
SaaS B ─┼─→ Polling → Central DB
SaaS C ─┘
```

これにより

**すべての情報が中央DBに集まります。**

---

# 4. 中央DBの役割

中央DBには

**統合されたデータ形式**

で保存します。

例

```
reservation
 ├ guest_name
 ├ dinner_time
 ├ source_system
 └ reservation_id
```

このDBは

**システム全体の正しい状態（Single Source of Truth）**

になります。

---

# 5. データの利用方法

中央DBのデータは2つの方法で使えます。

---

## パターンA

### 自作アプリから利用

```
Central DB
   ↓
Custom App
```

例

* 予約ダッシュボード
* 管理画面
* 売上分析

---

## パターンB

### AIエージェントが利用

```
Central DB
   ↓
AI Agent
   ↓
判断
```

例

```
予約増加
↓
AI判断
↓
スタッフ追加
```

---

# 6. データ更新（Write）

中央DBの変更を

**元のSaaSに反映します**

```
Custom App
   ↓
DB update
   ↓
Trigger
   ↓
SaaS update
```

例

```
予約時間変更
↓
DB更新
↓
SaaS A更新
```

---

# 7. 全体構造

```
        SaaS A
           │
        SaaS B
           │
        SaaS C
           │
         Polling
           ↓
        Central DB
           │
     ┌─────┴─────┐
     │           │
Custom App     AI Agent
     │           │
     └─────┬─────┘
           ↓
        SaaS Update
```

---

# 8. この仕組みのメリット

### APIがなくても統合できる

SaaSの制限に依存しない。

---

### SaaSを横断したデータ管理

例

* 予約
* 顧客
* 売上
* シフト

をまとめて扱える。

---

### AIによる自動運用

中央DBを見てAIが判断できる。

例

```
予約増加 → スタッフ追加
売上減少 → プロモーション
```

---

# 9. 一言で言うと

このプロジェクトは

**APIがないSaaSを統合する中央データ基盤**

を作る取り組みです。

---

もし希望があれば、次に

* **チーム用1枚アーキテクチャ図**
* **技術スタック版（Cloud Run / Firestore / Gmail Listener）**
* **実際のデータフロー（予約→DB→AI→更新）**

を整理します。
これは**エンジニア以外のメンバーでも一発で理解できる図**になります。

実際のSE論文を参照しながら調査する。実際の論文テキストを直接取得する。十分な実例が取得できた。これをもとに詳細に解説する。

---

## CS/SE論文の大枠構成【実論文例付き改訂版】

参照論文：
- **AgentOccam** (Yang et al., 2024, ICLR) — LLMベースWebエージェント
- **WebDreamer** (2024, arXiv) — LLMワールドモデルによるWeb計画
- **Building Browser Agents** (2024, arXiv) — ブラウザエージェント設計

Haruの卒業研究テーマ（APIなしSaaS統合）と直接隣接するドメインの論文を選んでいる。

---

### 全体構造の実例

AgentOccamの目次構成は：Abstract → 1. Introduction → 2. Related Work → 3. Problem Formulation → 4. Method（4.1 Action Space Alignment / 4.2 Observation Space Alignment）→ 5. Experimental Results and Analysis → 6. Conclusion → Appendix という構成になっている。

重要な点は、前回の説明にあった「Implementation」セクションが**独立していない**ことだ。SE論文の種別によって構造が変わる。

```
【ツール・システム提案系】         【評価・実証系（Empirical）】
Abstract                          Abstract
Introduction                      Introduction
Related Work                      Related Work  
Problem Formulation               Study Design / Methodology
Method / Approach                 Results
  ├ System Design                 Discussion
  ├ Implementation                Threats to Validity
  └ (詳細はAppendixへ)            Conclusion
Evaluation / Experiments
Conclusion
Appendix
```

---

### 各セクション：実際の英文と解説

---

#### Abstract — 4要素を厳密に圧縮する

実際のAbstractをStructureごとに分解して示す。

**AgentOccamのAbstract（実文）:**

> *"Autonomy via agents based on large language models (LLMs) that can carry out personalized yet standardized tasks presents a significant opportunity to drive human efficiency. There is an emerging need and interest in automating web tasks (e.g., booking a hotel for a given date within a budget). Being a practical use case itself, the web agent also serves as an important proof-of-concept example for various agent grounding scenarios, with its success promising advancements in many future applications. Meanwhile, much prior research focuses on handcrafting their web agent strategies..."*

この冒頭部分を4要素に分解すると：

```
[Motivation]  "...presents a significant opportunity to drive
               human efficiency."
[Problem]     "There is an emerging need...in automating web tasks"
[Gap]         "much prior research focuses on handcrafting
               their web agent strategies"
[Proposal]    → この後にAgentOccamの提案が続く
```

**Abstractで使われる典型的な動詞パターン：**

```
提案を示す    We propose / We present / We introduce / In this paper, we...
問題を示す    X remains challenging / X is limited by / X fails to...
結果を示す    Experiments demonstrate / Results show / Evaluation reveals...
貢献を示す    Our approach achieves / This enables / X outperforms...
```

---

#### Introduction — 5ムーブ構造を実文で確認

**WebDreamerのIntroduction冒頭（実文）:**

> *"Driven by the goal of automating tedious and repetitive web-based tasks, web agents powered by (multimodal) language models have made substantial progress in various aspects."*

これが**Move 1（Motivation）**。「なぜこの研究が重要か」を1文で示している。

続いて**Move 2（Problem）** と **Move 3（Gap）**：

> *"However, their effectiveness is significantly constrained when task-specific observation and action representations diverge from the parametric knowledge encoded during training of LLMs. For instance, in web-based tasks, these agents perform notably below human levels."*

`However` で始まる転換が**Gapの開始を示す典型的なシグナル**。

**Move 4（Proposal）** の書き方：

> *"In this work, we aim to enhance an LLM-based web agent's proficiency by optimizing the text-based task understanding and reasoning of existing LLMs, rather than refining the agent strategies."*

`In this work, we...` は論文のIntroductionで最頻出のProposalの書き出しパターン。

**Move 5（Contributions）** の書き方：

SE論文の貢献リストは必ず箇条書きで、かつ動詞始まりになる。

```
"The main contributions of this paper are as follows:
(1) We propose...
(2) We implement...
(3) We evaluate... and demonstrate that..."
```

---

#### Related Work — 差別化が目的

単なる紹介ではなく「自分の研究がなぜ必要か」を証明する場所。

**AgentOccamのRelated Workの差別化パターン（実文）:**

> *"While those pre-defined strategies can be effective for certain tasks, they struggle to generalize to diverse websites and varying skill requirements. Another emerging trend is to adopt sampling or search algorithms for a dynamic exploration of web navigation actions, which reduces dependence on pre-defined strategies but increases the cost of LLM inferences."*

構造を抽出すると：

```
"While [先行研究X] can be effective for [条件A],
 they struggle to [問題B].
 Another [アプローチY] reduces [課題C]
 but increases [別の課題D]."
```

これが**Contrastive Related Work**の基本テンプレート。先行研究を否定せず、限界を指摘する書き方。

---

#### Problem Formulation — SE論文固有のセクション

数式・定義でシステムの入出力を厳密に記述する。

**実例（WebOperator論文）:**

> *"LLM-based WebAgents are increasingly applied to automate complex web interactions, ranging from form filling and content retrieval to multi-step workflows over dynamic pages. However, planning and executing such tasks remains challenging due to unique characteristics of web environments, such as being partially observable: the agent can access the current page's DOM, UI elements, and visible content, but has no direct access to hidden server-side state or the broader global context."*

**問題の定式化に使われる典型表現：**

```
We define X as...
Formally, the problem can be stated as...
Given [input], the goal is to [output]
X is modeled as a tuple (A, B, C) where...
```

---

#### Evaluation — RQ駆動の実例

**AgentOccamのEvaluationセクション（実文 / RQ形式）:**

AgentOccamの評価は3つのResearch Questionで構成されており、「AgentOccamはどの程度のパフォーマンスを示すか」「各観察・行動空間の変更がどれだけ貢献するか」「他のエージェント戦略と組み合わせることができるか」という問いに順番に答える構造になっている。

これをテンプレートとして書くと：

```
5. Evaluation

5.1 Experimental Setup
    "We evaluate on [benchmark] with [baseline systems].
     All experiments were conducted with [model] under
     [conditions]."

RQ1: [具体的な問い]
     "Table 1 shows that our approach achieves X%,
      outperforming the best baseline by Y points."

RQ2: [ablation study]
     "To isolate the contribution of each component,
      we conduct an ablation study. Removing [A]
      leads to a X% drop, suggesting that..."
```

---

#### Discussion — Threats to Validityの書き方

SE論文特有の、**結果の限界を自己申告するセクション**。査読者に先んじて弱点を開示することで誠実さを示す。

**実際の論文でよく使われる書き方：**

```
"Our study has several limitations.
 First, our evaluation is conducted on [benchmark],
 which may not fully represent real-world [domain].
 Second, while our approach demonstrates X,
 it does not address [Y], which we leave as future work."
```

---

### Haruの卒業研究への直接マッピング

```
セクション         内容案                          参考論文パターン
──────────────────────────────────────────────────────────────────
Abstract          3層フレームワーク提案 +           AgentOccam形式
                  旅館DXでの評価結果

Introduction      日本SaaSのAPIなし問題(M1)         WebDreamer冒頭
                  既存RPA/Browser Useの限界(M3)     AgentOccam Gap表現
                  3層提案(M4) + 貢献3点(M5)

Problem           APIなし統合を                     WebOperator形式
Formulation       状態遷移として定式化

Related Work      RPA / GUI Grounding /             Contrastive形式
                  Browser Use との差別化

Method            知覚層・操作層・正規化層            サブセクション構成

Evaluation        RQ1: 状態認識精度                 AgentOccam RQ形式
                  RQ2: UI変更への耐性
                  RQ3: 旅館ユースケース実証

Discussion        日本固有SaaSへの汎化限界           Threats to Validity
```

次のステップとして、IntroductionのMotivation（Move 1〜3）を実際に英語で書いてみると、ここまでの構造理解が一気に定着する。書いたらフィードバックする。
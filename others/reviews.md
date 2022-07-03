# abductive reasoning

## general
* Generating Hypothetical Events for Abductive Inference
    - 概要: alpha-NLIを解く為に，仮説に続く事象を生成し，その事象と選択肢事象と類似度を用いて選択する．事象生成のために，TIMETRAVELコーパス(counter-factualなスクリプトを含むデータセット)で学習．
* ABDUCTIVE COMMONSENSE REASONING
    - 概要: 仮説推論タスクであるalha-NLI/NLGを提唱．
* Language Generation with Multi-Hop Reasoning on Commonsense Knowledge Graph
    - 概要: タイトルの通り．alpha-NLGなどで検証している．
    - [todo] きちんと読む．multi-hopは，ベースラインになるかもしれない．
* Hybrid Autoregressive Inference for Scalable Multi-hop Explanation Regeneration
    - 概要: science x abduction を手法Xで解いた．手法Xは，仮説の選択をautoregressiveに行う．
    - [todo] きちんと読む．
        * multi-hopは，ベースラインになるかもしれない．
        * タスクは評価に使える．
* Learning as Abduction: Trainable Natural Logic Theorem Prover for Natural Language Inference
    - 概要: NLIの含意関係を観測として，語彙関係(guy->human)をabduceする．
    - [todo] きちんと読む．タスク設定は使えるかもしれない．


## 乾研
* 言語処理のための仮説推論エンジン Phillip
    - 主要な課題は以下だという
        - Learning
            * 仮説推論の過程で"うまくいった"ルールを強めることができないか．
        - Search
            * 言語モデルの"直感"で絞れないか．
        - Knowledge
            * 言語モデルから知識を抽出
            * 言語モデルをそのまま使うアプローチがもある．
            * 文脈も考慮したい
                * => 文脈付きルール
                * 言語モデルをそのまま使う
* 共参照解析における事象間関係知識の適用
    - 仮説推論で問題となるのは，知識の量で無く，使い方だという(うまく文脈を扱えない)．
        - 挙げられている例を見ると，たしかに，単純な述語間の推論ルールを貯めていくだけでは厳しそう(スパースそう)．言語モデルの力が必要．
* 共参照解析のための事象間関係知識の文脈化
    - 文脈付きでルールを構築する．

## CCG2lambda (野村)
* 仮説推論と充足可能問題、BERTによる推理小説の知識処理
    - 概要: KG challengeを仮説推論で解く
    - 手法
        * 背景知識はタスクからgiven (e.g., motivate(x), invade(x) -> murder(s, y))
        * 観測事実は，テキストから自動で抽出．
        * 仮説推論器としてopen-davidを採用

* 含意関係認識による金融ドキュメントチェックへの取り組み
    - 概要
        * 金融ドメインにおいて，論理推論を用いた含意関係認識器を作成した．
    - 手法
        * 背景知識はなし(含意であるため)
            - 正確には，少量のみ人手で入れている．
        * 述語論理への変換は，ccg2lambda
        * 証明はCoq
        * 仮説推論をccg2lambda
            - ただし，この仮説自体は，モデルに取り込んでいない．
            - 実応用時は，担当者がこの仮説を人手で確認する．

## abduction for semi-supervised learning
* Semi-Supervised Abductive Learning and Its Application to Theft Judicial Sentencing
    - 概要
        - ドメイン知識がパラメータ + 述語論理の形で蓄えれているときに，少量ラベルから，pseudo-labelの付与 => パラメータの更新 => abductionを用いてpseudo-labelを訂正 => と繰り返すことで，良いモデルと述語論理を交互に学修する．
    - 手法
        - Fix.2
    - idea
        * [rejected] 言語理解への適用
            1. 言語処理との違いは何？
                - 本論文では， 1. ルールはgiven (ただし，一部は曖昧性があり，パラメトライズされる), 2. 観測の大部分が不正確(ここも機械学習モデルで予測する必要がある．) という設定である．
                - 一方で，言語処理では， 1. ルールはテキストデータから獲得する必要があり，十分量では無い上に，正確性も低い 2. 観測(テキスト)は完全 という設定．
            2. よって，直接適用はできない．一方，ルールのrefineの方法(derivative-free optimization)などは参考になる．
        * [rejected] derivative free optimizationの強化学習化
            - derivative-free optimizationは強化学習でepisode_length=1としたようなもの．今，episode_length=1なので，derivateie-free optimizationが最適である．
            - 行動は何か？
                * inference modelによるpseudo-labelの付与
                * abduceするときに，どのpseudo-labelを訂正最小にするのか．
    - 備考
        - Bridging論文と著者が同じ．
* Bridging Machine Learning and Logical Reasoning by Abductive Learning
    - Semi-Supervised の著者と同じ．


# neural solver

## general
* Neural Logic Machines
    - 概要: ニューロベースのソルバーを提案．
* Improving Coherence and Consistency in Neural Sequence Models with Dual-System, Neuro-Symbolic Reasoning
    - ideas
        - ここでのfilterの結果を報酬として強化学習するのはどうか．
* Improving Coherence and Consistency in Neural Sequence Models with Dual-System, Neuro-Symbolic Reasoning
    - 概要: 生成モデルは，logically inconsistentな文を生成してしまう．これを，後段のlogical reasonerで発見し，排除する．
    - 評価
        - 小さい世界(bAbI)で評価している．
        - ruleは人手で作成．
    - ideas
        - 本手法のルールを，我々のcommonsense ruleに変える．
        - 評価実験を流用する．
            * commonsenseでやりたかったことに近い気がする．
* Learning Explanatory Rules from Noisy Data
    - 概要: 微分可能なILP (inductive logic programming)を提唱した．
    - ideas
        - ILPはabductionと近い．ILP x NLPは面白そうな方向性．我々もILPを使って推論ルールを抽出できないか．
            - [transferred] ILPのお勉強をする．
* Neuro-Symbolic Inductive Logic Programming with Logical Neural Networks
    - 概要: LNN x ILP
    - [todo] 読む．ILP x NLPのために．

## logical neural networks (IBM)
* Logical Neural Networks
    - 概要: ニューロベースのソルバーを提案．従来手法より色々良い．
* A Semantic Parsing and Reasoning-Based Approach to Knowledge Base Question Answering
    - 概要: AMR -> AMR to logic -> LNNで含意判定 というパイプラインによって，データ量少ない場合でも良い性能が出る質問応答システムを構築した．
        - 各処理が使い回せるので，end-to-endデータが無くてもよい，ということ．
    - 手法
        - AMR to Logic module is a rule-based system
    - 備考
        - AMR to Logicでなくとも，CCGパーザとかでも良い?
* Leveraging Abstract Meaning Representation for Knowledge Base Question Answering
    - 概要: "A Semantic Parsing and" と同様．
* Neuro-Symbolic Reinforcement Learning with First-Order Logic
    - 概要: text-worldにおいて，観測テキストから行動を導く時にLNNを使う．
    - [todo] ちゃんと読む．この方向性は興味があるため．
* Reinforcement Learning with External Knowledge by using Logical Neural Networks
    - 概要: text-worldにおいて，観測テキストから行動を導く時にLNNを使う．
    - [todo] ちゃんと読む．この方向性は興味があるため．

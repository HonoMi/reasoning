# libraries
* ccg2lambda
    - 自然文を命題に変換する．
    - 証明
    - 仮説推論
* Coq
    - 証明
* open-david
    - 仮説推論



# neural solver系
* Neural Logic Machines







# abductive reasoning
* [todo] open-question
    - ルールを十分量獲得できるか？
    - ルールをうまく組み合わせられるか？
        - 教師あり
            * reasoning データセットの
        - 教師なし
            * 教示は？
    * ルールを有限時間で探索できるか？
    - ルールをrefineできるか？
        - 文脈による詳細化
        - ルールのスコアの精度を上げる
    * open world仮説にできるか？
        - 自然言語処理の場合は，命題の数は無限なので，open world仮説が必要．

### general
* 仮説推論と充足可能問題、BERTによる推理小説の知識処理
    - 概要: KG challengeを仮説推論で解く
    - 手法
        * 背景知識はタスクからgiven (e.g., motivate(x), invade(x) -> murder(s, y))
        * 観測事実は，テキストから自動で抽出．
        * 仮説推論器としてopen-davidを採用
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

### CCG2lambda
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

### abduction for semi-supervised learning
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


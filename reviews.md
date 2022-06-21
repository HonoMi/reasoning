# logical reasoning by statistical approach

## RuleTaker系列
* Transformers as Soft Reasoners over Language (RuleTaker)
    - 概要: formal logicの前提と結論を入出力として与えることで，Transformerがtheorem provingを実現できることを示した．
        - length 方向にもgeneralizeしているように見える．
        - ただ，簡単な推論だけ．
    - 評価
        - 自分で作ったデータセット
    - [discussions]
        - 長さ方向への汎化はできているように見える．
        - ルールやファクトの言い換えには，ゼロショット対応できていない．
            - これを事前学習できないか？
    - ideas
        - [transferred] theoryが簡単すぎる．
            * ファクトの数
            * 一度に考慮すべきルールの数
            * ルールの種類
            * n項述語
        - [transferred] 自然言語への対応
            - ルールやファクトの言い換えを事前学習したい．
            - また，ルールがless unclearな言語も取り入れたい．
        - [rejected] ルールが明示されない場合もinferしたい．
            - これは，LeapOfThoughtか．
    - **Future workがしっかりしているので，読むべき．**
    * ソースコード
        * [allenai/ruletaker](https://github.com/allenai/ruletaker)
            * theory generatorが公開されている．
* PRover: Proof Generation for Interpretable Reasoning over Rules
    - 概要: RuleTakerを，proofの生成ができるようにした．「各導出が証明に含まれるべきかどうか」を分類し，整数計画問題で組み合わせる．
    - 評価
        - CWA + OWA
    - あまり詳しくは読んでいない．ProofWriterが上位互換っぽいから．
    - ソースコード
        * [swarnaHub/PRover](https://github.com/swarnaHub/PRover)
            - 全て公開されている．
            - RuleTakerデータセットを使っている．
* ProofWriter: Generating Implications, Proofs, and Abductive Statements over Natural Language
    - 概要:
        * implicationの1-step生成を繰り返すことにより，proofを生成しつつ，解を得る．length 方向に良くgeneralizaする．
    - 評価
        * RuleTakerと同じデータセット．
            - CWA + OWA
        * ベースラインはPRover
    - ideas
        * [transferred] 「全ての導出を網羅する」は効率が悪く，大きな理論ではmax_lenからはみ出る．
            - 強化学習で必要な操作だけにできないか．
    - 備考
    * ソースコード
        * [データセットは公開されている．](https://allenai.org/data/proofwriter)
        * しかし，[リポジトリは公開されていない．](https://github.com/allenai/ruletaker/issues/26)
        * FaiRR に乗った方が良い．
* Neural Unification for Logic Reasoning over Natural Language (NeuralUnifier)
    - 概要:
        * NNにbackward chainingを模擬させた．
        * とてもよくlength generalizationする．(RuleTakerよりももっと)
    - 評価
        - RuleTakerデータセットを使う．
            - CWA
        - ベースラインはRuleTaker
    - [discussions]
        - end-to-endであることに注意．
        - この手法は，以下の強い仮定を置いている
            1. 問題はbackward chainingで解ける．
                * 問題の規模が増えたら，探索が発散して解けないように思う．
    - [transferred]
        - length normalizationしているからこれだけでいいんだっけ？
            - 全てのlogicはbackward chainingで解ける？
                * [transferred] 複雑なルールではダメだろう．
    - ソースコード
        * [IBM/Neural_Unification](https://github.com/IBM/Neural_Unification_for_Logic_Reasoning_over_Language)
            - 全て公開されている．
            - ruletakerデータセットを使っている．
* FaiRR: Faithful and Robust Deductive Reasoning over Natural Language
    - 概要:
        * ProofWriterをmodularにすることで，
            * 導出過程を信頼できる(透明性が保てる)ようにした．
            * 表層の変化にロバストになった．各moduleの入力が限られるので，自分の役割のみをこなすから．
                - c.f., ProofWriterはend-to-end generation
            * 関連ルールだけをselectするので，探索が効率的になった．
    - [ideas]
        - [todo] 本論文に載れば，e2eと性能で勝負する必要が無くなる．信頼性を推せるから．
        - [todo] subject robustness (=変項x)が低いのは，本質的な課題である気がする．
            - おそらく，「任意の記号」というお気持ちが現れていないからではないか．Johnなどを使っているため，何かJohnの表層につられてしまっているのではないか．
    - 評価
        - RuleTakerデータセットを使う．また，subject robustnessなるデータセットも自作している．
            - CWA + OWA
        - ベースラインはProofWriter
    - 備考
        * ACL2022
    * ソースコード
        * [リポジトリ](https://github.com/ink-usc/fairr)
            - 全て公開されている．
            - ProofWriterに乗っかっている．

## others
* Leap-Of-Thought: Teaching Pre-Trained Models to Systematically Reason Over Implicit Knowledge
    - 概要: 推論に当たって，LMに埋め込まれている知識(ファクト)も使えることを示した．
    - 手法
        1. is_type(X, Y), has_property(Y, Z) => has_property(X, Z) (ture/false) というデータをたくさん作る．
            - CONCEPT NETなどを使う．
        2. is_type(X, Y)は隠す．
        3. 答えを解かせる． すると，is_type(X, Y)が明示されていないにもかかわらず，解ける． => LM内部のis_type(X, Y)というファクトを使えている．
    - ideas
        - 推論ルールは固定で，2つしか使えていない．オープンドメインだと，困る？
    - ソースコード
        * [alontalmor/LeapOfThought](https://github.com/alontalmor/LeapOfThought)
            * 全て公開されている．
* Critical Thinking for Language Models
    - 概要: 演繹規則を学ぶためのデータセットを作成．また，GPT2に学習させた際に，1. 少数の規則の学習のみで複雑な規則へと汎化できること 2. NLIタスクで性能が上がること，を確認．
    - 手法
        * 演繹規則 + テンプレート
    - 評価
        * 本データセットでの評価
        * GLUE AX / SNLI
            - 適当なルールでプロンプトに置き換える．
    - ideas
        - [transferred] 性能は十分か？
            * topicに依らない
                - [transferred] いっそ記号のままで学習した方が汎化するのでは？
                    * A is B, B -> C -> C
            * 別の論理スキームも学習できる．
                * [transferred] 記号論理なら無限にサンプルを作れるので，様々なスキームを無限に学習させれば良いのでは？
        - [rejected] negationに汎化しない．どうするか？
            - [rejected] これは先行研究がありそう．ACL2022で"negation"で調べよ．
                - 無いなら，stackを積むべきなのだが，それだけで研究にはならないので，できれば他の研究から借りたい．
                - LSTMを上にのせて対応すればよいのでは？
        - [transferred] 複雑な規則と言うが，どれくらい複雑なのか？
            * 全然足りていない．
                * 深さ方向は1のみ．
                * ルールの数も，一度に2つまで(３段論法)．
            * [transferred] やる．
        - [transferred] Dropin で使えるようなモデルを提供しているのか？
            - NO
                * [transferred] PLMを公開する．
        - どうやって使うのか？
            - NLIタスクをpromptingで解ける．
                * [todo] もう少し賢い使い方ができないか．例えば，NLIタスクにおいて，証明を導出することによって，タスクを解く，など．
        - [transferred] 見たことの無いファクト・ルールにも汎化するのか？
            * 特に，toyではなく，open-domainのテキストで使えるか？
                * 本研究では，ルールの学習はしていない
                    * [transferred] COMET + reasoning で，NLIタスクで最強になるのでは？
        - [transferred] open domainに適用したい場合，ルールをどこから取るのか？
            - LeapOfThoughtが一定の解を与えている．
                * 一方で，LeapOfThoughtでのタスクは， 1. ドメインが限られている 2. entity knowledge しか扱っていない．
                    - [transferred] open domain x event間ルールでやれないか．
        - [transferred] 一部のNLIタスクでは効かなかったとい．これを攻められないか？
    - 備考
        * 関連論文が非常によくまとまっている．参考にするべき．
        * ソースコード
            - [debatelab/aacorpus](https://github.com/debatelab/aacorpus)
                - theory generator / pre-trained modelが公開されている．
* Measuring Systematic Generalization in Neural Proof Generation with Transformers
    - 概要: CLUTRR taskにおいて，GPTでtheorem generationをしてみた．長さ方向へのgeneralizationがうまくいかない．
    - [discussions]
        - ProofWriterと矛盾するか？
            - 両者とも，proofを生成
            - 両者とも，1-stepづつ生成
            - ProofWriterはn=5まで．本論文はn=10．
            - ProofWriterはD*データセット．本論文は，CLUTRR．
        - NeuralUnifierと矛盾するか
            - NeuralUnifierはe2e, 本論文はgeneration
            - また，NeuralUnifierは ProofWriterの実験設定でやっているので，その違いもある．
    - ideas
        - [transferred] 論文中にメモとして記載したが，
            * 新規のfactに対応できない => A, Bによらないような学習をすればよい．
            * ノイズの影響を受ける => A, Bによらないような学習をすればよい．
            * length generalizationしない => 無限長の学習を行う
        - [todo] 本論文の設定だと，まだ全然解けていない．実験設定を借りるべき．
        - [todo] 事前学習してゼロショットで解く．
            - タスクごとにCLUTRRみたいなデータセットを用意するのはコストがかかる．
    * ソースコード
        * [NicolasAG/SGinPG](https://github.com/NicolasAG/SGinPG)
            - 全て公開されている．


# dataset
* ARC
    - Critical Thinking 論文参照のこと．
* CLUTRR: A Diagnostic Benchmark for Inductive Reasoning from Text
    - 概要: ILP (inductive logic programming) のデータセットを作成．is_parentなどの述語から成るルールを自動で学習できるか，を計測する．




# commonsense reasoning
* ASER: A Large-scale Eventuality Knowledge Graph
    * 一言で: 事象間因果関係のデータベースを作成した．巨大なテキストデータから，依存関係などを駆使して，自動で作成．
    * 備考
        * [HKUST-KnowComp/ASER](https://github.com/HKUST-KnowComp/ASER)
* Benchmarking Commonsense Knowledge Base Population with an Effective Evaluation Dataset
    * 一言で: 常識知識をテキストデータから自動で拾う(commonsense knowledge base population)ことの難しさを示した．
    * 備考
        - この仮定で，様々なcommonsense KB を一つのフォーマットにまとめており，そのデータも公開している．
            * [HonoMi/CSKB-Population](https://github.com/HonoMi/CSKB-Population)
        - **また，commonsense KBのまとめもあり，非常に有用．**
* COMET: Commonsense Transformers for Automatic Knowledge Graph Construction
    - 一言で: COMET論文
* COMET-ATOMIC 2020: On Symbolic and Neural Commonsense Knowledge Graphs
    - TransOMCS extends ASER
        - 手法
            - dependency parsingで取った
        - 結果
            - KGの精度が悪い
            - COMETで学習させた場合，new entityが出てこない
                - 精度が悪いせいだと結論づけている．



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

# todo




# ベースラインリポジトリ
* CoT系列 (GPT-3)
    - Pros
        * APIを叩くだけなので，楽．
    - Cons
        * fine-tuningはお金がかかるかもしれない．
        * fine-tuning前提のタスク(ruletaker)の場合，few-shot性能が弱く，ベースラインとして不適切．
- Generating Natural Language Proofs with Verifier-Guided Search
    - Pros
        * ソースコードが綺麗．
        * RuleTakerもやっている．
        * SOTA
        * proofを生成できる．
    - Cons
        * データセットに証明が必要．
            - modal logicでこれを出すのは面倒くさいかもしれない．
- FaiRR
    - Pros
        - 証明を生成できるので，楽しい．また，デモになるので，社の貢献になる．
        - 性能がSOTA．
    - Cons
        - データセットに証明が必要．
            - modal logicでこれを出すのは面倒くさいかもしれない．
        - コードが汚い．
        - 複雑(モデルが３つ必要)．
- Neural Unifier
    - Pros
    - Cons
        - 性能が弱い
            - おそらくFaiRRよりも弱い?
            - とくに，CWA queryに対して弱い．
            - 実際，ベースラインがRTと古い．
            - 論文のベースラインとしては使えないかも？
* Verifier-Guided Search
    - Pros
        - コードがとても綺麗．
        - RuleTaker に加えてEntailmentBankも使っている．
        - lightning CLIを使っているので，タスクの追加がテンプレ作業．
    - Cons
        - lightning CLIを使っているので，コードの拡張性が低い．




# proof generation

## ideas
* 正しい推論グラフは無数にある．これを許容できないか？
* 複数コンポーネントの場合，局所最適になりがち．全体最適にできないか？
    - trace全体のverifier
* 依然としてgeneratorがネックだという．
* longer proofは依然として苦手だという．
* ここら辺の論文のfuture workを見ること．

## 観点マトリクス
* reasoning type
    - classification
    - single-shot generation
    - stepwise generation
* components : selector, generator, verifier, search
* model      : LM, LLM(few-shot)

## LLM
* Chain of Thought Prompting Elicits Reasoning in Large Language Models
    - 一言で: LLM (few-shot prompt)での single-shot reasoning
    - 評価
        - reasoning系のデータセット詰め合わせ
        - ruletakerはやっていない．
    - ソースコード: https://github.com/jasonwei20/chain-of-thought-prompting
* Large Language Models are Zero-Shot Reasoners
    - 一言で: LLM (zero-shot prompt)での single-shot reasoning
    - 概要: "Let's think step by step"というプロンプトを加えることで，reasoningタスクを解く．CoTよりも良い．
    - 評価
        - reasoning系のデータセット詰め合わせ
        - ruletakerはやっていない．
    - ソースコード: https://github.com/kojima-takeshi188/zero_shot_cot
* Selection-Inference: Exploiting Large Language Models for Interpretable Logical Reasoning
    - 一言で: LLM (few-shot prompt)での component reasoning
    - 概要
        - few-shot promptのLLM (ただし，7B)において， selector + generatorの構成でreasoning taskの性能が爆上がりすることを示した．
            - 大きなモデル(280B)のCoTよりも
    - 観点
        - components
            - selector  : yes (few-shot promopt)
            - generator : yes (few-shot prompt)
            - verifier  : no
            - search    : no
        - transformer
            - Gopher (7B)
    - [comment]
        - selectionが本質的そうなことが分かる．
            - しかし，なぜ？
    - 評価
        - 様々なreasoning task
        - ProofWriterもある．
            - few-shotの場合は，どのdepthでも60%と低い．
            - fine-tuning(Depth=1)の場合でも，depth-5はacc=60%くらいしか解けない．
    - ideas
        - future work
            * 正しい推論グラフは無数にある．これを許容できないか？
            * verifierの追加
            * searchの追加
    - ソースコード: 無し

## fine-tuning
* Transformers as Soft Reasoners over Language (RuleTaker)
    - 一言で: LM (fine-tuning)での SAT
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
    - 一言で: LM (fine-tuning)での stepwise generation
    - 概要: RuleTakerを，proofの生成ができるようにした．「各導出が証明に含まれるべきかどうか」を分類し，整数計画問題で組み合わせる．
    - 評価
        - CWA + OWA
    - あまり詳しくは読んでいない．ProofWriterが上位互換っぽいから．
    - ソースコード
        * [swarnaHub/PRover](https://github.com/swarnaHub/PRover)
            - 全て公開されている．
            - RuleTakerデータセットを使っている．
* ProofWriter: Generating Implications, Proofs, and Abductive Statements over Natural Language
    - 一言で: LM (fine-tuning)での stepwise generation
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
    - 一言で: LM (fine-tuning)での classification (backward chaining)
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
    - 一言で: LM (fine-tuning)での stepwise generation with components
    - 概要:
        * ProofWriterをmodularにすることで，
            * 導出過程を信頼できる(透明性が保てる)ようにした．
            * 表層の変化にロバストになった．各moduleの入力が限られるので，自分の役割のみをこなすから．
                - c.f., ProofWriterはend-to-end generation
            * 関連ルールだけをselectするので，探索が効率的になった．
    - [ideas]
        - 本論文に載れば，e2eと性能で勝負する必要が無くなる．信頼性を推せるから．
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
* Generating Natural Language Proofs with Verifier-Guided Search
    - 一言で: LM (fine-tuning)での stepwise generation with components
    - 概要
        * 既存のstepwise methodは，hypothesisを与えるとinvalidな推論(hallucination)につながり，与えないとirrelevantな推論に繋がるというトレードオフがあった．
        * 本研究では，generatorにはhypothesisを与えることでrelevantな推論をさせ，verifierにはhypothesisを与えないことでvalidityを確かめさせる．
    - 評価
        - EntailmentBank
        - RuleTaker
            - ただし，D3までしかやっていない．
    - 観点
        - components
            - selector  : 無し．
                * その代わり，verifierがselectされたfactを用いて推論の妥当性を判定する．
                * また，sent2 & sent4 => int2 みたいなものを出力させるので，情報の制限を学習させてはいる．
            - generator : yes
            - verifier  : yes
            - search    : yes
        - transformer
            - T5-large (~1B)
    - ideas
        * future work
            - generatorがネック．searchのために，accurate and diverseな候補を生成したい．
            - longer proof
    - 備考
        - 既存研究との比較がある．必見．
        - FaiRRなどは，selectorがhypothesisとのrelevanceを見て，generatorがvalidityを見る．本研究は，コンポーネントの役割が入れ替わっている．
    - ソースコード
        - [リポジトリ](https://github.com/princeton-nlp/NLProofS)




# SAT / single shot
* Transformers Generalize to the Semantics of Logics
    - 概要
        - TransformerがSATを解けることを示した．汎化性能も高い．
    - 手法
        - 論理体系は，命題論理 + 線形時相論理
            - LTLは様相論理である．
    - 評価
        - 自動作成したデータセット
    - ソースコード: https://github.com/reactive-systems/deepltl
* Teaching Temporal Logics to Neural Networks
    - "Transformers Generalize to the Semantics of Logics" の前段論文．線形時相論理のみ．
* Pushing the Limits of Rule Reasoning in Transformers through Natural Language Satisfiability
    - 概要
        - NLSatを提唱．RuleTakerのSAT版．
        - SATなので，証明過程は含まれていないことに注意．
        - hard sampleをサンプリングする手法を提案．有効性を確認．
        - 問題のサイズ(変数の数) の方向への汎化は少ししか見られない．
    * ソースコード
        * [allenai/language_fragments](https://github.com/allenai/language_fragments)
            - 理論の生成コードも含まれている．
            - 動かすのは難しそう．
            - Z3というsolverを使っている．
                * [Z3はmodal logicを扱えない](https://stackoverflow.com/questions/20005601/does-z3py-support-linear-temporal-logic-ltl)
* Transformer-based Machine Learning for Fast SAT Solvers and Logic Synthesis
    - 概要: SATを transformerで解くための何かアーキテクチャの工夫
* TRANSFORMERS SATISFY
    - 概要: 何かSATを解くためのgraph transformerみたいなアーキテクチャの提案．
    - ICLRにrejectされている．






# commonsense
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



# others
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
        * データセットも公開されているが，そんなに引用されていない．
        * 関連論文が非常によくまとまっている．参考にするべき．
        * ソースコード
            - [debatelab/aacorpus](https://github.com/debatelab/aacorpus)
                - theory generator / pre-trained modelが公開されている．
            - **1 stepの理論しか生成できないように思う．**
                - これは，solverを使ってデータセットを作っているのでは無く，単にルールで生成しているからであろう．
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





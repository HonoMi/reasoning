# todo
* [todo] 様相論理の勉強をして，意義を見いだす．
* "ideas"
* "勉強"
* "研究戦略"に従う．
* 先行研究調査
    * [todo](./reviews.md)を読む．まだ読んでいない先行研究があり，そこからideaを出せそう


# 勉強
* 資料の戦略
    1. まず，研究に必要な知識を得る．
        - 日本語でパッと読める本を探す．
        * [Modal Logic for Open Minds](https://www.amazon.co.jp/dp/B00IL4MAJG/)
    2. 次に，(興味があれば)深めていく．
* 教科書
    * 英語
        * [Modal Logic for Open Minds](https://www.amazon.co.jp/dp/B00IL4MAJG/)
            - applicationが豊富．読むべき．
        * [Modality by Portner, Paul](https://www.amazon.co.jp/dp/B005NJPARQ/)
            - modal logic x modality (in linguistic)
        * [Prolog Programming for Artificial Intelligence](https://www.amazon.co.jp/Prolog-Programming-Artificial-Intelligence-4th/dp/0321417461/)
            - 日本語だと第二版までだが，第三版以降はILPやAIも追加されてすごいらしい．

        * 他にも評価が高い本はある．ただし，applicationへの記述はは豊富では無い．
    * 日本語
        * [情報科学における論理](https://www.amazon.co.jp/dp/4535608148/)
            - 様相論理に1章
            - 中級．
            - ここまで詳しいのは，研究にとっては必要無いのでは？
        * [コンピュータサイエンスにおける様相論理](https://www.amazon.co.jp/dp/4627856415/)
            - コンピュータサイエンス x 様相論理
            - コンピュータサイエンスへの応用を扱っているのは嬉しいが，ここまで詳しいのは，研究にとっては必要無いのでは？
        * [数理論理学](https://www.amazon.co.jp/dp/4254117655/)
            - 数理論理一般, 鹿島先生
            - ここまで詳しいのは，研究にとっては必要無いのでは？
        * [数理論理学](https://www.amazon.co.jp/dp/4130629158/)
            - 数理論理一般
            - ここまで詳しいのは，研究にとっては必要無いのでは？
        * 日本語の本はこれくらい．
* 英語の本
* web上の資料




# 研究戦略
* メインストリームを避ける．今，論理推論は加熱しつつある．
    - ただ，LLMでも ruletaker(D5)は解けないようなので，ruletakerは攻められる．
* 形式論理など，我々しか興味のないニッチを攻める．
* 落とし所として，デモなども考慮に入れること．ここのテーマは難しいので，中間的な成果が欲しい．
* "考察"を意識する．
* "勉強"をする．




# 考察
* 本質的な突っ込み
    - Transformerで解くと良いことは，"soft"に解けるということである．逆に，softに解く必要が無いのであれば，solverを用意すれば良い．
    - あなたの課題は，softに解く必要がありますか？
* syntax(proof generation)に対してsemantic(SAT)のマルチタスクは意味があるか？
    - 論理学一般論
        - SAT
            * 一群の論理式が同時に真になるか当てる．
        - proof
            * 前提から結論を，ルールに従い生成していく．
        - つながり
            - 導きたい結論Cの否定^CをSATで否定すれば，Cを導ける．
                - この意味では，syntaxで導出するのと同じ結果が出せる．
            - しかし，上記のように，違うアプローチである．
                - SATの場合は前提も結論も対照的である．よって，例えば，「結論が真で前提が偽」のようなケースの考察も必要となる．
    - 学習ではどう違うか？
        - 機械学習の場合，前提は無矛盾であるという強い制約がある．結果として，「一群の論理式のSAT」は結局，「前提と結論が矛盾しないか」という問題に落ちる．そして，そのラベルは，syntax学習のそれと等しい．よって，syntax学習から情報量は増えない．
            - SATの問題が単に「前提が1の時に結論が1になるか」という問題に帰着する，とも言える．
    - 注意: 「SATを解く」というタスクには依然として意味があることに注意．proof generationのデータセットでSATマルチタスクをすることには意味が無い，というだけである．

## 論理学
* 論理学の主要問題 (論理学を作る P265)
    - 論証の妥当性
        - <=> SAT
    - 矛盾
    - 論理的真理
* 決定可能性: 「SATを解く有限の手続きが存在するか」

## ハイレベルな課題
* どのようにreasoningするか(どのように学習させるか)．
    - deduction
    - abduction
    - どのようにルールを組み合わせるか?
        - ナイーブにやると発散
        - また，ルールが増えるとTransformerのmax_lenに乗らなくなる
        - 探索の効率
    - 証明
        - forward chainig
        - backward chainig
        - others: 何か効率の良いアルゴリズムはあるか？
        * もっと複雑な証明ができるか？
            - 複雑とは？
                - length
                - predicateの種類数
                - 一度に組み合わせないといけないpredicateの数
                - ファクトの数
                - n項述語のn
                - template言語でない．
                - distractorがたくさん．
            - RuleTaker論文のfuture workに書かれている．
    - zero-shot
        - CLUTRRはILPのタスクだが，これをzero-shotで解きたい．すなわち， 1. ruleの獲得 2. reasoningの学習 の両方を事前学習しておきたい．
            - few-shotではダメ．commonsense は無限にあるので，commonsense reasoning したいならば，few-shotすら許されないはず．
    - オープンドメインで何をやるか？
* どのようにルールを獲得するか？ (学習するか)
    - from dataset (e.g., CLUTRR)
        - ILP
    - pre-training
        - commonsense
    - ルールの精緻化
        - 文脈による詳細化
        - ルールのスコアの精度を上げる
* abductionのopen-question (by 乾研)
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
    * 何ができるようになるのか？

## modular vs e2e
- modular
    - Pros
        * 証明に関する誠実性
        * 説明性
        * 各モジュールを再利用できるので，新規タスクでデータセットが必要ない．とくに，closed domain => open domain のtransfer
            - モジュール
                - ルール選択モジュール
                - ファクトチェックモジュール
                - 推論モジュール
            - 「logical reasoningデータセットは無限に作れる」と仮定するならば，この特徴は不要．
        * 問題の規模をスケールさせることができる．
            * スケールしない理由
                - fact / rule が多くなると，計算時間が発散．
                - また，max_len問題もある．
            * FaiRRでは，関連ルールだけを選択することで小規模の問題に分割して解いている．
                * 例えば将来的には，factを外部DBに貯めて検索，などができるかもしれない．
    - Cons
        * 複雑化
        * 性能が低い．最適化できないので．
    - 備考: FaiRR 論文を参考のこと．
- e2eは上記の逆

## 評価案
* CLUTRR
    - 事前学習でlogical reasoningを学んでいれば，zero-shotで解けるはず．
* COMET
* Population論文
* zero-shot on GLUE, RTE
* alpha-NLI
    - サチっていて厳しいかも．
    - zero-shotで解くとか？
* alpha-NLG
    - zero-shotで解くとか？

## 仮説を作るのに使えるモデル
- GPT with prompt
    * [gpt2 Hugging Face](https://huggingface.co/gpt2)
- NLI (の逆方向版)を学修する．
    * [roberta-large-mnli  Hugging Face](https://huggingface.co/roberta-large-mnli)
- commonsense
    * [Mosaic Knowledge Graphs - Commonsense Inferences about Entities and Events](https://mosaickg.apps.allenai.org/model_comet2020_people_events/)
- commonsense KB

## 論理推論に使えるモデル・ライブラリ
* ccg2lambda
    - 自然文を命題に変換する．
    - 証明
    - 仮説推論
* Coq
    - 証明
* open-david
    - 仮説推論
* LangPro
* [Reasoning — Logical Neural Networks  documentation](https://ibm.github.io/LNN/education/examples/reasoning.html)



# ideas
* １つのモデルでできないか？
* なぜselectionが必要なのか？
    - SIやFaiRRを見ると，selectorが重要であることが分かる．
    - 明示的にselectionしないと，学習しきれない？
        * ruletakerの表層などに引っかかっているなら，formal logic learning
* [logical_transfer](./logical transfer.md)
* **"ideas.タスク拡張"**
    - [conclusion] 優先度中．
        - [todo] 様相論理の勉強をして，意義を見いだす．
* "ideas.モデルの工夫"
* "ideas.どのように推論ルールを使うか？"

## others
* 他タスクへのtransfer
    - 研究の評価
        * [conclusion] 優先度中
        * Pros
            - 誰もが知りたい内容である．
            - 他の研究と一緒にできる．
        * Cons
            - 実験コストがそれなりに高い
                - いや，そうでもないか．自分で作るプログラムの割合が小さい気がする．
    - 最終的に解きたいのは，自然言語での推論
    - 以下の関係を調べる
        - 手法
            * e2e
            * step by step
        - タスクの性能
            * reasoning task
            * NLP task
            * NLI task




# ideas.タスク拡張
* 先行研究
    * soft reasoner
        - strategy: syntax (proof generation)
        - 論証    : modus ponens
        - 公理系  : predicate logic
        - 言語    : NL
        - fact
            - universal quantifier (If someone is kind, he is white)
                - [todo] ここら辺微妙．ruletakerのオリジナルのデータセットだと，universal以外もあるかもしれない．
            - atomic (he is whtie)
    * Pushing the Limits
        - strategy: semantic (SAT)
        - 論証    : universal quantifier / existential quantifier
        - 公理系  : predicate logic
        - 言語    : NL
        - fact
    * Critical Thinking
        - strategy: syntax (proof generation)
        - 論証    : たくさん
        - 公理系  : predicate logic
        - 言語    : NL
        - 備考
            - 1 stepしかやっていない．
                - これは，solverを使ってデータセットを作っているのでは無く，単にルールで生成しているからであろう．
    * Transformer Generalize
        - strategy: semantic (SAT)
        - 論証    : 無し (SATなので)
        - 公理系  : predicate logic
        - 言語    : symbol
        - fact
* idea: タスク拡張
    - [conclusion] 優先度中．
        - [todo] 様相論理の勉強をして，意義を見いだす．
    - Pros
        - 誰もやらない．
    - Cons
        - **本質的な突っ込み: 様相論理へ拡張したところで，ルールが1つ 増えるだけでは？**
        - 意義が言いづらい．
        - 工数が大きい．
    - 拡張する
        - 上記を見ると，「たくさんの論証/様相論理への拡張」x「深いdepth」x「NL」はやられていない．
        - [todo] 論証の拡張
        - [todo] 公理系の拡張
            * n項述語
            * 多値論理
            * 直観主義論理
            * [modal logic]($PROJECTS/modal-logic/ideas.md)
            * 高階論理

## ストーリー & 評価
* 研究の評価
    * Pros
        * 楽しい．ロマンがある．
        * すごいので，やれれば多分通る．
        * 多分だれもやらないので，過疎．
    * Cons
        * FWの構築が面倒．おそらくスクラッチで作る必要があるかも．
            - ruletakerに乗れば良いと思われる．
* 背景
    - 既存研究:
        - LNNなどは，logic専用のニューラルネットワークを構築
        - このlogical inverse extreme = 学習データを用意して論理を学ばせる
        - 先行研究はある程度，これができることを示したが，複雑な論理を扱えているかはまだ未知数．
        - 自然言語の推論を行うには，様々な推論が必要．
            - 様々:
                * 深さ
                * ファクトの数
                * 推論形式
                    - 命題論理，述語論理，様相論理，時相論理
    - 一方，記号論理では，少数の公理を元に，無限の導出を行える．
    - よって，自然な質問として，「Transformerがこれを行えるか？」．これに答える．
    - 一方，自然な質問として，
* 質問: 
    - 解けるなら，なぜか？
        - 例えば，公理を帰納的に学べるか？
        - 逆に，公理を教え込んで様々な証明を解けるようになるのか？
    - 何が嬉しい？
        - 解くだけなら，
            - solverがある．
            - また，一般的な公式もある．
            => soft reasoningできる．
        - transformerの能力を確かめることが目的．
            - そうすると，理論とも結びつけたい．
- 手法
    * やる．
    * 事前学習モデルも出す．
        * 様々な論理を一緒くたにやる？ あるいは，T5みたいにprefixで制御する．




# ideas.モデルの工夫
* RL reasoner: end-to-end-wise optimzied transformer reasoner on modular architecture
    * [todo] FaiRR
    * 研究の評価
        * [conclusion] 優先度低．教師データがあるので，RLが必要無い可能性が大きい．
        * Pros
            - RLを使える．楽しい．
            - すごそうに見えるので，やれば通る．
        * Cons
            - シミュレータを自作する必要があるのでは..?
                - いや，最終的な答えを報酬にすればよいだけ？
                - それでデータ量が足りるか?
            - RLをする余地は無いのでは？
                - rule selector: 教師ありで十分．
                - fact selector: 教師ありで十分．
                - reasoner: 教師ありで十分．
    - 背景:
        * e2eの良い点=e2e最適化
        * modular architectureの良い点=(..)．
            - また，目的関数に最適化されていないので，「生成が途中で止まる」なども起こっていた．
        * これらを併せ持つRL modular reasonerを提唱．
    - 手法
        - solverの模倣学習から始めるべき．
        - 行動
            - forward chainingを行う．
            - backward chainingを行う．
            - inference(生成する)
                - 必ずしも1step後とは限らないことに注意．
            - max_lenに達したら，どれかを捨てる．
        - 報酬
            - 証明中に含まれる導出を出したら加点
            - 含まれない導出を出したら原点
            - 答えを出したら加点
        - ロバスト性 = max-ent RLを使う．
    - 効果
        - modular architectureの良い点("modular vs e2e")をキープしつつ，e2e最適化できる．
        - 生成が止まらない．
        - ロバストになる．
            * 推論が誤った方向に行った場合からの回復
            * 導出規則の適用順序(本来は順不同)
                * 導出規則の適用は，順不同．これは教師あり学習では学べない(順序が強制される)のでは？
    - ウリ
        * すべての問題を自然に統合的に解決
    - 答えるべき質問
        * 古典solverをニューロで模擬しただけの何かを作り出すだけになるのでは？
* 自然言語 x 記号推論のPLMを出す
    * [conclusion] 優先度中
    * Pros
        - やれれば凄いので通る．
    * Cons
        - コストがとても高い．
    * Reasonformer
        - 全てマルチタスク(プロンプト)で解く
            - syntax (proof generation)
            - semantic (SAT)
            - system: propositional, predicate, intuitionistic, modal, ...




# ideas.どのように推論ルールを使うか？
* [commonsense LM]($PROJECTS/commonsense-lanugage-model/docs/ideas.md)
* commonsense reasoner
    - 研究の評価
        - [conclusion] 優先度中．差分が小さい．評価ができないかも．一方で，他の研究の延長線上にあるので，やってもよい．デモにもなるかも．
        - Pros
            * ロマンがある．面白いとは思ってもらえそう．
            * 他の研究の延長線上にある．
                - commonsense (COMET)
                - 記号論理
            * 出力が格好良ければ，デモになるかもしれない．
        - Cons
            * 効果が分からない．評価ができないかもしれない．
            * COMET + 記号論理学習 だけだと，差分が小さいかも．
    - [accept] 良い．しかも，「推論ルールを LeapOfThoughtする．」と一緒にできる．
    - 背景
        - LeapOfThoughtで，LMに埋め込まれた知識を用いてreasoningできることが分かった．
        - しかし，open domainでのcommonsense reasoningができるかは自明で無い．
    * 手法
        - commonsense rule を学習したLMを構築．
        - "同じLMで"，abductive / deductive reasoningを学習
        - 更に，commonsense ruleでreasoningする学習も行う:
            - タスク
                - X go to restaurant, (X go to restaurant => X have dinner) => X have dinner (true/false)
                - X go to restaurant, __ => X have dinner (true/false)
            - うーん，COMETから情報量が増えていない？ 結局(X go to restaurant => X have dinner) が理解できていればよい．
                * COMEtから情報量が増えない理由は，commonsenseのルールが自明な形(1項述語， if節はファクトが1つのみ)だからか．
    - 評価
        - NLIで上がる?
* 推論ルール x LeapOfThought (ルールをどうやって使うか)
    - 研究の評価
        - [conclusion] 優先度中．LeapOfThoughtからの差分ちょい微妙．一方，コストが低いのがよい．
        - Pros
            * 課題が良質
            * コストは低いかもしれない．LeapOfThoughtに乗るだけだから．
        - Cons
            * LeapOfThoughtからの差分がしょぼいか？
            * データセットを作る必要があるかもしれない．
                * 一方，LeapOfThoughtのように，自動？でできるかも．
            * LeapOfThoughtに完全に乗っかっている．
                - 彼らがやってしまうのでは？
                - 彼らがやっていないのであれば，何か理由があるのでは？
    - 背景
        - LeapOfThought, 埋め込まれたfactは使えているが，"埋め込まれたルール"は使えるのかどうかは自明で無い．
        - そこで，ルールが使えるかどうかを調査する．
    - 手法
        - 「彼はレストランに行った」 => 「彼は夕食を食べた」
            - A => B
            - このルールが埋まっているのは自明．ただの後続事象生成なので．
        - "使える"とは何か？
            - 後続事象を生成するだけだと，できる．単なるテキスト生成だから．
            - よって，やはり，「様々なルールを組み合わせて最終的な結論を得られるか？」という評価が必要．
                - これもストーリー生成に近いのでは？
                - [todo] また，これはすなわち，"natural language reasoning"の研究に近いのでは？
        - ルール
            - gold rule: is_type(X, Y), has_property(Y, Z) => has_property(X, Z)
            - 先行研究
                - given: has_property(Y, Z)
                - predict: has_property(X, Z)
            - ours
                - given: is_type(X, Y)
                - predict: has_property(X, Z)
    - 懸念
        * COMETなどの推論ルールを埋め込んで推論させる場合，COMET自体と何が違うのか，となる．できることが同じな気がする．
            - COMET: A -> B
            - ours : A, (A->B) -> B
* 推論ルール x LeapOfThought (ルールをどうやって埋め込むか)
    - 研究の評価
        - [conclusion] 優先度中．「推論ルール x LeapOfThought」と一緒にできる．
        * Pros
            * コストは低いかもしれない．LeapOfThoughtに乗るだけだから．
        * Cons
            * LeapOfThoughtからの差分．
    - 背景
        - 正確に言うと，「ルールの種みたいなのはLMに埋め込まれている．これをどうやって，推論に最適な表現に変えるか」
    - 手法
        - 以下をマルチタスクする．
            1. A, (A => B) => B
            2. A,    __    => B
                * __ = 「is_type(X, Y), has_property(Y, Z) => has_property(X, Z)」
        - すると，__は自分で保持していないと行けないので，ルール表現が学習される．
    - 評価
        - LeapOfThoughtに従う．
    - 懸念
        - 最終的にやりたいことは，open domainでの推論である．これは自明かもしれない:
            - X go to restaurant, __ => X have dinner
                - 「__」に「X go to restaurant => X have dinner」を埋め込む，という話であるが，これはCOMETと全く同じ．
            - commonsense のルールが 1項述語かつ1条件で自明なのが問題なのか？

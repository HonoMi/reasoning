# todo
* [todo](./reviews.md)を読む．まだ読んでいない先行研究があり，そこからideaを出せそう
* **落とし所として，デモなども考慮に入れること．ここのテーマは難しいので，フル評価ができるかは怪しい．**

## 戦略
1. 情報を仕入れる
    * 何のためにやるかというと，我々が選ぶ課題がmake senseなのかを確かめるため．
        - e.g.) 導出させる．
    * 論理プログラミングの本 satなど
    * 前提知識のお勉強
        * 記号論理 => 述語論理 => 様相論理
        * 論理プログラミング，特にILPを勉強する．
            - http://redwood.cs.ttu.edu/~mgelfond/PAPERS/survey.pdf
            - 記号論理，数理論理学に近い．
            - 不完全性定理もやる．
        * オートマトン
    * [論文の精読](./reviews.md)
        - [todo] なところをやる．
    * [Prolog Programming for Artificial Intelligence](https://www.amazon.co.jp/Prolog-Programming-Artificial-Intelligence-4th/dp/0321417461/)
        - 日本語だと第二版までだが，第三版以降はILPやAIも追加されてすごいらしい．
1. 話を整理する．
    - ハイレベルに各研究を見る．
    - 我々の研究が意味をもっているか，意味を持つには何を仮定に置いているか，仮定は正しいか，を精査する．
2. 以下の順番で研究を進める
    1. [commonsense LM]($PROJECTS/commonsense-lanugage-model/README.md)
    2. その他の研究を進めるためのベースリポジトリを動かしてみる．



# 考察

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
* どのようにルールを獲得するか？
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








# logical reasoning
* **研究途上でデモ論文を出すこと．** 長い研究になりそうだから．
* **根本的な疑問: classicalなアルゴリズム(e.g., PrologのDFS solver)に対して，ニューラルネットワークによる言語処理ユニット(ファクトマッチング，ルールの選択)を追加するだけよいのでは？**
    - これは言わないお約束で通る？

* RL solver on modular architecture
    - [todo] FaiRR を動かしてみる．
    * 研究の評価
        * [conclusion] 優先度高 RLを使える点，FaiRRのリポジトリに乗れる点．
    - 背景:
        * e2eの良い点=e2e最適化, modular architectureの良い点=(..)．
        * これらを併せ持つRL modular reasonerを提唱．
    - 手法
        - solverの模倣学習から始めるべき．
        - 行動
            - forward chainingを行う．
            - backward chainingを行う．
            - 答えを出す．
            - max_lenに達したら，捨てる．
        - 報酬
            - 証明中に含まれる導出を出したら加点
            - 含まれない導出を出したら原点
            - 答えを出したら加点
    - 効果
        - modular architectureの良い点("modular vs e2e")をキープしつつ，e2e最適化できる．
        - ロバストになる．
            * 推論が誤った方向に行った場合からの回復
            * 導出規則の適用順序(本来は順不同)
                * 導出規則の適用は，順不同．これは教師あり学習では学べない(順序が強制される)のでは？
    - 答えるべき質問
        * 古典solverをニューロで模擬しただけの何かを作り出すだけになるのでは？
* 自然言語ロバスト学習
    - todo リポジトリ探し．
    - 研究の評価
        * [conclusion] 優先度高．
        * Pros
            - 既存研究に乗りやすい．テンプレートに自然言語を入れるだけの可能性がある．
        * Cons
            - この研究だけだとちょっと弱いかも．
                * 必要なら，"述語論理カリキュラム学習"にうつる．
    - 効果
        * 記号論理をより効率よく学習できる．sytaxを意識できるから．
        * 自然言語の揺らぎにロバストになる．
        * 任意のテキストデータを使えるので，データセットを無限にできる．
        * n項述語など，複雑なルールを学習させることができる．
    - 先行研究
        - 自然文でやっていた．
        - 意味的につながりのあるルールを用いて，汎化させようとしていた．よって，
            - データ量が小さくなりがち．
            - また，記号論理特有の原則(意味に依らないsyntaxだけで導出できる)を学習するのに効率が悪い．
        - 端的に言うと，命題論理のsemanticを意識してしまっていた．
    - しかし，
        - (A, A -> B) -> B はAやBの"内容に依らずに"成立する．
            * 先行研究では，syster parent など，意味的なつながりのあるルールで学習しようとしていた．
    - 原則
        - 記号論理
            - 命題(A, B)     : 任意の内容でよい．
            - ルール (A -> B): 任意の内容で良い．
        - 自然言語
            - 命題(A, B)
                * 文法も意味も正しい必要がある．
            - ルール
                * 意味的につながりがある必要がある．(e.g., commonsense)
    - 手法
        1. A, B には適当な文を入れる．
            * 文を入れたることにより，文法・意味が通るものが入る．
            * いや，unificationは？
                * coreferenceか．
        2. A, B によらないように，encoderをfreezeする．
            - freezeさせたら学習すべきパラメータが無くなってしまうのでは？
        3. distractor文も混ぜる．関係ない命題に依存しないようにするため．
        3. ルールには，commonsenseを使う？
            - 「意味的につながりがある必要がある」に対処．
            - これは，知識を埋め込む話なので，推論を学習させる話とは別か．
* 記号論理学習: Transformerは(複雑な)記号論理を学習しきれるのか．
    * [todo] 乗れるリポジトリを探す．乗れないならスクラッチ．
    * 研究の評価
        * [conclusion] 優先度中．スクラッチでFWを開発しないといけないかもしれないのでコストが高い．
        * Pros
            * 楽しい．ロマンがある．
            * すごいので，やれれば多分通る．
        * Cons
            * FWの構築が面倒．おそらくスクラッチで作る必要があるかも．
    * 背景
        - これは，自然言語への適用を意識したものであり，簡単なルールのみ．
        - 一方，記号論理では，少数の公理を元に，無限の導出を行える．
        - よって，自然な質問として，「Transformerがこれを行えるか？」．これに答える．
        - 一方，自然な質問として，
    * 背景
        - LNNなどは，logic専用のニューラルネットワークを構築
        - このlogical inserve extreme = 学習データを用意して論理を学ばせる
        - 先行研究はある程度，これができることを示したが，複雑な論理を扱えているかはまだ未知数．
    * 質問: 
        - 解けるなら，なぜか？
            - 例えば，公理を帰納的に学べるか？
            - 逆に，公理を教え込んで様々な証明を解けるようになるのか？
        - 何が嬉しい？
            - 解くだけなら，
                - solverがある．
                - また，一般的な公式もある．
            - transformerの能力を確かめることが目的．
                - そうすると，理論とも結びつけたい．
    - 手法
        1. solverの過程を教師あり学習
        2. 報酬だけを頼りに強化学習
    - 備考
        - 他の論理体系を学習させてみる．
            - [accept] これだけでは研究にならないから，「Transformerは記号論理を学習しきれるのか．」と一緒にやる．
            - 対象
                - 記号論理
                - 述語論理
                - 様相論理
                - 時相論理
* 記号論理カリキュラム学習
    - 研究の評価:
        * [conclusion] 優先度中．コストが高い．他の研究の総体なので，他の研究をやってから始めるべき．
    - 手法
        * いくつかのステップに分解することができる．curriculum learningする．それぞれはseparateして学習すべき．
            - 記号論理の学習 (自然言語ロバスト学習 + 記号論理学習)
                - 既存の研究のリポジトリに乗る．
                - "記号論理学習"に乗る．
            - 論理記号と自然言語のマッピング
                - 記号とのマッピングはカバレッジが低いという課題がありそう．これへの対策として，事前にcommonsense ruleを埋め込んだモデルにおいて，commonsense ruleを使った推論を解かせる．
                    * 関係抽出で，関係パターンとその両側を交互に学習させるようなものに近い．
            - ルールの学習



## どのようにルールを使うか？
* [commonsense LM]($PROJECTS/commonsense-lanugage-model/docs/ideas.md)
* commonsense reasoning with commonsense rule and reasoning ability
    - 研究の評価
        - [conclusion] 優先度高．差分が小さいかも．一方で，他の研究の延長線上にあるので，やってもよい．デモにもなるかも．
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
        - しかし，LMは汎用的な学習をしているため，ルールを効率的に学習できていない可能性がある．
    * 手法
        - commonsense rule を学習したLMを構築．
        - "同じLMで"，abductive / deductive reasoningを学習
    - 評価
        - NLIで上がる?

* 推論ルールを LeapOfThoughtする．
    - 研究の評価
        - [conclusion] 優先度中．コストが高いかもしれないこと，差分が小さく見えるかもしれないこと，による．
        - Pros
            * 課題が良質
        - Cons
            * LeapOfThoughtからの差分がしょぼいか？
            * データセットを作る必要があるかもしれない．
                * 一方，LeapOfThoughtのように，自動？でできるかも．
    - 背景
        - LeapOfThought, 埋め込まれたfactは使えているが，"埋め込まれたルール"は使えていない．
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

## additionsl
* [accept] 自然言語 x 記号推論の初めてのPLMを出す
    - [accept] これは，additionalである．

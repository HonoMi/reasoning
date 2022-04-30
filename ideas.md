# todo
* **落とし所として，デモなども考慮に入れること．ここのテーマは難しいので，フル評価ができるかは怪しい．**
* 戦略
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
    2. 以下の順番で研究を進める
        1. commonsense
            - 他と独立してできるため．
            - また，一番やりやすい．
        2. logical reasoning
            - 他と独立してできる．
            - logical reasoningで何ができるか，"慣れ"させられる．
        3. natural langauage reasoning (deduction/abduction)
            - deductionの方が簡単?



# 考察

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

## logical reasoning
* 記号論理×自然言語処理のはじめてのplmを出す．
* 何をやりたいか？
    - CLUTRRみたいなのをzero-shotで解きたい．
        - CLUTRRはILPのタスクだが，これをzero-shotで解きたい．すなわち，ruleの学習とreasoningnの学習は，事前に済ませておく．
* 「TransformerはStackを模擬できない」は，logical inferneceする上で課題になるか？ なるなら，解決が必要．
    - [pending] 少し難しいので．ただし，not not not みたいなのはTransformerは苦手かもしれない．それを無視して研究を進めると，うまくいかないか，reviewerに突っ込まれるかもしれない．
    - not not not not みたいなのは認識できない可能性が高い．
    - 自然言語では問題にならない? not not not みたいなのは現れないから．
    - 理論的には，「decoderステップが無限」「infinite precision」などの仮定を置けば，Transformersはturing完全である．一方で，これを置かないと，Turing完全では無いという．
    - On the Ability and Limitations of Transformers to Recognize Formal Languages
        * counter languageの一部しかうまくいかないという．not not not は認識できないか？
* logical reasoning
    * 背景
        - LNNなどは，logic専用のニューラルネットワークを構築
        - このlogical inserve extreme = 学習データを用意して論理を学ばせる
        - 先行研究はある程度，これができることを示したが，複雑な論理を扱えているかはまだ未知数．
    - [todo]
        - 教科書: 様相論理など．
        - 先行研究調査
        - 「Transformerで記号論理が解ける」という仮定が，先行研究で示されたTransformerの理論的・経験的な挙動と不一致を起こさないか，を考える．
    - もっと複雑な(何ステップも必要な)論理を解けるか？
        - 解けるなら，なぜか？
            - 例えば，公理を帰納的に学べるか？
            - 逆に，公理を教え込んで様々な証明を解けるようになるのか？
        - 何が嬉しい？
            - 解くだけなら，
                - solverがある．
                - また，一般的な公式もある．
            - transformerの能力を確かめることが目的．
                - そうすると，理論とも結びつけたい．
    - 別の論理体系を学べるか？
        - 記号論理
        - 述語論理
        - 様相論理
    - 手法
        1. solverの過程を教師あり学習
        2. 報酬だけを頼りに強化学習
* 自然言語との統合をどうやるか？
    - A みたいな記号だと，自然言語まで汎化するかは怪しい．
    - 任意のevent表現を命題として扱えないか．

## natural language reasoning


### deduction
* natural language prolog
    - [todo] prologの用途を調べる．教科書でよい．
    - 論理推論器は何を使うか？
        * CCG2lambdaによって導出 => Transformerを半教師あり学習
    - 要は，「自然言語でdeductionができるか？」という話に落ちる気がする．
* LeapOfThought x 推論ルール
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
* natural language reasoning
    - 背景
        - 自然言語での論理的な推論は，AIの聖杯．
        - 先行研究によって，小さい規模の統制言語(=小規模のルール，かつ，文脈無し)ならうまくいく
        - 一方，大規模かつ統制されていない自然言語言語でうまくいくか不明．
            - 前者は，先行研究からの流れ．後者は，乾研の研究みたいなところからの流れ．
            - つまり，commonsenseでうまくいくか不明ってこと．
        - そこで
            - 大規模テキストデータでatomicなルールを，言語モデルによってatomicなルールを学習
                - coreference
            - 同じネットワークで，abductive / deductive reasoningを学習
    - 研究を進める上で埋めるべき観点
        - 何が出来るようになるのか?
            - deduction / abduction
            - まずは，単にデモでよいか？
            - COMETと違うことができる？
            - "間"が出せる．
            - "前提条件"が出せる．
        - データは？
            * 教師ありデータセットを作成する．
        - 教示は？
            - alpha NLG
            - LeapOfThought形式
            - データセットを作成する．

### abduction
* [todo] open-questionを攻める．
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

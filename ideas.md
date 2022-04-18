# ideas
* 評価
    * COMET
    * Population論文
    * zero-shot on GLUE, RTE
* learning to reason
    - 先行研究によって，小さい規模の統制言語(=小規模のルール，かつ，文脈無し)ならうまくいく
    - 一方，大規模かつ統制されていない言語でうまくいくか不明．また，述語に落とす方法は，文脈が落ちるのでだめ．
        - つまり，commonsenseでうまくいくか不明ってこと．
    - そこで
        - 大規模テキストデータでatomicなルールを，言語モデルによってatomicなルールを学習
            - coreference
        - 同じネットワークで，abductive / deductive reasoningを学習
    - これによって，
* CCG2lambdaによって導出 => Transformerを半教師あり学修
* 単にlogic neuroに対して「commonsense ruleを教えまくった」でどうか．
    - COMETから何が良くなる？
* logic neuro
    - LNNを使ってlogical inferenceだけでabductive inferenceを解けないか？
    - 自然言語でpre-trainingする．
    - テキストデータから命題を抽出
* 教師あり学習をする．データセットを作成する．
* 観点
    - 何が出来るようになるのか?
        - COMETと違うことができる？
        - "間"が出せる．
        - "前提条件"が出せる．
    - 教示は？
        - alpha NLG
        - LeapOfThought形式
        - データセットを作成する．
    * 知識の質は十分か？
    - 知識の量は十分か？
* LeapOfThought x commonsense
    - LeapOfThought, 推論形式が限られている．
    - LeapOfThought, 埋め込まれたfactは使えているが，"埋め込まれたルール"は使えていない．
* デモ論文でもよい．
* 「様相論理など，他の論理形式にも使えるかを確認した」論文
* 仮説を作るのに使えるモデル
    - GPT with prompt
        * [gpt2 · Hugging Face](https://huggingface.co/gpt2)
    - NLI (の逆方向版)を学修する．
        * [roberta-large-mnli · Hugging Face](https://huggingface.co/roberta-large-mnli)
    - commonsense
        * [Mosaic Knowledge Graphs - Commonsense Inferences about Entities and Events](https://mosaickg.apps.allenai.org/model_comet2020_people_events/)
    - commonsense KB
* 論理推論に使えるモデル
    * [Reasoning — Logical Neural Networks  documentation](https://ibm.github.io/LNN/education/examples/reasoning.html)
- まだopen domainのルールが全然取れていないことも課題になりそう．
    * 上記のモデルで「，PersonX is a criminal -> PersonX will be arrested.」 を導けない..．
    - Benchmarking Commonsense Knowledge Base Population with an Effective Evaluation Dataset
- commet commonsense => 仮説を大量に生成 => forward NLI
    - いや，forwardの時のルールを結局，仮説を生成したモデルに頼るしかないので，完全なマッチポンプ．学習できない．
        * phillipの時は公理をどうやって導いていたのか．
            - coreference
        * いや，我々がやりたいのは，「長い文」を原始的な命題から説明すること．
- research question: "Could a SOTA LM conduct abductive reasoning"
- alhpa NLGはサチっていない．
    - あるいは，alpha- をzero-shotで解く．
- contextualizeされた状態のまま，事象間関係知識を得たい => 共参照文をlogic neuroに入れて学習させまくる．
- 仮説推論, logic neuro を逆向きに動かせば良いのでは？
- 言語モデルをどう生かすか？
- 疑問: commonsense KB で「前提条件」を探す以上のことが実現されれるのだろうか？
    * 複数ルールの組み合わせになる?
    * 前提条件の探し方を「KBにあるものから探す」とやる限り，ただのマッチポンプでは．
- テキストデータで学習すると言っても，PersonXみたいな知識しか無いぞ．
- ニューロの入れ方：仮説生成のところ　　記号ではなく文ベクトルを入れる　　ルール作成の際は、文脈も取っておく　




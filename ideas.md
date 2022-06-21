# todo
* step by step論文はデモがあるという．これを試す．
* reasoning の先行研究を調査した方が良いのでは？
* (論理学の本を一通り復習したら) 研究ideaを出す．
* [todo](./reviews.md)を読む．まだ読んでいない先行研究があり，そこからideaを出せそう
* 研究戦略
    - 優先
         * [commonsense LM]($PROJECTS/commonsense-lanugage-model/README.md)
         * 形式主義学習
         * natural language injection
         * カリキュラム学習
    * 落とし所として，デモなども考慮に入れること．ここのテーマは難しいので，フル評価ができるかは怪しい．

## お勉強
* [Prolog Programming for Artificial Intelligence](https://www.amazon.co.jp/Prolog-Programming-Artificial-Intelligence-4th/dp/0321417461/)
    - 日本語だと第二版までだが，第三版以降はILPやAIも追加されてすごいらしい．



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








# logical reasoning

## tmp
* 論理の学習を"lets think step by step" プロンプトを入れてやっておき，別のタスクでそのプロンプトを入れて解かせる．
    * 先行研究: [Large Language Models are Zero-Shot Reasoners](https://arxiv.org/abs/2205.11916)
* 2項述語にして，関係抽出などと関係づける．


## いける
* 形式主義学習 (flormal logic learning)
    - 研究の評価
        - [conclusion] **優先度高．** ただ，これだけだと自明かもしれないので，「記号論理カリキュラム学習」と一緒にやるべきか．
        - Pros
            - 上がる気がする．
            - コストが低い．
            - 偉そうなことを言えるので，通るかも．
        - Cons
            - これだけだと自明なアイディアに見える．
    - ストーリー
        - On one hand, formal reasoning
        - On the other hand, should be robust to natural language expressions
    - リポジトリ: FaiRRを使う．subject robustnessを計測したいので．
        - しかし，FaiRRを使うには，proofstepを生成する必要があるぞ..？
    - 評価
        - RuleTakerと同じデータセットサイズで，よりロバストな学習ができる，と主張すべき．
    - 先行研究
        - FaiRRやProofWriterでは，subject robustnessがattribute robustnessが低い．
        - これは，「任意のx」「任意の述語」というお気持ちが足りず，John / Redなどの表層に何かがつられてしまっているから．
            * これはとりもなおさず，論理に意味を見いだしてしまっていたから．
        - 一方，論理とは，形式であることが知られている．
        - そこで，任意の記号，任意の述語を入れて学習させる．
    - 効果
        * 記号論理をより効率よく学習できる．「論理とは，形式のみで決まる」ということを意識できるから．
            * 主語・述語にロバストになる．
            * その他何かあるか？
    - 懸念
        - limitation
            - OWAのデータセットを作るのが難しそう．これがlimitationとなる．
                - 論文の本筋には関係無いはず．
                - また，PRover, MultiProver, NeuralUnifier はCWAでやっている．
* natural language injection (NL injection)
    - 経過: $PROJECTS/commonsense-lanugage-model/00.extract_eventualities_and_relations_from_cc100.py の出力で，S-V-Oなどのbegin/endを出す．これを使って，表層を操ることができる．
    - 研究の評価
        * [conclusion] **優先度高**
            - おそらくこの研究は，「形式主義学習」「自然言語へのドメイン適応」の2つの目的を掲げることができる．
            - 前者であるならば「記号論理は，任意の記号を扱える必要がある一方で，自然言語では，語句間の選好によってありうる系列が定まる．両者の特徴をうまく取り入れた学習手法として，..」
            - 後者を捧げるのであれば，必然的に，カリキュラム学習の後段の位置づけとなる．
        * Pros
            - 既存研究に乗りやすい．テンプレートに自然言語を入れるだけの可能性がある．
        * Cons
            - この研究だけだとちょっと弱いかも．
                * 必要なら，"述語論理カリキュラム学習"にうつる．
    - 懸念
        - 形式主義学習を目的として掲げる場合:
            - template言語を複雑にするのと変わらないのでは?
                - 文全体を取らないと，template言語と大差無い．
                - **いや，template言語でやると，でたらめな選好の語句が言語モデルに入ってくるので，自然言語を忘れてしまうかもしれない．**
            - reasoningを学ぶ目的では，template言語の方が良いように思う．「任意の記号」を良く表しているから．
        - 自然言語へのドメイン適応を目的として掲げる場合
            - LeapOfThought では，KBから取ったファクトを使って学習している．これと若干似ているかもしれない．
                - 我々の手法は，自然言語表現を使うので，KBから取ったファクトよりもファジーであるが．
    - 効果
        * 自然言語の揺らぎにロバストになる．
            * というか，D5データセットは限られた語彙しか使っていないので，その語彙のembeddingも最適化されてしまい，他の語彙に汎化しなさそう．
            * 自然言語タスクを忘却しない．
                - 最終的には，推論と自然言語タスクの両方を合わせたタスクを解きたいので，自然言語タスクの性能が下がってしまってはダメ．
        * 任意のテキストデータを使えるので，データセットを無限にできる．
        * n項述語など，複雑なルールを学習させることができる．
        * 自然言語へのロバスト性を測るデータセットを公開
    - しかし，
        - (A, A -> B) -> B はAやBの"内容に依らずに"成立する．
            * 先行研究では，syster parent など，意味的なつながりのあるルールで学習しようとしていた．
    - 原則
        - 記号論理
            - 命題(A, B)     : 任意の内容でよい．
            - ルール (A -> B): 任意の内容で良い．
            - ルール表現
                * if A then B など，決まり切ったのでよい．
        - 自然言語
            - 命題(A, B)
                * 文法も意味も正しい必要がある．
            - ルール
                * 意味的につながりがある必要がある．(e.g., commonsense)
            - ルール表現
                * 様々． B because A => A -> B を含意する．
    - 手法
        * 適当にテキストデータからeventを取ってきて，A, Bに突っ込む．
            - ASER?
            - いくつかのversionを作りたい．
                - skeleton
                - subtree
                - w/ context
        * ルール表現も，自然言語表現とする．
            - B because A
        * 共有項の取り方
            * 主語を無理矢理同じものに取り替える．
            * coreferenceを使う．
                - これで共有項を取ってしまうと，データセットに，論理とは関係無いバイアスがかかってしまうのでは？ しかもそれは，事前学習で容易に学習していそう．
    - 考察
        - He talked [quickly] -> He left
            * quicklyはルールに含めるべきなのか？
            * 我々の最終目標では，これは文脈依存．quicklyという条件が無いと推論が成り立たないのであれば，含めるべきだし，そうでなければ，はずすべき．
            * それでは，我々のデータセットにこの考えをどうやって反映させるのか？
                - 先行研究では，ルールが保たれる程度の言い換えを人手で作成していた．
                - 常に文脈語もも含める．「A, Bは任意であるべき」「推論ルールは厳密に守るべき」との考えを反映させたものになる．
        - 本論文での自然言語揺らぎ
            - A, B に入るテキストは任意の自然言語とする．ロバスト性を獲得するため．
            - A -> B のルールに関しては，「自然言語のルール」にとどまらず，任意のルールを学習させる．これは，記号論理の原則に従っている．
        - 先行研究と我々の違い
            - 先行研究
                - A, B   : テンプレート言語
                - ルール : 任意の組み合わせ
            - 我々
                - A, B   : 自然言語 (ロバスト性獲得のため)
                - ルール : 任意の組み合わせ (記号論理の原則に従う)
            - ルールに関しては，「自然言語のルール=常識知識によるルール」を対象とする，という考え方もできる．しかしそれは，記号論理の推論を捉えていない．推論の獲得と，ルールの獲得は，別の話である．常識知識ルールの利用は，また別のテーマとしてやるべきである．
* 記号論理カリキュラム学習
    - 研究の評価:
        * [conclusion] **優先度高．** ただ，一気にここまではいけないので，他の研究の延長戦としてやる．
        * Pros
            - 形式主義の学習 + 自然言語へのドメイン適応 というストーリーは，論文として意味があり，通りそう．
        * Cons
            - 少々コストが高い．
    - 手法
        * いくつかのステップに分解することができる．curriculum learningする．それぞれはseparateして学習すべき．
            - 記号論理の学習 (記号論理学習 + 形式主義学習)
                - 既存の研究のリポジトリに乗る．
                - "記号論理学習"に乗る．
            - 自然言語への"ドメイン適応"
                - "NL injection"
                - ProofWriter dataset で学習させるだけでもよいかも？
                - 記号とのマッピングはカバレッジが低いという課題がありそう．これへの対策として，事前にcommonsense ruleを埋め込んだモデルにおいて，commonsense ruleを使った推論を解かせる．
                    * 関係抽出で，関係パターンとその両側を交互に学習させるようなものに近い．
* 他タスクへのtransfer
    - 研究の評価
        * [conclusion] **優先度高．**
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
* 記号論理学習: Transformerは(複雑な)記号論理を学習しきれるのか．
    * [todo]
        * 記号論理の本で基礎知識を得る．
    * 研究の評価
        * [conclusion] **優先度高．**スクラッチでFWを開発しないといけないかもしれないのでコストが高い．
        * Pros
            * 楽しい．ロマンがある．
            * すごいので，やれれば多分通る．
            * 多分だれもやらないので，過疎．
        * Cons
            * FWの構築が面倒．おそらくスクラッチで作る必要があるかも．
                - ruletakerに乗れば良いと思われる．
                    * いや，様相論理のsolverなんて存在しなさそう．
    * 背景
        - 既存研究:
            - LNNなどは，logic専用のニューラルネットワークを構築
            - このlogical inserve extreme = 学習データを用意して論理を学ばせる
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
            - transformerの能力を確かめることが目的．
                - そうすると，理論とも結びつけたい．
    - 手法
        * やる．
        * 事前学習モデルも出す．
            * 様々な論理を一緒くたにやる？ あるいは，T5みたいにprefixで制御する．
    - 備考
        - 他の論理体系を学習させてみる．
            - [accept] これだけでは研究にならないから，「Transformerは記号論理を学習しきれるのか．」と一緒にやる．
            - 対象
                - 記号論理
                - 述語論理
                - 様相論理
                - 時相論理
* RL reasoner: end-to-end-wise optimzied transformer reasoner on modular architecture
    * [todo] FaiRR
        * 懸念を解決する．
            - RLをする余地は無いのでは？
                - rule selector: 教師ありで十分．
                - fact selector: 教師ありで十分．
                - reasoner: 教師ありで十分．
            - いや，教師あり学習だとreasonerのinputがteacher forcing．RLだと，自分の出した結果を使える．
    * 研究の評価
        * [conclusion] **優先度高** RLを使える点，FaiRRのリポジトリに乗れる点．
        * Pros
            - RLを使える．楽しい．
            - すごそうに見えるので，やれば通る．
        * Cons
            - シミュレータを自作する必要があるのでは..?
                - いや，最終的な答えを報酬にすればよいだけ？
                - それでデータ量が足りるか?
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

## どのようにルールを使うか？
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

## additionsl
* [accept] 自然言語 x 記号推論の初めてのPLMを出す
    - [accept] これは，additionalである．

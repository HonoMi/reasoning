# todo
* (森尾)に話してみる．


# 背景
* 背景1
    * LLMに工夫を加えることによって，ある程度の推論が解けることが分かった．
    * research question
        * このような推論能力はLLMに特有なものなのか
        * LMに対して論理推論能力を付与することはできないのか？
        * どのように転移させられるか？
            * データセットは？ 
            * 転移方法(プロンプト, 証明の形式)
        * 転移された推論能力に，言語モデルのcommonsense を加えて，推論できるか？
* 背景2
    * LLMでさえも，長い推論は解けないことが分かった．




# LLMからの転移 (logical distillation)
- Pros
    - 面白いので，通るかもしれない．
    - logical transfer と一緒にできる．
- Cons
    - [todo] (是枝) LLMを何度も叩くことができるか？ 金銭上の問題．
- 手法
    - LLMに"think step by step" => 推論データセットを生成 => LMで学習
- 備考
    - tiny reasoner: distilling logical thinking from large pre-traeined language models




# RuleTakerからの転移

## 背景
* 一方，生の推論タスクは人手コストがかかるので，証明を作りづらい．
* RuleTakerのような人工データセットならば自動でたくさん作れる．

## 手法
* モデルの工夫
    * prompt
        - think step by step を入れて，事前学習時の推論能力をスタート地点にする．
        - RuleTakerで(question, "let's think step by step", answer)を学習させた後，ターゲットタスクで(question, "let's think step by step", X)のXを埋める．
        - あるいは，答えで条件付けて生成することを学習しておく？ hindsight に近い．
* データの工夫
    * [EntailmentBank](https://allenai.org/data/entailmentbank)との違いと対策．
        1. EntailmentBankでは，atomic propositionの中に更に推論ルールが含まれる．
            - 比較
                - RuleTaker
                    - "Allen is red"
                - EntailmentBank
                    - "eruptions produce ash clouds" (A->B)
                    - "ash blocks sunlght" (B->C)
            - ideas: 2に準じる．
        2. 1の結果として，EntailmentBankでは，modus ponens 以外の推論(e.g., syllogistic)を使う必要がある．
            - e.g.) "e"
                1. "eruptions produce ash clouds" (A->B)
                2. "ash blocks sunlght" (B->C)
                3. 1 + 2 => eruptions block sunlght
            - idea
                - RuleTakerデータセットに多様な推論ルールを入れる．
        3. EntailmentBankでの推論ルールは，多様な表現がある．
            - e.g.)
                * eruptions produce ash clouds.
                * producers will die without sunlght = if sunlght then producers dies- 
            * idea: 因果関係データセット・因果関係判定モデルなどで，以上のような因果関係文を収集し，RuleTakerデータセットに混ぜる．
        4. EntailmentBankでは，明示されない知識も使う必要がある．
            - e.g.) "plants are producers"
    * "記号論理カリキュラム学習"
    * "形式主義学習"
    * "natural language injection"
        - 人工データセットでやると命題の内容(Allen is red)に偏りが出る．これをmarginalizeするために=演繹能力だけをtransferするために，行う．
    * "logical pre-training"
    * LeapOfThoughtのように，内在的知識を必要とする推論に対してどうやって転移させるか？
        - 人工データにおいて，一部のルールをわざと欠損させて推論させる．
            - 欠損させるルールは，LMに内在するcommonsensen ruleである必要がある．よって，テキストから自動抽出したルールを使う．

## 詳細
* 記号論理カリキュラム学習
    - [doing] $PROJECTS/fairr-launcher/, $PROJECTS/logical-reasoning
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
* 形式主義学習 (formal logic learning)
    - [doing] $PROJECTS/fairr-launcher/, $PROJECTS/logical-reasoning
    - 研究の評価
        - [conclusion] 優先度中． これだけだと自明かもしれないので，「記号論理カリキュラム学習」と一緒にやるべきか．
        - Pros
            - 上がる気がする．
            - コストが低い．
            - 偉そうなことを言えるので，通るかも．
        - Cons
            - これだけだと自明なアイディアに見える．
            - 効果が「言語表現にロバストになる」だけだと，ショボい．
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
    - [doing] $PROJECTS/fairr-launcher/, $PROJECTS/logical-reasoning
    - 経過: $PROJECTS/commonsense-lanugage-model/00.extract_eventualities_and_relations_from_cc100.py の出力で，S-V-Oなどのbegin/endを出す．これを使って，表層を操ることができる．
    - 研究の評価
        * [conclusion] 優先度中
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
* logical pre-training
    - [conclusion] 優先度中 工数が大きい割にうまくいくか不明なので，まずは他の研究からやる．
    - Pros
        - 他の研究の延長上にある．
        - インパクトがあるので，うまくいけば，必ず通りそう．
    - Cons
        - 工数が大きい．
        - うまくいくか不明．
    - 普通の言語モデル: 言語で学習 => 論理を解く
    - ours: 論理を解く => 言語で学習
    - 論理が言語構造をよく捉えているなら，性能が上がるはず．





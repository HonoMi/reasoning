# ideas.rejected
* [rejected] verifier x MBRL
    - 我々のタスク，教師あり学習ができる．探索は必要ない．
* [rejected] syntactic + semantic (SAT) のマルチタスク
    - [rejected] "syntax(proof generation)に対してsemantic(SAT)のマルチタスクは意味があるか？"
    - 考察
    - Pros
        - 奇抜なので通りそう．
    - Cons
        - 上手くいくか不明．
    - 手法
    - 備考
        - Pushing the limit をベースにするのが良い．
* 「TransformerはStackを模擬できない」は，logical inferneceする上で課題になるか？ なるなら，解決が必要．
    - [rejected]
        * 我々の研究をやる上では，LSTMを上にのせて対応する．
        * 少し難しいので．ただし，not not not みたいなのはTransformerは苦手かもしれない．それを無視して研究を進めると，うまくいかないか，reviewerに突っ込まれるかもしれない．
        * 競争率が激しそう．
    - not not not not みたいなのは認識できない可能性が高い．
    - 自然言語では問題にならない? not not not みたいなのは現れないから．
    - 理論的には，「decoderステップが無限」「infinite precision」などの仮定を置けば，Transformersはturing完全である．一方で，これを置かないと，Turing完全では無いという．
    - On the Ability and Limitations of Transformers to Recognize Formal Languages
        * counter languageの一部しかうまくいかないという．not not not は認識できないか？

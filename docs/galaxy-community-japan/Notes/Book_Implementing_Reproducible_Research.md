------------------------------------------

### 1.1 Introduction

現在の研究において計算ツールは重要である。しかし、計算結果が信頼でき、理解でき、再現可能であることにあまり注意が払われていない。

### 1.2 Computational research

-   数ヶ月後に結果に問題があることがわかった場合にどうするか
-   図表などを作成するためにどのボタンをクリックしたか覚えているか
-   プログラムやワークフロー全体にエラーがないことを検証できるか
-   他の研究者が新しい方法を試すためにこのステップを再現できるか、また結果がどのように得られたか理解できるか

再現性の意味するところでは、計算結果を出すという「move forward」だけでなく、「move back」ー ステップを遡って、前提を変更し、最後計算を実行する ー が必要である。残念なことに、研究者はこれを難しくするか多くの場合不可能にしてしまっている。

＃ この実例や統計はここでは提示されていない

再実験が難しい場合（ハッブル宇宙望遠鏡）や再計算が難しい場合（最高性能のスパコンを使用している、データが膨大で移動できない）にも、縮小データを使用するなどの方法で試験することができる。

＃ なぜこのような事例を挙げたのか。ゲノム情報解析の場合には、実験データは多くの場所で取得され多くの研究者がそれぞれ解析するという異なる背景がある。

#### 1.2.1 Computational research life cycle

いくつかの教訓

-   結果を公表するという圧力により、結果をつくるために前に進めることをしても、前提に戻り、新たなコードやパラメータを用いて以前の実験を再現するすることは稀である。再現のためにはワークフローを遡ることが必要であり、これは著者にとってさえ難しい

<!-- -->

-   多くのワークフローは手作業による情報の移動を伴うため、計算パイプラインが実際に出力する最終結果がいつでも同じになる可能性は低い

<!-- -->

-   技術的な問題と社会的な問題がある。このチャプターではツールに焦点を当てているが、実際には研究者が意識的に向上された仕事の方法を採用してはじめて、この問題は解決される。

ジャーナルに投稿しようとして準備している時に再現性を問われても既に遅い。研究の初めから取り組むべき問題である。

1.2.2 Open source ecosystem

1.2.3 Communities of practice

1.3 Routine practice

1.3.1 Version control

1.3.2 Execution automation

1.3.3 Testing

1.3.4 Readability

#### 1.3.5 Infrastructure

テストの実行、プロジェクトのビルド、レポートの作成、に要する負荷が大きくなる。

-   Hosted version control
-   Continuous integration
-   Documentation generation systems

\[24\] C. Neylon, J. Aerts, C. T. Brown, D. Lemire, K. J. Millman, P. Murray-Rust, F. P ́erez, N. Saunders, A. Smith, G. Varoquaux, and E. Willighage. Changing computational research. The challenges ahead. Source Code for Biology and Medicine, 7(1):2, 2012.

1.4 Collaboration

1.4.1 Distributed version control

1.4.2 Code review

1.4.3 Infrastructure redux

1.5 Communication

1.5.1 Literate programming

1.5.2 Literate computing

1.5.3 IPython notebook

### 1.6 Conclusion

(1) sharing of scientific software, data, and knowledge necessary for reproducible research

(2) readable, tested, validated, and documented software as the basis for reliable scientific outcomes

(3) high standards of computational literacy in the education of mathemati- cians, scientists, and engineers

(4) open source software developed by collaborative, meritocratic communities of practice.
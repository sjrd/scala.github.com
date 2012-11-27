---
layout: overview-large
title: はじめに

disqus: true

partof: collections
num: 1
language: ja
---

**Martin Odersky, Lex Spoon 著**<br>
**Eugene Yokota 訳**  

Scala 2.8 の変更点で最も重要なものは新しいコレクションフレームワークだと多くの人が思っている。確かに以前の Scala にもコレクションはあったが、Scala 2.8 になって一貫性があり、包括的な、統一コレクション型のフレームワークを提供することができた。(大部分において新しいフレームワークは旧版と互換性がある)

コレクションの変更点は、一見すると微細なものだがプログラミングスタイルに重大な変化をもたらすことができる。コレクション内の要素ではなくコレクション全体を、プログラムを組む時の基本的なパーツとすることで、あたかも一つ上の階層でプログラミングをしているかのように感じるだろう。この新しいスタイルには慣れを必要とするが、幸い新しいコレクションの特長のお陰で楽に適合できるようになっている。
新しいコレクションは簡単に使えて、短く書けて、安全、高速で、統一性がある。

**簡単に使える:** 基本的なボキャブラリは、演算 (operation) とよばれる 20〜50 のメソッドから成る。これを身につけてしまえば、二つの演算を組み合わせるだけで、ほとんどのコレクションの問題を解決することができる。複雑なループ構造や再帰に頭を悩ませる必要はない。
永続的なコレクションと副作用のない演算は、既存のコレクションを間違って新しいデータで壊してしまう心配をする必要がないことを意味する。イテレータとコレクションの更新の間の干渉は完全になくなった。

**短く書ける:** 一つまたは複数のループが必要だった作業を単語一つで実現することができる。関数型の演算もライトウェイトな構文で表現でき、簡単に演算を組み合わせることでカスタム代数を扱っているかのように感じるはずだ。 

**安全である:** これは実際に経験してみないと分からないだろう。
静的型付きでかつ関数型という Scala のコレクションの特徴は、可能なエラーの圧倒的多数はコンパイル時に捕捉されることを意味する。
その理由は、

<ol>
<li>コレクションの演算はが大量に使用されているため、十分にテスト済みである。</li>
<li>コレクション演算を使うことで、インプットとアウトプットが関数のパラメータと結果という形で明示的になる。</li>
<li>これらの明示的なインプットとアウトプットは静的な型チェックの対象だ。</li>
</ol>

結論としては、誤用の大多数が型エラーとして表出するということだ。数百行のプログラムが初回の一発で実行できることは決して稀ではない。

**速い:** コレクション演算はライブラリの中で調整され最適化されている。その結果、コレクションを使用することは一般的にかなり効率的だ。手作業で丁寧に調整されたデータ構造と演算により多少高速化することができるかもしれないが、不適切な実装上の決断を途中でしてしまうとかなり遅くなってしまうということもありえる。さらに、現在コレクションはマルチコア上での並列実行に適応されている途中だ。並列コレクションは順次コレクションと同じ演算をサポートするため、新しい演算を習ったりコードを書き変えたりする必要はない。`par` メソッドを呼ぶだけで順次コレクションを並列に変えることができる。

**統一性がある:** コレクションは出来る限りどの型にも同じ演算を提供している。これにより、少ないボキャブラリの演算でも多くのことができる。例えば、文字列は概念的には文字の列 (sequence) だ。その結果、Scalaのコレクションでは、文字列は列の演算の全てをサポートする。配列に関しても同様だ。

**用例:** これは Scala のコレクションの多くの利点を示す一行コードだ。

    val (minors, adults) = people partition (_.age < 18)

この演算が何をしているかは一目瞭然だ: `people` のコレクションを年齢によって　`minors` と `adult` に分割している。`partition` メソッドはコレクションの基底型である `TraversableLike` で定義されているため、このコードは配列を含むどのコレクションでも動作する。これによって生じる `minors` と `adult` のコレクションは、`people`コレクションと同じ型となる。

このコードは、従来の 1〜3個のループを用いたコレクション処理に比べて、より簡潔なものになっている　(中間結果をどこかにバッファリングする必要があるため、配列を用いた場合はループ3個)。基本的なコレクションのボキャブラリを覚えてしまえば、明示的なループを書くよりも、このようなコードを書く方が簡単かつ安全だと思うようになるだろう。さらに、`partition` 演算は高速であり、将来的にはマルチコア上で並列コレクションにかけると更に速くなるだろう。(並列コレクションは開発ビルドに入っており、Scala 2.9 の一部としてリリースされる予定。)

このガイドはユーザーの視点から Scala 2.8 のコレクションクラスの API について詳細に説明する。全ての基本的なクラスと、それらのメソッドをみていこう。
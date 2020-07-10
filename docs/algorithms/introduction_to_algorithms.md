# 概要

## 本

- [アルゴリズムイントロダクション 第3版 総合版 (世界標準MIT教科書) | T. コルメン, R. リベスト, C. シュタイン, C. ライザーソン, Thomas H. Cormen, Clifford Stein, Ronald L. Rivest, Charles E. Leiserson, 浅野 哲夫, 岩野 和生, 梅尾 博司, 山下 雅史, 和田 幸一](https://amzn.to/2F8X8M4)

## 参考

- [Introduction to Algorithms, Third Edition | The MIT Press](https://mitpress.mit.edu/books/introduction-algorithms-third-edition)
- [演習問題の解答](http://mitp-content-server.mit.edu:18180/books/content/sectbyfn?collid=books_pres_0&id=8030&fn=Intro_to_Algo_Selected_Solutions.pdf)

## かかった時間

- x 時間

## 読む前の状態

- xxx

## 進め方

- 演習問題は解いていない

## 感想

- xxx

# 読書メモ

## 1. 計算におけるアルゴリズムの役割

- アルゴリズムは、ある値または値の集合を入力(input)として取り、ある値または値の集合を出力(output)として生成する、明確に定義された計算手続き
- データ構造は、アクセスと更新を容易にする目的のために、データを蓄積し組織化する方法
  - どのデータ構造もすべての目的に対して満足に働くことはない。したがって、いくつかのデータ構造について、その長所と限界を理解することが重要

## 2. さあ、始めよう

- 挿入ソート(insertion sort)
  - トランプの手札をソートするようなイメージ
    - 左手を空にし、テーブルの上にカードを裏向きに置く
    - テーブルから 1 枚ずつカードを取って、左手の正しい位置に挿入していく

```
for j = 2 to A.length
  key = A[j]
  // A[j]をソート済みの列A[1..j-1]に挿入する
  i = j - 1
  while i > 0 かつ A[i] > key
    A[i + 1] = A[i]
    i = i - 1
  A[i + 1] = key
```

- ループ不変式(loop invariant)
  - 簡単に言うと、ループの全ての反復に対して、保持される条件
  - 厳密に言うと以下の 3 つの性質がある
    - 初期条件
      - ループの実行開始直前で、ループ不変式は真
    - ループ内条件
      - ループの何回目かの繰り返しの直前でループ不変式が真ならば、次の繰り返しの直前でも真である
    - 終了条件
      - ループが停止した時、アルゴリズムの正当性の証明につながる有力な性質が不変式から得られる
- アルゴリズムの解析
  - アルゴリズムの実行に必要な資源量を予測することを、アルゴリズムを解析する(analyzing)という
  - 多くの場合、測定したいのは計算時間
  - アルゴリズムを解析するには、使用する実現技術のモデルを設定する必要がある。このモデルには実現技術で用いられる資源とそのコストのモデルが含まれる
    - 本書では、基本的な単一プロセッサの計算モデルであるランダムアクセスマシン(RAM)を実現技術として仮定し、アルゴリズムは計算機プログラムとして実現する
    - RAM モデルでは命令は 1 つずつ逐次的に実行され、並列演算は存在しない
- 実行時間
  - ある特定の入力に対するアルゴリズムの実行時間(running time)は、実行される基本演算または「ステップ」の数
- 最悪時と平均時の解析
  - 本書では通常の場合、最悪実行時間(worst-case running time)だけを考える
    - 以下の 3 つが理由
      - 実行時間の上界を知ることで、アルゴリズムの実行にそれ以上の時間がかからないことを保証できる
      - あるアルゴリズムでは、最悪の場合がかなり頻繁に生ずることがある
      - 平均的な場合が、最悪の場合と同じくらい悪いことが多い
- 増加のオーダ(order of growth)
  - 実行時間の増加率が重要
  - 例えば挿入ソートだと θ(n^2) となる
- 分割統治法(devide-and-conquer)
  - 再帰構造を持つアルゴリズムは、多くの場合、分割統治法に基づいて設計されている
  - 分割統治パラダイムの再帰の各レベルは以下の 3 つの段階から構成される
    - 分割
      - 問題をいくつかの同じ問題のより小さいインスタンスである部分問題に分割する
    - 統治
      - 部分問題を再帰的に解くことによって統治する。ただし、部分問題のサイズが十分小さい場合は直接的な方法で解く
    - 結合
      - 部分問題の解を組み合わせて元の問題の解を得る
- マージソート(merge sort)

```
MERGE-SORT(A, p, r)
if p < r
  q = [(p + r) / 2]
  MERGE-SORT(A, p, q)
  MERGE-SORT(A, q + 1, r)
  MERGE(A, p, q, r)

MERGE(A, p, q, r)
n1 = q - p + 1
n2 = r - q
// L[1..n1+1] と R[1..n2+1] を2つの新しい配列とする
for i = 1 to n1
  L[i] = A[p + i - 1]
for j = 1 to n2
  R[i] = A[q + j]
L[n1 + 1] = ∞
R[n2 + 1] = ∞
i = 1
j = 1
for k = p to r
  if L[i] <= R[j]
    A[k] = L[i]
    i = i + 1
  else
    A[k] = R[j]
    j = j + 1
```

- 分割統治アルゴリズムの解析
  - 問題を a 個の部分問題に分割され、各部分問題のサイズは元の問題の 1/b であるとする
  - また問題を部分問題に分割するのに D(n) 時間かかり、部分問題の解を結合するのに C(n) 時間かかるとする
  - 以下の漸化式で表せる
    - T = θ(1) (n <= c の時)
    - T = aT(n/b) + D(n) + C(n) (それ以外の時)
  - このような形式の漸化式を解く方法は 4 章で学ぶ
  - マージソートの場合は θ(nlgn)

## 3: 関数の増加

- θ記法(θ-notation)
  - 定義
    - ある与えられた関数 g(n) に対して、θ(g(n)) を以下のように定義する
      - θ(g(n)) = {f(n): ある正の定数 c1, c2, n0 が存在して、すべての n >= n0 に対して 0 <= c1g(n) <= f(n) <= c2g(n) を満たす}
  - 上記の時、g(n) は f(n) の漸近的にタイトな限界(asymptotically tight bound)という
- Ｏ記法(Ｏ-notation、ビッグオーまたはオー)
  - θ記法は関数を漸近的に上下から限定する。漸近的上界(asymptotic upper bound)だけに関心がある時はＯ記法を用いる
  - 定義
    - Ｏ(g(n)) = {f(n): ある正の定数 c, n0 が存在して、すべての n >= n0 に対して 0 <= f(n) <= cg(n) を満たす}
- Ω記法(Ω-notation、ビッグオメガまたはオメガ)
  - Ｏ記法が関数の漸近的上界を与えるのに対し、Ω記法は漸近的下界(asymptotically lower bound)を与える
  - 定義
    - Ω(g(n)) = {f(n): ある正の定数 c, n0 が存在して、すべての n >= n0 に対して 0 <= cg(n) <= f(n) を満たす}
- o 記法(o-notation、リトルオー)
  - 漸近的にタイトでない上界を表すのに o 記法を用いる
  - 定義
    - o(g(n)) = {f(n): 任意の正の定数 c > 0 に対して、ある正の定数 n0 > 0 が存在して、すべての n >= n0 に対して 0 <= f(n) < cg(n) を満たす}
- ω記法(ω-notation、リトルオメガ)
  - 漸近的にタイトでない下界を表すのにω記法を用いる
  - 定義
    - ω(g(n)) = {f(n): 任意の正の定数 c > 0 に対して、ある正の定数 n0 > 0 が存在して、すべての n >= n0 に対して 0 <= cg(n) < f(n) を満たす}
- 関数の比較
  - 推移性
    - f(n) = θ(g(n)) かつ g(n) = θ(h(n)) ならば f(n) = θ(h(n))
    - f(n) = Ｏ(g(n)) かつ g(n) = Ｏ(h(n)) ならば f(n) = Ｏ(h(n))
    - f(n) = Ω(g(n)) かつ g(n) = Ω(h(n)) ならば f(n) = Ω(h(n))
    - f(n) = o(g(n)) かつ g(n) = o(h(n)) ならば f(n) = o(h(n))
    - f(n) = ω(g(n)) かつ g(n) = ω(h(n)) ならば f(n) = ω(h(n))
  - 反射性
    - f(n) = θ(f(n))
    - f(n) = Ｏ(f(n))
    - f(n) = Ω(f(n))
  - 対称性
    - f(n) = θ(g(n)) の時、かつその時に限り g(n) = θ(f(n))
  - 転置対称性
    - f(n) = Ｏ(g(n)) の時、かつその時に限り g(n) = Ω(f(n))
    - f(n) = o(g(n)) の時、かつその時に限り g(n) = ω(f(n))

## 4: 分割統治

- 漸化式(recurrence)
  - 漸化式はある入力に対する関数値を、それより小さい入力に対する関数値を用いて記述する等式または不等式である
- 漸化式を解く方法 3 つ
  - 置換え法(substitution method)
    - まず限界を推測し、次にその推測が正しいことを数学的帰納法を用いて証明する
  - 再帰木法(recursion-tree method)
    - 節点が再帰の各レベルで必要なコストを表現する木の形に漸化式を変形し、和を上または下から抑える技法を用いて漸化式を解く
  - マスター法(master method)
    - 定数 a >= 1、b >= 1 と与えられた関数 f(n) によって `T(n) = a*T(n/b) + f(n)` と表現できる漸化式に対する限界を与える
- 最大部分配列問題
  - 配列 A に対して、要素の和を最大化する A の連続する部分配列を発見する問題
  - この連続する部分配列を最大部分配列(maximum subarray)と呼ぶ
  - 分割統治による解
    - 部分配列の中央点 mid を計算し、部分配列 A[low ... mid] と A[mid+1 ... high] を考察する。すると、最大部分配列 A[i ... j] が見つかるのは以下の 3 つのどこかになる
      - 完全に部分配列 A[low ... mid] の中にある。従って low <= i <= j <= mid
      - 完全に部分配列 A[mid+1 ... high] の中にある。従って mid + 1 <= i <= j <= high
      - 中央点をまたいでいる。従って low <= i <= mid < j <= high

```
FIND-MAXIMUM-SUBARRAY(A, low, high)
if high == low
  return (low, high, A[low]) // 基底。要素数1
else mid = (low + high) / 2
  (left-low, left-high, left-sum) = FIND-MAXIMUM-SUBARRAY(A, low, mid)
  (right-low, right-high, right-sum) = FIND-MAXIMUM-SUBARRAY(A, mid + 1, high)
  (cross-low, cross-high, cross-sum) = FIND-MAX-CROSSING-SUBARRAY(A, low, mid, high)
if left-sum >= right-sum and left-sum >= cross-sum
  return (left-low, left-high, left-sum)
else if right-sum >= left-sum and right-sum >= cross-sum
  return (right-low, right-high, right-sum)
else
  return (cross-low, cross-high, cross-sum)

FIND-MAX-CROSSING-SUBARRAY(A, low, mid, high)
left-sum = -∞
sum = 0
for i = mid downto low
  sum = sum + A[i]
  if sum > left-sum
    left-sum = sum
    max-left = i
right-sum = -∞
sum = 0
for j = mid + 1 to high
  sum = sum + A[j]
  if sum > right-sum
    right-sum = sum
    max-right = i
return (max-left, max-right, left-sum + right-sum)
```

- 行列積
  - 単純な分割統治
    - 以下を利用する
      - `C11 = A11 * B11 + A12 * B21`
      - `C12 = A11 * B12 + A12 * B22`
      - `C21 = A21 * B11 + A22 * B21`
      - `C22 = A21 * B12 + A22 * B22`
    - n/2 * n/2 型行列の積の計算は T(n/2) 時間かかる
    - 加算にはそれぞれ θ(n^2) 時間かかる
    - 以上から
      - `T(n) = θ(1)` n = 1 の時
      - `T(n) = 8T(n/2) + θ(n^2)` n > 1 の時

```
SQUARE-MATRIX-MULTIPLY-RECURSIVE
n = A.rows
// C を新しい n * n 型行列とする
if n == 1
  c11 = a11 * b11
else
  C11 = SQUARE-MATRIX-MULTIPLY-RECURSIVE(A11, B11) + SQUARE-MATRIX-MULTIPLY-RECURSIVE(A12, B21)
  C12 = SQUARE-MATRIX-MULTIPLY-RECURSIVE(A11, B12) + SQUARE-MATRIX-MULTIPLY-RECURSIVE(A12, B22)
  C21 = SQUARE-MATRIX-MULTIPLY-RECURSIVE(A21, B11) + SQUARE-MATRIX-MULTIPLY-RECURSIVE(A22, B21)
  C22 = SQUARE-MATRIX-MULTIPLY-RECURSIVE(A21, B12) + SQUARE-MATRIX-MULTIPLY-RECURSIVE(A22, B22)
return C
```

- Strassen の方法
  - 8 回あった n/2 * n/2 型行列の計算を 7 回で済ませる
  - 10 個の n/2 * n/2 型行列 S1, S2,..., S10 を生成し、7 個の行列積 P1, P2,..., P7 を再帰的に計算する
  - すると Strassen のアルゴリズムの実行時間 T(n) は以下のようになる
    - `T(n) = θ(1)` n = 1 の時
    - `T(n) = 7T(n/2) + θ(n^2)` n > 1 の時
- 置換え法(substitution method)
  - 2 段階で漸化式を解く
    - 解の形を推定する
    - 数学的帰納法を用いて定数を求め、推定した解がうまく働くことを示す
  - 解の正しさを簡潔に証明するのに適しているが、うまい解の推定に苦労することがある
- 再帰木法(recursion tree)
  - 再帰木では、各節点はある再帰関数呼び出しに対応する部分問題のコストを表す
  - 再帰木の各レベルにおいて、そのレベルに属する節点のコストの総和を取ることでレベルごとのコストを求め、レベルごとのコストの総和を取ることで再帰木全体のコストを求める
  - 再帰木はいい推定を得るための最適の道具。そして得た推定の正しさを置換え法によって検証する
- 漸化式を解くためのマスター法
  - a >= 1 と b > 1 を定数、f(n) を関数とする。非負整数上の関数 T(n) を漸化式 `T(n) = a * T(n/b) + f(n)` によって定義する。すると T(n) は漸近的に次の限界を持つ
    - ある定数 ε > 0 に対して f(n) = Ｏ(n^logb a-ε) ならば、`T(n) = θ(n^logb a)`
    - f(n) = θ(n^logb a) ならば、`T(n) = θ(n^logb a * lg n)`
    - ある定数 ε > 0 に対して f(n) = Ω(n^logb a+ε) であり、しかもある定数 c < 1 と十分大きな全ての n に対して `a * f(n/b) <= c * f(n)` ならば、`T(n) = θ(f(n))`

## 5: 確率的解析と乱択アルゴリズム

- 確率的解析(probabilistic analysis)
  - 確率を用いる問題の解析を確率的解析という
- 乱択アルゴリズム(randomized algorithm)
  - 一般的に、アルゴリズムの振る舞いが入力と乱数生成器(random-number generator)が生成する値の両方によって決まる時、このアルゴリズムを乱択アルゴリズムと呼ぶ
- 期待実行時間(expected running time)
  - 乱択アルゴリズムの実行時間を解析する時には、乱数生成器が返す値の分布の上で実行時間の期待値を取る
  - 乱択アルゴリズムの実行時間を期待実行時間と呼ぶ
- PERMUTE-BY-SORTING
  - 多くの乱択アルゴリズムでは、与えられた入力配列を置換することで入力をランダム化する。PERMUTE-BY-SORTING はこのやり方の 1 つ。あとでもう 1 つの RANDOMIZE-IN-PLACE を紹介する
  - PERMUTE-BY-SORTING は配列の各要素 A[i] に対してランダムに優先度 P[i] を割り当て、A の要素を優先度に従ってソートするやり方
  - マージソートに `θ(n * lg n)` 時間かかるので、このやり方は `Ω(n * lg n)` 時間かかる

```
PERMUTE-BY-SORTING(A)
n = A.length
P[1..n] を新しい配列とする
for i = 1 to n
  P[i] = RANDOM(1, n^3) // 1 から n^3 の間で乱数を選ぶ
P をキーとして A をソートする
```

- RANDOMIZE-IN-PLACE
  - これは `Ｏ(n)` 時間で実現でき、PERMUTE-BY-SORTING より速い

```
RANDOMIZE-IN-PLACE(A)
n = A.length
for i = 1 to n
  A[i] と A[RANDOM(i, n)] を置き換える
```

## 6: ヒープソート

- ヒープ(heap)
  - (2 分木)ヒープデータ構造は、おおよそ完全 2 分木と見なすことができる配列オブジェクト
  - ヒープを表現する配列は 2 つの属性を持つ
    - `A.length` は通常通り配列の要素数を表す
    - `A.heap-size` は配列 A に格納されているヒープの要素数を表す
- max ヒープと min ヒープ
  - max ヒープ
    - 根以外の任意の節点 i が `A[Parent(i)] >= A[i]` の max ヒープ条件を満たすもの
  - min ヒープ
    - 根以外の任意の節点 i が `A[Parent(i)] <= A[i]` の max ヒープ条件を満たすもの
- 高さ
  - ヒープを木とみなした時、ヒープにおける節点の高さを、その節点からある葉に下る最長の単純パスに含まれる変数と定義する
  - ヒープの高さは、その根の高さと定義する
  - n 個の要素を含むヒープの高さは完全二分木に基づいているので θ(lg n) である
- ヒープ条件の維持
  - max ヒープ条件を維持するために手続き MAX-HEAPIFY を呼び出す
  - 入れ替える対象となるどちらの子の部分木のサイズも 2n/3 以下だから（最悪の場合、は木の最ものレベルがちょうど半分だけ埋まっている時に起こる）、MAX-HEAPIFY の実行時間は漸化式 `T(n) <= T(2n/3) + θ(1)` で表現できる
    - 計算すると `T(n) = Ｏ(lg n)` である

```
MAX-HEAPIFY(A, i)
l = LEFT(i)
r = RIGHT(i)
if l <= A.heap-size かつ A[l] > A[i]
  largest = l
else
  largest = i
if r <= A.heap-size かつ A[r] > A[largest]
  largest = r
if largest != i
  A[i] を A[largest] と交換する
  MAX-HEAPIFY(A, largest)
```

- ヒープの構築
  - ボトムアップ的に手続き MAX-HEAPIFY を適用することで、配列 A[1..n] を max ヒープに変換できる

```
BUILD-MAX-HEAP(A)
A.heap-size = A.length
for i = A.length/2 downto 1
  MAX-HEAPIFY(A, i)
```

- ヒープソートアルゴリズム
  - 最初に max ヒープを構築し、そこから順々に最大要素を取り出していく
  - 実行時間は `Ｏ(n * lg n)`
    - 実行時間 Ｏ(lg n) の MAX-HEAPIFY が n - 1 回呼ばれる

```
HEAPSORT(A)
BUILD-MAX-HEAP(A)
for i = A.length downto 2
  A[1] を A[i] と交換する
  A.heap-size = A.heap-size - 1
  MAX-HEAPIFY(A, 1)
```

## 7: クイックソート

- クイックソート(quick sort)
  - 分割統治法の 3 つの段階を以下に示す
    - 分割
      - 配列 A[p..r] を 2 つの(空の可能性もある)部分配列 A[p..q-1] と A[q+1..r] に、A[p..q-1] のどの要素も A[q] 以下となり、A[q+1..r] のどの要素も A[q] 以上となるように分割(再配置) する。添字 q はこの分割手続きの中で計算する
    - 統治
      - 2 つの部分配列 A[p..q-1] と A[q+1..r] をクイックソートを再帰的に呼び出すことでソートする
    - 結合
      - 2 つの部分配列はソート済みだから、これらを結合するために必要な仕事はない。配列全体 A[p..r] はソートされている

```
QUICKSORT(A, p, r)
if p < r
  q = PARTITION(A, p, r)
  QUICKSORT(A, p, q - 1)
  QUICKSORT(A, q + 1, r)

PARTITION(A, p, r)
x = A[r]
i = p - 1
for j = p to r - 1
  if A[j] <= x
    i = i + 1
    A[i] を A[j] と交換する
A[i + 1] を A[r] と交換する
return i + 1
```

- クイックソートの性能
  - クイックソートの実行時間はピボットとなる要素に依存する
  - 最悪の分割
    - 分割手続きが元の問題を要素数 n - 1 の部分問題と要素数 0 の部分問題に分割した時
    - `T(n) = θ(n^2)` となる
  - 最良の分割
    - 各サイズが高々 n / 2 の部分問題となるよう分割した時
    - `T(n) = θ(n * lg n)` となる
  - クイックソートの平均実行時間は最悪の場合よりも最良の場合に近い
    - 分割アルゴリズムが常に 9:1 で分割するとしても総コストは `θ(n * lg n)` である
- クイックソートの解析
  - 比較総数の期待値を計算すると `Ｏ(n * lg n)` となる。詳しくは p149

## 8: 線形時間ソート

- 決定木モデル(decision tree)
  - 比較ソートは決定木の概念を用いて抽象的に表現できる
  - 決定木は与えられたサイズの入力上で動作する特定のソーティングアルゴリズムが実行する要素間の比較を表現する全 2 分木である
  - すべての置換が、到達可能な葉として出現する
- 最悪時の下界
  - 決定木の根から任意の葉までの単純パスの長さの最大値が、対応するソーティングアルゴリズムの最大比較回数となる
- 計数ソート(counting sort)
  - n 個の入力要素はある整数 k に対して 0 から k の範囲の整数から選ばれると仮定する
  - k = Ｏ(n) ならば計数ソートは Ｏ(n) 時間で走る
  - 計数ソートは比較ソートではないので、下界 Ω(n * lg n) を超えることができる

```
COUNTING-SORT(A, B, k)
C[0..k] を新しい配列とする
for i = 0 to k
  C[i] = 0
for j = 1 to A.length
  C[A[j]] = C[A[j]] + 1 // C[i] は i と等しい要素の数を示す
for i = 1 to k
  C[i] = C[i] + C[i - 1] // C[i] は i 以下の要素の数を示す
for j = A.length downto 1
  B[C[A[j]]] = A[j]
  C[A[j]] = C[A[j]] - 1
```

- 安定性(stability)
  - 同じ値の要素は入力に出現する順序で、出力に出現するという性質
- 基数ソート(radix sort)
  - 紙パンチカードのためのソート機械が用いていたアルゴリズム
  - 最下位の桁から順にソートしていく。ただし、各桁のソートは安定でなければならない
  - 計数ソートを安定な中間ソートに用いる基数ソートはその場でのソートではないという欠点もある
    - 主記憶領域が高価な時は、クイックソートのようなその場でのソーティングアルゴリズムを選択することもある

```
RADIX-SORT
for i = 1 to d
  安定ソートを用いて、第 i 桁に関して配列 A をソートする
```

- バケツソート(bucket sort)
  - バケツソートは入力が一様分布から抽出されると仮定する時、平均実行時間 Ｏ(n) を達成する
  - バケツソートは入力を n 個の等しい大きさのバケツと呼ぶ部分区間に入力し、n 個の入力をバケツに分配する

```
BUCKET-SORT(A)
n = A.length
B[0..n-1] を新しい配列とする
for i = 0 to n - 1
  B[i] を空リストに初期化する
for i = 1 to n
  A[i] をリスト B[[nA(i)]] に挿入する
for i = 0 to n - 1
  リスト B[i] を挿入ソートでソートする
リスト B[0], B[1],...,B[n - 1] をこの順序で連結する
```

## 9: 中央値と順序統計量

- 順序統計量(ith order statistic)
  - n 個の要素を持つ集合の i 番目の順序統計量はその集合の中で i 番目に小さい要素である
- 最小値の決定

```
MINIMUM(A)
min = A[1]
for i = 2 to A.length
  if min > A[i]
    min = A[i]
return min
```

- RANDOMIZED-SELECT
  - 配列 A[p..r] の i 番目に小さい要素を返す
  - 最悪実行時間は `θ(n^2)` だが、期待実行時間は `Ｏ(n)`

```
RANDOMIZED-SELECT(A, p, r, i)
if p == r
  return A[p]
q = RANDOMIZED-PARTITION(A, p, r)
k = q - p + 1
if i == k // ピボット値が答
  return A[q]
else if i < k
  return RANDOMIZED-SELECT(A, p, q - 1, i)
else
  return RANDOMIZED-SELECT(A, q + 1, r, i - k)
```

- SELECT
  - 最悪実行時間 `Ｏ(n)` で配列を必ず上手に分割する
  - 手順
    - 1: 要素数 n の入力配列をそれぞれ 5 個の要素からなる n/5 個のグループと残りの n mod 5 個の要素からなる高々 1 個のグループに分割する
    - 2: 各グループの(高々 5 個の)要素を挿入ソートによってソートし、ソートされた n/5 個のグループのそれぞれから中央値を選択する
    - 3: SELECT を再帰的に用いて、ステップ 2 で発見した n/5 個の中央値の中央値 x を発見する
    - 4: 修正版の PARTITION を用いて入力配列を入力配列を中央値の中央値 x をピボットとして分割する。k を分割の下側(x よりも小さい要素の集合)に属する要素数 +1 とする。この時、x は k 番目に小さい値であり、分割の上側には n - k 個の要素が属する
    - 5: i = k ならば x を返す。i < k ならば、分割の下側から i 番目に小さい要素を SELECT を再帰的に用いて発見し、i > k ならば、分割の上側から (i - k) 番目に小さい要素を SELECT を再帰的に用いて発見する
  - 実行時間
    - `T(n) <= Ｏ(1)` n < 140 の場合
    - `T(n) <= T(n/5) + T(7n/10 + 6) + Ｏ(n)` n >= 140 の場合
      - これを解くと、最悪実行時間が線形になることがわかる

## 10: 基本データ構造

- スタックとキュー
  - スタックとキューは削除操作 DELETE が決まった要素を削除する動的集合である
    - スタックは最後に挿入した要素を削除する。後入れ先出し(LIFO: last-in, first-out)方策を実現する
    - キューは集合に最も長時間滞在した要素を削除し、先入れ先出し(FIFO: first-in, first-out)方策を実現する
- スタック(stack)
  - スタックでは INSERT 操作を PUSH、DELETE 操作を POP と呼ぶ

```
STACK-EMPTY(S)
if S.top == 0
  return TRUE
else
  return FALSE

PUSH(S)
S.top = S.top + 1
S[S.top] = x

POP(S)
if STACK-EMPTY(S)
  error "アンダーフロー"
else
  S.top = S.top - 1
  return S[S.top + 1]
```

- キュー(queue)
  - キューでは挿入操作 INSERT を ENQUEUE、削除操作 DELETE を DEQUEUE と呼ぶ
  - 操作の実行にはそれぞれ `Ｏ(1)` 時間かかる

```
ENQUEUE(Q, x)
Q[Q.tail] = x
if Q.tail == Q.length
  Q.tail = 1
else
  Q.tail = Q.tail + 1

DEQUEUE(Q)
x = Q[Q.head]
if Q.head == Q.length
  Q.head = 1
else
  Q.head = Q.head + 1
return x
```

- 連結リスト(linked list)
  - オブジェクトがある順序で一列に並ぶデータ構造を連結リストという
  - 配列では添字によってオブジェクトの線形順序が決まるが、連結リストの線形順序は各オブジェクトが持つポインタによって決まる
  - いくつかの種類がある。一方向 / 双方向、ソート済み / 未ソート、循環 / 非循環、がある
- 双方向連結リスト(doubly linked list)
  - 双方向連結リスト L の各要素はキー属性 key と 2 つのポインタ属性、next と prev を持つオブジェクト
- 循環リスト(circular list)
  - リストの先頭の prev ポインタがリストの末尾を指し、末尾の next ポインタがリストの先頭を指す
- 以下では、未ソート双方向連結リストを仮定して議論を進める
- 連結リストの探索
  - 手続き LIST-SEARCH(L, k) は、簡単な線形探索によってリスト L からキー k を持つ最初の要素を発見し、その要素を指すポインタを返す

```
LIST-SEARCH(L, k)
x = L.head
while x != NIL かつ x.key != k
  x = x.next
return x
```

- 連結リストへの挿入
  - x を連結リストの先頭に継ぎ足す

```
LIST-INSERT(L, x)
x.next = L.head
if L.head != NIL
  L.head.prev = x
L.head = x
x.prev = NIL
```

- 連結リストからの削除

```
LIST-DELETE(L, x)
if x.prev != NIL
  x.prev.next = x.next
else
  L.head = x.next
if x.next != NIL
  x.next.prev = x.prev
```

- 番兵(sentinel)
  - 番兵は境界条件を簡略化するためのダミーオブジェクト
  - 通常の双方向リストを番兵を持つ双方向循環リストに変える。リストの先頭と末尾の間に番兵 L.nil が置かれ、属性 L.nil.next がリストの先頭を、L.nil.prev がリストの末尾を指す
- 未使用リスト(free list)
  - 多重配列表現が使用する配列の長さを m とし、ある時点で動的集合が n <= m 個の要素を含むとする
  - n 個のオブジェクトが動的集合の要素を表現し、残りの m - n 個のオブジェクトは未使用(free)である
  - 未使用オブジェクトを未使用リストと呼ぶ一方向リストとして管理する

```
ALLOCATE-OBJECT()
if free == NIL
  error = "容量不足"
else
  x = free
  free = x.next
  return x

FREE-OBJECT(x)
x.next = free
free = x
```

- 根付き木の表現
  - 2 分木
    - 属性 p、left、right を用いて 2 分木 T の各節点の親、左の子、右の子を指すポインタを格納する
  - 分岐数に制約のない根付き木
    - 左-子、右-兄弟表現(left-child, right-sibling representation)
      - 2 つのポインタを持つ
        - x.left-child は節点 x の最左の子を指す
        - x.right-sibling は x のすぐ右の兄弟を指す
  - これら以外にも、根付き木は別の方法で表現できる

## 11: ハッシュ表

- ハッシュ表と配列の直接アドレス指定法
  - 出現可能なキーの個数に比べ、実際に格納されるキーの個数が少ない場合には、実際に格納されるキーの個数に比例する大きさの配列しか使用しないハッシュ表が、配列に対する直接アドレス指定法に代わる効果的な手法となる
- 直接アドレス表(direct-address table)
  - 動的集合を表現するために、直接アドレス表と呼ぶ配列 T[0..m-1] を用いる
  - 配列の各位置を枠(slot)と呼ぶ
  - 辞書操作の実現は以下。どれも `Ｏ(1)` 時間で走る

```
DIRECT-ADDRESS-SEARCH(T, k)
return T[k]

DIRECT-ADDRESS-INSERT(T, x)
T[x.key] = x

DIRECT-ADDRESS-DELETE(T, x)
T[x.key] = NIL
```

- ハッシュ表
  - 直接アドレス表の弱点
    - キーの普遍集合 U が非常に大きい時は、利用可能な記憶領域上に表 T を格納できない
    - 実際に格納されるキーの集合が U に比較して非常に小さい時には、T に割り付けられた領域のほとんどが無駄になる
  - 直接アドレス指定法では、キー k を持つ要素は枠 k に格納する。ハッシュ法では、この要素を枠 h(k) に格納する
    - キー k から枠を計算するためにハッシュ関数(hash function) h を用いる
    - h はキーの普遍集合 U からハッシュ表(hash table) T[0..m-1] の枠の集合への写像、すなわち `h: U → {0, 1, ..., m - 1}` であり、普通ハッシュ表のサイズ m は |U| に比べて十分に小さい
  - 衝突(collision)
    - 同じ枠に 2 つのキーがハッシュされる可能性があること
    - チェイン法(chaining)
      - チェイン法では同じ枠にハッシュされた全ての要素を 1 つの連結リストに置く。枠 j に格納されるのは、j にハッシュされた全ての要素を格納するリストの先頭を指すポインタである

```
CHAINED-HASH-INSERT(T, x)
x をリスト T[h(x.key)] の先頭に挿入する

CHAINED-HASH-SEARCH(T, k)
リスト T[h(k)] の中からキー k を持つ要素を探索する

CHAINED-HASH-SEARCH(T, x)
リスト T[h(x.key)] から x を削除する
```

- チェイン法を用いるハッシュ法の解析
  - 負荷率(load factor)
    - n 個の要素を格納している枠数 m のハッシュ表 T の負荷率 α を n / m で定義する。すなわち、α は 1 つのチェインに格納されている要素数の平均
  - 最悪時は n 個のキーが全て同じ枠に入り、探索時間は θ(n) とハッシュ関数の計算時間の和となる
  - 単純一様ハッシュ(simple uniform hashing)仮定
    - 任意に与えられた要素は、他の要素が既にどの枠にハッシュされたかとは無関係に、m 個の任意の枠に等確率でハッシュされると仮定する
    - 失敗に終わる探索、成功に終わる探索どちらの時間の平均も `θ(1 + α)`
- ハッシュ関数
  - 優れたハッシュ関数の条件
    - 単純一様ハッシュ仮定を満足すること
  - 除算法
    - `h(k) = k mod m`
  - 乗算法
    - 最初、キー k にある定数 A(0 < A < 1) をかけ、kA の小数部分を取り出す。続いてこの値に m をかけ、結果の小数部分を切り捨てる
  - 万能ハッシュ法
    - 悪意を持った敵対者がハッシュされるキーを選択するならば、同じ枠にハッシュされる n 個のキーを選択する
    - この状況を打開するための有効な唯一の手段は、格納されつつあるキーとは独立にハッシュ関数をランダムに選択すること
      - 万能ハッシュ法では、利用するハッシュ関数を実行開始時に、関数の集合の中からランダムに選択する
    - こうすると、敵対者が常に最悪になる操作系列を選択することは不可能となり、ハッシュ関数の選択をうまくランダム化することで、どんな操作の列も短い平均実行時間で処理できることが保証される
- オープンアドレス指定法(open addressing)
  - 表の各枠に格納されているのは動的集合の要素か NIL
  - チェイン法のように、表の外部にリストを持ち、そこに要素を格納することはしない。したがって、負荷率 α は 1 を超えない
  - ポインタを格納しないことで空いた記憶はハッシュ表の一部に還元され、同じ記憶料でより多くの枠が実現できる
  - 挿入
    - 挿入を実行するには、キーを置くことができる空の枠に出会うまでハッシュ表を次々と検査、あるいは探査(probe)する
    - 枠を探査する順序は、その時挿入しようとしているキーに依存して決める
  - キーの削除
    - あるキーを枠 i から削除する時、そこに NIL を格納し、i が空であることを示すだけでは不十分
    - NIL の代わりに特別な値 DELETED を格納することで探索を続ける

```
HASH-INSERT(T, k)
i = 0
repeat
  j = h(k, i)
  if T[j] == NIL
    T[j] = k
    return j
  else
    i = i + 1
until i == m
error "ハッシュ表オーバーフロー"

HASH-SEARCH
i = 0
repeat
  j = h(k, i)
  if T[j] == k
    return j
  i = i + 1
until T[j] == NIL または i == m
return NIL
```

- オープンアドレス指定法で頻繁に用いられる 3 つの手法
  - 線形探査法(linear probing)
    - i = 0, 1, ..., m - 1 に対して、ハッシュ関数 `h(k, i) = (h'(k) + i) mod m` を用いる
    - h' を補助ハッシュ関数(auxiliary hash function)と呼ぶ
    - 主クラスタ化(primary clustering)の問題
      - 長い区間の枠がすべて使用中になり、平均探査時間が悪化する問題
      - 連続する i 個の使用中の枠に続く空の枠が次に使用される確率が (i + 1) / m であることがクラスタが現れる理由
  - 2 次関数探査法(quadratic probing)
    - `h(k, i) = (h'(k) + c1 * i + c2 * i^2) mod m` で表現される
    - 同じ初期探査位置を持つ 2 つのキーは同じ探査列を持ってしまう
      - この性質から、副クラスタ化(secondary clustering)と呼ばれる
  - ダブルハッシュ法
    - オープンアドレス指定法に利用できる最良の方法の 1 つがダブルハッシュ法
    - `h(k, i) = (h1(k) + i * h2(k)) mod m` で表現される
      - h1 と h2 は補助ハッシュ関数
    - 線形探査や 2 次関数探査と違い、初期位置と次に探査する位置までの距離の一方あるいは両方が変化する
- オープンアドレスハッシュ法の解析
  - 一様ハッシュを仮定する
  - 負荷率が α = n / m < 1 であるオープンアドレスハッシュ表において、失敗に終わる探索に必要な探査数の期待値は `1/(1 - α)` 以下である
  - 成功に至る探索に必要な探査回数の期待値は高々 `1/α * ln 1/(1-α)`
- 完全ハッシュ法(perfect hashing)
  - 探索に必要な記憶アクセス回数が最悪時に Ｏ(1) であるハッシュ法を完全ハッシュ法と呼ぶ
  - 完全ハッシュ法を構成するために、各段階で万能ハッシュを用いる 2 段階ハッシュ法を用いる
    - 第 1 段階はチェイン法を用いるハッシュ法と基本的に同じで、万能ハッシュ関数の族から注意深く選んだハッシュ関数を用いて n 個のキーを m 個の枠にハッシュする
    - しかし、枠 j にハッシュされるキーの連結リストを作る代わりに、ハッシュ関数 hj を用いる小さな副ハッシュ表(secondary hash table) Sj を用いるところが違う

## 12: 2 分探索木

- 2 分探索木
  - 2 分探索木では、キーを以下の 2 分探索木条件(binary-search-tree property)を満たすように格納する
    - x を 2 分探索木の節点とする。y が x の左部分木の節点ならば y.key <= x.key、右部分木の節点ならば y.key >= x.key を満たす
- ソートされた順序での印刷
  - キー集合が 2 分探索木条件を満たすように格納されている時、中間順木巡回(inorder tree walk)と呼ぶ簡単な再帰的アルゴリズムを用いて、すべてのキーをソートされた順序で印刷できる
  - x を n 個の節点を持つ部分木の根とする。INORDER-TREE-WALK(x) の実行に `θ(n)` 時間かかる

```
INORDER-TREE-WALK(x)
if x != NIL
  INORDER-TREE-WALK(x.left)
  x.key を印刷する
  INORDER-TREE-WALK(x.right)
```

- 探索
  - 木の根を指すポインタとキー k が与えられた時、TREE-SEARCH はキー k を持つ節点が存在すれば、その節点を指すポインタを返し、存在しなければ NIL を返す
  - 木の高さが h の時、TREE-SEARCH の実行時間は `Ｏ(h)` である
  - 再帰を while の形に開くことで、同じ手続きを反復形に書き換えることができる

```
TREE-SEARCH(x, k)
if x == NIL または k == x.key
  return x
if k < x.key
  return TREE-SEARCH(x.left, k)
else
  return TREE-SEARCH(x.right, k)
```

```
ITERATIVE-TREE-SEARCH(x, k)
while x != NIL かつ k != x.key
  if k < x.key
    x = x.left
  else
    x = x.right
return x
```

- 最小値と最大値
  - 木の高さが h の時、実行時間は `Ｏ(h)` である

```
TREE-MINIMUM
while x.left != NIL
  x = x.left
return x

TREE-MAXIMUM
while x.right != NIL
  x = x.right
return x
```

- 次節点と先行節点
  - 木の高さが h の時、実行時間は `Ｏ(h)` である
  - 以下は次節点の求め方、先行節点はこの対称となる

```
TREE-SUCCESSOR(x)
if x.right != NIL
  return TREE-MINIMUM(x.right)
y = x.p
while y != NIL かつ x == y.right
  x = y
  y = y.p
return y
```

- 挿入
  - TREE-INSERT では、ポインタ x は木の根から開始し、単純パスに沿って木を下る
  - 最初の while 文で木を下り、z.key と y.key の比較結果によって、右側か左側か決める

```
TREE-INSERT(T, z)
y = NIL
x = T.root
while x != NIL
  y = x
  if z.key < x.key
    x = x.left
  else
    x = x.right
z.p = y
if y == NIL
  T.root = z // 木 T が空の場合
else if z.key < y.key
  y.left = z
else
  y.right = z
```

- 削除
  - 削除するための方針全体は以下の 3 つからなる
    - z が子を持たない場合は簡単。z の親の子を z から NIL に置き換えて z を削除する
    - z がちょうど 1 つの子を持つ場合は、z の親の子を z から z の子に変更することで、z の子を z の場所に持ち上げる
    - z が 2 つの子を持つ場合は、z の右の部分木の中から z の次節点 y を発見し、y を z の場所に置く
      - y が z の右の子である場合、y は右の子には触れず、z と y を置き換える
      - y は z の右部分木の中にあるが、z の右の子ではない場合、最初に y と y の右の子を置き換え、次に z と y を置き換える
  - 2 分探索木の中を部分木を移動させる必要があるため、ある節点の子である部分木を別の部分木に置き換えるサブルーチン TRANSPARENT を定義する
    - 節点 u を根とする部分木を節点 v を根とする部分木と置き換えると、u の親が v の親になり、u の親が v を適切な子として持つことになる
  - 木の高さが h の時、実行時間は `Ｏ(h)` である

```
TRANSPARENT(T, u, v)
if u.p == NIL
  T.root = v
else if u == u.p.left
  u.p.left = v
else
  u.p.right = v
if v != NIL
  v.p = u.p
```

```
TREE-DELETE(T, z)
if z.left == NIL
  TRANSPARENT(T, z, z.right)
else if z.right == NIL
  TRANSPARENT(T, z, z.left)
else
  y = TREE-MINIMUM(z.right)
  if y.p != z
    TRANSPARENT(T, y, y.right)
    y.right = z.right
    y.right.p = y
  TRANSPARENT(T, z, y)
  y.left = z.left
  y.left.p = y
```

- ランダムに構成した 2 分探索木(randomly built binary search tree)
  - n 個の異なるキー上のランダムに構成した 2 分探索木を空の木に n 個のキーをランダムな順序で挿入して作る 2 分探索木と定義する
  - ただし、入力される可能性のある n! 種類の順列はどれも等確率で出現すると仮定する
  - この場合、高さの期待値は `Ｏ(lg n)` である

## 13: 2 色木

- 2 色木(red-black tree)
  - 2 色木は各節点がその色を記憶するために 1 ビットの余分な領域を持つ 2 分探索木である。節点の色は RED または BLACK である
  - 2 色木では、根と葉を結ぶ任意の単純道上の節点の配色を節約することで、ある道の長さが他のある道の長さの 2 倍を超えることがないことを保証する
    - すなわち、木はおおよそ平衡している(balanced)
  - 木の各節点は属性 color, key, left, right, p を持つ
  - 次の 5 つの 2 色条件(red-black properties)をすべて満たす 2 分探索木を 2 色木と呼ぶ
    - 各節点は赤または黒のどちらかである
    - 根は黒である
    - すべての葉(NIL)は黒である
    - ある節点が赤ならば、その子は共に黒である
    - 各節点について、その節点とその子孫の任意の葉を結ぶ単純道は同数の黒節点を含む
  - 黒高さ(black-height)
    - ある節点 x から葉までの(x 自身は含まない)単純道上の黒節点の数を x の黒高さと呼び、bh(x) で表す
    - 2 色木の黒高さをその根の黒高さと定義する
  - 2 色木はいい探索木
    - n 個の内部節点を持つ 2 色木の高さは高々 `2 * lg (n + 1)` である
    - 動的集合 SEARCH, MINIMUM, MAXIMUM, SUCCESSOR, PREDECESSOR は高さ h の探索木上で Ｏ(h) 時間で走るように実現できる。この補題から、n 個の節点を持つ任意の 2 色木は高さが Ｏ(lg n) の探索木だから、これらの操作を 2 色木上で `Ｏ(lg n)` 時間で走るように実現できる
- 回転(rotation)
  - LEFT-ROTATE と RIGHT-ROTATE の 2 種類がある
  - 両方とも `Ｏ(1)` 時間で走る

```
LEFT-ROTATE(T, x)
y = x.right       // y を x の右の子とする
x.right = y.left  // y の左部分木を x の右部分木にする
if y.left != T.nil
  y.left.p = x
y.p = x.p         // x の親を y にする
if x.p == T.nil
  T.root = y
else if x == x.p.left
  x.p.left = y
else
  x.p.right = y
y.left = x         // x を y の右の子にする
x.p = y
```

- 挿入
  - n 個の節点からなる 2 色木へある節点を Ｏ(lg n) 時間で挿入できる
  - TREE-INSERT を用い、2 色木 T を通常の 2 分木探索とみなして節点 z を木 T に挿入し、z を赤に彩色する。次に 2 色条件を保存するために、補助手続き RB-INSERT-FIXUP を呼び出して、節点の再彩色と回転を行う
  - TREE-INSERT と RB-INSERT との違い
    - TREE-INSERT に現れる NIL はすべて T.nil で置き換える
    - 正しい木構造を維持するために、RB-INSERT では T.nil を z.left と z.right に代入する
    - z を赤に彩色する
    - RB-INSERT-FIXUP(T, z) を呼び出して 2 色条件を回復する
  - RB-INSERT の実行に `Ｏ(lg n)` 時間かかる
    - 場合 1 が生起した時だけ while 文が繰り返され、最大繰り返し回数は Ｏ(lg n) である。場合 2 または 3 が生起すると while 文が停止する

```
RB-INSERT(T, z)
y = T.nil
x = T.root
while x != T.nil
  y = x
  if z.key < x.key
    x = x.left
  else
    x = x.right
z.p = y
if y == T.nil
  T.root = z
else if z.key < y.key
  y.left = z
else
  y.right = z
z.left = T.nil
z.right = T.nil
z.color = RED
RB-INSERT-FIXUP(T, z)

RB-INSERT-FIXUP(T, z)
while z.p.color == RED
  if z.p == z.p.p.left
    y = z.p.p.right
    if y.color == RED        // 場合1: z の叔父 y は赤
      z.p.color = BLACK
      y.color = BLACK
      z.p.p.color = RED
      z = z.p.p
    else
      if z == z.p.right      // 場合2: z の叔父 y は黒であり、z は右の子
        z = z.p
        LEFT-ROTATE(T, z)
      z.p.color = BLACK      // 場合3: z の叔父 y は黒であり、z は左の子
      z.p.p.color = RED
      RIGHT-ROTATE(T, z.p.p)
  else
    then 節と同様。ただし「right」と「left」を交換する
T.root.color = BLACK
```

- 削除
  - 他の基本操作と同様、n 個の節点を持つ 2 色木上からの節点の削除も `Ｏ(lg n)` 時間で実現できる
  - TRANSPARENT を 2 色木に適用できるよう修正する
    - RB-TRANSPARENT と TRANSPARENT との違い
      - NIL ではなく番兵 T.nil を参照する
      - v.p に対する代入を無条件で実行する

```
RB-TRANSPARENT(T, u, v)
if u.p == T.nil
  T.root = v
else if u == u.p.left
  u.p.left = v
else
  u.p.right = v
v.p = u.p
```

```
RB-DELETE(T, z)
y-original-color = y.color
if z.left == T.nil
  x = z.right
  RB-TRANSPARENT(T, z, z.right)
else if z.right == T.nil
  x = z.left
  RB-TRANSPARENT(T, z, z.left)
else
  y = TREE-MINIMUM(z.right)
  y-original-color = y.color
  x = y.right
  if y.p == z
    x.p = y
  else
    RB-TRANSPARENT(T, y, y.right)
    y.right = z.right
    y.right.p = y
  RB-TRANSPARENT(T, z, y)
  y.left = z.left
  y.left.p = y
  y.color = z.color
if y-original-color == BLACK
  RB-DELETE-FIXUP(T, x)
```

- RB-DELETE-FIXUP
  - 手続き RB-DELETE-FIXUP が性質 1、2、4 を回復することを証明する
  - while 文では、以下の 3 つの条件のいずれかが成立するまで木の中で特黒を持ち上げる
    - x が赤黒節点を指す。この場合は最後に x を(普通の)黒に彩色する
    - x が根を指す。この場合には特黒を単に取り除く
    - 適切な回転と再彩色を行って、ループを停止する

```
RB-DELETE-FIXUP(T, x)
while x != T.root かつ x.color == BLACK
  if x == x.p.left
    w = x.p.right
    if w.color == RED                                     // 場合 1: x の兄弟 w が赤の場合
      w.color = BLACK
      x.p.color = RED
      LEFT-ROTATE(T, x.p)
      w = x.p.right
    if w.left.color == BLACK かつ w.right.color == BLACK  // 場合 2: x の兄弟 w も w の両方の子もすべて黒の場合
      w.color = RED
      x = x.p
    else
      if w.right.color == BLACK                           // 場合 3: x の兄弟 w は黒、w の左の子は赤、w の右の子は黒の場合
        w.left.color = BLACK
        w.color = RED
        RIGHT-ROTATE(T, w)
        w = x.p.right
      w.color = x.p.color                                 // 場合 4: x の兄弟 w が黒で w の右の子が赤の場合
      x.p.color = BLACK
      w.right.color = BLACK
      LEFT-ROTATE(T, x.p)
      x = T.root
    else
      then 節と同様。ただし「right」と「left」を交換する
x.color = BLACK
```

## 14: データ構造の補強

- ほとんどの場合、追加情報を格納して教科書的なデータ構造を補強することで、必要とする応用に即したデータ構造上の新しい操作がプログラムできる
- 順序統計量木(order-static tree)
  - 各節点に追加情報を格納した 2 色木
  - 動的集合の任意の順序統計量を Ｏ(lg n) 時間で決定できるようにする
    - ようは i 番目に小さいキーを持つ要素を Ｏ(lg n) 時間で決定できる
  - 通常の 2 色木の節点 x が保持する x.key, x.color, x.p, x.left, x.right の他に x.size を持ち、x を根とする部分木に属する(x 自身を含む)内部接点数、すなわちその部分木のサイズを格納する
- 与えられた順位を持つ要素の検索
  - x を根とする部分木の中で i 番目に小さいキーを持つ節点へのポインタを返す

```
OS-SELECT(x, i)
r = x.left.size + 1
if i == r
  return x
else if i < r
  return OS-SELECT(x.left, i)
else
  return OS-SELECT(x.right, i - r)
```

- 与えられた要素の順位の決定
  - 順序統計量木 T に属する節点 x へのポインタが与えられた時、線形順序における x の順位を返す
  - 最悪計算時間は木の高さに比例するので、接点数が n の場合 Ｏ(lg n) 時間

```
OS-RANK(T, x)
r = x.left.size + 1
y = x
while y != T.root
  if y == y.p.right
    r = r + y.p.left.size + 1
  y = y.p
return r
```

- 部分木のサイズの維持
  - 各節点が size 属性を維持することで、手続き OS-SELECT と OS-RANK は順序統計的情報を高速に計算できる
  - LEFT-ROTATE(T, x) に次の 2 行を付け加えることでサイズが維持される

```
y.size = x.size
x.size = x.left.size + x.right.size + 1
```

- データ構造補強法
  - 4 つの段階
    - 基礎となるデータ構造を選ぶ
    - 基礎データ構造の中で新たに管理する情報を決定する
    - 追加された情報が基礎データ構造上の基本変更操作によって維持できることを検証する
    - 新しい操作を開発する
  - ただし、この順番に盲目的に従うべきではない
- 閉区間(closed interval)
  - 閉区間は t1 <= t2 を満たす実数の順序対 [t1, t2] である。区間 [t1, t2] は集合 {t ∈ R: t1 <= t <= t2} を表す
  - 開区間(open interval)と半開区間(half-open interval)はそれぞれ閉区間から両端点と片端点を除去したもの
- 区間木(interval tree)
  - 区間 [t1, t2] を属性 i.low = t1 (下端点: low endpoint) と i.high = t2 (上端点: high endpoint) を持つオブジェクト i として表現できる
  - 任意の 2 つの区間 i と i' の関係は区間 3 分律(interval trichotomy)を満たす
    - i と i' は重なっている
    - i は i' の左にある(すなわち i.high < i'.low)
    - i は i' の右にある(すなわち i'.high < i.low)
  - 各要素 x がある区間 x.int を格納している時、これらの要素から構成される動的集合を維持する 2 色木を区間木と呼ぶ
  - 区間木では以下の操作が利用できる
    - INTERVAL-INSERT(T, x) は要素 x の int 属性がある区間を含むと仮定する時、x を区間木 T に挿入する
    - INTERVAL-DELETE(T, x) は区間木 T から要素 x を削除する
    - INTERVAL-SEARCH(T, i) は、区間 i と重なる区間 x.int を持つ要素 x が区間木 T の中にあれば x へのポインタを返し、そのような要素がない時には番兵 T.nil を返す

## 15: 動的計画法

- 動的計画法(dynamic programming)
  - 動的計画法は、分割統治法と同様、部分問題の解を統合することによって問題を解く方法(ここで「programming」は表を用いる方法を意味し、コンピュータプログラムを書くことではない)
- 動的計画アルゴリズムは以下の 4 つの段階を経て開発される
  - 最適解の構造を特徴づける
  - 最適解の値を再帰的に定義する
  - (多くの場合にはボトムアップ方式で)最適解の値を計算する
  - 計算された情報から 1 つの最適解を構成する

### 15.1: ロッド切出し

- ロッド切出し問題(rod-cutting problem)
  - 長さ n インチの 1 本の金属棒と、i = 1, 2, ..., n に対する価格 pi の表が与えられた時、この金属棒から切り出される金属棒を、価格表に従って販売して得る事ができる収入の最大値を計算する問題
- 部分構造最適性(optimal substructure)
  - 問題の最適解は、関連する部分問題の最適解を含んでいて、これらの部分問題は独立に解ける
- 再帰的トップダウン型実現
  - 以下のように書く
  - しかし、これだと実行に n の指数時間がかかる

```
CUT-ROD(p, n)
if n == 0
  return 0
q = -∞
for i = 1 to n
  q = max(q, p[i] + CUT-ROD(p, n - i))
return q
```

- 動的計画法を用いて CUT-ROD を効率的なアルゴリズムに変換する方法
  - 各部分問題を解いた時にその解を保存することによって、各部分問題を 1 度だけ解けば十分なようにする
  - 「履歴管理を用いるトップダウン方式」と「ボトムアップ方式」の 2 つの等価な実現方法がある
- 履歴管理を用いるトップダウン方式(top-down with memoization)
  - 手続きを再帰的に記述するが、各部分問題の解を保存するように変更を加える

```
MEMOIZED-CUT-ROD(p, n)
r[0..n] を新しい配列とする
for i = 0 to n
  r[i] = -∞
return MEMOIZED-CUT-ROD-AUX(p, n, r)

MEMOIZED-CUT-ROD-AUX(p, n, r)
if r[n] >= 0
  return r[n]
if n == 0
  q == 0
else
  q = -∞
  for i = 1 to n
    q = max(q, p[i] + MEMOIZED-CUT-ROD-AUX(p, n - i, r))
r[n] = q
return q
```

- ボトムアップ方式(bottom-up method)
  - ボトムアップ方式ではすべての部分問題をサイズの昇順でソートし、サイズの小さいものから順に解いていく

```
BOTTOM-UP-CUT-ROD(p, n)
f[0..n] を新しい配列とする
r[0] = 0
for j = 1 to n
  q = -∞
  for i = 1 to j
    q = max(q, p[i] + r[j - i])
  r[j] = q
return r[n]
```

- 解の再構成
  - 各部分問題に対して、その最適値だけでなく、最適値を達成する選択を保存できるようにする
  - 以下は配列 s を計算し、長さ n の金属棒に対する最適な切り出し方を保存する

```
EXTENDED-BOTTOM-UP-CUT-ROD(p, n)
r[0..n] と s[1..n] を新しい配列とする
r[0] = 0
for j = 1 to n
  q = -∞
  for i = 1 to j
    if q < p[i] + r[j - 1]
      q = p[i] + r[j - 1]
      s[j] = i
  r[j] = q
return r and s
```

### 15.2: 連鎖行列積

- 連鎖行列積問題(matrix-chain multiplication problem)
  - n 個の行列の連鎖 <A1, A2, ..., An> が与えられた時、スカラー乗算回数を最小化するように積 A1A2...An を完全に括弧付けする問題
- 動的計画法の適用
  - 第 1 段階: 最適括弧付けの構造
    - i <= j とする時、AiAi+1...Aj の最適括弧付けは、ある整数 k(i <= k <= j) について Ai...Ak と Ak+1...Aj を計算し、最も良い k を探すと言い換えられる
  - 第 2 段階: 再帰的な解
    - m[i, j] を行列 Ai..j の計算に必要な最小現必要なスカラー乗算の回数とする
    - `m[i, j] = m[i, k] + m[k + 1, j] + pi-1 * pk * pj` が成立する
  - 第 3 段階: 最適コストの計算
    - ボトムアップ方式で計算するためにサイズの小さいものから順々に計算していくようにする
    - 以下の MATRIX-CHAIN-ORDER のようになる
  - 第 4 段階: 最適解の構成
    - 配列 s を使って、最適な括弧付けを出力できるようにする
    - 以下の PRINT-OPTIMAL-PARENS のようになる

```
MATRIX-CHAIN-ORDER(p)
n = p.length - 1
m[1..n, 1..n] と s[1..n-1, 2..n] を新しい表とする
for i = 1 to n
  m[i, i] = 0
for l = 2 to n       // l は連鎖の長さ
  for i = 1 to n - l + 1
    j = i + l - 1
    m[i, j] = ∞
    for k = i to j - 1
      q = m[i, k] + m[k + 1, j] + pi-1 * pk * pj
      if q < m[i, j]
        m[i, j] = q
        s[i, j] = k
return m と s

PRINT-OPTIMAL-PARENS(s, i, j)
if i == j
  print "A"i
else
  print "("
  PRINT-OPTIMAL-PARENS(s, i, s[i, j])
  PRINT-OPTIMAL-PARENS(s, s[i, j + 1], j)
  print ")"
```

### 15.3: 動的計画法の基本要素

- 動的計画法が適用できるために最適化問題が持たなければならない重要な 2 つの特徴がある
  - 部分構造最適性と部分問題重複性
- 部分構造最適性(optimal substructure)
  - 問題の最適解が、その内部に、その部分問題に対する最適解を含む時、この問題は部分構造最適性を持つという
- 動的計画アルゴリズムの実行時間はおおよそ 2 つの要因、部分問題の総数と各部分問題で考慮しなければならない候補数の積に依存する
- 部分問題の独立性
  - ある部分問題の解が別の部分問題の解に影響を与えない時、独立である
- 部分問題重複性(overlapping subproblems)
  - 再帰アルゴリズムが同じ問題を繰り返し訪れる時、その最適化問題は部分問題重複性を持つという
- ボトムアップ型の動的計画法と、履歴管理を用いるトップダウン型アルゴリズム
  - すべての部分問題を 1 度は解かなければならないなら、ボトムアップ型の動的計画法の性能は、履歴管理を用いるトップダウン型アルゴリズムより定数倍優れている
  - 一方、部分問題空間に属するすべての部分問題を解く必要がないなら、履歴管理型の解は、どうしても解かなければならない部分問題だけを解くという長所がある

### 15.4: 最長共通部分列

- 部分列(sequence)
  - 列 X = <x1, x2, ..., xm> が与えられた時、ある列 Z = <z1, z2, ..., zk> が X の部分列であるとは、真に増加する X の添字の列 <i1, i2, ..., ik> が存在して、すべての j = 1, 2, ..., k に対して、xij = zj を満たす時に言う
- 共通部分列(common sequence)
  - X = <A, B, C, B, D, A, B>, Y = <B, D, C, A, B, A> とすると、列 <B, C, A> は共通部分列である
- 最長共通部分列(LCS, longest common sequence)
  - 与えられた 2 つの列 X と Y の最長共通部分列を求める問題
- LCS の部分構造最適性
  - X = <x1, x2, ..., xm> と Y = <y1, y2, ..., yn> を列 Z = <z1, z2, ..., zk> を X と Y の任意の LCS とする
  - 以下の 3 つのどれかとなる
    - xm = yn ならば zk = xm = yn であり、Zk-1 は Xm-1 と Yn-1 の LCS である
    - xm != yn の時 zk != xm ならば、Z は Xm-1 と Yn の LCS である
    - xm != yn の時 zk != yn ならば、Z は Xm と Yn-1 の LCS である
- 漸化式で表すと以下のようになる(列 Xi と Yj の LCS の長さを c[i, j] と定義する)
  - c[i, j] = 0 (i = 0 または j = 0 の時)
  - c[i, j] = c[i - 1, j - 1] + 1 (i, j > 0 かつ xi = yj の時)
  - c[i, j] = max(c[i, j - 1], c[i - 1, j]) (i, j > 0 かつ xi != yj の時)

```
LCS-LENGTH(X, Y)
m = X.length
n = Y.length
b[1..m, 1..n] と c[0..m, 0..n] を新しい表とする
for i = 1 to m
  c[i, 0] = 0
for j = 0 to n
  c[0, j] = 0
for i = 1 to n
  for j = 1 to n
    if xi == yj
      c[i, j] = c[i - 1, j - 1] + 1
      b[i, j] = "↖"
    elseif c[i - 1, j] >= c[i, j - 1]
      c[i, j] = c[i - 1, j]
      b[i, j] = "↑"
    else
      c[i, j] = c[i, j - 1]
      b[i, j] = "←"
return c と b
```

### 15.5: 最適 2 分探索木

- 最適 2 分探索木(optimal binary search tree)
  - 与えられた確率の集合に対して、探索コストの期待値を最小化する 2 分探索木

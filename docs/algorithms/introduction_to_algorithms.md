# 概要

## 本

- [アルゴリズムイントロダクション 第3版 総合版 (世界標準MIT教科書) | T. コルメン, R. リベスト, C. シュタイン, C. ライザーソン, Thomas H. Cormen, Clifford Stein, Ronald L. Rivest, Charles E. Leiserson, 浅野 哲夫, 岩野 和生, 梅尾 博司, 山下 雅史, 和田 幸一](https://amzn.to/2F8X8M4)

## 参考

- [Introduction to Algorithms, Third Edition | The MIT Press](https://mitpress.mit.edu/books/introduction-algorithms-third-edition)

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
      - C11 = A11 * B11 + A12 * B21
      - C12 = A11 * B12 + A12 * B22
      - C21 = A21 * B11 + A22 * B21
      - C22 = A21 * B12 + A22 * B22
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
    - ある定数 ε > 0 に対して f(n) = Ω(n^logb a+ε) であり、しかもある定数 c < 1 と十分大きな全ての n に対して a * f(n/b) <= c * f(n) ならば、`T(n) = θ(f(n))`

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

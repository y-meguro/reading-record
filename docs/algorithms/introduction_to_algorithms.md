# 概要

## 本

- [アルゴリズムイントロダクション 第3版 総合版 (世界標準MIT教科書) | T. コルメン, R. リベスト, C. シュタイン, C. ライザーソン, Thomas H. Cormen, Clifford Stein, Ronald L. Rivest, Charles E. Leiserson, 浅野 哲夫, 岩野 和生, 梅尾 博司, 山下 雅史, 和田 幸一](https://amzn.to/2F8X8M4)

## 参考

- [Introduction to Algorithms, Third Edition | The MIT Press](https://mitpress.mit.edu/books/introduction-algorithms-third-edition)
- [演習問題の解答](http://mitp-content-server.mit.edu:18180/books/content/sectbyfn?collid=books_pres_0&id=8030&fn=Intro_to_Algo_Selected_Solutions.pdf)
  - 公式の解答。一部の問題のみ、解答が記載されている

## かかった時間

- x 時間

## 読む前の状態

- xxx

## 進め方

- 演習問題は解いていない

## 感想

- xxx

# 読書メモ

# Ⅰ: 基礎

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

# Ⅱ: ソートと順序統計量

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

- 乱択版クイックソート
  - 5.3 節では入力を明示的に置換することでアルゴリズムを乱択化した。しかし、無作為抽出(random sampling) と呼ばれる別の乱択化手法を用いると解析が簡単になる
    - これは A[r] を常にピボットとする代わりに部分配列 A[p..r] から要素を無作為に抽出し、それをピボットとして用いる
  - 乱択版クイックソートは、PARTITION の代わりに RANDOMIZED-PARTITION を呼び出す

```
RANDOMIZED-PARTITION(A, p, r)
i = RANDOM(p, r)
A[r] と A[i] を交換する
return PARTITION(A, p, r)

RANDOMIZED-QUICKSORT(A, p, r)
if p < r
  q = RANDOMIZED-PARTITION(A, p, r)
  RANDOMIZED-QUICKSORT(A, p, q - 1)
  RANDOMIZED-QUICKSORT(A, q + 1, r)
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
  - 任意の比較ソートアルゴリズムは最悪時に `Ω(n * lg n)` 回の比較が必要(定理 8.1)
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
  - 計数ソートはこの安全性を保証する
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

- 線形期待時間選択アルゴリズム
  - 期待実行時間 `θ(n)` で i 番目に小さい要素を返すアルゴリズムを検討する。RANDOMIZED-SELECT は上記の条件を満たす
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

- 線形最悪時間選択アルゴリズム
  - 最悪実行時間が `Ｏ(n)` の選択アルゴリズムを検討する。SELECT は上記の条件を満たす
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

# Ⅲ: データ構造

- 数学の集合は変化しないが、アルゴリズムが扱う集合は成長したり、縮んだり、時間とともに変化する。この集合を動的であるという
- 10 章ではスタック、キュー、連結リスト、根付き木のような単純なデータ構造を扱う上でキーとなることを説明する
- 11 章ではハッシュ表、12 章では 2 分探索木を扱う
- 13 章で 2 分探索木の改良である 2 色木、14 章では 2 色木を補強する方法を示す

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

- 連結リストの先頭と末尾の境界条件が無視できるならば、LIST-DELETE のコードはもっと簡単になる

```
LIST-DELETE'(L, x)
x.prev.next = x.next
x.next.prev = x.prev
```

- 番兵(sentinel)
  - 番兵は境界条件を簡略化するためのダミーオブジェクト
  - 通常の双方向リストを番兵を持つ双方向循環リストに変える。リストの先頭と末尾の間に番兵 L.nil が置かれ、属性 L.nil.next がリストの先頭を、L.nil.prev がリストの末尾を指す
- ポインタやオブジェクトが提供されていない言語で、連結データ構造を実現する 2 つの方法
  - オブジェクトの多重配列表現
  - オブジェクトの単一配列表現
- オブジェクトの割付けと解放
  - あるキーを双方向連結リストが表現する動的集合に挿入するには、連結リスト表現の中から現在使用されていないオブジェクトを見つけ、それにポインタを割り付ける必要がある
  - この未使用オブジェクトを未使用リストと呼ぶ一方向リストとして管理する
- 未使用リスト(free list)
  - 多重配列表現が使用する配列の長さを m とし、ある時点で動的集合が n <= m 個の要素を含むとする
  - n 個のオブジェクトが動的集合の要素を表現し、残りの m - n 個のオブジェクトは未使用(free)である
  - 未使用リストは配列 next だけを使用し、そこに未使用リストの next ポインタを格納する
- オブジェクトの割付けと解放は以下のように行う

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

- 上で紹介したリスト表現方法は、任意の均質なデータ構造に拡張できる。ここでは、その中から根付き木ポインタに基づくデータ構造によって表現する問題を考察する
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
  - 出現する可能性のあるキーの全集合 U がそれほど大きくない場合に、直接アドレス指定法はうまく働く
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
    - 悪意を持った敵対者がハッシュされるキーを選択するならば、同じ枠にハッシュされる n 個のキーを選択する。この場合、ハッシュ表の最悪検索時間は `θ(n)` になる
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
    - キー k が与えられた時、最初に T[h'(k)] を探索し、次に T[h'(k) + 1] と順々に探査していく。T[m - 1] までいったら T[0], T[1] の順で T[h'(k) - 1] まで探査する
    - 主クラスタ化(primary clustering)の問題
      - 長い区間の枠がすべて使用中になり、平均探査時間が悪化する問題
      - 連続する i 個の使用中の枠に続く空の枠が次に使用される確率が (i + 1) / m であることがクラスタが現れる理由
  - 2 次関数探査法(quadratic probing)
    - `h(k, i) = (h'(k) + c1 * i + c2 * i^2) mod m` で表現される
    - 同じ初期探査位置を持つ 2 つのキーは同じ探査列を持ってしまう
      - この性質から、副クラスタ化(secondary clustering)と呼ばれる、望ましくない状況が発生する。線形探査の場合と同様、最初に探査される場所が探査列を決定するので、m 個の異なる探査列しか利用できない
  - ダブルハッシュ法
    - オープンアドレス指定法に利用できる最良の方法の 1 つがダブルハッシュ法
    - `h(k, i) = (h1(k) + i * h2(k)) mod m` で表現される
      - h1 と h2 は補助ハッシュ関数
    - 線形探査や 2 次関数探査と違い、初期位置と次に探査する位置までの距離の一方あるいは両方が変化する
- オープンアドレスハッシュ法の解析
  - 一様ハッシュを仮定する
  - 負荷率が α = n / m < 1 であるオープンアドレスハッシュ表において、失敗に終わる探索に必要な探査数の期待値は `1/(1 - α)` 以下である
  - 成功に至る探索に必要な探査回数の期待値は高々 `1/α * ln 1/(1-α)`
- 平均性能が優れているという理由でハッシュ法が利用されることが多いが、キー集合が静的な場合(一度表に蓄えられた後は永遠に変化しない場合)、最悪時にも優れた性能を示す
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
      - y が z の右の子である場合、y の右の子には触れず、z と y を置き換える
      - y は z の右部分木の中にあるが、z の右の子ではない場合、最初に y と y の右の子を置き換え、次に z と y を置き換える
  - 2 分探索木の中を部分木を移動させる必要があるため、ある節点の子である部分木を別の部分木に置き換えるサブルーチン TRANSPARENT を定義する
    - 節点 u を根とする部分木を節点 v を根とする部分木と置き換えると、u の親が v の親になり、u の親が v を適切な子として持つことになる
  - 木の高さが h の時、実行時間は `Ｏ(h)` である

```
TRANSPLANT(T, u, v)
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
  TRANSPLANT(T, z, z.right)
else if z.right == NIL
  TRANSPLANT(T, z, z.left)
else
  y = TREE-MINIMUM(z.right)
  if y.p != z
    TRANSPLANT(T, y, y.right)
    y.right = z.right
    y.right.p = y
  TRANSPLANT(T, z, y)
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
  - 区間木は 2 色木を基礎データ構造として、4 段階で設計できる

# Ⅳ: 高度な設計と解析の手法

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

## 16: 貪欲アルゴリズム

- 貪欲アルゴリズム(greedy algorithm)
  - 最適化問題を解く場合、動的計画法を用いなくても、もっと簡単で効率の良いアルゴリズムで十分な場合も多い
  - 貪欲アルゴリズムは、常にその時点で最適と思われる選択を行う

### 16.1: 活動選択問題

- 活動選択問題(activity-selection problem)
  - 共有資源の排他的利用を要求して競合する複数の活動の中から両立可能なものを選択肢、選択された活動数を最大化する問題
  - 各活動は開始時刻と終了時刻を持ち、時間が重ならない場合は両立可能とする
  - この問題は部分構造最適性を持つので、動的計画法で解きたくなるかもしれないが、もっと効率よく解ける(すべての部分問題を解く必要はない)
- 貪欲な選択
  - 実はこの問題では、たった 1 つの貪欲に選択される候補だけが考慮すればよい
  - つまり、できる限り多くの活動が利用できる資源を残すような選択をすればよい。この時、選択される活動の集合は、常に終了時刻最小の活動を含む
  - 典型的な貪欲アルゴリズムは、選択をする前に部分問題を解くボトムアップ手法ではなく、選択を行った後で部分問題を解くトップダウン的設計に基づいている
- 再帰型貪欲アルゴリズム
  - 以下のようになる

```
// s は各活動の開始時刻の配列、f は各活動の開始時刻の配列、k は最後に選択された活動の index、n は活動のサイズ
// また、各活動はすでに終了時刻でソート済みとする
RECURSIVE-ACTIVITY-SELECTOR(s, f, k, n)
m = k + 1
while m <= n かつ s[m] < f[k]
  m = m + 1
if m <= n
  return {am} ∪ RECURSIVE-ACTIVITY-SELECTOR(s, f, m, n)
else
  return 0
```

- 繰り返し型貪欲アルゴリズム
  - 上記のアルゴリズムを繰り返し型に変換すると以下のようになる
  - 入力となる活動がソート済みと仮定すると、n 個の活動からなる集合を `θ(n)` 時間でスケジュールできる

```
GREEDY-ACTIVITY-SELECTOR(s, f)
n = s.length
A = {a1}
k = 1
for m = 2 to n
  if s[m] >= f[k]
    A = A ∪ {am}
    k = m
return A
```

### 16.2: 貪欲戦略の基本要素

- 貪欲アルゴリズムは常に最適解(heuristic strategy)を導くわけではないが、活動選択問題のように、最適解を得ることができることもある
- 貪欲アルゴリズム開発のための手続き
  - 1: 対象となる最適化問題を、ある選択を行うと、その結果として解かなければならない 1 つの部分問題が残る問題とみなす
  - 2: 元の問題に対して貪欲な選択を行う最適解が常に存在すること、したがって貪欲な選択は常に安全であることを保証する(安全 = その選択が最適解への到達可能性を閉ざさないこと)
  - 3: 貪欲な選択と残された部分問題に対する最適解の組み合わせから、元の問題に対する最適解が構成できることを示すことで、部分構造最適性を論証する
- どの貪欲アルゴリズムの背後にも、より複雑な動的計画法に基づく解法がほとんど常に存在する
  - 貪欲アルゴリズムで解けるか、どう判断すればいいだろうか?
  - 常に適用可能な方法はないが、「貪欲選択性」と「部分構造最適性」が 2 つの重要な要素
- 貪欲選択性(greedy-choice property)
  - 局所最適な(貪欲な)選択から対局最適な解を構成できるという性質
  - 動的計画法でも各段階で選択を行うが、選択は部分問題の解に依存することが多い
- 部分構造最適性(optimal substructure)
  - ある問題の最適解が、その部分問題に対する最適解を含む時、その問題は部分構造最適性を持つという
  - これは貪欲アルゴリズムだけでなく、動的計画法の適用可能性を査定する重要な要素でもある

### 16.3: ハフマン符号

- ハフマン符号はデータを効果的に圧縮する。各文字の出現頻度を与える表を用いて、各文字をビット文字列として最適に表現する方法を見出す
- 可変長符号(variable-length code)
  - 出現頻度の高い文字に短い符号語、出現頻度の低い文字に長い符号語を割り当てる
  - 固定長符号(fixed-length code)に比べて有効にリソースを使える
- 接頭語符号(prefix code)
  - どの符号語も別の符号語の接頭語とはならない符号
  - 複合が簡単なことが接頭語符号の長所
- ハフマン符号の構成
  - 出現頻度最初の 2 つの節点 x と y をキューから削除し、これらを統合したものを表す新たな節点 z で置き換える操作を繰り返す
  - 実行時間は `Ｏ(n * lg n)`
  - 2 分 min ヒープを van Emde Boas 木(20 章参照)に置き換えると、実行時間を `Ｏ(n * lg lg n)` に改善できる

```
HUFFMAN(C)  // C は n 個の文字の集合で、各文字 c ∈ C は出現頻度 c.freq を持つオブジェクト
n = |C|
Q = C  // Q は min 優先度付きキュー
for i = 1 to n - 1
  // 新しい接点 z を割り付ける
  z.left = x = EXTRACT-MIN(Q)
  z.right = y = EXTRACT-MIN(Q)
  z.freq = x.freq + y.freq
  INSERT(Q, z)
return EXTRACT-MIN(Q)  // 構成された木の根を返す
```

### 16.4: マトロイドと貪欲法

- 貪欲法によって最適解が求まる状況の多くが「マトロイド」の理論によって説明できる
- マトロイド(matroid)は以下の条件を満たす順序対 M = (S, F)
  - 1: S はある有限集合
  - 2: F を S の独立部分集合族(independent subsets)と呼ぶ。F は S の空でない部分集合の族であり、B ∈ F かつ A ⊆ B ならば A ∈ F を満たす。この性質を満たす時、F は遺伝的であるという。空集合は必ず F の要素であることに注意せよ
  - 3: A ∈ F、B ∈ F かつ |A| < |B| ならば、ある要素 x ∈ B - A に対して、A ∪ {x} ∈ F である。この時、M は交換性(exchange property)を持つという
- 重み付きマトロイド(weighted matroid)
  - マトロイド M = (S, F) に対して、S の各要素 x に正の重み w(x) を割り当てる重み関数 w が与えられている時、M は重み付けられているという
- 重み付きマトロイド上の貪欲アルゴリズム
  - 貪欲法が最適解を与える問題の多くは重み付きマトロイドの最大重み独立部分集合を求める問題として定式化できる
  - すなわち、与えられた重み付きマトロイド M = (S, F) に対して w(A) を最大化する独立集合 A ∈ F を求める問題

### 16.5: マトロイドとしてのタスクスケジューリング問題

- 飛ばした

## 17: ならし解析

- ならし解析(amortized analysis)
  - ならし解析では、データ構造の操作列の実行にかかった時間を実行したすべての操作でならす(平均化する)
  - ならし解析を用いれば、ある特別な操作が高価であっても、操作列について平均化してみると 1 操作あたりのコストは小さいことを証明できる

### 17.1: 集計法

- 集計法(aggregate method)
  - 集計法では、任意の n に対して、長さ n の操作列の実行は最悪時に合計 T(n) 時間かかることを示す
  - したがって、最悪時の平均コスト(ならしコスト)は 1 操作あたり T(n)/n となる
  - 複数の種類の操作が列に出現する場合でもこのならしコストがすべての操作に適用されることに注意する
- スタック操作の解析
  - 集計法の例として、新しい操作を付加したスタックを考える
  - 10.1 節では、スタックの操作としてそれぞれ `Ｏ(1)` 時間で実行できる PUSH と POP の操作を説明した
  - ここで新しいスタック操作 MULTIPOP(S, k) を導入する
    - スタック S の上から k 個のオブジェクトをポップする。ただし、S が k 個未満のオブジェクトしか含まない時には、これらのオブジェクトをすべてポップする
  - PUSH / POP / MULTIPOP 操作から構成される長さ n の操作列を空のスタックに適用する場合を解析する
    - スタックサイズは n まで大きくなりうるから、この列における 1 回の MULTIPOP 操作の最悪コストは `Ｏ(n)`
    - この MULTIPOP が `Ｏ(n)` 回現れるかもしれないから、長さ n の操作列のコストは `Ｏ(n^2)` となる。この解析は正しいが、改良の余地がある
  - 集計法を用いると、もっと良い上界が証明できる(最悪コストは `Ｏ(n)` であることを証明できる)
    - PUSH 操作が呼び出される回数は最大 n 回、各オブジェクトは高々 1 回しかポップされないため
    - なので、任意の n に対して、PUSH / POP / MULTIPOP 操作をどのように適用しても、全体でかかる時間は `Ｏ(n)` である
    - したがって、1 回の操作の平均コストは`Ｏ(n) / n = Ｏ(1)` である
    - ここで確率に基づく議論をしなかったことに注意。ここでは、「最悪時」の上限 `Ｏ(n)` を証明し、そこからならしコストを導出した

```
MULTIPOP(S, k)
while STACK-EMPTY(S) = FALSE かつ k > 0
  POP(S)
  k = k - 1
```

- 2 進カウンタによる計数
  - 2 つ目の例として、0 から計数を開始する k ビットの 2 進カウンタを実現する問題を考える
  - 各 INCREMENT 操作のコストは反転するビット数に比例する(図 17.2 を見るとわかりやすい)
  - 大雑把に考えると、最悪時には INCREMENT を 1 回実行するのに `θ(k)` 時間かかるので、初期値が 0 のカウンタに対して、INCREMENT 操作を n 回実行すると最悪時には `Ｏ(nk)` 時間かかると思われる
  - しかし、INCREMENT が呼び出されるたびに、すべてのビットが反転するわけではないことに注意して考えると、最悪コストが `Ｏ(n)` とわかる
    - 初期値が 0 のカウンタに対して、INCREMENT 操作を n 回実行すると、A[1] は n/2 回反転する。A[2] は n/4 回反転する
    - 一般に、i = 0, 1, ..., k-1 に対して、ビット A[i] は n / 2^i 回反転するので、0 〜 ∞ まで合計しても、これは 2n より小さい
    - したがって、初期値が 0 のカウンタに対する長さ n の INCREMENT 操作列の実行には、最悪時に `Ｏ(n)` かかる
    - 1 操作あたりのならしコストは `Ｏ(n) / n = Ｏ(1)`

```
INCREMENT(A)
i = 0
while i < A.length かつ A[i] == 1
  A[i] = 0
  i = i + 1
if i < A.length
  A[i] = 1
```

### 17.2: 出納法

- 出納法(accounting method)
  - ならし解析における出納法では、操作ごとに異なる額を課金する。ある操作に課金する額をならしコストと呼ぶ
  - 総ならしコストと総実コストとの差を預金総額と考え、預金総額が負にならないようにする必要がある
- スタック操作の場合
  - 実コストは PUSH: 1 / POP: 1 / MULTIPOP: min(k, s) であった(k は MULTIPOP の引数、s は呼び出された時点でのスタックサイズ)
  - ここで各操作に対し、ならしコスト PUSH: 2 / POP: 0 / MULTIPOP: 0 を割り当てる
  - PUSH 操作に少し多めに課金することにより、POP 操作や MULTIPOP 操作に課金しなくて済むことが保証できる
  - 総ならしコストは総実コストの上界となる。長さ n の PUSH / POP / MULTIPOP 操作から構成される任意の列に対して、総ならしコストが `Ｏ(n)` なので、総実コストも `Ｏ(n)` となる

### 17.3: ポテンシャル法

- ポテンシャル法(potential method)
  - ポテンシャル法では、先行支出を特定のオブジェクトに蓄える「預金」として表現する代わりに、将来の操作の支払いのために放出できる「ポテンシャルエネルギー(またはポテンシャル)」として表現する
    - ポテンシャルはデータ構造全体に対して定義するのであって、データ構造内の特定のオブジェクトに対して定義するのではない
  - 各 i = 1, 2, ..., n に対して i 番目の操作の実コストを ci、i 番目の操作をデータ構造 Di-1 に適用した結果のデータ構造を Di とする
  - ポテンシャル関数 Φ は各データ構造 Di をある実数 Φ(Di) に写す関数である
  - i 番目の操作のならしコスト ci' = ci + Φ(Di) - Φ(Di-1) と定義する

### 17.4: 動的な表

- 表に割り付けた領域が不十分な場合にもっと大きい領域を再割付けし、表から多数のアイテムを削除する時にはもっと小さい領域を再割付けすることを考える
  - この操作のならしコストやポテンシャルを考える
- この動的な表では TABLE-INSERT 操作と TABLE-DELETE 操作が利用できる
- TABLE-INSERT の最悪コストは大雑把に考えると `Ｏ(n^2)` のように思えるが、集計法を用いて考えると n 回の TABLE-INSERT 操作の総コストが 3n で抑えられることがわかり、1 回の操作のならしコストは高々 3 となる
- 出納法で考えるとならしコストが 3 になる理由がわかりやすい
  - 自分自身を表に挿入するために 1 回、表を次に拡大するときに自分自身を移動するために 1 回、あわせて過去に一度移動したことがあるアイテムを再度移動させるために 1 回の計 3 回
- ポテンシャルについては省略

# Ⅴ: 高度なデータ構造

- Ⅲ部で紹介しなかった高度なデータ構造を紹介する
- 18 章では B 木、19 章ではマージ可能ヒープの実現方法を説明する
- 20 章では、キーが限られた範囲の自然数である時に、動的集合操作 SEARCH / INSERT / DELETE / MINIMUM / MAXIMUM / SUCCESSOR / PREDECESSOR が `o(lg n)` 時間で実行できるデータ構造を設計できるか否かを問う
- 21 章では、互いに素な集合のためのデータ構造を紹介する

## 18: B 木

- B 木
  - B 木は磁気ディスクを含む 2 次記憶装置上で効率よく操作できるように設計された平衡探索木である
  - B 木は 13 章で紹介した 2 色木に似ているが、ディスクの I/O 操作数の最小化という点で優っている
  - B 木の節点は 2 色木とちがい、数千もの子を持つことができる。節点数 n の B 木の高さは `Ｏ(lg n)` という点で B 木は 2 色木と似ているが、分岐数、すなわち木の高さを表現する対数の底が 2 色木に比べて十分に大きいから、B 木の高さを 2 色木より十分に低くできる

### 18.1: B 木の定義

- 2 分探索木や 2 色木と同様、キーの「付属情報」はキーと同じ節点に格納されていると仮定して、以下の議論を単純化する
  - B 木のよく知られた改良版である B+ 木(B+tree)はすべての付属情報を葉に格納し、内部節点にはキーと子を指すポインタだけを格納することで内部節点の分岐数を最大化する
    - 参考: [B TreeとB+ Treeの違い - Carpe Diem](https://christina04.hatenablog.com/entry/2017/05/17/190000)
- B 木(B-tree)は以下の条件を満足する根付き木であり、その根を T.root で示す
  - 1: 各節点 x は以下の属性を持つ
    - 節点 x に現在格納されているキー数 x.n
    - 格納されている x.n 個のキー自身 x.key1, x.key2, ..., x.key x.n。キーは昇順に格納されており、x.key1 <= x.key2 <= ... <= x.key x.n
    - x が葉であれば TRUE、内部節点であれば FALSE を取るブール変数 x.leaf
  - 2: 各内部節点 x はその子を指す x.n+1 個のポインタ x.c1, x.c2, ..., x.c x.n+1 を持つ。葉はその子を持たないので、その ci 属性は未定義である
  - 3: x.n 個のキー x.keyi は各部分木に格納されているキーの存在範囲を分離する。すなわち、x.ci を根とする部分木に格納されている任意のキーを ki とすると k1 <= x.key1 <= k2 <= x.key2 <= ... <= x.key x.n <= k x.n+1
  - 4: すべての葉は同じ深さを持ち、その深さは木の高さ h である
  - 5: 1 つの節点が格納できるキー数に上限と下限が存在す。これらの限界は B 木の最小次数と呼ぶ。ある固定された整数 t >= 2 を用いて表現する
    - 根を除くすべての節点は少なくとも t - 1 個のキーを持つ。したがって、根を除くすべての内部節点は少なくとも t 個の子を持つ。木が空でなければ根は少なくとも 1 個のキーを持つ
    - どの節点も最大 2t - 1 個のキーを持てる。したがって、内部節点は最大 2t 個の子を持つことができる。ちょうど 2t - 1 個のキーを持つ内部節点は飽和しているという

### 18.2: B 木上の基本操作

- B 木の探索

```
B-TREE-SEARCH(x, k)  // ある部分木の根 x と探索対象であるキー k を与える
i = 1
while i <= x.n かつ k > x.keyi
  i = i + 1
if i <= x.n かつ k == x.keyi
  return (x, i)
elseif x.leaf
  return NIL
else
  DISK-READ(x.ci)
  return B-TREE-SEARCH(x.ci, k)
```

- 空の B 木の作成
  - B 木 T を作るには、まず B-TREE-CREATE を用いて空の根を生成し、次に B-TREE-INSERT を呼び出して新しいキーをそれに追加する
  - B-TREE-CREATE は `Ｏ(1)` 回のディスクアクセスと `Ｏ(1)` の CPU 時間が必要である

```
B-TREE-CREATE(T)
x = ALLOCATE-NODE()
x.leaf = TRUE
x.n = 0
DISK-WRITE(x)
T.root = x
```

- B 木へのキーの挿入
  - キーを B 木に挿入する手続きは、すでに存在する葉に新たなキーを挿入する
  - しかし、飽和した葉にキーを挿入することはできないから、(2t - 1 個のキーを持つ)飽和節点 y をその中央キー y.keyt を境にして、それぞれが t - 1 個のキーを持つ 2 つの節点に分割する操作を導入する。中央キーは y の親に移し、新しく作った 2 つの木を分離する場所を特定するために用いる
  - しかし、y の親もまた飽和節点ならば、この新しいキーを挿入する前にこれを分割しておく必要があるから、飽和節点に対する分割操作は木を上方に伝播していく可能性がある
- B 木における節点分割
  - p411 参照
  - 未飽和な内部節点 x と、x.ci が x の飽和した子である添字 i を入力とする
  - 手続きはこの子を 2 つに分割し、x が新たに 1 個の子を獲得するように x を変形する
  - B-TREE-SPLIT-CHILD の実行に必要な CPU 時間は `θ(t)` である。この手続きは `Ｏ(n)` 回のディスク操作を行う

```
B-TREE-SPLIT-CHILD(x, i)
z = ALLOCATE-NODE()
y = x.ci
z.leaf = y.leaf
z.n = t - 1
for j = 1 to t - 1
  z.keyj = y.keyj+t
if not y.leaf
  for j = 1 to t
    z.cj = y.cj+t
y.n = t - 1
for j = x.n + 1 downto i + 1
  x.cj+1 = x.cj
x.ci+1 = x.cj
for j = x.n downto i
  x.keyj+1 = x.key
x.keyi = y.keyt
x.n = x.n + 1
DISK-WRITE(y)
DISK-WRITE(z)
DISK-WRITE(x)
```

- 単一の道を辿る B 木へのキーの挿入 B-TREE-INSERT
- 補助再帰手続き B-TREE-INSERT-NONFULL
  - 節点 x を根とする未飽和な部分木にキー k を挿入する
  - B-TREE-INSERT-NONFULL の間に `Ｏ(1)` 回しか DISK-READ と DISK-WRITE 操作を実行しないから、高さ h の B 木上の B-TREE-INSERT が実行するディスクアクセス回数は `Ｏ(h)` である
  - また、必要な総 CPU 時間は `Ｏ(th) = Ｏ(t * logt n)` である

```
B-TREE-INSERT(T, k)
r = T.root
if r.n == 2t - 1  // 根 r が飽和節点の場合。根を分割し、新しい節点 s を根とする
  s = ALLOCATE-NODE()
  T.root = s
  s.leaf = FALSE
  s.n = 0
  s.c1 = r
  B-TREE-SPLIT-CHILD(s, 1)
  B-TREE-INSERT-NONFULL(s, k)
else
  B-TREE-INSERT-NONFULL(r, k)

B-TREE-INSERT-NONFULL(x, k)
i = x.n
if x.leaf
  while i >= 1 かつ k < x.keyi
    x.keyi+1 = x.keyi
    i = i - 1
  x.keyi+1 = k
  x.n = x.n + 1
  DISK-WRITE(x)
else
  while i >= 1 かつ k < x.keyi
    i = i - 1
  i = i + 1
  DISK-READ(x.ci)
  if x.ci.n == 2t - 1
    B-TREE-SPLIT-CHILD(x, i)
    if k > x.keyi
      i = i + 1
  B-TREE-INSERT-NONFULL(x.ci, k)
```

### 18.3: B 木からのキーの削除

- B-TREE-DELETE はキー k を x を根とする部分木から削除する
  - B-TREE-DELETE が節点 x 上に再帰する時には、x のキー数が常に最小次数 t 以上となるように手続きは構成されている
  - B 木の(根以外の)各接点のキー数は t - 1 以上でなければならないが、この条件はそれよりも 1 だけ大きいキー数を要求していることに注意する
- 手順(p416 参照)
  - 1: キー k が節点 x に存在し、x が葉ならば k を x から削除する
  - 2: キー k が節点 x に存在し、x が内部節点ならば、以下の処理を行う
    - x における k の直前の子 y が少なくとも t 個のキーを持つならば、y を根とする部分木の中から k の直前のキー k' を見つける。再帰的に k' を削除し、x において k を k' で置き換える
    - 対称的に、x における k の直後の子 z が少なくとも t 個のキーを持つならば、z を根とする部分木の中から k の直後のキー k' を見つける。再帰的に k' を削除し、x において k を k' で置き換える
    - それ以外の場合、すなわち y と z が共に t - 1 個しかキーを持たない場合には、k と z のすべてのキーを y とマージする。その結果、x は k と z へのポインタを共に失い、y は 2t - 1 個のキーを持つ。z を開放し、再帰的に k を y から削除する
  - 3: キー k が内部節点 x に存在しない時、k が木の中に存在するならば必ず k を含む部分木の根 x.ci を決定する。x.ci が t - 1 個のキーしか持たないならば、再帰する節点が少なくとも t 個のキーを持つことを保証するために、ステップ 3a または 3b を必要に応じて実行する。最後に x の適切な子の上に再帰する
    - x.ci は t - 1 個のキーしか持たないが、少なくとも t 個のキーを持つ隣り合う兄弟が存在するならば、x のキーを x から x.ci に移し、この兄弟のキーを x に移し、適切な子ポインタをこの兄弟から x.ci に移すことで、x.ci のキー数を増やす
    - x.ci も左右の隣り合う兄弟もすべて t - 1 個のキーしか持たないならば、x.ci をどちらかの兄弟とマージする。この操作の中で、x のキーがマージされた新しい節点に移り、その中央キーになる
- この手続きは複雑そうに見えるが、DISK-READ と DISK-WRITE は手続きの再帰呼び出しの間に `Ｏ(1)` 回しか呼び出されないので、手続きは高さ h の B 木に対して `Ｏ(h)` 回のディスク操作を含むに過ぎない。必要な CPU 時間は `Ｏ(th) = Ｏ(t * logt n)` である

## 19: フィボナッチヒープ

- フィボナッチヒープデータ構造の目的は 2 つある
  - 「マージ可能ヒープ」データ構造を構成する操作が利用できる
  - いくつかのフィボナッチヒープ操作が定数ならし計算時間で走る
- マージ可能ヒープ(mergeable heap)
  - マージ可能ヒープは以下の 5 つの操作が利用できるデータ構造
    - 1: MAKE-HEAP(): 要素を含まない空の新しいヒープを生成し、それを返す
    - 2: INSERT(H, x): 要素 x をヒープ H に挿入する。ただし x のキーはすでに書き込まれていると仮定する
    - 3: MINIMUM(H): ヒープ H に属する最小のキーを持つ要素を指すポインタを返す
    - 4: EXTRACT-MIN(H): ヒープ H に属する最小のキーを持つ要素を抽出(削除)し、その要素を指すポインタを返す
    - 5: UNION(H1, H2): ヒープ H1 と H2 に属するすべての要素を含むヒープを新たに生成し、それを返す。ヒープ H1 と H2 はこの操作の結果「破壊」される
- フィボナッチヒープ(Fibonacci heap)
  - 上記のマージ可能ヒープ操作以外に、フィボナッチヒープではさらに以下の 2 つの操作が利用できる
    - DECREASE-KEY(H, x, k): ヒープ H に属する要素 x に新しいキー値 k を代入する。ただし、k は x の現在のキー値以下であると仮定する
    - DELETE(H, x): ヒープ H から要素 x を削除する
- 2 分ヒープとフィボナッチヒープ
  - UNION 操作が不必要ならば、6 章で利用した普通の 2 分ヒープが使いやすい(図 19.1 参照)
  - 理論的見地からは、フィボナッチヒープは EXTRACT-MIN と DELETE の操作数が他の操作に比べて少ない時に有利
  - しかし、実用的見地からは、オーダーに隠れた大きい定数と複雑なプログラムのせいで、フィボナッチヒープは通常の 2 分ヒープほど魅力的ではない

### 19.1: フィボナッチヒープの構造

- フィボナッチヒープは min ヒープ性を満たす min ヒープ順序付けられた根付き木の集合(p422 参照)
  - すなわち、任意の節点のキーはその親のキー以上である
  - 各節点は x は親へのポインタ x.p と任意の 1 個の子へのポインタ x.child を持つ。x のすべての子は巡回双方向連結リストで互いに連結されている。このリストを x の子リストと呼ぶ
  - x の子リスト中の子 y はそれぞれ左と右の兄弟を指すポインタ y.left と y.right を持つ
  - 属性 x.degree は節点 x の子リストに属するこの数
  - 属性 x.mark は節点 x が最後にある節点の子になった後、子を失ったか否かを示すブール値
- 2 つの理由からフィボナッチヒープの中で巡回双方向連結リストを利用する
  - 1: 巡回双方向連結リストの任意の位置へのある節点の挿入、および任意の位置からの節点の削除がそれぞれ `Ｏ(1)` 時間でできる
  - 2: 与えられた 2 つの巡回双方向連結リストを `Ｏ(1)` 時間で 1 つの巡回双方向連結リストに隣接できる(または繋げる)
- 与えられたフィボナッチヒープ H には、最小キーを含む木の根へのポインタ H.min によってアクセスする
- フィボナッチヒープに属するすべての木の根はポインタ left と right によって互いに連結され、1 つの巡回双方向連結リストを構成している
  - この巡回双方向連結リストを値リスト(root list)と呼ぶ
- ポテンシャル法を用いて、フィボナッチヒープ操作の性能を解析する
  - 与えられたフィボナッチヒープ H に対して、t(H) で H の値リストに属する木の数、m(H) で H に属するマークされた節点数を表す
  - フィボナッチヒープ H のポテンシャルを `Φ(H) = t(H) + 2m(H)` と定義する
  - 1 単位のポテンシャルはある定数量の仕事を賄うことができると仮定する
- 最大次数
  - 本章の残りの節で行うならし解析では、節点数 n のフィボナッチヒープに属する節点の最大次数の上界 D(n) が既知であると仮定する
  - 証明はしないが、マージ可能ヒープ操作だけが利用可能ならば、`D(n) = lg n` である

### 19.2: マージ可能ヒープ操作

- 新しいフィボナッチヒープの生成 MAKE-FIB-HEAP
  - H.n = 0 かつ H.min = NIL であるフィボナッチヒープオブジェクト H を生成し、これを返す。H には木は存在しない
  - t(H) = 0 かつ m(H) = 0 なので、ポテンシャル Φ(H) = 0
  - したがって MAKE-FIB-HEAP のならしコストはその実コスト `Ｏ(1)` に等しい
- 節点の挿入(p425 参照)
  - H を入力のフィボナッチヒープ、H' を結果として得るフィボナッチヒープとする
  - t(H') = t(H) + 1 かつ m(H') = m(H) であり、ポテンシャルの増加は 1
  - 実コストが `Ｏ(1)` なので、ならしコストは `Ｏ(1) + 1 = Ｏ(1)`

```
FIB-HEAP-INSERT
x.degree = 0
x.p = NIL
x.child = NIL
x.mark = FALSE
if H.min == NIL
  x だけを含む H の根リストを生成する
  H.min = x
else
  x を H の根リストに挿入する
  if x.key < H.min.key
    H.min = x
H.n = H.n + 1
```

- 最小節点の発見
  - ポインタ H.min がフィボナッチヒープ H の最小節点を指しているので、最小節点はコスト `Ｏ(1)` で発見できる
  - H のポテンシャルは変化しないので、この操作のならしコストは `Ｏ(1)` となる
- 2 つのフィボナッチヒープの統合
  - 以下の手続きはフィボナッチヒープ H1 と H2 を統合し、その過程で H1 と H2 を破壊する
  - FIB-HEAP-INSERT と同様、この操作ではすべての根は根のまま残る

```
FIB-HEAP-UNION(H1, H2)
H = MAKE-FIB-HEAP()
H.min = H1.min
H2 の根リストを H の根リストに連接する
if(H1.min == NIL) または (H2.min != NIL かつ H2.min.key < H1.min.key)
  H.min = H2.min
H.n = H1.n + H2.n
return H
```

- 最小節点の抽出
  - これまで延期してきた根リスト内の木の整理もここで行う
  - 図 19.4 に示すように FIB-HEAP-EXTRACT-MIN は、まず最小節点の子すべてを根リストに挿入し、根リストから最小節点を削除する
  - つぎに各次数の木が高々 1 つになるまで次数が等しい木の根を連結して、根リストを整理する
  - 根リストの整理
    - 1: 根リスト内から等しい次数を持つ根 x と y を見つける。一般性を失うことなく、x.key < y.key とする
    - 2: y を x に連結する。すなわち FIB-HEAP-LINK を呼び出して、根リストから y を取り出し、y を x の子とし、属性 x.degree を 1 増やし、y のマークを外す
  - 必要な実コスト
    - 最小節点が持つ高々 D(n) 個の子の処理と、CONSOLIDATE の 2-3 行目と 16-23 行の仕事に  `Ｏ(D(n))` 時間かかる
    - CONSOLIDATE を呼び出した後の根リストのサイズは、元々あった t(H) 個の節点から 1 つの節点を抽出し、代わりに高々 D(n) 個存在する抽出された節点の子を挿入したので、高々 D(n) + t(H) - 1
    - 4-14 行目について、7-13 行目の while 文の繰り返しは、高々根リストが含む根の個数。したがって、for 文が実行する総仕事量は D(n) + t(H) に比例する
    - よって、最小節点を抽出する総実コストは `Ｏ(D(n) + t(H))`
  - ポテンシャルの変化
    - 抽出する直前のポテンシャルは `Φ(H) = t(H) + 2m(H)`
    - 抽出後は高々 D(n) + 1 個の根が残っており(次数が同じものは連結されているため)、この操作中にはどの節点もマークされないので、ポテンシャルは高々 `(D(n) + 1) + 2m(H)`
  - ならしコスト
    - Ｏ(D(n) + t(H)) + (D(n) + 1) + 2m(H) - (t(H) + 2m(H)) = Ｏ(D(n)) + Ｏ(t(H)) - t(H) = Ｏ(D(n))
    - D(n) = Ｏ(lg n) なので(証明は 19.4 で扱う)、最小節点を抽出するためのならし計算量は `Ｏ(lg n)`

```
FIB-HEAP-EXTRACT-MIN(H)
z = H.min
if z != NIL
  for z の各子 x
    H の根リストに x を加える
    x.p = NIL
  H の根リストから z を削除する
  if z == z.right
    H.min = NIL
  else
    H.min = z.right
    CONSOLIDATE(H)
  H.n = H.n - 1
return z

CONSOLIDATE(H)
A[0..D(H.n)] を新しい配列とする
for i = 0 to D(H.n)
  A[i] = NIL
for H の根リストの各接点 w
  x = w
  d = x.degree
  while A[d] != NIL
    y = A[d]
    if x.key > y.key
      x と y を交換する
    FIB-HEAP-LINK(H, y, x)
    A[d] = NIL
    d = d + 1
  A[d] = x
H.min = NIL
for i = 0 to D(H.n)
  if A[i] != NIL
    if H.min == NIL
      A[i] だけを含む H の根リストを生成する
      H.min = A[i]
    else
      A[i] を H の根リストに挿入する
      if A[i].key < H.min.key
        H.min = A[i]

FIB-HEAP-LINK(H, y, x)
H の根リストから y を削除する
y を x の子とし、x.degree を 1 増やす
y.mark = FALSE
```

### 19.3: キー値の下方修正と節点の削除

- キー値の下方修正(図 19.5 参照)
  - FIB-HEAP-DECREASE-KEY のならしコストが `Ｏ(1)` であることを示す
    - 実コスト
      - Ｏ(1) 時間と縦列切断を実行する時間
      - 縦列切断は CASCADING-CUT を再帰的に c 回呼び出すと仮定する。再帰呼び出しを除けば、CASCADING-CUT の各呼び出しには Ｏ(1) 時間かかる
      - したがって、実コストは再帰呼び出しを含めて `Ｏ(c)`
    - ポテンシャルの変化
      - H を FIB-HEAP-DECREASE-KEY を行う前のフィボナッチヒープとする
      - FIB-HEAP-DECREASE-KEY の 6 行目の CUT の呼び出しは節点 x を根とする新しい木を生成し、x のマークビットを FALSE にする
      - 操作終了直後、このフィボナッチヒープには t(H) + c 個の木と、高々 m(H) - c + 2 個のマークされた節点が存在しうる
      - したがって、ポテンシャルの変化は高々 ((t(H) + c) + 2(m(H) - c + 2)) - (t(H) + 2m(H)) = 4 - c
    - Ｏ(c) に隠されている定数よりもポテンシャルの 1 単位を大きくとると、FIB-HEAP-DECREASE-KEY のならしコストは高々 `Ｏ(c) + 4 - c = Ｏ(1)`
  - ポテンシャル関数をマークされた節点数の 2 倍の項を含むように定義しておいた理由がここでようやく理解できる
    - マークされた節点 y を縦列切断で切断する時、マークビットをリセットするので、ポテンシャルは 2 だけ減少する
    - ポテンシャルの 1 単位を切断とマークビットのリセットに支払い、他の 1 単位は節点 y が根になったことによるポテンシャルの 1 単位の増加を保証するために使う

```
FIB-HEAP-DECREASE-KEY(H, x, k)  // 新しいキー k を節点 x に代入する
if k > x.key
  error "新しいキーは現在のキーより大きい"
x.key = k
y = x.p
if y != NIL かつ x.key < y.key
  CUT(H, x, y)
  CASCADING-CUT(H, y)
if x.key < H.min.key
  H.min = x

CUT(H, x, y)
y の子リストから x を削除し、y.degree を 1 減らす
x を H の根リストに加える
x.p = NIL
x.mark = FALSE

CASCADING-CUT(H, y)
z = y.p
if z != NIL
  if y.mark == FALSE
    y.mark = TRUE
  else
    CUT(H, y, z)
    CASCADING-CUT(H, z)
```

- 節点の削除
  - 以下の擬似コードは節点数 n のフィボナッチヒープから `Ｏ(D(n))` ならし時間で節点を削除する。ただし、このフィボナッチヒープ内にキー値 -∞ を持つ節点が存在しないと仮定する
  - この操作では、x のキー値を -∞ に下方修正して、FIB-HEAP-EXTRACT-MIN で削除する
  - FIB-HEAP-DECREASE-KEY のならしコストが Ｏ(1)、FIB-HEAP-DECREASE-KEY のならしコストが Ｏ(lg n) なので、FIB-HEAP-DELETE のならしコストは `Ｏ(lg n)`

```
FIB-HEAP-DELETE(H, x)
FIB-HEAP-DECREASE-KEY(H, x, -∞)
FIB-HEAP-EXTRACT-MIN(H)
```

### 19.4: 最大次数の上界

- 節点数 n のフィボナッチヒープの任意の節点の次数の上界 D(n) が Ｏ(lg n) であることを示す
- x を根とする部分木が含む節点数を size(x) とする(x 自身も含む。x は根リストに属する必要はなく、任意の節点である)
- 方針
  - size(x) が x.degree の指数関数であることを示す
- x をフィボナッチヒープの任意の節点とし、x.degree = k とする。y1, y2, ..., yk を x の子とし、この順序で x に連結されたとする。このとき、y1.degree >= 0 かつ yi.degree >= i - 2 が i = 2, 3, ..., k に対して成立する(補題 19.1)
- x をフィボナッチヒープの任意の節点とし、k = x.degree とする。φ = (1 + √5) / 2 とする時、size(x) >= F k+2 >= φ^k である(補題 19.4)
- すると節点数 n のフィボナッチヒープに属する任意の節点を x とし、k = x.degree とする。補題 19.4 より n >= size(x) >= φ^k なので φ を底として対数を取ると、k <= logφ n となる
  - したがって、任意の節点の最大次数 D(n) は `Ｏ(lg n)` である

## 20: van Emde Boas(ファン・エンデ・ボス) 木

- 集合 {0, 1, ..., u - 1} を格納できる値の普遍集合と予備、u を普遍集合サイズと呼ぶ
- 本章を通して、u は 2 のべき乗、すなわち、ある k >= 1 に対して u = 2^k であると仮定する

### 20.1: 設計方針の予備的考察

- 本節では動的集合を格納するための様々な方針を検討する
- 直接アドレス指定法
  - 普遍集合 {0, 1, ..., u - 1} に属する値を格納するために、u ビットの配列 A[0...u-1] を管理する
  - 値 x がその動的集合に属するならば A[x] = 1、そうでなければ A[x] = 0 である
  - INSERT / DELETE / MEMBER 操作はそれぞれ `Ｏ(1)` 時間で実行できる
  - MINIMUM / MAXIMUM / SUCCESSOR / PREDECESSOR の実行には θ(u) 個の要素を走査しなければならなくなる可能性があり、最悪時には `θ(u)` 時間がかかる
- 2 分木構造の添加(図 20.1 参照)
  - ビットベクトル上に 2 分木構造を添加することで長い走査を短縮できる
  - ビットベクトルの要素が 2 分木の葉に対応しており、2 分木の各内部節点が 1 を持つのは、その内部節点を根とする部分木に属するある葉が 1 を持つ時、かつその時に限る
  - これを使うと、INSERT / DELETE / MINIMUM / MAXIMUM / SUCCESSOR / PREDECESSOR を `Ｏ(lg u)` 時間で実行でき、MEMBER を `Ｏ(1)` 時間で実行できる
  - 単純な 2 色木と比べて MEMBER 操作が早いが、格納されている要素数 n が普遍集合サイズ u に比べて十分に小さい時には、MEMBER 以外のすべての操作の実行速度は 2 色木が勝る
- 定数高さを持つ木の添加
  - ある整数 k に対して普遍集合サイズを u = 2^2k と仮定する。したがって √u は整数である
  - 2 分木をビットベクトルの上に添加する代わりに、次数 √u の木を添加する
  - これは MINIMUM / SUCCESSOR / PREDECESSOR / DELETE を `Ｏ(√u)` 時間で実行できる

### 20.2: 再帰構造

- 次数 √u の木をビットベクトルの上に添加するというアイデアを本節では改良する
  - 前節では構造 summary を用いたが、今度はこの構造を再帰的にし、各再帰のレベルで普遍集合サイズが 1/2 乗に減少するようにする
  - 簡単のため、ある整数 k に対して、2^(2^k) であると本節では仮定する(次節でこの制約を緩和し、k = 2^k であれば十分であるようにする)
  - 各操作に対して `Ｏ(lg lg u)` 実行時間を達成することが目的
- 以下の関数を用意する
  - high(x) = x / √u の整数部分
  - low(x) = x mod √u
  - index(x, y) = x√u + y
- プロト van Emde Boas 構造(proto van Emde Boas structure)
  - 普遍集合 {0, 1, ..., u - 1} に対して、プロト van Emde Boas 構造または proto-vEB 構造(proto-vEB structure) を proto-vEB(u) で表し、以下のように再帰的に定義する
    - u = 2 が基底となる普遍集合サイズで、2 ビットの配列 A[0..1] を含む
    - ある整数 k >= 1 に対して、u = 2^(2^k)、したがって u >= 4 の場合、データ構造 proto-vEB(u) は普遍集合サイズ u 以外に図 20.3 に示す次の属性を含む
      - summary と名付けられた proto-vEB(√u) 構造を指すポインタ
      - proto-vEB(√u) 構造を指す √u 個のポインタの配列 cluster[0..√u - 1]
  - 図 20.4 がわかりやすい
- 集合への所属判定
  - 各再帰呼び出しは、それが行う再帰呼び出しの時間を除くと、定数時間しかかからない
  - すると実行時間は T(u) = T(√u) + Ｏ(1) で特徴づけられる
  - よって、実行時間は `Ｏ(lg lg u)` となる

```
PROTO-VEB-MEMBER(V, x)
if V.u == 2  // 基底か判断
  return V.A[x]
else
  return PROTO-VEB-MEMBER(V.cluster[high(x)], low(x))
```

- 最小要素の発見
  - 再帰呼び出しを 2 回行うので、最悪実行時間は期待する `Ｏ(lg lg u)` ではなく `Ｏ(lg u)`

```
PROTO-VEB-MINIMUM(V)
if V.u == 2
  if V.A[0] == 1
    return 0
  elseif V.A[1] == 1
    return 1
  else
    return NIL
else
  min-cluster = PROTO-VEB-MINIMUM(V.summary)
  if min-cluster == NIL
    return NIL
  else
    offset = PROTO-VEB-MINIMUM(V.cluster[min-cluster])
      return index(min-cluster, offset)
```

- 直後の要素の発見
  - 最悪の場合、PROTO-VEB-SUCCESSOR は proto-vEB(√u) 構造上でそれ自体を再帰的に 2 回呼び出すと共に、proto-vEB(√u) 構造上で PROTO-VEB-MINIMUM を 1 回呼び出す
  - 最悪時の実行時間は `θ(lg u * lg lg u)`

```
PROTO-VEB-SUCCESSOR(V, x)
if V.u == 2
  if x == 0 かつ V.A[1] == 1
    return 1
  else
    return NIL
else
  offset = PROTO-VEB-SUCCESSOR(V.cluster[high(x)], low(x))
  if offset != NIL
    return index(high(x), offset)
  else
    succ-cluster = PROTO-VEB-SUCCESSOR(V.summary, high(x))
    if succ-cluster == NIL
      return NIL
    else
      offset = PROTO-VEB-MINIMUM(V.cluster[succ-cluster])
      return index(succ-cluster, offset)
```

- 要素の挿入
  - 要素を挿入するには、要素を正しいクラスタに挿入し、対応する要約ビットを 1 に設定する設定する必要がある
  - 最悪時に再帰呼び出しを 2 回行うので、`θ(lg u)` 時間で走る

```
PROTO-VEB-INSERT(V, x)
if V.u == 2
  V.A[x] = 1
else
  PROTO-VEB-INSERT(V.cluster[high(x)], low(x))
  PROTO-VEB-INSERT(V.summary, high(x))
```

- 要素の削除
  - DELETE 操作は挿入よりも複雑
  - 挿入では常に要約ビットを 1 にできるが、削除では対応する要約ビットを簡単に 0 に戻せない
  - proto-vEB 構造の定義から、クラスタ中に値 1 を持つビットが残っているかどうかを判断するために、√u 個のビットをすべて調べなければならない
  - これは大変なので、代案として、保持する 1 の個数を計数する属性 n を proto-vEB 構造に追加することが考えられる
  - proto-vEB 構造の改良を次節で考える

### 20.3: van Emde Boas 木

- 前節の proto-vEB 構造によって目標の `Ｏ(lg lg n)` 実行時間に近づいたが、多くの操作の実行で再帰回数が多すぎるという問題を抱えた
  - 本節では、proto-vEB 構造によく似たデータ構造を設計し、再帰のいくつかを回避するようにする
- ここから先は 2 の任意のべき乗を u として許す
- van Emde Boas(ファン・エンデ・ボス)木、あるいは vEB 木
  - vEB 木は proto-vEB 構造が含まない次の 2 つの属性を含む
    - vEB の最小要素を表す min
      - ただし、min に格納された要素は cluster 配列が指す再帰的 vEB(√u) 木に出現しない(max は出現する)
    - vEB の最大要素を表す max
  - 図 20.6 参照
- 属性 min と max が果たす役割
  - 1: MINIMUM および MAXIMUM 操作は min あるいは max 値を返せばよいから、再帰をする必要がない
  - 2: SUCCESSOR 操作において値 x の直後の値が high(x) の中にあるか否かを判定する再帰呼び出しを回避できる。PREDECESSOR と min に対しても同様
  - 3: min と max が共に NIL ならばこの vEB 木は要素を持たない。min = max != NIL ならこの vEB 木はちょうど 1 個の要素を持つ。min と max が共に NIL ではなく、しかも min != max ならばこの vEB 木は 2 個以上の要素を持つ
    - このように、min と max の値から vEB 木が要素を何個持つか定数時間で判定できる
    - この能力は INSERT と DELETE 操作を助ける
  - 4: 要素を持たない vEB 木に対しては min と max 更新するだけで要素が挿入できる。同様に要素をちょうど 1 個持つ vEB 木からの(この要素の)削除は min と max 属性を更新するだけで定数時間で実現できる
- すると vEB 木操作を実現する再帰手続きはすべて漸化式 `T(u) <= T(√u) + Ｏ(1)` で表すことができ、これは `T(u) = Ｏ(lg lg u)` となる
- 最小および最大要素の発見

```
VEB-TREE-MINIMUM(V)
return V.min

VEB-TREE-MAXIMUM(V)
return V.max
```

- 集合への所属判定
  - 実行時間は `Ｏ(lg lg u)`

```
VEB-TREE-MEMBER(V, x)
if x == V.min または x == V.max
  return TRUE
elseif V.x == 2
  return FALSE
else
  return VEB-TREE-MEMBER(V.cluster[high(x)], low(x))
```

- 直後および直前の要素の発見
  - VEB-TREE-SUCCESSOR
    - 1 回の再帰呼び出しが行われるが、普遍集合サイズが高々 √u、VEB-TREE-MINIMUM と VEB-TREE-MAXIMUM は `Ｏ(1)` 時間で実行できるので、最悪実行時間は `Ｏ(lg lg u)` となる
  - VEB-TREE-PREDECESSOR
    - 基本的には VEB-TREE-SUCCESSOR と対称(詳細は p460)。最悪時に `Ｏ(lg lg u)` で走る

```
VEB-TREE-SUCCESSOR(V, x)
if V.u == 2
  if x == 0 かつ V.max == 1
    return 1
  else
    return NIL
elseif V.min != NIL かつ x < V.min
  return V.min
else
  max=low = VEB-TREE-MAXIMUM(V.cluster[high(x)])
  if max-low != NIL かつ low(x) < max-low
    offset = VEB-TREE-SUCCESSOR(V.cluster[high(x)], low(x))
    return index(high(x), offset)
  else
    succ-cluster = VEB-TREE-SUCCESSOR(V.summary, high(x))
    if succ-cluster == NIL
      return NIL
    else
      offset = VEB-TREE-MINIMUM(V.cluster[succ-cluster])
      return index(succ-cluster, offset)
```

- 要素の挿入
  - まず空の vEB 木に対する要素の挿入は、以下のように再帰を必要としない
  - 実行時間は `Ｏ(lg lg u)` となる

```
VEB-EMPTY-TREE-INSERT(V, x)
V.min = x
V.max = x

VEB-TREE-INSERT(V, x)
if V.min == NIL
  VEB-EMPTY-TREE-INSERT(V, x)
else
  if x < V.min
    x と V.min を交換する
  if V.u > 2
    if VEB-TREE-MINIMUM(V.cluster[high(x)]) == NIL
      VEB-TREE-INSERT(V.summary, high(x))
      VEB-EMPTY-TREE-INSERT(V.cluster[high(x)], low(x))
    else
      VEB-TREE-INSERT(V.cluster[high(x)], low(x))
  if x > V.max
    V.max = x
```

- 要素の削除
  - VEB-TREE-DELETE(V, x) では x が VEB 木 V が表現する集合に属していると仮定する
  - 2 つの VEB-TREE-DELETE が同時に起こるのは、x がそのクラスタの唯一の要素である場合
    - これは 1-3 行を実行するだけで終了する
  - よって、再帰呼び出しは最大 1 回しか起こらず、最悪実行時間は `Ｏ(lg lg u)` となる

```
VEB-TREE-DELETE(V, x)
if V.min == V.max
  V.min = NIL
  V.max = NIL
elseif V.u == 2
  if x == 0
    V.min = 1
  else
    V.min = 0
  V.max = V.min
else
  if x == V.min
    first-cluster = VEB-TREE-MINIMUM(V.summary)
    x = index(first-cluster, VEB-TREE-MINIMUM(V.cluster[first-cluster]))
    V.min = x
  VEB-TREE-DELETE(V.cluster[high(x)], low(x))
  if VEB-TREE-MINIMUM(V.cluster[high(x)]) == NIL
    VEB-TREE-DELETE(V.summary, high(x))
    if x == V.max
      summary-max == VEB-TREE-MAXIMUM(V.summary)
      if summary-max == NIL
        V.max = V.min
      else
        V.max = index(summary-max, VEB-TREE-MAXIMUM(V.cluster[summary-max]))
  elseif x == V.max
    V.max = index(high(x), VEB-TREE-MAXIMUM(V.cluster[high(x)]))
```

# 概要

## 本

- [CPUの創りかた | 渡波 郁](https://amzn.to/31iMqeQ)

## かかった時間

- 100.0 時間（退職前に使っていた時間はカウントできていない、たぶんプラス 6 時間くらい）

## 作成メモ

- [「CPUの創りかた」の4bitCPUをブレッドボードで作りました - Qiita](https://qiita.com/y-meguro/items/9ed2a1b772eba4395ea6)

## コード

- [Arduino/td4_rom at master · y-meguro/Arduino](https://github.com/y-meguro/Arduino/tree/master/td4_rom)

# 読書メモ

## 1章: はじめの一歩のその前に

- ここで単純化した CPU を作ることを目標にするが、基本的な動作自体は一般的な CPU と同じ
  - プログラムカウンターに従って、命令をメモリーから読み出す（命令フェッチ）
  - 命令を解読（デコード）
  - デコード結果に従い、実際の演算を行う（実行）
  - 演算結果を格納（ストア）
- 各装備
  - セラミックコンデンサ: 応答速度は速いが、容量は小さい
  - 電解コンデンサ: 応答速度は悪いが、大きな容量が安価に得られる
  - ダイオード:「電気の一方通行」デバイス

## 2章: LED

- オームの法則
  - E = IR（電圧 V = 電流 A * 抵抗 Ω）
  - P = IE（電力 W = 電流 A * 電圧 V）
- LED: Light Emitted Diode。発光ダイオード
  - ダイオードなので電流は一方通行となる
  - アノードがプラス側（図だと三角形の方、足の長い方）、カソードがマイナス（図だと線の方）
  - 電源と LED だけだと、電流が無制限になるので、抵抗もつなげればよい

## 3章: デジタル回路の基礎の基礎

- 汎用 IC: 様々な論理回路に共通して必要とされる個々の機能を 1 つの小型パッケージにまとめた集積回路
- 専用 IC: 特定の用途に特化した集積回路
- CPLD / FPGA は「回路を自由にプログラム可能なロジック IC」なので、必要な回路そのものをプログラムできる
- 1 と 0 の表現
  - 電圧が高い（5V）場合を High、電圧が低い（0V）場合を Low
  - High に 1 を割り当てる場合を「正論理」、Low に 1 を割り当てる場合を「負論理」という
  - 回路図上で、上にバーがある場合は負論理
  - 74HC シリーズの場合、H は 4.4V 以上、L は 0.3V 以下
- 簡単な論理回路
  - NOT は 74HC04、OR は 74HC32、3 入力 NAND は 74HC10
  - DIP: Dual Inline Package。長方形のパッケージの双方の長辺に 2 列の足を配置したもの
- 実際の回路
  - 使わない入力ピンは H か L に固定する
  - Vcc に +5V を接続すればいい
  - IC の近くに 0.1μF 程度の積層セラミックコンデンサを配置する
  - 電解コンデンサには極性があるので注意。基板に 1 つ、100μF 程度の電解コンデンサを配置する
  - IC への電源の配線の表記は省略されているので注意
  - 電源の配線は網目のように行う。電源パターン付きのユニバーサル基板が便利

## 4章: リセットとクロック回路

- 手動クロック回路: 手動で H と L を出力する回路
- プルアップ抵抗
  - スイッチを切り替える時に、一瞬だけ CLOCK 信号は GND にも 5V にもつながっていない状態（オープン）になる
  - すると電圧が不定になってしまうので、これを防ぐために、CLOCK 信号を抵抗で 5V（Vcc）につないでおく
  - このような抵抗を「プルアップ抵抗」という。逆に低い電位で保っておく抵抗を「プルダウン抵抗」という
- チャタリングの問題
  - スイッチが接触する瞬間は、激しくバウンドが起きる。このバウンドを「チャタリング」という
  - スイッチはバウンドした回数だけ、入力されたと認識してしまうため、対応が必要
  - これに対応するのが CR のフィルタ（C がコンデンサ、R が抵抗）。回路の応答を遅くすることで解決する
- デジタル IC では中途半端な電圧（0V は L、5V は H だが、2V とか 3V の場合）でも H か L に判定される
  - ゆっくり電圧が上昇する場合は、出力が不安定になる
  - デジタル IC の中でも中途半端な電圧を扱えるものがいて、それが 74HC14 シュミットトリガ・インバーター
- リセット回路
  - ここでは L でリセットがかかる仕様とする（電源を ON にしたら、一時的に L を出力する回路にすればよい）
- クロックジェネレータ
  - ここで作るクロックは 1Hz とする
  - 抵抗を 33KΩ、コンデンサを 10μF とすると発信周期がおおよそ 1Hz となる
  - 抵抗を 3.3KΩ にするとクロックを 10Hz にできる
  - ここで使うコンデンサはプラスにもマイナスにも充電されるので、無極性電解コンデンサを使用する

## 5章: ROM を作る

- ROM というのは
  - ここで作る CPU はプログラムを ROM に格納する
  - ROM にアドレスを指定すると、データが返されるようにする
  - すべてのデータを一度に読み出すわけではなく、指定されたアドレスの 8bit だけ読み出す
- ROM の回路
  - メモリーセルは DIP スイッチで代用する
  - プルアップ抵抗を使い、また出力側で論理反転することで、スイッチ ON で H とする
  - 8bit 出力、アドレスは 0〜15 番地までとし、16 バイトの ROM を作る
  - 74HC154 で 4 本の信号を 0〜15 に変換する。G1・G2 入力は両方 L に固定する
- プリント基板
  - 以下の 2 つをまとめて指す総称
    - 絶縁体でできた板の上や内部に、導体の配線が施された（だけの）もの。電子部品が取り付けられる前の状態。プリント配線板（PWB = Printed Wiring Board）と呼ばれる
    - （上記の板に）電子部品がはんだ付けされ、電子回路として動作するようになったもの。プリント回路板（PCB = Printed Circuit Board）と呼ばれる

## 6章: CPU の設計準備

- CPU の仕様
  - 4bitCPU を作成する
  - レジスタ構成
    - 演算用のレジスタは A と B の 2 つで、4bit。メモリーも兼用する
    - リセット直後は A・B 共に「0000」となる
    - プログラムカウンタも 4bit で、プログラムは最大でも 16 ステップとする
    - フラグは C（キャリー）のみとし、加算命令でキャリーが発生すると「1」になる
    - 入力ポート、出力ポート共に 4bit とする
  - 命令フォーマット
    - すべての命令は 8bit で構成される。上位 4bit がオペレーションコード、下位 4bit がイミディエイトデータ

## 7章: 1bitCPU（らしきもの）

- TD4 の A レジスタと B レジスタは 4bit なので、それぞれフリップ・フロップ 4 つで構成される
- フリップ・フロップは 74HC74 を利用する
- 入力に切り替えスイッチを付けて、「新しいデータをロード（書き込み）」するのか「自分自身をコピー（保持）」するのかを選択できるようにする
- データ保持の方法
  - フリップ・フロップの出力を入力に戻す
- レジスタ間のデータコピーの方法
  - 転送元のフリップ・フロップの出力を転送先のフリップ・フロップへつなぐ
- 演算の方法
  - データを転送する途中で演算回路を経由させる
- 4 チャンネルのデータセレクタには 74HC153 を利用する
- また、2 チャンネルデータセレクタと 4 つのフリップ・フロップが入っている 74HC161 を利用する

## 8章: ALU とプログラムカウンタ

- 4bit 加算器の 74HC283 を利用する
- キャリー入力がない加算器を半加算器（Half Adder）、ある加算器を全加算器（Full Adder）と呼ぶ
- TD4 のプログラムカウンタに必要な機能は 2 つ
  - リセット信号により、ゼロにリセットされる
  - 1 クロックの立ち上がりごとに +1（カウントアップ）される
- 74HC181 を流用して、ENT と ENP を H に固定することで、カウントできる
- 出力ポートは C レジスタに LED をぶらさげる

## 9章: 命令デコーダ

- 命令フェッチした後、実行部の動作の流れ
  - SELECT A 信号と SELECT B 信号を設定し、4 チャンネルデータセレクタがレジスタを選択、ALU へ送る
  - ALU は演算を行い、全レジスタへ演算結果を送る
  - LOAD0〜LOAD3 信号により、各レジスタの保持またはロードが決定される
  - 後は次のクロックの立ち上がりを合図に実行する
- 命令デコーダが指示しなくてはならない信号は SELECT A / SELECT B / LOAD0〜LOAD3

## 10章: 全回路図

- 全回路図の記載あり

## 11章: 動作確認

### 最初は部分的な確認

- 通電しなくても確認できること
  - IC がソケットに挿さっていない状態からスタートする。またコンセントを抜くだけでなく、電源と CPU を切り離す
  - 一番怖いのは電源のショートなので、まず基板のどこかで +5V と GND がショートしていないか、テスターを使ってチェックする
  - 次に配線をテスターで 1 本ずつチェックする。単なる配線をチェックするので、テスターの極性は気にしなくてもいい。正常に繋がっていればテスターは 0Ω を示すはず
- 電源のチェック
  - この時点では IC はソケットに挿さっていないはずなので IC は壊れないが、電解コンデンサは極性が間違っていると吹き飛ぶことがある
  - 5V を通電して、特に問題（異臭・異音）がないようだったら、各 IC ソケットの電源ピンをテスターで測る
- リセット回路のチェック
  - リセット回路の 74HC14 だけソケットに挿す
  - 3 点の電圧を確認する
- プログラムカウンタのチェック
  - ROM の DIP スイッチはすべて OFF にしておく
  - テスターですべて確認するのは大変なので、LED をぶらさげておくと簡単
  - LD 信号がおかしくても、深追いはせずに LD 信号と命令デコーダを一時的に切り離して H に直結し、プログラムカウンタのカウントアップ動作だけを確認する
- ROM のチェック
  - 入力 A〜D と G1、G2 が正しいか
  - Y0〜Y15 が正しいか。これで 74HC154 の入力と出力が確認できる
  - このあとは「74HC540 の G1 と G2」「74HC154 の出力に従って、DIP スイッチの設定値が 74HC540 の入力に反映されるか」「74HC540 の入力が論理反転された信号が出力されているか」を確認する
- 命令デコーダのチェック
  - 全命令について、順々に出力が正しいか確認していく
  - 6bit の LED をぶらさげておくと楽かも
  - C フラグの影響はまだ確認できないので、ここだけは後で確認すれば良い
- イミディエイトデータのチェック
  - ROM から適当なイミディエイトデータを出力させて、これが加算器の入力に届いているか確認する
- 加算器のチェック・加算期からレジスタへの配線
  - まず入力ポートの DIP スイッチの状態がデータセレクタの出力まで届いているか確認する
  - 次に加算器の出力をチェックする。イミディエイトデータの値を変えて、加算動作を確認する
  - 最後に加算器の出力が各レジスタに到達しているか確認する
- 実際に命令を実行する

### プログラムの実行

- サンプルプログラム 1: LED ちかちか
- サンプルプログラム 2: ラーメンタイマー

### もう少しまともな CPU

- 8bit 化 / ALU の強化 / CPLD など / 既製品の CPU をいじってみる

# 概要

## 本

- [30日でできる! OS自作入門 | 川合 秀実](https://amzn.to/2JS3LoS)

## かかった時間

- x 時間

## 読む前の状態

- OS についての本は何も読んでいなく、1 冊目として手を動かしながら概要を抑えるために読み始めた

## 読む前後の変化

- xxx

# 作成メモ

## 0章: 開発を始める前に

- バイナリエディタとして [0xED](http://www.suavetech.com/0xed/0xed.html) を使うことにした
- エミュレータとして [QEMU](https://www.qemu.org/) を使うことにした
  - バージョンは 4.1.0
- `qemu-system-i386 helloos.img` を実行すると、以下の warning が出る

```
$ qemu-system-i386 helloos.img
WARNING: Image format was not specified for 'helloos.img' and probing guessed raw.
         Automatically detecting the format is dangerous for raw images, write operations on block 0 will be restricted.
         Specify the 'raw' format explicitly to remove the restrictions.
```

- エラーメッセージと、[QEMU version 4.1.0 User Documentation](https://qemu.weilnetz.de/doc/qemu-doc.html) を読んで、以下のように実行したところ、warning は消えた

```
qemu-system-i386 -drive file=helloos.img,format=raw,if=floppy
```

# 読書メモ

## 0章: 開発を始める前に

- 今回は次の手順で作る
  - Windows などを使ってソースプログラムを書く
  - それを C コンパイラでコンパイルして、機械語がいっぱい詰まったファイルを作る
  - そのファイルを加工して、フロッピーディスクのイメージファイルを作る
  - イメージファイルをディスクへ書き込んで、OS がインストールされたディスクを作る
- OS を作ること =「自動で起動する OS が入ったディスク」を作ること

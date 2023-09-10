# Storage

ストレージに関する知識を羅列する。

## Software

### Fils System

ストレージにどうデータを乗せるかやその管理をするためのシステム。OSで管理される。
`df -T`で調べられる。

#### File Systemの種類

File Systemにはいくつかの種類がある。
Linuxでは以下のFile Systemが使われる。Linuxではこれらの差異をVFS (Virtual File System)として吸収できる。
- ext2
  - 初期のLinuxシステム
  - 最大16TiBのボリュームサイズ
- ext3
  - ext2の後継
  - 最大ボリュームサイズは2TiB~32TiB
- ext4
  - ext3の後継。ext3としてマウント可能
  - 最大ボリュームサイズは1EiB
- xfs
  - CentOS7で使われるらしい

#### i-nodeについて

i-nodeはindex-nodeの略。
File Systemはグラフ構造を持つが、そのnodeのこと。
i-nodeはディレクトリとファイルのエントリと対応している。
i-nodeは以下の情報を持つ。ファイル名の情報は持っていないらしい。
- inode番号
- ファイルを格納するデバイスのID
- ファイル所有者のユーザID
- mode (permission)
- timestamp
  - ctime (inodeの更新時)
  - mtime (ファイルの更新時)
  - atime (参照時)

### Distributed Storage
　
クラウドなどスケーラブルな環境で動作するストレージについてまとめる。
これらはファイルシステムの上に立つソフトウェアである。

- HDFS (Hadoop Distributed File System)
  - Apache
- Amazon S3 (Amazon Simple Storage Service)  
  - Amazon
- Google Could Storage
  - Google
- MinIO
  - MinIO inc.
  - S3 compatible

## Hardware

### Drive

ドライブは以下のようなものがある。
- 光学ドライブ
- HDD (Hard Disk Drive)
- SSD (Solid State Drive)

linuxなら`lsblk`(list block storage)コマンドで接続されているストレージの一覧を調べられる。

#### 光学ドライブ

DVDやCDなど。凹凸を光で読み取る。
これをコンピュータのドライブとして使っている例は今はなさそう。

#### HDD

磁気ヘッドでプラッターと呼ばれるディスクから読み取る。
データは磁気で保存される。
プラッターはデリケートなので、故障の際もプラッターを直接触ったり露出させないほうがよい。

#### SSD

以下からなる。
- メモリコントローラ
  - 各コンポーネントとの処理のやり取りをする
  - 埋め込みプロセッサが搭載されていrう
  - SSDの性能はここがボトルネックらしい
- キャッシュメモリ
  - 必須ではないので安いSSDには搭載されてなかったりする
- NAND型フラッシュメモリチップ
  - 実際のデータを格納する場所
- インターフェース

##### HDDとの比較

- メリット
  - 速い
  - 信頼性が高い
  - 耐久性が高い
  - 静音
- デメリット
  - 高い

##### インターフェースの規格について

Driveとマザーボード間での情報のやり取りにはいくつかの規格がある。

- ATA (Advanced technology attachment)
  - 大昔の規格
  - IDE (Integrated Drive Electronics)を標準化したもの
  - 8, 16bitのパラレル伝送方式
    - 複数の線でつなぐ
  - MB/sくらいの伝送速度感
- SATA (Serial ATA)
  - シリアル伝送方式
    - パラレルにするよりシリアルにしたほうがシンプルで、クロック数が上がる効果のほうがでかくて高速化されるらしい
  - GB/sくらいの伝送速度間
- NVMe
  - SATAより速い
  - SATAはあくまでHDDの技術（ドライバなど）を使っているのがウィークポイントらしい
  - PCIeソケットでCPUと通信する
  - 最近のSSDはNVMeらしい


なお、M.2がこれらと混同されることがあるが、M.2はフォームファクタである。
フォームファクタとは物理的な配置やコネクタの企画などを指す。
M.2 SSDが速いと誤解されることがあるが、
それはM.2自体が新しい規格であるためNVMeなど高速な規格を使っていることが多いためである。

      
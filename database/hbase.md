# HBase

HBaseとはKey-value storeの一種である。
BigTableの論文をOSSと実装したものである。

BigTableはSparse, distributed, persistent, multi-dimentional sorted mapと説明される。HBaseも同様の性質を持つ。

- Sparse
  - Row keyおよびColumnについてSparse
- Distributed
  - UnderlyingなStoreとして主にHDFSが使われており、分散されている
- Persistent
  - WALなどを用いて永続性を担保している
    - HDFSへの書き込みは低速だが、Append onlyな書き込みは比較的高速にできる
- Multi-dimentional
  - Rowkey, column, timestampでkeyを指定する
- Sorted
  - Sortedであるため、範囲のScan操作が効率的に実行できる


## Comparison

### Compared to RDBMS

HBaseは仕組み上、一般的なRDBMSに備わっている以下の機能が存在しない。
- Transaction
  - Atomicityはrow levelで保障される
- Constraint
  - Unique制約のような制約をつけることはできない
    - もちろんcheck and putのようなmethodで疑似的に再現はできるが
- Typed Column
  - 例えば数字としての演算や文字列としての演算は基本的に用意されていない
    - HBaseは原則的にBytes-in/outであるため
    - IncreaseのようなByteとして1増やす演算は存在する
- Secondary index
  - HBaseは単一のkeyで引く仕組みであり、複数のkeyから引くことはできない  

一方、スループットやスケールはHBaseに利がある。
一般的に、RDBMSは1Mレコード程度では効率よく動作するので、1Mレコード程度の利用であればRDBMSを使うのがよい。それ以上（100M~1B以上）のデータを用いる場合はHBaseが選択肢である。

### Compared to Hive, presto, Spark

HiveやPresto, Sparkは内部でORCなどのフォーマットでデータを持つ。
ある程度のindexを持ってはいるが、低レイテンシーで動作するような設計になっていない。
これらはデータウェアハウスとして利用されるのが適切である。

### Compared to Redis

Key-value storeとしてはRedisも比較対象である。
Redisはin-memoryな設計上、スケールが圧倒的にHBaseに利がある。

### When to use HBase.

HBaseが適切なユースケースは以下を全て満たすときである。
- 100M~1Bレコード以上のデータを用いる
- RDBMSのようなリッチなfeatureが不必要
- 十分なハードウェアを用意できる
  - HBaseはHDFS上で動く以上、多くのノードを使うことが前提条件
    - 最低限6 Nodeは必要
      - 5 DataNode + 1 MasterNodeは必要（MasterNodeも3つは欲しいのでそうなると8つ）

## Data Model

- Row
- Column
- Timestamp
- Cell
  - CellとはRow, Column, Timestampで指定されるkeyとvalue両方を合わせたもの

### Column

ColumnはColumn FamilyとColumn Qualifierからなる。

#### Column Family

Column FamilyはColumnのGroupingである。
特徴はColumn FamilyごとにStoreを用意するため物理的な保存方法が異なることである。
アクセスパターンが違う、アクセスコントロールを実施したいなど特定のユースケースで使われるべきである。

#### Column Qualifier

Columnの指定子。


### Timestamp

UnixTimestamp x 1000.
指定することもできる。

#### Version

HBaseはマルチバージョン管理ができる。デフォルトで保存されるVersionの最大数は1。
最大数を超えたputはCompaction時に掃除される。それまではScanでオプション指定することで取得可能。


## Architecture

### Rowkey distribution

HBaseはrowkeyの範囲に基づいてシャーディングする。
Rowkeyは一定の範囲ごとにRegionに分けられる。
この分割は事前に設定しておくか、Splitによって決定する。
SplitはRegionが一定以上データ量を超えたときに発生する。ちょうど真ん中程度のkeyで二つのRegionに分割される。


### Storage

以下の階層構造を持って保存される
- Table
  - Region (複数)
    - Store (per column family)
      - MemStore
      - StoreFile (複数)

#### MemStore

HBaseではPutされたデータはまずMemStoreに保存される。
MemStoreは一定量（デフォルトで64MiB程度）たまると、StoreFileとしてFlushされる。
MemStoreはMSLAB（MemStore Local Allocation Buffer）と呼ばれるバッファでデータを保存する。
MSLABは設定次第でoff-heapになる。

#### StoreFile

StoreFileは実装としてはHFileというフォーマットに従い保存される。
HFileは以下の構造が並んでいる。
- Scanned block section
- Non-scanned block secion
- Load-on-open section
- Trailer

#### Scanned block section

実際のデータが入っているSectionである。
データをBlockという単位（64kiBくらい）に分けて保存される。
これらがおおよそ並んでいる構造をとる（実際にはLeaf indexやBloom blockも含む）。

#### Non-Scanned block section

メタデータや中間indexが含まれる。

#### Load-on-open section

indexなど、まず読み込むべきデータが含まれる。
- Root Data Index
  - あるkeyから始まるものがどのoffsetのBlockに保存されているかを持つ
  - 2階以上のindexを持つ場合はBlockではなく次のindexを指す
- Bloom index
  - Bloom filter
    - Bloom filterはあるデータが集合に含まれるかを返す確率的データ構造である
      - 偽陰性の結果を返さないため、GETに用いられる。Scanでは不要

#### Trailer

各SectionのOffsetやHFileのVersion情報を持つ。

### Compaction

StoreFileはflushのたび増えていく。一定数を超えるとCompactionが発生する。
CompactionはStoreFileの大きさや数によってどういう戦略をとるかが変化するが、
おおよそStoreFileが増えるとCompactionが発生すると思ってよい。
デフォルトでStoreFileが3つ以上になるとCompactionする。
Compactionでは古いVersionのデータの消去やtombstone markerの除去も行われる。

## Interface

以下がclientのinterfaceとして使える。

- Rest
- Thrift

### JDK Versions

To run HBase server (and maybe Java API too),
you can use Java8 or Java11.
- See https://hbase.apache.org/book.html#basic.prerequisites.

Currently, JDK17 is not supported but there is a plan to support it.
See https://issues.apache.org/jira/browse/HBASE-26038.

### Refernce

- https://hbase.apache.org/book.html
- https://static.googleusercontent.com/media/research.google.com/ja//archive/bigtable-osdi06.pdf

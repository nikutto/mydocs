# Database的なソフトウェア

Database（またはクエリエンジンなど）として使われているソフトウェアについてまとめる。

## RDBMS系

### MySQL

- オープンソースRDBMS
- Oracle社により開発、流通、サポート
- ストレージエンジンはInnoDBが主
  - 行ロックが可能

### Maria DB

- MySQLと完全互換
- GPLライセンス
- OracleがMySQLを買収したときに創設者らが作成

#### MySQLとの違い

- JSON用の型がNativeで存在しない
  - JSONというデータ型があるが、これはLONGTEXT
- MariaDBはビューの最適化（不必要なテーブルを見ない）があるらしい
- スレッドのPooling
  - MySQLはエンタープライズ版のみ
  - MariaDBは標準機能
- MariaDBはColumnStoreというストレージが使える
  - ColumnStore: カラム型データベースを作れる
  

### Oracle Database

- 有料ライセンス制
- よくしらない

### PostgreSQL

- データ量が多かったり複雑なクエリを打つ場合はつよいらしい
- よくしらない

### SQL Server

- Microsoftが作ったRDBMS
- 有料ライセンス
- よく知らない

## Data warehouse系のSQLベースクエリエンジン

### Hive

- HDFS上などで動くクエリエンジン
- HiveQLというSQLで動く
- Clientは以下の言語を話せる
  - JDBC
  - ODBC
  - Thrift

### Presto

- HDFS上などで動くクエリエンジン
- Hiveと比べるとメモリを使う代わりに高速
- 商標の問題でtrinoと呼ばれることもある
  - PrestoSQLが商標的にだめでリブランドしたらしい

### Spark

- HDFS上などで動くクエリエンジン
- Prestoと同様だが、Pythonなどから触りやすいAPIになっている

### Flink

- しらない
- ここにいるべきものじゃないかも

### Kyuubi

- クエリエンジンではなくSQLクエリエンジンのgateway
- 以下のエンジンのgatewayになる
  - Hive
  - Spark
  - Trino
  - Doris
  - Flink
- Driverは以下
  - Thrift
  - JDBC
  - ODBC
  - pyhive
  - JayDeBeAPI

### BigQuery

- Google Cloud Storage上にSQLクエリを打てる
- 以下のデータフォーマットがサポート
  - CSV
  - JSON
  - Avro
  - ORC
  - Parquet
  - etc.

### Amazon Redshift

- Redshift Managed Storegeをストレージにするらしい

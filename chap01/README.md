# 第1章 データベースとSQL
## 1-3. SQLの概要
### 標準SQL
* SQL(Structured Query Language)はリレーショナルデータベースを操作するための言語。
* もともと、データベースから効率よくデータを検索する（取り出す）ことを目的に開発された言語ですが、現在ではデータの登録や削除といったデータベース操作のほとんどをSQLで行うことができる。

### SQLの文とその種類
* SQLは、いくつかのキーワードと、テーブル名や列名などを組み合わせた1つの文として、操作の内容を記述する。

#### DDL(Data Definition Language)
* データを格納する入れ物であるデータベースやテーブルなどを作成したり削除する。
  * `CREATE`: データベースやテーブルなどを作成する
  * `DROP`: データベースやテーブルなどを削除する
  * `ALTER`: データベースやテーブルなどの構成を変更する

#### DML(Data Manipulation Language)
* テーブルの行を検索したり変更したりする。
  * `SELECT`: テーブルから行を検索する
  * `INSERT`: テーブルに新規行を追加する
  * `UPDATE`: テーブルの行を更新する
  * `DELETE`: テーブルの行を削除する

#### DCL(Data Control Language)
* データベースに対して行なった変更を確定したり取り消したりする。他にも、RDBMSのユーザーがデータベースにあるものを操作する権限の設定も行う。
  * `COMMIT`: データベースに対して行なった変更を確定する
  * `ROLLBACK`: データベースに対して行なった変更を取り消す
  * `GRANT`: ユーザーに操作の権限を与える
  * `REVOKE`: ユーザーから操作の権限を奪う


## 1-4. テーブルの作成
### データベースの作成
```sql
CREATE DATABASE <データベース名>;
```
* `CREATE DATABASE shop;` でデータベースを作成する。

### テーブルの作成
```sql
CREATE TABLE <テーブル名>
(
    <列名1> <データ型> <列の制約>,
    <列名2> <データ型> <列の制約>,
    <列名3> <データ型> <列の制約>,
    <列名4> <データ型> <列の制約>,
    ...
    <このテーブルの制約1>, <このテーブルの制約2>,...
);
```

* 以下のようにして `Shohin` テーブルを作成する（`create_table.sql`）
```sql
CREATE TABLE Shohin
(
    shohin_id CHAR(4) NOT NULL,
    shohin_mei VARCHAR(100) NOT NULL,
    shohin_bunrui VARCHAR(32) NOT NULL,
    hanbai_tanka INTEGER,
    shiire_tanka INTEGER,
    torokubi DATE,
    PRIMARY KEY (shohin_id)
);
```

## 1-5. テーブルの削除と変更
### テーブルの削除
```sql
DROP TABLE <テーブル名>;
```
> [!WARNING]
> 一度DROPしたテーブルは元には戻せない。

### テーブル定義の変更
```sql
-- 列の追加
ALTER TABLE <テーブル名> ADD COLUMN <列の定義>;

-- 列の削除
ALTER TABLE <テーブル名> DROP COLUMN <列名>;
```

* 以下のSQLで `Shohin` テーブルにデータを追加する（`insert_into.sql`）
```sql
-- DML: データ登録
BEGIN TRANSACTION;

INSERT INTO Shohin VALUES ('0001', 'Tシャツ', '衣服', 1000, 500, '2009-09-20');
INSERT INTO Shohin VALUES ('0002', '穴あけパンチ', '事務用品', 500, 320, '2009-09-11');
INSERT INTO Shohin VALUES ('0003', 'カッターシャツ', '衣服', 4000, 2800, NULL);
INSERT INTO Shohin VALUES ('0004', '包丁', 'キッチン用品', 3000, 2800, '2009-09-20');
INSERT INTO Shohin VALUES ('0005', '圧力鍋', 'キッチン用品', 6800, 5000, '2009-01-15');
INSERT INTO Shohin VALUES ('0006', 'フォーク', 'キッチン用品', 500, NULL, '2009-09-20');
INSERT INTO Shohin VALUES ('0007', 'おろしがね', 'キッチン用品', 880, 790, '2008-04-28');
INSERT INTO Shohin VALUES ('0008', 'ボールペン', '事務用品', 100, NULL, '2009-11-11');

COMMIT;
```

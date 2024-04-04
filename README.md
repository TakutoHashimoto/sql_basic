# sql_basic
## 資料
* [SQL 第2版 ゼロからはじめるデータベース操作](https://www.shoeisha.co.jp/book/detail/9784798144450)


## 環境構築
テキストにはWindows向けの方法だけ書いているので、Mac向けの環境構築を実施した方法を書く。
以下、Homebrewでコマンドを使ったインストールの方法を書く。

### Homebrewを確認してアップデートする
```
% brew --version  # Homebrewのバージョンを確認する
% brew update  # Homebrew自体の更新
% brew upgrade  # Homebrewで管理しているパッケージの更新
% brew cleanup  # 不要なformulaeを削除する
% brew services list  # brew上で起動しているサービス一覧を確認する
```

### インストールできるPostgreSQLのバージョンを確認する
```
% brew search postgresql
```
今回はバージョン14をインストールする（初めバージョン16をインストールしたが、psqlを起動したときにバージョン14でないと起動できないと出てきたため。理由は不明。あとで調べる。）

### Postgresqlをインストールする
```
% brew install postgresql@14
```
`@` 以降を省略すると、最新版がインストールされる。

### バージョンを確認する
```
% psql --version
```
ここで、`zsh: command not found: psql` が出る場合は、以下のコマンドを実行してパスを通す。
```
% echo 'export PATH="/opt/homebrew/opt/postgresql@16/bin:$PATH"' >> ~/.zshrc
% source ~/.zshrc
```
### 参考にしたサイト
* [公式ドキュメント](https://www.postgresql.org/download/macosx/)
* [MacでPostgreSQLをインストールする](https://zenn.dev/eguchi244_dev/articles/sql-postresql-install-20230620)
* [PostgreSQLのインストールと基本的な使い方-Mac編-](https://qiita.com/Tiger-Kid/items/8cacb8b89a2d201f4cf8)

<details>
<summary>インストールしたときのターミナルのログ</summary><div>

```
% brew install postgresql@14  
==> Downloading https://ghcr.io/v2/homebrew/core/postgresql/14/manifests/14.11_1
Already downloaded: /Users/takuto/Library/Caches/Homebrew/downloads/cc5ce32be3ad755d6853a306c7d20db8221cceaa63708acb1937567a4ab65fda--postgresql@14-14.11_1.bottle_manifest.json
==> Fetching postgresql@14
==> Downloading https://ghcr.io/v2/homebrew/core/postgresql/14/blobs/sha256:d49e8b487d6b7e696e74ebf7d0bddbb9b8c343d7
Already downloaded: /Users/takuto/Library/Caches/Homebrew/downloads/695bc5763b3ab3f65140681669c474e2b18ff40d6ff809534e5436f2105b6d90--postgresql@14--14.11_1.arm64_sonoma.bottle.tar.gz
==> Pouring postgresql@14--14.11_1.arm64_sonoma.bottle.tar.gz
==> Caveats
This formula has created a default database cluster with:
  initdb --locale=C -E UTF-8 /opt/homebrew/var/postgresql@14
For more details, read:
  https://www.postgresql.org/docs/14/app-initdb.html

To start postgresql@14 now and restart at login:
  brew services start postgresql@14
Or, if you don't want/need a background service you can just run:
  /opt/homebrew/opt/postgresql@14/bin/postgres -D /opt/homebrew/var/postgresql@14
==> Summary
🍺  /opt/homebrew/Cellar/postgresql@14/14.11_1: 3,319 files, 46MB
==> Running `brew cleanup postgresql@14`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
```

</div></details>

## 基本的な使い方
### 起動
```
brew services start postgresql@14
```

### データベースの一覧を確認する
```
% psql -l
List of databases
   Name    | Owner  | Encoding | Collate | Ctype | Access privileges 
-----------+--------+----------+---------+-------+-------------------
 postgres  | ***    | UTF8     | C       | C     | 
 template0 | ***    | UTF8     | C       | C     | =c/***        +
           |        |          |         |       | ***=CTc/***
 template1 | ***    | UTF8     | C       | C     | =c/***        +
           |        |          |         |       | ***=CTc/***
(3 rows)
```
* `Owner` に書いてある名前は、DBに接続するときのロール名として使う。

### データベースに接続する
```
% psql -h localhost -p 5432 -U *** -d postgres
```

### データベースを作成する
```
postgres=# CREATE DATABASE test;  # SQLの実行
postgres=# \l  # データーベース一覧を表示する
postgres=# \c test  # データーベースを切り替える
```

### 終了
* データベースの接続を終了する
```
postgres=# \q
```

* Macのバックグラウンドで動いているPostgreSQLを終了する
```
% brew services stop postgresql@14
```
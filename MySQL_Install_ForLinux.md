# MYSQL再入門 - インストール

LinuxにM公式のMySQLをダウンロードし、インストールする手順を記します。

最新バージョンは"MySQL Community Server 8.0"です

## インストール(debian、Ubuntu)

1. Mysql公式から.debファイルを落としてくる(wgetでOK）
`https://dev.mysql.com/downloads/repo/apt/`
`dpkg -i **落としてきたファイル名**`

途中質問されるが基本そのまま→OKを選択しておく

2. パッケージのアップデートとインストール

`sudo apt-get update` (必須)

`sudo apt-get install mysql-server -y`

3.途中でrootパスワードの設定を求められるので忘れないように。

確認のため2回タイプする。

**ここまででインストールは完了。自動的にmysqlサービスが起動されます**

## サーバーログイン

1. mysqlサーバーにログインする（要パスワード）

`mysql -u root -p`

2. サーバーの停止と起動

- サーバーの停止は
`sudo service mysql stop`
- サーバーの起動は
`sudo service mysql start`

### MYSQLを削除する
1.パッケージを削除

`sudo apt-get remove mysql-server -y`

一緒にインストールされたソフトウェアも削除する場合は

`sudo apt-get autoremove`

## インストール(Redhat,CentOS)

1. Mysql公式から.debファイルを落としてくる
`https://dev.mysql.com/downloads/repo/yum/`

「Red Hat Enterprise Linux ~~ RPM Package」のDownloadをクリックしてyumファイルを落としてくる	

`sudo rpm -Uvh --落としてきたファイル名--.rpm`

細かいバージョンが指定できるがここではスキップ

2. MYSQLのインストール

`sudo yum install mysql-community-server`

3. MYSQLサーバーの起動

`sudo service mysqld start`

- ステータス確認するには `sudo service mysqld status`

4.MYSQサーバーの初期化

**サーバーの最初の起動時に、サーバーのデータディレクトリが空の場合、次のことが起こります。**

サーバー初期化・SSHの証明書キー生成・validateプラグインのインストール・rootアカウントがSUで作成

```cmd
1.ログ参照
sudo grep 'temporary password' /var/log/mysqld.log

2. 初期パスワードでログインします
mysql -u root -p

3. 新しいパスワードを設定
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4';
```
**[注意]8文字以上で英数大文字小文字と記号が含まれていないとポリシー違反で弾かれてしまいます**

### MySQL5.6のみの作業

`mysql_secure_installation --use-default`　を実行してrootパスワードを設定してください

**(注意)5.7以降をインストールした後に実行しないでください。**


# MariaDBをインストールする (centOS)

>MySQL:オープンソース。だがOracle社によって開発・管理されているDB。圧倒的なシェアを誇る。汎用性が高い

>MariaDB:SQLからフォークした互換DB。完全なオープンソースでOracle社が関与していない。セキュリティが強化されGoogleにも採用

0. 既に古いmariadbがインストールされているか確認

`$ rpm -qa | grep -i "mariadb"`

あれば削除。

1. yumパッケージからインストール

```cmd
$ sudo yum update

$ sudo yum install -y mariadb mariadb-server
```

1. 1 または公式HPからレポジトリを取得してインストール
   
`https://downloads.mariadb.org/mariadb/repositories/#mirror=yamagata-university`

 `/etc/yum.repos.d/`に移動後、--好きな名前--.repoで保存後、yum installコマンドで入れる

1. mariaDBサーバーを立てる

`$ systemctl start mariadb`

3. 初期パスワード・初期設定をする

`mysql_secure_installation`
指示に従ってパスワードを設定・あとはすべてy

4. 設定ファイルを編集（文字化け対策）

`/etc/my.cnf.d/server.cnf`内に

以下を追加して保存
```json
[client]
default-character-set = utf8
[mysqld]
character-set-server = utf8
```

5. サーバー再起動

`$ systemctl restart mariadb`

6. mariadbにログイン(mysql -u root -p)後、

`> show variables like "chara%";`　と入力。

7. character-setがutf8になっていることを確認


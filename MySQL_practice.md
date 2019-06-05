# MySQL基礎
## 基本

## 記述とその意味の理解

```sql
SELECT どのカラムのデータを？
FROM どのテーブルから？
WHERE どこのレコードを？（条件指定）
```
## 実際に取得してみる
*kakeibo* テーブルから カラム *category* が**雑貨**のレコードを**すべて取得**します
```sql
SELECT * (すべてのカラムを取得する)
FROM kakeibo
WHERE category = "雑貨"; 
```


### データの型に注意

||     ||
|:---|:-----|:-------|
|数値|1|そのまま|
|文字|あああ| ""で囲む|
|日付|  2017-07-1 | ""で囲む|

## 比較演算子
* a < b :: aがbより小さい

* a <= b :: aはb以下

* a > b :: aがbより大きい

* a >= b :: aはb以上

>日付型のデータ
* <= "2017-08-01" :: 2017/08/01 より以前



## LIKE演算子
```sql
SELECT *
FROM kakeibo
WHERE name LIKE "% ケーキ %";
```
% =ワイルドカードが指定できる

## NOT演算子
```sql
SELECT *
FROM kakeibo
WHERE NOT name LIKE "%ケーキ%";
```
条件を満たさないデータを取得

### NULLデータを取得
```sql
SELECT * FROM kakeibo 
WHERE price IS NULL;
```
NULLのデータを取得

反対に
```sql
SELECT * FROM kakeibo 
WHERE price IS NOT NULL
```
NULLでないものを取得する際に使う

## AND演算子

```sql
SELECT * FROM kakeibo 
WHERE name = "Roboto"
AND category = "レジャー";
```
name が **roboto** かつ **category**が「レジャー」を満たすデータを取得する

## OR演算子
```sql
SELECT * FROM kakeibo 
WHERE name = "android"
OR name = "doroido";
```
name が **android** または **doroido** を満たすデータを取得する

## ORDER BY
取得した結果(price)を並び替える
SQL文の末尾に記述するため、WHEREとも併用可

```sql
SELECT * FROM kakeibo 
ORDER BY price DESC;
```
- ASC で昇順 `1,2,3,4,5..`

- DESC で降順 `5,4,3,2,1..`

---

## LIMIT
データの数を制限します

これも末尾に記述するためWHEREとも併用可

```sql
SELECT * FROM kakeibo 
LIMIT 10;
```
kakeibo のデータを**10件だけ**取得します 

末尾に記述するためWHEREとも併用可
 
## DISTINCT （重複を除外する）
重複したデータを除いて取得します
```sql
SELECT DISTINCT(goods)
FROM kakeibo;
```

## 四則演算
```sql
SELECT price * 1.08
FROM kakeibo
```
**price** データを取得し、消費税を含めた結果を出力します

```sql
SELECT SUM(price)
FROM kakeibo
```
SUM関数を使って、priceの合計を取得します

```sql
SELECT AVG(price)
FROM purchases
WHERE character_name = "にんじゃわんこ";
```
AVG関数を使って、「にんじゃわんこ」がprice一回あたりの平均値を取得します

**COUNT(koko)**

カラム"koko"に保存されているデータ数を取得します。
nullは無視されます

**COUNT(*)**

テーブル全てのデータ数を取得します。
nullも全て含めます

**MAX(koko)**

もっとも大きいkokoカラムの値を取得します。

**MIN(koko)**

もっとも小さいkokoカラムの値を取得します。

---

## GROUP BY（グループ化）
```sql
SELECT sum(price),date
FROM kakeibo
GROUP BY date;
```
SUMでpriceの合計・dateを取得する↓
dateでグループ化することで「日付ごとのpriceの合計値」が取得できる。

```sql
SELECT SUM(price),purchased_at
FROM kakeibo
WHERE character_name = "にんじゃわんこ"
GROUP BY purchased_at
```
* SELECT dateを取得
* WHERE "にんじゃわんこ"のデータを取得
* GROUP BYでグループ化
* MAX で最大値取得

「にんじゃわんこ」のprice合計値を取得し、さらに日付ごとにグループ化する


## HAVING

```sql
SELECT SUM(price),date
FROM kakeibo
GROUP BY date
HAVING SUM(price) > 2000;
```
priceの合計値をdateでグループ化した後、**HAVING** を使って更に *priceの合計値が2000以上のレコードを抽出*する。

# ユーザーの追加・権限

### ユーザー名とホストの確認 
```sql
select user,host FROM mysql.user;
```
### 権限の確認方法
```sql
SHOW GRANTS FOR 'ユーザ名'@'ホスト名';
```

## ユーザーの追加

すべてrootユーザーで操作してください。

```sql
CREATE USER hogeuser IDENTIFIED BY 'password';
```
hogeuserというユーザーを作り、パスワードはpasswordを指定（省略も可能）

### ユーザーに権限を追加

ユーザーを作成しただけでは権限が無いので(USAGE)何もできません。

以下のコマンドで権限を付与します。

```sql
-- 各権限を持つユーザーの作成
GRANT 権限内容(all) ON 権限対象(database名または*).* TO ユーザー@ホスト名 identified BY パスワード; 

-- すべての権限を付与（グローバル権限）
GRANT 権限 ON *.* TO hogeuser:

-- データベース操作権限（特定のデータベースにのみ権限）
GRANT 権限 ON db_name.* TO user;

-- テーブル操作権限（特定のテーブルのみ権限）
GRANT 権限 ON db_name.table_name TO user;
```

## 権限

* all ・・・ 権限の付与以外のすべて
* alter ・・・ テーブル変更
* create ・・・ テーブル作成
* drop ・・・ テーブル削除
* index ・・・ インデックス作成、削除
* select ・・・ select文
* insert ・・・ insert文
* update ・・・ update文
* delete ・・・ delete文
* showdatabeses ・・・ SHOW DATABESESを可能に
* showview ・・・ SHOW CREATE VIEWを可能に
* usage ・・・ 権限なし

```sql
-- 例：USERを作成、データベースDB_BASICに対して、select, insert, update, deleteのみ許可する。
grant select, insert, update, delete on DB_BASIC.* to USER@localhost identified by 'password';
```

### 権限を確認する

```SQL
SHOW GRANTS FOR 'USERNAME'@'HOST';
    ユーザーに設定した権限を表示

SHOW GRANTS;
    データベース内のすべてのユーザー権限を表示
```

### 権限の削除

```sql
revoke 権限 on *.* from ユーザー名@ホスト;
```

### 権限を再読込

flush privilegesで権限を再度読み込むことで設定が反映されます。
```
flush privileges;
```

## rootパスワードが不明なとき

* MySQL5.7インストール時、rootパスワードは聞いてこない。管理者権限でログインする必要がある
```sql
sudo mysql -u root -p
```

### 1. 初期設定に入る

sudo mysql_secure_installation でここでパスワードが聞かれる？ので設定する。
VALIDATE PASSWORD plugin（文字数制限）はNOと答える。

### 2. skip-grant-tables を設定する

/etc/mysql/my.cnf 内 [mysqld] に skip-grant-tables を指定するとパスワード無しログインできる。
```
[mysqld]
skip-grant-tables
```
保存したら再起動。sudo無しでログインできるか確認。
```
sudo server mysql restart
```

## ユーザーの削除
```sql
DROP USER 'hogeuser'@'localhost';
```

## データベース操作コマンド

最初にMySQL内に新しくデータベースを新規作成し、USEで使用します。

### データベースの新しく追加

`create databases test_db;`

### データベース一覧の表示

`show databases;`

### データベース選択

`use test_db;`


## レコードの追加

### テーブルの作成

```sql
CREATE TABLE [dbname](
    id int,
    name varchar(10),
    other varchar(20)
);
```
* 全カラムの一覧表示

desc [TableName];

### レコードを挿入

* 全てのカラムにレコードを挿入する

```sql
INSERT INTO [dbname] VALUES(
    1, 'hogeSAN', 'hogehoge'
);
```

* 特定のカラムを指定してレコードを挿入

```sql
INSERT INTO [dbname](
    id,name,other) VALUES(
        1, 'hogeSAN', 'hogehoge'
    );
```

### すべてのテーブル内容を一覧表示する

```SQL
SELECT * FROM [dbname];
```

## DBバックアップ（すべて）
```sql
mysqldump --opt --all-databases --events --default-character-set=binary -u ユーザ名 --password=パスワード > 保存先hoge.sql
```

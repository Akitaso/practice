## htmlspecialchars

### フォームなどを送信後に文字列を出力する際に特殊な文字列を通常の文字列に変換、悪意のあるスクリプトを防止。

```php
function h($s) {
  return htmlspecialchars($s, ENT_QUOTES, 'UTF-8');
}

# example
echo h($entity);
```

### mb_substr

#### 日本語文字列を決められた数だけ切り出す

```php
$example=abcd123;
echo mb_substr($example,1,5,"UTF-8");

#結果	bcd12
```

### PDO->SQLサーバー接続構文
```
$dbh = 'mysql:dbname=データベース名;host=サーバー名;charset=utf8mb4';
$USERNAME="ユーザー名";
$PASSWORD="パスワード";

try {
$dbh = new PDO($dsn, $USERNAME, $PASSWORD,
    [PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION]);
	
	//ＤＢから値を取ったりデータを挿入する処理をここに書く
	
	}catch (PDOException $e) {
	
		//ここからエラー処理
		
        echo "接続失敗: " . $e->getMessage() . "\n";
        exit();
	}
	
	//ここから下はHTML文だとブラウザに示す
	
	header('Content-Type: text/html; charset=utf-8');
```

## SQLクエリーをPHPで実行するPDO Steatment

### PDO::query

#### 入力値がとくにないSQLクエリーの場合

1回だけしか実行しないクエリーならたぶんこれだけで良い

`$stmt = $dbh->query('SELECT * FROM tests');`

取得するのならfetchへ。

### PDO::exec

#### 入力値がとくにないけどINSERTやUPDATEだけしたい場合
なんかの件数取得や集計したい場合などに

`$count = $dbh->exec('UPDATE users SET age = age + 1');

### PDO::prepare

#### プレースホルダーを作りそこに値（プリペアドステートメント）を入れて実行する場合（３ステップ）

##### プレースホルダー，プリペアステートメント

* 疑問符
最初は1番めから ? を付ける
`$stmt = $pdo->prepare('SELECT * FROM users WHERE gender = ? AND age = ?');`

1番目の？に数値や変数を入れる

`$stmt->bindValue(1, 'male');`


* 名前付き

：を頭に付け、名前を付けてあげる

`$stmt = $dbh->prepare('SELECT * FROM users WHERE age = :age AND gender = :gender');`


bindValue(:プレースホルダーの名前，入れる値か変数，データ型の指定）
```php
$stmt = $dbh->bindValue(':age',入れる数値や変数,PDO::PARAM:INT);
$stmt = $dbh->bindValue(':gender',$gender,PDO::PARAM:STR);

```
データ型の指定はだいたいこれ

* PDO::PARAM:STR	CHAR,VARVHAR型を表す

* PDO::PARAM:INT	INTEGER型を表す

##### SQL文を実行

`$stmt->execute();`

### prepare->execute（２ステップ）で実行する場合

* ただし、NULL値以外はすべてSTR扱いに。既に定義したBindValueの値もすべて無視されるので注意
```
// 疑問符
$stmt = $dbh->prepare('SELECT * FROM users WHERE city = ? ');
$stmt ->execute( [$city]);

// 名前付き
$stmt = $dbh->prepare('SELECT * FROM users WHERE city = :city ');
$stmt ->execute(['city' => $city ]);
```

## Fetch,FetchAll

### ＤＢから値を取得する

**Fetch**
PDOステートメントから1行取得して1次元配列にします 

PDOフェッチオプション
FETCH_ASSOC	結果をカラム名の添字で返します
FETCH_NUM		結果を0から始まる番号で返します

```
$row = $stmt->fetch(PDO::FETCH_ASSOC);

//取得したデータをforeachで1行ずつずらしながら出力します。

foreach($stmt as $row){
	printf($row['name'] , $row['city']);
	}
```

結果：
```php
array(
 ["ID"]=> int() 
 ["TITLE"]=> string() "。。。。" 
 ["DETAIL"]=> string() "本文テストだよ"
 ["created_at"]=> string(19) "2019-06-05 14:57:16" 
 ["updated_at"]=> string(19) "2019-06-18 10:30:25" 
)
```

**FetchAll**
PDOステートメントから全件取得して２次元配列にします
```
$row = $stmt->fetchAll();

// 結果

array{ [0]=> array(5)
{ ["ID"]=> int(1) 
  ["TITLE"]=> string(45) "タイトル" 
  ["DETAIL"]=> string(62) "本文テストだよ"
  ["created_at"]=> string(19) "2019-06-05 14:57:16"
  ["updated_at"]=> string(19) "2019-06-18 10:30:25"  
} [1]=> array(5) 
  { ["ID"]=> int(2) 
  ["TITLE"]=> string(4) "test" 
  ["DETAIL"]=> string(6) "detail" ["created_at"]=>...
```

**取得した結果をforeachでまわす**

```php
foreach( $row as $list ){
	echo $list['ID'];
	echo $list['NAME']
}
```


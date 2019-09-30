* isset
変数に中身が「入っているか」確認する。中身が数字の0でも入っていると判断する

`if(!isset($value)):`

* empty
変数に中身が「入っているか」確認する。変数の中身が0だった場合でも空だと判断する

`if(empty($value)):`

ただし、NULLはどちらの関数も空だと判断されます

* htmlspecialchars

XSS（クロスサイトスクリプティング）対策の為に特殊文字のみをエスケープ処理させる

`htmlspecialchars($str, ENT_QUOTES, 'UTF-8');`

その都度エスケープ関数を挿入するのは面倒なのでユーザー定義関数にして省略する方法がよく使われている
```php
function h($str){
	ehcho htmlspecialchars($str, ENT_QUOTES, 'UTF-8');}

	h($hoooge);
```
* 従来のinputボタンから汎用性の高いButtonタグへ

`<button type="submit" name="btn_submit" value="btn_submit">送信ボタン</button>`

typeにsubmitと指定することでinputと同じ効果のボタンが配置できる。ちなみに何も指定しないとデフォルトでsubmitになる
inputタグとは違い、文字列やCSSで装飾することが容易になる



* ヒアドキュメント（EOTやEODどっちも同じ）

長いコードや文字列などを出力したり変数に入れたりしたいときに使う。記述の仕方に注意。
```php
$str = <<<EOT
<h1>Midashi</h1>
<h2>Matamidashi</h2>
<p>koreha,reibun desu.</p>
EOT;
```
`<<<EOT`でヒアドキュメント開始位置を指定し、EOT;で終了位置を決定。大文字小文字を区別するので統一する。

このとき、**EOTの前後にスペースやコメント等があると記述エラーとなるの要注意。**

* 定数の定義
あとから中身を変更する予定の無い変数（定数）を定義したいときに使う

  * const
クラス内で定義する時に使う
```php
class ConstTestClass{
	const VALUE1 = "hogee";
	const VALUE2 = "hogeeee2";
...
```
**注意　クラス外で定義しようとするとエラーが発生します**
* define 同じく後で変更できない値を定義する
大文字小文字を区別するので統一すること。
```php
define('NAME',"nanashisan");
define('NICKNAME',"nanashin");

echo NAME . NICKNAME;
```
**使う時$マークをつける必要はありません**

* チェックボックス（複数）の値を受け取る

チェックボックスの内容が入った配列は直接エスケープ関数で処理できないため、is_array(配列か否か)で判定しforeachで１つづつ回してエスケープ処理させる。

```php
foreach($array as $key => $value) {
    $array[$key] = htmlspecialchars($value);
}
```

* 配列を一行で出力したい場合はimplode関数を使う。**implode(区切り文字,配列)**

```php
echo implode(" ,",$value);
```

# PHP開発には必須のComposerを入れよう

## ダウンロードとインストール

* Windows版の場合

https://getcomposer.org/download/ からセットアップ用のexeを落としてきてインストールに従って下さい。これで完了。

* Linux版の場合

Windowsと異なり、セットアップ用PHPをディレクトリに落としてきてそこからコマンドを実行していく流れになります。

👆のリンクにコードがあるのでコピペしてコンソールに貼り付けます。

Linuxの全ユーザーで使えるようにしたいのでGlobalでパスを通します。

```
mv composer.phar /usr/local/bin/composer
```
## インストール確認

これで`composer`コマンドを使う準備ができました。

以下のコマンドを端末に入力し、バージョン情報が出てくるか確認して下さい。

```
composer -v
```

出てこない場合はパスが通っているかもう一度確認。

## Composerを使おう

composerはまずcomposer.jsonファイルを作り、その内容を元にパッケージを管理します。

### パッケージのインストール

####  プロジェクトを作る＆インストール
```
composer init # プロジェクトファイルjsonファイル生成

composer require <packagename>:~0.02
```

多分よく使うコマンド。ディレクトリ内にcomposer.jsonが無い場合警告されるが自動的に作ってくれる。
**--dev** を後ろに付けると開発環境としてディレクトリ下にファイルがインストールされます。

この時、過去バージョンも指定したりできます。

## インストールしたパッケージをPHPで使う
vendor内にあるオートローダー(autoload.php)を呼び出し、useでパッケージ名を指定するとライブラリーが読み込まれ、使うことができる。

あとはパッケージのドキュメントを参考にしてください。

```php
# PHPファイルの先頭
require 'vendor/autoload.php';
use  package/package    #パッケージ名をuseで指定

$hello = new Package();
echo $hello;
```

## おまけ
* パッケージを取り除きます。やっぱりいらないと思ったら

`composer remove <package>`

* 別環境でも同じパッケージを入れたいなという時

composer.jsonを別環境にコピーしてから、
`composer install`

これで他のメンバーにも同じ環境が構築されます
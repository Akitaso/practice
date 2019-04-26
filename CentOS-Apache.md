## [Vagrant]秘密鍵と公開鍵を合わせる

1. PuttyGenを起動

2. 既存の秘密鍵を読み込み→vagrantのprivate_keyを選択

3. 「秘密鍵を保存」で新しく秘密鍵ファイル(ppk)を保存

4. パスフレーズなしでも大丈夫か？聞かれるので「はい」

5. PuttyでSSH接続する際に秘密鍵ppkファイルを指定してから接続

6. 秘密鍵で接続できたら成功 


## [Apache]ルートディレクトリ変更

デフォルトでrootディレクトリになっているのを一般ユーザーでもアクセスできるよう変更します。

予めホームディレクトリーに専用のディレクトリーを作成しておいてください

>ディレクトリ作成 : `mkdir DIRNAME`

1. homeディレクトリーに実行権限を付けてあげます(デフォだとApacheが読みに行けない）

`chmod 755 /home/USERNAME`

2. Apache設定ファイルからルートディレクトリをhomeディレクトリーに変更

`/etc/httpd/conf/httpd.conf`

3. いったんApacheを再起動

`service httpd restart`

4. 権限グループを変更してグループにApacheを変更します

`chown USERNAME.apache /home/USERNAME`




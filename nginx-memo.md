# nginx入門メモ

## バーチャルホストの設定

>バーチャルホストとは...
通常は一つのドメイン、一つのサーバーで動作するものを、一つのサーバーで複数のドメインを運用する技術です。

`/etc/nginx/conf.d/default.conf` 内Listenディレクティブを編集すれば簡単に増やすことが可能。

```
server{
        listen 80;
        server_name example2.com 
}
server{
        listen 80 default_server;
        #default_serverを指定するとデフォルトでバーチャルホストが設定されます。何も指定してない場合は一番上の設定が優先されます。
        server_name example.com;
}
```

設定を保存後、nginxを再起動。


## ファイルやディレクトリの所有者を変えるコマンド

`ls -l`コマンドで所有者、グループを随時確認しておきましょう。

### [chown] ファイルの所有者やグループを変更する

* ファイル所有者をrootに変更

`chrown root example.rb`

* 所有者・グループを共にrootに変更

`chown root:root example.rb`

* ディレクトリ所有権を変える

`chown -R apache:apache /var/wordpress`

'wordpress'以下の所有権をグループをapacheに変えます

### [chmod] ファイルのアクセス権限を指定する

* 文字列で指定

`chmod +(rwx) example`      権限(r,w,x)を属性を追加する

`chmod =rx example`         権限を読み出しと実行(r,x)だけにする

* 数値で指定
  * 所有者,グループ権限,その他の権限の区分に
  * r=4 w=2 x=1 をすべて足した数値で指定

`chmod 755 example`          所有者はすべて、グループとその他は読み書きだけ可能

`chmod 744 example`         所有者以外はすべて読み出しのみ

`chmod 777 example`         所有者もグループもすべて許可

### [umask] 既定の権限を指定する

ファイルやディレクトリ作成時にデフォルトで適用される権限を設定する

* 現在の権限値を確認するコマンド

`umask`

ファイルの権限は`0666`から`umask値`引いた数値

ディレクトリー権限は`777`から`umask値`引いた数値

`umask 0022`    デフォルトの権限が755になります。実行権限は付けることができません



# WSL入れてXAMPP化する手順まとめ

既にWSLはインストール済み前提で進めます。


## apache(2)とPHPを入れる

~省略~

## Apacheを起動してみる

Webサーバーを起動する
```
sudo service apache2 start
```

### Windows側
ブラウザーを起動し、アドレス`localhost`にアクセス

It Works!ページが出てきたら成功。

## Ubuntu起動時にApache2サーバーも起動させる

```
sudo systemd apache2
```
これでUbuntu起動と同時にサーバーも自動起動します。最初からしてくれ。

systemctlで解説してるサイトもありますが旧式コマンドのひとつです。

## DocumentRootの変更

初期設定では `/var/www/html` の中身がルートディレクトリになってます。

いちいちここに移動するのは面倒なので`シンボリックリンク`を設定してショートカットできるようにします。

### WindowsPC側のフォルダーを参照するように設定

#### Windows側

Userフォルダにhtmlというフォルダを作ってから
```
C:\Users\hoge\html\
```

#### Linux側
```
/var/www/ に移動した後
sudo ln -s /mnt/c/User/hogge/html
```

## phpファイルを表示させてみる

### windows側

先程作ったhtmlフォルダーにローカルで作ったphpファイルを入れる。ブラウザのアドレス欄に

```
localhost/（phpinfoでもいいので適当なphpで記述されたファイル）.php
```

表示できてたら成功。

ルートが間違ってたり見つからなかったりするとパーミッションエラーが表示されてうまくいかない。

## さいごに

こんな面倒くさいことしなくても数クリックでローカルサーバー立てられるXAMPPは偉大だね
# さくらのサーバーにSSHで入るための備忘録

```$ ssh [ten-system]@[ten-system].sakura.ne.jp```

でも一応はサーバーに入れますが、毎回とパスワードを聞かれるのでこれを省く公開鍵認証で接続します。
安全のためパスワード認証を禁止しているサーバーもあります


## コマンドで公開鍵を生成する
```
＄ssh-keygen
```
暗号化形式はデフォルトでRSAが指定される

↓
```
Generating public/private rsa key pair.
Enter file in which to save the key (~/.ssh/id_rsa):
```
鍵を保存する場所を聞かれるのでこのままEnter

↓

```Enter passphrase (empty for no passphrase):```

パスフレーズを聞いてくる
省略する場合は空白のまま。

↓

```passphrase again:```

もう一度入力

↓
```
Your identification has been saved in /~/.ssh/id_rsa.
Your public key has been saved in /~/.ssh/id_rsa.pub
```
鍵が生成された。id_rsaが秘密鍵、id_rsa.pubが公開鍵のこと。

## 作った鍵をサーバーに設置する作業

```
$scp ~/.ssh/id_rsa.pub [ten-system]@[ten-system].sakura.ne.jp:/home/[ten-system]/.ssh/authorized_keys
```
>SCP：ローカルのファイルをリモートディレクトリにコピーする

## 公開鍵のパーミッションを変更しておく

```
$chmod 600 .ssh/authorized_keys
```
> Chmod600:所有者のみ読み込み書き込み可能。他は禁止

## 接続
```
$ ssh [ten-system]@[ten-system].sakura.ne.jp
```
パスフレーズを設定した場合ここで聞かれるのでこの時入力。

成功すれば、次回からパスワード無しでサーバーに入れる
# SSH認証を公開鍵認証にする

パスフレーズは総当たりされやすいので代わりにセキュアな公開鍵を使ってサーバーにログインします。

---

## 鍵の種類

### 公開鍵
サーバー側に持たせておく鍵。 
複数の公開鍵を作る場合は、authorized_keysに追記していく。

### 秘密鍵
クライアント側で保有しておく鍵。誰にも渡してはいけない。

## 鍵を作成する

まずは通常ログインでクライアントからサーバーに接続します。

### 認証用鍵生成コマンドを入力

`$ ssh-keygen -t rsa -b 2056 -C "example@gmail.com"`

保存パス、パスフレーズを聞かれますが、何も入力せずにEnterを押下します。

* 一般ユーザーで作成した場合

/home/user-mei/.ssh/ に `id_rsa`(秘密鍵) `id_rsa.pub`(公開鍵)が生成されます。

* ルートユーザーで作成した場合

/root/.ssh/ 内に作成されます。

## 作成した秘密鍵をサーバーからクライアントに転送

1. id_rsaをauthorized_keysに変えます。catコマンドで内容追記
`cat id_rsa.pub >> authorized_keys`

2. パーミッションも今設定します
```
chmod 600 authorized_keys
chmod 700 ~/.ssh
```

3. 秘密鍵(id_rsa.pub)をクライアントPCに移動してください

>**scpコマンド**で転送する場合
`$ scp /home/user-mei/.ssh/authorized_keys 192.168.11.1:/root/.ssh`

>**TeraTermの場合**
ファイル->SSH-SCP 
Fromとtoに場所を入力して OK

`id_rsa.pub`は不要なので削除します。

4. 一旦ログアウトして公開鍵を用いてログインできるか確認

## SSHサーバーの設定
### 設定ファイルの場所：
`/etc/ssh/sshd_config`

```yml
# チャレンジレスポンス認証を無効
ChallengeResponseAuthentication no
# 空パスワード禁止
PermitEmptyPasswords no
# rootログイン禁止
PermitRootLogin no
# 特定ユーザー名の拒否
DenyUsers ec2-user
```

設定ファイルを編集を終えたら再起動することを忘れずに。
`$ systemctl restart sshd`


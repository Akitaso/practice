# Let's Encrypt

無料で利用できる自動化されていてオープンな認証局（ＣＡ）です。

GoogleがSSL化されたWebサイトをSEO評価を優遇すると発表しており、Webページのhttps化は必須の流れとなっています。

## [certbot-auto] 発行・更新ツールを入れる

Encryptが配布しているツール。証明書の取得やHTTPSサーバーを有効化する機能等があります

```
yum install epel-release
# EPELパッケージを有効化してインストールします
yum install certbot python-certbot-apache
# Apacheプラグインを使わない場合はcertbot-apacheは必要ないです
```

* 利用規約が出てくるのでAgreeでエンターキーを押下します

>firewalldでhttp通信を許可しないとこの証明書発行でこけます。

```
firewall-cmd --add-service=http --permanent
#success
firewall-cmd --add-service=https --permanent
#success
firewall-cmd --reload
#success
```

長い文章が流れてきたら証明書ファイルが生成成功です。ファイルは `/etc/letsencript`以下にあります。

### nginxサーバーの場合

生成した証明書をnginxで読み込む設定をします。

`/etc/php-fpm.d/www.conf` nginx設定ファイルを編集します。

```
server {
        listen  443 ssl;
        server_name     secure.example.net;
        # 証明書ファイルの場所を指定してあげる
        ssl_certificate         /etc/letsencrypt/live/secure.example.net/cert.pem;
        ssl_certificate_key     /etc/letsencrypt/live/secure.exampole.jp/privkey.pem;

        root                    /usr/share/nginx/html;
        access_log  /var/log/nginx/ssl-access.log  main;
}
```

nginxを再起動させます。

ブラウザからhttps://secure.example.netを開き、HTTPS通信が出来たか確認してください。

### apacheサーバーの場合

`sudo /usr/local/bin/certbot-auto --apache`

コマンドを実行すると証明書が自動的に発行され、apacheの設定も自動で編集されます。

## 証明書更新

Let's EncryptはSSL有効期限が3ヶ月です。定期的に更新する作業があります。

`certbot renew --dry-run`
証明書の自動更新テストをします。サーバーの接続や設定の確認をするためのコマンドです。

`certbot-auto renew --post-hook "systemctl restart nginx" ` 
 すべての証明書の有効期限をチェックし、30日以内に期限が切れるものを自動的に更新します。

cron等で定期的に実行させましょう

--post-hockは証明書更新後、一度だけ実行させるオプションです。この場合更新と同時にnginxの再起動をさせてます。

### 強制的に更新

`certbot-auto renew --force-renew`  更新日に関わらず強制的に証明書を更新します。余計な負荷がかかるので定期的に実行するべきではありません。

### 証明書の有効期限を確認

`certbot certificates` 有効期限確認をするcertbotコマンド

```
------------------------------------------------------------------------
Found the following certs:
  Certificate Name: blog.example.com
    Domains: blog.example.com

    Expiry Date: 2018-06-03 10:31:54+00:00 (VALID: 88 days)
    
    Certificate Path: /etc/letsencrypt/live/blog.example.com/fullchain.pem
    Private Key Path: /etc/letsencrypt/live/blog.example.com/privkey.pem
------------------------------------------------------------------------
```

Expiry Dateを見ると 2018年6月03日 。VALID: 88 daysは期限切れまで88日までということが確認できます


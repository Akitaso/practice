# nginxでPHPを動かすメモ

環境 : CentOS 7 PHP7.0 

## ダウンロードとインストール

https://nginx.org/en/linux_packages.html から　RHEL/CentOSを参照

レポジトリ追加後、インストールで安定版が入る。

### サービスの自動起動化と起動のしかた

#### 起動時にnginxサーバーも同時に起動

`systemctl enable nginx`

>created symlink from ... を確認

#### nginxを起動

`systemctl start nginx`

ホスト名にアクセスして **Welcome to nginx!** と表示されることを確認

* nginxの標準設定ファイル

/etc/nginx/conf.d/default.conf


* nginxのログファイルの場所

アクセスログ
/var/log/nignx/access.log 

エラーログ
/var/log/nignx/error.log 

## PHP-fpmのインストール

[apachefpm](/apache-php-fpm.md)に詳しく書いてあるので割愛。

UNIX user/groupの設定欄をapache->nginxに書き換えます。

設定ファイル編集 /etc/php-fpm.d/www.conf

```
; Unix user/group of processes
; Note: The user is mandatory. If the group is not set, the default user's group
;       will be used.
; RPM: apache user chosen to provide access to the same directories as httpd
user = nginx	#ユーザー名をapacheから書き換え
; RPM: Keep a group allowed to write in log dir.
group = nginx	#グループ名をapacheから書き換え
```

## nginxの設定を修正

/etc/nginx/conf.d/default.conf

に以下の箇所を書き換えます

```
location / {
	#ルートディレクトリの指定
	root	/home/vagrant/public_html;
	
	#PHPの追記
	index   index.html index.htm index.php;
	}

location ~ \.php$ {
	root           /home/vagrant/public_html; #locationと同じディレクトリ
	fastcgi_pass   127.0.0.1:9000;
	fastcgi_index  index.php;
	fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name; #fastcgiのパラメータ設定
	include        fastcgi_params;
	}
```

## SELINUXの無効化
権限が原因でアクセス拒否されるのはだいたいこいつのせい

ファイル編集 /etc/selinux/config

```
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=disabled
```
disableに書き換えサーバー再起動。


## HTML内でPHPを動かす


### php-fpm設定ファイルの書き換え

/etc/php-fpm.d/www.conf 内

コメントアウトを解除

`security.limit_extensions = .php .html`
>指定した拡張子以外は開けないようにする設定
### nginx設定ファイルの書き換え

/etc/nginx/conf.d/default.conf 内

```
 location ~ \.(php|html)$ {	#ここを書き換え
        root           /home/vagrant/public_html;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
```
サーバーとphp-fpmを再起動後、反映されているか確認。


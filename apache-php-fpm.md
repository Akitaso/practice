## ApacheをpreforkからMPM導入・変更

* Prefork…デフォルトの通信モジュールであり安定した通信が可能。メモリを多く消費する

* Worker…マルチプロセス・マルチスレッド。メモリ消費量が少ない

* eventMPM…2.4から導入されたマルチスレッド・マルチプロセス。CPU/メモリ消費量が少ない

パッケージインストールした場合Prefork、公式ソースコードはMPMがデフォルト。

### MPMの設定ファイルをvim等で編集

`/etc/httpd/conf.modules.d/00-mpm.conf`

> 重要なファイルなのでバックアップをとっておきましょう

```
# デフォルトでは以下が有効なのでコメントアウト
LoadModule mpm_prefork_module modules/mod_mpm_prefork.so

# こちらを有効にします
LoadModule mpm_event_module modules/mod_mpm_event.so
```

### apacheを再起動します

`systemctl restart httpd`

`systemctl status httpd`

エラーが出ていないことを確認後、インストール済みApacheモジュールを確認`httpd -M grep mpm`を実行

**mpm_event_module(shared)**が表示されればｏｋ

## php-fpmの導入

PHPはモジュール版としてApacheに入っていますが少し重いのでphpをスクリプトCGIとして動作させてパフォーマンスの向上を図ります。

FastCGIは通常のCGIよりパフォーマンスの向上が見込めるものになります。モジュール版と比較して、高負荷なサイトでのパフォーマンスアップを期待できます

### インストール

php7に対応したphp-fpmはremiレポジトリに含まれています。なので、

`yum -y install --enablerepo=remi,remi-php73 php-fpm`
> バージョンをよく確認してください

完了しました！と出ればOKです

`yum list installed | grep php-fpm` と打って

**php-fpm.x86_64**のパッケージが出ていることを確認してください。

### php-fpm設定ファイルを変える

php-fpmの設定は`/etc/php-fpm.d/www.conf`で行います。

```
; Unix user/group of processes
; Note: The user is mandatory. If the group is not set, the default user's group
;       will be used.
; RPM: apache user chosen to provide access to the same directories as httpd
user = apache

; RPM: Keep a group allowed to write in log dir.
group = apache
```

ここでapacheユーザーを指定してあげます。デフォルトでApacheになってますが、nginxの場合はここでnginxユーザーを指定してあげます。

あとはパフォーマンスに応じてpm.max_children等の数値を調整しますがここでは省略します。

### php-fpmの起動

php-fpmを起動してみます　`systemctl start php-fpm`

エラーが表示されず、Active(running) と出れば完了です


### 自動起動の設定

サーバー起動・再起動時にphp-fpmが自動で立ち上がるようにします

`systemctl enable php-fpm`

チェックのために

`systemctl is-enabled php-fpm`

**enabled**と表示されれば自動起動完了です。

## Apacheの設定変更

ここまでで、ApacheのPHPがモジュール型からCGI型へ変更されます。これに対応するためにApache側も設定変更を行います。

httpdの設定ファイル
`/etc/httpd/conf/httpd.conf`

ファイルの一番下に以下の文を追記します

```
# event MPM setting
<IfModule mpm_event_module>
        StartServers             2
        MinSpareThreads         25
        MaxSpareThreads         50
        ThreadsPerChild         50
        MaxRequestWorkers       50
        MaxConnectionsPerChild   0
       <FilesMatch \.php$>
                SetHandler "proxy:fcgi://127.0.0.1:9000"
       </FilesMatch>
</IfModule>
```

エラーチェックのため

`httpd -t` 最後にSyntan OKと出たらOK。

>Could not reliably determine the server's fully qualified domain name...と出ますがデフォルトでServerNameを入ってない為問題ありません
この警告を消す場合、`/etc/httpd/conf/httpd/conf`内に *SeverName example.com...* の記述がコメントアウトされている場所があるのでそのに適当な文字列を入れてあげれば警告は出なくなります。

### 再起動時 

最後にhttpdを再起動します `systemctl restart httpd`

再起動後 `systemctl status httpd`でエラーが出てないか確認

PHPモジュール型が残ってないか確認 `httpd -M | grep php`

何も表示されてなければ無効化できています。 


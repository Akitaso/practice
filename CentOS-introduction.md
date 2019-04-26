# CentOS 6 Apache を インストール・起動

## 日本国内ミラーサーバーにしてアップデートを高速化

1. リポジトリ設定ファイル`/etc/yum.repos.d/CentOS-Base.repo`を開く

* 念の為バックアップをとっておくこと

2. 次のようにurlを日本国内のミラーサーバーに書き換え

```
[base]
name=CentOS-$releasever - Base
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
baseurl=http://ftp.riken.jp/Linux/centos/$releasever/os/$basearch/
　　　　http://ftp.jaist.ac.jp/Linux/centos/$releasever/os/$basearch/
　　　　http://ftp.iij.ad.jp/Linux/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
priority=1

#released updates 
[updates]
name=CentOS-$releasever - Updates
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates
#baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
baseurl=http://ftp.riken.jp/Linux/centos/$releasever/os/$basearch/
　　　　http://ftp.jaist.ac.jp/Linux/centos/$releasever/os/$basearch/
　　　　http://ftp.iij.ad.jp/Linux/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
priority=1
```

## httpdのインストール
`yum -y install httpd`

### httpd(Apache)の起動と停止
* service httpd start : 起動
* service httpd stop : 停止
* service httpd restart 再起動
* service httpd graceful : 設定だけ再読込み

### 起動時にhttpdも同時に立ち上げる
`chkconfig httpd on`

### バージョン確認
`httpd -version`

ipアドレスを直接打てば、WEBページが表示されるようになる

### 設定ファイルの場所(CentOS)

`/etc/httpd/conf/httpd.conf`

ルートディレクトリ・バーチャルホストの設定はここで

`sudo apachectl configtest`

configファイルの文法チェック。最後にSyntax OK が出れば問題なし


## 最新版php のインストール

* 古いverのPHPが入ってないか確認

`yum list installed | grep php`

何も出なければ次へ、入っていたら `yum remove php-*`コマンドで削除してください

**CentOS標準リポジトリにはPHPがver5しか入ってません。**

外部のremiリポジトリを追加してそこからインストールすることになります

1. rpmからインストール
> remi's repository http://rpms.remirepo.net/

`yum -y install http://ftp.riken.jp/Linux/remi/enterprise/remi-release-7.rpm`


2. --enablerepoオプションを付けて一時的にrepoリポジトリ経由でphpとモジュールをインストール

`sudo yum -y install --enablerepo=remi,remi-php73 php php-mbstring php-pdo`

> php -m でインストールしたモジュール確認
3. 最後にphpバージョンを確認

```cmd
php -v
PHP 7.3.4 (cli) (built: Apr  2 2019 13:48:50) ( NTS )
Copyright (c) 1997-2018 The PHP Group
```

### モジュールの有効化(mbstring)
>php.iniの場所は `etc/php.ini`(デフォルト) 

```
zend.multibyte = On
zend.script_encoding = UTF-8
mbstring.language = Japanese
mbstring.internal_encoding = UTF-8
```

mbstringを有効化するために上記をコメントアウト

## Apacheの設定ファイルの場所
/etc/httpd/conf/httpd.conf





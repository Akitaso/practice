# CentOSにRubyをインストール


## rbenvを入れる 

1. gitをインストールしておく（必須
2. ホームディレクトリに移動。以下のコマンドを打つ

`git clone https://github.com/rbenv/rbenv.git ~/.rbenv`

3. Linuxにパスを通す作業 以下のコマンドを打つ

`echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile`

>Ubuntu系OSの場合は ~/.bashsrc 

4. 続いて次のコマンドを打つ

`~/.rbenv/bin/rbenv init`

5. コマンドが指示されるのでbashファイルに書き込むコマンドを打つ

`echo 'eval "$(rbenv init -)"' >> ~/.bashrc`

6. 一旦サーバーを再起動。または`source ~/.bashrc`コマンド

これでrbenvコマンドが使えるようになります。次はruby-buildをインストールします。

## ruby-buildのインストール

1. 専用ディレクトリ"plugins"を作成

`mkdir -p "$(rbenv root)"/plugins`

2. ruby-buildをそこにクローンします

`git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build`

ここまででrbenvを用いたRubyのインストールが可能になりました

## Rubyのインストール

1. 念の為以下のコマンドで他プラグインをインストールしておく

`yum install -y openssl-devel readline-devel zlib-devel`

* インストール可能なRubyをすべて一覧表示します

`rbenv install -l`

2. バージョンを確認して、下記コマンドを打ちます

`rbenv install 2.6.0`

**Rubyのインストールにはしばらく時間がかかります。**

3.完了したら以下のコマンドを打つ

```cmd
rbenv global 2.6.0	//グローバルで使うバージョンを指定
rbenv rehash			//設定を反映させる
ruby -v				//Rubyのバージョン確認
```

* Rubyのコマンドがエラーで通らなかったらもう一度bash追加コマンドを打つ

`echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile`

### エラーが出たら：nodejsを入れる

node.js公式からrpmをコピー

`https://github.com/nodesource/distributions/blob/master/README.md`

いったんroot権限(sudo su -)に切り替えた後にインストール。

`curl -sL https://rpm.nodesource.com/setup_12.x | bash -`

`sudo yum install nodejs`


### SELinuxの無効化

`sudo vi /etc/selinux/config`

SELINUX=disabledに書き換え

>一時的に無効： getenforce 0

### railsとSqlite3のエラー解決

Railsディレクトリ内　Gemfile(頭文字大文字)のsqlite3を

`gem 'sqlite3' , '-> 1.4.0' `

に書き換え後、`bundle install`

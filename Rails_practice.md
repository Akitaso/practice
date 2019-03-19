# rails Practice

## find_by メソッド
**モデル.find_by(カラム:値)**

*post*テーブルにある*id*が*3*のデータを取得する場合；
```ruby
rails console
post = Post.find_by(id:3)
post.content
=> 結果
```

*find_by*メソッドでidカラムが*params[:id]*(現在のURLiD)と等しい投稿をデータベースから取得。
```ruby
@post = Post.find_by(id:params[:id])
```
あとはhtml.erbに表示させる
```
<%= @post.content %>
<%= @post.created_at %>
```

link_to(post.contents, "/posts/#{post.id}")

get "post/new" => "posts#new"



## <% と <%= のちがい
* ここにRubyコードを記入するが、変数や文字列の表示はしない。
```ruby
<%  画面上に表示されない。変数の定義などに使う。%>

<%= イコールを付けることによりコードの中身は画面に表示される %>
```

## @付き変数のちがい
* コントローラー内アクションで定義された変数は変数はビューには使えないが、＠マークを先頭につけると特殊な変数としてビューにも使うことができる。
```ruby
---<<Controller>>------------------------

class ExampleController < ApplicationController
    def index 
        @post1 = "Wanko"    
        post2 = "Nyanko"    
    end
end
---<<View>>--------------------------------------
<%= @post1 %>       #@付きなので使える
<%= post2 %>        #このままだと使えない。エラー
```

## コントローラーの追加

$ rails generete controller :コントローラ名 :アクション名
 または rails g controller...
```ruby
----<<routes>>--------------------------------

Rails.application.routes.draw do
    get "toppage" => "home#toppage"
    get "about" => "home#about"
end
----<<ExampleController>>---------------------
#に対応するルーティングとアクションを追記する
class ExampleController < ApplicationController
    def toppage
    end

    def about
    end
end
```

##  ルーティング設定を確認する
コマンドから

* rails routes

ブラウザから(ローカル開発環境)

* localhost:3000/rails/info

ファイルから直接

* config/routes.rb

#### For Example...

URL : localhost:3000/home/toppage

routes : home#toppage の場合

|URL            |コントローラ|アクション|
|:--------------|:-----------|:---------|
|localhost:3000/|home        |/toppage  |


### 画像ファイルはどこに置く？

* /app/aseets/images内 (アセットパイプライン)

javascript,cssを入れて置くと自動で最適化してくれる。（アセットパイプライン）画像は除外。
フィンガープリントを自動で更新して、ファイル名に挿入するのでブラウザキャッシュされた古い画像が表示される心配が無い。
スラッシュを入れる必要は無い。

* /public内

先頭に必ず /(スラッシュ) を入れる必要がある。
src値に何も入らない。

    <%= image_tag('/tennis_ball.png') %>

## マイグレーションを生成⇒データベースに反映する

    rails generate model [モデル名](単数形) [カラム名]:[データ型(text)]

/db/migrate/20190201*****.rb に生成される。

次のコマンドで実際にデータベースに反映していく。

    rails db:migrate


## データベースからデータを取り出そう

    インスタンスの作成：.new メソッド

    インスタンスの保存：.save メソッド

* **Post**モデルから**post**インスタンスを生成

    post = Post.new

  *  テーブルにある最初のデータを取り出す

    post.first

  * カラムを取り出す

    post=**Post**.first

    post.content

    >**Post.first.content** 一行で済ますことも可

  * すべてのデータを取り出す

    post.all


  *  インデックス番号で取り出す

     **Post**.all[0]

## すべての投稿をビューで表示させよう

* **@posts**に、**Post**.allで取得したデータを代入

* **@posts**に代入された配列データをeach do文で1つずつ変数postに代入し、投稿内容を繰り返し表示。

```ruby
---<<Controller>>-----------------

def index
    @posts = Post.all
end 
---<<View index.html>>------------

<% @posts.each do | post | %>
    <div class="items">
        <%= post.content %>
    </div>
<% end %>
```

### 共通のビューを使おう
>共通のビューファイル

    views/layouts/application.html.erb

に、ヘッダー・フッター・スタイルシート等の共通点を追加まとめることができます。

```ruby
<%= yield %>

#各ビューファイル(about.html.erb等)を表示させたい部分に挿入します

<% provide(:title, "Home") %>

#各ビューファイルにprovideを記入しておき、共通のビューファイル(application.html.erb)に<%= yield:title %>を記入するとタイトル名を渡すことができます。
```
>**provide**(あらかじめ準備して供給)したものを**yield**(生み出す)

* views/layouts/application.html.erb の例

```html


<!DOCTYPE html>
<html>
  <head>
    <title><%= yield(:title) %> | サンプルのアプリ</title>
    <%= csrf_meta_tags %>   //クロス型攻撃を防ぐための記述
    <%= stylesheet_link_tag    'application', media: 'all',
                               'data-turbolinks-track': 'reload' %>
                               // スタイルシートに関する記述

    <%= javascript_include_tag 'application',
                               'data-turbolinks-track': 'reload' %>
                               //  javascriptに関する記述
  </head>

  <body>
    <%= yield %>
  </body>
</html>
```

## attr_accessor (Rubyメソッド)
attr_accessorメソッドは、クラスやモジュールにインスタンス変数を読み書きするためのアクセサメソッドを定義します。

引数には、インスタンス変数名をシンボルか文字列で指定（複数指定できます）。
たとえば

```ruby
class BookExample
    def name
    @name
    end

    def price
    @price
    end   
end

book=BookExample.new
book.title= 'TAKARAJIMA'
book.price= 1500
```
と定義していたのが、こうなります
```ruby
class BookExample
    attr_accessor :title ,:price
end

book=BookExample.new
book.title= 'TAKARAJIMA'
book.price= 1500
```
コードがすっきりします。

## Before_action

アクションの前に処理を差し込むことが可能。

このコントローラを継承すると必ずこのアクションが実行されます。

これだとnewやcreateなど全てのアクションも影響を受けてしまうのでこれだけは除外するように設定します。

```ruby
---<Controller>--------------------------------------
class SampleController < ApplicationController
  skip_before_action :require_login only:[:new, :create]
                                    #only:特定のアクションはスキップ
                                    #except:onlyとは逆の動作  
  private

  def require_login     #アクションを定義
    unless logged_in?   # unless =条件式が偽の場合の処理を記述する
      flash[:error] = "エラーメッセージをflashに保存してます。"
      redirect_to #new_login_url
    end
  end
end
```

## Strong Parameter とPrivateメソッド

記述されたコントローラの内部でのみ、実行することができるPrivateメソッド。セキュリティ上、DBに関する重要な関数を外部から使えないようにします。

ストロングパラメーターは、値を書き換えて送信できてしまう脆弱性を防ぐ為に導入されています。

なお、privateと書いた以下の処理すべてに適用されてしまうので、通常はコントローラの一番下に書きます。

```ruby
def create
  @user = user.new(sign_up_params)
    if  @user.save
      redirect_to @user,
    else
      render new
    end
end

private
      def sign_up_params
        # Storong Parameter
        params.require(:user).permit(:name, :email, :password, :password_confirmation)
        # requireで設定したキーだけを取得し、permitで決まった値だけを取得しています。
      end
```

## テスト

Minitestを使うことでルーティング、コントローラ、データベース、メソッド、ブラウザに返されるHTMLファイルの内容、様々なものを簡単にテストすることができます。

* Webリクエストが成功したか
* 正しいページにリダイレクトされたか
* ユーザー認証が成功したか
* レスポンスのテンプレートに正しいオブジェクトが保存されたか
* ビューに表示されたメッセージは適切か

### フォルダ構造

|ディレクトリー|役割|
|:-------------|:---|
|controller|ControllerViewRootingをまとめたテスト|
|fixture|テストデータ|
|helper|ビューヘルパーテスト|
|integration|コントローラーのやりとり
|models|モデル用テスト
|system|javascript等のテスト
|test_helper.rb|システムデフォルト設定とか

### 単体テスト
>test/controller/{コントローラー名}_controller.rb

ライブラリ・モデルが正しい動作をするかのチェック

```ruby
def setup
@user = User.new(name:"example",email:....)
end
test "should be vaild?" do
  assert @user.vaild?
  end
```
#### アサーション(assert) メソッド

assert メソッドは、第1引数がtrue である場合に、テストが成功したものとみなします。

インスタンス変性vaild?＝メソッドで有用性を確認している

|アサーション表記|目的|
|:-----------|:---|
|assert (test,[msg])|testがtrueか確認|
|assert_not|testがfalseか|
|assert_equal|testが == trueか|
|assert_not_equal|testが != trueか|
|assert_match(reg, string)|regがstringにマッチすることを確認|
|assert_no_match|マッチしないか確認|

### 機能テスト

コントローラー、ビューの通しチェックをする為のテスト。

機能テストではHTTPリクエストを擬似的に生成し、コントローラーがリクエストやレスポンスがビューに期待通り返されるかどうかテストします。

* リクエストが成功したか
* リダイレクトは正しく行われたか
* ユーザー認証は正しく行われるか
* レスポンスがビューに正しく返されているか

等をテストします。

```ruby
test "KINOU Test infomation" do
  get path名(リクエスト先のパス)  #get メソッドで擬似的にリクエストを生成
  assertメソッド 
    |assert_select      
    |assert_templete    
    |assert_redirect_to 






### setup / teardownメソッド
テスト実行前に毎回実行してほしい処理があれば、setupメソッドが使えます。
```ruby
setup do    # def setup を省略した書き方です
  @article = article(:example)
end
```
逆に、テスト実行後にしてほしい処理があればteardownメソッドを使います。
```ruby
teardown do
  rails.cache.clear()
end
```

### fixtures / seeds
どちらもDBにデータを入れるための仕組みですが、それぞれ違いがあります。
* fixtures
主にテスト実行環境(minitest) で実行する際に使うデータを定義します
>/test/fixtures/user.yml
```ruby 
temple:
  name: Mickey
  email: example@mickey.
  
# rails test した際自動的に読み込まれます
```
* seeds
動作確認のためにマスターテーブルに入れるデータを定義
モデルに対応したクラス(User)、Createメソッドを使ってデータを追加します
> /db/seeds.rb
```ruby
User.create(name: "Example User",
            email: "example@railstutorial.org",
)
# 最後に  rails db:seed をすると初期データが追加されます。
```

# ヘルパーメソッド(Rails Helper)

## helperとは

view内でちょっとした処理をやりたい時に使う。

viewをシンプルにする為に、viewとは別の場所(helper)で定義しておいたメソッドをviewから呼び出す。

よく使われるLink_toメソッド,image_tagメソッドもヘルパーを呼び出している。

### 実体

>/app/helpers/{コントローラー名} _helper.rb

application_helper.rbはアプリ全体で使えるヘルパー。

使い方はヘルパーにメソッドを定義しておいて、
```ruby
# Helpers
module ApplicationHelper
def Example
    "Ruby on Rails5"
    end
end
```
ビューにさっきの定義名を呼び出す。
```ruby
# Views
<%= Example %>
```

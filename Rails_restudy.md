** MODEL
データベースの準備
rails generate model Post（単数形にすること） collomn(カラム名):text(データ型)

db/migrate/201708011245.create.posts.rb
マイグレーションファイルができる 
rails db:migrate
DBにテーブルが作成される。同時にid,created,updatedも自動生成

app/models内のpost.rb
ApplicationRecordを継承したクラスをモデルと呼ぶ

** VIEW **
hoge.html.erb

<% posts1 = "投稿一覧をRubyコードで" %>
<%=posts1 %>
ブラウザに表示するには<%=でposts1の中身を表示

application.html.erbには共通のHTMLを書く
＜％＝yield%>に各ビューファイルの部分に代入され、application.html.erbの一部となる仕組み

** CONTROLLER **
hoge_controller.rb
class Controller < ApplicationController
	def top
	end
end

postController
投稿のルートとアクションをpostで生成
rails generate controller posts index

** 変数はビューでも定義できるがアクションで定義しよう
def index
	@posts=[
		"文字列を配列の",
		"中へ入れましょう",
		"ね！"
		]
	%>
end

ビューで変数を使う場合、頭に@マークを付けることを忘れずに！
@posts = "hoge" -> <%=@post1 %>


** ROUTING **
config/route.rb
ルーティングとそのアクション設定はここ

ルート"/"でトップページを指定できる

** STYLE SHEET**
app/assets/stylesheets
aaplication.scss
すべてに適応するスタイルシート
home.scss
HOMEコントローラだけに適応させるスタイルシート

** PICTURE PLACE **
public/
ここに画像を置きます

<img src="/tweets.jpg">
画像名を入力するだけで表示

** リンク **
<a href="/">Top</a>
<a href="/about">About</a>
hrefの文字をルーティングと同じにすることでビューでリンク先を指定

** EACH **
変数postsを用意、複数の文字列を配列にします
<%posts=[
	"文字列を配列の",
	"中へ入れましょう",
	"ね！"
	]
%>
postsを1つずつ取り出してpostに代入しながら表示phpでいうforeach?
<%posts.each do |post| %>
<div class="post-item">
	<%=post%>
</div>
<%end %>
each文をendで終わりにします。

** DATABASES **
テーブルにデータを入れる
newメソッドでインスタンスを作成、それをpost変数へ
post = Post.new(content:"Hello,World")
contentがHelloworldであるPostインスタンスを作成する

作成したインスタンスをpostsテーブルに保存する
post.save
PostモデルがApplicationRecordを継承しているためsaveを使える

Postにある最初のデータのcontentを取り出す
post=Post.first
post.content

すべての投稿を取り出す
posts=Post.all

配列から要素を取り出す
Post.all[0]
配列要素から投稿を取り出す
Post.all[0].content

すべての投稿を表示
**Controller**
postsコントローラindexアクションに@post変数に取得したデータを入れる
def index
	@posts=Post.all
end

**View**
変数postsをeach doで回し、表示させる

<% @posts.each do |post| %>
	<div class="posts-index-item">
		<%=post.content %>
	</div>
<% end %>


1. ルートにアクセスしたらどのアクションを起こすか指定:: Routes.rb
`root :to =>'lists#first'`
2. コントローラーにアクションを書く::/app/controller/lists_controller.rb
```ruby
def first
    @lists = List.all
end
```
アクションにfirstと名前を付け、すべてのデータをlist変数に入れる

3. ビューにアクション名を書いたファイルを生成:: first.html.erb
```ruby
html~
<% @list.each do |list| %>
	<%=list.title %>
	<%=list.detail %>...
<% end%>
```
これで

ルート(/)にアクセスするとlistsコントローラー内firstを呼び出されてviewに渡されてブラウザに表示される


```
### 使用したgemと使い方
```ruby
# ページネーションと見た目をbootstrapにするgem
gem 'will_paginate', '~> 3.1.0'
gem 'will_paginate-bootstrap4'
# 使い方::専用のメソッドを呼び出し
::コントローラー　List.allのところを書き換え。
@lists = List.paginate(page: params[:page], per_page: 何ページで区切るか)
::ビュー　ページネーション部分を表示する
<%=will_paginate @lists,:renderer=>WillPaginate::ActionView::BootstrapLinkRenderer %>

# ページをbootstrap化するgem
gem 'bootstrap', '~> 4.3.1'
# アセットCSS内stylesheet.scssに名前変更して、@import "bootstrap";　を追記
```
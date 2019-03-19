# Ruby Practice

## 配列をつくる
* 大括弧［］で括ります
```ruby
["suzuki","sato","kato"]
[41,22,53]
```
数字以外はダブルクォーテーションで囲む

### 配列を代入
```ruby
names = ["suzuki","sato","kato"]
```

### インデックス番号を用いる
```ruby
names = ["suzuki","sato","kato"]
puts names[1]
# 左から0番目、1番目、2番…と数える
_> sato
```

### 文字列の中に変数を展開する
```ruby
names = ["suzuki","sato","kato"]
puts "私の名前は #{names[1]} です"
_>私の名前はsatoです
```

## くりかえし処理(each)
```ruby
names = ["suzuki","sato","kato"]
names.each do | name |
    puts name
end

_>私の名前はsuzukiです
_>私の名前はsatoです
_>私の名前はkatoです
```

## シンボルとハッシュ
* 波括弧｛｝で括る

```ruby
#空のシンボルハッシュ宣言
hash = {}

#ハッシュ   配列と違いキーと値の組み合わせ。ミュータブル
{"name"=> "suzuki", "age"=>21}

#シンボル   名前を識別するためのラベル。文字列と同様。イミュータブル（あとから変更不可）
{:name => "suzuki", :age =>21}

#どちらも出力結果はおなじ
```

### さらに省略した書き方
* シンボル
```ruby
user = {name: "suzuki",age: 21}
puts user[:name]
#出力する時はコロンを先頭に持ってくること。 => を省ける
_>Suzuki
```
#### 


## if文
配列の要素を確認する
```ruby
user = {name: "suzuki" , age:21}
if user[:age]
    puts "#{user[:age]}が存在した場合のコード"
else
    #値がない(nil)場合のコード
end
```

### 繰り返し(each文)の中でハッシュ値を使おう
```ruby
users =[
    {name: "suzuki" , age:21}
    {name: "sato" , age:26}
    ]

users.each do | user |
    puts user[:name]
end

_> suzuki
_> sato
```

## メソッドを自作しよう
```ruby
def introduce(name)
    puts "私の名前は#{name}です"
end

#メソッドの呼び出し
introduce("watanabe")

_> 私の名前はwatanabeです
```

### 複数の引数を渡そう
```ruby
def introduce(name , age)
    puts "私の名前は#{name}です"
    puts "#{age}歳です"
end

introduce ("watanabe" , 15)
#引数の順番は対応しているので注意
```

### 戻り値とは
戻り値(return)がある場合は
```ruby
def add(a,b)
    return a + b
    # 1+3 が計算されて、また戻る
end

sum = add(1,3)
#add メソッド呼び出し後、 値がsumに代入される
puts sum

_> 4
```

### 真偽値を返す

```ruby
def negative?(number)
    return number < 0    
end

puts negative?(5)
```
if文で使うような真偽値を返す

この場合条件(numberが0以下か)の結果の真偽が返される

メソッドの末尾に?を付けるのが約束事

### 返り値をif文で分岐するメソッド
```ruby
def shohin(price)
    if price >= 5000
        return price
    end
    return price + 500
end
```
priceが5000以上だったらそのまま値を返し、それ以下であればpriceに500円足した数字を返します。

### キーワード引数

```ruby
def shohin(item:,price:,count:)
    puts "#{item} ... #{price} x #{count}"
end

shohin(item:"pocky",price:150,count:1)
```
順番通りに並べなくとも、引数をキーワードで指定することも可能。
値の後ろ（コロン：）付け忘れに注意

## クラスの定義

## インスタンス変数(attr_accessor)
Menuクラスインスタンスに変数を持たせることが出来る。外部からでも変更が可能に
```ruby
class Menu
    attr_accessor :name
    attr_accessor :price
end

menu1 = Menu.new
#Menu クラスから空インスタンス生成。それをmenu1 に代入

menu1.name ="すし"
#さっき作った空のインスタンスに変数に値をセット
puts menu1.name
#インスタンス変数のname値を出力する
```

### クラス内でメソッドを定義する

{インスタンス名.メソッド}でそのメソッドを呼び出すことが可能

```ruby
class Menu
  attr_accessor :shohin
  attr_accessor :price

  def info
    puts "infomationです。"
  end
end
menu1 = Menu.new
# メソッドの呼び出し
menu1.info

_>infomation
```

### インスタンスメソッド

```ruby
class Menu
    def show(data) <-引数
        return "私は#{data}です" <-戻り値
    end
menu1 = Menu.new
puts menu1.show("メニュー")
```

### インスタンスの中でインスタンス変数を使う
```ruby
class Menu
    def show_name
     puts "私は #{self.name} です"
    #  self.変数で扱う
    end
end

menu1 = Menu.new
menu.name ="ピザ"
# インスタンスに変数を代入
menu1.show_name
# インスタンスメソッド呼び出し

_>私はピザです
```

### インスタンス作成と同時に変数を代入する(initialize)

java言語でいうところのコンストラクタ。

```ruby
class Menu
    def initialize
        puts "メニューが作成されました"
    end
end
menu1 = Menu.new
#Menu.newが実行されると同時にinitialzeメソッドが呼び出される
```
self.変数でインスタンス変数を扱える
```ruby 
def initialize
    self.name = "ピザ"
end
menu1=Menu.new
puts menu1.name
```

### initializeメソッド

initializeメソッドにも引数を渡すことができる。

クラス.newの後に引数を書くと値を渡せる。

```ruby
def initialize(message)
puts message
end

menu1 = Menu.new("こんにちは")
_>こんにちは
```

### initializeにインスタンス変数と値を代入する
```ruby
def initialize(name: ,price:)
    self.name = name
    self.price = price
end
menu1 = Menu.new(name:"ピザ", price:800)
        キーワード引数を使う事で見やすく↑
```

### require | ファイルを分割して読み込む
```ruby
# 一番上の行に記入(menu.rbを読み込む場合)
require "./menu"
```

```ruby
menu1 = Menu.new(name:"ピザ", price:800)
menu2 = Menu.new(name:"すし", price:1000)
#menusに配列を代入
menus = [menu1,menu2]
#menus配列を繰り返して結果をmenuに代入
menus.each do | menu |
 puts menu.info
 #infoメソッド：nameとpriceを表示させる
end
```

## gets.chomp | 入力を受け取る
入力された値を変数に代入
```ruby
puts "名前を入れてください"
name = gets.chomp
puts "あなたの名前は#{name}です"
```

### gets.chomp.to_i | 文字列を数字に変換する

「getsメソッド」で得られる入力は文字列として扱われるので数値に変換する必要がある
```ruby
number = gets.chomp.to_i
puts "#{number}の２倍は#{number *2}です"
```

> MemoRandom
```ruby
#入力値をorderに代入
order = gets.chomp.to_i
#メニュー欄(menus)にorder番目の数値をselected~に代入する
selected_menu = menus[order]
#selected~内のnameを取得する
selected_menu.name

#入力値をcountに代入
count = gets.chomp.to_i
#インスタンスselected~をgetTotal~メソッドcountと一緒に渡す。
selected_menu.get_total_price(count)
```


## 継承

class 新クラス < 元になるクラス

        子class      親class

インスタンス変数やインスタンスメソッドを引き継ぐ

継承することで別のメソッドや変数が使えるようになる

## 子クラスにインスタンス変数の追加
```ruby
class Food < Menu
    attr_accessor :calorie
end
```
子クラスにインスタンス変数*calorie*が追加されました。

親クラスでもこの変数が使えるようになります。


## 子クラスにインスタンスメソッドを追加
```ruby
food1 = food.new(name:"ピザ",price:800)
food1.calorie =700
# 子クラス内にインスタンスメソッドを追加
def calorie_info
    return "ナマエ#{self.name} カロリー#{self.calorie}"
end

# 子クラスから~infoを呼び出して出力
puts food1.calorie_info
```

## オーバーライド

親クラスと同じ名前を子クラスで定義すると上書きする。

## SUPER
オーバーライドしたメソッド内で*super*を呼び出すと、親クラスの同名メソッドを呼び出すことができる。
引数は親クラスに合わせる必要あり

```ruby
def initialize(name: ,price: ,calorie:)
    super(name:name,price:price)
      self.calorie = calorie
    end
end

#親クラスのinitializeを呼び出してます
def initialize(name:,price:)
      self.name =name
      self.price=price
end
```
共通部分の多い処理等で使われる場面が多い

## Dateクラス（日付を扱う）

```ruby
require "date"
----------------------------
date1 = Date.new(2014,7,31)
puts date1

_>2014-07-31

#日付が日曜日かどうか（真偽値
date1=Date.new(2014,7,31)
puts date1.sunday?

_>false
```
今日の日付インスタンスを作る
```ruby
require "date"
date1 = Date.today
puts date1
```
## クラスメソッドの定義
*Date(クラス名).today(クラスメソッド)*
```ruby
class Menu
    def Menu.is_discount_day?
    today = Date.today
    end
end
_>false
```
変数todayにsunday?メソッド用いた例

### クラスメソッドを呼び出しす
クラス名｡メソッド名
```ruby
class Menu
    def Menu.is_discount_day?
    end 
end
puts Menu.is_discount_day? 
```

### インスタンスメソッド内でクラスメソッドを呼び出す
```ruby
class Menu
    def get_total_price
    if (Menu.is_discount_day?)
    end
end
```


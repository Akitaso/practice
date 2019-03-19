# 正規表現

## ドット( . )
### 一文字だけの表現

```
検索
[私は.です]
結果
私は鳥です   私は馬です  私は0です
```
```
[私は...が...です]

私はうどんが嫌いです
```
```
[TEXT\.LOG]

\を付けるとドットの文字を検索

TEXT.LOG
```
## ＾と＄
### 行の最初と最後を検索
>例文
* 今日はありがとうございました。
* ありがとうと言いたい
* 君に心よりありがとう

```
[^ありがとう]

行の先頭だけ検索
＝＞ありがとうと言いたい

ありがとう$

行の終端を検索
＝＞君に心よりありがとう

.*ありがとう$

ありがとうで終わる文字列
```

## ＊と＋と？
### 同じ文字の繰り返し表現
```
[おー*い]
おい　おーーーい　おーーーーーーい
直前文字がないか文字が連続する場合

[おー+い]
おい　おーーーい　おーーーーーーい
直前文字があってかつそれが連続する場合

[おー?い]
おい　おーい
直前文字がないか１つだけ
```

## ．＊の組み合わせ
### なんでもいい文字の連続

```
[君が好き.*。]

君が好きです。
君が好きかもね。 
君が好きだっちゅうの。

[楽.*ね]
楽しいかもね、そうかもね
何でもいい位置文字と直前文字の連続＝色んな文字の連続
```
## いずれかの文字列 ( | )
```
区切られた文字列いずれかに合致
[ＩＢＭ|マイクロソフト|Ａｐｐｌｅ|ネットスケープ]
ＩＢＭ　マイクロソフト　Ａｐｐｌｅ　ネットスケープ
```
## グループ化
### 指定した文字のどれか ( [ ] )

>例文
* 明日は晴です
* 明日は曇です
* 明日は雨です
* 明日は雪です

```
[　]内のどれか一つ一致した時
<明日は[晴曇雨]です>

明日は晴です明日は曇です明日は雨です

アルファベットA~Z数字0~9日本語をまとめて指定できます
<A[A-Z]CODE>
<.[0-9]CODE>
<た[か-こ]こ>

AECODE
A8CODE
たかこ  たきこ  たけこ

[ABC^]
A,B,C,^いずれかの文字。カッコ内の^は普通の文字列として処理される

[^^4]
^ =否定
^か4以外の文字
```
# React Practice

React(リアクト)とは、Facebook製の**JavaScriptライブラリ**です。Webアプリケーションのユーザーインターフェースを効率的に構築することを目的としており、主にView部分を実装します。

---

## JSX

HTMLとJavascriptが混同している書き方ができるのが特徴。

MVCモデルのVIEW部～MODEL部分を担当する

**render()内に複数要素が入っているとエラーが出るので注意。**

```js
render(){
    return(
<h1>aaaaa</h1>
<h2>bbbbb</h2>
<h3>ccccc</h3>
        );
}

// ではなく、一つの要素にまとめよう

render(){
    return(
    <div>
        <h1>aaaaa</h1>
        <h2>bbbbb</h2>
        <h3>ccccc</h3>
    </div>
        );
}
```

> imgタグでは最後にスラッシュが必要になる。

```js
render(){
    return(
        <img src="https:/s3-ap-northeast-1.amazonaws.com/progate/shared/images/lesson/react/ninjawanko.png" />
    )
}
```

## App.js

```js
import React from 'react';
class App extends react.Component {
    //--ここからjs部分
    //Reactをインポート、Componentを継承するクラスを定義
    render(){
        |//--ここがJSX部分。ブラウザに表示される
        |// <h1>Hello,React! </h1>
    }
}
export default App;
//クラスをエクスポート
```

### Javascriptの記述

return外にはjavascriptを記述することが出来る。
この空間に定数を定義する場合が多い。

```js
class App extends React.Component {
    render() {
        //js定数をここで定義
        const text ="Hello World";
        return(
            ...
        );
    }
}
```

> Javascriptで変数は let 関数、定数は const 関数
>
> 違いは後から値を変更できかできないか。

### JSXにJSを埋め込む

return()の中にJSを埋め込む場合、JSの部分を {} で囲む必要がある。

```js
render(){
    //JS記述部分
    const text='Hello,World';
    return(
        //ここはJSX記述
        <div>{ text } </div>
    );
}
```

### JSとJSX内コメントを記入

#### JSX部分は {/* */}で囲む

```js
render(){
    return(
        <div>
        {/* コメント部分は */}
        </div>
    );
}
```

#### JS部分は 先頭に //

```js
render(){
    //コメント部分
    const text = 'Hello World';
    return(
        |
    );
}
```

## イベントを書く

```js
<button EVENT_NAME={() => { _SHOLI_ } }></button>
```

アロー関数(=>)を使ってイベント名と処理を記述。

Javascriptの記述なので{}で囲むことを忘れずに

> アロー関数:
>
> 関数の定義にFunctionやthisを書かずに短い記述で定義できる。
> 引数が1つの時に限り()が省略して書ける。また一行記述だとreturn、{}も必要無い。

### onClickイベント

ボタンがクリックされた時に発動するイベントを書きます。

#### ボタンをクリックしたらログに文字列を出力する：

```js
<button onClick={()=>{console.log('押したよ') }}>押せ</button>
```

## Stateで動的なイベントを作る

>State : 状態
>
>ユーザーの動きに合わせてテキスト等を動的に変えることが出来る

- Stateを定義する
- State取得し、表示する
- Stateを更新させる

>オブジェクト :
>
>複数の値をプロパティと名前を付けて管理できるモノ

```js
const user = {name:'あああ', age:14};
console.log(user.age);

//  出力結果:14
```

### Stateの定義

- constructor(props)内で**this.state**というオブジェクトを定義

```js
class App extends React.Componemt{
    constructor(prop) {
        super(prop);
        this.state = {name:'あああ'};
    //  　<--代入--- オブジェクトを定義
    }
}
```

### Stateの取得

定義したStateをrender内、**this.state**で取得する。

- オブジェクトをconsoleに出力してみる

```js
class App extends React.Componemt{
    constructor(prop) {
        super(prop);
        this.state = {name:'あああ'};
    }
    render {
        console.log(this.state);
    //              取得してる
        return()
    }
}
```

### stateを表示する

**this.state.name**でStateを取得してブラウザ上に出力します

```js
class App extends React.Componemt{
    constructor(prop) {
        super(prop);
        this.state = {name:'あああ'};
    }
    render() {
        retrun(
            <h1>こんにちは、{ this.state.name }さん！</h1>
            //              this.state.プロパティ名
        );
    }
}
```

### stateを変更

this.**setState**({プロパティ名:'変更する値'})

- 例:ボタンをonClickしたらsetStateでnameをあああに変えるjs

```js
<button onClick={()=>{this.setState({name:'あああ'})}}>
```

> ReactではStateの値を直接代入することで値を変えてはいけないルールがあるので注意

### Stateを変更するメソッドを作る

```js
class App extends React.Component{
    constructor(props){
        |
    }
    //メソッド名の定義
    handleClick(name){
        this.setState({name:name});
    }
    render(){
        return(
            <button onClick={()=>{this.handleClick('いぬ')}}>押してね</button>
            //                    this.メソッド名(引数)を呼び出し
        )
    }
}
```

### 数字カウントアップを作る

```js
class App extends React.Component{
    constructor(props){
        super(props);
        // 1.stateの定義    カウント用
        this.state = {count:0};
    }
    handleClick() {
        //3.stateのcountに1足すメソッドを定義
        this.setState({count:this.state.count+1 });
    }
    render(){
        return(
            <div>
                {/* 2.stateカウントを表示 }  */}
                <h1>{this.state.count}</h1>
                {/* 4.ボタンをクリックしたらhandleClickを呼び出すようにする */}
                <button onClick={()=>{this.handleClick()}}> + </button>
            </div>
        );
    }
}
export default App;
```

## babelのインストール

JSXを通常のJSファイルに変換します

```cmd
 npm init -y
 npm install --save-dev webpack
 npm install --save-dev babel-loader @babel/core @babel/cli @babel/preset-env
```

>--save-devオプション プロジェクト毎にインストール
>-g オプション  グローバルインストール

- npm init -y
設定ファイルの作成。-yは初期設定を無視
`package.json`というファイルが作られます
- webpack
webpackの目的はモジュールバンドリング。複数に分けたjsファイルをひとつにまとめる際にも使われる
- @babel/core cf. @babel/core · Babel
babelのコア
- @babel/cli cf. @babel/cli · Babel
babelコマンドが使えるようにする
- @babel/preset-env cf. @babel/preset-env · Babel
指定している環境に合わせていい感じにプラグインを使って変換

### .babelrc設定ファイルを追加

`$ touch .babelrc`

ここにプリセットやオプションなどを追記。

```json
{
  "presets": [
    "@babel/preset-env", "@babel/preset-react"
  ]
}
```

## Webpackのインストール

```cmd
npm init -y
npm install webpack webpack-cli --save-dev
```

`webpack.config.js`内にbabelを使用する旨を書き足し、entry、moduleの変換対象をjsxに変更します

```js
const path = require('path')
module.exports = {
  entry: './src/index.jsx',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
    }
  },
  module: {
  rules: [
    { test: /\.js$/, exclude: /node_modules/, loader: "babel-loader" }
  ]
}
...
```
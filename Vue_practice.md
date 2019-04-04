# Vue.js Practice

## Vueインストール（CDN）

`<script src="https://cdn.jsdelivr.net/npm/vue"></script>`

テンプレートを記述

```js
new Vue({
  el: '#app', //適用するIDをまず指定
  data: {    //以下プロパティ等を指定
    message: 'Hello Vue.js!'
  }
})
```

## ディレクティブ

既存のHTMLに独自の属性を追加することで属性に応じた変化を操作する。この属性のことをディレクティブと呼びます。

インスタンスを指定した要素とその子要素でしか使えません。

## 双方向データバインディング

データとUIを結びつける仕組み

### データからUIに反映

* HTML部分  {{ name }}カッコで囲む。

`<div>Hello, {{ name }} !</div>`

* JS部分  name: 'taguchi' を定義する。

> **Hello, Taguchi**

### UIからデータに反映 (v-model)

* V-modelが Vueと結びつくので、

`<input type="text" v-model="name">`

* リアルタイムで値が変更される。

`{{ name.toUpperCase() }}`

* .toUpper~で結果を大文字にすることも可能

```js myscript.js
var vm = new Vue({
  el: '#app',
  data: {
    name: 'taguchi'
  }
})
```

### ループさせる命令(v-for)

`<li v-for="todo in todos">{{ todo }}</li>`


```js
new Vue({
el:'#app',
data:{
    'task 1',
    'task 2',
    'task 3'
      }
})
```

リストがループで出力される
 * task1
 * task2
 * task3

## 条件付きレンダリング

### v-show

html
`<span v-show="Hour <= 11">こん・・にちわ？</span>`

js

```js
var Date = new Date();
var Hour = Date.getHours();
```

ディレクティブ属性値に11以下という条件式を記述。
**条件式に一致すれば、「こん・・にちわ？」を表示して、不一致ならば非表示になります。**
(CSS、Display:noneが適用されます)

### v-if

html
`<span v-if="Hour <= 11">おはようございます</span>`
`<span v-else-if="Hour <= 17">こんにちわ</span>`
`<span v-else-if="Hour <= 17">こんばんわ</span>`
js

hourで現在時刻を取得し、spanタグをifディレクティブに変更しました。
**11時より前だった場合、「おはようございます」と表示します。それ以外はこんにちわと表示します**

spanタグが３つ続いてますが、ブラウザーには１つのspanタグしか描写されません。

これがv-showと違うところです。

## 宣言レンダリング

### v-bind : この要素のtitle属性をmessageプロパティによって更新されている

```html
<div id="app2">
  <span v-bind:title="message">Hands Up!boy and Girls!</span>  
</div>
```

```js
new Vue({
	el:'#app2',
  data:{
    message:'You loaded this page on' + new Date().toLocaleString()
  }
})
```

>Model値を表示したいだけなら**v-bind**。ユーザ入力を即座にModelに反映したいなら**v-model**を使いましょう。

## ユーザー入力制御（v-on）

ユーザーがボタンをクリックすると{{message}}文字が逆向きになる

属性値にメソッド名（メソッドイベントハンドラ）を書くやり方です。

```html
<p>{{ message }}</p>
<button v-on:click="reverseMessage">逆になるよ</button>
```

```js
new Vue({
  el:'#app3',
  data:{      //messageに文字列を入れる
    message: 'Hello Vue.js!'
    },
  methods:{   //ここからロジックを書く
    reverseMessage: function () {
    this.message = this.message.split('').reverse().join('')
        }
      }
})
```

## コンポーネント(Component)

本質的にはVueのインスタンス的存在

```html
<div id="app4">
  <ol>
    <todo-item></todo-item>
  </ol>
</div>
```

```js
//まずコンポーネントを宣言してから
Vue.component('todo-item',{
  template : '<li>This is a todo</li>'
})
//今まで通りVue呼び出し
var app4 = new Vue({
  el: '#app4'
})
```

> 1.This is todo

todo-itemタグの位置にリストが挿入される

### 子にデータを渡すコンポーネントの構成

> Vue.js公式ガイドより

```html
<div id="app4">
<ol>
<!--
  todoにオブジェクトを与える
  v-for=でitemをループさせ
  v-bindでitemをtodoを渡す
-->
  <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
</ol>
</div>
```

```js
Vue.component('todo-item',{
  //コンポーネントをプロパティで受け取れるように名前を「todo」とする
  props: ['todo'],
  template : '<li>{{todo.text}}</li>'
})

var app4 = new Vue({
  el: '#app4',
  data:{
    groceryList: [
        { id: 0, text: 'Vegetables' },
        { id: 1, text: 'Cheese' },
        { id: 2, text: 'Whatever else humans are supposed to eat' }
      ]
    }
})
```


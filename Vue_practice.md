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

## 双方向データバインディング

データとUIを結びつける仕組み

### データからUIに反映

* HTML部分  {{ name }}カッコで囲む。

`<div>Hello, {{ name }} !</div>`

* JS部分  name: 'taguchi' を定義する。

> **Hello, Taguchi**

### UIからデータに反映 v-model

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

`<li>{{ todos[0]] }}</li>`

### ループさせる命令(v-for)

`<li v-for="todo in todos">{{ todo }}</li>`

```js
data:{
    'task 1',
    'task 2',
    'task 3'
}
```

リストがループで出力される
 * task1
 * task2
 * task3

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

## ユーザー入力制御（v-on ディレクティブ）

ユーザーがボタンをクリックすると{{message}}文字が逆向きになる

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


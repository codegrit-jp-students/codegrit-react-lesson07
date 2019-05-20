## イベントハンドリングの基礎

## はじめに

さて、ここまでのレッスンで、イベント処理の仕方については既に簡単に説明しました。イベント処理はReactアプリを作成する上で最も重要なことの一つです。このレッスンではReactにおけるイベント処理について更に詳しく見ていきましょう。

## イベントハンドラの書き方

ここまでのレッスンでは、コンポーネント内でのファンクションは以下のように書いてきました。

```js
import React, { Component } from 'react'

export default class extends Component {
  someFunc = (arg1, arg2) => {
    return
  }
  render() {
    return <a onClick={this.someFunc}/>
  }
}
```

これ以外の書き方として、以下のように書くこともできます。

```js
import React, { Component } from 'react'

export default class extends Component {
  constructor() {
    super()
    this.someFunc = this.someFunc.bind(this)
  }
  someFunc(arg1, arg2) {
    return
  }
  render() {
    return <a onClick={this.someFunc}/>
  }
}
```

2つ目の書き方では、constructorの中で以下のような記述があります。

```js
this.someFunc = this.someFunc.bind(this)
```

これは、render内でファンクションを呼び出した時に、thisがそのコンポーネントクラスと結びつくようにするために必要です。JavaScriptにおいて、通常クラスメソッドはそのクラスと結びついておらずthisの中身が`undefined`となっているためです。しかしES2019では、class propertiesというものが導入されることが予定されており、create-react-appではBabelを通して、これを既に使えるようにしています。

```js
someFunc = (arg1, arg2) => {
  return
}
```

そのため、上記のように書くことによってbindの記述を書かずにクラスメソッドをコンポーネントと結びつけることが出来るようになりました。これはcreate-react-appを利用しない場合はデフォルトでは使えないので、Babelのplugin-proposal-class-propertiesというプラグインを入れる必要があるので注意してください。

## Reactで扱えるイベント

Reactでは、通常のブラウザイベントの処理は実装されています。JavaScriptで既に学んだ通り、以下のようなイベントは頻繁に利用する機会があります。

### マウスイベント

- onClick - 要素がクリックされた
- onMouseEnter - 要素にマウスのポインタが入った
- onMouseLeave - 要素からマウスのポインタが出た
- onMouseDown - 要素内でマウスのクリックボタンが押された
- onMouseMove - 要素内でマウスが動いた
- onMouseUp - 要素内でマウスのクリックボタンが戻された

例えば、Googleカレンダーであれば、onMouseDownイベントでイベントの開始時間を選び、onMouseMoveイベントでイベントの時間範囲を選択肢、onMouseUpでイベントの終了時間を確定するというような処理をしています。

### キーボードイベント

- onKeyPress - キーボードのキーがクリックされた

例えば、よくあるのはフォームでEnterボタンを押したらフォームを提出ボタンを押さなくてもフォームを提出出来るようにしたいという場合に使うことが出来ます。

### フォームイベント

- onChange - インプット内容が変更された
- onSubmit - 提出ボタンが押された

### タッチイベント

タッチイベントはタッチパネルのついたパソコンやモバイルサイト上でよく利用します。

- onTouchStart - タッチされた
- onTouchEnd - タッチしている指が離れた
- onTouchMove - タッチされたまま指が動いた

### UIイベント

- onScroll - スクロールされた

この他に、React内では実装されていませんがwindowのresizeイベントもよく利用されます。

## preventDefaultファンクション

JavaScriptにおいて、あるイベントがイベントハンドラによってうまく処理されない場合、更に上のスコープ上で上手くイベントを処理出来るイベントハンドラを探そうとします。これを泡が上に登っていく様子に例えてバブリングと言います。このデフォルトの機能によってイベントを処理出来なかった場合に思わぬ副作用が発生する場合があります。これを防ぐにはpreventDefaultを必ずイベントを処理する前に入れるようにしましょう。preventDefaultイベントはブラウザ固有のデフォルトのイベント処理を防ぐためのファンクションです。

stopPropagationという

```js
handleClick = (e) => {
  e.preventDefault()
  (イベント処理)
}
```

## インラインでのイベント処理とファンクション呼び出し

イベントの処理の方法は既にレッスン2でも説明したとおり、インラインで処理する方法とファンクションを呼び出す方法のどちらも利用できます。

### インラインでイベントを処理

インラインでイベントを処理するには以下のようにonClick Props内でファンクションを定義します。

```js
import React from 'react'

export default class extends Component {
  render() {
    return (
      <a onClick={(e) => {
        e.preventDefault()
        console.log("clicked")
      }}>クリック</a>
    );
  }
}
```

またクラスファンクションやPropsのファンクションを呼び出す場合で、引数をダイナミックに変えたい場合にも利用できます。例えば、以下の例ではprops内のsomeFuncというファンクションに対して、eventオブジェクトと、isLoggedInステートの2つを引数として与えています。

```js
import React from 'react'

export default class extends Component {
  state = {
    isLoggedIn: false
  }
  render() {
    const { isLoggedIn } = this.state
    return (
      <a onClick={(e) => this.props.someFunc(e, isLoggedIn)}>クリック</a>
    );
  }
}
```

### ファンクションを呼び出す

既にこれまでのレッスンで見てきている通り、ファンクションを指定するのがより一般的な方法です。この場合は`onClick={(e) => this.handleClick(e)}`のような書き方をしてもいいですが、`onClick={this.handleClick}`のようにすれば、自動的に最初の引数にeventオブジェクトが渡されます。

```js
import React from 'react'

export default class extends Component {
  render() {
    return (
      <a onClick={this.props.handleClick}>クリック</a>
    );
  }
}
```

## コンテナーコンポーネントとプレゼンテーションコンポーネント

レッスン3で説明した、コンテナーコンポーネントとプレゼンテーションコンポーネントを再度見ていきましょう。
上記の例で、handleClickイベントをpropsから呼び出していることに気づいたかと思います。なぜこんなことをするのでしょうか? それは、Reactでは、ステートは一部のコンポーネントのみが持つようにすべきという考えがあるからです。

プレゼンテーションコンポーネントはStateを持たないようにするため、ステートレス(stateless)コンポーネントという呼ばれ方もよくされます。

今後自分でコンポーネントを書くときは、基本的にデータの保持と処理を行うコンポーネントと、ステートレスコンポーネントを分けるようにしましょう。
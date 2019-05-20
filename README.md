# レッスン7. イベント処理

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

## Accordionを作ろう

実際にレッスン2よりも更に複雑な例を以下では見ていきましょう。この例では料理のレシピを取得して、アコーディオンを利用して手順を順番に表示して、クリックすると詳しい手順が表示されるようなコンポーネントを考えます。

完成イメージは以下のようになります。

![recipe-accordion-sample.gif](https://firebasestorage.googleapis.com/v0/b/codegrit-images.appspot.com/o/codegrit-react%2FLesson07%2Frecipe-accordion-sample.gif?alt=media&token=5a80d2b1-9ad6-4025-b30e-c688320fad0c)

サンプルコードはこちらにあります。

- [codegrit-react-lesson07-recipe-accordion](https://github.com/codegrit-jp-students/codegrit-react-lesson07-recipe-accordion)

## 1. アプリを作成する

まずはいつものようにcreate-react-appを利用してアプリを作成します。

```bash
npx create-react-app recipe-accordion
```

作成したら、必要なNPMパッケージをインストールします。今回はemotionのみを利用します。

```bash
cd recipe-accordion
yarn add @emotion/core @emotion/styled
```

インストールが完了したら、アプリを立ち上げておきましょう。

```bash
yarn run start
```

## 2. 必要なフォルダを作成する

エディタを立ち上げて、src直下に、以下の3つのフォルダを作ります。

- containers - コンテナーコンポーネント向けのフォルダ
- components - コンポーネント向けのフォルダ
- assets - ローディングイメージを保存するためのフォルダ

## 3. ローディング用のイメージを作成する

こちらのサイトでローディング用のイメージを作成しましょう。作成するイメージはなんでも構いません。作成し終えたらSVG形式でダウンロードし、spiner.svgという名前でassetsフォルダに入れます。

[loading.io](https://loading.io/)

自分で作成するのが面倒な場合は以下のファイルを使ってください。

![loading.svg](https://firebasestorage.googleapis.com/v0/b/codegrit-images.appspot.com/o/codegrit-react%2FLesson07%2Floading.svg?alt=media&token=70d88c3b-2743-4303-843f-7bdfa61055a4)

## 4. index.cssを編集する

次に、index.cssを編集します。初期状態では、`box-sizing: border-box`が設定されていないため、以下を追記します。

```css
* {
  box-sizing: border-box;
}
```

## 5. サンプルデータ用のファイルを作成する

実際のアプリにおいては、Fetch APIを利用してデータを読み込みますが、今回のアプリでは簡単のためにサンプルデータファイルを作成して、そこからデータを読み込みます。

そのためにtestData.jsというファイルをsrc直下に作成して次のデータを加えます。今回は親子丼のレシピを加えました。実際に使うようなデータに近くするため材料などについても書いてありますが、今回利用するのは`procedures`の部分のみです。

```js
// src/testData.js
export const sampleData = {
  id: 1,
  name: "親子丼",
  estimate_time: "30分",
  calory: "700",
  good_for: 2,
  ingredients: [
    {
      name: "卵",
      quantity: "3",
      unit: "個"
    },
    {
      name: "鶏もも肉",
      quantity: "200",
      unit: "g"
    },
    {
      name: "玉ねぎ",
      quantity: "1/2",
      unit: "個"
    },
    {
      name: "醤油",
      quantity: "2",
      preface: "大さじ",
      unit: "杯"
    },
    {
      name: "みりん",
      quantity: "1.5",
      preface: "大さじ",
      unit: "杯"
    },
    {
      name: "砂糖",
      quantity: "2",
      preface: "小さじ",
      unit: "杯"
    },
    {
      name: "だし",
      quantity: "2/3",
      unit: "カップ"
    }
  ],
  procedures: [
    {
      order: 1,
      description: "鶏肉を一口大、玉ねぎを幅1cm程度に切る。卵以外の材料をすべて小さめのフライパンに入れる"
    },
    {
      order: 2,
      description: "中火にして、煮立つまで待ち、煮立ってから5,6分ほど鶏肉に日が入るまでそのまま煮る。"
    },
    {
      order: 3,
      description: "火を弱火にする。卵をボールで溶きほぐし、全体的に行き渡るように入れる。卵に少し火が固まり始めたら火を止めて蓋をする。１分ほど蒸らして半熟状態になったら完成。"
    },
    {
      order: 4,
      description: "器にご飯と一緒に盛って、三つ葉やのりを散らして召し上がれ。"
    }
  ]
}
```

## 6. Recipeコンテナーを作成する

### 6-1. ファイルを作成し、App.jsから呼び出す。

まずは、Recipeのデータを読み込むためのRecipeコンテナーを作成します。containersフォルダ直下に`Recipe.js`というファイルを作成してください。

ファイルが作成出来たら、簡単な枠組みだけを作ります。

```js
// src/containers/Recipe.js
import React, { Component } from 'react'

class RecipeContainer extends Component {
  render() {
    return <div />
  }
}
export default RecipeContainer
```

App.jsとRecipeコンテナーをつなげます。

```js
// src/App.js
import React, { Component } from 'react';
import RecipeContainer from './containers/Recipe'

class App extends Component {
  render() {
    return (
      <div className="App">
        <RecipeContainer />
      </div>
    );
  }
}

export default App;
```

### 6-2. サンプルデータを読み込む

Recipeコンテナーでは、Sampleデータを読み込みこれをRecipeコンポーネントに渡します。Fetch APIの挙動を真似するために、setTimeoutファンクションを利用して、1秒の遅れを発生させます。データの読み込みにはloadRecipeというファンクションを利用します。

```js
import React, { Component } from 'react'
import { sampleData } from '../testData' // testDataファイルからsampleDataをインポートする

class RecipeContainer extends Component {
  state = {
    recipe: null // 初期の状態ではrecipeのデータがないためnull
  }
  loadRecipe = () => {
    return new Promise(function (resolve) {
      // setTimeoutを利用して１秒の遅延を発生させ、その直後にsampleDataを渡す。
      setTimeout(() => {
        resolve(sampleData)
      }, 1000)
    })
  }
  render() {
    return <div />
  }
}

export default RecipeContainer
```

### 6-3. componentDidMount内で、データを取得しstateに保存する。

Recipeコンテナーがマウントされた直後に、データの読み込みを開始するようにします。そのためにはライフサイクルイベントの一つである`componentDidMount`を利用します。またローディング中に、スピナーが表示されるようにしたいのでstateに`isLoading`を追加し、これをマウントされてからデータの読み込みまでの間`true`となっているようにします。

```js
import React, { Component } from 'react'
import { sampleData } from '../testData' // testDataファイルからsampleDataをインポートする

class RecipeContainer extends Component {
  state = {
    recipe: null, // 初期の状態ではrecipeのデータがないためnull
    isLoading: true // マウントされ、データが読み込まれるまでの間はローディング表示を行う
  }
  componentDidMount() {
    this.loadRecipe()
      .then((data) => {
        this.setState({
          isLoading: false, // 読み込み完了されたのでfalseとする
          recipe: data // 読み込んだデータを保存する
        })
      })
  }
  loadRecipe = () => {
    return new Promise(function (resolve) {
      // setTimeoutを利用して１秒の遅延を発生させ、その直後にsampleDataを渡す。
      setTimeout(() => {
        resolve(sampleData)
      }, 1000)
    })
  }
  render() {
    return <div />
  }
}

export default RecipeContainer
```

### 6-4. データを表示してみる

さて、ここまでの内容がちゃんと実装されているか試してみましょう。render内部を以下のように変更します。

```js
render() {
  const { recipe, isLoading } = this.state
  if (isLoading) return <p>Loading...</p>
  if (recipe) return <div>{recipe.name}</div>
}
```

`http://localhost:3000/`を確認してみて、レシピ名の「親子丼」が表示されていればここまでの実装は完了です。

## 7. Recipeコンポーネントを作成する

### 7-1. ファイルを作成する

次にRecipeコンテナーからデータを渡すRecipeコンポーネントを`src/components`下に作成します。このコンポーネントは、recipeとisLoadingの2つのPropsを受け取ります。このコンポーネントはAccordionコンポーネントの呼び出しを行いますが、まだAccordionコンポーネントは作成していないので、一旦コメントにしておきます。

```js
// src/components/Recipe.js
import React from 'react';

const Recipe = ({ recipe, isLoading }) => {
  return (
    <main>
      {/* <Accordion procedures={procedures} /> */}
    </main>
  );
}

export default Recipe
```

### 7-2. ローディング中とデータ取得失敗時用のコンポーネントを作成

今回作成しているアプリでは、データ取得に失敗することはありませんが実際のアプリではAPIサーバーが落ちていたりしてデータが取得出来ない場合があります。こうした時のためにローディング中と、データ取得失敗時用用のコンポーネントを作成します。

まずは、emotionを利用して、何もデータが表示されていない時にボックスだけを表示しておくプレースホルダー(Placeholder)を作成します。

```js
import React from 'react';
import styled from '@emotion/styled/macro' // Babelマクロを利用してstyledをインポート

const EmptyBox = styled.div({
  width: '300px',
  height: '300px',
  display: 'flex',
  justifyContent: 'center', // ローディングイメージを真ん中に表示(横位置)
  alignContent: 'center', // ローディングイメージを真ん中に表示(縦位置)
  border: '1px solid #ddd'
})

...省略
```

次に、RecipeWithEmptyDataというコンポーネントを作成します。作成するのは同じファイル内のEmptyBoxの下です。

```js
const RecipeWithEmptyData = (props) => {
  return <EmptyBox>{props.children}</EmptyBox>
}
```

さて、ここで`props.children`という見慣れないものが出てきました。これはReactアプリでは非常によく出てくるキーワードです。childrenというのはこのReact要素の子要素すべてを意味しています。言葉だけでは意味が分かりづらいかと思いますので、続けて`Loading`コンポーネントと、`NoRecipe`コンポーネントの2つを作りましょう。作成するのは、RecipeWithEmptyDataの下です。

```js
/** @jsx jsx */
import React from 'react';
import { css } from '@emotion/core/macro'
import styled from '@emotion/styled/macro'
import { ReactComponent as Spinner } from '../assets/Spinner.svg' //最初に保存したSpinner.svgをインポート

...省略
const Loading = () => {
  return (
    <RecipeWithEmptyData>
      <Spinner css={css({
        height: 40,
        width: 40
      })} />
    </RecipeWithEmptyData>
  )
}

const NoRecipe = () => {
  return (
    <RecipeWithEmptyData>
      <p>データの取得に失敗しました。</p>
    </RecipeWithEmptyData>
  )
}
```

ここで先程作成した、`RecipeWithEmptyData`を呼び出しており、その下にLoadingとNoRecipeコンポーネントでそれぞれ異なるReact要素を入れています。`{props.children}`の部分では、このように、コンポーネント名を持つタグで囲まれた部分の下にくるReact要素、コンポーネントのことを指します。

また、Loadingコンポーネントでは`Spinner.svg`をコンポーネントとして呼び出していることに気づいたかと思います。create-react-appでは以下のような形式で、SVGファイルをコンポーネントとしてインポートすることが可能です。

```js
import { ReactComponent as コンポーネント名 } from './ファイル名.svg';
```

### 7-3. Recipeコンポーネントを完成させる

さて、ここまで来たら作成したコンポーネントを利用してRecipeコンポーネントを完成させましょう。

```js
...省略

const Recipe = ({ recipe, isLoading }) => {
  if (isLoading) return <Loading />
  if (recipe === null) return <NoRecipe />
  const { procedures } = recipe
  return (
    <main>
      {/* <Accordion procedures={procedures} /> */}
    </main>
  );
}

export default Recipe
```

RecipeコンポーネントとRecipeコンテナーをつなぎます。

```js
import React, { Component } from 'react'
import Recipe from '../components/Recipe'
import { sampleData } from '../testData'

class RecipeContainer extends Component {
  ...省略
  render() {
    const { isLoading, recipe } = this.state;
    return <Recipe recipe={recipe} isLoading={isLoading} />
  }
}
```

再度、画面を確認してローディング画像が最初に出ていれば成功です。

## 8. Accordionコンポーネントの作成

次にAccordionコンポーネントを作成します。Accordionコンポーネントでは、まずレシピの手順をorderの値を利用して並び替え、AccordionItemコンポーネントを呼び出します。まだAccordionItemコンポーネントがありませんので、一旦はプレースホルダーとしてdiv要素のみを返す形で実装しましょう。

```js
// src/components/Accordion.js
import React from 'react'
import styled from '@emotion/styled/macro'

const Wrapper = styled.div({
  height: 'auto',
  border: '1px solid #ddd',
  display: 'flex',
  flexDirection: 'column'
})

const Accordion = ({procedures}) => {
  // orderの値で手順の並び替えを行う。
  const orderedProcedures = procedures.sort((p1, p2) => {
    return p1.order < p2.order
  })
  // mapファンクションを利用して、並び替えた手順を処理する。
  const AccordionItems = orderedProcedures.map((procedure) => {
    return <div key={`procedure-${order}`}>{order}</div>
  })
  return (
    <Wrapper id="accordion-items">
      {AccordionItems}
    </Wrapper>
  );
}

export default Accordion;
```

## 9. AccordionItemコンポーネントの作成

次に、AccordionItemを作成します。このコンポーネントは以下の特徴を持ちます。

- isOpenというステートを持ち、Accordionが開いているかどうかを判断する。
- AccordionHeaderをクリックした際に、isOpenのステータスを変更する。
- AccordionHeaderとAccordionBodyの2つのコンポーネントを呼び出す。

ここまでを実装すると以下のようになります。AccordionHeader、AccordionBodyはまだありませんので、一旦コメントアウトしています。

```js
// src/components/AccordionItem.js
import React, { Component } from 'react'
import styled from '@emotion/styled/macro'

const Wrapper = styled.div({
  width: '400px',
  display: 'flex',
  flexDirection: 'column' // headerとbodyを縦に並べる
})

class AccordionItem extends Component {
  constructor(props) {
    super(props);
    this.state = {
      isOpen: false
    }
  }
  handleToggle = (e) => {
    e.preventDefault()
    // クリックされると、isOpenの値を現在のstateの値を反転させてセットする
    this.setState(state => ({
      isOpen: !state.isOpen
    }))
  }
  render() {
    const { procedure } = this.props;
    const { isOpen } = this.state;
    return (
      <Wrapper>
        {/* <AccordionHeader procedure={procedure} handleToggle={this.handleToggle} /> */}
        {/* <AccordionBody procedure={procedure} isOpen={isOpen} /> */}
      </Wrapper>
    );
  }
}

export default AccordionItem;
```

見て分かる通り、handleToggleの内容は非常にシンプルです。ただし、レッスン3で学んだ通り、現在のstateを利用して新しいstateをセットしたい場合は、以下のような形になることに気をつけてください。

```js
this.setState(state => ({
  isOpen: !state.isOpen
}))
```

## 10. AccordionHeader、AccordionBodyコンポーネントの作成

ここまで来たら後一息です。AccordionHeader、AccordionBodyコンポーネントを実装してAccordionを完成させましょう。

### 10-1: AccordionHeaderを実装する

AccordionHeaderでは、「手順1」のように手順の順番のみを表示し、クリックするとAccordionItemの`handleToggle`ファンクションを呼び出し、AccordionBodyに手順の詳細を表示します。

実装してみましょう。

```js
// src/components/AccordionHeader.js
import React from 'react'
import styled from '@emotion/styled/macro'

const Wrapper = styled.div({
  backgroundColor: 'rgb(242, 241, 239)',
  cursor: 'pointer', // クリックできることが分かるようにマウスポインタを表示する
  padding: '10px 20px',
  height: 'auto',
  borderBottom: '1px solid #ddd'
})

const Title = styled.h3({
  fontSize: '14px',
  fontWeight: 'bold',
  margin: 0, // ブラウザごとにmarginが勝手に入ることがあるのでmarginを0とする。
})

const AccordionHeader = ({
  procedure,
  handleToggle // AccordionItemからhandleToggleファンクションを渡す
}) => (
  <Wrapper onClick={handleToggle}>
    <Title>手順{procedure.order}</Title>
  </Wrapper>
);

export default AccordionHeader
```

### 10-2: AccordionBodyを実装する

次にAccordionBodyを実装します。こちらもコンポーネント自体はシンプルですが、emotionを利用したStyled Componentに工夫が必要です。isOpenというPropsを渡して、trueなら内容を表示し、falseなら内容を隠すようにします。

```js
// src/components/AccordionBody.js
import React from 'react';
import styled from '@emotion/styled/macro'

const Wrapper = styled.div({
  width: '100%',
  height: 0, // Accordionが開いていない場合は、高さは0。
  overflow: 'hidden', // Accordionが開いていない場合は、文字の部分を非表示にする。
  transition: '0.5s',
}, ({isOpen}) => { // propsにisOpenを渡す。
  let styles = []
  if (isOpen) { // Accordionが開いている場合には、内容を表示する。
    styles.push({
      height: 'auto',
      minHight: '100px',
      padding: '10px',
    })
  }
  return styles
})

const AccordionBody = ({ procedure, isOpen }) => (
  <Wrapper isOpen={isOpen}>
    <p>
      {procedure.description}
    </p>
  </Wrapper>
)

export default AccordionBody
```

### 10-3. AccordionItemからAccordionHeaderとAccordionBodyを呼び出す。

完成したら、AccordionItemで2つのコンポーネントを呼び出します。

```js
import React, { Component } from 'react'
import styled from '@emotion/styled/macro'
import AccordionHeader from './AccordionHeader'
import AccordionBody from './AccordionBody'

...省略

class AccordionItem extends Component {
  ...省略

  render() {
    const { procedure } = this.props;
    const { isOpen } = this.state;
    return (
      <Wrapper>
        <AccordionHeader procedure={procedure} handleToggle={this.handleToggle} />
        <AccordionBody procedure={procedure} isOpen={isOpen} />
      </Wrapper>
    );
  }
}
```

## 11. すべてのコンポーネントをつなげて完成させる

さて、Accordionが完成しましたので後はすべてのコンポーネントをつなげたら完成です。

```js
// src/components/Recipe.js

import Accordion from './Accordion' //追加する

const Recipe = ({ recipe, isLoading }) => {
  if (isLoading) return <Loading />
  if (recipe === null) return <NoRecipe />
  const { procedures } = recipe
  return (
    <main>
      <Accordion procedures={procedures} />
    </main>
  );
}
```

```js
// src/components/Accordion.js

import AccordionItem from './AccordionItem' // 追加する

const Accordion = ({procedures}) => {
  const orderedProcedures = procedures.sort((p1, p2) => {
    return p1.order < p2.order
  })
  const AccordionItems = orderedProcedures.map((procedure) => {
    return <AccordionItem 
      key={`procedure-${procedure.id}`}
      procedure={procedure} />
  })
  return (
    <Wrapper id="accordion-items">
      {AccordionItems}
    </Wrapper>
  );
}
```

ここまで出来たらアプリを再度確認してみましょう。次のように表示されていれば無事完成です。

![recipe-accordion-sample.gif](https://firebasestorage.googleapis.com/v0/b/codegrit-images.appspot.com/o/codegrit-react%2FLesson07%2Frecipe-accordion-sample.gif?alt=media&token=5a80d2b1-9ad6-4025-b30e-c688320fad0c)

## 更に学ぼう

- [React公式 - イベント処理](https://ja.reactjs.org/docs/handling-events.html)
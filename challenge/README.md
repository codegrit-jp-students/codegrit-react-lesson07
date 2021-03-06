# チャレンジ7

## 目的

- データの追加読み込みを実装する
- イベントハンドリングにより慣れる

## チャレンジの取り組み方

1. スターターファイルのリポジトリを、フォークして自分のGitHubアカウントにリポジトリを作りましょう。
2. マイルストーンごとに要件に合うようにファイルを編集していきます。
3. 分からない部分があれば、テキストを復習して、再度チャレンジしてみましょう。
4. 再チャレンジしてしばらく考えても分からない場合はチャットでメンターに質問しましょう。
5. 完了したら、GitHubリポジトリをメンターにシェアしてください。(質問時に既にシェアしている場合は、レビューを直接依頼しましょう。)
6. メンターから課題レビューが届きます。
7. ビデオチャットの際は、分からない点を更に突っ込んで聞いたり、より良い書き方を聞いてみましょう。

## 概要

このレッスンではHTML/CSSコースのチャレンジ4で作成したチャットルームリストをアップデートして、スクロールが行われると更にページを表示していくような無限スクロールを仮想的に実装しましょう。実際には、データベースやサーバーサイドの実装が必要なので今回作成するものと少し違いますが考え方は同一です。

## 完成イメージ

![codegrit-react-ch07-image](https://firebasestorage.googleapis.com/v0/b/codegrit-images.appspot.com/o/codegrit-react%2FLesson07%2Fchallenge%2Fcodegrit-react-ch07-image.gif?alt=media&token=490a414b-94cc-450e-b982-6856d8792ece)

## スターターファイル

- [codegrit-jp-students/react-ch07-starter](https://github.com/codegrit-jp-students/codegrit-react-ch07-starter)

上記のURL先のリポジトリをフォークして、コードを書き進めて行きましょう。

## マイルストーン１

このチャレンジでは、既に必要なファイルが以下のツリーのように配置されています。既に配置されたコンポーネントを編集していき、完成イメージと同じものを作成しましょう。

![srcフォルダーのツリー構造](https://firebasestorage.googleapis.com/v0/b/codegrit-images.appspot.com/o/codegrit-react%2FLesson07%2Fchallenge%2Fcodegrit-react-ch07-tree.png?alt=media&token=36bd76a8-6cb0-4d28-87d7-ec0514cbb0d8)

### 要件

- componentDidMountでデータを読み込み、結果を表示させましょう。

- ConversationListコンポーネントにローディング中の表示を加えます。

ConversationListを編集して、最初のローディング中にローディングイメージが表示されるようにしましょう。既に定義されているEmptyBoxコンポーネントを利用してください。

- LoadMoreコンポーネントを実装しましょう。

LoadMoreコンポーネントは以下の4つの状態を持ちます。

1. 最初のローディング時 - 何も表示しない
2. 最初のローディング終了後でhasNextPageがtrue - 「更に読み込む」と表示
3. 「更に読み込む」をクリックしローディング中 - ローディングイメージを既にある会話一覧のすぐ下に表示(幅、高さは40px)
4. hasNextPageがfalse - 「これ以上ありません」と表示

これらの4つの表示を渡されたPropsの値に応じて行いましょう。ヒントとして、LoadMoreBoxとLoadMoreMessageというStyled Componentを既に書いてあります。また`src/Chat/index.js`内の初期ステートも参考にしてください。

- fetchMoreConversationsを実装する

fetchMoreConversationsファンクションを実装し、LoadMoreコンポーネントの「更に読み込む」という文字がクリックされた際に、会話を更に読み込むようにしましょう。最初の会話データと読み込まれたデータは時系列順につながなければなりません。

- 現在選ばれている会話の部分の色を変更する

ConversationListItemコンポーネントに渡される`isChosen`というPropsを設定し、選ばれている会話の背景色が変わるようにしましょう。また、chatコンポーネントの`handleChooseConversation`ファンクションを編集して、会話がクリックされた時に`chosenId`が変わるようにしましょう。(ただし異なるIDの会話がクリックされた場合のみ)

- getFormattedDateファンクションを実装する

最後にConversationListItemコンポーネントのgetFormattedDateファンクションを利用して、最後のメッセージが送られた日時を以下の3つの場合に分けて表示するようにしましょう。

- 現在時刻から7日未満 - 1日前、数秒前のように現在時刻からの時間を表示する。
- 7日以上前かつ、年の開始時より後 - 5月1日のように月日のみを表示します。
- 年の開始時より前 - 2018年11月15日のように年月日を表示します。

## 評価

課題の後、以下の２つについてメンターにフィードバックをお願いします。

1. 要件のカバー度: 1.全く出来なかった 2.ほとんど出来なかった 3. 半分ほどは出来た 4.8割ほどは出来た 5. 全部出来た
2. 難易度: 1. とても難しかった 2. 難しかった 3. ちょうど良かった 4. 簡単だった 5. とても簡単だった

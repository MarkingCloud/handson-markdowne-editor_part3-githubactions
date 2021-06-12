# 7. Firestore を使ってみる

Firebase の DB である Cloud Firestore を利用してみましょう。

<img :src="$withBase('/firestore2.png')">

## 1. Cloud Firestore とは

Firebase が標準で用意している **NoSQL** の DB です。

ここで **RDB** と **NoSQL** の違いを説明しておきます。

**RDB** とは**データの整合性**に重点を置き、データを比較/調整しながらを処理する DB の事です。  
Oracle Database、MySQL などはこれを操作するため **RDBMS** と呼ばれます。

**NoSQL** とは RDB 以外の全ての DB の事を指します。  
データの整合性よりも**早く処理できること**を優先しているという特徴があります。

特徴を比較すると以下のようになります。

|                                | RDB                                    | NoSQL                                    |
| ------------------------------ | -------------------------------------- | ---------------------------------------- |
| 重点                           | データの整合性を重視                   | 処理が速く軽量である点を重視             |
| データ形式                     | 行列による管理                         | json/ドキュメント/Key-Value 指向など様々 |
| データ操作                     | SQL 文を利用                           | DB ごとに独自実装、共通言語は無い        |
| データの精度                   | 高い                                   | 低い                                     |
| 分散性<br>(多拠点展開への対応) | 低い                                   | 高い                                     |
| サービス例                     | Oracle Database<br>MySQL<br>PostgreSQL | Cloud Firestore<br>MongoDB<br>Redis      |

Cloud Firestore はドキュメント指向の NoSQL になります。  
**コレクション**と**ドキュメント**ごとに**オブジェクト**でデータを管理することができます。

## 2. Firestore に書き込みを行う

Firestore にデータを書き込む処理を実装してみましょう。

次の操作を行ってください。

- `store/memos.js` を次の通り編集する
  - L100 ～ L110 のコメントアウトを解除

`code.7-1` _store/memos.js_

```js{5-15}
// 非同期処理を実行する
export const actions = {
  // Firestoreに要素を保存する
  async saveDB(context, { saveId, saveData }) {
    await context.commit('save', { id: saveId, data: saveData })
    const db = this.$fire.firestore.collection('markdowns').doc(saveId)
    try {
      db.set({
        text: saveData.text,
        title: saveData.title,
        timestamp: saveData.timestamp,
      })
    } catch (e) {
      alert(e)
    }
  },
}
```

- 次のコマンドを実行して Nuxt アプリをローカルで立ち上げる。

`code.7-2` _shell_

```properties
npm run dev
```

- 出力された URL をクリックする。

<img :src="$withBase('/dev.png')">

MarkdownEditor ページの SAVE をクリックすることでデータが格納されて入れば成功です。  
この時**事前の型定義などが一切不要**である点に注目してください。

<img :src="$withBase('/save.gif')">

## 3. Firestore からデータを読み込む

Firestore からデータを読み込んでみましょう。

次の操作を行ってください。

- `store/memos.js` を次の通り編集する
  - L69 ～ L82 のコメントアウトを解除
  - L86 ～ L96 のコメントアウトを解除

`code.7-3` _store/memos.js_

```js{5-18,22-32}
// 非同期処理を実行する
export const actions = {
  // Firestoreからデータを取得してリストを更新する
  async readDB(context) {
    // ローカルの情報が最新の場合Firestoreにアクセスしない
    const isListLatest = await context.dispatch('checkListLatest')
    if (isListLatest === true) return
    // データ取得
    await context.commit('reset')
    const db = this.$fire.firestore.collection('markdowns').orderBy('timestamp', 'desc')
    try {
      const snapshot = await db.get()
      snapshot.forEach((doc) => {
        context.commit('add', { id: doc.id, data: doc.data() })
      })
    } catch (e) {
      alert(e)
    }
  },
  // Firestoreの変更を検知してリストを更新する
  listenDB(context) {
    const db = this.$fire.firestore.collection('markdowns').orderBy('timestamp', 'desc').limit(1)
    try {
      db.onSnapshot(async (snapshot) => {
        await snapshot.forEach((doc) => {
          context.commit('save', { id: doc.id, data: doc.data() })
        })
        context.commit('sort')
      })
    } catch (e) {
      alert(e)
    }
  },
}
```

ローカルの Nuxt アプリを更新して、データが取得できていれば成功です。  
`db.onSnapshot`というメソッドを利用して、リアルタイムにデータを取得するようにしています。

## 4. Firestore を上手に使うための工夫

Firestore の料金体系は次のようになっています。

[https://cloud.google.com/firestore/pricing](https://cloud.google.com/firestore/pricing)

> ### 料金の概要
>
> Firestore を使用すると、以下の項目に対し課金されます。
>
> - 読み取り、書き込み、削除するドキュメントの数。
> - データベースにより使用されるストレージの容量（メタデータとインデックスのオーバーヘッドを含む）。
> - ネットワーク帯域幅の使用量。

そのため Firestore と**なるべく通信させないような工夫**が必要になります。

今回のサービスでは SessionStrage のデータと Firestore の最新データの比較し、  
無駄に全データ取得の処理が走らないようにしています。

_store/memos.js_

```js{3-5}
  // Firestoreからデータを取得してリストを更新する
  async readDB(context) {
    // ローカルの情報が最新の場合Firestoreにアクセスしない
    const isListLatest = await context.dispatch('checkListLatest')
    if (isListLatest === true) return
    // データ取得
    (略)
  },
```

::: details checkListLatest の詳細

```js
// ローカルの情報が最新かチェックする
  async checkListLatest() {
    // セッションストレージの最新更新時間を取得
    const localList = await JSON.parse(sessionStorage.getItem('markdownEditor')).memos.list
    if (!localList) return
    const localUpdateItem = await localList.reduce((a, b) =>
      new Date(a.data.timestamp) > new Date(b.data.timestamp) ? a : b
    )
    const localUpdateTime = localUpdateItem.data.timestamp
    // Firestoreの最新更新時間を取得
    const db = this.$fire.firestore.collection('markdowns').orderBy('timestamp', 'desc').limit(1)
    let dbUpdateTime = new Date()
    try {
      const snapshot = await db.get()
      await snapshot.forEach((doc) => {
        dbUpdateTime = doc.data().timestamp
      })
    } catch (e) {
      alert(e)
      return
    }
    // ローカルとDBの最新更新時間比較
    if (new Date(localUpdateTime) < new Date(dbUpdateTime)) {
      return false
    } else {
      return true
    }
  },
```

:::

## 5. 完成したサービスをデプロイする

では完成したサービスをデプロイしましょう。

次の操作を行ってください。

- 次のコマンドを実行する

`code.7-4` _shell_

```properties
npm run build
npm run generate
firebase deploy
```

これで Firestore と連携したサービスを公開することができました！

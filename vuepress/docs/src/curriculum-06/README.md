# 6. nuxt/firebase を導入する

Firestore(Firebase の持つ DB)を利用するために SDK をインストールする必要があります。  
[nuxt/fierbase](https://firebase.nuxtjs.org/)は nuxt に Firebase SDK を簡単に統合できるモジュールです。

<img :src="$withBase('/nuxtfirebase.png')">

## 1. nuxt/firebase をインストールする

nuxt/firebase をプロジェクトへインストールして使えるようにしましょう。

次の操作を行ってください。

- 次のコマンドを実行してモジュールをインストールする。

`code.6-1` _shell_

```properties
npm install firebase
npm install @nuxtjs/firebase
```

- `nuxt.config.js` を次の通り編集する。
  - L50 のコメントアウトを解除
  - L79 ～ L93 のコメントアウトを解除

`code.6-2` _nuxt.config.js_

```js{4,7-20}
export default {
  modules: [
    '@nuxtjs/dotenv',
    '@nuxtjs/firebase', //firebase
  ],

    firebase: {
    config: {
      apiKey: process.env.API_KEY,
      authDomain: process.env.AUTH_DOMAIN,
      projectId: process.env.PROJECT_ID,
      storageBucket: process.env.STORAGE_BUCKET,
      messagingSenderId: process.env.MESSAGING_SENDER_ID,
      appId: process.env.APP_ID,
      measurementId: process.env.MESSAGING_SENDER_ID,
    },
    services: {
      firestore: true,
    },
  },
}
```

## 2. SDK の構成情報を env ファイルに書き込む

SDK を利用するために必要な ID などの情報を入力します。  
Github などに push すると問題なため、env ファイルに記述して呼び出すようにします。

次の操作を行ってください。

- Firebase コンソールから次の手順で SDK の構成情報を確認する。
  - 歯車マーク > プロジェクトを設定 > SDK の設定と構成-構成 を選択

<img :src="$withBase('/config.png')">

- `.env` を次の通り編集する。
  - 対応する値を「""」の中に記入する。

`code.6-3` _.env_

```json{1-6}
API_KEY="xxxxxxxxxxxxxxxx"
AUTH_DOMAIN="xxxxxxxx.firebaseapp.com"
PROJECT_ID="xxxxxxxx"
STORAGE_BUCKET="xxxxxxxx.appspot.com"
MESSAGING_SENDER_ID="xxxxxxxx"
APP_ID="x:xxxxxxxx:xxxxxxxx"
```

これで nuxt/firebase の導入は完了となります。

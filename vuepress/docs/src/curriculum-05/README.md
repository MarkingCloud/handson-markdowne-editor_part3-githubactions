# 5. Hosting を使ってみる

Hosting を使って web ページのデプロイを行ってみましょう。

<img :src="$withBase('/hosting2.png')">

## 1. さっそくデプロイしてみる

さっそくですが Hosting にデプロイしてみましょう。

Gitpod のコンソールから次の操作を行ってください。

- 次のコマンドを実行する

`code.5-1` _shell_

```properties
firebase deploy
```

以上でデプロイは完了です。（簡単！）

出力された URL にアクセスしてみましょう。

<img :src="$withBase('/deploy.png')">

## 2. ちょっと解説

Hosting へのデプロイについて簡単に解説を行います。

`Firebase deploy` コマンドによって `public/` が Hosting 上へデプロイされます。  
どのフォルダが対象となるかは `firebase.json` の中に記述されています。

_firebase.json_

```json{7}
{
  "firestore": {
    "rules": "firestore.rules",
    "indexes": "firestore.indexes.json"
  },
  "hosting": {
    "public": "public",
    "ignore": ["firebase.json", "**/.*", "**/node_modules/**"]
  }
}
```

Firebase Hosting ではデフォルトで次のようなことを自動で行ってくれます。

- ドメインの付与
- TLS による暗号化（HTTPS 化）

## 3. Nuxt アプリをデプロイする

では Nuxt で作成したアプリをビルドしてデプロイしてみましょう。

次の操作を行ってください。

- `nuxt.config.js` を次の通り編集してビルド結果が `public/` に格納されるように変更。
  - L75 ～ L77 のコメントアウトを解除

`code.5-2` _nuxt.config.js_

```js{2-4}
export default {
  generate: {
    dir: 'public', // デフォルトは`dist`
  },
}
```

- 次のコマンドで Nuxt アプリをビルドする。

`code.5-3` _shell_

```properties
npm run build
npm run generate
```

::: tip データ収集への協力するか聞かれますが、Yes/No で大丈夫です。

```shell
? Are you interested in participating? 'Yes'
```

:::

- 次のコマンドを実行してデプロイを行う。

`code.5-4` _shell_

```properties
firebase deploy
```

発行 URL に再度アクセスしてページが変更されていれば成功です。

<img :src="$withBase('/page.png')">

これで Hosting へのデプロイができるようになりました。

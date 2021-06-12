# 7. Firebase にデプロイする

Firebase にデプロイしてみましょう。

## 1. トークンを取得する

Firebase を CI/CD ツールなどから操作するために、トークンを発行する必要があります。

次の操作を行ってください。

- 次のコマンドを実行する。

`code.7-1` _shell_

```properties
firebase login:ci --no-localhost
```

- 出力された URL をクリックする。
- Firebase にログインしているアカウントと同じアカウントを選択。
- 権限を確認して許可をクリックする。
- 出力された認証コードをコンソールに貼り付けてエンターを押下する。

<img :src="$withBase('/login.gif')">

- 出力された `1//` から始まるトークンをコピーしてメモしておく。

::: danger トークンの取り扱いに注意！
このトークンは**アカウントに紐づく**ものになっています。  
トークンが漏出すると**Firebase の全リソースの操作権限を渡す**ことになり、大変危険です。  
取り扱いには十分注意してください。

失効させたい時は次のコマンド利用します。

```properties
firebase logout トークン文字列
```

:::

## 2. プロジェクト ID を取得する

デプロイするためにプロジェクト ID が必要になります。

次の操作を行ってください。

- 次のコマンドで先ほど作成したプロジェクトのプロジェクト ID を確認してメモしておきます。

`code.7-2` _shell_

```properties
firebase projects:list
```

出力

```shell
$ firebase projects:list

✔ Preparing the list of your Firebase projects
┌─────────────────────────────┬──────────────────────┬────────────────┬──────────────────────┐
│ Project Display Name        │ Project ID           │ Project Number │ Resource Location ID │
├─────────────────────────────┼──────────────────────┼────────────────┼──────────────────────┤
│ test-markdown-0501          │ 'test-markdown-0501' │ xxxxxxxxxxxxx  │ asia-northeast1      │
└─────────────────────────────┴──────────────────────┴────────────────┴──────────────────────┘
```

::: tip プロジェクト ID
プロジェクト ID は「プロジェクトを設定」からも確認可能です。

<img :src="$withBase('/id.png')">
:::

## 3. Github Secrets に登録する

先ほどメモしたトークンとプロジェクト ID を GIthub の Secrets に登録します。

次の操作を行ってください。

- Github のリポジトリ > Settings > Secrets > New repository secret を選択
- Firebase のトークンを登録
  - Name：`FIREBASE_TOKEN`
  - Value：出力されたトークン
- プロジェクト ID を登録
  - Name：`FIREBASE_PROJECT`
  - Value：メモしたプロジェクト ID

<img :src="$withBase('/secret.png')">

## 4. Firebase にデプロイする

Firebase へデプロイする処理を記述しましょう。

次の操作を行ってください。

- `.github/workflows/ci.yml` を次の通り編集する。
  - L30 ～ L33 のコメントアウトを解除

`code.7-3` _.github/workflows/ci.yml_

```yml{30-33}
name: deploy

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: '14.15.0'

      - name: npm install
        run: npm install

      - name: eslint
        run: |
          npx eslint "pages/*.vue"
          npx eslint "components/*.vue"
          npx eslint "store/*.js"

      - name: generate
        run: npm run generate

      - name: deploy
        run: |
          npx firebase use ${{ secrets.FIREBASE_PROJECT }} --token=${{ secrets.FIREBASE_TOKEN }}
          npx firebase deploy --only hosting --token=${{ secrets.FIREBASE_TOKEN }}
```

- 次のコマンドを実行する。

`code.7-4` _shell_

```properties
git add .
git commit -m "Firebaseにデプロイする"
git push origin HEAD
```

Firebase にデプロイされていることを確認しましょう！

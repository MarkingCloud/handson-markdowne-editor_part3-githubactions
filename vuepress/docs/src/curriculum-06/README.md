# 6. テストしてビルドする

テストが失敗したパターンを確認し、ビルドまでの記述を行いましょう。

## 1. テストに失敗させてみる

CI の中身として、ESLint での簡易的なテストを実施してから、ビルドする処理を記述しましょう。

次の操作を行ってください。

- `.github/workflows/ci.yml` を次の通り編集する。
  - L21 ～ L28 のコメントアウトを解除

`code.6-1` _.github/workflows/ci.yml_

```yml{21-28}
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

    #   - name: deploy
    #     run: |
    #       npx firebase use ${{ secrets.FIREBASE_PROJECT }} --token=${{ secrets.FIREBASE_TOKEN }}
    #       npx firebase deploy --only hosting --token=${{ secrets.FIREBASE_TOKEN }}
```

- 次のコマンドを実行する。

`code.6-2` _shell_

```properties
git add .
git commit -m "テストに失敗させてみる"
git push origin HEAD
```

今回、余分な一文をコードに書いているため ESLint が通らず、失敗となります。

Actions のログからどこで失敗となっているのか確認しましょう。

<img :src="$withBase('/error.png')">

失敗の原因になっている一文を削除します。

次の操作を行ってください。

- `pages/home.vue`を次の通り編集する。
  - L9 の一文を削除

`code.6-3` _pages/home.vue_

```diff{9}
<template>
  <v-row>
    <v-col class="text-center">
      <img src="/markingcloud-icon.png" />
      <blockquote class="blockquote">
        &#8220;NO CLOUD, NO LIFE&#8221;
        <footer>
          <small>
-           <>エラー原因の一文
            <em>&mdash;by MarkingCloudTest</em>
          </small>
        </footer>
      </blockquote>
    </v-col>
  </v-row>
</template>
```

- 次のコマンドを実行する。

`code.6-4` _shell_

```properties
git add .
git commit -m "エラー原因の一文削除"
git push origin HEAD
```

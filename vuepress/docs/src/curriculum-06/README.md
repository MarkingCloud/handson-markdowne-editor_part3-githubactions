# 6. テストしてビルドする

テストが失敗したパターンを確認し、ビルドまでの記述を行いましょう。

## 1. テストに失敗させてみる

CI の中身として、ESLint での簡易的なテストを実施してから、ビルドする処理を記述しましょう。

次の操作を行ってください。

- `.github/workflows/ci.yml` を次の通り編集する。
  - L21 ～ L28 のコメントアウトを解除

`code.6-1` _.GitHub/workflows/ci.yml_

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

- 次のコマンドでエラーになるような分を挿入する。

`code.6-2` _shell_

```properties
echo エラー原因の一文 >> store/memos.js
```

- 次のコマンドを実行する。

`code.6-3` _shell_

```properties
git add .
git commit -m "テストに失敗させてみる"
git push origin HEAD
```

今回、余分な一文をコードに書いているため ESLint が通らず、失敗となります。

コミットログからどこで失敗となっているのか確認しましょう。

<img :src="$withBase('/error.png')">

## 2. エラー原因の一文を削除

失敗の原因になっている一文を削除します。

次の操作を行ってください。

- `store/memos.js`を次の通り編集する。
  - L124 の一文を削除

`code.6-4` _store/memos.js_

```diff{13}
(略)
  // FireStoreの要素を削除する
  async removeDB(context, removeId) {
    await context.commit('remove', removeId)
    const db = this.$fire.firestore.collection('markdowns').doc(removeId)
    try {
      db.delete()
    } catch (e) {
      alert(e)
    }
  },
}
- エラー原因の一文
```

- 次のコマンドを実行する。

`code.6-5` _shell_

```properties
git add .
git commit -m "エラー原因の一文削除"
git push origin HEAD
```

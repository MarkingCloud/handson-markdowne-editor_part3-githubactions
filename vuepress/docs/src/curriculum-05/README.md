# 5. テンプレート を使ってみる

Github の提供しているテンプレートを使ってみましょう。

<img :src="$withBase('/marketplace.png')">

## 1. 基本的な使い方

まずは Github Actions の基本的な使い方を見てみましょう。

Actions の機能は、`.github/workflows/` 配下に YAML ファイルを配置するだけで利用できます。

複数のファイルを使って並列に実行させることも可能です。 ([**こちら**](https://blog.kondoumh.com/entry/2021/01/22/133427)のブログが参考になります。)

YAML ファイルの中身は以下のような構成で記述します。

```yml
name: # ワークフローの名前
on: # トリガーするGitHubイベントの名前
jobs: # 実行する処理
  ジョブ名
    steps: # Jobが実行する処理の集合
      各アクション
```

今回のハンズオンで登場するアクションは次の通り。

```yml
uses: パブリックリポジトリ、または公開されているアクションを使用
name: GitHubで表示されるステップの名前。
run: シェルによって実行されるコマンド
```

## 2. Marketplace とは

Marketplace には Github ユーザーが作った Apps や Actions が投稿されています。

Marketplace へは Github の管理ページ上部のタブから飛ぶことができます。

試しに Node の環境セットアップのワークフローを見つけてみましょう。

次の操作を行ってください。

- Github の管理ページ > Marketplace を選択
- Types で Actions を選択
- 検索窓に「node」を打ち込みエンターで検索
- Setup Node.js environment を選択

<img :src="$withBase('/node.png')">

## 3. Setup Node.js environment を使ってみる

Setup Node.js environment の Usage に従って Node の環境構築の記述を行いましょう。

次の操作を行ってください。

- `.github/workflows/ci.yml` を次の通り編集する。
  - L12 ～ L19 のコメントアウトを解除

`code.5-1` _.github/workflows/ci.yml_

```yml{12-19}
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

    #   - name: eslint
    #     run: |
    #       npx eslint "pages/*.vue"
    #       npx eslint "components/*.vue"
    #       npx eslint "store/*.js"
    #
    #   - name: generate
    #     run: npm run generate

    #   - name: deploy
    #     run: |
    #       npx firebase use ${{ secrets.FIREBASE_PROJECT }} --token=${{ secrets.FIREBASE_TOKEN }}
    #       npx firebase deploy --only hosting --token=${{ secrets.FIREBASE_TOKEN }}
```

- 次のコマンドを実行する。

`code.5-2` _shell_

```properties
git add .
git commit -m "テンプレートを使ってみる"
git push origin HEAD
```

では Github の Actions タブから処理が実行している様子を見てみましょう。

<img :src="$withBase('/node.png')">

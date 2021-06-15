# 4. 事前準備

## 1. リポジトリを用意する

利用するリポジトリを用意します。

次の操作を行ってください。

- [**GitHub**](https://GitHub.com) に移動する。
- Repositories > New を選択
- Repository name へ適当な名前を入力する。
- Public を選択する。
- Create Repository を選択する。

<img :src="$withBase('/github.png')">

## 2. Gitpod でコードを開く

Gitpod を開いてコードをクローンしましょう。

次の操作を行ってください。

- [**Gitpod に登録**](https://gitpod.io/login)して、管理画面に移動する。
- 必要な権限を許可する。
  - setting > Integrations > \[...\] > Edit Permissions を選択。
  - 次の項目にチェックを入れ、Update Permissions を選択。
    - public_repo
    - workflow
  - Authorize gitpod-io を選択し、パスワードを入力。
- Gitpod の管理画面はこれ以上操作しないため、タブを閉じる。

<img :src="$withBase('/setting.png')">

- [**Gitpod で今回のリポジトリを開く**](http://gitpod.io/#https://github.com/MarkingCloud/handson-markdowne-editor_part3-GitHubactions)

::: tip Gitpod とは

- ブラウザでアクセス可能な、クラウドベースの統合開発環境(IDE)
- リポジトリ URL の頭に`gitpod.io/#`を付けるだけで簡単に開発環境を構築
- 50 時間/月まで無料で利用可能
- [https://www.gitpod.io/](https://www.gitpod.io/)

:::

::: tip テーマをダークモードに変更

テーマをダークモードに変更したい場合は次の手順を実行してください。

- 歯車マーク > Color Theme > Dark (Visual Studio) を選択

<img :src="$withBase('/dark.png')">

:::

ディレクトリ構成は次のような構成になっています。

色々とファイルがありますが、今回利用するのは`.github/workflows/ci.yml`のみです。

```shell{3-5}
handson-markdowne-editor_part2-firebase
├──.firebase
├──.GitHub
│   └── workflows
│       └── 'ci.yml   <----------- GitHub Actions の設定ファイル'
├── assets
├── components
├── firebase.json
├── firestore.indexes.json
├── firestore.rules
├── layouts
├── middleware
├── node_modules
├── nuxt.config.js
├── package.json
├── package-lock.json
├── pages
├── plugins
├── README.md
├── static
├── store
└── .env
```

## 3. Firebase プロジェクトを作成する

Firebase のプロジェクトを作成しましょう。

[**Firebase コンソール**](https://console.firebase.google.com/)を開き、次の操作を行ってください。

- 「プロジェクトを追加」を選択。
- 好きなプロジェクト名を入力して「続行」を選択。
- Google アナリティクスは無効にして「プロジェクトを作成」を選択。

<img :src="$withBase('/project.png')">

## 4. リモートリポジトリを変更する

Gitpod 上から新しく作ったリポジトリへリモートリポジトリの設定を変更します。

次の操作を行ってください。

- 先ほど作成したリポジトリの URL をコピーする。
  - **HTTPS**の方をコピー

<img :src="$withBase('/url.png')">

- 次のコマンドを実行してリモートリポジトリを変更する。

`code.4-1` _shell_

```properties
git remote set-url origin コピーしたURL
```

ここまでで準備は完了です。

# 4. 事前準備

## 1. Gitpod でコードを開く

Gitpod を開いてコードをクローンしましょう。

[**Gitpod に登録する**](https://gitpod.io/login)

[**Gitpod で今回のリポジトリを開く**](http://gitpod.io/#https://github.com/MarkingCloud/handson-markdowne-editor_part2-firebase)

<img :src="$withBase('/gitpod.png')">

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
少しややこしいですが、Nuxt のルートディレクトリで `firebase init` を行っただけの状態です。  
(今回は時短のため init 作業は省略します)

```shell{4,10,15,19}
handson-markdowne-editor_part2-firebase
├── assets
├── components
├── 'firebase.json   <----------- Firebase の設定ファイル'
├── firestore.indexes.json
├── firestore.rules
├── layouts
├── middleware
├── node_modules
├── 'nuxt.config.js  <----------- Nuxt の設定ファイル'
├── package.json
├── package-lock.json
├── pages
├── plugins
├── 'public          <----------- デプロイする Build 後のファイル'
├── README.md
├── static
├── store
└── '.env            <----------- 接続情報'
```

## 2. Firebase プロジェクトを作成する

Firebase のプロジェクトを作成しましょう。

[**Firebase コンソール**](https://console.firebase.google.com/)を開き、次の操作を行ってください。

- 「プロジェクトを追加」を選択。
- 好きなプロジェクト名を入力して「続行」を選択。
- Google アナリティクスは無効にして「プロジェクトを作成」を選択。

<img :src="$withBase('/project.png')">

## 3. Firebase プロジェクトを作成する

作成したプロジェクトを Gitpod に紐づけます。

Gitpod のコンソールから以下の操作を行ってください。

- 次のコマンドを実行してアカウントでログインする。

`code.4-1` _shell_

```properties
firebase login --no-localhost --reauth
```

- 出力された URL をクリックする。
- Firebase にログインしているアカウントと同じアカウントを選択。
- 権限を確認して許可をクリックする。
- 出力された認証コードをコンソールに貼り付けてエンターを押下する。

<img :src="$withBase('/login.gif')">

- 次のコマンドで先ほど作成したプロジェクトのプロジェクト ID を確認する。

`code.4-2` _shell_

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

- 次のコマンドで作成したプロジェクトを設定する。

`code.4-3` _shell_

```properties
firebase use <PROJECT_ID> // <PROJECT_ID>は先ほど確認したものに書き換えます
```

- 再度確認コマンドを実施し、正しいプロジェクトに (current) とついていることを確認する。

`code.4-4` _shell_

```properties
firebase projects:list
```

出力

```shell
$ firebase projects:list

✔ Preparing the list of your Firebase projects
┌─────────────────────────────┬────────────────────────────────┬────────────────┬──────────────────────┐
│ Project Display Name        │ Project ID                     │ Project Number │ Resource Location ID │
├─────────────────────────────┼────────────────────────────────┼────────────────┼──────────────────────┤
│ test-markdown-0501          │ test-markdown-0501 '(current)' │ xxxxxxxxxxxx   │ asia-northeast1      │
└─────────────────────────────┴────────────────────────────────┴────────────────┴──────────────────────┘
```

## 4. アプリを作成する

Firebase ではアプリという単位でリソースを管理します。

次の操作を行ってください。

- コンソールトップ画面の web マーク (</>) を選択
- 任意のアプリのニックネームを設定
- 「このアプリの Firebase Hosting も設定します。」にチェック
- 「アプリを登録」を選択
- ②③④ は事前に準備済みなの部分なので何もせず「次へ」

<img :src="$withBase('/webapp.png')">

ここまでで準備は完了です。

## 5. Firestore を作成する

コンソールから Firestore を有効化します。

次の操作を行ってください。

- Firestore > データベースの作成　を選択
- テストモードで開始する　を選択
- asia-northeast1 > 有効にする　を選択

<img :src="$withBase('/storeconsole.png')">

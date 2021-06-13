# 3. GitHub Actions とは

CI/CD と GitHub Actions について簡単に説明しておきます。

## CI/CD とは

CI/CD とは「Continuous Integration／Continuous Delivery」の略称です。

日本語では**継続的インティグレーション／継続的デリバリー**といいます。

チーム間の連携を円滑にするために、開発から運用までの流れを体系化させたモノを指します。

([**こちらの資料**](https://www2.circleci.com/rs/485-ZMH-626/images/001_DevOpsCulture.pdf)が CI/CD の考え方についてよくまとめられていました。)

自動でビルド・テストまでする仕組みを **CI**、デプロイまで自動化されたものを **CD** と言います。

<img :src="$withBase('/cicd.png')">

## GitHub Actions の特徴

GitHub Actions は GitHub の提供する CI/CD ツールです。次のような特徴があります。

- リポジトリとの紐づけ作業などが不要で導入が簡単
- ワークフローをパッケージ化して公開することができる
- パブリックリポジトリでの利用なら無料

### リポジトリとの紐づけ作業などが不要で導入が簡単

サードパーティー製のサービスであればリポジトリとの紐づけ作業などが必要です。

対して Actions は yml ファイルを決まった場所に作成するだけで、**設定不要で勝手に動きます**。

Actions の動きを確認したいときはリポジトリの Actions タブから見ることができます。

<img :src="$withBase('/actions2.png')">

### ワークフローをパッケージ化して公開することができる

作成したワークフローはパッケージにして [Marketplace](https://GitHub.com/marketplace) に登録することができます。

大量のリポジトリでワークフローを使いまわす場合、パッケージ化することで管理が楽になります。

<img :src="$withBase('/marketplace.png')">

### パブリックリポジトリでの利用なら無料

パブリックリポジトリであれば無料で使えるため、基本金額の心配は必要ないかと思います。

詳細な料金は[**プランごとの価格**](https://GitHub.co.jp/pricing.html)や[**Actions の支払いについて**](https://docs.GitHub.com/ja/billing/managing-billing-for-GitHub-actions/about-billing-for-GitHub-actions)から確認して下さい。

<img :src="$withBase('/place.png')">

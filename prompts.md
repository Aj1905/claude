company toiu

companyというディレクトリを作って、その中で所属する各企業の書類等をまとめたい

kamomemon, nocall, alanse, ARCRA, stardy, AJ, S

https://github.com/Aj1905/my_data/tree/main/app このアプリ上で僕に関するさまざまなデータをmdファイルとしてDBに保管している。これをコンテキストとして読み込んで、いろんなタスクをやってくれる秘書を作りたい

ただしこちら側からはデータの書き換え不可にして

Aj1905

githubリポジトリ上にはmdファイル全てをpushしていない

publicにしたよ

vercelにdeployした　https://mydata-sigma.vercel.app/

これまでのプロンプト、これからのプロンプトをあるmdファイルで保存してほしい

ただ並べるだけでいい

プロンプト管理ファイルを一個用意して

rootちょっかで

claudeディレクトリ全体をgithubで管理したい　Aj1905にpushして

ユーザー生成物のみ　後悔してはいけないものを適切にignoreして

制約: この環境には git も sudo もありません。GitHub REST API (curl) 経由でリポジトリ作成＆ファイルpushします（操作は十分可能）。

どういうこと

このサーバ上ではgitが入れられないので直接コミット履歴等を管理できないが、API経由でgithubにアクセスできるのでリモートでは管理できるということ？

今の情報をREADMEにまとめといて

githubのHTTPアクセストークンお発行の仕方を教えて

administration ha

今回のみ、contentsはpushするたびに必要？

pushは今後ずっと行うので期限は長めにしたほうがいい？

もう一回read and writeのトークンの設定を教えて

github_pat_<REDACTED>　リポジトリはclaude

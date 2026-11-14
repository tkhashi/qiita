---
title: Better_errors導入後、エラー画面が変わらない
tags:
  - Ruby
  - Gem
  - Rails5
private: false
updated_at: '2020-09-23T17:27:04+09:00'
id: 46ba6998beff9c0aced5
organization_url_name: null
slide: false
ignorePublish: false
posting_campaign_uuid: null
agreed_posting_campaign_term: false
---
#better_error入れて見たがエラー画面変わらず
エラー画面をリッチに使いやすくするruby gem "Better_errors"。
リポジトリ見ると、railsであれば素直にbundle installすれば使えるよと書いてあるが
エラー画面は変わらず…


#解決策
以下を追記して解決しました。

```ruby:/config/environments/development.rb
BetterErrors::Middleware.allow_ip! "0.0.0.0/0"
```

▼参考:Dockerコンテナでbetter_errorが働かない
https://github.com/BetterErrors/better_errors/issues/270
▼gitのソースはおそらくここ
https://github.com/BetterErrors/better_errors/wiki/Allowing-access-to-the-console

DockerやVagrantを利用して開発する場合は
railsがbetter_errorsを許可するか判断するので
設定が必要になるそうです。

gitの質疑だとDockerのIPアドレス入れても上手く行かない!と言っているので
じゃあもう0.0.0.0/0にして全てのIPアドレスを許可いた状態にしよう!という話しをしています。

localhostのIPアドレスでもOKなので

```ruby:/config/environments/development.rb
BetterErrors::Middleware.allow_ip! "127.0.0.1/0"
```
これでもいけました。

###因みに　127.0.0.1　とは
ローカル･ループバック･アドレス。
自分自身を表す特別なIPアドレス。localhostのIPアドレスと言ったらこれにあたる。
▼参考
https://www.atmarkit.co.jp/ait/articles/0610/14/news021.html

です。


ご参考までに。

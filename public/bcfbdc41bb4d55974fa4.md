---
title: githubから特定のファイルだけダウンロードする方法
tags:
  - Git
  - GitHub
private: false
updated_at: '2020-07-22T15:13:22+09:00'
id: bcfbdc41bb4d55974fa4
organization_url_name: null
slide: false
ignorePublish: false
posting_campaign_uuid: null
agreed_posting_campaign_term: false
---
作業中何故か特定のファイルだけ削除してしまったので
gitからそこだけダウンロードしたので方法を記録。
#terminalでwgetコマンドを使う
###githubを開く
github内で該当ファイルを開き、右上にある｢Raw｣ボタンを押す。

すると中身のコードが記述された画面が表示されるので
そのページのURLをコピーする。

###terminalを開く
$wget [コピーしたURL] -P [保存先URL]

これでカレントディレクトリに該当ファイルをダウンロードできる。
![スクリーンショット 2020-07-22 15.10.29.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/634043/87941fcf-3365-2a94-8d09-edd7863e50b3.png)

▼wgetオプションによる保存先指定
https://orebibou.com/ja/home/201610/20161020_002/


この方法だとフォルダごとのダウンロードは不可。
svnを利用した方法などがあるようですがそれは後日追記することにします。

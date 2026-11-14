---
title: Visual Studioが常に最前面に来てしまう
tags:
  - VisualStudio
private: false
updated_at: '2021-05-30T17:33:36+09:00'
id: 111df758ba7c4e0aecac
organization_url_name: null
slide: false
ignorePublish: false
posting_campaign_uuid: null
agreed_posting_campaign_term: false
---
# Visual Studioが常に最前面に来る

いつも忘れるのでメモです。

普段アプリの切り替えを `Alt + tab` や　AutoHotKeyで割り当てたショートカットにて呼び出すのだが
時々Visual Studioから別アプリに切り替えようとしても、Visual Studioが最前面に鎮座して画面が切り替わらないことがある。
(メニューバーを見るとアクティブなアプリが切り替わっていることを確認できる)

# 解決法
`Window + D`　`Window + D`
を押すことでどいてくれるようになる。

原因はよくわからないのですが、`Window + 矢印` を押すとそういった現象が起こるとか。
他にはアドインが原因だという意見もあった。

# 参考

https://www.it-swarm-ja.com/ja/visual-studio/visual-studio%E3%81%8C%E3%80%8C%E5%B8%B8%E3%81%AB%E6%9C%80%E5%89%8D%E9%9D%A2%E3%80%8D%E3%83%A2%E3%83%BC%E3%83%89%E3%81%A7%E5%AE%9F%E8%A1%8C%E3%81%95%E3%82%8C%E3%82%8B%E3%81%AE%E3%81%AF%E3%81%AA%E3%81%9C%E3%81%A7%E3%81%99%E3%81%8B%EF%BC%9F/959568603/

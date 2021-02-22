---
title: IE で開いていたページが Microsoft Edge にリダイレクトされる動作について
date: 2021-02-18
tags:
  - Internet Explorer
  - Microsoft Edge 
---

みなさんこんにちは。  
日本マイクロソフトの IE/Edge サポートチームです。  

近頃、「いつのまにか IE で表示されなくなり、新しい Microsoft Edge でサイトが表示されるようになった」とお問い合わせを頂くことがあります。  
IE での表示を前提として Web アプリを作成されている方では、IE で開くようにしたいという方もいらっしゃるかと思います。  

 
そこで、少しでも本事象でお困りな皆様のお力になれればと、本ブログにて情報をまとめました！ 
本記事は以下のような構成となってます。

# 目次 <!-- omit in toc -->
- [1 : どのような動作？](#1--どのような動作)
  - [具体的な内容](#具体的な内容)
- [2 : すべての Web サイトが Edge で開かれる？](#2--すべての-web-サイトが-edge-で開かれる)
- [3 :  対処法はあるのか？](#3--対処法はあるのか)
  - [予防策](#予防策)
  - [対処策](#対処策)
 
# 1 : どのような動作？

近年のモダンブラウザ移行に伴い、最新の web サイトの多くは、Internet Explorer と互換性のないデザインが多く使用されています。
今回の IE で利用していたサイトを新しい Microsoft Edge (以降、単に Edge と表記します)へリダイレクトさせる機能は、  最新のブラウザでユーザーにスムーズにブラウジングをして頂くための機能となります。  

[Internet Explorer から Microsoft Edge にユーザーを移動する - Microsoft Edge Development | Microsoft Docs](https://docs.microsoft.com/ja-jp/microsoft-edge/web-platform/ie-to-microsoft-edge-redirection#request-an-update-to-the-ie-compatibility-list)  

なお、私どもブラウザ チームは Edge へのお早めの移行をお奨めしております。  

[まだデフォルトIE？ 新しい Microsoft Edge を使いませんか？ | Japan Developer Support Internet Team Blog (jpdsi.github.io)](https://jpdsi.github.io/blog/internet-explorer-microsoft-edge/how-about-using-new-edge/)


## 具体的な内容

本機能はEdgeの既定のブラウザにおける 「Internet Explorer に Microsoft Edge でサイトを開かせる」の設定項目に依存します。  
具体的な設定は Edge を開き、アドレスバーに edge://settings/defaultBrowser  と入力することで確認できます。  

![Picture](/articles/internet-explorer-microsoft-edge/IEtoEdgeRedirection/Internet%20Explorer%20に%20Microsoft%20Edge%20でサイトを開かせる.png)

上記のように「Internet Explorer に Microsoft Edge でサイトを開かせる」の設定で「互換性のないサイトのみ(推奨)」を選択した状態で、  
互換性のないサイト (例： youtube.com) を IE で開こうとすると、IE上には以下のように表示され、Edgeで YouTube のサイトページが表示されます。

![Picture](/articles/internet-explorer-microsoft-edge/IEtoEdgeRedirection/アクセスしようとした%20Web%20サイトは、Internet%20Explorer%20では動作しません.jpg)

互換性のないサイトは以下にリスト化されており、本設定の際にはこちらに当てはまるものは Edge で開かれます。

https://edge.microsoft.com/neededge/v1

そのため、もし開こうとしていたサイトがこちらのリストに存在する場合は、  
「互換性のないサイトのみ(推奨)」の設定をしていた場合には IE ではなく、Edge で開かれます。
 
# 2 : すべての Web サイトが Edge で開かれる？  

また、上記互換性のないサイトのリストにない Web サイトを含め、全ての Web サイトが IE ではなく、Edge で開かれるパターンもあります。

IE でブラウジングをしていると、以下のようなオプトインが表示されることがありますが、  
「Microsoft Edge に切り替える」 を選択することで、今後 全ての Web サイトがEdge で開かれるようになります。

![Picture](/articles/internet-explorer-microsoft-edge/IEtoEdgeRedirection/こんにちは%20一部のサイトが%20Internet%20explorer%20で動作しなくなったことをご存知ですか.jpg)

# 3 :  対処法はあるのか？

## 予防策

2 の予防措置としては、上記オプトインが表示された場合は、「今は実行しない」を押して切り替えないようにすることが考えられます。

![Picture](/articles/internet-explorer-microsoft-edge/IEtoEdgeRedirection/Internet%20Explorer%20に%20Microsoft%20Edge%20でサイトを開かせる%20を%20なしにする.jpg)
 
## 対処策

一度設定を変更された場合においても、「Internet Explorer に Microsoft Edge でサイトを開かせる」において「なし」を選択していただくことによって解決可能です。
 

上記方法で、IE のままサイトを閲覧するように変更することも可能ですが、Web サイトの技術も日進月歩進化しており、新しい Microsoft Edge へお早めの移行をご検討ください。

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。
---
title: Internet Explorer および Microsoft Edge での Flash の今後の対応について
date: 2020-02-03
tags: 
  - Internet Explorer
  - Flash
---

> 本記事は Technet Blog の更新停止に伴い、もとの [記事](https://docs.microsoft.com/ja-jp/archive/blogs/jpieblog/flash-roadmap) に加筆／修正を加えた内容です。
> 元の記事の最新の更新情報については、本内容をご参照ください。

こんにちは。
たまにお問い合わせとしていただく Internet Explorer および Microsoft Edge での Flash の今後の対応について改めてご紹介します。

## 今後のロードマップ
Adobe Flash のサポート終了のロードマップは、以前下記の資料にて今後のロードマップについてお知らせしていました。

> The End of an Era – Next Steps for Adobe Flash
> https://blogs.windows.com/msedgedev/2017/07/25/flash-on-windows-timeline/

その後、2019 年 8 月 30 日に下記の情報をお知らせしています。

> Update on removing Flash from Microsoft Edge and Internet Explorer
> https://blogs.windows.com/msedgedev/2019/08/30/update-removing-flash-microsoft-edge-internet-explorer/

当初の予定から大きくは変わらず、今年 2020 年末には完全に廃止される予定です。
この方針は、マイクロソフトが独自に進めているのではなく、開発元の Adobe 社が中心となりブラウザー各社連携をとり進められています。
来年 2021 年には、どのブラウザーをご利用の場合でも Flash が実行できなくなることが予想されますため、html5 などの代替テクノロジーへの移行のご検討を何卒よろしくお願いします。

なお、新しい Chromium 版の Microsoft Edge は、Chromium をベースとするほかのブラウザーと同様のタイムラインで、既定での無効化および廃止の対応が行われる見込みです。このため、EdgeHTML 版の Microsoft Edge や Internet Explorer での対応予定とは異なる可能性が十分に考えられます。
代替テクノロジーへの移行を予定されている場合は、[Google 社の情報](https://www.blog.google/products/chrome/) も併せてご確認ください。

---
以降は、Internet Explorer および EdgeHTML 版の Microsoft Edge での Flash 無効化に向けたロードマップです。

### <span style="color: #cccccc;">■ 2017 年末から 2018 年にかけて</span>
<span style="color: #cccccc;">
Windows 10 Creators Update (v1703) 以降の Microsoft Edge では、初めて訪れる Web サイトでの Flash コンテンツの実行の許可を求め、許可した Web サイトの再訪問時には求められません。
また、Internet Explorer では Flash の実行に関しては特別な制御は行われておりません。
</span>

### <span style="color: #cccccc;">■ 2018 年後半にかけて</span>
<span style="color: #cccccc;">
Microsoft Edge 上で Flash が含まれる Web サイトを閲覧するたびに実行の許可を求める動作となります。
Internet Explorer においては引き続き Flash の実行は許可され、特別な制御は行われません。
</span>

### <span style="color: #cccccc;">■ 2019 年後半にかけて
<span style="color: #cccccc;">
Microsoft Edge および Internet Explorer 上での Flash が既定で無効となります。
ただし、Flash を実行できるよう構成を変更することも可能です。
Flash を実行できるよう構成したい場合、Microsoft Edge については、[2018 年後半にかけて] と同様に、Flash が含まれる Web サイトを閲覧するたびに実行の許可が求められる動作となります。
</span>

#### 補足
上記の資料のとおり、2019 年中については 2018 年までの動作から変更はおこなれませんでした。
つまり、Internet Explorer では Flash の既定での無効化の対応は実施されていません。

### ■ 2020 年末
サポートされるすべての Windows 上の Microsoft Edge (EdgeHTML 版、Chromium 版ともに) および Internet Explorer で Flash を実行することができなくなります。
Flash を再び実行できるように構成することもできなくなる予定です。

なお、どのようにこの対応が展開されるのか？いつそれが行われるのか？ 2021 年以降も Flash を使い続ける方法があるのか？というお問い合わせをいただくことがありますが、恐れ入りますが、現段階でご紹介できる情報は上記の内容のみです。
今後の予定についてお伝えできる情報が明確になった時点で改めて本情報を更新いたします。

#### Adobe 社公開情報
Adobe Flash Playerサポート終了情報ページ
https://www.adobe.com/jp/products/flashplayer/end-of-life.html


本日の記事は以上となります。
本情報はあくまでも現時点での予定となりますため、今後何らかの影響により本予定も変更となる可能性が十分にありえます。
そのため、Web サイト側での対応を計画されている場合には、十分に余裕をもった計画とされることをお勧めいたします。

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。

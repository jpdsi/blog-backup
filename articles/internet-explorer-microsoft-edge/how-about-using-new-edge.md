---
title: まだデフォルトIE？ 新しい Microsoft Edge を使いませんか？
date: 2020-06-03
tags: 
  - Microsoft Edge
  - IE モード
---

今回は、**Internet Explorer は可能な限り利用せずに Microsoft Edge などモダン ブラウザーを利用しましょう**という話です。

「[Internet Explorer の今後について](https://social.msdn.microsoft.com/Forums/ja-JP/47290e24-fc66-4d3e-a2de-429643758d40/internet-explorer-12398201702446012395123881235612390?forum=edgeiesupportteamja)」という記事を公開してから１年ちょっと経ちました。

> Windows 10 においては、Microsoft Edge と Internet Explorer というふたつのブラウザーが搭載されていますが、弊社としましては、<span style="background: linear-gradient(transparent 80%, #ffcc99 80%)">Internet Explorer との後方互換性が必要な業務 Web システムには Internet Explorer を利用いただき、Internet Explorer でなければならない場合以外は Microsoft Edgeをご利用いただくことを提案</span>してきました。

2015 年に Microsoft Edge をリリースして以降、**Internet Explorer は従来の Web アプリケーションとの後方互換性のために残しているブラウザー**という位置づけです。業務のための Web アプリケーションが、古いブラウザーである Internet Explorer 固有の機能 (ActiveX コントロール、ブラウザー ヘルパー オブジェクト、VBScript などの古い技術) に依存していて、モダン ブラウザーでは扱えないという悩みは多くの企業が抱えています。古い Web アプリケーションを見直して、モダン ブラウザーで表示できるようにするまでの間はどうしても Internet Explorer が不可欠ですので、Windows 10 でも Internet Explorer が残されています。

Internet Explorer と Microsoft Edge の位置づけは上述のとおりですが、私たちサポート チームが対応するお問い合せの中で「Internet Explorer で表示に時間がかかります。Chrome では問題ありません。」という内容をよくみかけます。こういうお問い合せでは『Internet Explorer でなければならない理由は何か？』が焦点になります。

以下は 5 年前の記事ですが、この当時でも、Microsoft Edge や Google Chrome と比較した場合 Internet Explorer は 5 割程度のスクリプト性能でした。

Delivering fast JavaScript performance in Microsoft Edge
https://blogs.windows.com/msedgedev/2015/05/20/delivering-fast-javascript-performance-in-microsoft-edge/

この 5 年の間もモダン ブラウザーは積極的な開発が続けられていますが、レガシー ブラウザーである Internet Explorer は最新の Web 標準のサポートやパフォーマンスの向上への注力はしていませんので、パフォーマンスの差はさらに広がっています。どのような Web ページの表示が遅いのかを伺ってみると、ほとんどの場合は、最新の Web 標準の技術を取り入れたリッチなコンテンツでしたので、上記のことを考えれば、Internet Explorer としてのパフォーマンス性能の限界と言えます。こういった問題を回避するためにも [Chromium ベースの新しい Microsoft Edge](https://blogs.windows.com/japan/2020/01/16/upgrading-new-microsoft-edge-79-chromium/) の利用を強くおすすめします。

新しい Microsoft Edge では『IE モード』という機能を搭載しています。一言でいうと『Microsoft Edge の画面の中で Internet Explorer が動く』というものです。詳細は [公式ドキュメント](https://docs.microsoft.com/ja-jp/deployedge/edge-ie-mode) や [サポート チームのブログ記事](https://jpdsi.github.io/blog/internet-explorer-microsoft-edge/IEMode/) をご覧いただければと思いますが、適切に制御することで、ユーザーが Internet Explorer と Microsoft Edge を意識的に使い分ける操作をせずに、Internet Explorer を必要とする古い技術で作られた Web アプリケーションを、Microsoft Edge の中でシームレスに閲覧できるようになります。IE モードの登場により、『Internet Explorer を単独で動かすのではなく、IE/Edge どちらにも対応したブラウザ環境を提供』→『今後は IE ではなく、Microsoft Edge で動作する Web アプリケーションを用意する』という段階的な移行を進めやすくなりました。ぜひ活用していただければと思います。

最後に、『最新の Web 標準の技術を取り入れたリッチなコンテンツ』の例を挙げます。弊社製品のひとつである Power BI ですが、以下の公式ドキュメントでサポートされるブラウザーについて記述があります。

Power BI のサポートされているブラウザー
https://docs.microsoft.com/ja-jp/power-bi/power-bi-browsers

> Power BI はこれらのサポートされているすべてのブラウザーで動作するように設計されていますが、<span style="background: linear-gradient(transparent 80%, #ffcc99 80%)">選択したブラウザーによってパフォーマンスは異なります。 特に Internet Explorer を使用している場合は、パフォーマンスが低下する可能性があります。 パフォーマンスを向上させるには、Internet Explorer から Microsoft Edge などの最新のブラウザーに切り替えることを強くお勧めします。</span> Edge Chromium を使用すると、Microsoft Edge のパフォーマンスは引き続き向上することが期待されています。 それでも許容できないパフォーマンスが発生する場合は、サポートされている他の最新のブラウザーをテストして、Power BI ソリューションでより良い結果が得られるかどうかを確認します。

私たちブラウザー サポート チームがいつも言いたいことをしっかりと書いてくれています。イマドキのことをやるなら最新のブラウザーを使うべきです。もちろん組織ごとに事情はあるでしょう。でも、古いブラウザーを使い続けると、多くのユーザーがパフォーマンスに不満を感じたり、組織としての生産性にも影響を及ぼしかねません。イマドキのコンテンツに見合った最新のブラウザーを使うことでユーザーは快適に作業できるようになります。IT 管理者のみなさん、まずはモダン化の一歩を踏み出してみませんか。

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。

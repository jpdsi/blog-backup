---
title: Windows Update による新しい Microsoft Edge の配信について
date: 2020-01-10
tags: 
  - Microsoft Edge
  - Chromium
---

<font color="red">**2020/7/1 追記: Chromium ベースの Microsoft Edge の Windows Update による配布を、いままで対象外だった Education や Enterprise に対しても実施する予定となりました。詳細は後述します。**</font>

皆様、こんにちわ！

新しい Microsoft Edge のリリースが、来週に迫ってまいりました！  
また、エンタープライズの皆様におかれましては、新しい Microsoft Edge が Windows Update から自動配信されるって噂があるけど、実際どうなる予定なんだ？？？と、ヤキモキされていることと思います。  
Windows Update による新しい Microsoft Edge の配信に関しては、やっと[公式の情報](https://docs.microsoft.com/en-us/DeployEdge/microsoft-edge-blocker-toolkit)が公開されましたが、まだ翻訳されていない状況ですので、要点を以下に簡単にまとめました。

<font color="red">※ 下記の「2020/7/1 追記」の部分も併せてご覧ください。</font>
- Windows Update での自動配信は、<u>**Windows 10 バージョン 1803 以上の Home または Pro エディションのみが対象**</u>です
- <u>**上記以外のすべての Windows バージョン (またはエディション) は自動更新の対象外**</u>です
- Windows 10 バージョン 1803 以上の Home または Pro エディションであっても、<u>**以下のケースでは自動更新の対象外**</u>です
  - Active Directory ドメインに参加している場合
  - 組織の環境が Windows Server Update Services (WSUS) や System Center Configuration Manager (SCCM) などの更新管理ソリューションにより管理されている場合
- 新しい Microsoft Edge は、Windows の累積的な更新プログラムに含められた形ではなく、<u>**個別の更新パッケージとして配信される予定**</u>です

以上のとおり、エンタープライズの多くの環境については、Windows Update による新しい Microsoft Edge の配信の対象外になるかと想定されますが、少しでも参考となれば嬉しいです。

---
## <font color="red">2020/7/1 追記</font>

Chromium ベースの Microsoft Edge の Windows Update による配布を、いままで対象外だった Education や Enterprise に対しても実施する予定となりました。早くても 7/30 以降で、最初は Education エディションを対象に開始します。

Windows Update for Business (WUfB)/WSUS で管理している端末は対象外です。

もし WUfB や WSUS で更新管理していなくて、急に新しい Edge に置き換えられてしまうと困る環境があれば、早めに Blocker Toolkit の設定を実施ください。

Microsoft Edge (Chromium ベース) の自動配布を無効にするための Blocker Toolkit
https://docs.microsoft.com/ja-jp/deployedge/microsoft-edge-blocker-toolkit

また、以前の Edge (EdgeHTML) と共存したい場合は、『事前に』グループ ポリシーを設定する必要があります。

Microsoft Edge の新しいバージョンをインストールした後 Microsoft Edge レガシにアクセスする
https://docs.microsoft.com/ja-jp/deployedge/microsoft-edge-sysupdate-access-old-edge


なお、今回お伝えした Windows Update による配布の対象拡大に関しての詳細については以下をご覧ください。

Upgrading to the new Microsoft Edge through Windows Update (expanded)
https://techcommunity.microsoft.com/t5/articles/upgrading-to-the-new-microsoft-edge-through-windows-update/m-p/1499724#M3025
---

なお、Windows Update により新しい Microsoft Edge に更新された場合でも、既定のブラウザーの設定は変更されません。(既定のブラウザーを Internet Explorer 11 や Google Chrome に設定してる場合は、既定のブラウザーが新しい Microsoft Edge に変更されることはないです。)

今回は以上です。  
それでは、また次回！

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。

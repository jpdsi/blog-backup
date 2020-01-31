---
title: 新しい Microsoft Edge の展開について
date: 2020-01-23
tags: 
  - Microsoft Edge
  - Chromium
---

こんにちは。

Chromium ベースの新しい Microsoft Edge の、最初の Stable (安定版) チャネル ビルド (Microsoft Edge 79 stable) のリリースから一週間が経ちました。

[公式ブログ](https://blogs.windows.com/japan/2020/01/16/upgrading-new-microsoft-edge-79-chromium/) にて下記の引用のとおりお伝えしていますが、Windows Update 経由での配信 (※1) までは、[こちらからインストーラーをダウンロード](https://www.microsoft.com/edge) いただく他には、[SCCMで展開](https://docs.microsoft.com/en-us/configmgr/apps/deploy-use/deploy-edge?toc=https://docs.microsoft.com/DeployEdge/toc.json&bc=https://docs.microsoft.com/DeployEdge/breadcrumb/toc.json) 、[Intuneで展開](https://docs.microsoft.com/en-us/intune/apps/apps-windows-edge?toc=https://docs.microsoft.com/DeployEdge/toc.json&bc=https://docs.microsoft.com/DeployEdge/breadcrumb/toc.json) などで新しい Microsoft Edge をご利用いただくことができます。

> 日本のお客様に対しては、確定申告への影響を考慮し、Windows Updateを通じた新しいMicrosoft Edgeの配信は令和２年４月１日以降、順次開始される予定です。

新しい Microsoft Edge のリリース前からも、多くのユーザー、IT Pro のみなさまから、Microsoft Edge についての様々なご質問をいただいております。
[エンタープライズ環境の Microsoft Edge についてよく寄せられる質問](https://docs.microsoft.com/ja-jp/DeployEdge/faqs-edge-in-the-enterprise) ではすでにいくつかの QA を掲載していますのでご覧ください。
随時こちらの公式ドキュメントへの追記も進めていきますが、以下に 3 つ QA を載せました。ご参考になるとうれしいです。

---
## Q1:インストーラーが [exe 形式](https://www.microsoft.com/en-us/edge) と [msi 形式](https://www.microsoft.com/en-us/edge/business/download) のふたつがありますが、違いはなんですか？
<u>Answer</u>
- exe 形式はインストール時にインターネットからファイルをダウンロードしながらインストールします。
- msi 形式はインターネット接続は不要なフルパッケージです。インターネット非接続環境などで、組織内の Web サーバーやファイル サーバーに msi パッケージを格納し、組織内の端末に展開するといったシナリオにご利用いただけます。
- exe 形式はその時点での最新の Stable (安定版) チャネル ビルドがインストールできます。
- msi 形式は Stable チャネルのほか、Beta チャネル、Dev チャネルのフルパッケージがダウンロードできます。

---
## Q2:今後の Windows Update での展開に備えて、展開を抑止する構成をしておきたいのですが...
<u>Answer</u>
[こちら](https://docs.microsoft.com/en-us/DeployEdge/microsoft-edge-blocker-toolkit) に沿って、**Blocker Toolkit**　をインストールします。
なお、Blocker Toolkit は Windows Update でのインストールを抑止するものであり、ユーザーによる手動インストールは制御できません。手動でのインストールやインストール後の自動更新の制御をする場合は [更新ポリシー](https://docs.microsoft.com/ja-jp/DeployEdge/microsoft-edge-update-policies) を構成します。

---
## Q3:古い Microsoft Edge (EdgeHTML) と新しい Microsoft Edge (Chromium ベース) の両方を使いたいときはどうすればいいですか？
<u>Answer</u>
[こちら](https://docs.microsoft.com/en-us/deployedge/microsoft-edge-sysupdate-access-old-edge) に沿って、**"Microsoft Edge でのブラウザーの同時実行エクスペリエンスを許可する"** ポリシーを **新しい Microsoft Edge のインストール前に** 有効にします。
[実際の設定の様子についての記事](https://jpdsi.github.io/blog/internet-explorer-microsoft-edge/side-by-side/) もぜひご覧ください。

---
(※1)
Windows Update による新しい Microsoft Edge の配信に関する [公式の情報](https://docs.microsoft.com/en-us/DeployEdge/microsoft-edge-blocker-toolkit)がありますが、以下に要点を簡単にまとめます。

- Windows Update での自動配信は、<u>**Windows 10 バージョン 1803 以上の Home または Pro エディションのみが対象**</u>です
- <u>**上記以外のすべての Windows バージョン (またはエディション) は自動更新の対象外**</u>です
- Windows 10 バージョン 1803 以上の Home または Pro エディションであっても、<u>**以下のケースでは自動更新の対象外**</u>です
  - Active Directory ドメインに参加している場合
  - 組織の環境が Windows Server Update Services (WSUS) や System Center Configuration Manager (SCCM) などの更新管理ソリューションにより管理されている場合
- 新しい Microsoft Edge は、Windows の累積的な更新プログラムに含められた形ではなく、<u>**個別の更新パッケージとして配信**</u>されます

以上のとおり、エンタープライズの多くの環境については、Windows Update による新しい Microsoft Edge の配信の対象外になるかと想定されますが、不明点などがありましたら私共サポートまでお問い合わせください。

今回は以上です。  
それでは、また次回！

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。

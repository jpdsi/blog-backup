---
title: 新しい Microsoft Edge の展開について
date: 2020-01-23
tags: 
  - Microsoft Edge
  - Chromium
---

(更新履歴)
2020/02/12 Q4 を追加しました。
2020/03/02 Windows Update を通じた新しい Microsoft Edge の配信について最新情報を反映しました。
<font color="red">2020/07/01 Chromium ベースの Microsoft Edge の Windows Update による配布を、いままで対象外だった Education や Enterprise に対しても実施する予定となりました。詳細は後述します。</font>

---
こんにちは。

Chromium ベースの新しい Microsoft Edge の、最初の Stable (安定版) チャネル ビルド (Microsoft Edge 79 stable) のリリースから一週間が経ちました。

[公式ブログ](https://blogs.windows.com/japan/2020/01/16/upgrading-new-microsoft-edge-79-chromium/) にて下記の引用のとおりお伝えしていますが、Windows Update 経由での配信 (※1) までは、[こちらからインストーラーをダウンロード](https://www.microsoft.com/edge) いただく他には、[SCCMで展開](https://docs.microsoft.com/en-us/configmgr/apps/deploy-use/deploy-edge?toc=https://docs.microsoft.com/DeployEdge/toc.json&bc=https://docs.microsoft.com/DeployEdge/breadcrumb/toc.json) 、[Intuneで展開](https://docs.microsoft.com/en-us/intune/apps/apps-windows-edge?toc=https://docs.microsoft.com/DeployEdge/toc.json&bc=https://docs.microsoft.com/DeployEdge/breadcrumb/toc.json) などで新しい Microsoft Edge をご利用いただくことができます。

> 国税庁より、申告所得税（及び復興特別所得税）、贈与税及び個人事業者の消費税（及び地方消費税）の申告期限・納付期限が令和2年4月16日まで延長されることが発表されたことに伴い、Windows Updateを通じた新しいMicrosoft Edgeの配信は令和２年４月17日以降となります（令和2年3月2日追記）。

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
## Q4:新しい Microsoft Edge (Chromium ベース) にブックマークなどは引き継がれますか？
<u>Answer</u>
はい。[公式ドキュメント](https://docs.microsoft.com/ja-jp/DeployEdge/faqs-edge-in-the-enterprise) に以下の記載がありますが、ブックマークなどを引き継ぐことができます。

> 現時点では、Microsoft Edge では、Microsoft Edge、Chrome、Internet Explorer、Firefox (Win10) の既存のインストールからのインポートがサポートされています。 インポートでは、ブックマーク、履歴、パスワード、オートフィル (支払い、住所、汎用フォーム) の設定がサポートされています。 インポートは、最初の実行エクスペリエンスで実行するか、ブラウザーの設定を使用して実行するかを選択できます。

なお「最初の実行エクスペリエンス (First-run Experience)」は随時改善を重ねておりますので、今後のバージョンアップで動作が変わる可能性はあります。
また、現時点では古い Edge で保持していた Cookie は、自動的に新しい Edge に移行されます。

---
(※1)
Windows Update による新しい Microsoft Edge の配信に関する [公式の情報](https://docs.microsoft.com/en-us/DeployEdge/microsoft-edge-blocker-toolkit)がありますが、以下に要点を簡単にまとめます。

<font color="red">※ 下記の「2020/7/1 追記」の部分も併せてご覧ください。</font>
- Windows Update での自動配信は、<u>**Windows 10 バージョン 1803 以上の Home または Pro エディションのみが対象**</u>です
- <u>**上記以外のすべての Windows バージョン (またはエディション) は自動更新の対象外**</u>です
- Windows 10 バージョン 1803 以上の Home または Pro エディションであっても、<u>**以下のケースでは自動更新の対象外**</u>です
  - Active Directory ドメインに参加している場合
  - 組織の環境が Windows Server Update Services (WSUS) や System Center Configuration Manager (SCCM) などの更新管理ソリューションにより管理されている場合
- 新しい Microsoft Edge は、Windows の累積的な更新プログラムに含められた形ではなく、<u>**個別の更新パッケージとして配信**</u>されます

以上のとおり、エンタープライズの多くの環境については、Windows Update による新しい Microsoft Edge の配信の対象外になるかと想定されますが、不明点などがありましたら私共サポートまでお問い合わせください。

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

今回は以上です。  
それでは、また次回！

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。

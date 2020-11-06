---
title: Internet Explorer および Microsoft Edge での Flash の今後の対応について
date: 2020-02-03
tags: 
  - Internet Explorer
  - Flash
---

更新履歴:
<font color="red">**2020/9/9 更新: 今後に関する最新情報を追記しました。**</font>
<font color="red">**2020/10/28 更新: Update for Removal of Adobe Flash Player がリリースされました。**</font>
<font color="red">**2020/10/30 更新: Flash のブロックに関して追記しました。**</font>
<font color="red">**2020/11/06 更新: 古くなった情報を削除し、最新情報を見やすくしました。よくあるお問い合わせを追記しました。**</font>

---

サポートされるすべての Windows 上の Microsoft Edge (EdgeHTML 版、Chromium 版ともに) および Internet Explorer で Flash を実行することができなくなります。

本情報はあくまでも現時点での予定となります。今後も新しい情報が公開され次第、こちらの記事にも順次追加していく予定ですが、最新情報が最速で公開されるのは [弊社開発部門のブログ](https://blogs.windows.com/msedgedev/) ですので併せてご確認ください。

<span style="color: #ff0000;font-weight:bold;">なお、弊社も Adobe 社も、2020 年 12 月末をもって Flash のサポートを終了します。それ以降はお問い合わせを受け付けることができなくなりますのでご注意ください。</span>

---

2020 年 9 月 4 日に今後の最新情報が公開されました。以下に概要をまとめます。

> Update on Adobe Flash Player End of Support
> https://blogs.windows.com/msedgedev/2020/09/04/update-adobe-flash-end-support/

> Adobe Flash Player のサポート終了に関する最新情報
> https://blogs.windows.com/japan/2020/09/14/update-adobe-flash-end-support/

- これまでの予定と変わりなく、<span style="color: #ff0000;font-weight:bold;">Adobe Flash Player のサポート終了は 2020 年 12 月 31 日です。</span>
- それ以降の法人向け対応サポートについては、[Adobe 社の情報](https://www.adobe.com/jp/products/flashplayer/enterprise-end-of-life.html)をご覧ください。
- [Adobe 社の情報](https://www.adobe.com/jp/products/flashplayer/enterprise-end-of-life.html)で案内される法人向け対応サポート契約を結び、今まで弊社から提供をしてきた Adobe Flash Player に代わるプラグインの提供を受けた場合、それを『Internet Explorer 11』 および 『Microsoft Edge (Chromium 版) の IE モード』で動作させることができますが、『サードパーティー製のプラグイン』の扱いとなり弊社でのサポートはありません。
- [2021 年 1 月リリース予定](https://docs.microsoft.com/en-us/DeployEdge/microsoft-edge-release-schedule) の Microsoft Edge (Chromium 版) の [バージョン 88 にて Flash が削除される予定](https://docs.microsoft.com/en-us/microsoft-edge/web-platform/site-impacting-changes) です。
- 2020 年末以降は、 Microsoft Edge (EdgeHTML 版) と Internet Explorer 向けの『Adobe Flash Player のセキュリティ更新プログラム』の提供はありません。
- 2021 年 1 月初旬に、Microsoft Edge (EdgeHTML 版) と Internet Explorer で Flash は既定で無効化され、また、2020 年 6 月リリースの [KB4561600](https://support.microsoft.com/ja-jp/help/4561600/security-update-for-adobe-flash-player) より古いバージョンの実行はブロックされます。
- Windows OS のコンポーネントとしての Flash を永久的に削除するための『[Update for Removal of Adobe Flash Player](https://support.microsoft.com/en-us/help/4577586/update-for-removal-of-adobe-flash-player)』というタイトルの更新プログラムがリリース予定です。<span style="color: #ff0000;font-weight:bold;">この更新を適用したあとは元に戻すことはできません。</span>
- 2020 年 10 月 28 日 (日本時間) から『[Update for Removal of Adobe Flash Player](https://support.microsoft.com/en-us/help/4577586/update-for-removal-of-adobe-flash-player)』を [Microsoft Update カタログから入手](https://www.catalog.update.microsoft.com/search.aspx?q=4577586) できるようになりました。2020 年 12 月 31 日のサポート終了より前に Flash を永久削除したい場合は、カタログから更新プログラムを入手して実行できます。
- 『[Update for Removal of Adobe Flash Player](https://support.microsoft.com/en-us/help/4577586/update-for-removal-of-adobe-flash-player)』は、2021 年の初めに Windows Update (オプション) と WSUS で配信予定で、その 2 ～ 3 か月後には Windows Update では推奨として配信予定です。
- 2021 年夏には、Windows 10 での OS のロールアップ更新プログラム、および Windows 8.1, Windows Server 2012 and Windows Embedded 8 Standard での Internet Explorer 用の累積的なセキュリティ更新プログラムやマンスリー ロールアップの一部として『[Update for Removal of Adobe Flash Player](https://support.microsoft.com/en-us/help/4577586/update-for-removal-of-adobe-flash-player)』が組み込まれ、<span style="color: #ff0000;font-weight:bold;">更新プログラムの適用により Flash が永久的に削除されます。</span>

#### Adobe 社公開情報

Adobe Flash Player法人向けサポート終了情報ページ
https://www.adobe.com/jp/products/flashplayer/enterprise-end-of-life.html

Update for Enterprise Customers Using Adobe Flash Player
https://blog.adobe.com/en/fpost/2020/update-for-enterprise-adobe-flash-player.html

Adobe Flash Playerサポート終了情報ページ
https://www.adobe.com/jp/products/flashplayer/end-of-life.html

---

## よくあるお問い合わせ
弊社サポートへよくお問い合わせいただくご質問についてまとめます。業務上やむをえない事情がある場合は、以下の『Flash の実行ブロック』および『Flash の既定で無効化』について、更新を適用しないなどの方法で 2021 年 1 月以降も Flash を実行することは可能ではあります。しかしながら、弊社も Adobe 社も 2020 年 12 月末をもってサポート (お問い合わせの受付やセキュリティ更新の提供) を終了しますので、2021 年以降の Flash の実行はお勧めできません。セキュリティの観点からは 2021 年夏予定の Flash の永久削除を待つのではなく、『Update for Removal of Adobe Flash Player』を使用して自発的に Flash を永久削除することをお勧めします。

### 全般
Q. 2021 年 1 月以降、Flash を使い続けるにはどうしたらいいですか？
A. Adobe Flash Player のサポート終了は 2020 年 12 月 31 日です。それ以降に使い続ける方法について、弊社ではサポートできません。2021 年以降の法人向け対応サポートについては、[Adobe 社の情報](https://www.adobe.com/jp/products/flashplayer/enterprise-end-of-life.html) をご覧ください。

### 『Flash の実行ブロック』について
Q. ブロックとは具体的にどのような動作ですか？
A. Flash の実行がブロックされた結果、画面上でどのように見えるのかなど具体的な挙動については、今後の情報公開をお待ちください。

Q. どうすれば回避できますか？
A. [KB4561600](https://support.microsoft.com/ja-jp/help/4561600/security-update-for-adobe-flash-player) 以上の Adobe Flash Player のセキュリティ更新プログラムを適用してください。

Q. 事前に検証できますか？
A. 今のところできません。Flash の実行をブロックするために必要な情報を弊社管理のサーバーから取得しますが、そのデータをまだ公開していないため、PC の時刻を 2021 年 1 月以降に進めても動作を確認することはできません。

Q. Adobe Flash Player のセキュリティ更新プログラムは、OS のロールアップ更新プログラムに含まれますか？
A. 含まれません。独立した更新プログラムです。前提条件などは更新プログラムの公開ドキュメントをご確認ください。

### 『Flash の既定で無効化』について
Q. ブロックとは別ですか？
A. はい。『ブロック』はブラウザー側の動作であり、『無効化』は Adobe Flash Player による動作です。

Q. どうすれば回避できますか？
A. [Adobe 社の公開ドキュメント (Adobe Flash Player法人向けサポート終了情報ページ)](https://www.adobe.com/jp/products/flashplayer/enterprise-end-of-life.html) 内の「デフォルトでFlashの再生をブロック」「Flash Playerは2020年末を過ぎても動作しますか？」をご確認ください。

Q. 無効化に関してサポートしてほしいのですが。
A. [Adobe 社の公開ドキュメント (Adobe Flash Player法人向けサポート終了情報ページ)](https://www.adobe.com/jp/products/flashplayer/enterprise-end-of-life.html) の内容も含め、Adobe 社へお問い合わせいただくようお願いしています。

### 『Update for Removal of Adobe Flash Player』について
Q. 今後のスケジュールについて教えてください。
A. 2021 年の初めに Windows Update (オプション) と WSUS で配信予定で、その 2 ～ 3 か月後には Windows Update では推奨として配信予定です。2021 年夏には、Windows 10 での OS のロールアップ更新プログラム、および Windows 8.1, Windows Server 2012 and Windows Embedded 8 Standard での Internet Explorer 用の累積的なセキュリティ更新プログラムやマンスリー ロールアップの一部としてが組み込まれます。

Q. 更新の適用時に OS の再起動は必要ですか？
A. 再起動は求められません。

Q. 更新の適用後にアンインストールできますか？ (Flash が存在する状態に戻せますか？)
A. できません。

Q. Windows 7 のときのようにユーザーが個別に Flash をインストールすることはできますか？
A. できません。

Q. Microsoft Edge (Chromium 版) に影響しますか？
A. 影響しません。Microsoft Edge (Chromium 版) は 2021 年 1 月リリース予定のバージョン 88 にて Flash が削除される予定です。それ以前のバージョンはそのままになります。

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。

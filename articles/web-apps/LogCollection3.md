---
title: IIS の調査にパフォーマンス系のログ情報について (Perfview)
date: 2020-01-24
tags: 
  - Internet Information Services
  - ログ採取
---

こんにちは。
 
IIS サポート チームです！
 
以前、IIS 上でのトラブル シューティングに役立つログの採取方法について、以下の 2 つの記事を書いてますが、現象の性質によってはこのログ以外の情報が必要となるログがございます。

　IIS の調査に必要な基本的なログ情報について
　https://jpdsi.github.io/blog/web-apps/LogCollection1/

　IIS の調査に必要な通信系のログ情報について
　https://jpdsi.github.io/blog/web-apps/LogCollection2/

今回は、IIS のパフォーマンス系に関する現象の調査に役立つログをご紹介いたします。

<br/>

---------------
 # <u>PerfView</u>
このログはCPU 高負荷やメモリに関するパフォーマンスの分析を行うのに有効なツールです。
事象再現時に、このログを取得することで原因究明の材料として役立てることが可能です。

  PerfView - Overview
  https://github.com/Microsoft/perfview#perfview-overview


以下の事前準備を行うことで、自動で Perfview のログを採取することが可能です。
なお、下記の採取手順は現象発生時に自動で採取できる手順となります。

<br/>

## 注意事項
PerfView のログについてはユーザーがログオンしている間に常時採取できるものになります。

そのため、ユーザーはログオフせずにロックさせておき、OS の再起動が実施された場合は再度コマンドの実行を行うように構成してください。

また、ツールを常時動作させることでサーバ上の動作に大きな影響をあたえるツールではないことを確認しておりますものの、
実際に検証環境などで採取の設定を行っても影響がないか、十分に検証いただいた上でご検討ください。

<br/>

## 事前準備
1. 下記ページより PerfView.exe をダウンロードします。
(Shortcut to Download the Latest PerfView.exe の部分にリンクがあります)

  Downloading PerfView
  https://github.com/Microsoft/perfview/blob/master/documentation/Downloading.md

2. 対象サーバーにて任意のフォルダに展開します。
3. サーバー マネージャーの役割サービスの追加で、[Web サーバー] - [状態と診断] - [トレース] が
インストールされていなければ、インストールします。インストールの際は、IIS の再起動が発生します。
4. 管理者コマンドプロンプトを起動し、PerfView を展開したフォルダへ移動します。
5. コマンドプロンプトにて以下のコマンドを実行します。

> PerfView.exe collect /nogui /ThreadTime /CircularMB=2000 "/StopOnPerfCounter=Processor:% Processor Time:_Total>90" /MinSecForTrigger:30 /DelayAfterTriggerSec:60

6. 新しいコマンドプロンプトウィンドウが表示され、10秒ごとに「Collecting ...」というログが流れ始めます。待機頂く際にそのログが流れ始めた事をご確認ください。

#### 手順 5 の補足
トレースの停止条件について、補足いたします。
上記手順でご案内した方法では、Processor\% Processor Time(_Total) のパフォーマンスカウンターが30秒間、90%以上を記録した場合に、さらにその60秒後にトレースを停止します。
それぞれのオプションの意味は以下の通りです。

[/StopOnPerfCounter=Processor:% Processor Time:_Total>90]
Processor\% Processor Time(_Total) のパフォーマンスカウンターが90%越えた場合を停止の条件とする。

[/MinSecForTrigger:30]
パフォーマンスカウンターの停止条件が最低30秒間継続した場合にトリガーする。

[/DelayAfterTriggerSec:60]
パフォーマンスカウンターによる停止がトリガーされてからトレースを60秒後に停止する。
※ なお、当該システムにて、まず perfview をオプションなしで実行して頂きますと、使用許諾契約が表示されます。そちらを Accept（受諾）して頂きますと、ツールが使用可能になります。もしコマンドラインから受諾する場合には、以下のコマンドをご利用ください 。

> PerfView.exe collect /nogui /ThreadTime /CircularMB=2000 /AcceptEULA "/StopOnPerfCounter=Processor:% Processor Time:_Total>90" /MinSecForTrigger:30 /DelayAfterTriggerSec:60

<br/>

## 採取手順
1. 現象が発生し、トレースが自動で停止するのを待ちます。
2. トレースが停止するとPerfView.exeのフォルダでログの圧縮等が行われます (場合によっては数十秒から数分掛かります)。 
「Press enter to close window」と表示された時点で圧縮が完了していますので、そちらが表示されましたら、Enter を押下し、ウィンドウを閉じます。 
3. 同フォルダに生成された PerfViewData.etl.zip と PerfViewData.log.txt をご確認ください。

<br/>

今回は以上です。 それでは、また次回！

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。 もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。
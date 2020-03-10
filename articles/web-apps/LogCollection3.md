---
title: IIS の調査にパフォーマンス系のログ情報について (Perfview)
date: 2020-01-31
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

本ブログでは下記の 2 つのパターンについてそれぞれの採取手順を紹介いたします。
なお、事前準備は 2 ついずれも共通で実施が必要なものとなりますので、"事前準備" と "いずれかのパターンの手順" を順に実施します。

- A. CPU 高負荷時に自動取得する場合
- B. ハング (応答なし) や応答遅延で再現時に手動採取する場合

<br/>

---------------

## 事前準備
0. PerfView は .NET Framework 4.0 を利用して開発されているため、.NET Framework 4.0 以降がインストールされている必要がございます。
対象の環境にて .NET Framework 4.x の機能が有効化されていない場合は有効化してください。

1. 下記ページより PerfView.exe をダウンロードします。
(Shortcut to Download the Latest PerfView.exe の部分にリンクがあります)

  Downloading PerfView
  https://github.com/Microsoft/perfview/blob/master/documentation/Downloading.md

2. 対象サーバーにて任意のフォルダに展開します。
（PerfView.exe と同じフォルダにデータが出力されます。データ サイズは採取時間にもよりますが、数10MB ～ 数100MBほどとなります）

3. サーバー マネージャーの役割サービスの追加で、[Web サーバー] - [状態と診断] - [トレース] がインストールされていなければ、インストールします。
インストールの際は、IIS の再起動が発生します。

<br/>

採取目的に応じて、下記の A、B のいずれかの該当する手順に移動します。

---------------

## A. CPU 高負荷時に自動取得する場合
#### 注意事項
以下の事前準備を行うことで、CPU 高負荷時に自動で Perfview のログを採取することが可能です。

PerfView のログについてはユーザーがログオンしている間に常時採取できるものになります。

そのため、ユーザーはログオフせずにロックさせておき、OS の再起動が実施された場合は再度コマンドの実行を行うように構成してください。

また、ツールを常時動作させることでサーバ上の動作に大きな影響をあたえるツールではないことを確認しておりますものの、
実際に検証環境などで採取の設定を行っても影響がないか、十分に検証いただいた上でご検討ください。

#### 手順
4. 管理者コマンドプロンプトを起動し、PerfView を展開したフォルダへ移動します。
5. コマンドプロンプトにて以下のコマンドを実行します。

> PerfView.exe collect /nogui /ThreadTime /CircularMB=2000 "/StopOnPerfCounter=Processor:% Processor Time:_Total>90" /MinSecForTrigger:30 /DelayAfterTriggerSec:60

6. 新しいコマンドプロンプトウィンドウが表示され、10秒ごとに「Collecting ...」というログが流れ始めます。待機頂く際にそのログが流れ始めた事をご確認ください。

##### 手順 5 の補足
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

7. 現象が発生し、トレースが自動で停止するのを待ちます。
8. トレースが停止するとPerfView.exeのフォルダでログの圧縮等が行われます (場合によっては数十秒から数分掛かります)。 
「Press enter to close window」と表示された時点で圧縮が完了していますので、そちらが表示されましたら、Enter を押下し、ウィンドウを閉じます。 
9. 同フォルダに生成された PerfViewData.etl.zip と PerfViewData.log.txt をご確認ください。

<br/>

## B. ハング (応答なし) や応答遅延で再現時に手動採取する場合
ハングや応答遅延時に手動で採取する場合、サーバー上でツールを起動し、事象を再現させた後、ツールを停止するという手順となります。

#### 手順

----- 起動と設定 -----

4. 管理者コマンド プロンプトを起動し、PerfView を展開したフォルダへ移動します。

----- 情報採取-----

5. コマンド プロンプトにて以下のコマンドを実行します。新しいコマンド プロンプト ウィンドウが表示され、
10秒ごとに「Collecting ...」というログが流れ始めます。(（6) にて待機頂く際にそのログが流れ始めた事をご確認ください）

> perfview /nogui /ThreadTime /CircularMB=2000 collect

※ なお、当該システムにて、まず perfview をオプションなしで実行して頂きますと、使用許諾契約が表示されます。そちらを Accept（受諾）して頂きますと、ツールが使用可能になります。もしコマンドラインから受諾する場合には、以下のコマンドをご利用ください。
> perfview /nogui /ThreadTime /CircularMB=2000 /AcceptEULA collect

6. クライアントから再度リクエストを送り、事象を再現させます。
少し待機した後、事象が発生していることを確認したら、事象発生中のトレースを最低30秒程度、可能であれば1分以上の採取をご検討ください。

----- 採取停止 -----

7. 手順 5 で新しく起動したコマンド プロンプト ウィンドウにて S キーを押下します。
8. ログの圧縮等が行われます。「Press enter to close window」と表示された時点で圧縮が完了しています。Enter を押下し、ウィンドウを閉じます。
9. 同フォルダに生成された PerfViewData.etl.zip と PerfViewData.log.txt をお送りください。


<br/>

今回は以上です。 それでは、また次回！

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。 もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。
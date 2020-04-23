---
title: Azure Bot Service の料金やプランについて
date: 2020-04-23
tags: 
  - Azure Bot Service
  - 料金プラン
---

※ これまで Azure Bot Service Support Team Blog にて公開していた記事を移しました。

こんにちは。Azure Bot Service Support Team です。

今回は Azure Bot Service の料金やプランについて、以下の Azure Bot Service の価格のページを元に説明します。

Azure Bot Service の価格
https://azure.microsoft.com/ja-jp/pricing/details/bot-service/

Azure Bot Service には Free(F0) と Standard(S1) の 2 つのプランがあります。

![plan](/articles/azure-bot-service/bot-plan/botplan.png)

Azure ポータルから Web App Bot 等の Azure Bot Service のリソースを作成しようとした際に表示される価格レベルがこちらに当たります。

![portalplan](/articles/azure-bot-service/bot-plan/portalplan.png)


上記の通り、プランを利用することに対してかかる料金はありませんが、ユーザーと Bot がチャネルを介してやりとり (会話) するメッセージに対して、選択するプランおよびチャネルごとに料金や制限がかかります。

ここで「チャネル」という単語が出てきましたが、Azure Bot Service では開発した Bot アプリケーションを複数のチャネルを経由して利用できます。例えば、開発した Bot に対して Microsoft Teams チャネルを有効にすることで Microsoft Teams クライアントから 1 on 1 のチャットやチームのチャネルから会話でき、かつ、同じ Bot に対して LINE チャネルも有効にすることで LINE アプリからも会話するといったことができます。

チャネルには Standard チャネルと Premium チャネルの 2 種類があり、それぞれのチャネルとしては以下が用意されています (2019 年 7 月現在)。「Azure Bot Service の価格」ページの画面下部の FAQ にも少し説明がありますので、ご覧ください。


![channels](/articles/azure-bot-service/bot-plan/channels.png)


1 つの Bot アプリケーションに対して、複数のチャネルを有効にすることができ、また、複数のチャネルを利用しても追加の料金はありません。また、Standard チャネルを利用する場合は、プランが Free であっても Standard であっても料金はかからず、かつ、制限も変わりません。

つまり、上述の Standard チャネルのいずれかのみを介して Bot を利用する予定の場合は、Free プランを選択していただければ、Azure Bot Service に対する料金はかかりません。

一方、Premium チャネルを利用する予定がある場合は、Free プランでは月ごとにメッセージ数が 10,000 という上限があるため、その上限を超えるかどうかを元に、Free プランを選択するか、Standard プランを選択するかを検討していただく必要があります。ここで注意いただく必要がある点は以下になります。


1) メッセージ数のカウントは、ユーザーが bot に送ったメッセージの数と、bot からユーザーに送られたメッセージ数の合算になります。つまり、以下のようなやりとりが実施された場合、メッセージ数は 3 になります。


![chat1](/articles/azure-bot-service/bot-plan/chat1.png)


2) Free プランを選択し、Premium チャネルを利用したメッセージのやりとりが月ごとの上限を超えた場合、Bot に対してメッセージが送信できないようになります (何らかの理由で Bot にメッセージが届かない場合と同様の状況となり、Bot アプリケーションにてメッセージは処理されません)。


![chat2](/articles/azure-bot-service/bot-plan/chat2.png)


例)

![console429](/articles/azure-bot-service/bot-plan/console429.png)


この際、Web Chat 等でアクセスした際にブラウザーの開発者ツール (Microsoft Edge の場合は F12 キーより起動) を利用してメッセージを送付すると、以下のようなエラーが返されていることがご確認いただけます。

上記のようなメッセージが返されている場合は、Standard プランへの変更をご検討ください。

また、Premium チャネルに対しては SLA (Service Level Agreement) が適用され、もし仮に障害が発生し、下記に記載の SLA を下回るようなことがあった場合に返金の対象となりますが、こちらは Standard プランをご利用いただいた場合にのみ適用となります。

 
Azure Bot Service の SLA
https://azure.microsoft.com/ja-jp/support/legal/sla/bot-service/v1_0/


上記のメッセージ数の上限や SLA を元にどちらのプランを選択されるか、ご検討ください。
 
なお、Azure Bot Service を利用することに対してかかる料金は以上となりますが、作成した Bot アプリケーションを配置する場所として Azure サービス (Azure Web App もしくは Azure Functions) を利用する場合や、Bot の使用量の分析やトラブルシュートに Application Insights サービスを利用する場合、また、Bot を LUIS や QnA といった自然言語処理サービスと連携する場合には、それぞれの料金が追加でかかります。

「Azure Bot Service の価格」ページの画面中部の「上記の料金とは別に、以下のリソース使用にも料金が発生します。」にも記載がありますが、こちらは次回 Azure Bot Service の種類の投稿にも記載予定ですので、ご参考いただければと思います。

それではまた。

<br />

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。

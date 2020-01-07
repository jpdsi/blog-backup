---
title: Azure Bot Service の IP 制限について
date: 2019-12-16
tags: 
  - Azure Bot Service
  - IP
  - アクセス制限
---

こんにちは。今回は “Azure Bot Service (Web App Bot) をご利用時にボットアプリケーションの Azure App Service 側に IP 制限をかけることができるの？” という疑問に答えます。

## 上記の回答
結論としては、Azure Bot Service の構成上、Bot アプリケーションをホストする Azure App Service のエンドポイントに IP 制限を行った場合、Web Chat を含む各チャネルのコネクタ部分と正常に接続ができなくなってしまうため、Bot 本体をホストする App Service 側で IP 制限を行うことはできません。

もし IP 制限を実施した場合は正常に Bot と会話を行うことができなくなります。

## 詳細
というのも、Azure Bot Service (Web アプリボット) は Node.js などの Bot Builder SDK を基に実装される Bot アプリをホストする Azure App Service と、Bot アプリとクライアントの接続を弊社側で管理している各チャネルのコネクタにより構成されます。
(Web アプリボット ではなく、Bot Channel Registration の利用の場合は Bot アプリのホスト先はご自身で用意します)

Azure Bot Service (Web アプリボット) を利用した Bot アプリケーション全体像は大まかな流れを示すと、以下のような構成です。
(括弧書きしているように QnA Maker などの外部 API や DB をボットでご利用の場合には Bot アプリからそちらにアクセスします)

> Web Chat 等の各チャネルのクライアントアプリケーション <-> 弊社で管理する各チャネルのコネクタ <-> Azure App Service 上で動作するお客様の Bot アプリ (<-> QnA Maker などの外部 API や DB など)

Bot アプリとクライアントの接続を仲介する弊社側で管理する Web Chat を含む各チャネルのコネクタサーバーについてはグローバルなリソースであり、IP アドレスを含むホスト先等の明確な情報については通常公開しておりません。

そのため、上記の右側部分の Azure App Service 上で何らかの IP アドレス制限をかけると、
コネクタを介してアクセスが出来なくなり、正常に応答ができなくなる状況になることが想定されます。

補足ですが、Web Chat チャネルをご利用の場合には下記のような回避案が考えられます。

### 参考 1 : Web チャット利用時の回避案について
チャンネルにて以下の Web Chat チャネルをクライアントにご利用の場合、クライアント側である Web Chat の HTML リソースを配置する場所に IP 制限をかけることは可能です。

> 参考 : Web チャットの概要
> https://docs.microsoft.com/ja-jp/azure/bot-service/bot-builder-webchat-overview?view=azure-bot-service-4.0#how-to-use

具体的には、Web Chat の HTML ファイル等のクライアント側のリソースを、Web App Bot の Azure App Service とは別の Azure App Service 等でホストし、そこに IP 制限をかけることで、ご要望に近い形で IP 制限をかけることが可能と存じます。

構成のイメージ : 
  - 別途 Azure App Service を新たに作成する
  -  Web Chat の HTML ファイルをシークレット キー変更の上、上記の App Service にホストする
  - 上記の App Service でご要望の IP 制限を設定する
  - クライアントは上記の Azure App Service のエンドポイントにアクセスし、その Web Chat (HTML) を介して Bot と会話する

ただ、その場は Bot アプリに直接アクセスできてしまうのではないか、と心配される方もいらっしゃかもしれませんが、Bot Framework を利用して開発した Bot には各チャンネルのコネクタと連携して自動的に認証を行う仕組みがあり、Bot アプリ側に設定している App ID および App Password を知らないユーザーからは Bot にアクセスできないよう保護されています。

> Authentication
> https://docs.microsoft.com/ja-jp/azure/bot-service/rest-api/bot-framework-rest-connector-authentication?view=azure-bot-service-4.0

そのため、Bot アプリ側に設定している App ID および App Password を適切に管理していただければ、Bot アプリへの不正なアクセスを防ぐことが可能ですので、ご安心いただければと思います。


### 参考 2 : QnA Maker 側の IP 制限について
Azure Bot Service とよく一緒に利用されるサービスとして、QnA Maker が挙げられます。
Web App Bot と同様、QnA Maker 作成時には QnA Maker 用の Azure App Service が別途自動作成されます。

この QnA Maker に付随する Azure App Service では QnA Maker のランタイムが動作しており、Microsoft 側の QnA Maker の管理サービスがこのランタイムにアクセスすることで KB の管理を行っています。
管理サービスからランタイムへのアクセスは管理 API の直接呼び出しや QnA Maker のポータルの操作に伴い発生します。

QnA Maker の管理サービスの送信元 IP アドレスは公開されておらず、予告なく変更される可能性がございますので、QnA Maker ランタイムの Azure App Service は基本的にパブリックなインターネットからアクセスできる状態である必要があり、Azure Bot Service 付随の Azure App Service と同様に、こちらも IP 制限をかけることはできません。

それではまた。

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。

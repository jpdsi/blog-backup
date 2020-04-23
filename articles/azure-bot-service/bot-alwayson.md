---
title: Web App Bot の常時接続について
date: 2020-04-23
tags: 
  - Azure Bot Service
  - Azure App Service
  - 設定
---

※ これまで Azure Bot Service Support Team Blog にて公開していた記事を移しました。


こんにちは。Azure Bot Service Support Team です。

Web App Bot をご利用の際に、ボットに ”暫く経ってからアクセスした際にレスポンスが遅いように感じる” といったような事がある場合があります。今回は、そんな時に事象解消の手助けができるような方法についてご紹介します。

まずはなぜ “暫く経ってからアクセスした際にレスポンスが遅いように感じる” といったことが起こるのかについて、下記にて記載させていただきます。

<br />

---

## 考えられる原因について

まず、Web App Bot については、Bot アプリケーションを Azure App Service 上にホストするものです。

なお、Web App Bot などの Azure Bot Service の種類に関しては下記の前回の Blog 記事をご参照ください。

 
参考 : Azure Bot Service サポートチーム - Azure Bot Service の種類について
https://social.msdn.microsoft.com/Forums/ja-JP/74ec7be1-10f9-4faf-af31-89c0d36cec6f/azure-bot-service-?forum=azurebotsupportteamja


ホストする Azure App Service では一定期間アイドル状態になるとリソース節約のために、Web アプリは自動的にアンロードされるような機能が備わっております。

この機能により、システムリソースを節約できる一方で、Web アプリがアンロードされた後の最初の要求への応答が結果的に長くなります。
 
Web App Bot をご利用の際に ”暫く経ってからアクセスした際にレスポンスが

遅いように感じる” といった事象の原因の多くが上記の機能の影響と考えられます。

---

## 考えられる解決法について

上記の機能を無効にするためには Bot をホストする Azure App Service で [常時接続] をオンにします。

具体的には、下記が [常時接続] の設定を変更する手順となります。


1. Azure Portal で、ホストする Azure App Service のリソースに移動します。
2. [構成] を選択し、上部の [全般設定] タブを選択します。
3. [常時接続] の [On] (オン) を選択し、保存します。


参考 : Azure での Web アプリのアプリケーションパフォーマンスに関するよくあるご質問 – 常時接続
https://docs.microsoft.com/ja-jp/azure/app-service/faq-availability-performance-application-issues#how-do-i-decrease-the-response-time-for-the-first-request-after-idle-time


参考 : Azure Bot Service - 一般的な問題のトラブルシューティング – 常時接続
https://docs.microsoft.com/ja-jp/azure/bot-service/bot-service-troubleshoot-general-problems?view=azure-bot-service-4.0#my-bot-is-slow-to-respond-to-the-first-message-it-receives-how-can-i-make-it-faster
 

なお、利用する App Service Plan によって制限があり、その注意点についても下記にて記載いたします。


### 上記設定の変更に必要な App Service Plan

2019 年 9 月現在、上記の設定を変更するには、Basic 以上の App Service Plan が必要となります。

参考 : App Service プラン
https://azure.microsoft.com/ja-jp/pricing/details/app-service/plans/


Free や Shared の App Service Plan をご利用の場合には、Basic 以上のプランへのスケールアップをご検討ください。
 

参考 : Azure でのアプリのスケールアップ
https://docs.microsoft.com/ja-jp/azure/app-service/web-sites-scale


なお、Free プラン等のプランをご利用の場合は設定部分が既定のオフのままグレーアウトされます。

![lowplan](/articles/azure-bot-service/bot-alwayson/lowplan.png)

---

## QnA Maker を連携してご利用の場合

Web App Bot と QnA Maker を連携してご利用の方々も多いかと存じます。

その場合には、QnA Maker のリソースで利用している側の Azure App Service についても、上記の Web App Bot の Azure App Service と同様に、[常時接続] を ON に設定変更頂く必要がございます。


今回の内容としては以上となります。

また、全体的なレスポンスの一般的なパフォーマンス向上という観点では、App Service Plan のスケールアップも有効かと存じますので、こちらも状況に応じてご検討ください。 


参考 : Azure でのアプリのスケールアップ
https://docs.microsoft.com/ja-jp/azure/app-service/web-sites-scale


それでは、また。

<br />

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。

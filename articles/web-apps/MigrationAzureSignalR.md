---
title: ASP.NET Core SignalR から Azure SignalR Service への移行
date: 2020-02-26
tags: 
  - Internet Information Services
  - ASP.NET
  - Azure SignalR Service
---


こんにちは。

IIS / ASP.NET サポート チームです！
Azure SignalR Service に関するブログです。

今回は ASP.NET Core SignalR から Azure SignalR Service への移行手順をご紹介いたします。

---

## 1. Azure SignalR Service について

まずはその前に、Azure SignalR Service について簡単に記載させていただきます。

### Azure SignalR Service の概要
Azure SignalR Serviceは、ASP.NET Core SignalRフレームワーク上に構築されており、
リアルタイムのWeb機能を実現するという点についてはASP.NET Core SignalRと大きな違いはございません。

主な特徴としてはAzure SignalR Serviceを使用する場合、クライアントはアプリケーションサーバーではなくAzure SignalR Serviceに接続することです。
この接続は次のような手順で行われます。

1.	クライアントはサーバーへ接続のネゴシエーション要求を送る
2.	サーバーはアクセストークンとURLとともにAzure SignalR Serviceにリダイレクトする
3.	クライアントはAzure SignalR Serviceと永続的な接続を確立する

![azuresignalr](/articles/web-apps/MigrationAzureSignalR/azuresignalr.png)

参考 : Azure SignalR サービスとは
https://docs.microsoft.com/ja-jp/azure/azure-signalr/signalr-overview

参考 : ASP.NET Core SignalR のホストとスケーリング - Azure SignalR Service (上記の画像引用)
https://docs.microsoft.com/ja-jp/aspnet/core/signalr/scale?view=aspnetcore-3.1#azure-signalr-service


### Azure SignalR Service のメリット

通常の ASP.NET Core SignalRと比較した場合のAzure SignalR Serviceのメリットをまとめると次のようになります。

-	Azure SignalR Serviceがクライアント接続を担い、サーバーが永続的なクライアント接続を行わないため、
    - 接続関連のリソース消費量はほかの一般的なWebアプリと同程度に抑えられる
    - 接続の信頼性が Signalr Service により保証される
-	Azure SignalR Serviceには様々なクライアントに対応した SDK が用意されているため、
暗号化、認証の実装等は SDK や Azure の機能を利用して構成することで、アプリケーション実装の開発に専念することができる
-	Azure SignalR Serviceがクライアント接続を担うため、
    - 大規模なクライアント接続を処理できる
    - 接続数に応じてAzure SignalR Serviceが自動的にスケールアウトされる
    - スケーリングの際のWebsocket接続の負荷分散処理をサーバー側で行う必要がない
-	上記のようなバックプレーン管理の必要がないためWebアプリケーションが簡素化され、ホスティングのコストを削減することができる
-	Azure Functionsと併用してサーバーレス環境でリアルタイム通信を提供することも可能

---


## 2.  Azure SignalR Service への移行手順

それでは本題の移行についてです。

今回は以下のホワイトボードのサンプルを ASP.NET Core SignalR から Azure SignalR Service へ移行する手順をご紹介します。

  GitHub – aspnet/SignalR-samples - WhiteBoard
  https://github.com/aspnet/SignalR-samples/tree/master/WhiteBoard

### A.	前提
-	有効な Azure サブスクリプションやアカウントを持っている
-	開発環境に Visual Studio 2019 や該当するバージョンの .NET Core SDK が入っている

### B.	Azure SignalR リソースの作成

1.	Azure portal (https://portal.azure.com/) にサインインする
2.	Azure Portal 上のリソースの作成を選択
3.	[Marketplaceを検索]テキストボックスに"SignalR Service"と入力
4.	SignalR Serviceの画面で[作成]を選択
5.	必要事項を入力し、作成を選択 (デプロイには時間がかかる場合があります)
6.	デプロイが完了したら、作成したリソースに移動し、[設定]の下の[Keys]を選択
PrimaryのCONNECTION STRINGをクリップボードにコピーする

![keys](/articles/web-apps/MigrationAzureSignalR/keys.png)


### C.	ASP.NET Core アプリケーションに Azure SignalR を追加

1.	プロジェクトに Microsoft.Azure.SignalR の NuGet パッケージを追加

(参考情報)
- https://docs.microsoft.com/ja-jp/nuget/quickstart/install-and-use-a-package-in-visual-studio
- https://docs.microsoft.com/ja-jp/nuget/quickstart/install-and-use-a-package-using-the-dotnet-cli


2.	接続文字列の設定
"appsettings.json" というファイルを作成し、以下を書き込む
”YourConnectionString” の部分はBの手順6で確認したCONNECTION STRING に置き換える。
 
 ![appsettings](/articles/web-apps/MigrationAzureSignalR/appsettings.png)

3.	Startup.csを開く
ConfigureServices メソッドで Azure SignalR Service を使うように更新するため、services.AddSignalR().AddAzureSignalR() メソッドを呼び出す。

 ![addazuresignalr](/articles/web-apps/MigrationAzureSignalR/addazuresignalr.png)
 
Configure メソッドにapp.UseAuthorization() を追加する。
 
 ![useauthorization](/articles/web-apps/MigrationAzureSignalR/useauthorization.png)

その他参考情報 : クイック スタート:SignalR Service を使用してチャット ルームを作成する
https://docs.microsoft.com/ja-jp/azure/azure-signalr/signalr-quickstart-dotnet-core#add-azure-signalr-to-the-web-app


今回は以上です。 それでは、また次回！

---
title: Azure Bot Service の Portal 上の事象の調査に必要な情報
date: 2020-05-11
tags: 
  - Azure Bot Service
  - Azure Portal
  - ログ採取
---

こんにちは。Azure Bot Service サポート チームです！  

以前 Azure Bot Service (Web App Bot もしくは Bot Channels Registration) のボットの動作に関する情報採取について以下をご紹介しましたが、今回は Azure Bot Service をご利用する際に、Azure Portal 上で問題が起こった際に必要となることが多い再現手順と通信ログの情報採取をご紹介いたします。

現象により必要な情報は異なりますが、今回はほとんどの Azure Bot Service の Portal 上の問題に関する調査に対して有効な情報について以下にご紹介いたします。

例えば、以下のような問題があげられます。

- Azure Portal 上にて Azure Bot Service のリソース作成時にエラーがでる
- Azure Portal 上の Azure Bot Service リソースのあるパネルのページのみ正常に開けない

<br />

---

## 0. 前置き
Azure Bot Service 作成時に出力されるエラーとして例えば以下のようなものがあり、
それぞれ該当する回避策を実施することで回避できる可能性がございます。
もし該当のエラーが出ているような場合にはまずは以下をお試しください。

#### パターン 1 : Azure Bot Service 作成時に “Authorization_RequestDenied” エラー

Azure Bot Service 作成時に “Authorization_RequestDenied” エラー が出ている
場合は、以下の事象に該当している可能性が高いので、下記情報をまずはご参照ください。

参考：Bot 作成時に “Authorization_RequestDenied” エラーが出力される
https://social.msdn.microsoft.com/Forums/en-US/eecc490d-4d0a-4e8d-bc90-3f927ebe78c6/bot-authorizationrequestdenied-?forum=azurebotsupportteamja

参考 : ボットを作成するときに Authorization_RequestDenied という例外が発生するのはなぜですか。
https://docs.microsoft.com/ja-jp/azure/bot-service/bot-service-troubleshoot-general-problems?view=azure-bot-service-4.0#why-do-i-get-an-authorization_requestdenied-exception-when-creating-a-bot

#### パターン 2 : Azure Bot Service 作成時に “Can't register resource provider 'Microsoft.BotService'” エラー

Azure Bot Service 作成時に “Can't register resource provider 'Microsoft.BotService” エラー が出ている場合は、
下記サイトを参考に Microsoft.BotService のリソースプロバイダーのご確認と登録を実施ください。

参考：リソース プロバイダーの登録エラーの解決
https://docs.microsoft.com/ja-jp/azure/azure-resource-manager/templates/error-register-resource-provider#solution-3---azure-portal


---

## 1. 事象解消のために考えうる回避案

上記 0 に該当しない場合、もしくはそれぞれの回避策を実施しても解決しない場合、
本事象が一時的な問題の可能性もございますので、以下をそれぞれ実施頂き、
いずれかで事象が解消するかどうか、まずは以下の手順でご確認ください。

### 確認事項 1 :ブラウザのシークレットモードで確認

一旦開いているブラウザ等を閉じ、ブラウザ (Google Chrome や Microsoft Edge など) のシークレットウィンドウにて
Azure Portal を新たに開き、該当の再現手順をあらためて実施した際に事象が解消するかご確認ください。


### 確認事項 2 : ポータルの言語設定を英語に変更し、確認

上記のシークレットモードのまま、Azure Portal を開き、右上の設定アイコンの以下の操作で言語を
[English] に変更し、該当の再現手順をあらためて実施した際に事象が解消するかご確認ください。 


### 確認事項 3 : 時間をおいて確認

時間を一定時間空けてから、該当の再現手順をあらためて実施した際に事象が解消するかご確認ください。 


もし上記の 3 つでも正常でない場合、以下の情報を採取しそれを基に調査方針を検討することが可能です。
主に、以下の手順ではクライアント側の HTTP 通信の応答と、具体的な再現時の操作状況を確認いたします。

---

## 情報採取 : Fiddler ログ、問題ステップ記録ツール (PSR) のログ

弊社サポート対象外のツールではございますが、 Fiddler というツールを利用することで HTTPS の通信を含めたキャプチャを取得することが可能です。
本ツールをご利用いただくには、対象の端末に本ツールをインストールしていただく必要があります。
以下に、HTTPS 通信を含めたネットワークアクセスのキャプチャ手順をご案内いたします。
 
### 事前準備

#### 1. Fiddler のダウンロード

以下のサイトから、Fiddler4 をダウンロードし、トレースを取得する端末にインストールします。

Download Fiddler
https://www.telerik.com/download/fiddler/fiddler4
 

#### 2. Portal 上の言語の変更

ポータルの言語設定を上記の確認事項 2 のように、言語設定を [英語 (English)]に変更します。
確認事項 1 のように、Chrome などのブラウザの [シークレットモード] で Azure Portal (https://portal.azure.com/) を再度新たに開きます。
※ まだ再現手順は行わずに、Azure Portal 上のトップページのまま、以下の手順 3 を実施します。


### 情報採取

#### 3. PSR 採取開始
Windows 標準の画面キャプチャソフトである問題ステップ記録ツール (PSR) を起動します。

3-1.クライアントにて、Win + R キーを押し、[ファイル名を指定して実行] より "psr" と入力し、[OK] をクリックします。

3-2. [ステップ記録ツール] が起動しましたら、右端の ▼ をクリックし、[設定] をクリックします。

3-3. [保存する最新の取り込み画像数] を "25" から "150" に変更し [OK] をクリックします。

3-4. [記録の開始] をクリックします。
 

#### 4. Fiddler ログの採取と PSR の採取終了

4-1. 接続元クライアントにて、Fiddler を起動します。

4-2. Fiddler のメニュー バーから [Tools] - [Fiddler Options] を選択します。

4-3. HTTPS タブをクリックし、"Capture HTTPS CONNECTS" および "Decrypt HTTPS traffic" のチェック ボックスをオンにし、OK ボタンをクリックし、Fiddler Options を閉じます。

* "Decrypt HTTPS traffic" のチェック ボックスをオンにすることで、以下の警告が表示されますので、Yes ボタンをクリックします。
"Fiddler generates a unique root CA certificate to intercept HTTPS traffic. You may choose to have Windows trust this root certificate to avoid security warnings about the untrusted root certificate. You should ONLY click 'Yes' on a computer used exclusively for TEST purposes."
 
* 上記警告で Yes を選択すると、セキュリティ警告が表示されますので、"はい" を選択します。
これにより、個人ストア、および信頼されたルート証明機関ストアに発行者が "DO_NOT_TRUST_FiddlerRoot" である証明書が追加されます。
 
4-4. 画面右側にある Filters - User Filter がオフになっていることを確認します。

4-5. Fiddler のキャプチャ画面にアクセスがトレースされていることを確認してください。

4-6. Azure Portal 上で現象を再現させます。

4-7. 現象やエラー等を確認できましたら、Fiddler のメニュー バーから [File] - [Save] - [All Sessions] を選択し、.saz 形式でログを保存します。

4-8. PSR の画面で [記録の停止] をクリックします。

4-9. PSR の画面で [保存] をクリックし、ファイルの保存場所、ファイル名を指定し、[保存] をクリックします。

4-10. Fiddler を終了します。

4-11. PSR と Fiddler で採取した情報を基に調査を行います。
 

#### 5. Fiddler アンインストールについて (任意)

アンインストールについては下記をご参考にいただければと存じます。

5-1. コントロール パネルから [プログラムと機能] を選択します。

5-2. Fiddler をダブル クリックし、"Uninstall" ボタンをクリックしアン インストールします。

5-3. [スタート] - [検索の開始] に certmgr.msc と入力し、Enter キーを押下します。

5-4. 証明書管理スナップインの左ペインから、[個人] - [証明書] を展開し、右ペインから発行者が "DO_NOT_TRUST_FiddlerRoot" となっている全ての証明書に関して、証明書を右クリックし、"削除" を選択し、証明書を削除します。

5-5. 証明書管理スナップインの左ペインから、[信頼されたルート証明機関] - [証明書] を展開し、右ペインから発行者が "DO_NOT_TRUST_FiddlerRoot" となっている全ての証明書に関して、証明書を右クリックし、"削除" を選択し、証明書を削除します。

                  
※ なお、Fiddler 自体のツールに関するトラブルシューティングや操作方法等のご質問は 3 rd party 製品となる為、本 Azure サポートでは承ることができませんのでご了承ください。


<br />

上記を基に私共サポートにて Azure Bot Service リソースにおける Azure Portal 上の事象に関して調査を承ることが可能です。

なお、状況に応じて、例えば Azure Bot Service のそのほかの情報や詳細な画面キャプチャなど追加の情報が必要になることも多々ございますが、上記をまず確認した上で、より詳細な調査に向けて調査方針や追加の情報採取を検討していくことが可能となります。

今回は以上です。それでは、また次回！

<br />

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。

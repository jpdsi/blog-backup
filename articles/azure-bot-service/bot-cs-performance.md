---
title: Web App Bot (C#) の応答遅延のログについて
date: 2020-03-25
tags: 
  - Azure Bot Service
  - C#
  - パフォーマンス
---

こんにちは。
Azure Bot Service サポートチームです。

今回は、Web App Bot (Azure Bot Service) と Bot Framework SDK (C#) 利用時の Bot アプリケーションの応答パフォーマンスの遅延系に関する現象の調査に役立つようなログの採取方法をご紹介いたします。

---

## 前提 1 : 常時接続機能の有効化と App Service Plan のスケールアップ

なお Web App Bot をご利用で “暫く経ってからアクセスした際にレスポンスが遅いように感じる”
といった場合には、まずは以下のブログ記事を参考に、“常時接続” 機能の有効化をご検討下さい。

参考 : Web App Bot の常時接続について

https://social.msdn.microsoft.com/Forums/ja-JP/954765bb-66d8-41d5-89be-eccdc7284a2b/web-app-bot-123982412026178255093215412395123881235612390?forum=azurebotsupportteamja

また、全体的な Bot アプリのレスポンスの一般的なパフォーマンス向上という観点では
App Service Plan のスケールアップも有効かと存じますので、こちらもご検討ください。

参考 : Azure でのアプリのスケールアップ

https://docs.microsoft.com/ja-jp/azure/app-service/web-sites-scale

なお、下記のように運用環境では Standard 以上の App Service プランを利用頂く事を推奨しております。

参考 : Azure App Service プランの概要

https://docs.microsoft.com/ja-jp/azure/app-service/overview-hosting-plans

今回は上記の常時接続やスケールアップを行っても、特定の処理が遅い場合に
有効と考えられる調査ログの採取方法について紹介いたします。

なお、このログは再現手順が確立されている、もしくは再現頻度が高く、再現が可能な場合に有効です。
再現手順が確立できていない場合には、まずは再現を行える条件を切り分けいただくようお願いいたします。

<br />

---

## 前提 2 : 該当の Web App Bot について
以下の Web App Bot のリソースがあると仮定して手順をご紹介いたします。

-	Web App Bot (C# - ASP.NET Core 2.1 / Echo Bot のテンプレートを選択)
-	Bot Framework SDK v 4.6.3 を利用 (2020 年 2 月現時点)
-	上述した Web App Bot に結び付く Azure App Service の App Service Plan は Standard プラン以上
-	上述した Azure App Service の常時接続機能が ON になっている

---
## 具体的な情報採取手順

### A. 該当の Web App Bot のコード変更

1. Program.cs を書き換え
Program.cs 内の CreateWebHostBuilder を下記のように書き換えます。

```
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .ConfigureLogging(logging =>
                {
                    logging.AddEventSourceLogger();
            });
```

2. appsettings.json にログ設定を追加
以下は appsettings.json を新たに新規作成し、以下の内容で保存します。

```
{
    "Logging": {
        "LogLevel": {
            "Default": "Information"
        }
    }
}
```

3. App Service Editor や Kudu で編集した場合は変更後に build します。

<br />

[ご参考 : App Service Editor や Kudu でのコード変更後 (C# の場合)]

以下の通りビルドが必要となりますので、ご注意いただけますと幸いです。

3-1. Azure Portal 上の該当の Bot リソースを選択

3-2. 左パネルの [すべての App Service 設定] > [高度なツール] > [移動] で Kudu を開く

3-3. 上部の [Debug console] の [PowerShell] を選択

3-4. [site] フォルダをクリック

3-5. [wwwroot] フォルダをクリック

3-6.   ./build.cmd を実行

3-7. 実行結果として エラーがなく Finished successfully. が下部に出力されたことを確認します 

![BuildCmd](/articles/azure-bot-service/bot-cs-performance/buildcmd.png)

build.cmd の実行イメージ

<br />

### B. 情報採取と再現確認

1. Azure Portal 上にて、Web App Bot に結び付く Azure App Service のリソースページに移動します。
(Azure Portal 上の該当の Web App Bot > [すべての App Service 設定] にて結び付く Azure App Service に移動)
2. [問題の診断と解決] パネルを選択
3. [Diagnostic Tools] に移動
4. [Collect .NET Profiler Trace] ボタンを選択
5. Instance(s) にてチェックボックスにチェックする
6. [Collect Profiler Trace] ボタンを押す。

7. 下記の画面 (Step 2) になったら、Web App Bot の Web チャットでテスト 等で再現を行う。
(60 秒間で Profiler が停止するので、Step 3 に行くまでの 60 秒間以内に再現を行います)

![Step2](/articles/azure-bot-service/bot-cs-performance/profiler.png)

8. 先ほどの手順 6 の Profiler 取得の画面に戻り、zip ファイルがダウンロードできるようになっているのでダウンロードする。
(具体的な解析方法はご紹介できかねますが、本 zip ファイルを基に、PerfView (https://github.com/microsoft/perfview) 等を利用し解析を行います。)

<br />

[ご参考]

参考情報 1 : App Service Diagnostics – Profiling an ASP.NET Web App on Azure App Service
https://azure.github.io/AppService/2018/06/06/App-Service-Diagnostics-Profiling-an-ASP.NET-Web-App-on-Azure-App-Service.html

参考情報 2 : PerfView Tutorial (Video)
https://channel9.msdn.com/Series/PerfView-Tutorial


今回は以上です。 それでは、また次回！

---
title: ラピッド フェール保護機能について
date: 2020-02-26
tags: 
  - Internet Information Services
  - Rapid Fail Protection
---

こんにちは。
 
IIS サポート チームです！

今回は、IIS サポート チームへのお問い合わせの中で、比較的お問い合わせ数が多い
ラピッド フェール保護機能（Rapid Fail Protection）について Blog 記事を執筆します。

英語の言葉の通り、**”迅速に障害を検出する”** 機能となりますが、より詳しく定義すると以下となります。

**「既定で有効であり、一定時間内に指定回数以上エラーが発生した際、"想定外のエラーが頻発している"
　とみなし、対象のアプリケーション プールを停止させることで、 システム全体を保護する機能」**
　※サーバーへの負荷増加を防ぐために、アプリケーション プールは自動的に起動しなくなります
　※上記の定義中にある “一定時間” と “一定回数” については後述いたします

　Process Management
　https://docs.microsoft.com/en-us/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#process-management

この機能により、最終的にアプリケーション プールを停止させますので、結果として、突然 IIS がHTTP 503
（Service Unavailable）エラーを返し、クライアントが IIS 上のサービスを利用できないこともございます。
ただ、この機能が働くトリガーとなった、頻発している想定外のエラーを把握・調査することが重要です。

実際に、IIS 上のサービスが使用できないとユーザーから申告があると仮定し、想定外のエラーの検知、
またラピッド フェール保護機能が働いたかどうかを、複数のログを見ながら一緒に確認していきましょう。

また、関連するよくあるお問い合わせや、ラピッド フェール保護機能に関する設定もご紹介いたします。

---------------

## <u> 1. ログの確認方法 </U>
#### 1.イベント ログ（システム）
ラピッドフェール保護が機能したことを示すイベント WAS 5002 のイベントを確認します。

1. ラピッド フェール保護機能が働いていたかどうか、またこの機能によりアプリケーション プールが
停止していたかどうかを確認するためにイベント ログの “システム” を開きます
2. 問題発生時刻に 5002 のイベント ID が出力され、[全般] タブに以下のメッセージの出力を確認します

　**「アプリケーションプールを提供しているプロセス内での一連のエラーのため～」**

![reference](/articles/web-apps/Rapid-Fail-Protection/pic-1.png)

3. ラピッド フェール保護機能は、既定では 5 分以内に 5 回のエラーが発生した際に働くため、
　 イベント ID 5002 が出力された時間の 5 分前を確認して、”ソース” が “WAS” となっている
　 イベントが出力されていないかを確認します

　 なお、この “5 分” と “5 回” が上記の定義中にある “一定時間” と “一定回数” に該当します

#### 2.パケット キャプチャ、HTTPERR ログ
ラピッド フェール保護機能に伴い HTTPERR ログにアプリケーション プールが停止したこと
(AppOffline) で 503 が返されたことを示すログが多数記録されていることを確認します。

1. WireShark や Network Monitor 等のパケット キャプチャ ソフト、また以下の Blog 記事中の
「4. HTTPERR ログ」を確認したところ、該当のアプリケーション プールで HTTP　503 エラーが記録されている

　　IIS の調査に必要な基本的なログ情報について
　　https://jpdsi.github.io/blog/web-apps/LogCollection1/

2. HTTPERR ログの場合、問題発生時刻に “AppOffline” が記録されている

>　#Fields: date time c-ip c-port s-ip s-port cs-version cs-method cs-uri sc-status s-siteid s-reason s-queuename
>　2020-XX-XX XX:XX:XX 10.XX.X.XX PORTNUM 10.XX.X.XX PORTNUM HTTP/1.1 GET /IISFolder / 503 1 AppOffline ApplicationPoolName

　　HTTP Api のエラー ログ
　　https://support.microsoft.com/ja-jp/help/820729/error-logging-in-http-apis

AppOffline	サービス利用不可のエラーには、(503 HTTP エラー) が発生しました。アプリケーション エラーの原因となったアプリケーションをオフラインにするため、サービスは使用できません。

#### 3.イベント ログ（アプリケーション）
想定外のエラーの頻発状況、つまりラピッド フェール保護機能が働いた契機となる問題を確認します。
（すべて原因が、イベント ログのアプリケーションから確認できるわけではない点は注意が必要です）

1. イベント ログの “アプリケーション” を開きます
2. 問題発生時刻に、例えばイベント ID 1000 のアプリケーション クラッシュの出力されているかを確認します

![reference](/articles/web-apps/Rapid-Fail-Protection/pic-2.png)

3. アプリケーション クラッシュが出力されている場合は、当該イベントをクリックした上で、
　 [全般] タグにてクラッシュの概要を確認します

![reference](/articles/web-apps/Rapid-Fail-Protection/pic-3.png)

　w3wp.exe がクラッシュしている場合には、その例外コードを使用し、以下の記事にある通り
　ダンプ ファイルを取得してクラッシュの根本原因を調査することも可能です

　　IIS の調査に必要なダンプ ファイルの採取について(Debug Diagnostic Tool)
　　https://jpdsi.github.io/blog/web-apps/LogCollection4/
　　※手順 11 に記載の「Exception Code 」や「Optional Exception Name」は変更が必要です

## <u> 2. アプリケーション クラッシュに至る原因（よくある事例）について</u>
上記で確認した様なクラッシュは、アプリケーションの例外に起因して発生することが一般的です。

ただし、短時間で頻繁に発生している場合には、クラッシュに至るアプリケーション処理が行われたわけではなく、
IIS やアプリケーションの処理がウイルス対策ソフト等のフィルター ドライバーによって一時的に阻害されていた可能性が考えられます。

アンチ ウィルス ソフトのようなセキュリティ関連ソフト等により、IIS 関連のフォルダーやファイルへの
アクセスが阻害された、アプリケーション プールが意図せず終了してしまう報告例もあるため、
意図せずラピッド フェール保護機能が働いていた場合には、以下もご確認いただければと思います。

　　IIS 観点でアンチ ウイルス スキャン対象から除外したいフォルダー
　　https://jpdsi.github.io/blog/web-apps/iis-exclude-antivirus-scanning/

## <u> 3. ラピッド フェール保護の設定について</u>
ラピッド フェール保護機能を無効化、または冒頭に記載の “一定時間” と “一定回数” は以下で変更可能です。

> 場所 : [IIS マネージャー]-[アプリケーション プール]-[(対象のアプリケーション プール)]-[詳細設定]-[ラピッド フェール保護]
> 項目 : [エラー間隔 (分)]、および [最大エラー数]

例えば、頻発しているエラーを既に把握している場合（エラーの発生を許容している場合）、
アプリケーション プールが停止に至らないよう “一定時間” と “一定回数” を引き上げることが出来ます。
「10分間に100回のエラーがあった場合に停止する」ように設定する場合には、以下を指定します。

> エラー間隔(分)：10
> 最大エラー数：100

　　Failure Settings for an Application Pool
　　https://docs.microsoft.com/en-us/iis/configuration/system.applicationhost/applicationpools/add/failure

今回は以上です。 それでは、また次回！

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。 もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。
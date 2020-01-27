---
title: IIS の調査に必要なダンプ ファイルの採取について
date: 2020-01-27
tags: 
  - Internet Information Services
  - ログ採取
---

こんにちは。
 
IIS サポート チームです！
 
IIS で問題が発生した時、実はイベント ログにもその問題に関する多くの情報が記録されます。
この中で Active Server Pages について最も多いものが [ソース] に [Active Server Pages]、
[全般] に以下のメッセージが記録されるパターンです。

　**エラー : ファイル （asp名称）  予期しないエラーです. 外部オブジェクトで**
　**トラップできるエラー (C0000005) が発生しました。スクリプトの実行を続行できません。**

![reference](/articles/web-apps/LogCollection4/reference1.png)

エラー "C0000005" は、アクセス違反の例外を示しておりますことから、
IIS 上でホストする ASP アプリケーションにおいてアクセス違反の例外が
発生した状況が推察されアプリケーションでアクセス違反が発生する要因については、
プログラム内部のメモリーアクセス状況を詳細に調査することが必要となります。

そこで、拡張子 .dmp のファイル（ダンプ ファイル）を取得して解析することが有用です。

---------------
 # <u>ダンプ ファイル採取手順</u>
  
1. 下記サイトにアクセスし、リダイレクトされたダウンロード センターから、対象のマシンの適切なプラットフォーム (32bit もしくは 64bit) に対する Debug Diagnostic Tool をダウンロードし、インストールします。

   Debug Diagnostic Tool v2 Update 3
   http://debugdiag.com/

   Debug Diagnostic Tool v2 Update 2
   https://www.microsoft.com/en-us/download/details.aspx?id=49924

　　　※v2 Update 3 の場合、特定の環境で正常にインストールできないという
　　　　報告例がございますので、その場合には v2 Update 2 をご使用ください

　　　※ツールのインストールが難しい場合は、任意のコンピューターにインストール後、
　　　　インストールしたフォルダーごと対象のサーバーにコピーし、Register.bat コマンドを
　　　　管理者権限で実行することでもご利用いただけます。
　　　　アンインストール時は、Unregister.bat コマンドを管理者権限で実行いただければ問題ございません

2. [すべてのプログラム] - [Debug Diagnostic Tool 2.0] - [Debug Diagnostics Tool 2.0 Collection] を起動します。
3. "Select Rule Type" 画面が自動的に表示された場合、一旦 [キャンセル] ボタンを押してダイアログを閉じます。
4. Debug Diagnostics Tool の [Tools] - [Options And Settings] メニューをクリックします。
5. [Performance log] タブの "Enable/Disable Performance Logging" にて、以下のラジオボタンにチェックを入れ、[OK] ボタンを押して閉じます。

   [Disable Performance Counter Data Logging]

6. 続けて、ウィンドウ下部の [Add Rules] ボタンを押し、表示された "Select Rule Type" 画面にて、[Crash] を選択し、[次へ] ボタンを押します。
7. "Select Dump Target" 画面にて、[A specific IIS web application pool] を選択し、[次へ] をクリックします。
8. "Select Target" 画面にて、表示されたアプリケーション プールの一覧から、現象が発生するアプリケーション プール名を選択し、[次へ] ボタンをクリックします。
9. "Advanced Configuration (Optional)" 画面にて、[Exceptions...] ボタンを押します。
10. "First Chance Exception Configuration" 画面が表示されますので、[Add Exception...] ボタンを押します。
11. "Configure Exception" 画面が表示されますので、各項目に以下を設定し、[OK] ボタンを押します。

   - **Exception Code (hex) : C0000005**
   - **Optional Exception Name : Access Violation**
   - Action Type : Full Userdump
   - Action Limit : 5

12. "First Chance Exception Configuration" 画面に戻りますので、[Save & Close] ボタンを押します。
13. "Advanced Configuration (Optional)" 画面に戻りますので、[次へ] ボタンを押します。
14. ルール名およびトレース ファイルの出力先 (Rule Output Location) を必要に応じてご希望の場所に変更し、[次へ] ボタンをクリックします。
15. [Activate the rule now] を選択し、[完了] ボタンをクリックします。

    ※[完了] 時にシンボル ファイルのパス (Symbol Search Path) が設定されていないことを示す
	 　　警告のポップアップが表示されることがございますが、ダンプ ファイル採取時にはシンボル ファイルは
	 　　不要であるため、[OK] ボタンでポップアップを閉じてください。

設定を完了後、現象の再現を待ちます。ワーカー プロセスの異常終了が発生しましたら、トレース ファイルの出力先に設定した箇所にダンプ ファイル (拡張子 .dmp) が出力されます。


今回は以上です。 それでは、また次回！

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。 もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。
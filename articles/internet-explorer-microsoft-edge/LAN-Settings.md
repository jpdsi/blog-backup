---
title: ローカル エリア ネットワーク (LAN) の設定について
date: 2020-04-20
tags: 
  - Internet Explorer
  - プロキシ
---

これまで Japan IE Support Team Blog にて公開していた記事のうち、プロキシ関連の記事を整理しました。
長い記事になってしまうため、下記の 4 つの記事に分けています。

**(今回の記事) ローカル エリア ネットワーク (LAN) の設定について**
[IE からみるプロキシの設定について](../ProxySettings/)
[PAC について](../pac/)
[WPAD について](../wpad/)

---

# ローカルエリアネットワーク(LAN) の設定について

### 1. ローカルエリアネットワーク(LAN) の設定

プロキシの設定は、Internet Explorer のインターネット オプション - [接続] タブの [LAN の設定] ボタンから開ける [ローカル エリア ネットワーク (LAN) の設定] ダイアログで行います。
![LANSettings](/articles/internet-explorer-microsoft-edge/LAN-Settings/LAN-Settings.png)

Windows 10 では、[Windows の設定] – [ネットワークとインターネット] – [プロキシ] とも連動しています。
![OSProxy](/articles/internet-explorer-microsoft-edge/LAN-Settings/OS-Proxy.png)

ここでは [ローカル エリア ネットワーク (LAN) の設定] ダイアログの表示内容に沿って解説します。

なお、よくあるお問い合せについて後述しましたので、そちらもご覧いただければと思います。

---

### 2. プロキシサーバーの利用を構成するための設定

プロキシ サーバーの利用を構成するための設定は大きく分けると、赤い枠で囲んだ『プロキシ自動構成 (Proxy Auto-Configuration, PAC) スクリプトを使う方法』と、青い枠で囲んだ『プロキシ サーバーのアドレスやポートを直接指定する方法』の二つがあります。
![LANSettings2](/articles/internet-explorer-microsoft-edge/LAN-Settings/LAN-Settings2.png)

それぞれの項目について順に説明します。

**(1) 自動構成**
『自動構成』の欄では、『自動構成スクリプト ファイルの取得先を自動的に検出する』か、『自動構成スクリプト ファイルの取得先を手動で指定する』かを設定します。
※ 自動構成スクリプト (PAC) とは、[アクセス先の URL や IP アドレスなどの条件](https://docs.microsoft.com/ja-jp/internet-explorer/ie11-ieak/proxy-auto-config-examples) を基に、『プロキシ サーバーを経由せずに直接通信する』か『指定したプロキシ サーバーを経由して通信する』かのどちらかの結果を返す JavaScript で記述されたファイルです。

[設定を自動的に検出する] のチェックをオンにすると、WPAD (Web Proxy Auto-Discovery) という技術を利用して、PAC ファイルの取得先 (http://contoso.com/proxy.pac や http://wpad.contoso.com/wpad.dat のような URL) を自動的に検出してファイルをダウンロードします。
自動的な検出を構成せずに、PAC ファイルの取得先を直接指定する場合には、[自動構成スクリプトを使用する] のチェックをオンにして [アドレス] 欄に PAC ファイルの取得先を入力します。

**(2) プロキシ サーバー**
![LANSettings3](/articles/internet-explorer-microsoft-edge/LAN-Settings/LAN-Settings3.png)

『プロキシ サーバー』の欄では、利用するプロキシ サーバーのアドレスやポートを直接指定します。
[LAN にプロキシ サーバーを使用する] のチェックをオンにして設定します。
[アドレス] には、ホスト名、FQDN名、IPアドレスが使用できます。
[詳細設定] ボタンを押すと、[プロキシの設定] ダイアログが表示されます。こちらについては後述します。
[ローカル アドレスにはプロキシ サーバーを使用しない] のチェックをオンにすると、"ローカル アドレス" でアクセスした場合に、指定されたプロキシ サーバーを経由せずに直接通信します。

<u>"ローカル アドレス" とは？</u>
http://contoso のように、ホスト名のみで指定された宛先です。簡単な見分け方は、ピリオド (.) を含んでいるかどうかです。IP アドレス (http://192.168.1.1 など) や FQDN (http://contoso.com など) はピリオドを含んでいるためローカル アドレスではありません。

たとえば、contoso の IP アドレスが 192.168.1.1 だったとした場合に、[ローカル アドレスにはプロキシ サーバーを使用しない] のチェックをオンにした状態で通信すると、宛先によって以下のようになります。

http://contoso
→ローカル アドレスなので、プロキシ サーバーを経由せずに直接通信します。

http://192.168.1.1 または http://contoso.com
→ローカル アドレスではないので、プロキシ サーバーを経由して通信します。

(参考情報)
Internet Explorer Uses Proxy Server for Local IP Address Even if the "Bypass Proxy Server for Local Addresses" Option Is Turned On
https://support.microsoft.com/en-us/help/262981/

<自動日本語訳版>
[ローカル アドレスにはプロキシ サーバを使用しない] オプションが有効であるにもかかわらず、Internet Explorer がローカル IP アドレスにプロキシ サーバーを使用する
https://support.microsoft.com/ja-jp/help/262981/

<br><br>

[詳細設定] ボタンを押すと表示される [プロキシの設定] ダイアログについて説明します。
![ProxySettings](/articles/internet-explorer-microsoft-edge/LAN-Settings/ProxySettings.png)

**<u>使用するプロキシのアドレス/ポート</u>**

プロキシ サーバーのアドレスとポート番号を入力します。プロトコル (http://、ftp:// など) で始まらない場合、HTTP プロキシであると見なされます。たとえば、「proxy」と入力すると、「http\://proxy」として扱われます。
また、すべての種類で同じプロキシ サーバー設定を使用するときは、"すべてのプロトコルに同じプロキシ サーバーを使用する" をオンにします。この場合は上のスクリーンショットでお分かりいただけるかと思いますが、HTTP に指定したプロキシ サーバー情報が適用されます。
(Internet Explorer 8 以降では、プロトコルの種類 gopher: は存在しません)

**<u>次で始まるアドレスにはプロキシを利用しない</u>**

プロキシ サーバーを経由せずにアクセスしたいアドレスを指定します。複数のアドレスを、セミコロンで区切って複数指定できます。
例外リストに指定可能な文字列 (最大 2064 バイト) の表記形式は以下となります。下記表記形式中の [] で括られる部分は省略可能を意味し、"" で括られる部分は固定の文字列を意味します。
[<プロトコル スキーム>"://"]<ホスト名、FQDN または IP アドレス>[":"<ポート番号>][";"*]
<プロトコル スキーム> は http や ftp 等のプロトコル名を指します。
なお、プロキシ サーバーの例外リストに指定可能なプロトコル名に特に制限はございませんが、IE がサポートする通信プロトコルは http、https、ftp のみとなります。

**<u>例外リストにワイルドカード (*) は利用できる？</u>**

はい。ワイルドカード (*) も利用できます。
例)
> \*.example.com ("some.example.com" "www.example.com" などにマッチ)
> "www.\*.com" ("www.example.com" "www.department.example.com" などにマッチ)
> "www.example.*" ("www.example.com" "www.example.org" "www.example.microsoft.com" などにマッチ)
> "123.1\*.66.\*" ("123.144.66.12" "123.133.66.15" "123.187.66.13." などにマッチ)
> "\*contoso\*"

なお、ワイルドカードの利用は充分にご注意ください。例えば "www.*.com" をプロキシ例外として指定した場合、非常に多くのサイトが該当してしまうので、意図しないアドレスがプロキシ サーバー経由せずにアクセスしてしまった、ということになりかねません。

---

### 3. よくあるお問い合わせ

**<u>複数の項目が有効だった場合の優先順位は？</u>**
[インターネットオプション] - [接続]タブの設定は、複数にチェックが入っていた場合、上から順に試行されます。つまり、優先順位は以下となります。
1. 設定を自動的に検出する
2. 自動構成スクリプトを使用する
3. LAN にプロキシ サーバーを使用する

「自動構成スクリプトを使用する」だけが設定されている場合、指定されたアドレスで自動構成スクリプトが取得できたかどうかによって、次にダイレクト接続を試行するかどうかが決定されます。
自動構成スクリプトの取得に失敗した場合は、プロキシを使用せずに対象の Web サイトへの接続を試みます。
自動構成スクリプトの取得に成功した場合は、自動構成スクリプトから返されたプロキシに接続を試みます。この場合、当該プロキシに接続できる/できないにかかわらず、以降のダイレクト接続の試行は行いません。
また「LAN にプロキシ サーバーを使用する」も同時に設定されている場合、IE自動構成スクリプトの取得に失敗すると、次に「LAN にプロキシ サーバーを使用する」のプロキシを使用した接続を試みます。
このプロキシに接続できなかった場合は、それ以上の試行は行わず「ページを表示できません」のエラーページを表示する動作となります。


**<u>自動構成スクリプトを取得するタイミング</u>**
Windows 7 までは、IE のプロセス起動後、最初に通信を行うタイミングで IE 自身が取得します。このとき、自動構成スクリプトのキャッシュが存在する場合は、一般的な Web ページのキャッシュと同じ仕組みでキャッシュが利用されます。ブラウザーのキャッシュについては こちら で詳しく説明していますので併せてご覧いただければと思いますが、有効期限内のキャッシュが存在する場合には自動構成スクリプトを Web サーバーに取得しにいくことなしに、キャッシュされた自動構成スクリプトを利用することなどにご注意いただければと思います。

一方、Windows 8 からは、WinHTTP Web Proxy Auto-Discovery Service (WinHttpAutoProxySvc) という OS の機能 (サービス) によって自動構成スクリプトを取得するようになり、IE などのアプリケーションはその情報を参照する形になりました。Windows 7 までのように IE の起動とは関係がなくなり、OS の起動時に取得されるほか、下記の状況下で再取得が試みられます。

  (A) WinHttpAutoProxySvc サービスの再起動時 (Windows 10 Version 1803 からはサービスの再起動はできません)
  (B) NLA (Network Location Awareness) サービスの再起動時
  (C) ネットワークへの接続時 (構成変更 (ネットワークアダプタの無効化／有効化、LAN ケーブルの抜き差し) を含む)

上記の状況下で再取得が試みられますが、Windows 7 までの IE 上での動作と同様にキャッシュが利用されます。そのため、キャッシュの期限を無視して強制的に再取得をさせたい場合は、IE 側で閲覧履歴の削除をしてから上記の A ～ C いずれかを実施します。


**<u>自動プロキシキャッシュについて</u>**
"自動プロキシ キャッシュ" とは、接続先のホスト名とプロキシ サーバーを紐づけて記憶する Internet Explorer の機能です。
"自動構成スクリプトのファイル (pac ファイル) 自体のキャッシュ" とは異なります。

自動プロキシ キャッシュ機能では、例えば、1 回目に http://www.microsoft.com/ にアクセスを行った際には自動構成スクリプトを解析し対象となるプロキシを決定する必要がありますが、2 回目以降のアクセスでは自動構成スクリプトを再度解析することなく 1 回目に得られた結果を用いプロキシに対してアクセスを試みるような高速化に関わる動作を実現しています。

たとえば、以下のような pac で、192.168.100.100 がダウンして、192.168.100.200 が利用されたというシナリオを考えます。

```
function FindProxyForURL(url, host)
{
if (shExpMatch(host, "*.microsoft.com")) {
  return "PROXY 192.168.100.100; PROXY 192.168.100.200"; }

return "DIRECT";
}
```

自動プロキシ キャッシュの機能によって *.microsoft.com へのアクセスは 192.168.100.200 を利用するということが Internet Explorer のメモリ上に保持され、プロセスを再起動するまで (※) キャッシュされた情報でプロキシへのアクセスをします。(つまり、最初から 192.168.100.200 からアクセスします)
※ Windows 10 Version 1607 からは、自動プロキシ キャッシュの機能の処理が WinHTTPAutoProxySvc に移動したため、Internet Explorer を再起動することでは自動プロキシ キャッシュの情報はクリアされず、WinHTTPAutoProxySvc の再起動 (または、Pac の再読み込み) でクリアされるように変わりました。

   Internet Explorer の自動プロキシ キャッシュを無効にする方法
   https://support.microsoft.com/ja-jp/help/271361/how-to-disable-automatic-proxy-caching-in-internet-explorer

これに加えて、プロキシ サーバーとの接続を確立できない場合は、当該プロキシ サーバーは「無効なプロキシ サーバー」の一覧というメモリ上で「30 分間」保持される情報として管理されるものがあります。
この動作により KB320507 で説明する、無効と判断したプロキシ サーバーに対しては、IE を再起動するか、30 分間経過するまでは接続を再試行しないという状態になり得ます。

   Internet Explorer が 30 分間に無効なプロキシ サーバーを再試行しません。
   https://support.microsoft.com/ja-jp/help/320507/internet-explorer-does-not-retry-bad-proxy-server-for-30-minutes


**<u>IE11 以降における、file プロトコルでの自動構成スクリプトのアドレス指定について</u>**
IE11 では、自動構成スクリプトのアドレスに file プロトコルでアドレスを指定できません。
例えば以下のような指定は無効で、この指定では自動構成スクリプトを利用することができません。
- C:\temp\proxy.pac
- file://example/proxy.pac

* 動作変更の背景
もともと IE10 以前でも、上記のようなfile プロトコルによる指定は推奨されていませんでした。理由は、IE だけではなく、他のアプリケーションも自動構成スクリプトの設定を利用するものがあるためです。
具体的には、IE が通信に利用するモジュール “WinInet” では上の指定は有効なのですが、Windows Update などが利用する通信モジュール “WinHTTP” では、file プロトコルによる自動構成スクリプトのアドレス指定はサポートされていません。そのため、IE は問題なく自動構成スクリプトを利用でき、通信が行えても、Windows Update 等の他のアプリケーションが自動構成スクリプトを利用できず、問題が発生することがあるのです。これは以下の技術文書でも説明しています。

 自動構成スクリプトの指定方法によって Windows Update が失敗する
  https://support.microsoft.com/ja-jp/help/890444

以上のような背景を踏まえ、file プロトコルでのアドレス指定は、Windows Update 等 WinHTTP を利用しているアプリケーションとの相互運用性を向上させるため、IE11から利用できなくなりました。

なお、Windows 10 Version 1607 より前の環境については、アドレス指定の見直しを進めていただく間の暫定的な対応策として、以下のレジストリを設定していただくと、IE11 でも file プロトコルでのアドレス指定が有効となります。

|  |  |
|---|---|
| キー | HKLM\SOFTWARE\Policies\Microsoft\Windows\CurrentVersion\Internet Settings |
| 名前 | EnableLegacyAutoProxyFeatures |
| 種類 | REG_DWORD |
| 値 | 1 |

あくまで暫定的な対応のために用意された方法であり、<font color="red">Windows 10 Version 1607 からはこの方法は利用できません。</font>お手数をおかけしますが、自動構成スクリプトは http または https プロトコルでアクセス可能な Web サーバーに配置してご利用いただけますようお願いいたします。


**<u>プロキシサーバーの設定を、外部のアプリケーションやスクリプトなどから変更したい</u>**
WinINet というモジュールに含まれる公開 API の InternetSetOption 関数を利用することでプロキシ サーバーの設定を行うことができます。

 InternetSetOptionW function
 https://docs.microsoft.com/en-us/windows/desktop/api/wininet/nf-wininet-internetsetoptionw

実装例は以下のドキュメントにありますので併せてご覧ください。

 Setting and Retrieving Internet Options
 https://docs.microsoft.com/en-us/windows/desktop/WinInet/setting-and-retrieving-internet-options
  ※ 「Setting Connection Options」の部分です。

なお、プロキシの情報は下記の DefaultConnectionSettings とそれに関連する複数のレジストリが自動的に同期をとりながら管理されています。

HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\Connections\DefaultConnectionSettings
HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\AutoConfigURL
HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\ProxyEnable
HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\ProxyOverride
HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\ProxyServer

DefaultConnectionSettings はフォーマットが非公開のバイナリー データであり、基本的にはこれらのレジストリが直接編集されることは想定しておらず、たとえば、ProxyEnable や ProxyOverride などを個別に直接変更するとレジストリ間で不整合が生じて予期しない動作 (プロキシの設定が消えてしまったり、想定した通信にならないなど) となる場合があります。
技術的な観点からは、これらのレジストリをまとめて配布することでプロキシの設定を行うことができますが、サポートされる方法としては、上記の InternetSetOption 関数を利用することです。

以上です。

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。
---
title: WPAD について
date: 2020-04-20
tags: 
  - Internet Explorer
  - プロキシ
  - pac
  - WPAD
---

これまで Japan IE Support Team Blog にて公開していた記事のうち、プロキシ関連の記事を整理しました。
長い記事になってしまうため、下記の 4 つの記事に分けています。

[ローカル エリア ネットワーク (LAN) の設定について](../LAN-Settings/)
[IE からみるプロキシの設定について](../ProxySettings/)
[PAC について](../pac/)
**(今回の記事) WPAD について**

---

今回は、お問い合わせいただくことが多い「プロキシの自動検出」について紹介します。

# "プロキシの自動検出 - WPAD (Web Proxy Auto-Discovery)" とは

WPAD は、クライアントにプロキシの設定を自動配布するための方法として開発された技術です。

プロキシの設定はネットワーク管理者が DHCP サーバーや DNS サーバーに対して行います。クライアント (IE) 側では [設定を自動的に検出する] のチェックをオンにするだけで OK です。

これによりユーザーにプロキシ サーバー設定の手間をかけることなく、管理者がプロキシ サーバー経由の通信を管理できます。

[設定を自動的に検出する] のチェックがオンであると、IE はネットワーク管理者が DHCP サーバーや DNS サーバーに登録した自動構成スクリプトの宛先を「自動的に検出」して取得します。

そして、取得した自動構成スクリプトに記載された条件に従ってプロキシを利用します。

以下の順でスクリプトファイルの格納場所を検出します。

a. DHCP サーバーへ問い合わせ
b. DNS サーバーへ問い合わせ
c. NetBIOS 名を用いた問い合わせ

まず DHCP の “DHCPINFORM” プロトコルを使って自動構成スクリプトの URL を取得しようとします。

この取得に失敗した場合は、”wpad” というホスト名に対して HTTP 通信で “/wpad.dat” というファイルの取得を試みます。

wpad.dat というファイル名は固定ですが、ファイルの中身自体は一般的に .pac という拡張子が使われる自動構成スクリプトです。

ホスト名の解決は、DNS → NetBIOS の順に問合わせを行います。

DNS サーバーへの問い合わせは、具体的には以下の手順で行います。ここでは例として、クライアントのコンピュータ名が PC01.example.contoso.com とします。

i. http://wpad.example.contoso.com/wpad.dat へ問い合わせを行う
ii. http://wpad.contoso.com/wpad.dat へ問い合わせを行う

※ DHCP サーバーにて DHCPINFORM オプションが定義されていない場合のみ、DNS サーバーへの問い合わせを行います。DHCPINFORM オプションが定義されていた場合は、DNS サーバーへの問い合わせは行いません。

※ Windows 7 + Internet Explorer 11 の組み合わせでは、i. の問い合わせのみ行い、ii. の問い合わせは行いません。

---

(※) NetBIOS 名を用いた問い合わせについて

中間者攻撃の脅威を低減するために、WPAD に関連した機能の動作変更が以下のセキュリティ更新プログラムにて行われています。

[MS16-077] WPAD のセキュリティ更新プログラムについて (2016 年 6 月 14 日)
https://support.microsoft.com/ja-jp/help/3161949/

セキュリティ更新プログラム適用以前は、WPAD の名前解決に DNS を利用している環境で、DNS の名前解決が失敗した場合でも NetBIOS を名前解決に利用する仕組みでした。 セキュリティ更新プログラム適用後は、更新プログラムで設定されるレジストリによって WPAD の名前解決に NetBIOS を使用しないようになります。 これにより、WPAD の名前解決が NetBIOS にフォールバックした際に、意図せず外部のネットワークへ到達することを制限し、WPAD の脆弱性を低減することを可能としています。

この動作変更により、WPAD 解決のために NetBIOS のみに依存している場合、失敗することがあります。 WPAD 解決のために NetBIOS の代わりに DHCP オプションまたは DNS を使用することをおすすめしますが、更新プログラム適用後の新しい既定の動作を変更するには、次のレジストリ エントリを作成します。

キー : HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\WinHttp
名前 : AllowOnlyDNSQueryForWPAD
種類 : REG_DWORD
値   : 0 (既定値 1)

---

DHCP および DNS でスクリプト ファイルの場所が特定できない場合、NetBIOS 名を使用し「wpad.dat」ファイルの問い合わせを行います。

以下の弊社ブログにも情報がありますので、併せてご覧ください。

Insight WPAD proxy setting on IE
https://docs.microsoft.com/en-us/archive/blogs/asiatech/insight-wpad-proxy-settings-on-ie

また、プロキシ サーバーの設定については [こちらの記事](../LAN-Settings/) をご覧ください。
 
---

## DHCP サーバーを使う方法

DHCP サーバーに対してプロキシ設定の自動配布の設定を行うには、コード番号 252 の新しい種類のオプションを作成し、自動構成スクリプト ファイルの URL を設定します。

具体的な手順は以下をご覧ください。(ここでは Windows Server 2012 R2 の画面を例としていますが、これ以前のバージョンでも同様です)

![DHCP1](/articles/internet-explorer-microsoft-edge/wpad/DHCP01.png)

上図のように、[IPv4] を右クリックし [既定のオプションの設定] をクリックします。

![DHCP2](/articles/internet-explorer-microsoft-edge/wpad/DHCP02.png)

[既定のオプションと値] ダイアログで、[追加] ボタンをクリックします。

![DHCP3](/articles/internet-explorer-microsoft-edge/wpad/DHCP03.png)

[名前] は、任意の文字列を入力します。
[データ型] は、"文字列" を選択します。
[コード] は、 252 と入力します。
[説明] は、任意の文字列を入力します。

入力後 [OK] ボタンをクリックします。

![DHCP4](/articles/internet-explorer-microsoft-edge/wpad/DHCP04.png)

[既定のオプションと値] ダイアログに戻りますので、[オプション名] に今追加した コード 252 が選択されていることを確認し、[文字列] の部分に自動構成スクリプト ファイルの URL を入力します。

[OK] ボタンをクリックします。

![DHCP5](/articles/internet-explorer-microsoft-edge/wpad/DHCP05.png)

[スコープ オプション] を右クリックし、[オプションの構成] をクリックします。

![DHCP6](/articles/internet-explorer-microsoft-edge/wpad/DHCP06.png)

[スコープ オプション] ダイアログで、[全般] タブの [利用可能なオプション] から先ほど追加したコード 252 を選択します。

[文字列の値] に先ほど設定したものが表示されていることを確認し、[OK] ボタンをクリックします。

![DHCP7](/articles/internet-explorer-microsoft-edge/wpad/DHCP07.png)

オプションに、コード 252 の設定ができたことを確認します。

---

## DNS サーバーを使う方法

DNS サーバーに対してプロキシ設定の自動配布の設定を行うには、まず、”wpad.dat” というファイル名で Web サーバーのルート ディレクトリに自動構成スクリプト ファイルを格納します。
次に、「wpad」という名前の DNS レコード (A レコードまたは CNAME レコード) を登録します。
以下に Windows Server 2012 R2 の画面での手順をまとめましたのでご参照ください。

![DNS1](/articles/internet-explorer-microsoft-edge/wpad/DNS01.png)

[前方参照ゾーン] に A レコードを追加します。

![DNS2](/articles/internet-explorer-microsoft-edge/wpad/DNS02.png)

[名前] には、”wpad” を入力します。
[IP アドレス] は、wpad.dat を配置した Web サーバーの IP アドレスを入力します。(この例では 192.168.1.1 としています)

---

Windows Server 2008 から既定で wpad というホスト名の問い合わせを拒否します。

このため Windows Server 2008 以降で DNS を使用して WPAD を構成する場合は、以下の技術情報を参考にグローバル クエリ禁止リストを更新する必要があります。

Managing the Global Query Block List
https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc794902(v=ws.10)

Removing WPAD from DNS block list
https://docs.microsoft.com/en-us/previous-versions/tn-archive/cc441517(v=technet.10)

---

#### Smart WPAD について

Windows 7 環境の IE8 以降では、Smart WPAD と呼ばれる機能があります。
Windows 7 より前の環境の IE では、WPAD による pac ファイルが『利用できない』ネットワーク環境においても [設定を自動的に検出する] が有効な場合に検出処理を行うため、IE 起動直後の初回のページ表示が遅いという報告が寄せられていました。

Smart WPAD の機能は処理の高速化のための機能であり、OS のネットワーク識別機能を利用し「WPAD により pac ファイルが取得できなかった環境」では同ネットワーク環境下において 30 日間検出を試みないようにするというものです。

Smart WPAD は、高速化のために、ネットワーク識別子ごとにかつての WPAD 解決結果を 30 日間保持する機能です。
解決結果には取得できなかった場合も含まれるので、"WPAD を使わない" という結果も保持されます。
WPAD 解決ができた場合は、解決した URL を保持します。
一度 WPAD が使用できないネットワークと判断された場合、そのネットワークに対して 30 日間は WPAD を使用しないように動作します。

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。
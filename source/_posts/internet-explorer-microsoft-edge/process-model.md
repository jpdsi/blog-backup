---
title: Internet Explorer のプロセス モデル
date: 2019-12-17
tags: 
  - Internet Explorer
  - IE
---

> 本記事は Technet Blog の更新停止に伴い、もともとばらばらに存在していた記事を一つのブログに集約／移行したものです。
> 元の記事の最新の更新情報については、本内容をご参照ください。

こんにちは。
いくつか記事が分かれてしまっていたことにより分かりづらくなってしまっていたため、改めて Internet Explorer 11 のプロセス モデルについてまとめます。


Internet Explorer 7 以前のバージョンではプロセス iexplore.exe が一つだけ起動し Web ページの処理などを行っていました。
しかしながら、このプロセス モデルの場合、閲覧中の Web ページのどれかで問題が生じた場合、ほかの Web ページへの閲覧にも影響が生じていました。
例えば、何等かの問題によりプロセスがクラッシュしてしまう状況の場合、すべての Web ページが閉じられてしまいます。

ユーザビリティを考慮した場合このような動作は好ましいものではないため、Internet Explorer 8 以降のバージョンでは、新しく **Loosely-Coupled IE (LCIE)** という新しいプロセス モデルを採用しています。
このモデルの下では、各タブ ウィンドウをホストする UI フレーム ウィンドウ (ウィンドウのアドレス バーやお気に入りバーが表示されている部分) と Web ページを実際に表示するタブ ウィンドウとが、別々のプロセス (プロセスとしてはいずれも iexplore.exe です) として動作します。

UI フレーム ウィンドウの役割を持つプロセスを "**マネージャー プロセス**" (従来は "フレーム プロセス" と呼んでいました)、タブ ウィンドウの役割を持つプロセスを "**コンテンツ プロセス**" (従来は "タブ プロセス" と呼んでいました) とそれぞれ呼んでいます。
一つのマネージャー プロセスに複数のコンテンツ プロセスが子プロセスとして紐づき、動作しています。
このような構成とすることにより、ある Web ページで問題が発生した場合でも、問題が発生したコンテンツ プロセスのみを切り離すことにより、影響を最小限に抑えることができます。
これによって、ブラウザーとしての信頼性、パフォーマンス、スケーラビリティの向上を図っています。

なお、コンテンツ プロセスは、使用可能なメモリー量、表示中のタブ数、表示中の整合性レベルなどから総合的に判断をし増減します。
このため、任意の Web ページを別ウィンドウで表示する場合に、新しくコンテンツ プロセスが起動することもあれば、既存のコンテンツ プロセス内で処理が行われる場合もあります。

## プロセスのビット数
Internet Explorer 8 および 9 では 32 ビット版と 64 ビット版が明確に分かれていたため、64 ビット OS には「64 ビット版の Internet Explorer」と「32 ビット版の Internet Explorer」がそれぞれ独立していました。
しかしながら Internet Explorer 10 以降では 32 ビット版と 64 ビット版は区別されなくなりました。
マネージャー プロセスは常に 64 ビット プロセスとして動作し、コンテンツ プロセスは 32 ビット プロセス (例外あり) として動作するよう変更されています。

なお、32 ビット OS 環境の場合、いずれのプロセスも 32 ビット プロセスとして動作します。
また、64 ビット OS 環境の場合、マネージャー プロセスを 32 ビット プロセスとして動作させる方法はありません。

> 参考資料 (2019 年 12 月 17 日現在記事が存在することを確認しています)
>
> IE8 and Loosely-Coupled IE (LCIE)
> https://blogs.msdn.microsoft.com/ie/2008/03/11/ie8-and-loosely-coupled-ie-lcie/
>
> Opening a New Tab may launch a New Process with Internet Explorer 8.0
> https://blogs.msdn.microsoft.com/askie/2009/03/09/opening-a-new-tab-may-launch-a-new-process-with-internet-explorer-8-0/


# LCIE を制御するレジストリ値
最近はお問い合わせをいただくことは少なくなりましたが、過去にはこの機能の影響により、ブラウザー ヘルパー オブジェクト (BHO) や ActiveX などのアドオンに影響が生じることがありました。
LCIE は下記のレジストリ値やグループ ポリシーを設定することにより無効にすることができるため、このような状況の場合にはお試しください。

```
< レジストリ >
キー：HKEY_CURRENT_USER\Software\Microsoft\Internet Explorer\Main
名前：TabProcGrowth
種類：REG_DWORD
値：0
※ 既定では存在しません

< グループ ポリシー >
[ユーザーの構成] または [コンピューターの構成]
 + [管理用テンプレート]
 + [Windows コンポーネント]
 + [Internet Explorer]
   - [タブ プロセスの増加率を設定] を "有効" に設定し、増加率に 0 を指定します。
```

### ※ 影響範囲 ※
上記のいずれかの方法で LCIE 機能を無効化すると、マネージャー プロセスのみで動作するようになります。
この結果、Internet Explorer 7 以前と同様に単一のプロセスで動作するようになります。
しかしながら、64 ビット OS 環境の場合は常に 64 ビット プロセスとして動作するようになるため、32 ビットにのみ対応した BHO や ActiveX などは利用することができません。

また、LCIE 機能を無効にすることにより、保護モードや拡張保護モード機能が無効となるためセキュリティが低下します。
さらに、ここでは詳しく触れませんが、クラッシュ回復機能やハング回復機能といった信頼性を向上させるための仕組みも同様に無効化されます。

このように、LCIE の無効化はデメリットの方が多いため、無効化しない状態での利用を検討いただくことをお勧めします。

> 32-bit browser applications may not work as expected in Internet Explorer 10
> https://support.microsoft.com/en-us/help/2716529/32-bit-browser-applications-may-not-work-as-expected-in-internet-explo


# セッション共有
LCIE 機能がもたらす機能の一つとして、フレーム マージによる "セッション" (資格情報、非永続的な Cookie) の共有があります。

従来の Internet Explorer では、異なるプロセスとして iexplore.exe が起動するとそれぞれが独立して動作するため、"セッション" は共有されませんでした。
LCIE 機能が有効な Internet Explorer 8 以降では、すでに起動済みの Internet Explorer が存在している場合にはその起動済みの Internet Explorer にマージされて動作いたします。
したがって "セッション" も共有されるため、例えば、すでにログオン済みなのに再度ログオンを求められる・・・というようなことがなくなり、利便性が向上しています。

ただ、このような動作が問題となる場合もあります。
下記のいずれかの方法で、セッションを共有しない状態で Internet Explorer を起動することができます。
何等かの問題が発生した場合にはお試しください。

## 方法 1) -noframemerging オプションを指定する
[ファイル名を指定して実行] (Windows キー + R キー) などから iexplore.exe -noframemerging を実行することで、明示的にセッションを共有しない状態で起動することができます。

> Internet Explorer Setup command-line options and return codes
> https://docs.microsoft.com/en-us/internet-explorer/ie11-ieak/ie-setup-command-line-options-and-return-codes

## 方法 2) FrameMarging のレジストリ値を設定しフレーム マージを無効にする
```
キー：HKEY_CURRENT_USER\Software\Microsoft\Internet Explorer\Main
名前：FrameMerging
種類：REG_DWORD
値：0
※ 既定では存在しません
```

> Session management within Internet Explorer 8.0
> https://blogs.msdn.microsoft.com/askie/2009/05/08/session-management-within-internet-explorer-8-0/

今日の記事はここまで！
いかがでしたでしょうか。改めて Internet Explorer のプロセス モデルを理解するきっかけになりましたら幸いです。

なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。

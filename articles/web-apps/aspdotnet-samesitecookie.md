---
title: ASP.NET における SameSite Cookie について
date: 2019-12-23
tags: 
  - ASP.NET
  - Cookie
  - SameSite
---

こんにちは。

今回は 2019 年 12 月の .NET Framework のセキュリティおよび品質ロールアップで導入された、ASP.NET の SameSite Cookie に関する動作変更により、Session や Form 認証の Cookie に発生しうる現象について記載します。

> .NET Framework December 2019 Security and Quality Rollup
> https://devblogs.microsoft.com/dotnet/net-framework-december-2019-security-and-quality-rollup/
>
> .NET Framework での SameSite をサポートするサポート技術情報の記事
> https://docs.microsoft.com/ja-jp/aspnet/samesite/kbs-samesite

詳細は以下のページもご参照ください。

> ASP.NET で SameSite cookie を使用する
> https://docs.microsoft.com/ja-jp/aspnet/samesite/system-web-samesite

# 現象
2019 年 12 月の .NET Framework のセキュリティおよび品質ロールアップを IIS/ASP.NET アプリケーションが動作するサーバーに適用した際に、ASP.NET の Session や Form 認証を利用してログインを管理しているサイトでこれまでと動作が異なる場合があります。
具体的には、外部のサイトから (クロスドメインで) 以下のようなパターンで該当の ASP.NET アプリケーションにアクセスしようとすると、ブラウザーから Session や Form 認証で使用している Cookie がサーバーに送られず、再認証が必要になる等の動作が発生する可能性があります。

- POST メソッドでリクエストする
- iframe 内からリクエストする
- XMLHttpRequest を使ってリクエストする


# 要因
2019 年 12 月の .NET Framework のセキュリティおよび品質ロールアップにて、[Chrome における SameSite cookie の扱いの変更](https://developers-jp.googleblog.com/2019/11/cookie-samesitenone-secure.html)に対応するために行われた変更により、サーバーから発行される ASP.NET の Session Cookie および Form 認証 Cookie に対して、SameSite = Lax 属性が自動的に付与されるように変更されています。

以下は Session Cookie および Form 認証 Cookie が発行された際の HTTP ヘッダーの例ですが、2019 年 12 月の .NET Framework のセキュリティおよび品質ロールアップを適用する前は SameSite 属性自体が付与されない動作となっていました。

### 出力例)
##### Session Cookie
> set-cookie: ASP.NET_SessionId=nqmz1oeyemsnpmqmzq5vjkzz; path=/; HttpOnly; SameSite=Lax

##### Form 認証 Cookie
> set-cookie: .ASPXAUTH=025097FC125A24EB1EE247AF15F4E318407E54DEE920EA7AAA3288F73D12701974C8715372B1809280EEA100E89F0C1C1F8354CF1CE4D2ED7C8FB83491DFC033B3643665C631A04BD6C0A60D02239DFC8D14B685A4633C2995BB9E42175E92A7; path=/; HttpOnly; SameSite=Lax

Cookie の SameSite 属性が Lax が設定されている場合、下記のページ等にも記載がありますが、上述のような方法でのクロス ドメインでのリクエスト時にブラウザーから Cookie が送付されなくなります。

> HTTP cookies
> https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies

結果、ログイン状態を保持するために利用している Session Cookie や Form 認証 Cookie がブラウザーから送られないことで、認証が求められる等の Web アプリケーションの動作に影響が出る可能性があります。


# 対処方法
サーバーが発行する Cookie の SameSite 属性を None にすることで、クロス ドメインでのリクエスト時にもブラウザーから Cookie を送られるようになります。
具体的には、ASP.NET の Session Cookie および Form 認証 Cookie に対して、それぞれ以下のような設定を行うことで、サーバーが各 Cookie の SameSite 属性を None に設定して発行するようになります。

##### Session Cookie
```xml web.config
<system.web>
<sessionState … cookieSameSite="None"/>
</system.web>
```

##### Form 認証 Cookie
```xml web.config
<system.web>
<forms … cookieSameSite="None" />
</system.web>
```

なお、この変更は上述の通り [Chrome における SameSite cookie の扱いの変更](https://developers-jp.googleblog.com/2019/11/cookie-samesitenone-secure.html)に対応するための変更となりますが、該当の変更のドキュメントにも記載の通り、SameSite 属性を None に設定した場合は Secure 属性も設定する必要があります。
以下の設定等で各 Cookie に Secure 属性も付与して発行するようにする設定も合わせて実施し、かつ、HTTPS で Web サイトにアクセスさせるように構成することもご検討ください。

##### Session Cookie
```xml web.config
<system.web>
<httpCookies … requireSSL="true"/>
</system.web>
```

##### Form 認証 Cookie
```xml web.config
<system.web>
<forms … requireSSL="true" />
</system.web>
```

もし 2019 年 12 月の .NET Framework のセキュリティおよび品質ロールアップを適用後に、ASP.NET の Session や Form 認証を利用しているサイトで予期しない動作が発生するようになりましたら、上記のような SameSite 属性が付与された Cookie が発行され、ASP.NET の Session や Form 認証に関する Cookie が現象が発生時に送付されない状況に変わっていないか、ブラウザーの開発者ツールや [Fiddler ツール](https://www.telerik.com/fiddler)等で確認してみてください。

それでは、また。


---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。

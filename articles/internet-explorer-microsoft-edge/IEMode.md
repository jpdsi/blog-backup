---
title: 新しいバージョンの Microsoft Edge の "IE モード" について
date: 2020-05-29
tags: 
  - Microsoft Edge
  - IE モード
---

こんにちは。今回は IE モードを紹介します。

IE モードは、一言で表すと『Microsoft Edge の画面の中で Internet Explorer が動く』というものです。

構成方法などを含めた詳細は以下のドキュメントをご覧いただければと思いますが、適切に制御することで、ユーザーが Internet Explorer と Microsoft Edge を意識的に使い分ける操作をせずに、Internet Explorer を必要とする古い技術で作られた Web アプリケーションを、Microsoft Edge の中でシームレスに閲覧できるようになります。

IE モードの登場により『Internet Explorer を単独で動かすのではなく、IE/Edge どちらにも対応したブラウザー環境を提供する』ことから『今後は Internet Explorer ではなく、Microsoft Edge で動作する Web アプリケーションを用意する』というように段階的な移行を進めやすくなりました。
ぜひ活用していただければと思います。

IE モードの概要
https://docs.microsoft.com/ja-jp/deployedge/edge-ie-mode

IE モード ポリシーの構成
https://docs.microsoft.com/ja-jp/deployedge/edge-ie-mode-policies

エンタープライズ モード サイト リストでのサイトの構成
https://docs.microsoft.com/ja-jp/deployedge/edge-ie-mode-sitelist

IE モードに関する FAQ
https://docs.microsoft.com/ja-jp/deployedge/edge-ie-mode-faq


何か困ったことがありましたら、私たちサポート チームまでお問い合わせください。

---

IE モードを利用するとき『認証がうまくいかない』というお問い合せをよくいただきますので、ここで紹介します。

認証がうまくいかないという話では [保護モードをまたいだ Cookie について](../cross-protected-mode-cookie/) という記事で、Azure AD (AAD) 認証を必要とする Web アプリケーションを例としました。
対象の Web アプリケーションのドメインと認証時にアクセスする login.microsoftonline.com が異なったセキュリティ ゾーンに属していた場合、証済みを示す Cookie がリクエストに含まれないことで、ユーザーがログオンしていないと判断され、再度認証を求められてしまうという現象を解説しました。

IE モードを利用するとき『どの URL を IE モードで表示するか』を設定しますが、対象の Web アプリケーションの URL に加えて、**<font color="red">認証サーバー / シングル サインオン サーバーも IE モードで動作するように明示的に構成する</font>** 必要があります。
認証サーバーも含めておかないと、以下のように IE モードと Edge とで行き来するときに認証に必要な Cookie が共有されずに認証ができません。

1. 対象の Web アプリケーションの URL にアクセスします。(IE モードで動作します)
2. 認証のために認証サーバーにアクセスします。(Edge で通信します)
3. Edge 側で認証済みを示す Cookie を受け取ります。
4. 対象の Web アプリケーションにリダイレクトしますが、ここで IE モードに切り替わります。
5. IE モードとしては証済みを示す Cookie を持っていないので、Cookie なしの状態でリクエストすることになります。
6. 証済みを示す Cookie がリクエストに含まれないことで、ユーザーがログオンしていないと判断され、再度認証を求められます。

ただ、認証時に通信するサーバーをすべて IE モードで動作するように明示的に指定してしまうと、逆に Edge でアクセスする必要があるパターンでうまくいきません。

具体例として、Azure AD (AAD) 認証を必要とする Web アプリケーションとして Office.com を使って試してみます。
(本来は IE モードを使わずに Edge でご覧いただくことをおすすめします)

まずは以下のように対象サイト (office.com) だけを登録してみます。
```
<site url="office.com">
    <open-in>IE11</open-in>
</site>
```

このサイト リストを読み込んで、office.com にアクセスすると IE モードで開きます。
サインインの操作をしてみるとわかりますが、サインインが完了しません。(場合によっては、認証画面の表示がループしてしまうこともあります)
これは、上述のとおり、証済みを示す Cookie がリクエストに含まれないことで、ユーザーがログオンしていないと判断されたためです。

そこで以下のように、今回の認証時に経由する可能性がある、login.microsoftonline.com, login.windows.net, login.live.com に対しても IE モードになるように \<open-in\>IE11\</open-in\> を設定してみます。

```
<site url="office.com">
    <open-in>IE11</open-in>
</site>
<site url="login.microsoftonline.com">
    <open-in>IE11</open-in>
</site>
<site url="login.windows.net">
    <open-in>IE11</open-in>
</site>
<site url="login.live.com">
    <open-in>IE11</open-in>
</site>
```

これにより、認証時に通信するサーバーすべてが IE モードで動作し、認証完了まで進みます。

それでは、この状態で [Outlook on the web (OWA)](https://outlook.live.com/owa/) を開いてみましょう。
OWA は IE モードで開くように登録をしていないので、通常の Edge で開きます。
サインインをしようとすると、login.live.com が IE モードとして登録されているので、サインイン画面が IE モードで開きます。
アカウント情報を入力してみると...

エラーになってしまいました :(

![owa](/articles/internet-explorer-microsoft-edge/IEMode/owa.png)


認証サーバー (login.microsoftonline.com, login.windows.net, login.live.com) を IE モードの対象から外せば、OWA は開きます。
でもそうすると、先ほどの Office.com がまたダメになってしまいます。

どちらもうまくいくように構成するにはどうしたらよいか...

**<font color="red">Edge と IE のどちらからも経由され得るサイトを『ニュートラル』に登録することで解決できます！</font>**

---

ニュートラルの機能は、その URL に遷移してきた際の直前のブラウザーを維持することです。
つまり、Edge で遷移してきた場合には Edge を維持し、IE モードで遷移してきた場合には IE モードを維持するにようにできます。

それでは、先ほどのサイト リストを以下のように認証サーバー (login.microsoftonline.com, login.windows.net, login.live.com) に対してそれぞれ \<open-in\>None\</open-in\> に変えます。

```
<site url="office.com">
    <open-in>IE11</open-in>
</site>
<site url="login.microsoftonline.com">
    <open-in>None</open-in>
</site>
<site url="login.windows.net">
    <open-in>None</open-in>
</site>
<site url="login.live.com">
    <open-in>None</open-in>
</site>
```

こうすると、IE モードで開く Office.com から認証サーバーにアクセスするときは IE モードが維持され、Edge で開く OWA から認証サーバーにアクセスするときは Edge が維持されますので、認証処理の途中でモードが変わってしまうことを回避できます。

なお、下記ドキュメントの「ニュートラル サイトを構成する」の部分にも記載がありますが、上記の例のように Edge と IE のどちらからも経由され得るサイトはすべてニュートラルに登録する必要があります。

エンタープライズ モード サイト リストでのサイトの構成
https://docs.microsoft.com/ja-jp/deployedge/edge-ie-mode-sitelist

一時的な対応策としては [Internet Explorer モード ページから起動したときに未構成サイトへの「ページ内」ナビゲーションの動作を指定する] というポリシーが利用できますので、状況に合わせた対応をご検討いただければと思います。

Internet Explorer モードでページ内ナビゲーションを保持する
https://docs.microsoft.com/ja-jp/deployedge/edge-learnmore-inpage-nav

---

#### 参考

アドレス バーから edge://compat/enterprise と入力してページを開くと、以下のように実際に読み込まれているサイト リストの情報を一覧できます。
![compat_enterprise](/articles/internet-explorer-microsoft-edge/IEMode/compat_enterprise.png)

検証の際にぜひご活用ください。

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。
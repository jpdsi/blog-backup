---
title: 新しいバージョンの Microsoft Edge のインストール後も Microsoft Edge レガシーを利用する方法
date: 2020-02-01
tags: 
  - Microsoft Edge
  - Chromium
---

こんにちは。

[こちらの記事](https://jpdsi.github.io/blog/internet-explorer-microsoft-edge/Deploy-Edge/) で、『古い Microsoft Edge (EdgeHTML) と新しい Microsoft Edge (Chromium ベース) の両方を使いたいときはどうすればいいですか？』という質問に対して、実際の設定の様子をお伝えするとしていましたが、以下にまとめますのでご覧ください！

<br />
<br />

まず、何もしないで新しいバージョンの Edge をインストールした場合は、古いバージョンの Edge は隠された状態になります。

[こちらのドキュメント](https://docs.microsoft.com/en-us/deployedge/microsoft-edge-sysupdate-access-old-edge) にあるように、[Microsoft Edge でのブラウザーの同時実行エクスペリエンスを許可するというポリシー](https://docs.microsoft.com/ja-jp/deployedge/microsoft-edge-update-policies#allowsxs) を **事前に有効にした状態で** 新しいバージョンの Edge をインストールすると、両方のバージョンの Edge が利用できるようになります。

では実際にやってみます。

<br />
<br />

## 管理用テンプレートのダウンロード

[こちら](https://www.microsoft.com/en-us/edge/business/download) から管理用テンプレートをダウンロードします。

![ファイルのダウンロード](/articles/internet-explorer-microsoft-edge/side-by-side/getpolicyfile.png)

ダウンロードしたファイル (MicrosoftEdgePolicyTemplates) を任意の場所に展開し、以下のファイルをコピーします。

|  | File1 |
| :--- | :--- |
| コピー元 | MicrosoftEdgePolicyTemplates\windows\admx\msedge.admx |
| コピー先 | C:\Windows\PolicyDefinitions\msedge.admx |

|  | File2 |
| :--- | :--- |
| コピー元 | MicrosoftEdgePolicyTemplates\windows\admx\msedgeupdate.admx |
| コピー先 | C:\Windows\PolicyDefinitions\msedgeupdate.admx |

|  | File3 |
| :--- | :--- |
| コピー元 | MicrosoftEdgePolicyTemplates\windows\admx\ja-JP\msedge.adml |
| コピー先 | C:\Windows\PolicyDefinitions\ja-JP\msedge.adml |

|  | File4 |
| :--- | :--- |
| コピー元 | MicrosoftEdgePolicyTemplates\windows\admx\ja-JP\msedgeupdate.adml |
| コピー先 | C:\Windows\PolicyDefinitions\ja-JP\msedgeupdate.adml |

<br />
<br />

## ポリシーを構成

Windows + R キーで **ファイル名を指定して実行** を開き、gpedit.msc (ポリシー エディター) を起動します。

コンピューターの構成 - 管理用テンプレート - Microsoft Edge の更新 - アプリケーション を開きます。

![ポリシー](/articles/internet-explorer-microsoft-edge/side-by-side/AllowMicrosoftEdgeSideBySideBrowserExperience.png)

**Microsoft Edge でのブラウザーの同時実行エクスペリエンスを許可する** ポリシーを有効にします。
![ポリシー](/articles/internet-explorer-microsoft-edge/side-by-side/configurepolicy.png)

<br />
<br />

## 新しい Edge をインストール

特に気にするポイントはありません。

<br />
<br />

## 新旧ふたつの Edge が起動することを確認

![ふたつの Edge](/articles/internet-explorer-microsoft-edge/side-by-side/sidebyside.png)

<br />
<br />

大変申し訳ありませんが、上記の方法で新旧の Edge を共存させた場合、古い Edge が通常通りに起動できない場合があります。
現在、修正に向けて対応が進められていますが、以下のように古い Edge のアイコンを右クリックし「新しいウィンドウ」から起動することはできますので、こちらで暫定回避をお願いします。

![新しいウィンドウ](/articles/internet-explorer-microsoft-edge/side-by-side/newwindow.png)

<br />
<br />

---

## ポリシーを構成する前に新しい Edge をインストールしてしまったときは？

こちらも [ドキュメント](https://docs.microsoft.com/en-us/deployedge/microsoft-edge-sysupdate-access-old-edge#best-practice-guidance) に説明がありますが、**Microsoft Edge でのブラウザーの同時実行エクスペリエンスを許可する** ポリシーを **有効** に設定してから **もう一度インストーラーを実行 (修復インストール)** することで、古い Edge も起動できるようになります。

![修復](/articles/internet-explorer-microsoft-edge/side-by-side/recover.png)

<br />
<br />

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。

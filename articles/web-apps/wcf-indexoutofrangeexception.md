---
title: WCF のバインディング処理にて競合状態により起きる問題
date: 2020-05-27
tags: 
  - Internet Information Services
  - WCF
---

こんにちは。IIS サポート チームです。

今回は WCF のバインディング処理にて競合状態により起きる問題についてご説明いたします。
1 つのプロセス (IIS でホストしている場合は同一アプリケーションプール) で複数の WCF のバインディングをご利用で、以下の例外とスタックトレースが記録される場合がございます。
これは下記コミュニティより報告があり、調査が進められた結果、弊社製品の問題と判断され、修正が検討されています。(2020/5/27 時点)

IndexOutOfRangeException in MessageSecurityOverHttpElement when creating WCF client from service reference
https://developercommunity.visualstudio.com/content/problem/692599/indexoutofrangeexception-in-messagesecurityoverhtt.html

## 例外/例外メッセージ
System.IndexOutOfRangeException/インデックスが配列の境界外です。


## スタックトレース
```
  場所 System.Collections.ArrayList.ArrayListEnumeratorSimple.MoveNext()
  場所 System.Configuration.PropertyInformationCollection..ctor(ConfigurationElement thisElement)
  場所 System.ServiceModel.Configuration.MessageSecurityOverHttpElement.ApplyConfiguration(MessageSecurityOverHttp security)
  場所 System.ServiceModel.Configuration.NonDualMessageSecurityOverHttpElement.ApplyConfiguration(NonDualMessageSecurityOverHttp security)
  場所 System.ServiceModel.Description.ConfigLoader.LookupBinding(String bindingSectionName, String configurationName, BindingCollectionElement bindingCollectionElement, Binding defaultBinding)
  場所 System.ServiceModel.Description.ConfigLoader.LookupBinding(String bindingSectionName, String configurationName, ContextInformation context)
  場所 System.ServiceModel.Description.ConfigLoader.LoadChannelBehaviors(ServiceEndpoint serviceEndpoint, String configurationName)
  場所 System.ServiceModel.ChannelFactory.ApplyConfiguration(String configurationName, Configuration configuration)
  場所 System.ServiceModel.ChannelFactory.InitializeEndpoint(String configurationName, EndpointAddress address)
  場所 System.ServiceModel.ChannelFactory`1..ctor(String endpointConfigurationName, EndpointAddress remoteAddress)
  場所 System.ServiceModel.ConfigurationEndpointTrait`1.CreateSimplexFactory()
  場所 System.ServiceModel.ClientBase`1.CreateChannelFactoryRef(EndpointTrait`1 endpointTrait)
  場所 System.ServiceModel.ClientBase`1.InitializeChannelFactoryRef()
```

回避策、対処方法としては以下の 2 点ございます。
1. WCF のアプリケーションにおいてクライアントのクラスをインスタンス化を複数同時に発生しないよう排他制御を行っていただく
2. プロセスの再起動を行う (IIS でホストしている場合にはアプリケーションプールのリサイクル) 

ご不便をおかけしており誠に恐れ入りますが、何卒よろしくお願いいたします。



---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。 もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。
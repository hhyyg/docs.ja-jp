---
title: "トランスポート: UDP | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 738705de-ad3e-40e0-b363-90305bddb140
caps.latest.revision: 48
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 48
---
# トランスポート: UDP
UDP トランスポートのサンプルでは、UDP ユニキャストとマルチキャストを [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] カスタム トランスポートとして実装する方法を示します。このサンプルでは、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] でカスタム トランスポートを作成するための推奨手順について説明します。作成時には、チャネル フレームワークと次に示す [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] のベスト プラクティスを使用します。カスタム トランスポートを作成する手順は、次のとおりです。  
  
1.  ChannelFactory と ChannelListener でサポートする、チャネルの[メッセージ交換パターン](#MessageExchangePatterns) \(IOutputChannel、IInputChannel、IDuplexChannel、IRequestChannel、または IReplyChannel\) を決定します。次に、こうしたインターフェイスのセッションフル バリエーションをサポートするかどうかを決定します。  
  
2.  メッセージ交換パターンをサポートするチャネル ファクトリおよびリスナーを作成します。  
  
3.  ネットワーク固有の例外が、<xref:System.ServiceModel.CommunicationException> の適切な派生クラスに標準化されていることを確認します。  
  
4.  チャネル スタックにカスタム トランスポートを追加する [\<binding\>](../../../../docs/framework/misc/binding.md) 要素を追加します。詳細については、「[バインド要素の追加](#AddingABindingElement)」を参照してください。  
  
5.  バインド要素拡張セクションを追加して、新しいバインド要素を構成システムに公開します。  
  
6.  他のエンドポイントに機能を伝達するメタデータ拡張を追加します。  
  
7.  適切に定義されたプロファイルに従って、バインド要素のスタックを事前構成するバインディングを追加します。詳細については、「[標準バインディング要素の追加](#AddingAStandardBinding)」を参照してください。  
  
8.  構成システムにバインディングを開示する、バインディング セクションおよびバインディング構成要素を追加します。詳細については、「[構成サポートの追加](#AddingConfigurationSupport)」を参照してください。  
  
<a name="MessageExchangePatterns"></a>   
## メッセージ交換パターン  
 カスタム トランスポートを記述する最初の手順として、どのメッセージ交換パターン \(MEP\) がトランスポートに必要かを判断します。次の 3 つの MEP から選択できます。  
  
-   データグラム \(IInputChannel\/IOutputChannel\)  
  
     データグラム MEP を使用している場合、クライアントは "ファイア アンド フォーゲット \(撃ち放し\)" の交換を使用してメッセージを送信します。このような交換では、配信の成否について帯域外での確認が必要になります。メッセージが移動中に失われて、サービスに到達しない可能性があります。クライアントで送信操作が正常に完了したとしても、リモート エンドポイントでメッセージが受信されたとは限りません。データグラムはメッセージングの基礎となるビルド ブロックであり、その上に信頼できるプロトコルや安全なプロトコルなどの独自のプロトコルを構築できます。クライアント データグラム チャネルには、<xref:System.ServiceModel.Channels.IOutputChannel> インターフェイスが実装され、サービス データグラム チャネルには <xref:System.ServiceModel.Channels.IInputChannel> インターフェイスが実装されます。  
  
-   要求 \- 応答 \(IRequestChannel\/IReplyChannel\)  
  
     この MEP では、メッセージが送信されて、応答が受信されます。パターンは、要求 \- 応答のペアで構成されます。要求 \- 応答呼び出しの例には、リモート プロシージャ コール \(RPC\) やブラウザ GET などがあります。このパターンは、半二重とも呼ばれます。この MEP では、クライアント チャネルには <xref:System.ServiceModel.Channels.IRequestChannel> が実装され、サービス チャネルには <xref:System.ServiceModel.Channels.IReplyChannel> が実装されます。  
  
-   二重 \(IDuplexChannel\)  
  
     二重 MEP では、クライアントにより任意の数のメッセージを送信して、任意の順序で受信できます。二重 MEP は、話される語の 1 つずつがメッセージである電話の会話に似ています。この MEP ではどちらの側も送信および受信できるので、クライアントおよびサービス チャネルによって実装されるインターフェイスは <xref:System.ServiceModel.Channels.IDuplexChannel> になります。  
  
 これらの MEP では、それぞれセッションをサポートすることもできます。セッション対応チャネルにより追加される機能として、チャネルで送信および受信されるすべてのメッセージが関連付けられます。要求\/応答パターンはスタンドアロンの 2 メッセージ セッションで、要求と応答が相互に関連付けられています。一方、セッションをサポートする要求 \- 応答パターンは、そのチャネルのすべての要求\/応答ペアが互いに関連付けられることを意味しています。これにより、合計で 6 つの MEP \(データグラム、要求 \- 応答、二重、セッション対応データグラム、セッション対応要求 \- 応答、およびセッション対応二重\) を選択できます。  
  
> [!NOTE]
>  UDP トランスポートでは、サポートされている MEP はデータグラムだけです。これは、UDP が "ファイア アンド フォーゲット \(撃ち放し\)" のプロトコルだからです。  
  
### ICommunicationObject および WCF オブジェクトのライフサイクル  
 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] は、通信に使用される <xref:System.ServiceModel.Channels.IChannel>、<xref:System.ServiceModel.Channels.IChannelFactory>、および <xref:System.ServiceModel.Channels.IChannelListener> などのオブジェクトのライフサイクルの管理に使用される共通ステート マシンです。これらの通信オブジェクトには、5 つの状態があります。これらの状態は、<xref:System.ServiceModel.CommunicationState> 列挙値で表され、次のようになります。  
  
-   Created: <xref:System.ServiceModel.ICommunicationObject> が初めてインスタンス化されたときの状態です。この状態では、入出力 \(I\/O\) は行われません。  
  
-   Opening: <xref:System.ServiceModel.ICommunicationObject.Open%2A> が呼び出されると、オブジェクトはこの状態に移行します。この時点では、プロパティは不変であり、入出力を開始できます。この移行は、Created 状態からのみ有効です。  
  
-   Opened: オープン処理が完了すると、オブジェクトはこの状態に移行します。この移行は、Opening 状態からのみ有効です。この時点では、オブジェクトは完全に転送に使用可能です。  
  
-   Closing: 正常なシャットダウンを行うために <xref:System.ServiceModel.ICommunicationObject.Close%2A> が呼び出されると、オブジェクトはこの状態に移行します。この移行は、Opened 状態からのみ有効です。  
  
-   Closed: Closed 状態では、オブジェクトは使用できません。通常、ほとんどの構成には検査中もアクセスできますが、通信が発生することはありません。この状態は、破棄されるのと同じです。  
  
-   Faulted: Faulted 状態では、オブジェクトにアクセスして検査できますが、使用することはできません。回復不可能なエラーが発生した場合、オブジェクトはこの状態に移行します。この状態からは、`Closed` 状態にのみ移行できます。  
  
 各状態移行には、発生するイベントがあります。<xref:System.ServiceModel.ICommunicationObject.Abort%2A> メソッドはいつでも呼び出すことができます。このメソッドを呼び出すことにより、オブジェクトは現在の状態から直ちに Closed 状態に移行します。<xref:System.ServiceModel.ICommunicationObject.Abort%2A> を呼び出すと、完了していない作業が終了します。  
  
<a name="ChannelAndChannelListener"></a>   
## チャネル ファクトリとチャネル リスナー  
 カスタム トランスポートを記述する次の手順では、クライアント チャネルでの <xref:System.ServiceModel.Channels.IChannelFactory> の実装とサービス チャネルでの <xref:System.ServiceModel.Channels.IChannelListener> の実装を作成します。チャネル レイヤーでは、チャネルの構築にファクトリ パターンが使用されます。[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] には、このプロセスに対する基本クラス ヘルパーが用意されています。  
  
-   <xref:System.ServiceModel.Channels.CommunicationObject> クラスには <xref:System.ServiceModel.ICommunicationObject> が実装され、前述の手順 2. で説明したステート マシンが強制実行されます。  
  
-   ``  <xref:System.ServiceModel.Channels.ChannelManagerBase> クラスには <xref:System.ServiceModel.Channels.CommunicationObject> が実装され、<xref:System.ServiceModel.Channels.ChannelFactoryBase> と <xref:System.ServiceModel.Channels.ChannelListenerBase> の統合基本クラスが提供されます。<xref:System.ServiceModel.Channels.ChannelManagerBase> クラスは、<xref:System.ServiceModel.Channels.IChannel> を実装する基本クラスである <xref:System.ServiceModel.Channels.ChannelBase> との組み合わせによって動作します。  
  
-   ``  <xref:System.ServiceModel.Channels.ChannelFactoryBase> クラスでは、<xref:System.ServiceModel.Channels.ChannelManagerBase> および <xref:System.ServiceModel.Channels.IChannelFactory> が実装され、`CreateChannel` オーバーロードが `OnCreateChannel` 抽象メソッドに統合されます。  
  
-   <xref:System.ServiceModel.Channels.ChannelListenerBase> クラスは、<xref:System.ServiceModel.Channels.IChannelListener> を実装しています。基本状態管理を行います。  
  
 このサンプルのファクトリの実装は UdpChannelFactory.cs に、リスナーの実装は UdpChannelListener.cs に含まれています。<xref:System.ServiceModel.Channels.IChannel> 実装は、UdpOutputChannel.cs と UdpInputChannel.cs に含まれています。  
  
### UDP チャネル ファクトリ  
 `UdpChannelFactory` は <xref:System.ServiceModel.Channels.ChannelFactoryBase> から派生します。サンプルでは、<xref:System.ServiceModel.Channels.ChannelFactoryBase.GetProperty%2A> をオーバーライドして、メッセージ エンコーダーのメッセージ バージョンにアクセスできるようにします。さらに、<xref:System.ServiceModel.Channels.ChannelFactoryBase.OnClose%2A> をオーバーライドして、ステート マシンの移行時に <xref:System.ServiceModel.Channels.BufferManager> のインスタンスを破棄できるようにします。  
  
#### UDP 出力チャネル  
 `UdpOutputChannel` では、<xref:System.ServiceModel.Channels.IOutputChannel> が実装されます。このコンストラクターは、引数を検証し、渡される <xref:System.ServiceModel.EndpointAddress> に基づいて出力先の <xref:System.Net.EndPoint> オブジェクトを構築します。  
  
```  
this.socket = new Socket(this.remoteEndPoint.AddressFamily, SocketType.Dgram, ProtocolType.Udp);  
  
```  
  
 チャネルが閉じる際には、正常終了することも異常終了することもあります。チャネルが正常に閉じた場合はソケットも終了し、基本クラスの `OnClose` メソッドが呼び出されます。このときに例外がスローされると、インフラストラクチャによって `Abort` が呼び出され、チャネルがクリーンアップされます。  
  
```  
this.socket.Close(0);  
  
```  
  
 次に、`Send()` と `BeginSend()`\/`EndSend()` を実装します。この実装は、2 つの主要セクションに分かれます。最初に、メッセージを次のようにシリアル化してバイト配列で表します。  
  
```  
ArraySegment<byte> messageBuffer = EncodeMessage(message);  
  
```  
  
 次に、結果として生成されたデータを次のようにネットワークに送信します。  
  
```  
this.socket.SendTo(messageBuffer.Array, messageBuffer.Offset, messageBuffer.Count, SocketFlags.None, this.remoteEndPoint);  
```  
  
### UdpChannelListener  
 サンプルに実装されている ``UdpChannelListener は、<xref:System.ServiceModel.Channels.ChannelListenerBase> クラスから派生します。単一の UDP ソケットを使用して、データグラムを受信します。`OnOpen` メソッドは、非同期ループ内で UDP ソケットを使用してデータを受信します。その後、メッセージ エンコーディング フレームワークを使用して、データを次のようにメッセージに変換します。  
  
```  
message = MessageEncoderFactory.Encoder.ReadMessage(new ArraySegment<byte>(buffer, 0, count), bufferManager);  
```  
  
 複数のソースから到着するメッセージが同じデータグラム チャネルで表されるので、`UdpChannelListener` はシングルトン リスナーです。このリスナーに同時に関連付けられるアクティブな <xref:System.ServiceModel.Channels.IChannel>`` は、最大 1 つです。このサンプルでは、`AcceptChannel` メソッドによって返されるチャネルがその後破棄される場合のみ、もう 1 つ生成されます。メッセージが受信されると、このシングルトン チャネルのキューに置かれます。  
  
#### UdpInputChannel  
 `UdpInputChannel` クラスは、`IInputChannel` を実装しています。このクラスは `UdpChannelListener` のソケットによって設定される受信メッセージのキューで構成されています。これらのメッセージは、`IInputChannel.Receive``` メソッドによってキューから削除されます。  
  
<a name="AddingABindingElement"></a>   
## バインド要素の追加  
 ファクトリおよびチャネルを作成したので、バインディングを使用してそれらを ServiceModel ランタイムに開示する必要があります。バインディングは、サービス アドレスに関連する通信スタックを表すバインディング要素のコレクションです。スタックの各要素は、[\<binding\>](../../../../docs/framework/misc/binding.md) 要素によって表されます。  
  
 このサンプルでは、バインド要素は `UdpTransportBindingElement` で、<xref:System.ServiceModel.Channels.TransportBindingElement> から派生しています。バインディングに関連したファクトリを作成すると、次のメソッドが無効になります。  
  
```  
public IChannelFactory<TChannel> BuildChannelFactory<TChannel>(BindingContext context)  
{  
            return (IChannelFactory<TChannel>)(object)new UdpChannelFactory(this, context);  
}  
  
public IChannelListener<TChannel> BuildChannelListener<TChannel>(BindingContext context)  
{  
            return (IChannelListener<TChannel>)(object)new UdpChannelListener(this, context);  
}  
```  
  
 また、この要素には、`BindingElement` を複製したり、スキーム \(soap.udp\) を返したりするためのメンバーも含まれます。  
  
## トランスポート バインディング要素のメタデータ サポートの追加  
 トランスポートをメタデータ システムに統合するには、ポリシーのインポートとエクスポートの両方をサポートする必要があります。これにより、[ServiceModel メタデータ ユーティリティ ツール \(Svcutil.exe\)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) を使用してバインディングのクライアントを生成できます。  
  
### WSDL サポートの追加  
 バインディングのトランスポート バインド要素は、メタデータのアドレス指定情報のインポートとエクスポートを行います。SOAP バインディングを使用する場合は、トランスポート バインド要素によっても、メタデータの正しいトランスポート URI がエクスポートされます。  
  
#### WSDL エクスポート  
 アドレス指定情報をエクスポートするには、`UdpTransportBindingElement` に `IWsdlExportExtension` インターフェイスを実装します。`ExportEndpoint` メソッドにより、正しいアドレス指定情報が WSDL ポートに追加されます。  
  
```  
if (context.WsdlPort != null)  
{  
    AddAddressToWsdlPort(context.WsdlPort, context.Endpoint.Address, encodingBindingElement.MessageVersion.Addressing);  
}  
```  
  
 エンドポイントが SOAP バインディングを使用する場合、`ExportEndpoint` メソッドの `UdpTransportBindingElement` の実装でも、次のようにトランスポート URI がエクスポートされます。  
  
```  
WsdlNS.SoapBinding soapBinding = GetSoapBinding(context, exporter);  
if (soapBinding != null)  
{  
    soapBinding.Transport = UdpPolicyStrings.UdpNamespace;  
}  
```  
  
#### WSDL インポート  
 WSDL インポート システムを拡張してアドレスのインポートを処理するには、次の構成を Svcutil.exe の構成ファイルに追加する必要があります。Svcutil.exe.config ファイルの次の例を参照してください。  
  
```  
<configuration>  
  <system.serviceModel>  
    <client>  
      <metadata>  
        <wsdlImporters>  
          <extension type=" Microsoft.ServiceModel.Samples.UdpBindingElementImporter, UdpTransport" />  
        </policyImporters>  
      </metadata>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
 Svcutil.exe を実行する場合、Svcutil.exe に WSDL インポートの拡張を読み込ませるために次の 2 つのオプションがあります。  
  
1.  \/SvcutilConfig:\<ファイル名\> を使用して、Svcutil.exe に構成ファイルを指定します。  
  
2.  Svcutil.exe と同じディレクトリにある Svcutil.exe.config に構成セクションを追加します。  
  
 `UdpBindingElementImporter` 型は、`IWsdlImportExtension` インターフェイスを実装します。`ImportEndpoint` メソッドは、次のようにして WSDL ポートからアドレスをインポートします。  
  
```  
BindingElementCollection bindingElements = context.Endpoint.Binding.CreateBindingElements();  
TransportBindingElement transportBindingElement = bindingElements.Find<TransportBindingElement>();  
if (transportBindingElement is UdpTransportBindingElement)  
{  
    ImportAddress(context);  
}  
```  
  
### ポリシー サポートの追加  
 カスタム バインド要素では、WSDL バインディング内のポリシー アサーションをエクスポートして、サービス エンドポイントでそのバインド要素の機能を表現します。  
  
#### ポリシーのエクスポート  
 `UdpTransportBindingElement` 型はポリシーのエクスポートをサポートするために ```IPolicyExportExtension` を実装します。その結果、`System.ServiceModel.MetadataExporter` には、これを含む任意のバインディングでのポリシーの生成時に `UdpTransportBindingElement` が含まれます。  
  
 マルチキャスト モードの場合、`IPolicyExportExtension.ExportPolicy` には UDP のアサーションや他のアサーションが追加されます。これは、マルチキャスト モードは通信スタックの構築方法に影響を与えるため、両方の側において調整される必要があるためです。  
  
```  
ICollection<XmlElement> bindingAssertions = context.GetBindingAssertions();  
XmlDocument xmlDocument = new XmlDocument();  
bindingAssertions.Add(xmlDocument.CreateElement(  
UdpPolicyStrings.Prefix, UdpPolicyStrings.TransportAssertion, UdpPolicyStrings.UdpNamespace));  
if (Multicast)  
{  
    bindingAssertions.Add(xmlDocument.CreateElement(  
UdpPolicyStrings.Prefix, UdpPolicyStrings.MulticastAssertion,     UdpPolicyStrings.UdpNamespace));  
}  
```  
  
 カスタム トランスポート バインド要素はアドレス指定の処理を実行するため、`UdpTransportBindingElement` への `IPolicyExportExtension` の実装でも、WS\-Addressing ポリシー アサーションのエクスポートが処理されて、使用されている WS\-Addressing のバージョンが示されます。  
  
```  
AddWSAddressingAssertion(context, encodingBindingElement.MessageVersion.Addressing);  
```  
  
#### ポリシーのインポート  
 ポリシー インポート システムを拡張するには、次の構成を Svcutil.exe の構成ファイルに追加する必要があります。Svcutil.exe.config ファイルの次の例を参照してください。  
  
```  
<configuration>  
  <system.serviceModel>  
    <client>  
      <metadata>  
        <policyImporters>  
          <extension type=" Microsoft.ServiceModel.Samples.UdpBindingElementImporter, UdpTransport" />  
        </policyImporters>  
      </metadata>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
 次に、登録されたクラス \(`UdpBindingElementImporter`\) から `IPolicyImporterExtension` を実装します。`ImportPolicy()` で、名前空間内のアサーションを調べ、そのアサーションを処理してトランスポートを生成し、マルチキャストであるかどうかをチェックします。さらに、処理したアサーションをバインディング アサーションの一覧から削除する必要もあります。Svcutil.exe を実行する場合、ここでも、統合用に次の 2 つのオプションがあります。  
  
1.  \/SvcutilConfig:\<ファイル名\> を使用して、Svcutil.exe に構成ファイルを指定します。  
  
2.  Svcutil.exe と同じディレクトリにある Svcutil.exe.config に構成セクションを追加します。  
  
<a name="AddingAStandardBinding"></a>   
## 標準バインディング要素の追加  
 バインディング要素は、次の 2 つの方法で使用できます。  
  
-   カスタム バインディングの使用 : カスタム バインディングを使用すれば、バインディング要素の任意のセットに基づいて独自のバインディングを作成できます。  
  
-   バインディング要素が含まれるシステム指定のバインディングを使用することによって、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] は `BasicHttpBinding`、`NetTcpBinding`、`WsHttpBinding` など、これらのシステム定義バインディングの数々を提供します。これらの各バインドは、適切に定義されたプロファイルに関連付けられます。  
  
 このサンプルでは、プロファイル バインディングを、<xref:System.ServiceModel.Channels.Binding> から派生した `SampleProfileUdpBinding` に実装します。`SampleProfileUdpBinding` は、`UdpTransportBindingElement`、`TextMessageEncodingBindingElement CompositeDuplexBindingElement`、および `ReliableSessionBindingElement` の、最大 4 つのバインディング要素を格納します。  
  
```  
public override BindingElementCollection CreateBindingElements()  
{     
    BindingElementCollection bindingElements = new BindingElementCollection();  
    if (ReliableSessionEnabled)  
    {  
        bindingElements.Add(session);  
        bindingElements.Add(compositeDuplex);  
    }  
    bindingElements.Add(encoding);  
    bindingElements.Add(transport);  
    return bindingElements.Clone();  
}  
```  
  
### カスタムの標準バインディング インポーターの追加  
 Svcutil.exe と `WsdlImporter` 型は、既定でシステム定義のバインディングを識別してインポートします。これ以外の場合、バインディングは、`CustomBinding` インスタンスとしてインポートされます。Svcutil.exe と `WsdlImporter` を有効にして `SampleProfileUdpBinding` をインポートする場合、`UdpBindingElementImporter` もカスタムの標準バインディング インポーターとして機能します。  
  
 カスタムの標準バインディング インポーターは、`IWsdlImportExtension` インターフェイスの `ImportEndpoint` メソッドを実装しており、メタデータからインポートされた `CustomBinding` インスタンスを調べて、これが特定の標準バインディングによって生成されたものであるかどうかを判別できます。  
  
```  
if (context.Endpoint.Binding is CustomBinding)  
{  
    Binding binding;  
    if (transportBindingElement is UdpTransportBindingElement)  
    {  
        //if TryCreate is true, the CustomBinding will be replace by a SampleProfileUdpBinding in the  
        //generated config file for better typed generation.  
        if (SampleProfileUdpBinding.TryCreate(bindingElements, out binding))  
        {  
            binding.Name = context.Endpoint.Binding.Name;  
            binding.Namespace = context.Endpoint.Binding.Namespace;  
            context.Endpoint.Binding = binding;  
        }  
    }  
}  
```  
  
 一般に、カスタムの標準バインディング インポーターを実装すると、インポートしたバインド要素のプロパティを調べて、標準バインディングによって設定されたプロパティのみが変更され、その他のすべてのプロパティは既定のままであることが検証されます。標準バインディング インポーターを実装する場合の基本的な方法は、標準バインディングのインスタンスを作成し、標準バインディングがサポートするバインド要素のプロパティを標準バインディング インスタンスに反映し、標準バインディングのバインド要素とインポートしたバインド要素とを比較します。  
  
<a name="AddingConfigurationSupport"></a>   
## 構成サポートの追加  
 構成を通じてトランスポートを公開するには、2 つの構成セクションを実装する必要があります。1 つ目のクラスは、`UdpTransportBindingElement` の `BindingElementExtensionElement` です。これは、`CustomBinding` の実装がバインディング要素を参照するためのものです。2 つ目のクラスは、`SampleProfileUdpBinding` の `Configuration` です。  
  
### バインディング要素の拡張要素  
 セクション `````UdpTransportElement` は、`UdpTransportBindingElement` を構成システムに公開する `BindingElementExtensionElement` です。いくつかの基本的なオーバーライドを行うことで、構成セクションの名前、バインディング要素の種類とバインディング要素の作成方法を定義します。その後、次のコードに示すように、拡張セクションを構成ファイルに登録できます。  
  
```  
<configuration>  
  \<system.serviceModel>  
    <extensions>  
      <bindingElementExtensions>  
      \<add name="udpTransport" type="Microsoft.ServiceModel.Samples.UdpTransportElement, UdpTransport />  
      </bindingElementExtensions>  
    </extensions>  
  \</system.serviceModel>  
</configuration>  
  
```  
  
 この拡張は、UDP をトランスポートとして使用するためにカスタム バインディングから参照されます。  
  
```  
<configuration>  
  \<system.serviceModel>  
    <bindings>  
      <customBinding>  
       <binding configurationName="UdpCustomBinding">  
         <udpTransport/>  
       </binding>  
      </customBinding>  
    </bindings>  
  \</system.serviceModel>  
</configuration>  
  
```  
  
### バインディング セクション  
 セクション `SampleProfileUdpBindingCollectionElement` は `StandardBindingCollectionElement` で、`SampleProfileUdpBinding` を構成システムに公開します。実装の大部分は `SampleProfileUdpBindingConfigurationElement` で代行されます。これは `StandardBindingElement` の派生です。`SampleProfileUdpBindingConfigurationElement` には `SampleProfileUdpBinding` にあるプロパティに対応するプロパティがあり、`ConfigurationElement` バインディングからマップする関数があります。最後に、`SampleProfileUdpBinding` 内で `OnApplyConfiguration` メソッドをオーバーライドします。次のサンプル コードを参照してください。  
  
```  
protected override void OnApplyConfiguration(string configurationName)  
{  
            if (binding == null)  
                throw new ArgumentNullException("binding");  
  
            if (binding.GetType() != typeof(SampleProfileUdpBinding))  
            {  
                throw new ArgumentException(string.Format(CultureInfo.CurrentCulture,  
                    "Invalid type for binding. Expected type: {0}. Type passed in: {1}.",  
                    typeof(SampleProfileUdpBinding).AssemblyQualifiedName,  
                    binding.GetType().AssemblyQualifiedName));  
            }  
            SampleProfileUdpBinding udpBinding = (SampleProfileUdpBinding)binding;  
  
            udpBinding.OrderedSession = this.OrderedSession;  
            udpBinding.ReliableSessionEnabled = this.ReliableSessionEnabled;  
            udpBinding.SessionInactivityTimeout = this.SessionInactivityTimeout;  
            if (this.ClientBaseAddress != null)  
                   udpBinding.ClientBaseAddress = ClientBaseAddress;  
}  
  
```  
  
 このハンドラを構成システムに登録するには、次のセクションを関連する構成ファイルに追加します。  
  
```  
<configuration>  
  <configSections>  
     <sectionGroup name="system.serviceModel">  
         <sectionGroup name="bindings">  
                 \<section name="sampleProfileUdpBinding" type="Microsoft.ServiceModel.Samples.SampleProfileUdpBindingCollectionElement, UdpTransport />  
         </sectionGroup>  
     </sectionGroup>  
  </configSections>  
</configuration>  
```  
  
 これは serviceModel 構成セクションから参照できます。  
  
```  
<configuration>  
  \<system.serviceModel>  
    <client>  
      <endpoint configurationName="calculator"  
                address="soap.udp://localhost:8001/"   
                bindingConfiguration="CalculatorServer"  
                binding="sampleProfileUdpBinding"   
                contract= "Microsoft.ServiceModel.Samples.ICalculatorContract">  
      </endpoint>  
    </client>  
  \</system.serviceModel>  
</configuration>  
```  
  
## UDP テスト サービスとクライアント  
 このサンプルのトランスポートを使用するテスト コードは、UdpTestService ディレクトリと UdpTestClient ディレクトリで使用できます。サービス コードは 2 つのテストで構成されています。1 つ目はコードからバインディングとエンドポイントをセットアップするテストで、2 つ目は構成を使用してバインディングとエンドポイントをセットアップするテストです。両方のテストで、2 つのエンドポイントを使用します。1 つ目のエンドポイントでは、[\<reliableSession\>](http://msdn.microsoft.com/ja-jp/9c93818a-7dfa-43d5-b3a1-1aafccf3a00b) を `true` に設定した `SampleUdpProfileBinding` を使用します。もう 1 つのエンドポイントでは、 `UdpTransportBindingElement` が含まれるカスタム バインディングを使用します。これは、[\<reliableSession\>](http://msdn.microsoft.com/ja-jp/9c93818a-7dfa-43d5-b3a1-1aafccf3a00b) を `false` に設定して `SampleUdpProfileBinding` を使用することと同じです。両方のテストでサービスが作成され、各バインドのエンドポイントが追加されてサービスが開きます。その後 Enter キーを押すと、サービスが閉じます。  
  
 このサービス テスト アプリケーションを開始すると、次の出力が表示されます。  
  
```  
Testing Udp From Code.  
Service is started from code...  
Press <ENTER> to terminate the service and start service from config...  
```  
  
 次に、公開されたエンドポイントに対してクライアント テスト アプリケーションを実行できます。このクライアント テスト アプリケーションでは、各エンドポイントに対してクライアントが作成され、5 つのメッセージが各エンドポイントに送信されます。クライアントでの出力を次に示します。  
  
```  
Testing Udp From Imported Files Generated By SvcUtil.  
0  
3  
6  
9  
12  
Press <ENTER> to complete test.  
```  
  
 サービスでのすべての出力を次に示します。  
  
```  
Service is started from code...  
Press <ENTER> to terminate the service and start service from config...  
Hello, world!  
Hello, world!  
Hello, world!  
Hello, world!  
Hello, world!  
   adding 0 + 0  
   adding 1 + 2  
   adding 2 + 4  
   adding 3 + 6  
   adding 4 + 8  
```  
  
 構成を使用して公開されたエンドポイントに対してクライアント アプリケーションを実行するには、サービス側で Enter キーを押してテスト クライアントを再実行します。サービスには、次の出力が表示されます。  
  
```  
Testing Udp From Config.  
Service is started from config...  
Press <ENTER> to terminate the service and exit...  
```  
  
 クライアントを再実行すると、前と同じ結果が表示されます。  
  
 Svcutil.exe を使用してクライアント コードと構成を再生成するには、サービス アプリケーションを開始し、その後、サンプルのルート ディレクトリで次のように Svcutil.exe を実行します。  
  
```  
svcutil http://localhost:8000/udpsample/ /reference:UdpTranport\bin\UdpTransport.dll /svcutilConfig:svcutil.exe.config  
```  
  
 Svcutil.exe を実行しても `SampleProfileUdpBinding` のバインディング拡張構成は生成されません。したがって、手動で追加する必要があります。  
  
```  
<configuration>  
    \<system.serviceModel>      
        …  
        <extensions>  
            \<!-- This was added manually because svcutil.exe does not add this extension to the file -->  
            <bindingExtensions>  
                <add name="sampleProfileUdpBinding" type="Microsoft.ServiceModel.Samples.SampleProfileUdpBindingCollectionElement, UdpTransport" />  
            </bindingExtensions>  
        </extensions>  
    \</system.serviceModel>  
</configuration>  
```  
  
#### サンプルを設定、ビルド、および実行するには  
  
1.  ソリューションをビルドするには、「[Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
2.  単一コンピューター構成か複数コンピューター構成かに応じて、「[Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。  
  
3.  前の「UDP テスト サービスとクライアント」セクションを参照してください。  
  
> [!IMPORTANT]
>  サンプルは、既にコンピューターにインストールされている場合があります。続行する前に、次の \(既定の\) ディレクトリを確認してください。  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  このディレクトリが存在しない場合は、「[.NET Framework 4 向けの Windows Communication Foundation \(WCF\) および Windows Workflow Foundation \(WF\) のサンプル](http://go.microsoft.com/fwlink/?LinkId=150780)」にアクセスして、[!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] および [!INCLUDE[wf1](../../../../includes/wf1-md.md)] のサンプルをすべてダウンロードしてください。このサンプルは、次のディレクトリに格納されます。  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Transport\Udp`  
  
## 参照
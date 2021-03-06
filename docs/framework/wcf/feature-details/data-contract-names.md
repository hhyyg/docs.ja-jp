---
title: "データ コントラクト名 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
helpviewer_keywords: 
  - "データ コントラクト [WCF], 名前付け"
ms.assetid: 31f87e6c-247b-48f5-8e94-b9e1e33d8d09
caps.latest.revision: 27
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 27
---
# データ コントラクト名
クライアントとサービスが同じ型を共有しないことがあります。このような場合でも、双方のデータ コントラクトが等価であれば、相互にデータを受け渡すことができます。[データ コントラクトの等価性](../../../../docs/framework/wcf/feature-details/data-contract-equivalence.md)は、データ コントラクトとデータ メンバーの名前に基づいているため、型とメンバーを名前にマップするための機構が用意されています。ここでは、データ コントラクトの命名規則に加えて、名前を作成する際の [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] インフラストラクチャの既定の動作についても説明します。  
  
## 基本的な規則  
 データ コントラクトの名前付けに関する基本的な規則は次のとおりです。  
  
-   データ コントラクトの完全修飾名は、名前空間と名前から構成されます。  
  
-   データ メンバーは名前のみを持ち、名前空間はありません。  
  
-   データ コントラクトを処理するときに、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] インフラストラクチャは、データ コントラクトの名前空間と名前およびデータ メンバーの名前について大文字と小文字を区別します。  
  
## データ コントラクト名前空間  
 データ コントラクトの名前空間は、URI \(Uniform Resource Identifier\) の形式を使用します。URI は、絶対 URI でも相対 URI のどちらでもかまいません。既定では、特定の型のデータ コントラクトには、その型の共通言語ランタイム \(CLR: Common Language Runtime\) 名前空間に基づく名前空間が割り当てられます。  
  
 既定では、任意の CLR 名前空間 \(*Clr.Namespace* の形式\) が、名前空間 "http:\/\/schemas.datacontract.org\/2004\/07\/Clr.Namespace" にマップされます。この既定をオーバーライドするには、<xref:System.Runtime.Serialization.ContractNamespaceAttribute> 属性をモジュールまたはアセンブリ全体に適用します。また、型ごとにデータ コントラクト名前空間を制御するには、<xref:System.Runtime.Serialization.DataContractAttribute> の <xref:System.Runtime.Serialization.DataContractAttribute.Namespace%2A> プロパティを設定します。  
  
> [!NOTE]
>  "http:\/\/schemas.microsoft.com\/2003\/10\/Serialization" 名前空間は予約されているため、データ コントラクトの名前空間として使用することはできません。  
  
> [!NOTE]
>  `delegate` 宣言を含むデータ コントラクト型では、既定の名前空間をオーバーライドすることはできません。  
  
## データ コントラクト名  
 ある型のデータ コントラクトの既定の名前は、その型の名前になります。この既定をオーバーライドするには、<xref:System.Runtime.Serialization.DataContractAttribute> の <xref:System.Runtime.Serialization.DataContractAttribute.Name%2A> プロパティを別の名前に設定します。ジェネリック型に関する特別な規則については、このトピックで後述される「ジェネリック型のデータ コントラクト名」で説明します。  
  
## データ メンバー名  
 フィールドまたはプロパティのデータ メンバーの既定の名前は、そのフィールドまたはプロパティの名前になります。この既定をオーバーライドするには、<xref:System.Runtime.Serialization.DataMemberAttribute> の <xref:System.Runtime.Serialization.DataMemberAttribute.Name%2A> プロパティを別の値に設定します。  
  
### 例  
 次の例では、データ コントラクトおよびデータ メンバーの既定の名前付け動作をオーバーライドする方法を示します。  
  
 [!code-csharp[C_DataContractNames#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#1)]
 [!code-vb[C_DataContractNames#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#1)]  
  
## ジェネリック型のデータ コントラクト名  
 ジェネリック型のデータ コントラクト名を決定する場合は、特別な規則があります。これらの規則は、同じジェネリック型の 2 つのクローズ ジェネリックの間でデータ コントラクト名の競合を回避するのに役立ちます。  
  
 既定では、ジェネリック型のデータ コントラクト名は、型の名前の後に文字列 "Of"、ジェネリック パラメーターのデータ コントラクト名、ジェネリック パラメーターのデータ コントラクト名前空間を使用して計算された*ハッシュ*が続きます。ハッシュとは、1 つのデータを一意に識別するための "指紋" として機能する、数学関数の結果です。ジェネリック パラメーターがすべてプリミティブ型の場合は、ハッシュは省略されます。  
  
 たとえば、次の例の型を見てください。  
  
 [!code-csharp[C_DataContractNames#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#2)]
 [!code-vb[C_DataContractNames#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#2)]  
  
 この例では、`Drawing<Square,RegularRedBrush>` 型は "DrawingOfSquareRedBrush5HWGAU6h" というデータ コントラクト名を持ちます。ここで "5HWGAU6h" は "urn:shapes" および "urn:default" 名前空間のハッシュになります。`Drawing<Square,SpecialRedBrush>` 型は "DrawingOfSquareRedBrushjpB5LgQ\_S" というデータ コントラクト名を持ちます。ここで "jpB5LgQ\_S" は "urn:shapes" および "urn:special" 名前空間のハッシュになります。ハッシュを使用しないと 2 つの名前は同一になり、名前の競合が発生することに注意してください。  
  
## ジェネリック型のデータ コントラクト名のカスタマイズ  
 前述のようにジェネリック型用に生成されたデータ コントラクト名を容認できない場合があります。たとえば、名前の競合が起こらないことが前もってわかっているため、ハッシュを削除するとします。この場合、`DataContractAttribute` 属性の <xref:System.Runtime.Serialization.DataContractAttribute.Name%2A> プロパティを使用して、名前を生成する別の方法を指定できます。`Name` プロパティの中かっこ内に数字を指定して、ジェネリック パラメーターのデータ コントラクト名を参照できます \(0 は最初のパラメーターを参照し、1 は 2 番目のパラメーターを参照します。以下同様です\)。中かっこ内にシャープ記号 \(\#\) を指定すると、ハッシュを参照できます。これらの各参照は、複数回使用しても、まったく使用しなくてもかまいません。  
  
 たとえば、前の `Drawing` ジェネリック型は次の例に示すように宣言できます。  
  
 [!code-csharp[c_DataContractNames#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#3)]
 [!code-vb[c_DataContractNames#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#3)]  
  
 この場合、`Drawing<Square,RegularRedBrush>` 型は、"Drawing\_using\_RedBrush\_brush\_and\_Square\_shape" というデータ コントラクト名になります。<xref:System.Runtime.Serialization.DataContractAttribute.Name%2A> プロパティに "{\#}" があるため、名前にはハッシュは含まれません。したがって、この型では名前の競合が発生しやすくなります。たとえば、`Drawing<Square,SpecialRedBrush>` 型は、まったく同じデータ コントラクト名を持ちます。  
  
## 参照  
 <xref:System.Runtime.Serialization.DataContractAttribute>   
 <xref:System.Runtime.Serialization.DataMemberAttribute>   
 <xref:System.Runtime.Serialization.ContractNamespaceAttribute>   
 [データ コントラクトの使用](../../../../docs/framework/wcf/feature-details/using-data-contracts.md)   
 [データ コントラクトの等価性](../../../../docs/framework/wcf/feature-details/data-contract-equivalence.md)   
 [Data Contract Names](../../../../docs/framework/wcf/feature-details/data-contract-names.md)   
 [データ コントラクトのバージョン管理](../../../../docs/framework/wcf/feature-details/data-contract-versioning.md)
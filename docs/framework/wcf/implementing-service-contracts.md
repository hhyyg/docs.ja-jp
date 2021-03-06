---
title: "サービス コントラクトの実装 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
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
  - "サービス コントラクトの実装 [WCF]"
ms.assetid: aefb6f56-47e3-4f24-ab0a-9bc07bf9885f
caps.latest.revision: 17
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 17
---
# サービス コントラクトの実装
サービスは、1 つ以上のエンドポイントでクライアントが使用できる機能を公開するクラスです。サービスを作成するには、[!INCLUDE[indigo1](../../../includes/indigo1-md.md)] コントラクトを実装するクラスを記述します。2 つの方法のいずれかでこれを行うことができます。コントラクトを個別にインターフェイスとして定義し、そのインターフェイスを実装するクラスを作成できます。または、クラスに <xref:System.ServiceModel.ServiceContractAttribute> 属性を配置し、サービスのクライアントが使用できるメソッドに <xref:System.ServiceModel.OperationContractAttribute> 属性を配置することによって、クラスとコントラクトを直接作成することもできます。  
  
## サービス クラスの作成  
 個別に定義された `IMath` コントラクトを実装するサービスの例を次に示します。  
  
```csharp  
// Define the IMath contract.  
[ServiceContract]  
public interface IMath  
{  
    [OperationContract]   
    double Add(double A, double B);  
  
    [OperationContract]  
    double Multiply (double A, double B);  
}  
  
// Implement the IMath contract in the MathService class.  
public class MathService : IMath  
{  
    public double Add (double A, double B) { return A + B; }  
    public double Multiply (double A, double B) { return A * B; }  
}  
```  
  
 また、サービスは直接コントラクトを公開できます。`MathService` コントラクトを定義し、実装するサービス クラスの例を次に示します。  
  
```csharp  
// Define the MathService contract directly on the service class.  
[ServiceContract]  
class MathService  
{  
    [OperationContract]  
    public double Add(double A, double B) { return A + B; }  
    [OperationContract]  
    private double Multiply (double A, double B) { return A * B; }  
}  
```  
  
 コントラクト名が異なるので、上記の 2 つのサービスが公開するコントラクトはそれぞれ異なります。最初の例では、公開されるコントラクトの名前が "`IMath`" であるのに対し、2 番目の例では、コントラクトの名前は "`MathService`" です。  
  
 同時実行やインスタンス化など、いくつかの項目は、サービス実装レベルと操作実装レベルで設定できます。[!INCLUDE[crdefault](../../../includes/crdefault-md.md)][サービスの設計と実装](../../../docs/framework/wcf/designing-and-implementing-services.md).  
  
 サービス コントラクトを実装したら、そのサービスに 1 つ以上のエンドポイントを作成する必要があります。[!INCLUDE[crdefault](../../../includes/crdefault-md.md)][エンドポイントの作成の概要](../../../docs/framework/wcf/endpoint-creation-overview.md).サービスの実行方法[!INCLUDE[crabout](../../../includes/crabout-md.md)]、「[ホスティング サービス](../../../docs/framework/wcf/hosting-services.md)」を参照してください。  
  
## 参照  
 [サービスの設計と実装](../../../docs/framework/wcf/designing-and-implementing-services.md)   
 [方法 : コントラクト クラスを使用してサービスを作成する](../../../docs/framework/wcf/feature-details/how-to-create-a-wcf-contract-with-a-class.md)   
 [方法 : コントラクト インターフェイスを使用してサービスを作成する](../../../docs/framework/wcf/feature-details/how-to-create-a-service-with-a-contract-interface.md)   
 [サービスのランタイム動作の指定](../../../docs/framework/wcf/specifying-service-run-time-behavior.md)
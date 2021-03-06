---
title: "軽減策: プールのロック期間"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-bcl
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: 92d2de20-79be-4df1-b182-144143a8866a
caps.latest.revision: 4
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: caaaafaff44f2a679b16fe3f1aae59bcf717388e
ms.contentlocale: ja-jp
ms.lasthandoff: 07/28/2017

---
# <a name="mitigation-pool-blocking-period"></a>軽減策: プールのロック期間
Azure SQL データベースへの接続に関して、接続プールのブロック期間が削除されました。  
  
## <a name="additional-description"></a>その他の説明  
 [!INCLUDE[net_v461](../../../includes/net-v461-md.md)] および以前のバージョンでは、データベースへの接続時にアプリで一時的な接続エラーが発生した場合、接続はすぐに再試行されません。接続プールがエラーをキャッシュし、 5 秒 ～ 1 分の間にエラーを再スローするためです。詳細については、「[SQL Server の接続プール (ADO.NET)](../../../docs/framework/data/adonet/sql-server-connection-pooling.md)」を参照してください。 この動作は、Azure SQL データベースへの接続時に問題となります。多くの場合、一時的なエラーが発生し、通常数秒内に回復します。 接続プールのブロック機能を使用すると、データベースが使用可能な場合でも、長い期間にわたってアプリがデータベースに接続できなくなるということです。 この動作が特に問題となるのは、Azure SQL データベースに接続し、数秒内でレンダリングする必要がある Web アプリの場合です。  
  
 [!INCLUDE[net_v462](../../../includes/net-v462-md.md)] 以降では、既知の Azure SQL データベースへの接続確立要求の場合 (*.database.windows.net、\*.database.chinacloudapi.cn, \*.database.usgovcloudapi.net、\*.database.cloudapi.de)、接続確立のエラーはキャッシュされません。 他のすべての接続を試みる場合は、接続プールのブロック期間が引き続き適用されます。  
  
## <a name="impact"></a>影響  
 この変更により、Azure SQL データベースへの接続確立がすぐに再試行するされるため、クラウド対応アプリケーションのパフォーマンスが向上します。  
  
## <a name="mitigation"></a>軽減策  
 この変更から悪影響を受けるアプリの場合、新しい <xref:System.Data.SqlClient.SqlConnectionStringBuilder.PoolBlockingPeriod%2A> プロパティを設定することで、接続プールのブロック期間を構成できます。  プロパティ値が <xref:System.Data.SqlClient.PoolBlockingPeriod?displayProperty=fullName> 列挙型のメンバーである場合、次の 3 つの値のいずれかを使用できます。  
  
-   `PoolBlockingPeriod.AlwaysBlock` 
  
-   `PoolBlockingPeriod.Auto`  
  
-   `PoolBlockingPeriod.NeverBlock` 
  
 <xref:System.Data.SqlClient.SqlConnectionStringBuilder.PoolBlockingPeriod%2A> プロパティを `PoolBlockingPeriod.AlwaysBlock` に設定して、以前の動作を復元することができます。  
  
## <a name="see-also"></a>関連項目  
 [ランタイムの変更点](../../../docs/framework/migration-guide/runtime-changes-in-the-net-framework-4-6-2.md)


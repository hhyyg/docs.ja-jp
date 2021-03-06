---
title: "確認 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 8637aeaf-ac9e-49b8-93f4-da15dee45277
caps.latest.revision: 7
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 7
---
# 確認
このサンプルでは、<xref:System.Activities.Statements.CompensableActivity> および確認を使用する 4 つの一般的なシナリオを示します。確認を示すために、4 つのワークフローを実行します。このサンプルには、宣言型バージョンと命令型バージョンが用意されています。  
  
## 補正可能なアクティビティの確認  
 最初のワークフローでは、補正可能なアクティビティを確認する方法を示します。このワークフローは、2 つの <xref:System.Activities.Statements.CompensableActivity> インスタンスのシーケンスで構成されています。2 つ目の <xref:System.Activities.Statements.CompensableActivity> の完了後、確認アクティビティによって 1 つ目のアクティビティが確認されます。ワークフローが正常に完了すると、確認または補正されていないすべての <xref:System.Activities.Statements.CompensableActivity> インスタンスが既定の順序で自動的に確認されます。確認アクティビティを使用しなかった場合、2 つ目の <xref:System.Activities.Statements.CompensableActivity> が最初に確認されていたことになります。  
  
## 明示的な補正  
 2 番目のシナリオでは、<xref:System.Activities.Statements.CompensableActivity> が明示的に補正される場合の既定の確認に対する影響を示します。このワークフローは、2 つの <xref:System.Activities.Statements.CompensableActivity> アクティビティのシーケンスで構成されています。2 つ目のアクティビティが完了すると、1 つ目のアクティビティが明示的に補正されます。したがって、ワークフローが完了すると、2 つ目の <xref:System.Activities.Statements.CompensableActivity> のみが確認されます。  
  
## 入れ子になった CompensableActivity アクティビティの確認順序の制御  
 3 番目のシナリオでは、入れ子になった <xref:System.Activities.Statements.CompensableActivity> の確認順序を制御する方法を示します。このシナリオは、ワークフローのルートとしての <xref:System.Activities.Statements.CompensableActivity> で構成されています。ルート <xref:System.Activities.Statements.CompensableActivity> の本体は 3 つの <xref:System.Activities.Statements.CompensableActivity> のシーケンスです。ルート <xref:System.Activities.Statements.CompensableActivity> の <xref:System.Activities.Statements.CompensableActivity.ConfirmationHandler%2A> は、1 つ目および 2 つ目の入れ子になった <xref:System.Activities.Statements.CompensableActivity> をこの順序で確認するように指定するシーケンスです。ワークフローが完了すると、ルート <xref:System.Activities.Statements.CompensableActivity> が確認され、その後、1 つ目、2 つ目、および 3 つ目の入れ子になった <xref:System.Activities.Statements.CompensableActivity> がこの順序で確認されます。したがって、既定の確認順序は基本的に逆になります。3 つ目の <xref:System.Activities.Statements.CompensableActivity> はルート <xref:System.Activities.Statements.CompensableActivity> の一部として <xref:System.Activities.Statements.CompensableActivity.ConfirmationHandler%2A> によって明示的に確認されなかったため、既定の順序に従って確認されます。ただし、このアクティビティが唯一未確認の <xref:System.Activities.Statements.CompensableActivity> であるので、このアクティビティが確認されるだけです。  
  
## 変数のスコープ  
 4 番目のシナリオでは、アクティビティが完了した場合またはワークフローが完了した場合でも、<xref:System.Activities.Statements.CompensableActivity.CompensationHandler%2A>、<xref:System.Activities.Statements.CompensableActivity.ConfirmationHandler%2A>、または<xref:System.Activities.Statements.CompensableActivity.CancellationHandler%2A> の各ハンドラーが実行されるときに、<xref:System.Activities.Statements.CompensableActivity> またはその親のいずれかに対して定義された変数が有効期間中にスコープ外にならないようにする方法を示します。このワークフローは、2 つの <xref:System.Activities.Statements.CompensableActivity> のシーケンスで構成されています。1 つ目のアクティビティの本体では、変数 `mySum` は、5 と 10 の合計、および印刷される結果に設定されます。2 つ目の <xref:System.Activities.Statements.CompensableActivity> の本体では、合計は、既存の合計と 7 の合計、および印刷される結果に設定されます。1 つ目の <xref:System.Activities.Statements.CompensableActivity> に対しては、カスタムの確認ハンドラーが定義されています。このアクティビティは、合計を印刷し、7 を差し引いて、合計を再度印刷します。印刷された合計を調べることで、両方の <xref:System.Activities.Statements.CompensableActivity> の本体だけでなく <xref:System.Activities.Statements.CompensableActivity.ConfirmationHandler%2A> についても変数がスコープ内にあることがわかります。  
  
#### サンプルを実行するには  
  
1.  [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)] でソリューションを読み込みます。  
  
2.  ConfirmationSample.exe アプリケーションを実行します。  
  
3.  次の出力を確認します。  
  
 **明示的な確認: workflowCompensableActivity1 の開始: BodyCompensableActivity2: BodyCompensableActivity1: Confirmation HandlerEnd \(workflowCompensableActivity2\): Confirmation HandlerExplicit の補正: workflowCompensableActivity1 の開始: BodyCompensableActivity2: BodyCompensableActivity1: Compensation HandlerEnd \(workflowCompensableActivity2\): Confirmation HandlerCustom 確認ハンドラー: workflowCompensableActivity1 の開始: BodyCompensableActivity2: BodyCompensableActivity3: BodyEnd \(workflowCompensableActivity1\): Confirmation HandlerCompensableActivity2: Confirmation HandlerCompensableActivity3: Confirmation HandlerVariable のアクセス \(確認ハンドラー内\): workflowCompensableActivity1 の開始: BodyCompensableActivity1: 合計: 15 CompensableActivity2: BodyCompensableActivity2: 7 を sumCompensableActivity2 に加算: 現在の合計: 22 workflowCompensableActivity2 の終了: Confirmation HandlerCompensableActivity1: Confirmation HandlerCompensableActivity2: 合計: 22 CompensableActivity2: 12 を引いた後の現在の合計: 10 終了するには、Enter キーを押します。**  
  
> [!IMPORTANT]
>  サンプルは、既にコンピューターにインストールされている場合があります。続行する前に、次の \(既定の\) ディレクトリを確認してください。  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  このディレクトリが存在しない場合は、「[.NET Framework 4 向けの Windows Communication Foundation \(WCF\) および Windows Workflow Foundation \(WF\) のサンプル](http://go.microsoft.com/fwlink/?LinkId=150780)」にアクセスして、[!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] および [!INCLUDE[wf1](../../../../includes/wf1-md.md)] のサンプルをすべてダウンロードしてください。このサンプルは、次のディレクトリに格納されます。  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Compensation\Confirmation`  
  
## 参照
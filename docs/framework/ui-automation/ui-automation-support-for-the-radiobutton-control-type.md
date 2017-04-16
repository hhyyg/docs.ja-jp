---
title: "UI Automation Support for the RadioButton Control Type | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-bcl"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "control types, Radio Button"
  - "UI Automation, Radio Button control type"
  - "RadioButton control type"
ms.assetid: 87170464-7857-41f1-bcf7-bb41be31cb53
caps.latest.revision: 21
author: "Xansky"
ms.author: "mhopkins"
manager: "markl"
caps.handback.revision: 21
---
# UI Automation Support for the RadioButton Control Type
> [!NOTE]
>  このドキュメントは、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 名前空間で定義されているマネージ <xref:System.Windows.Automation> クラスを使用する .NET Framework 開発者を対象としています。[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] の最新情報については、「[Windows Automation API: UI オートメーション](http://go.microsoft.com/fwlink/?LinkID=156746)」をご覧ください。  
  
 このトピックでは、RadioButton コントロール型の [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] サポートについて説明します。[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] でのコントロール型とは、コントロールが <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> プロパティを使用するために満たす必要がある一連の条件のことです。 これらの条件には、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリー構造、[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] のプロパティ値、およびコントロール パターンに関する特定のガイドラインが含まれます。  
  
 オプション ボタンは、丸形のボタンとアプリケーションで定義されたテキスト \(ラベル\)、アイコン、またはユーザーがボタンを選択することで選択できる選択肢を示すビットマップで構成されます。 多くのアプリケーションでは、関連しているものの相互に排他的なオプションのセットからユーザーが選択できるようにするグループ ボックス内でオプション ボタンが使用されます。 たとえば、ユーザーがクライアント領域で選択したテキストの書式設定を選択できるオプション ボタンのグループをアプリケーションで表示することができます。 ユーザーは、対応するオプション ボタンを選択することによって、左揃え、右揃え、または中央揃えの形式を選択できます。 通常、ユーザーは、オプション ボタンのセットから、一度に 1 つのオプションしか選択することができません。  
  
 以降のセクションでは、RadioButton コントロール型に必要な [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリー構造、プロパティ、コントロール パターン、およびイベントを定義します。[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] の要件は、[!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)]、[!INCLUDE[TLA#tla_win32](../../../includes/tlasharptla-win32-md.md)]、または [!INCLUDE[TLA#tla_winforms](../../../includes/tlasharptla-winforms-md.md)] であるかどうかに関係なく、すべてのリスト コントロールに適用されます。  
  
<a name="Required_UI_Automation_Tree_Structure"></a>   
## 必須の UI オートメーション ツリー構造  
 次の表に、オプション ボタン コントロールに関連する [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーのコントロール ビューとコンテンツ ビューを示し、それぞれのビューに含めることができる内容について説明します。[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーの詳細については、「[UI Automation Tree Overview](../../../docs/framework/ui-automation/ui-automation-tree-overview.md)」を参照してください。  
  
|コントロール ビュー|コンテンツ ビュー|  
|----------------|---------------|  
|RadioButton|RadioButton|  
  
 コントロール ビューまたはコンテンツ ビューに子は存在しません。  
  
<a name="Required_UI_Automation_Properties"></a>   
## 必須の UI オートメーション プロパティ  
 次の表に、RadioButton コントロール型に特に関連する値または定義を持つ [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロパティを列挙します。[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロパティの詳細については、「[UI Automation Properties for Clients](../../../docs/framework/ui-automation/ui-automation-properties-for-clients.md)」を参照してください。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] プロパティ|値|ノート|  
|---------------------------------------------------------------------------------|-------|---------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|ノートを参照してください。|このプロパティの値は、アプリケーションのすべてのコントロールで一意である必要があります。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|ノートを参照してください。|コントロール全体を格納する最も外側の四角形。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|ノートを参照してください。|コントロールがキーボード フォーカスを受け取ることができる場合は、このプロパティをサポートする必要があります。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|ノートを参照してください。|オプション ボタン コントロールの名前は、選択状態を保持するボタンの横に表示されたテキストです。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|ノートを参照してください。|オプション ボタン コントロールのクリック可能なポイントは、マウス ポインターでクリックされた場合にオプション ボタン上に選択が設定されるポイントにする必要があります。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|`Null`|オプション ボタンはそれ自体がラベルであるコントロールです。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|RadioButton|この値は、すべての [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] フレームワークで同じです。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|"オプション ボタン"|RadioButton コントロール型に対応する、ローカライズされた文字列。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|オプション ボタン コントロールは、常に [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーのコンテンツ ビューに組み込まれます。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|オプション ボタン コントロールは、常に [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ツリーのコントロール ビューに組み込まれます。|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>   
## 必須の UI オートメーション コントロール パターン  
 次の表に、すべてのオプション ボタン コントロールでサポートされなければならない [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] コントロール パターンを列挙します。 コントロール パターンの詳細については、「[UI Automation Control Patterns Overview](../../../docs/framework/ui-automation/ui-automation-control-patterns-overview.md)」を参照してください。  
  
|コントロール パターン\/コントロール パターン プロパティ|サポート\/値|ノート|  
|------------------------------------|-------------|---------|  
|<xref:System.Windows.Automation.Provider.ISelectionItemProvider>|はい|すべてのオプション ボタン コントロールが、それ自体を選択可能にする選択項目パターンをサポートする必要があります。|  
|<xref:System.Windows.Automation.Provider.ISelectionItemProvider.SelectionContainer%2A>|ノートを参照してください。|UI オートメーション クライアントが特定のコンテキスト内で相互に関連する他のオプション ボタンを特定できるためには、`SelectionContainerProperty` が設定されている必要があります。  オプション ボタンの [!INCLUDE[TLA#tla_win32](../../../includes/tlasharptla-win32-md.md)] バージョンでは、このプロパティがサポートされません。これは、従来のフレームワークからこの情報を取得できないためです。|  
|<xref:System.Windows.Automation.Provider.IToggleProvider>|Never|オプション ボタンは、いったん設定されると、その状態を循環させることができません。  このパターンは、オプション ボタンではサポートしないでください。|  
  
<a name="Required_UI_Automation_Events"></a>   
## 必須の UI オートメーション イベント  
 次の表に、すべてのオプション ボタン コントロールでサポートされなければならない [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] イベントを列挙します。 イベントの詳細については、「[UI Automation Events Overview](../../../docs/framework/ui-automation/ui-automation-events-overview.md)」を参照してください。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] イベント|Support|ノート|  
|--------------------------------------------------------------------------------|-------------|---------|  
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementRemovedFromSelectionEvent>|必要|なし|  
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementSelectedEvent>|必要|なし|  
|<xref:System.Windows.Automation.TogglePatternIdentifiers.ToggleStateProperty> プロパティ変更イベント。|Never|なし|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty> プロパティ変更イベント。|必須|なし|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty> プロパティ変更イベント。|必須|なし|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty> プロパティ変更イベント。|必須|なし|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|必要|なし|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|必要|なし|  
  
## 参照  
 <xref:System.Windows.Automation.ControlType.RadioButton>   
 [UI Automation Control Types Overview](../../../docs/framework/ui-automation/ui-automation-control-types-overview.md)   
 [UI Automation Overview](../../../docs/framework/ui-automation/ui-automation-overview.md)
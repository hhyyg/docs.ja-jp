---
title: "構成エディター ツール (SvcConfigEditor.exe) | Microsoft Docs"
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
  - "構成ファイル"
  - "構成ファイル スキーマ"
  - "構成ファイル"
  - "構成ファイル, 作成"
ms.assetid: 2db21a57-5f64-426f-89df-fb0dc2d2def5
caps.latest.revision: 45
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 45
---
# 構成エディター ツール (SvcConfigEditor.exe)
管理者と開発者は、[!INCLUDE[indigo1](../../../includes/indigo1-md.md)] サービス構成エディター \(SvcConfigEditor.exe\) のグラフィカル ユーザー インターフェイスを使用して、[!INCLUDE[indigo2](../../../includes/indigo2-md.md)] サービスの構成設定を作成および変更できます。このツールを使用すると、XML 構成ファイルを直接編集せずに、[!INCLUDE[indigo2](../../../includes/indigo2-md.md)] のバインディング、動作、サービス、および診断の設定を管理できます。  
  
 サービス構成エディターは、C:\\Program Files\\Microsoft SDKs\\Windows\\v6.0\\Bin フォルダーにあります。  
  
## WCF 構成エディター  
 サービス構成エディターには、[!INCLUDE[indigo2](../../../includes/indigo2-md.md)] のサービスやクライアントを構成する手順をすべてガイドするウィザードが付属しています。エディターで直接編集する代わりにウィザードを使用することを強くお勧めします。  
  
 標準の System.Configuration スキーマに準拠する構成ファイルが既にいくつかある場合は、ユーザー インターフェイスを使用してバインディング、動作、サービス、および診断の固有の設定を管理できます。サービス構成エディターを使用すると、既存の [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] 構成ファイルの設定だけでなく、実行可能ファイル、COM\+ サービス、および Web ホスト サービスの設定を管理できます。サービス構成エディターで Web ホスト サービスを開くと、上位レベル ノードのサービス固有の構成セクションおよび継承された構成セクションの両方が表示されます。  
  
 [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] 構成設定は、構成ファイルの `<system.serviceModel>` セクションにあるため、エディターはこの要素の内容だけを操作し、同じファイル内の他の要素にはアクセスしません。既存の構成ファイルに直接移動したり、サービス、仮想ディレクトリ、または COM\+ サービスを含むアセンブリを選択することができます。エディターがその特定のサービスの構成ファイルを読み込むと、ユーザーは新しい要素を追加したり、構成ファイルの `<system.serviceModel>` セクションで入れ子になっている既存の要素を編集することができます。  
  
 エディターは、IntelliSense をサポートし、スキーマ準拠を強制します。結果として得られる出力は、構成ファイルのスキーマに準拠し、構文的に正しいデータ値を持つことが保証されます。ただし、エディターは構成ファイルのセマンティクスが有効であることは保証しません。つまり、エディターは構成ファイルが関連するサービスで有効に機能することは保証しません。  
  
> [!CAUTION]
>  要素を変更すると、エディターは構成ファイルからその構成要素を削除できません。たとえば、エディターを使用してエンドポイント名を空でない文字列に設定して保存すると、構成ファイルは次の内容になります。  
>   
>  `<endpoint binding="basicHttpBinding" name="somename" />`  
>   
>  そのエンドポイント名を削除するために名前を空の文字列に設定して保存しようとしても、構成ファイルには、`name` 属性が残ります。  
>   
>  `<endpoint binding="basicHttpBinding" name="" />`  
>   
>  属性を削除するには、別のテキスト エディターを使用して要素を手動で編集する必要があります。  
>   
>  特に、`clientCredential` エンドポイント動作の `issueToken` 要素を使用する場合は、この問題に注意する必要があります。具体的には、`localIssuer` サブ要素の `address` 属性を空の文字列にしないようにします。構成エディターを使用して既に `address` 属性を変更している場合にその属性を完全に削除したい場合は、構成エディター以外のツールを使用して削除する必要があります。そうでない場合、属性は空の文字列を含むことになるため、アプリケーションは例外をスローします。  
  
## 構成エディターの使用  
 サービス構成エディターは、次の Windows SDK のインストール場所にあります。  
  
 C:\\Program Files\\Microsoft SDKs\\Windows\\v6.0\\Bin\\SvcConfigEditor.exe  
  
 サービス構成エディターを起動したら、**\[ファイル\]** メニューの **\[開く\]** を使用して、管理するサービスまたはアセンブリを参照できます。構成ファイルを直接開き、[!INCLUDE[indigo2](../../../includes/indigo2-md.md)] \/COM\+ サービスを参照し、Web ホスト サービスの構成設定を開くことができます。  
  
 サービス構成エディターのユーザー インターフェイスは、次の領域に分割されています。  
  
-   ツリー ビュー ペイン: 左側に構成要素をツリー構造で表示します。ノードを右クリックと、ツリーで操作を実行できます。  
  
-   タスク ペイン: ウィンドウの左下に現在の要素の一般的なタスクを表示します。  
  
-   詳細ペイン: ツリー ビューで選択された構成ノードの詳細設定を右側に表示します。  
  
### 構成ファイルを開く  
  
1.  サービス構成エディターを起動するには、コマンド ウィンドウで [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] のインストール場所に移動し、「`SvcConfigEditor.exe`」と入力します。  
  
2.  **\[ファイル\]** メニューの **\[開く\]** を選択し、管理するファイルの種類をクリックします。  
  
3.  **\[開く\]** ダイアログ ボックスで、管理する特定のファイルに移動し、ダブルクリックします。  
  
 ビューアーは、構成のマージ パスを自動的にたどり、マージされた構成のビューを生成します。たとえば、ホストなしのサービスの実際の構成は、Machine.config と App.config の組み合わせです。変更があれば、SvcConfigEditor のアクティブ ファイルに適用されます。構成マージ パスの特定ファイルを編集する場合は、直接開く必要があります。  
  
> [!NOTE]
>  構成ファイルが構成エディター以外で変更されている場合、構成エディターは、現在開いている構成ファイルを再読み込みします。再読み込みが発生すると、エディター内で永続的に保存されていないすべての変更が失われます。再読み込みが連続して発生する場合は、構成ファイルに定期的にアクセスするサービス \(バックグラウンドで実行するウイルス対策ソフトウェアなど\) が原因と考えられます。この問題を解決するには、ファイルが開いているときに、構成エディターがそのファイルにアクセスできる唯一のプロセスであることを保証する必要があります。  
  
### サービス  
 **\[サービス\]** ノードは、構成ファイルで現在割り当てられているすべてのサービスを表示します。ツリー内の各サブノードは、構成ファイル内の \<`services`\> 要素のサブ要素に対応します。  
  
 **\[サービス\]** ノードをクリックして、**詳細**ペインのサービスの概要ページでタスクを表示または実行できます。  
  
#### 新しいサービス構成の作成  
 新しいサービス構成は次の方法で作成できます。  
  
-   ウィザードの使用: ウィザードを起動するには、タスク ペインまたは概要ページの **\[新しいサービスの作成\]** リンクをクリックします。**\[ファイル\]** メニューの **\[新しい項目の追加\]** を選択することでも起動できます。  
  
-   手動による作成 : **\[サービス\]** ノードを右クリックし、**\[新しいサービス\]** を選択します。  
  
#### 新しいサービス エンドポイント構成の作成  
 新しいサービス エンドポイント構成は次の方法で作成できます。  
  
-   ウィザードによる作成: ウィザードを起動するには、タスク ペインまたは概要ページの **\[新しいサービス エンドポイントの作成\]** リンクをクリックします。**\[ファイル\]** メニューの **\[新しい項目の追加\]** を選択することでも起動できます。  
  
-   手動による作成 : サービスを作成したら、**\[エンドポイント\]** ノードを右クリックし、**\[新しいサービス エンドポイント\]** を選択します。  
  
#### サービス構成の編集  
  
1.  **\[サービス\]** ノードをクリックします。  
  
2.  プロパティ グリッドで設定を編集します。  
  
#### サービス エンドポイント構成の編集  
  
1.  **\[サービス エンドポイント\]** ノードをクリックします。  
  
2.  プロパティ グリッドで設定を編集します。  
  
#### ベース アドレスの追加  
  
1.  **\[ホスト\]** ノードをクリックします。  
  
2.  **\[ベース アドレス\]** セクションの **\[新規作成\]** ボタンをクリックします。  
  
3.  ダイアログ ボックスにベース アドレスの URI を入力します。  
  
4.  **\[OK\]** をクリックします。  
  
> [!NOTE]
>  このツール内では、[\<baseAddressPrefixFilters\>](../../../docs/framework/configure-apps/file-schema/wcf/baseaddressprefixfilters.md) の値を編集できません。この要素を追加または変更するには、テキスト エディターまたは [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] を使用する必要があります。  
  
### クライアント  
 **\[クライアント\]** ノードは、構成ファイルのすべてのクライアント エンドポイントを表示します。ツリー内のすべてのサブノードは、構成ファイル内の \<`client`\> 要素のサブ要素に対応します。  
  
 **\[クライアント\]** ノードをクリックして、**\[詳細ペイン\]** のクライアントの**概要ページ**でタスクを表示または実行できます。  
  
#### 新しいクライアント エンドポイント構成の作成  
 新しいクライアント エンドポイント構成は次の方法で作成できます。  
  
-   ウィザードによる作成: ウィザードを起動するには、ウィンドウの左下のタスク ペインまたは概要ページの **\[新しいクライアントの作成\]** リンクをクリックします。**\[ファイル\]** メニューの **\[新しい項目の追加\]** をクリックして起動することもできます。クライアント構成を生成するサービス構成の場所の指定を求めるプロンプトが表示されます。接続するサービス エンドポイントを選択できるようになります。  
  
-   手動による作成: クライアントの下のエンドポイント ノードを右クリックし、**\[新しいクライアント エンドポイント\]** をクリックします。  
  
#### クライアント エンドポイント構成の編集  
  
1.  クライアント エンドポイント ノードをクリックします。  
  
2.  プロパティ グリッドで設定を編集します。  
  
### 標準エンドポイント  
 標準エンドポイントは、既定値に設定されたアドレス、コントラクト、およびバインディングの 1 つ以上の特性を持つ特殊なエンドポイントです。  
  
 これらの構成設定は、**\[標準エンドポイント\]** ノードに格納されます。**\[標準エンドポイント\]** ノードは、構成ファイルのすべての標準エンドポイント設定を表示します。ツリー内のすべてのサブノードは、構成ファイル内の `<standardEndpoints>` 要素のサブ要素に対応します。  
  
 **\[標準エンドポイント\]** ノードをクリックして、**詳細ペイン**の標準エンドポイントの概要ページで、タスクを表示または実行できます。  
  
#### 新しい標準エンドポイント構成の作成  
 新しい標準エンドポイント構成は、次の方法で作成できます。  
  
-   **\[標準エンドポイント\]** ノードを右クリックし、**\[新しい標準エンドポイント構成\]** を選択します。ダイアログ ボックスでバインディングの種類を選択し、**\[OK\]** をクリックします。  
  
-   **\[標準エンドポイント\]** ノードを選択し、ウィンドウの左下にあるタスク ペインの **\[新しい標準エンドポイント構成\]** をクリックします。  
  
 **\[新しい標準エンドポイントの作成\]** ダイアログ ボックスが表示され、登録されているすべての標準エンドポイントの種類が一覧表示されます。  
  
#### 標準エンドポイント構成の表示および編集  
 表示および編集する標準エンドポイント構成は、次の方法で開くことができます。  
  
-   **\[標準エンドポイント\]** ノードをクリックして展開し、それぞれのエンドポイント サブノードをクリックします。  
  
-   **\[標準エンドポイント\]** ノードをクリックし、詳細ペインでそれぞれのエンドポイントをクリックします。  
  
 エンドポイントの属性を右ペインに表示して、編集できます。  
  
#### 標準エンドポイント構成の削除  
 標準エンドポイント構成は次の方法で削除できます。  
  
-   **\[標準エンドポイント\]** ノードをクリックして展開し、それぞれのエンドポイント サブノードを右クリックします。コンテキスト コマンド **\[標準エンドポイント構成の削除\]** を使用して、エンドポイントを削除します。  
  
-   **\[標準エンドポイント\]** ノードをクリックします。**タスク** ペインで、**\[標準エンドポイント構成の削除\]** をクリックします。  
  
 標準エンドポイントが使用中の場合は、削除しようとしたときに警告メッセージが表示されます。警告は**「標準エンドポイントは使用されています。削除する場合は、構成の他の部分 \(サービス エンドポイントやクライアント エンドポイントなど\) からもすべての参照を必ず削除してください。そうしないと、構成が無効になり、次回から開けなくなります。標準エンドポイントを削除しますか?」**です。  
  
### バインディング  
 バインディング構成は、エンドポイントでバインディングを構成するために使用されます。これらの構成設定は、**\[バインド\]** ノードに格納されます。エンドポイントはバインディング構成を名前で参照し、複数のエンドポイントは単一のバインディング構成を参照できます。  
  
 **\[バインド\]** ノードは、構成ファイルのすべてのバインディング設定を表示します。ツリー内のすべてのサブノードは、構成ファイル内の \<`bindings`\> 要素のサブ要素に対応します。  
  
 **\[バインド\]** ノードをクリックして、**\[詳細ペイン\]** のバインディングの**概要ページ**でタスクを表示または実行できます。  
  
#### 新しいバインディング構成の作成  
 新しいバインディング構成は次の方法で作成できます。  
  
-   **\[バインド\]** ノードを右クリックし、**\[新しいバインド構成\]** を選択します。ダイアログ ボックスでバインディングの種類を選択し、**\[OK\]** をクリックします。  
  
-   **\[バインド\]** ノードを選択し、ウィンドウの左下のタスク ペインで **\[新しいバインド構成\]** をクリックします。  
  
-   サービスまたはクライアントの概要ページで、**\[バインドの構成\]** フィールドの **\[クリックして作成\]** をクリックすると、対応するエンドポイントのバインディング構成が作成されます。  
  
#### カスタム バインディングへのバインディング要素拡張の追加  
  
1.  拡張要素を追加するバインディングを選択します。  
  
2.  **\[追加\]** をクリックします。  
  
3.  使用できる拡張一覧から、追加するバインディング要素拡張を選択します。複数の項目を選択するには、Ctrl キーを同時に押します。  
  
4.  **\[追加\]** をクリックします。  
  
#### カスタム バインディングの拡張位置の調整  
 カスタム バインディングは、スタックを形成するバインディング要素のコレクションです。スタックの各バインディング要素には、独自の構成設定があります。カスタム バインディング内のバインディング要素拡張の順序は、スタック内のそれらの位置を示します。スタックの最上位の要素が最初に適用されます。順序を変更するには  
  
1.  カスタム バインディング ノードを選択します。  
  
2.  **\[バインド要素の拡張の位置\]** セクションでバインディング拡張要素の 1 つを選択します。  
  
3.  一覧の左側にある **\[Up\]** ボタンまたは **\[Down\]** ボタンを使用して、選択した要素の位置を変更します。  
  
#### カスタム バインディングのバインディング要素拡張の構成の編集  
  
1.  ツリー内のバインディング ノードを選択します。  
  
2.  編集する要素を含むカスタム バインディングを選択します。  
  
3.  編集するバインディング要素拡張を選択します。要素の設定が、編集場所である右ペインに表示されます。  
  
### 診断  
 **\[診断\]** ノードは、構成ファイルのすべての診断設定を表示します。診断を使用すると、パフォーマンス カウンターのオンまたはオフ、Windows Management Instrumentation \(WMI\) の有効または無効、[!INCLUDE[indigo2](../../../includes/indigo2-md.md)] トレースの構成、および [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] メッセージ ログの構成を行うことができます。**\[診断\]** ノードの設定は、\<`system.diagnostics`\> セクションと構成ファイルの `<diagnostics>``<system.serviceModel>` の セクションに対応します。  
  
 **\[診断\]** ノードをクリックして、**\[詳細ペイン\]** の診断の**概要ページ**でタスクを表示または実行できます。  
  
#### パフォーマンス カウンターと WMI の構成  
  
1.  診断ノードをクリックします。  
  
2.  **\[パフォーマンス カウンターの切り替え\]** をクリックします。パフォーマンス カウンターには、Off \(既定\)、ServiceOnly、All の 3 つの状態があります。リンクをクリックすると、これらの 3 つの状態の間で設定が切り替わります。  
  
#### WMI プロバイダーの構成  
  
1.  診断ノードをクリックします。  
  
2.  WMI プロバイダーを有効にするには、**\[WMI プロバイダーの有効化\]** リンクをクリックします。  
  
#### WCF トレースの有効化  
 標準プロパティを持つ [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] トレース ファイルを作成したり、カスタム トレース ファイルを設定したりできます。  
  
1.  診断ノードをクリックします。  
  
2.  **\[トレースの有効化\]** をクリックします。  
  
3.  トレース レベルを調整するには、**\[トレース レベル\]** リンクをクリックします。トレース レベルは、Off、Critical、Error、Warning、Information、Verbose の 6 つあります。**\[アクティビティのトレース\]** と **\[アクティビティの伝達\]** オプションを使用すると、[!INCLUDE[indigo2](../../../includes/indigo2-md.md)] アクティビティ トレース機能を使用できます。  
  
4.  トレース ファイルとオプションを指定するには、トレース リスナー名をクリックします。  
  
#### WCF ログの有効化  
 標準プロパティを持つ [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] トレース ファイルの作成またはカスタム トレース ファイルを作成できます。  
  
1.  診断ノードをクリックします。  
  
2.  **\[メッセージ ログの有効化\]** をクリックします。  
  
3.  ログ レベルを調整するには、**\[ログ レベル\]** リンクをクリックします。ログ レベルには、無効な形式、サービス、トランスポートの 3 つがあります。  
  
4.  ログ ファイルとオプションを指定するには、リスナー名をクリックします。  
  
> [!NOTE]
>  アプリケーションを閉じるときにトレース ログとメッセージ ログを自動的にフラッシュする場合は、**\[ログの自動フラッシュ\]** オプションを有効にします。  
  
 **\[診断\]** の**概要ページ**を使用すると、診断の構成で最も一般的なタスクを実行できます。ただし、リスナーとソースの設定を手動で編集する場合は、診断ノードを展開して、メッセージ ログ、リスナー、およびソースの各ノードの設定を編集する必要があります。  
  
#### WCF カスタム トレースまたはメッセージ ログの有効化  
  
1.  診断ノードをクリックして展開します。  
  
2.  リスナー ノードを右クリックし、**\[新しいリスナー\]** を選択します。  
  
3.  **\[InitData\]** フィールドにトレース ファイル名を入力します。\[...\] ボタンをクリックすると、パスを参照できます。  
  
4.  **\[TypeName\]** ラインをクリックすると、\[…\] ボタンが表示されます。このボタンをクリックすると、**\[トレース リスナー型ブラウザー\]** が開きます。これを使用すると、既にインストールされている事前構成のトレース リスナーを見つけることができます。  
  
5.  **\[ソース\]** セクションに注意してください。このセクションで **\[追加\]** をクリックすると、ダイアログ ボックスが開き、使用できるトレース ソースの一覧がドロップダウン メニューに表示されます。トレース ソースを選択して、**\[OK\]** をクリックします。  
  
6.  メッセージ ログ設定を編集するには、メッセージ ログ ノードをクリックします。プロパティ グリッドで設定を編集できます。  
  
### 詳細設定  
  
#### 動作  
 **\[動作\]** ノードは、現在構成ファイルに定義されている動作を表示します。  
  
 動作構成は、エンドポイントとサービスの動作を構成するために使用されます。これらの構成設定は、**\[サービス動作\]** と **\[エンドポイント動作\]** の下にある **\[詳細設定\]** ノードに格納されます。サービス動作はサービスによって使用され、エンドポイント動作はエンドポイントによって使用されます。  
  
 動作は、スタックを形成する拡張要素のコレクションです。スタック最上位の要素が最初に適用されます。各拡張要素は独自の構成を持つことができます。  
  
##### 新しい動作構成の作成  
 新しい動作構成は次の 2 つの方法で作成できます。  
  
-   動作ノードの 1 つを右クリックし、**\[新しいエンドポイント動作の構成\]** または **\[新しいサービス動作の構成\]** を選択します。  
  
-   動作ノードの 1 つを選択し、ウィンドウの左下のタスク ペインの **\[新しいサービス動作の構成\]** または \[新しいエンドポイント動作の構成\] をクリックします。  
  
##### 動作への動作要素拡張の追加  
  
1.  動作ノードのいずれかを選択します。  
  
2.  編集する動作を選択します。  
  
3.  **\[追加\]** をクリックします。  
  
4.  使用できる拡張一覧から、追加する動作要素拡張を選択します。  
  
5.  **\[追加\]** をクリックします。  
  
##### 動作の拡張位置の調整  
 動作は、スタックを形成する要素のコレクションです。スタックの各要素には、独自の構成があります。動作内の動作要素拡張の順序は、スタック内のそれらの位置を示します。スタックの最上位の要素が最初に適用されます。順序を変更するには  
  
1.  動作ノードのいずれかを選択します。  
  
2.  編集する動作を選択します。  
  
3.  **\[動的要素の拡張の位置\]** セクションで動作拡張要素を選択します。  
  
4.  一覧の左側にある **\[Up\]** ボタンまたは **\[Down\]** ボタンを使用して、選択した要素の位置を変更します。  
  
##### 動作要素拡張の構成の編集  
  
1.  ツリー内の動作ノードの 1 つを選択します。  
  
2.  編集する要素を含む動作を選択します。  
  
3.  編集する動作要素拡張を選択します。要素の設定が右ペインに表示されるので、ここで編集できます。  
  
#### ProtocolMapping  
 このセクションでは、プロトコル アドレス スキームと可能なバインドの間に定義されているマッピングを介して、http、tcp、MSMQ、net.pipe などのさまざまなプロトコルに対する既定のバインドの種類を設定できます。他のプロトコルへの新しいマッピングを追加することもできます。  
  
#### 拡張  
 新しいバインディング拡張、バインディング要素拡張、標準エンドポイント拡張、および動作拡張を登録して、[!INCLUDE[indigo2](../../../includes/indigo2-md.md)] 構成に使用できます。拡張は、名前と種類のペアです。名前は構成の拡張の名前を定義し、種類は拡張を実装します。拡張には次の 4 種類があります。  
  
-   バインディング拡張は、バインディングの種類全体を定義します。例 : `basicHttpBinding`。  
  
-   バインディング要素拡張は、バインディングの要素を定義します。例: `textMessageEncoding`。  
  
-   標準エンドポイント拡張は、標準エンドポイント全体を定義します。例: `discoveryEndpoint`。  
  
-   動作要素拡張は、動作の要素を定義します。例 : `clientVia`。  
  
 構成に登録されている拡張は、同じ種類の他のすべての [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] コンポーネントと同じように使用できます。  
  
##### 新しい機能の追加  
 詳細設定ノード内の拡張ノードの 1 つを選択します。  
  
1.  **\[新規作成\]** をクリックします。  
  
2.  名前と種類を入力します。  
  
3.  **\[OK\]** をクリックします。  
  
4.  拡張は、エディターの適切な場所に表示されるようになります。たとえば、動作要素拡張を追加すると、使用できる拡張一覧に表示されます。  
  
#### ホスト環境  
 ここでは、環境をホストするサービスのインスタンス化設定を定義できます。  
  
### ウィザードによる構成ファイルの作成  
 新しい構成ファイルを作成する 1 つの方法として、新しいサービス要素ウィザードを使用します。ウィザードは、インストールされたサービスの種類および [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] と互換性のある、COM\+、Web ホストの仮想ディレクトリなどの他の要素をコンピューター上で見つけると、それらを読み込んで、構成の作成を効率よく行います。  
  
#### 構成ファイルの作成  
  
1.  サービス構成エディターを起動するには、コマンド ウィンドウで [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] のインストール場所に移動し、「`SvcConfigEditor.exe`」と入力します。  
  
2.  **\[ファイル\]** メニューの **\[開く\]** を選択し、作成する構成ファイルの種類に応じて **\[実行可能\]**、**\[COM\+ サービス\]**、または **\[Web ホスト サービス\]** をクリックします。  
  
3.  **\[開く\]** ダイアログ ボックスで、構成ファイルを作成する特定のファイルに移動し、ダブルクリックします。  
  
4.  **\[ファイル\]** メニューの **\[新しい項目の追加\]** をポイントし、**\[サービス\]** をクリックします。新しいサービス要素ウィザードが開きます。  
  
5.  ウィザードの手順に従って新しいサービスを作成します。  
  
> [!NOTE]
>  ウィザードによって生成される構成ファイルから NetPeerTcpBinding を使用する場合は、手動でバインディング構成要素を追加し、その `security` 要素の `mode` 属性を "None" に変更する必要があります。  
  
## COM\+ の構成  
 サービス構成エディターによって、既存の COM\+ アプリケーションに新しい構成ファイルを作成したり、既存の COM\+ 構成を編集することができます。構成ファイルに \<`comContract`\> セクションが存在する場合は、**\[COM コントラクト\]** ノードだけが表示されます。  
  
### 新しい COM\+ 構成の作成  
 新しい COM\+ 構成を作成する前に、COM\+ アプリケーションがコンポーネント サービスにインストールされて、グローバル アセンブリ キャッシュ \(GAC\) に登録されていることを確認します。  
  
1.  **\[ファイル\]** メニューの **\[統合\]** をポイントし、**\[COM\+ アプリケーション\]** を選択します。この操作は、現在開いているファイルを閉じます。現在のファイルに保存されていないデータがあれば、\[保存\] ダイアログ ボックスが表示されます。**COM\+ 統合ウィザード**が起動されます。  
  
2.  最初のページで、ツリーから COM\+ アプリケーションを選択します。ツリーで COM\+ アプリケーションが見つからない場合は、COM\+ アプリケーションがコンポーネント サービスにインストールされて、グローバル アセンブリ キャッシュ \(GAC\) に登録されていることを確認します。  
  
3.  次のページで、[!INCLUDE[indigo2](../../../includes/indigo2-md.md)] サービスとして公開するメソッドを選択します。既定では、COM\+ アプリケーションでサポートされるすべてのメソッドが表示され、選択されます。  
  
4.  ホスト メソッドを選択します。  
  
5.  ウィザードの手順に従って他の設定を構成します。  
  
6.  サービス構成エディターは、バックグラウンドで ComSvcConfig.exe を利用して、構成ファイルを生成します。これが完了すると、概要を表示してウィザードを終了できます。構成を直接編集できるように、生成された構成ファイルが開きます。  
  
### 既存の COM\+ 構成の編集  
  
1.  **\[ファイル\]** メニューの **\[開く\]** をポイントし、**\[COM\+ サービス\]** を選択します。  
  
2.  編集する COM\+ サービスを一覧から選択します。  
  
3.  **\[COM コントラクト\]** ノードで構成設定を編集します。  
  
    > [!NOTE]
    >  COM コントラクトを含む構成ファイルを直接開いて編集することもできます。  
  
## セキュリティ  
 構成エディターによって生成されるサービス構成ファイルは、セキュリティによる保護が保証されていません。[!INCLUDE[indigo2](../../../includes/indigo2-md.md)] サービスをセキュリティで保護する方法については、「[セキュリティ](../../../docs/framework/wcf/feature-details/security.md)」を参照してください。  
  
 また、構成エディターを使用して読み書きできるのは、有効な [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] 構成要素だけです。ツールは、スキーマに準拠するユーザー定義の要素を無視します。また、これらの要素を構成ファイルから削除したり、既知の [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] 要素に対する影響を究明することはありません。これらの要素がアプリケーションやシステムに脅威を与えるかどうかを究明するのはユーザーの責任です。
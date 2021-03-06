---
title: "さまざまな型の配列のマーシャリング"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
- CSharp
- C++
- jsharp
helpviewer_keywords:
- marshaling, Arrays sample
- data marshaling, Arrays sample
ms.assetid: c5ac9920-5b6e-4dc9-bf2d-1f6f8ad3b0bf
caps.latest.revision: 15
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 3a91c74cf6734776fd6356c201e6754f34a8fbcf
ms.contentlocale: ja-jp
ms.lasthandoff: 08/21/2017

---
# <a name="marshaling-different-types-of-arrays"></a>さまざまな型の配列のマーシャリング
配列は、同じ型の 1 つ以上の要素を含むマネージ コード内の参照型です。 配列は参照型ですが、アンマネージ関数には In パラメーターとして渡されます。 この動作は、マネージ配列がマネージ オブジェクトに渡される方法 (In/Out パラメーターとして渡される) と一致しません。 詳細については、「 [コピーと固定](../../../docs/framework/interop/copying-and-pinning.md)」を参照してください。  
  
 次の表では、配列のマーシャリング オプションをリストして、それらの使用方法を説明します。  
  
|配列|説明|  
|-----------|-----------------|  
|値による整数の。|整数の配列を In パラメーターとして渡します。|  
|参照による整数の。|整数の配列を In/Out パラメーターとして渡します。|  
|値による整数の (2 次元)。|整数のマトリックスを In パラメーターとして渡します。|  
|値による文字列の。|文字列の配列を In パラメーターとして渡します。|  
|整数による構造体の。|In パラメーターとして整数を含む構造体の配列を渡します。|  
|文字列による構造体の。|In/Out パラメーターとして整数のみを含む構造体の配列を渡します。 配列のメンバーを変更することができます。|  
  
## <a name="example"></a>例  
 このサンプルは、以下の種類の配列差を渡す法を示します。  
  
-   値による整数の配列。  
  
-   サイズを変更できる、参照による整数の配列。  
  
-   値による整数の多次元配列 (マトリックス)。  
  
-   値による文字列の配列。  
  
-   整数による構造体の配列。  
  
-   文字列による構造体の配列。  
  
 配列が参照によって明示的にマーシャリングされない限り、既定の動作は、配列を In パラメーターとしてマーシャリングすることです。 この動作は、 <xref:System.Runtime.InteropServices.InAttribute> と <xref:System.Runtime.InteropServices.OutAttribute> 属性を明示的に適用することで変更できます。  
  
 Arrays のサンプルで使用するアンマネージ関数とその元の関数宣言を次に示します。  
  
-   PinvokeLib.dll からエクスポートされる**TestArrayOfInts** 。  
  
    ```  
    int TestArrayOfInts(int* pArray, int pSize);  
    ```  
  
-   PinvokeLib.dll からエクスポートされる**TestRefArrayOfInts** 。  
  
    ```  
    int TestRefArrayOfInts(int** ppArray, int* pSize);  
    ```  
  
-   PinvokeLib.dll からエクスポートされる**TestMatrixOfInts** 。  
  
    ```  
    int TestMatrixOfInts(int pMatrix[][COL_DIM], int row);  
    ```  
  
-   PinvokeLib.dll からエクスポートされる**TestArrayOfStrings** 。  
  
    ```  
    int TestArrayOfStrings(char** ppStrArray, int size);  
    ```  
  
-   PinvokeLib.dll からエクスポートされる**TestArrayOfStructs** 。  
  
    ```  
    int TestArrayOfStructs(MYPOINT* pPointArray, int size);  
    ```  
  
-   PinvokeLib.dll からエクスポートされる**TestArrayOfStructs2** 。  
  
    ```  
    int TestArrayOfStructs2 (MYPERSON* pPersonArray, int size);  
    ```  
  
 [PinvokeLib.dll](http://msdn.microsoft.com/en-us/5d1438d7-9946-489d-8ede-6c694a08f614) はカスタム アンマネージ ライブラリであり、上記の関数および 2 つの構造体変数 **MYPOINT** および **MYPERSON**に関する実装を含んでいます。 構造体には次の要素が含まれます。  
  
```  
typedef struct _MYPOINT  
{  
   int x;   
   int y;   
} MYPOINT;  
  
typedef struct _MYPERSON  
{  
   char* first;   
   char* last;   
} MYPERSON;  
```  
  
 このサンプルでは、 `MyPoint` と `MyPerson` 構造体に埋め込み型が含まれています。 各メンバーが出現する順番でメモリ内に順次配列されることを保証するために、 <xref:System.Runtime.InteropServices.StructLayoutAttribute> 属性を設定します。  
  
 `LibWrap` クラスには、 `App` クラスによって呼び出されるメソッドのセットが含まれます。 配列を渡す特定の方法について詳しくは、次のサンプル内のコメントを参照してください。 参照型の配列は、既定では In パラメーターとして渡されます。 呼び出し元が結果を受け取るためには、 **InAttribute** と **OutAttribute** を配列が含まれる引数に明示的に適用する必要があります。  
  
### <a name="declaring-prototypes"></a>プロトタイプの宣言  
 [!code-csharp[Conceptual.Interop.Marshaling#31](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/arrays.cs#31)] [!code-vb[Conceptual.Interop.Marshaling#31](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/arrays.vb#31)]  
  
### <a name="calling-functions"></a>関数の呼び出し  
 [!code-csharp[Conceptual.Interop.Marshaling#32](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/arrays.cs#32)] [!code-vb[Conceptual.Interop.Marshaling#32](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/arrays.vb#32)]  
  
## <a name="see-also"></a>関連項目  
 [型の配列のマーシャリング](http://msdn.microsoft.com/en-us/049b1c1b-228f-4445-88ec-91bc7fd4b1e8)   
 [プラットフォーム呼び出しのデータ型](http://msdn.microsoft.com/en-us/16014d9f-d6bd-481e-83f0-df11377c550f)   
 [マネージ コードでのプロトタイプの作成](../../../docs/framework/interop/creating-prototypes-in-managed-code.md)


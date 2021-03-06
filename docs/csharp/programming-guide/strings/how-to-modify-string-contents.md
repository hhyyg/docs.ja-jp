---
title: "方法 : 文字列の内容を変更する (C# プログラミング ガイド)"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- strings [C#], modifying
ms.assetid: b6c20bba-ce22-43d7-ad1b-5ce65f714055
caps.latest.revision: 16
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 114b6fdabd235d7e57947e77b672352e28aff11e
ms.contentlocale: ja-jp
ms.lasthandoff: 07/28/2017

---
# <a name="how-to-modify-string-contents-c-programming-guide"></a>方法 : 文字列の内容を変更する (C# プログラミング ガイド)
文字列は*不変*であるため、作成後に文字列オブジェクトの値を (アンセーフ コードを使用せずに) 変更することはできません。 ただし、文字列の値を変更し、新しい文字列オブジェクトに結果を格納する方法は数多くあります。 <xref:System.String?displayProperty=fullName> クラスでは、入力文字列を操作し、新しい文字列オブジェクトを返すメソッドが提供されます。 多くの場合、元の文字列を保持する変数に新しいオブジェクトを割り当てることができます。 <xref:System.Text.RegularExpressions.Regex?displayProperty=fullName> クラスでは、同じように動作する追加のメソッドが提供されます。 <xref:System.Text.StringBuilder?displayProperty=fullName> クラスは、"埋め込み先" を変更できる文字バッファーを提供します。 <xref:System.Text.StringBuilder.ToString%2A?displayProperty=fullName> メソッドを呼び出して、バッファーの現在の内容を含む新しい文字列オブジェクトを作成することができます。  
  
## <a name="example"></a>例  
 次の例では、指定された文字列で部分文字列の置換や削除を行うさまざまな方法を示します。  
  
 [!code-cs[csProgGuideStrings#28](../../../csharp/programming-guide/strings/codesnippet/CSharp/how-to-modify-string-contents_1.cs)]  
  
## <a name="example"></a>例  
 配列表記を使用して文字列内の個々の文字にアクセスする場合は、<xref:System.Text.StringBuilder> オブジェクトを使用できます。このオブジェクトは、`[]` 演算子をオーバーロードして、その内部文字バッファーへのアクセスを提供します。 また、<xref:System.String.ToCharArray%2A> メソッドを使用して、文字列を文字配列に変換することもできます。 次の例では、`ToCharArray` を使用して配列を作成します。 その後、この配列の一部の要素が変更されます。 さらに、新しい文字列を作成するために、入力パラメーターとして文字配列を受け取る文字列コンストラクターが呼び出されます。  
  
 [!code-cs[csProgGuideStrings#24](../../../csharp/programming-guide/strings/codesnippet/CSharp/how-to-modify-string-contents_2.cs)]  
  
## <a name="example"></a>例  
 次の例は、C スタイルの文字配列と同様の方法で、アンセーフ コードを使用して埋め込み文字列を変更する、非常にまれな場合のために用意したものです。 この例では、fixed キーワードを使用して、"埋め込まれている" 個々の文字にアクセスする方法を示します。 また、文字列に対してアンセーフ操作を実行すると発生する可能性のある副作用を示します。この副作用の原因は、C# コンパイラが文字列を内部に格納する (保持する) 方法にあります。 通常、この方法は、特に必要な場合以外は使用しないでください。  
  
 [!code-cs[csProgGuideStrings#29](../../../csharp/programming-guide/strings/codesnippet/CSharp/how-to-modify-string-contents_3.cs)]  
  
## <a name="see-also"></a>関連項目  
 [C# プログラミング ガイド](../../../csharp/programming-guide/index.md)   
 [文字列](../../../csharp/programming-guide/strings/index.md)


---
title: "方法: LINQ を使用して ArrayList をクエリする (C#)"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
ms.assetid: 2bfb471c-6e9a-4e60-bd83-4a1778abde11
caps.latest.revision: 3
author: BillWagner
ms.author: wiwagn
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
ms.translationtype: HT
ms.sourcegitcommit: 77d7bbaa99f7b8becf53244211ad480736d9ffab
ms.openlocfilehash: 3ef12d014715237fe752038466be85c4b47437eb
ms.contentlocale: ja-jp
ms.lasthandoff: 08/08/2017

---
# <a name="how-to-query-an-arraylist-with-linq-c"></a>方法: LINQ を使用して ArrayList をクエリする (C#)
LINQ を使用して <xref:System.Collections.ArrayList> などの非ジェネリックの <xref:System.Collections.IEnumerable> コレクションをクエリする場合、範囲変数の型を明示的に宣言して、オブジェクトの特定の型をコレクションに反映させる必要があります。 たとえば、`Student` オブジェクトの <xref:System.Collections.ArrayList> がある場合、[from 句](../../../../csharp/language-reference/keywords/from-clause.md)は次のようになります。  
  
```  
var query = from Student s in arrList  
...  
```  
  
 範囲変数の型を指定することで、<xref:System.Collections.ArrayList> 内の各項目を `Student` にキャストします。  
  
 明示的に型指定された範囲変数をクエリ式で使用すると、<xref:System.Linq.Enumerable.Cast%2A> メソッドを呼び出した場合と同じ結果を得ることができます。 指定したキャストを実行できない場合、<xref:System.Linq.Enumerable.Cast%2A> は例外をスローします。 <xref:System.Linq.Enumerable.Cast%2A> および <xref:System.Linq.Enumerable.OfType%2A> は、非ジェネリックの <xref:System.Collections.IEnumerable> 型で動作する、2 つの標準クエリ演算子メソッドです。 詳細については、「[LINQ クエリ操作での型の関係](../../../../csharp/programming-guide/concepts/linq/type-relationships-in-linq-query-operations.md)」を参照してください。  
  
## <a name="example"></a>例  
 次の例では、<xref:System.Collections.ArrayList> に対して単純なクエリを実行しています。 この例では、コードが <xref:System.Collections.ArrayList.Add%2A> メソッドを呼び出すときにオブジェクト初期化子を使用していますが、これは必須ではありません。  
  
```csharp  
using System;  
using System.Collections;  
using System.Linq;  
  
namespace NonGenericLINQ  
{  
    public class Student  
    {  
        public string FirstName { get; set; }  
        public string LastName { get; set; }  
        public int[] Scores { get; set; }  
    }  
  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            ArrayList arrList = new ArrayList();  
            arrList.Add(  
                new Student  
                    {  
                        FirstName = "Svetlana", LastName = "Omelchenko", Scores = new int[] { 98, 92, 81, 60 }  
                    });  
            arrList.Add(  
                new Student  
                    {  
                        FirstName = "Claire", LastName = "O’Donnell", Scores = new int[] { 75, 84, 91, 39 }  
                    });  
            arrList.Add(  
                new Student  
                    {  
                        FirstName = "Sven", LastName = "Mortensen", Scores = new int[] { 88, 94, 65, 91 }  
                    });  
            arrList.Add(  
                new Student  
                    {  
                        FirstName = "Cesar", LastName = "Garcia", Scores = new int[] { 97, 89, 85, 82 }  
                    });  
  
            var query = from Student student in arrList  
                        where student.Scores[0] > 95  
                        select student;  
  
            foreach (Student s in query)  
                Console.WriteLine(s.LastName + ": " + s.Scores[0]);  
  
            // Keep the console window open in debug mode.  
            Console.WriteLine("Press any key to exit.");  
            Console.ReadKey();  
        }  
    }  
}  
/* Output:   
    Omelchenko: 98  
    Garcia: 97  
*/  
```  
  
## <a name="see-also"></a>関連項目  
 [LINQ to Objects (C#)](../../../../csharp/programming-guide/concepts/linq/linq-to-objects.md)


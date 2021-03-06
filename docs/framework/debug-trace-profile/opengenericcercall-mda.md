---
title: openGenericCERCall MDA
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
- MDAs (managed debugging assistants), CER calls
- open generic CER calls
- constrained execution regions
- openGenericCERCall MDA
- CER calls
- managed debugging assistants (MDAs), CER calls
- generics [.NET Framework], open generic CER calls
ms.assetid: da3e4ff3-2e67-4668-9720-fa776c97407e
caps.latest.revision: 13
author: mairaw
ms.author: mairaw
manager: wpickett
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 347f9efcf1b0cdaf9cd37bcf6045a42341e4f643
ms.contentlocale: ja-jp
ms.lasthandoff: 08/21/2017

---
# <a name="opengenericcercall-mda"></a>openGenericCERCall MDA
`openGenericCERCall` マネージ デバッグ アシスタントは、ルート メソッドにジェネリック型変数を持つ制約された実行領域 (CER) グラフが JIT コンパイル時またはネイティブ イメージ生成時に処理されている場合に、少なくとも 1 つのジェネリック型変数がオブジェクト参照型であることを警告するためにアクティブ化されます。  
  
## <a name="symptoms"></a>症状  
 スレッドが中止されたとき、またはアプリケーション ドメインがアンロードされたときに CER コードが実行されません。  
  
## <a name="cause"></a>原因  
 JIT コンパイル時には、処理結果のコードが共有され、オブジェクト参照型変数がそれぞれ任意のオブジェクト参照型になる可能性があるため、オブジェクト参照型を含むインスタンス化は代理にすぎません。 このため、一部のランタイム リソースを前もって準備することが妨げられる場合があります。  
  
 特に、ジェネリック型変数を含むメソッドは、バックグラウンドでリソースを遅延割り当てする可能性があります。 このようなメソッドは、ジェネリック辞書エントリと呼ばれます。 たとえば、`T` がジェネリック型変数である `List<T> list = new List<T>();` というステートメントの場合、ランタイムは、実行時に正確なインスタンス化を検索する必要があり、さらにその作成が必要になる場合があります (`List<Object>, List<String>` など)。 この操作は、メモリ不足など、開発者が制御できないさまざまな理由で失敗することがあります。  
  
 この MDA は、JIT コンパイル時にのみアクティブになり、正確なインスタンス化が存在するときにはアクティブになりません。  
  
 この MDA がアクティブになるとき、正しくないインスタンス化に対して CER が機能しないという症状が発生する可能性があります。 実際、MDA がアクティブになる状況下では、ランタイムは CER の実装を試みません。 そのため、開発者が CER の共有インスタンス化を使用している場合、目的の CER の領域内で発生した JIT コンパイル エラー、ジェネリック型の読み込みエラー、スレッドの中止などはキャッチされません。  
  
## <a name="resolution"></a>解決策  
 CER が存在する可能性があるメソッドには、オブジェクト参照型であるジェネリック型変数を使用しないでください。  
  
## <a name="effect-on-the-runtime"></a>ランタイムへの影響  
 この MDA は CLR に影響しません。  
  
## <a name="output"></a>出力  
 この MDA の出力サンプルを次に示します。  
  
 `Method 'GenericMethodWithCer', which contains at least one constrained execution region, cannot be prepared automatically since it has one or more unbound generic type parameters.`  
  
 `The caller must ensure this method is prepared explicitly at run time prior to execution.`  
  
 `method name="GenericMethodWithCer"`  
  
 `declaringType name="OpenGenericCERCall"`  
  
## <a name="configuration"></a>構成  
  
```xml  
<mdaConfig>  
  <assistants>  
    <openGenericCERCall/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>例  
 CER コードは実行されません。  
  
```  
using System;  
using System.Collections.Generic;  
using System.Runtime.CompilerServices;  
  
class Program  
{  
    static void Main(string[] args)  
    {  
        CallGenericMethods();  
    }  
    static void CallGenericMethods()  
    {  
        // This call is correct. The instantiation of the method  
        // contains only nonreference types.  
        MyClass.GenericMethodWithCer<int>();  
  
        // This call is incorrect. A shared version of the method that  
        // cannot be completely analyzed will be JIT-compiled. The   
        // MDA will be activated at JIT-compile time, not at run time.  
        MyClass.GenericMethodWithCer<String>();  
    }  
}  
  
    class MyClass  
{  
    public static void GenericMethodWithCer<T>()  
    {  
        RuntimeHelpers.PrepareConstrainedRegions();  
        try  
        {  
  
        }  
        finally  
        {  
            // This is the start of the CER.  
            Console.WriteLine("In finally block.");  
        }  
    }  
}  
```  
  
## <a name="see-also"></a>関連項目  
 <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareMethod%2A>   
 <xref:System.Runtime.ConstrainedExecution>   
 [マネージ デバッグ アシスタントによるエラーの診断](../../../docs/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)


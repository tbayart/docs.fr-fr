---
title: where, clause (Référence C#)
ms.date: 07/20/2015
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
f1_keywords:
- whereclause_CSharpKeyword
helpviewer_keywords:
- where keyword [C#]
- where clause [C#]
ms.assetid: 7f9bf952-7744-4f91-b676-cddb55d107c3
caps.latest.revision: 16
author: BillWagner
ms.author: wiwagn
ms.openlocfilehash: 0324346ee5e214bf467fcb522ef781c91fa1b76f
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2017
---
# <a name="where-clause-c-reference"></a>where, clause (Référence C#)
La clause `where` est utilisée dans une expression de requête pour spécifier les éléments de la source de données qui seront retournés dans l’expression de requête. Elle applique une condition booléenne (un *prédicat*) à chaque élément source (référencé par la variable de portée) et retourne ceux pour lesquels la condition spécifiée est remplie. Une expression de requête unique peut contenir plusieurs clauses `where` et une clause unique peut contenir plusieurs sous-expressions de prédicat.  
  
## <a name="example"></a>Exemple  
 Dans l’exemple suivant, la clause `where` élimine par filtrage tous les nombres excepté ceux inférieurs à cinq. Si vous supprimez la clause `where`, tous les nombres de la source de données seront retournés. L’expression `num < 5` est le prédicat qui est appliqué à chaque élément.  
  
 [!code-csharp[cscsrefQueryKeywords#5](../../../csharp/language-reference/keywords/codesnippet/CSharp/where-clause_1.cs)]  
  
## <a name="example"></a>Exemple  
 Dans une clause `where` unique, vous pouvez spécifier autant de prédicats que nécessaire à l’aide des opérateurs [&&](../../../csharp/language-reference/operators/conditional-and-operator.md) et [&#124;&#124;](../../../csharp/language-reference/operators/conditional-or-operator.md). Dans l’exemple suivant, la requête spécifie deux prédicats pour sélectionner uniquement les nombres pairs inférieurs à cinq.  
  
 [!code-csharp[cscsrefQueryKeywords#6](../../../csharp/language-reference/keywords/codesnippet/CSharp/where-clause_2.cs)]  
  
## <a name="example"></a>Exemple  
 Une clause `where` peut contenir une ou plusieurs méthodes qui retournent des valeurs booléennes. Dans l’exemple suivant, la clause `where` utilise une méthode pour déterminer si la valeur actuelle de la variable de portée est paire ou impaire.  
  
 [!code-csharp[cscsrefQueryKeywords#7](../../../csharp/language-reference/keywords/codesnippet/CSharp/where-clause_3.cs)]  
  
## <a name="remarks"></a>Remarques  
 La clause `where` est un mécanisme de filtrage. Elle peut être placée à presque n’importe quel endroit d’une expression de requête, sauf qu’elle ne peut pas être la première ni la dernière clause. Une clause `where` peut apparaître avant ou après une clause [group](../../../csharp/language-reference/keywords/group-clause.md) selon que vous devez filtrer les éléments sources avant ou après leur regroupement.  
  
 Si un prédicat spécifié n’est pas valide pour les éléments de la source de données, une erreur de compilation est générée. C’est l’un des avantages de la vérification de type fort fourni par [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)].  
  
 Au moment de la compilation, le mot clé `where` est converti en appel à la méthode d’opérateur de requête standard <xref:System.Linq.Enumerable.Where%2A>.  
  
## <a name="see-also"></a>Voir aussi  
 [Mots clés de requête (LINQ)](../../../csharp/language-reference/keywords/query-keywords.md)  
 [from, clause](../../../csharp/language-reference/keywords/from-clause.md)  
 [select, clause](../../../csharp/language-reference/keywords/select-clause.md)  
 [Filtrage des données](../../programming-guide/concepts/linq/filtering-data.md)  
 [Expressions de requête LINQ](../../../csharp/programming-guide/linq-query-expressions/index.md)  
 [Bien démarrer avec LINQ en C#](../../../csharp/programming-guide/concepts/linq/getting-started-with-linq.md)

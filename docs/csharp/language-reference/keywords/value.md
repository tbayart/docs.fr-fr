---
title: "value (r&#233;f&#233;rence C#) | Microsoft Docs"
ms.date: "2015-07-20"
ms.prod: ".net"
ms.technology: 
  - "devlang-csharp"
ms.topic: "article"
f1_keywords: 
  - "value_CSharpKeyword"
dev_langs: 
  - "CSharp"
helpviewer_keywords: 
  - "value (mot clé C#)"
ms.assetid: c99d6468-687f-4a46-89b4-a95e1b00bf6d
caps.latest.revision: 14
author: "BillWagner"
ms.author: "wiwagn"
caps.handback.revision: 14
---
# value (r&#233;f&#233;rence C#)
Le mot clé contextuel `value` est utilisé dans l'accesseur set dans les déclarations de propriété ordinaires.  Il est semblable à un paramètre d'entrée sur une méthode.  Le mot `value` référence la valeur que le code client essaie d'assigner à la propriété.  Dans l'exemple suivant, `MyDerivedClass` a une propriété appelée `Name` qui utilise le paramètre `value` pour assigner une nouvelle chaîne au champ de stockage `name`.  Du point de vue de code client, l'opération est écrite comme une assignation simple.  
  
 [!code-cs[csrefKeywordsModifiers#26](../../../csharp/language-reference/keywords/codesnippet/csharp/csrefKeywordsModifiers/csrefKeywordsModifiers.cs#26)]  
  
 Pour plus d'informations sur l'utilisation de `value`, consultez [Propriétés](../../../csharp/programming-guide/classes-and-structs/properties.md).  
  
## Spécification du langage C\#  
 [!INCLUDE[CSharplangspec](../../../csharp/language-reference/keywords/includes/csharplangspec-md.md)]  
  
## Voir aussi  
 [Référence C\#](../../../csharp/language-reference/index.md)   
 [Guide de programmation C\#](../../../csharp/programming-guide/index.md)   
 [Mots clés C\#](../../../csharp/language-reference/keywords/index.md)
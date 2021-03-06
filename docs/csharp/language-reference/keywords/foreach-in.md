---
title: "foreach, in (référence C#)"
ms.date: 10/11/2017
ms.prod: .net
ms.technology: devlang-csharp
ms.topic: article
f1_keywords:
- foreach
- foreach_CSharpKeyword
helpviewer_keywords:
- foreach keyword [C#]
- foreach statement [C#]
- in keyword [C#]
ms.assetid: 5a9c5ddc-5fd3-457a-9bb6-9abffcd874ec
caps.latest.revision: "29"
author: BillWagner
ms.author: wiwagn
ms.openlocfilehash: d5601682d53a01ff07aba7e416aa81ded4c03e4e
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2017
---
# <a name="foreach-in-c-reference"></a>foreach, in (référence C#)
L’instruction `foreach` répète un groupe d’instructions incorporées pour chaque élément d’un tableau ou d’une collection d’objets qui implémente l’interface <xref:System.Collections.IEnumerable?displayProperty=nameWithType> ou <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType>. L’instruction `foreach` est utilisée pour itérer la collection dans le but d’obtenir les informations dont vous avez besoin. Elle ne peut pas être utilisée pour ajouter ou supprimer des éléments de la collection source pour éviter des effets secondaires imprévisibles. Si vous avez besoin d’ajouter ou de supprimer des éléments de la collection source, utilisez une boucle [for](for.md).
  
 Les instructions incorporées continuent de s’exécuter pour chaque élément de la collection ou du tableau. Une fois l’itération terminée pour tous les éléments de la collection, le contrôle est transféré à l’instruction qui suit le bloc `foreach`.
  
 Vous pouvez quitter la boucle à n’importe quel endroit du bloc `foreach` à l’aide du mot clé [break](break.md), ou passer à l’itération suivante de la boucle à l’aide du mot clé [continue](continue.md).

 Une boucle `foreach` peut également être quittée à l’aide des instructions [goto](goto.md), [return](return.md) ou [throw](throw.md).

 Pour plus d’informations sur le mot clé `foreach` et sur les exemples de code, consultez les rubriques suivantes :  

 [Utilisation de foreach avec des tableaux](../../programming-guide/arrays/using-foreach-with-arrays.md)  

 [Guide pratique pour accéder à une classe de collection à l’aide de foreach](../../programming-guide/classes-and-structs/how-to-access-a-collection-class-with-foreach.md)  

## <a name="example"></a>Exemple
 Le code suivant présente trois exemples.

> [!TIP]
> Vous pouvez modifier les exemples pour faire des essais avec la syntaxe et essayez différentes utilisations qui ressemblent plus à votre cas d’usage. Appuyez sur « Exécuter » pour exécuter le code, puis modifier et appuyez à nouveau sur « Exécuter ».

-   Une boucle `foreach` typique qui affiche le contenu d’un tableau d’entiers

[!code-csharp-interactive[csrefKeywordsIteration#4](./codesnippet/CSharp/foreach-in_1.cs#L12-L26)]

-   Une boucle [for](../../../csharp/language-reference/keywords/for.md) qui fait la même chose

[!code-csharp-interactive[csrefKeywordsIteration#4](./codesnippet/CSharp/foreach-in_1.cs#L31-L46)]

-   Une boucle `foreach` qui compte le nombre d’éléments du tableau

[!code-csharp-interactive[csrefKeywordsIteration#4](./codesnippet/CSharp/foreach-in_1.cs#L51-L69)]
 
## <a name="c-language-specification"></a>Spécification du langage C#
[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>Voir aussi  

[Référence C#](../index.md)

[Guide de programmation C#](../../programming-guide/index.md)

[Mots clés C#](index.md)

[Instructions d’itération](iteration-statements.md)

[for](for.md)

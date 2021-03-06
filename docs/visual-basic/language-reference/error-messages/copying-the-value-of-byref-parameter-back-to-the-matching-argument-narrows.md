---
title: Copie la valeur des &#39; ByRef &#39; paramètre &#39; &lt;nom_paramètre&gt;&#39; retour à la correspondance restreint d’argument de type &#39;&lt; NomType1&gt;&#39; en type &#39;&lt; nom_type2&gt;&#39;
ms.date: 07/20/2015
ms.prod: .net
ms.reviewer: ''
ms.suite: ''
ms.technology:
- devlang-visual-basic
ms.topic: article
f1_keywords:
- bc32053
- vbc32053
helpviewer_keywords:
- BC32053
ms.assetid: 281564b7-99f7-451f-b10d-f985e831bb25
caps.latest.revision: 8
author: dotnet-bot
ms.author: dotnetcontent
ms.openlocfilehash: 4bf993639007162e2e17d4b8cb9dfe8d5316acaa
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2017
---
# <a name="copying-the-value-of-39byref39-parameter-39ltparameternamegt39-back-to-the-matching-argument-narrows-from-type-39lttypename1gt39-to-type-39lttypename2gt39"></a>Copie la valeur des &#39; ByRef &#39; paramètre &#39; &lt;nom_paramètre&gt;&#39; retour à la correspondance restreint d’argument de type &#39;&lt; NomType1&gt;&#39; en type &#39;&lt; nom_type2&gt;&#39;
Une procédure est appelée avec un argument qui s’étend au type de paramètre correspondant, et la conversion à partir du paramètre à l’argument est restrictive.  
  
 Quand vous définissez une classe ou une structure, vous pouvez définir un ou plusieurs opérateurs de conversion pour convertir le type de la classe ou de la structure en d’autres types. Vous pouvez également définir des opérateurs de conversion inverse pour convertir ces autres types vers le type de votre classe ou de votre structure. Quand vous utilisez votre type de classe ou de structure dans un appel de procédure, [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] peut utiliser ces opérateurs de conversion pour convertir le type d’un argument vers le type de son paramètre correspondant.  
  
 Si vous passez l’argument [ByRef](../../../visual-basic/language-reference/modifiers/byref.md), [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] copie parfois la valeur d’argument dans une variable locale de la procédure au lieu de passer une référence. Dans ce cas, quand la procédure est retournée, [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] doit recopier la valeur de la variable locale dans l’argument du code appelant.  
  
 Si une valeur d’argument `ByRef` est copiée dans la procédure, et si l’argument et le paramètre sont du même type, aucune conversion n’est nécessaire. Toutefois, si les types sont différents, [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] doit effectuer une conversion dans les deux sens. Si l’un des types est celui de votre classe ou de votre structure, [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] doit le convertir vers et à partir de l’autre type. Si une de ces conversions est étendue, la conversion inverse peut être restrictive.  
  
 **ID d’erreur :** BC32053  
  
## <a name="to-correct-this-error"></a>Pour corriger cette erreur  
  
-   Si possible, utilisez un argument d’appel du même type que celui du paramètre de procédure, pour que [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] n’ait pas besoin d’effectuer de conversion.  
  
-   Si vous avez besoin d’appeler la procédure avec un argument de type différent du type de paramètre, mais n’avez pas besoin de retourner une valeur dans l’argument d’appel, définissez le paramètre [ByVal](../../../visual-basic/language-reference/modifiers/byval.md) au lieu de `ByRef`.  
  
-   Si vous devez retourner une valeur dans l’argument d’appel, définissez l’opérateur de conversion inverse comme [Widening](../../../visual-basic/language-reference/modifiers/widening.md), si possible.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures](../../../visual-basic/programming-guide/language-features/procedures/index.md)  
 [Paramètres et arguments d’une procédure](../../../visual-basic/programming-guide/language-features/procedures/procedure-parameters-and-arguments.md)  
 [Passage d’un argument par valeur et par référence](../../../visual-basic/programming-guide/language-features/procedures/passing-arguments-by-value-and-by-reference.md)  
 [Procédures d’opérateur](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md)  
 [Operator (instruction)](../../../visual-basic/language-reference/statements/operator-statement.md)  
 [Guide pratique : définir un opérateur](../../../visual-basic/programming-guide/language-features/procedures/how-to-define-an-operator.md)  
 [Guide pratique : définir un opérateur de conversion](../../../visual-basic/programming-guide/language-features/procedures/how-to-define-a-conversion-operator.md)  
 [Conversions de type dans Visual Basic](../../../visual-basic/programming-guide/language-features/data-types/type-conversions.md)  
 [Conversions étendues et restrictives](../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)

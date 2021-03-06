---
title: -recurse
ms.date: 03/13/2018
ms.prod: .net
ms.suite: ''
ms.technology:
- devlang-visual-basic
ms.topic: article
helpviewer_keywords:
- /recurse compiler option [Visual Basic]
- -recurse compiler option [Visual Basic]
- recurse compiler option [Visual Basic]
ms.assetid: 84a0b670-33ae-44c4-a46a-b90388809317
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: bb1cc114c2882aa82787f94a271dd7684c716b01
ms.sourcegitcommit: 498799639937c89de777361aab74261efe7b79ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="-recurse"></a>-recurse
Compile les fichiers de code source dans tous les répertoires enfants du répertoire spécifié ou le répertoire du projet.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-recurse:[dir\]file  
```  
  
## <a name="arguments"></a>Arguments  
 `dir`  
 Facultatif. Répertoire dans lequel vous voulez commencer la recherche. Si non spécifié, la recherche commence dans le répertoire du projet.  
  
 `file`  
 Obligatoire. Le ou les fichiers à rechercher. Les caractères génériques sont autorisés.  
  
## <a name="remarks"></a>Notes  
 Vous pouvez utiliser des caractères génériques dans un nom de fichier pour compiler tous les fichiers correspondants dans le répertoire du projet sans utiliser `-recurse`. Si aucun nom de fichier de sortie n’est spécifié, le compilateur base le nom de fichier de sortie sur le premier fichier d’entrée traité. Il s’agit généralement du premier fichier dans la liste des fichiers compilés lorsqu’ils sont affichés par ordre alphabétique. Pour cette raison, il est préférable de spécifier un fichier de sortie à l’aide de la `-out` option.  
  
> [!NOTE]
>  Le `-recurse` option n’est pas disponible dans l’environnement de développement Visual Studio ; il est disponible uniquement lors de la compilation à partir de la ligne de commande.  
  
## <a name="example"></a>Exemple  
 La commande suivante compile tous [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] fichiers dans le répertoire actif.  
  
```console
vbc *.vb  
```  
  
 La commande suivante compile tous les [!INCLUDE[vbprvb](~/includes/vbprvb-md.md)] les fichiers dans le `Test\ABC` répertoire et ses sous-répertoires, puis génère `Test.ABC.dll`.  
  
```console
vbc -target:library -out:Test.ABC.dll -recurse:Test\ABC\*.vb  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Compilateur de ligne de commande de Visual Basic](../../../visual-basic/reference/command-line-compiler/index.md)  
 [-out (Visual Basic)](../../../visual-basic/reference/command-line-compiler/out.md)  
 [Exemples de lignes de commande de compilation](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)

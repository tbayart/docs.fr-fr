---
title: "CloseAssembly, méthode"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name:
- IALink.CloseAssembly
- CloseAssembly
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- CloseAssembly
helpviewer_keywords:
- CloseAssembly method
ms.assetid: f66a43bc-a5c5-4190-acbe-63fd27640634
topic_type:
- apiref
caps.latest.revision: 
author: mairaw
ms.author: mairaw
manager: wpickett
ms.workload:
- dotnet
ms.openlocfilehash: 160ec53440f4ecdb924537c732a367881e75acf9
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/22/2017
---
# <a name="closeassembly-method"></a>CloseAssembly, méthode
Finalise les opérations d’assembly. Appelez cette méthode avant de commencer un nouvel assembly ou un module indépendant.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
HRESULT CloseAssembly(  
    mdAssembly AssemblyID  
) PURE;  
```  
  
#### <a name="parameters"></a>Paramètres  
 `AssemblyID`  
 ID de l’assembly.  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne S_OK si la méthode réussit.  
  
## <a name="requirements"></a>Configuration requise  
 Requiert alink.h.  
  
## <a name="see-also"></a>Voir aussi  
 [IALink, interface](../../../../docs/framework/unmanaged-api/alink/ialink-interface.md)  
 [IALink2, interface](../../../../docs/framework/unmanaged-api/alink/ialink2-interface.md)  
 [API ALink](../../../../docs/framework/unmanaged-api/alink/index.md)

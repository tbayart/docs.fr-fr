---
title: "ASM_DISPLAY_FLAGS, énumération"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name: ASM_DISPLAY_FLAGS
api_location: fusion.dll
api_type: COM
f1_keywords: ASM_DISPLAY_FLAGS
helpviewer_keywords: ASM_DISPLAY_FLAGS enumeration [.NET Framework fusion]
ms.assetid: dbade6c9-9d26-4a79-9fd2-46108edd12d7
topic_type: apiref
caps.latest.revision: "8"
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.openlocfilehash: 5e9fcd6cc8c1a352567f635e848d49e322a35ea6
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2017
---
# <a name="asmdisplayflags-enumeration"></a><span data-ttu-id="966a4-102">ASM_DISPLAY_FLAGS, énumération</span><span class="sxs-lookup"><span data-stu-id="966a4-102">ASM_DISPLAY_FLAGS Enumeration</span></span>
<span data-ttu-id="966a4-103">Indique la version, version, culture, signature, etc., de l’assembly dont le nom complet sera récupéré par le [IAssemblyName::GetDisplayName](../../../../docs/framework/unmanaged-api/fusion/iassemblyname-getdisplayname-method.md) (méthode).</span><span class="sxs-lookup"><span data-stu-id="966a4-103">Indicates the version, build, culture, signature, and so on, of the assembly whose display name will be retrieved by the [IAssemblyName::GetDisplayName](../../../../docs/framework/unmanaged-api/fusion/iassemblyname-getdisplayname-method.md) method.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="966a4-104">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="966a4-104">Syntax</span></span>  
  
```  
typedef enum {  
  
    ASM_DISPLAYF_VERSION                 = 0x01,  
    ASM_DISPLAYF_CULTURE                 = 0x02,  
    ASM_DISPLAYF_PUBLIC_KEY_TOKEN        = 0x04,  
    ASM_DISPLAYF_PUBLIC_KEY              = 0x08,  
    ASM_DISPLAYF_CUSTOM                  = 0x10,  
    ASM_DISPLAYF_PROCESSORARCHITECTURE   = 0x20,  
    ASM_DISPLAYF_LANGUAGEID              = 0x40,  
    ASM_DISPLAYF_RETARGET                = 0x80,  
    ASM_DISPLAYF_CONFIG_MASK             = 0x100,  
    ASM_DISPLAYF_MVID                    = 0x200,  
    ASM_DISPLAYF_FULL                    =   
                      ASM_DISPLAYF_VERSION           |   
                      ASM_DISPLAYF_CULTURE           |   
                      ASM_DISPLAYF_PUBLIC_KEY_TOKEN  |   
                      ASM_DISPLAYF_RETARGET          |   
                      ASM_DISPLAYF_PROCESSORARCHITECTURE  
  
} ASM_DISPLAY_FLAGS;  
```  
  
## <a name="remarks"></a><span data-ttu-id="966a4-105">Remarques</span><span class="sxs-lookup"><span data-stu-id="966a4-105">Remarks</span></span>  
 <span data-ttu-id="966a4-106">`ASM_DISPLAYF_FULL`reflète toutes les modifications apportées à la version de la [IAssemblyName](../../../../docs/framework/unmanaged-api/fusion/iassemblyname-interface.md) objet.</span><span class="sxs-lookup"><span data-stu-id="966a4-106">`ASM_DISPLAYF_FULL` reflects any changes made to the version of the [IAssemblyName](../../../../docs/framework/unmanaged-api/fusion/iassemblyname-interface.md) object.</span></span> <span data-ttu-id="966a4-107">Ne supposez pas que la valeur retournée est immuable.</span><span class="sxs-lookup"><span data-stu-id="966a4-107">Do not assume that the returned value is immutable.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="966a4-108">Spécifications</span><span class="sxs-lookup"><span data-stu-id="966a4-108">Requirements</span></span>  
 <span data-ttu-id="966a4-109">**Plateformes :** consultez [requise](../../../../docs/framework/get-started/system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="966a4-109">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="966a4-110">**En-tête :** Fusion.h</span><span class="sxs-lookup"><span data-stu-id="966a4-110">**Header:** Fusion.h</span></span>  
  
 <span data-ttu-id="966a4-111">**Bibliothèque :** inclus en tant que ressource dans MsCorEE.dll</span><span class="sxs-lookup"><span data-stu-id="966a4-111">**Library:** Included as a resource in MsCorEE.dll</span></span>  
  
 <span data-ttu-id="966a4-112">**Versions du .NET framework :**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="966a4-112">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="966a4-113">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="966a4-113">See Also</span></span>  
 [<span data-ttu-id="966a4-114">IAssemblyName (Interface)</span><span class="sxs-lookup"><span data-stu-id="966a4-114">IAssemblyName Interface</span></span>](../../../../docs/framework/unmanaged-api/fusion/iassemblyname-interface.md)  
 [<span data-ttu-id="966a4-115">Énumérations de fusion</span><span class="sxs-lookup"><span data-stu-id="966a4-115">Fusion Enumerations</span></span>](../../../../docs/framework/unmanaged-api/fusion/fusion-enumerations.md)
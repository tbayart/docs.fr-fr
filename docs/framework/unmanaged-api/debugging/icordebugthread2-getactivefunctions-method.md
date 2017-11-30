---
title: "ICorDebugThread2::GetActiveFunctions, méthode"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name: ICorDebugThread2.GetActiveFunctions
api_location: mscordbi.dll
api_type: COM
f1_keywords: ICorDebugThread2::GetActiveFunctions
helpviewer_keywords:
- GetActiveFunctions method [.NET Framework debugging]
- ICorDebugThread2::GetActiveFunctions method [.NET Framework debugging]
ms.assetid: 27fae01a-ecec-423a-973e-24f8de55826c
topic_type: apiref
caps.latest.revision: "12"
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.openlocfilehash: d1f7f416a5441b788394e93a98d274d02fa2ec2f
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2017
---
# <a name="icordebugthread2getactivefunctions-method"></a><span data-ttu-id="f5df2-102">ICorDebugThread2::GetActiveFunctions, méthode</span><span class="sxs-lookup"><span data-stu-id="f5df2-102">ICorDebugThread2::GetActiveFunctions Method</span></span>
<span data-ttu-id="f5df2-103">Obtient des informations sur la fonction active dans chacun des frames de ce thread.</span><span class="sxs-lookup"><span data-stu-id="f5df2-103">Gets information about the active function in each of this thread's frames.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="f5df2-104">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="f5df2-104">Syntax</span></span>  
  
```  
HRESULT GetActiveFunctions (  
    [in]   ULONG32             cFunctions,  
    [out]  ULONG32             *pcFunctions,  
    [in, out, size_is(cFunctions), length_is(*pcFunctions)]  
        COR_ACTIVE_FUNCTION    pFunctions[]  
);  
```  
  
#### <a name="parameters"></a><span data-ttu-id="f5df2-105">Paramètres</span><span class="sxs-lookup"><span data-stu-id="f5df2-105">Parameters</span></span>  
 `cFunctions`  
 <span data-ttu-id="f5df2-106">[in] Taille du tableau `pFunctions`.</span><span class="sxs-lookup"><span data-stu-id="f5df2-106">[in] The size of the `pFunctions` array.</span></span>  
  
 `pcFunctions`  
 <span data-ttu-id="f5df2-107">[out] Un pointeur vers le nombre d’objets retournés dans le `pFunctions` tableau.</span><span class="sxs-lookup"><span data-stu-id="f5df2-107">[out] A pointer to the number of objects returned in the `pFunctions` array.</span></span> <span data-ttu-id="f5df2-108">Le nombre d’objets renvoyés est égal au nombre de frames managés sur la pile.</span><span class="sxs-lookup"><span data-stu-id="f5df2-108">The number of objects returned will be equal to the number of managed frames on the stack.</span></span>  
  
 `pFunctions`  
 <span data-ttu-id="f5df2-109">[dans, out] Tableau d’objets COR_ACTIVE_FUNCTION, dont chacun contient des informations sur les fonctions actives dans les frames de ce thread.</span><span class="sxs-lookup"><span data-stu-id="f5df2-109">[in, out] An array of COR_ACTIVE_FUNCTION objects, each of which contains information about the active functions in this thread's frames.</span></span>  
  
 <span data-ttu-id="f5df2-110">Le premier élément sera utilisé pour le frame terminal et ainsi de suite jusqu’à la racine de la pile.</span><span class="sxs-lookup"><span data-stu-id="f5df2-110">The first element will be used for the leaf frame, and so on back to the root of the stack.</span></span>  
  
## <a name="remarks"></a><span data-ttu-id="f5df2-111">Remarques</span><span class="sxs-lookup"><span data-stu-id="f5df2-111">Remarks</span></span>  
 <span data-ttu-id="f5df2-112">Si `pFunctions` a la valeur null en entrée, `GetActiveFunctions` retourne uniquement le nombre de fonctions qui se trouvent sur la pile.</span><span class="sxs-lookup"><span data-stu-id="f5df2-112">If `pFunctions` is null on input, `GetActiveFunctions` returns only the number of functions that are on the stack.</span></span> <span data-ttu-id="f5df2-113">Autrement dit, si `pFunctions` a la valeur null en entrée, `GetActiveFunctions` retourne une valeur uniquement dans `pcFunctions`.</span><span class="sxs-lookup"><span data-stu-id="f5df2-113">That is, If `pFunctions` is null on input, `GetActiveFunctions` returns a value only in `pcFunctions`.</span></span>  
  
 <span data-ttu-id="f5df2-114">Le `GetActiveFunctions` méthode est une optimisation sur l’obtention des informations mêmes des frames dans une trace de pile et inclut uniquement les frames qui auraient eu un objet ICorDebugILFrame pour eux dans la trace de pile complet.</span><span class="sxs-lookup"><span data-stu-id="f5df2-114">The `GetActiveFunctions` method is intended as an optimization over getting the same information from frames in a stack trace, and includes only frames that would have had an ICorDebugILFrame object for them in the full stack trace.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="f5df2-115">Spécifications</span><span class="sxs-lookup"><span data-stu-id="f5df2-115">Requirements</span></span>  
 <span data-ttu-id="f5df2-116">**Plateformes :** consultez [requise](../../../../docs/framework/get-started/system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="f5df2-116">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="f5df2-117">**En-tête :** CorDebug.idl, CorDebug.h</span><span class="sxs-lookup"><span data-stu-id="f5df2-117">**Header:** CorDebug.idl, CorDebug.h</span></span>  
  
 <span data-ttu-id="f5df2-118">**Bibliothèque :** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="f5df2-118">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="f5df2-119">**Versions du .NET framework :**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="f5df2-119">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>
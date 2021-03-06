---
title: Threading managé et non managé dans Windows
ms.custom: ''
ms.date: 03/30/2017
ms.prod: .net
ms.reviewer: ''
ms.suite: ''
ms.technology: dotnet-standard
ms.tgt_pltfrm: ''
ms.topic: article
helpviewer_keywords:
- threading [.NET Framework], unmanaged
- threading [.NET Framework], managed
- managed threading
ms.assetid: 4fb6452f-c071-420d-9e71-da16dee7a1eb
caps.latest.revision: 17
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.workload:
- dotnet
- dotnetcore
ms.openlocfilehash: 66bf8458a3f4f9dd622129e82acb659dddf8467a
ms.sourcegitcommit: 2e8acae16ae802f2d6d04e3ce0a6dbf04e476513
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
---
# <a name="managed-and-unmanaged-threading-in-windows"></a>Threading managé et non managé dans Windows
La gestion de tous les threads s'effectue par le biais de la classe <xref:System.Threading.Thread> , notamment les threads créés par le Common Language Runtime et ceux créés en dehors du runtime qui entrent dans l'environnement managé pour exécuter du code. Le runtime surveille tous les threads dans son processus qui ont exécuté du code dans l'environnement d'exécution managé. Il n'effectue le suivi d'aucun autre thread. Les threads peuvent entrer dans l’environnement d’exécution managé par le biais du service COM Interop (car le runtime expose les objets managés en tant qu’objets COM à l’environnement non managé), de la fonction COM [DllGetClassObject](https://msdn.microsoft.com/library/ms680760.aspx) et de l’appel de code non managé.  
  
 Quand un thread non managé entre dans le runtime via, par exemple, un wrapper CCW (COM Callable Wrapper), le système vérifie si le magasin de threads local de ce thread contient un objet <xref:System.Threading.Thread> managé interne. Si un objet de ce type est trouvé, le runtime connaît déjà ce thread. Sinon, le runtime crée quand même un objet <xref:System.Threading.Thread> et l’installe dans le magasin de threads local de ce thread.  
  
 Dans le modèle de thread managé, <xref:System.Threading.Thread.GetHashCode%2A?displayProperty=nameWithType> identifie les threads managés de manière stable. Durant toute la durée de vie de votre thread, cette identification n'entre en conflit avec la valeur d'aucun autre thread, quel que soit le domaine d'application à partir duquel vous obtenez cette valeur.  
  
> [!NOTE]
>  Un **ID de thread** de système d'exploitation n'est pas lié de manière fixe à un thread managé, car un hôte non managé peut contrôler la relation entre les threads managés et les threads non managés. En particulier, un hôte élaboré peut utiliser l'API Fiber pour planifier de nombreux threads managés par rapport au même thread de système d'exploitation, ou pour déplacer un thread managé parmi différents threads de système d'exploitation.  
  
## <a name="mapping-from-win32-threading-to-managed-threading"></a>Correspondance entre les éléments de thread Win32 et les éléments de thread managés  
 Le tableau suivant établit une correspondance entre les éléments de thread Win32 et leurs équivalents approximatifs dans le runtime. Notez que cette mise en correspondance ne représente pas des fonctionnalités identiques. Par exemple, **TerminateThread** n'exécute pas de clauses **finally** ou ne libère pas de ressources, et son exécution ne peut pas être empêchée. Toutefois, <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> exécute tout votre code de restauration, récupère toutes les ressources et peut être refusé à l'aide de <xref:System.Threading.Thread.ResetAbort%2A>. Veillez à lire la documentation attentivement avant d'émettre des hypothèses sur les fonctionnalités.  
  
|Dans Win32|Dans le Common Language Runtime|  
|--------------|------------------------------------|  
|**CreateThread**|Combinaison de **Thread** et <xref:System.Threading.ThreadStart>|  
|**TerminateThread**|<xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>|  
|**SuspendThread**|<xref:System.Threading.Thread.Suspend%2A?displayProperty=nameWithType>|  
|**ResumeThread**|<xref:System.Threading.Thread.Resume%2A?displayProperty=nameWithType>|  
|**Sleep**|<xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType>|  
|**WaitForSingleObject** sur le handle de thread|<xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType>|  
|**ExitThread**|Aucun équivalent|  
|**GetCurrentThread**|<xref:System.Threading.Thread.CurrentThread%2A?displayProperty=nameWithType>|  
|**SetThreadPriority**|<xref:System.Threading.Thread.Priority%2A?displayProperty=nameWithType>|  
|Aucun équivalent|<xref:System.Threading.Thread.Name%2A?displayProperty=nameWithType>|  
|Aucun équivalent|<xref:System.Threading.Thread.IsBackground%2A?displayProperty=nameWithType>|  
|Proche de **CoInitializeEx** (OLE32.DLL)|<xref:System.Threading.Thread.ApartmentState%2A?displayProperty=nameWithType>|  
  
## <a name="managed-threads-and-com-apartments"></a>Threads managés et cloisonnements COM  
 Un thread managé peut être marqué pour indiquer qu’il hébergera un [thread unique cloisonné](https://msdn.microsoft.com/library/windows/desktop/ms680112.aspx) ou un [multithread cloisonné](https://msdn.microsoft.com/library/windows/desktop/ms693421.aspx). (Pour plus d’informations sur l’architecture des threads COM, consultez l’article [Processes, threads, and Apartments](https://msdn.microsoft.com/library/windows/desktop/ms693344.aspx) (Processus, threads et cloisonnements).) Les méthodes <xref:System.Threading.Thread.GetApartmentState%2A>, <xref:System.Threading.Thread.SetApartmentState%2A> et <xref:System.Threading.Thread.TrySetApartmentState%2A> de la classe <xref:System.Threading.Thread> retournent et affectent l'état de cloisonnement d'un thread. Si l'état n’a pas été défini, <xref:System.Threading.Thread.GetApartmentState%2A> retourne <xref:System.Threading.ApartmentState.Unknown?displayProperty=nameWithType>.  
  
 La propriété ne peut être définie que quand le thread se trouve dans l’état <xref:System.Threading.ThreadState.Unstarted?displayProperty=nameWithType> et qu’une seule fois par thread.  
  
 Si l'état de cloisonnement n'est pas défini avant que le thread n'ait démarré, celui-ci est initialisé en tant que cloisonnement multithread (MTA). Le thread finaliseur et tous les threads contrôlés par <xref:System.Threading.ThreadPool> sont des threads MTA.  
  
> [!IMPORTANT]
>  Pour le code de démarrage d'application, la seule façon de contrôler l'état de cloisonnement consiste à appliquer les attributs <xref:System.MTAThreadAttribute> ou <xref:System.STAThreadAttribute> à la procédure de point d'entrée. Dans .NET Framework 1.0 et 1.1, la propriété <xref:System.Threading.Thread.ApartmentState%2A> peut être définie en tant que première ligne de code. Cette opération n'est pas autorisée dans .NET Framework 2.0.  
  
 Les objets managés exposés à COM se comportent comme s'ils avaient agrégé le marshaleur libre de threads. En d'autres termes, ils peuvent être appelés depuis n'importe quel cloisonnement COM d'une manière libre de threads. Les seuls objets managés qui ne présentent pas ce comportement libre de threads sont les objets qui dérivent de <xref:System.EnterpriseServices.ServicedComponent> ou <xref:System.Runtime.InteropServices.StandardOleMarshalObject>.  
  
 Dans l'environnement managé, l'attribut <xref:System.Runtime.Remoting.Contexts.SynchronizationAttribute> n'est pas pris en charge, sauf si vous utilisez des contextes et des instances managées liées au contexte. Si vous utilisez Enterprise Services, votre objet doit dériver de <xref:System.EnterpriseServices.ServicedComponent> (lui-même dérivé de <xref:System.ContextBoundObject>).  
  
 Quand un code managé appelle des objets COM, il suit systématiquement les règles COM. En d'autres termes, il effectue les appels par le biais de proxys de cloisonnement COM et de wrappers de contexte COM+ 1.0, conformément à OLE32.  
  
## <a name="blocking-issues"></a>Problèmes de blocage  
 Si un thread effectue dans le système d'exploitation un appel non managé et qu'il se retrouve bloqué dans un code non managé, le runtime ne prend pas le contrôle du thread pour <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> ou <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>. Dans le cas de <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>, le runtime marque le thread comme **Abandonné** et en prend le contrôle quand il revient dans le code managé. Pensez à utiliser un blocage managé plutôt qu'un blocage non managé. <xref:System.Threading.WaitHandle.WaitOne%2A?displayProperty=nameWithType>,<xref:System.Threading.WaitHandle.WaitAny%2A?displayProperty=nameWithType>, <xref:System.Threading.WaitHandle.WaitAll%2A?displayProperty=nameWithType>, <xref:System.Threading.Monitor.Enter%2A?displayProperty=nameWithType>, <xref:System.Threading.Monitor.TryEnter%2A?displayProperty=nameWithType>, <xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType>, <xref:System.GC.WaitForPendingFinalizers%2A?displayProperty=nameWithType>, etc. réagissent tous à <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> et à <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>. En outre, si votre thread se trouve dans un cloisonnement monothread, toutes ces opérations de blocage managé pompent correctement les messages dans votre cloisonnement pendant que votre thread est bloqué.  
  
## <a name="see-also"></a>Voir aussi  
 <xref:System.Threading.Thread.ApartmentState%2A?displayProperty=nameWithType>  
 <xref:System.Threading.ThreadState>  
 <xref:System.EnterpriseServices.ServicedComponent>  
 <xref:System.Threading.Thread>  
 <xref:System.Threading.Monitor>

---
title: "Stratégies de gestion des défaillances partielles"
description: "Architecture de Microservices .NET pour les Applications .NET en conteneur | Stratégies de gestion des défaillances partielles"
keywords: Docker, microservices, ASP.NET, conteneur
author: CESARDELATORRE
ms.author: wiwagn
ms.date: 05/26/2017
ms.prod: .net-core
ms.technology: dotnet-docker
ms.topic: article
ms.openlocfilehash: ff3bed530b13a9b1822c7cccf5a4d47df6fc6239
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2017
---
# <a name="strategies-for-handling-partial-failure"></a><span data-ttu-id="fbd4d-104">Stratégies de gestion des défaillances partielles</span><span class="sxs-lookup"><span data-stu-id="fbd4d-104">Strategies for handling partial failure</span></span>

<span data-ttu-id="fbd4d-105">Stratégies pour gérer les défaillances partielles sont les suivantes.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-105">Strategies for dealing with partial failures include the following.</span></span>

<span data-ttu-id="fbd4d-106">**Utiliser la communication asynchrone (par exemple, la communication basée sur message) sur microservices interne**.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-106">**Use asynchronous communication (for example, message-based communication) across internal microservices**.</span></span> <span data-ttu-id="fbd4d-107">Il est vivement conseillé ne pas de créer des longues chaînes d’appels HTTP synchrones entre la microservices interne, car cette conception incorrecte cesser la cause principale des pannes incorrects.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-107">It is highly advisable not to create long chains of synchronous HTTP calls across the internal microservices because that incorrect design will eventually become the main cause of bad outages.</span></span> <span data-ttu-id="fbd4d-108">Au contraire, à l’exception des communications entre les applications clientes et de premier niveau de microservices ou des passerelles API affinée frontales, il est recommandé d’utiliser uniquement (basée sur le message) communication asynchrone qu’une seule fois après la demande initiale / cycle de réponse, entre le microservices interne.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-108">On the contrary, except for the front-end communications between the client applications and the first level of microservices or fine-grained API Gateways, it is recommended to use only asynchronous (message-based) communication once past the initial request/response cycle, across the internal microservices.</span></span> <span data-ttu-id="fbd4d-109">Cohérence éventuelle et les architectures de pilotée par événements permettent de réduire les répercussions.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-109">Eventual consistency and event-driven architectures will help to minimize ripple effects.</span></span> <span data-ttu-id="fbd4d-110">Ces approches appliquent un niveau plus élevé d’autonomie de microservice et par conséquent éviter le problème indiqué ici.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-110">These approaches enforce a higher level of microservice autonomy and therefore prevent against the problem noted here.</span></span>

<span data-ttu-id="fbd4d-111">**Utiliser les nouvelles tentatives avec interruption exponentielle**.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-111">**Use retries with exponential backoff**.</span></span> <span data-ttu-id="fbd4d-112">Cette technique permet d’éviter un court et échecs intermittents en effectuant l’appel de tentatives d’un certain nombre de fois, au cas où le service n’était pas disponible uniquement pour une courte période.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-112">This technique helps to avoid short and intermittent failures by performing call retries a certain number of times, in case the service was not available only for a short time.</span></span> <span data-ttu-id="fbd4d-113">Cela peut se produire en raison de problèmes réseau intermittents ou lorsqu’un microservice/conteneur est déplacé vers un autre nœud dans un cluster.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-113">This might occur due to intermittent network issues or when a microservice/container is moved to a different node in a cluster.</span></span> <span data-ttu-id="fbd4d-114">Toutefois, si ces nouvelles tentatives ne sont pas conçus correctement avec les disjoncteurs, il peut aggraver les effets ripple, finalement même entraînant un [par déni de Service (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack).</span><span class="sxs-lookup"><span data-stu-id="fbd4d-114">However, if these retries are not designed properly with circuit breakers, it can aggravate the ripple effects, ultimately even causing a [Denial of Service (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack).</span></span>

<span data-ttu-id="fbd4d-115">**Pour contourner les délais d’attente réseau**.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-115">**Work around network timeouts**.</span></span> <span data-ttu-id="fbd4d-116">En règle générale, les clients doivent être conçus pour ne pas se bloquer indéfiniment et à toujours utiliser des délais d’attente pendant l’attente d’une réponse.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-116">In general, clients should be designed not to block indefinitely and to always use timeouts when waiting for a response.</span></span> <span data-ttu-id="fbd4d-117">À l’aide de délais d’attente garantit que ressources sont jamais liés indéfiniment.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-117">Using timeouts ensures that resources are never tied up indefinitely.</span></span>

<span data-ttu-id="fbd4d-118">**Utilisez le modèle de disjoncteur**.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-118">**Use the Circuit Breaker pattern**.</span></span> <span data-ttu-id="fbd4d-119">Dans cette approche, le processus client suit le nombre de demandes ayant échoué.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-119">In this approach, the client process tracks the number of failed requests.</span></span> <span data-ttu-id="fbd4d-120">Si le taux d’erreur dépasse la limite configurée, un aller-retour « disjoncteur » afin que d’autres tentatives échouent immédiatement.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-120">If the error rate exceeds a configured limit, a “circuit breaker” trips so that further attempts fail immediately.</span></span> <span data-ttu-id="fbd4d-121">(Si un grand nombre de demandes échouent, qui propose le service n’est pas disponible et qu’il est inutile d’envoyer des requêtes.) Après une période de délai d’attente, le client doit réessayer et, si les nouvelles requêtes réussissent, fermez le disjoncteur.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-121">(If a large number of requests are failing, that suggests the service is unavailable and that sending requests is pointless.) After a timeout period, the client should try again and, if the new requests are successful, close the circuit breaker.</span></span>

<span data-ttu-id="fbd4d-122">**Fournir de secours**.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-122">**Provide fallbacks**.</span></span> <span data-ttu-id="fbd4d-123">Dans cette approche, le processus client exécute la logique de secours en cas d’échec d’une demande, comme le retour de données mises en cache ou une valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-123">In this approach, the client process performs fallback logic when a request fails, such as returning cached data or a default value.</span></span> <span data-ttu-id="fbd4d-124">Est une approche appropriée pour les requêtes, il est plus complexe pour les mises à jour ou de commandes.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-124">This is an approach suitable for queries, and is more complex for updates or commands.</span></span>

<span data-ttu-id="fbd4d-125">**Limiter le nombre de demandes en file d’attente**.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-125">**Limit the number of queued requests**.</span></span> <span data-ttu-id="fbd4d-126">Les clients doivent également imposer une limite supérieure du nombre de demandes en suspens qui un microservice client peut envoyer à un service particulier.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-126">Clients should also impose an upper bound on the number of outstanding requests that a client microservice can send to a particular service.</span></span> <span data-ttu-id="fbd4d-127">Si la limite est atteinte, il est probablement inutile de faire des demandes supplémentaires, et ces tentatives doivent échouer immédiatement.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-127">If the limit has been reached, it is probably pointless to make additional requests, and those attempts should fail immediately.</span></span> <span data-ttu-id="fbd4d-128">En termes d’implémentation, la Polly [cloisonnement Isolation](https://github.com/App-vNext/Polly/wiki/Bulkhead) stratégie peut être utilisée pour satisfaire à cette exigence.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-128">In terms of implementation, the Polly [Bulkhead Isolation](https://github.com/App-vNext/Polly/wiki/Bulkhead) policy can be used to fulfil this requirement.</span></span> <span data-ttu-id="fbd4d-129">Cette approche est essentiellement une limitation de la parallélisation avec [SemaphoreSlim](https://docs.microsoft.com/dotnet/api/system.threading.semaphoreslim?view=netcore-1.1) comme implémentation.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-129">This approach is essentially a parallelization throttle with [SemaphoreSlim](https://docs.microsoft.com/dotnet/api/system.threading.semaphoreslim?view=netcore-1.1) as the implementation.</span></span> <span data-ttu-id="fbd4d-130">Il permet également à un « queue » à l’extérieur du cloisonnement.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-130">It also permits a "queue" outside the bulkhead.</span></span> <span data-ttu-id="fbd4d-131">Vous pouvez proactive ont une charge excessive même avant l’exécution (par exemple, étant donné que la capacité est considéré comme complet).</span><span class="sxs-lookup"><span data-stu-id="fbd4d-131">You can proactively shed excess load even before execution (for example, because capacity is deemed full).</span></span> <span data-ttu-id="fbd4d-132">Cela rend plus rapidement qu’un disjoncteur serait, étant donné que le disjoncteur attend que les échecs de sa réponse à certains scénarios d’échec.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-132">This makes its response to certain failure scenarios faster than a circuit breaker would be, since the circuit breaker waits for the failures.</span></span> <span data-ttu-id="fbd4d-133">L’objet BulkheadPolicy dans Polly expose quelle le cloisonnement sont de la file d’attente et offre des événements de dépassement de capacité par conséquent, peuvent également servir pour piloter la mise à l’échelle horizontale automatisée.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-133">The BulkheadPolicy object in Polly exposes how full the bulkhead and queue are, and offers events on overflow so can also be used to drive automated horizontal scaling.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fbd4d-134">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="fbd4d-134">Additional resources</span></span>

-   <span data-ttu-id="fbd4d-135">**Modèles de résilience**
    [*https://docs.microsoft.com/azure/architecture/patterns/category/resiliency*](https://docs.microsoft.com/azure/architecture/patterns/category/resiliency)</span><span class="sxs-lookup"><span data-stu-id="fbd4d-135">**Resiliency patterns**
[*https://docs.microsoft.com/azure/architecture/patterns/category/resiliency*](https://docs.microsoft.com/azure/architecture/patterns/category/resiliency)</span></span>

-   <span data-ttu-id="fbd4d-136">**Ajout de résilience et optimisation des performances**
    [*https://msdn.microsoft.com/en-us/library/jj591574.aspx*](https://msdn.microsoft.com/en-us/library/jj591574.aspx)</span><span class="sxs-lookup"><span data-stu-id="fbd4d-136">**Adding Resilience and Optimizing Performance**
[*https://msdn.microsoft.com/en-us/library/jj591574.aspx*](https://msdn.microsoft.com/en-us/library/jj591574.aspx)</span></span>

-   <span data-ttu-id="fbd4d-137">**Cloisonnement.**</span><span class="sxs-lookup"><span data-stu-id="fbd4d-137">**Bulkhead.**</span></span> <span data-ttu-id="fbd4d-138">Référentiel GitHub.</span><span class="sxs-lookup"><span data-stu-id="fbd4d-138">GitHub repo.</span></span> <span data-ttu-id="fbd4d-139">Implémentation de la stratégie de Polly. \\</span><span class="sxs-lookup"><span data-stu-id="fbd4d-139">Implementation with Polly policy.\\</span></span>
    [<span data-ttu-id="fbd4d-140">*https://github.com/app-vNext/Polly/Wiki/Bulkhead*</span><span class="sxs-lookup"><span data-stu-id="fbd4d-140">*https://github.com/App-vNext/Polly/wiki/Bulkhead*</span></span>](https://github.com/App-vNext/Polly/wiki/Bulkhead)

-   <span data-ttu-id="fbd4d-141">**Conception d’applications résilientes Azure**
    [*https://docs.microsoft.com/azure/architecture/resiliency/*](https://docs.microsoft.com/azure/architecture/resiliency/)</span><span class="sxs-lookup"><span data-stu-id="fbd4d-141">**Designing resilient applications for Azure**
[*https://docs.microsoft.com/azure/architecture/resiliency/*](https://docs.microsoft.com/azure/architecture/resiliency/)</span></span>

-   <span data-ttu-id="fbd4d-142">**Gestion d’erreurs transitoires**
    <https://docs.microsoft.com/azure/architecture/best-practices/transient-faults></span><span class="sxs-lookup"><span data-stu-id="fbd4d-142">**Transient fault handling**
<https://docs.microsoft.com/azure/architecture/best-practices/transient-faults></span></span>


>[!div class="step-by-step"]
<span data-ttu-id="fbd4d-143">[Précédente] (handle-partielle-failure.md) [suivant] (implémentez-tentatives-exponentielle-backoff.md)</span><span class="sxs-lookup"><span data-stu-id="fbd4d-143">[Previous] (handle-partial-failure.md) [Next] (implement-retries-exponential-backoff.md)</span></span>
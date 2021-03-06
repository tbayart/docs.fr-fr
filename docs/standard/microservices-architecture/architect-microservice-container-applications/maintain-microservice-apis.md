---
title: "Création, évolution et version des contrats et des API de microservices"
description: "Architecture en microservices .NET pour les applications .NET en conteneur | Création, évolution et version des contrats et des API de microservices"
keywords: Docker, microservices, ASP.NET, conteneur
author: CESARDELATORRE
ms.author: wiwagn
ms.date: 05/26/2017
ms.prod: .net-core
ms.technology: dotnet-docker
ms.topic: article
ms.workload:
- dotnet
- dotnetcore
ms.openlocfilehash: 3aaa7eaa8bc535874369cf08414f2211cae1bed9
ms.sourcegitcommit: e7f04439d78909229506b56935a1105a4149ff3d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/23/2017
---
# <a name="creating-evolving-and-versioning-microservice-apis-and-contracts"></a>Création, évolution et version des contrats et des API de microservices

Une API de microservice est un contrat entre le service et ses clients. Vous ne pouvez pas faire évoluer indépendamment un microservice si son contrat API est rompu. C’est pourquoi le contrat est si important. Tout changement apporté au contrat impacte vos applications clientes ou votre passerelle API.

La nature de la définition de l’API dépend du protocole que vous utilisez. Par exemple, si vous utilisez une messagerie (comme [AMQP](https://www.amqp.org/)), l’API comprend les types de messages. Si vous utilisez des services HTTP et RESTful, l’API comprend les URL et les formats JSON de requête et de réponse.

Cependant, même si votre contrat initial est l’aboutissement d’une réflexion, une API de service est contrainte de changer au fil du temps. Quand ce moment arrive, vous ne pouvez généralement pas forcer tous les clients à passer à votre nouveau contrat d’API, surtout s’il s’agit d’une API publique utilisée par plusieurs applications clientes. Dans la plupart des cas, vous devez déployer par incrément les nouvelles versions d’un service en faisant en sorte que les anciennes et les nouvelles versions du contrat de service s’exécutent simultanément. Il est donc important de disposer d’une stratégie pour la gestion des versions de votre service.

Si vous apportez des changements minimes à l’API, comme l’ajout d’attributs ou de paramètres, il est recommandé que les clients qui utilisent une API antérieure passent à la nouvelle version du service. Il est possible que vous puissiez fournir des valeurs par défaut pour tous les attributs obligatoires manquants et que les clients ignorent les attributs de réponse en trop.

Toutefois, vous pouvez parfois être amené à apporter des changements majeurs à une API de service qui débouchent sur des incompatibilités. Dans l’éventualité où vous ne pourriez pas forcer les applications ou services clients à passer tout de suite à la nouvelle version, un service doit prendre en charge les anciennes versions de l’API pendant un certain temps. Si vous utilisez un mécanisme basé sur HTTP tel que REST, une approche consiste à incorporer le numéro de version de l’API dans l’URL ou dans un en-tête HTTP. Vous pouvez ensuite soit implémenter simultanément les deux versions du service dans la même instance de service, soit déployer des instances distinctes gérant des versions différentes de l’API. Une bonne approche consiste à utiliser le [modèle Médiateur](https://en.wikipedia.org/wiki/Mediator_pattern) (par exemple, la [bibliothèque MediatR](https://github.com/jbogard/MediatR)) pour découpler les versions de l’implémentation en gestionnaires indépendants.

Enfin, si vous utilisez une architecture REST, [Hypermedia](https://www.infoq.com/articles/mark-baker-hypermedia) constitue la meilleure solution pour la gestion des versions de vos services et la prise en charge d’API évolutives.

## <a name="additional-resources"></a>Ressources supplémentaires

-   **Scott Hanselman. ASP.NET Core RESTful Web API versioning made easy**
    <http://www.hanselman.com/blog/ASPNETCoreRESTfulWebAPIVersioningMadeEasy.aspx>

-   **Gestion de version d’une API web RESTful**
    [*https://docs.microsoft.com/azure/architecture/best-practices/api-design#versioning-a-restful-web-api*](https://docs.microsoft.com/azure/architecture/best-practices/api-design#versioning-a-restful-web-api)

-   **Roy Fielding. Versioning, Hypermedia, and REST**
    <https://www.infoq.com/articles/roy-fielding-on-versioning>


>[!div class="step-by-step"]
[Précédent] (asynchronous-message-based-communication.md) [Suivant] (microservices-addressability-service-registry.md)

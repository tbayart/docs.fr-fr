---
title: "Communication asynchrone basée sur message"
description: "Architecture de Microservices .NET pour les Applications .NET en conteneur | Communication asynchrone basée sur message"
keywords: Docker, microservices, ASP.NET, conteneur
author: CESARDELATORRE
ms.author: wiwagn
ms.date: 05/26/2017
ms.prod: .net-core
ms.technology: dotnet-docker
ms.topic: article
ms.openlocfilehash: df39771295d12e122edbe27e91cd899e3318e301
ms.sourcegitcommit: c2e216692ef7576a213ae16af2377cd98d1a67fa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/22/2017
---
# <a name="asynchronous-message-based-communication"></a><span data-ttu-id="502be-104">Communication asynchrone basée sur message</span><span class="sxs-lookup"><span data-stu-id="502be-104">Asynchronous message-based communication</span></span>

<span data-ttu-id="502be-105">Messagerie asynchrone et pilotés par les événements de communication sont critiques lors de la propagation des modifications sur plusieurs microservices et leurs modèles de domaine connexes.</span><span class="sxs-lookup"><span data-stu-id="502be-105">Asynchronous messaging and event-driven communication are critical when propagating changes across multiple microservices and their related domain models.</span></span> <span data-ttu-id="502be-106">Comme mentionné précédemment dans la discussion microservices et contextes délimitée (BCs), les modèles (utilisateur, client, produit, compte, etc.) peuvent signifier différentes choses pour différentes microservices ou BCs.</span><span class="sxs-lookup"><span data-stu-id="502be-106">As mentioned earlier in the discussion microservices and Bounded Contexts (BCs), models (User, Customer, Product, Account, etc.) can mean different things to different microservices or BCs.</span></span> <span data-ttu-id="502be-107">Cela signifie que lorsque des modifications se produisent, vous devez rapprocher les modifications dans les différents modèles.</span><span class="sxs-lookup"><span data-stu-id="502be-107">That means that when changes occur, you need some way to reconcile changes across the different models.</span></span> <span data-ttu-id="502be-108">Une solution est la cohérence éventuelle et communication pilotée par événements en fonction de la messagerie asynchrone.</span><span class="sxs-lookup"><span data-stu-id="502be-108">A solution is eventual consistency and event-driven communication based on asynchronous messaging.</span></span>

<span data-ttu-id="502be-109">Lorsque vous utilisez la messagerie, les processus communiquent en échangeant des messages de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="502be-109">When using messaging, processes communicate by exchanging messages asynchronously.</span></span> <span data-ttu-id="502be-110">Un client établit une commande ou une demande à un service en l’envoyant un message.</span><span class="sxs-lookup"><span data-stu-id="502be-110">A client makes a command or a request to a service by sending it a message.</span></span> <span data-ttu-id="502be-111">Si le service doit répondre, il envoie un autre message au client.</span><span class="sxs-lookup"><span data-stu-id="502be-111">If the service needs to reply, it sends a different message back to the client.</span></span> <span data-ttu-id="502be-112">Dans la mesure où il s’agit d’une communication basée sur message, le client suppose que la réponse ne reçoit pas immédiatement, et qu’il ne peut y avoir aucune réponse du tout.</span><span class="sxs-lookup"><span data-stu-id="502be-112">Since it is a message-based communication, the client assumes that the reply will not be received immediately, and that there might be no response at all.</span></span>

<span data-ttu-id="502be-113">Un message est composé d’un en-tête (métadonnées telles que les informations d’identification ou de sécurité) et un corps.</span><span class="sxs-lookup"><span data-stu-id="502be-113">A message is composed by a header (metadata such as identification or security information) and a body.</span></span> <span data-ttu-id="502be-114">Les messages sont généralement envoyés via les protocoles asynchrones comme AMQP.</span><span class="sxs-lookup"><span data-stu-id="502be-114">Messages are usually sent through asynchronous protocols like AMQP.</span></span>

<span data-ttu-id="502be-115">L’infrastructure préféré pour ce type de communication au sein de la Communauté microservices est un léger de message service broker, qui est différent de celle de la grandes courtiers et orchestrators utilisées dans SOA.</span><span class="sxs-lookup"><span data-stu-id="502be-115">The preferred infrastructure for this type of communication in the microservices community is a lightweight message broker, which is different than the large brokers and orchestrators used in SOA.</span></span> <span data-ttu-id="502be-116">Dans un service broker de message léger, l’infrastructure est en général « passif, « agissent uniquement comme un courtier de messages avec des implémentations simples telles que RabbitMQ ou un bus des services évolutifs dans le cloud comme Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="502be-116">In a lightweight message broker, the infrastructure is typically “dumb,” acting only as a message broker, with simple implementations such as RabbitMQ or a scalable service bus in the cloud like Azure Service Bus.</span></span> <span data-ttu-id="502be-117">Dans ce scénario, la majeure partie de l’approche « intelligente » se trouve toujours dans les points de terminaison qui sont de production et de consommer des messages, autrement dit, dans le microservices.</span><span class="sxs-lookup"><span data-stu-id="502be-117">In this scenario, most of the “smart” thinking still lives in the endpoints that are producing and consuming messages—that is, in the microservices.</span></span>

<span data-ttu-id="502be-118">Une autre règle, qu'il est conseillé de suivre autant que possible, consiste à utiliser la messagerie asynchrone uniquement entre les services internes et pour utiliser la communication synchrone (par exemple, HTTP) uniquement à partir des applications client pour les services frontaux (passerelles API ainsi que le premier niveau de microservices).</span><span class="sxs-lookup"><span data-stu-id="502be-118">Another rule you should try to follow, as much as possible, is to use only asynchronous messaging between the internal services, and to use synchronous communication (such as HTTP) only from the client apps to the front-end services (API Gateways plus the first level of microservices).</span></span>

<span data-ttu-id="502be-119">Il existe deux types de communication asynchrone : récepteur unique basée sur message communications et plusieurs récepteurs basée sur message.</span><span class="sxs-lookup"><span data-stu-id="502be-119">There are two kinds of asynchronous messaging communication: single receiver message-based communication, and multiple receivers message-based communication.</span></span> <span data-ttu-id="502be-120">Dans les sections suivantes, nous fournir plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="502be-120">In the following sections we provide details about them.</span></span>

## <a name="single-receiver-message-based-communication"></a><span data-ttu-id="502be-121">Communication de messagerie unique-récepteur</span><span class="sxs-lookup"><span data-stu-id="502be-121">Single-receiver message-based communication</span></span> 

<span data-ttu-id="502be-122">Une communication asynchrone basée sur message avec un seul destinataire signifie communication point à point qui remet un message à un seul des consommateurs qui lit à partir du canal, et que le message est traité une seule fois.</span><span class="sxs-lookup"><span data-stu-id="502be-122">Message-based asynchronous communication with a single receiver means there is point-to-point communication that delivers a message to exactly one of the consumers that is reading from the channel, and that the message is processed just once.</span></span> <span data-ttu-id="502be-123">Toutefois, il existe des situations particulières.</span><span class="sxs-lookup"><span data-stu-id="502be-123">However, there are special situations.</span></span> <span data-ttu-id="502be-124">Par exemple, dans un système de cloud qui tente de récupérer automatiquement des défaillances, le même message peut être envoyé plusieurs fois.</span><span class="sxs-lookup"><span data-stu-id="502be-124">For instance, in a cloud system that tries to automatically recover from failures, the same message could be sent multiple times.</span></span> <span data-ttu-id="502be-125">En raison de réseau ou d’autres défaillances, le client doit être en mesure de nouvelles tentatives d’envoi de messages, et le serveur doit implémenter une opération idempotente afin de traiter un message particulier qu’une seule fois.</span><span class="sxs-lookup"><span data-stu-id="502be-125">Due to network or other failures, the client has to be able to retry sending messages, and the server has to implement an operation to be idempotent in order to process a particular message just once.</span></span>

<span data-ttu-id="502be-126">Communication de messagerie unique-récepteur est particulièrement bien adaptée pour l’envoi de commandes asynchrones à partir d’un microservice à un autre comme indiqué dans la Figure 4-18 qui illustre cette approche.</span><span class="sxs-lookup"><span data-stu-id="502be-126">Single-receiver message-based communication is especially well suited for sending asynchronous commands from one microservice to another as shown in Figure 4-18 that illustrates this approach.</span></span>

<span data-ttu-id="502be-127">Une fois que vous démarrez l’envoi de communication basée sur message (soit à des commandes ou des événements), vous devez éviter de mélanger les communication basée sur le message avec la communication HTTP synchrone.</span><span class="sxs-lookup"><span data-stu-id="502be-127">Once you start sending message-based communication (either with commands or events), you should avoid mixing message-based communication with synchronous HTTP communication.</span></span>

![](./media/image18.PNG)

<span data-ttu-id="502be-128">**Figure 4-18**.</span><span class="sxs-lookup"><span data-stu-id="502be-128">**Figure 4-18**.</span></span> <span data-ttu-id="502be-129">Un seul microservice réception d’un message asynchrone</span><span class="sxs-lookup"><span data-stu-id="502be-129">A single microservice receiving an asynchronous message</span></span>

<span data-ttu-id="502be-130">Notez que, lorsque les commandes proviennent des applications clientes, ils peuvent être implémentés en tant que les commandes synchrones HTTP.</span><span class="sxs-lookup"><span data-stu-id="502be-130">Note that when the commands come from client applications, they can be implemented as HTTP synchronous commands.</span></span> <span data-ttu-id="502be-131">Vous devez utiliser basée sur message des commandes lorsque vous avez besoin d’une évolutivité plus élevée ou si vous êtes déjà dans un processus d’entreprise basé sur le message.</span><span class="sxs-lookup"><span data-stu-id="502be-131">You should use message-based commands when you need higher scalability or when you are already in a message-based business process.</span></span>

## <a name="multiple-receivers-message-based-communication"></a><span data-ttu-id="502be-132">Communication basée sur message de récepteurs de multiples</span><span class="sxs-lookup"><span data-stu-id="502be-132">Multiple-receivers message-based communication</span></span> 

<span data-ttu-id="502be-133">Une approche plus souple, vous souhaiterez également utiliser un mécanisme de publication/abonnement afin que votre communication de l’expéditeur sera disponible pour les microservices supplémentaires pour les abonnés ou des applications externes.</span><span class="sxs-lookup"><span data-stu-id="502be-133">As a more flexible approach, you might also want to use a publish/subscribe mechanism so that your communication from the sender will be available to additional subscriber microservices or to external applications.</span></span> <span data-ttu-id="502be-134">Par conséquent, il vous aide à suivre le [principe d’ouverture/fermeture](https://en.wikipedia.org/wiki/Open/closed_principle) dans le service d’envoi.</span><span class="sxs-lookup"><span data-stu-id="502be-134">Thus, it helps you to follow the [open/closed principle](https://en.wikipedia.org/wiki/Open/closed_principle) in the sending service.</span></span> <span data-ttu-id="502be-135">De cette façon, les abonnés supplémentaires peuvent être ajoutés à l’avenir sans avoir besoin de modifier le service de l’expéditeur.</span><span class="sxs-lookup"><span data-stu-id="502be-135">That way, additional subscribers can be added in the future without the need to modify the sender service.</span></span>

<span data-ttu-id="502be-136">Lorsque vous utilisez une communication de publication/abonnement, vous utilisez peut-être interface du bus d’événements aux événements de publication à un abonné.</span><span class="sxs-lookup"><span data-stu-id="502be-136">When you use a publish/subscribe communication, you might be using an event bus interface to publish events to any subscriber.</span></span>

## <a name="asynchronous-event-driven-communication"></a><span data-ttu-id="502be-137">Communication asynchrone pilotée par événements</span><span class="sxs-lookup"><span data-stu-id="502be-137">Asynchronous event-driven communication</span></span>

<span data-ttu-id="502be-138">Lorsque vous utilisez une communication asynchrone commandé par événement, un microservice publie un événement de l’intégration lorsque quelque chose se produit au sein de son domaine et microservice un autre doit connaître, comme une modification de prix dans un microservice de catalogue de produits.</span><span class="sxs-lookup"><span data-stu-id="502be-138">When using asynchronous event-driven communication, a microservice publishes an integration event when something happens within its domain and another microservice needs to be aware of it, like a price change in a product catalog microservice.</span></span> <span data-ttu-id="502be-139">Microservices supplémentaires s’abonner aux événements afin qu’ils peuvent les recevoir en mode asynchrone.</span><span class="sxs-lookup"><span data-stu-id="502be-139">Additional microservices subscribe to the events so they can receive them asynchronously.</span></span> <span data-ttu-id="502be-140">Lorsque cela se produit, les destinataires peuvent mettre à jour leurs entités de domaine, ce qui peuvent entraîner plusieurs événements d’intégration à publier.</span><span class="sxs-lookup"><span data-stu-id="502be-140">When that happens, the receivers might update their own domain entities, which can cause more integration events to be published.</span></span> <span data-ttu-id="502be-141">Ce système de publication/abonnement est généralement effectué à l’aide d’une implémentation d’un bus d’événements.</span><span class="sxs-lookup"><span data-stu-id="502be-141">This publish/subscribe system is usually performed by using an implementation of an event bus.</span></span> <span data-ttu-id="502be-142">Le bus d’événements peut être conçu comme une abstraction ou une interface, avec l’API qui est nécessaire pour s’abonner ou annuler l’abonnement aux événements et publier des événements.</span><span class="sxs-lookup"><span data-stu-id="502be-142">The event bus can be designed as an abstraction or interface, with the API that is needed to subscribe or unsubscribe to events and to publish events.</span></span> <span data-ttu-id="502be-143">Le bus d’événements peut également avoir l’une ou plusieurs implémentations en fonction de n’importe quel broker entre les processus et de messagerie, telle qu’une file d’attente de messagerie ou le bus de service qui prend en charge les communications asynchrones et un modèle de publication/abonnement.</span><span class="sxs-lookup"><span data-stu-id="502be-143">The event bus can also have one or more implementations based on any inter-process and messaging broker, like a messaging queue or service bus that supports asynchronous communication and a publish/subscribe model.</span></span>

<span data-ttu-id="502be-144">Si un système utilise la cohérence éventuelle pilotée par les événements d’intégration, il est recommandé que cette approche sont complètement clair à l’utilisateur final.</span><span class="sxs-lookup"><span data-stu-id="502be-144">If a system uses eventual consistency driven by integration events, it is recommended that this approach be made completely clear to the end user.</span></span> <span data-ttu-id="502be-145">Le système ne doit pas utiliser une approche qui reproduit les événements d’intégration, telles que SignalR ou des systèmes d’interrogation du client.</span><span class="sxs-lookup"><span data-stu-id="502be-145">The system should not use an approach that mimics integration events, like SignalR or polling systems from the client.</span></span> <span data-ttu-id="502be-146">L’utilisateur final et le propriétaire de l’entreprise doivent explicitement adopter la cohérence éventuelle dans le système et notez que dans de nombreux cas l’entreprise n’a pas de problème avec cette approche, à condition qu’il soit explicite.</span><span class="sxs-lookup"><span data-stu-id="502be-146">The end user and the business owner have to explicitly embrace eventual consistency in the system and realize that in many cases the business does not have any problem with this approach, as long as it is explicit.</span></span>

<span data-ttu-id="502be-147">Comme indiqué précédemment dans le [défis et solutions pour la gestion de données distribuée](#challenges-and-solutions-for-distributed-data-management) section, vous pouvez utiliser des événements d’intégration pour implémenter des tâches d’entreprise qui s’étendent sur plusieurs microservices.</span><span class="sxs-lookup"><span data-stu-id="502be-147">As noted earlier in the [Challenges and solutions for distributed data management](#challenges-and-solutions-for-distributed-data-management) section, you can use integration events to implement business tasks that span multiple microservices.</span></span> <span data-ttu-id="502be-148">Vous aurez ainsi la cohérence éventuelle entre ces services.</span><span class="sxs-lookup"><span data-stu-id="502be-148">Thus you will have eventual consistency between those services.</span></span> <span data-ttu-id="502be-149">Une transaction cohérente est constituée d’une collection d’actions distribuées.</span><span class="sxs-lookup"><span data-stu-id="502be-149">An eventually consistent transaction is made up of a collection of distributed actions.</span></span> <span data-ttu-id="502be-150">À chaque action, le microservice connexe met à jour une entité de domaine et publie un autre événement d’intégration qui déclenche l’action suivante dans la même tâche professionnelle de bout en bout.</span><span class="sxs-lookup"><span data-stu-id="502be-150">At each action, the related microservice updates a domain entity and publishes another integration event that raises the next action within the same end-to-end business task.</span></span>

<span data-ttu-id="502be-151">Un point important est que vous pouvez souhaiter communiquer avec plusieurs microservices qui sont abonnés au même événement.</span><span class="sxs-lookup"><span data-stu-id="502be-151">An important point is that you might want to communicate to multiple microservices that are subscribed to the same event.</span></span> <span data-ttu-id="502be-152">Pour ce faire, vous pouvez utiliser la publication/abonnement de messagerie en fonction de communication pilotée par événements, comme indiqué dans la Figure 4-19.</span><span class="sxs-lookup"><span data-stu-id="502be-152">To do so, you can use publish/subscribe messaging based on event-driven communication, as shown in Figure 4-19.</span></span> <span data-ttu-id="502be-153">Ce mécanisme de publication/abonnement n’est pas exclusif à l’architecture de microservice.</span><span class="sxs-lookup"><span data-stu-id="502be-153">This publish/subscribe mechanism is not exclusive to the microservice architecture.</span></span> <span data-ttu-id="502be-154">Il est similaire à la façon dont [limitées de contextes](http://martinfowler.com/bliki/BoundedContext.html) dans DDD doit communiquer, ou à la façon dont vous propagez les mises à jour à partir de la base de données de l’écriture de la base de données en lecture la [commande et requête responsabilité répartition (CQRS)](http://martinfowler.com/bliki/CQRS.html)modèle d’architecture.</span><span class="sxs-lookup"><span data-stu-id="502be-154">It is similar to the way [Bounded Contexts](http://martinfowler.com/bliki/BoundedContext.html) in DDD should communicate, or to the way you propagate updates from the write database to the read database in the [Command and Query Responsibility Segregation (CQRS)](http://martinfowler.com/bliki/CQRS.html) architecture pattern.</span></span> <span data-ttu-id="502be-155">L’objectif est d’avoir cohérence éventuelle entre plusieurs sources de données sur votre système distribué.</span><span class="sxs-lookup"><span data-stu-id="502be-155">The goal is to have eventual consistency between multiple data sources across your distributed system.</span></span>

![](./media/image19.png)

<span data-ttu-id="502be-156">**Figure 4-19**.</span><span class="sxs-lookup"><span data-stu-id="502be-156">**Figure 4-19**.</span></span> <span data-ttu-id="502be-157">Communication par message asynchrone de pilotée par événements</span><span class="sxs-lookup"><span data-stu-id="502be-157">Asynchronous event-driven message communication</span></span>

<span data-ttu-id="502be-158">Votre implémentation de détermine ce que le protocole à utiliser pour les communications pilotée par événements, basée sur message.</span><span class="sxs-lookup"><span data-stu-id="502be-158">Your implementation will determine what protocol to use for event-driven, message-based communications.</span></span> <span data-ttu-id="502be-159">[AMQP](https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol) peuvent aider à atteindre une communication en file d’attente fiable.</span><span class="sxs-lookup"><span data-stu-id="502be-159">[AMQP](https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol) can help achieve reliable queued communication.</span></span>

<span data-ttu-id="502be-160">Lorsque vous utilisez un bus d’événements, vous souhaiterez peut-être utiliser un niveau d’abstraction (par exemple, une interface de bus d’événements) basé sur une implémentation connexe dans les classes avec le code à l’aide de l’API à partir d’un service broker de message comme [RabbitMQ](https://www.rabbitmq.com/) ou un service bus comme [Azure Service Bus avec rubriques](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="502be-160">When you use an event bus, you might want to use an abstraction level (like an event bus interface) based on a related implementation in classes with code using the API from a message broker like [RabbitMQ](https://www.rabbitmq.com/) or a service bus like [Azure Service Bus with Topics](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="502be-161">Vous souhaiterez également utiliser un bus de service de niveau supérieur comme NServiceBus, MassTransit ou Brighter à formuler votre bus d’événements et de système de publication/abonnement.</span><span class="sxs-lookup"><span data-stu-id="502be-161">Alternatively, you might want to use a higher-level service bus like NServiceBus, MassTransit, or Brighter to articulate your event bus and publish/subscribe system.</span></span>

## <a name="a-note-about-messaging-technologies-for-production-systems"></a><span data-ttu-id="502be-162">Remarque à propos des technologies pour les systèmes de production de messagerie</span><span class="sxs-lookup"><span data-stu-id="502be-162">A note about messaging technologies for production systems</span></span>

<span data-ttu-id="502be-163">Les technologies de messagerie disponibles pour l’implémentation de votre bus d’événements abstraits sont à des niveaux différents.</span><span class="sxs-lookup"><span data-stu-id="502be-163">The messaging technologies available for implementing your abstract event bus are at different levels.</span></span> <span data-ttu-id="502be-164">Par exemple, les produits comme RabbitMQ (transport de messagerie service broker) et Azure Service Bus se trouvent à un niveau inférieur à d’autres produits comme, NServiceBus, MassTransit ou Brighter, ce qui peut fonctionner sur RabbitMQ et Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="502be-164">For instance, products like RabbitMQ (a messaging broker transport) and Azure Service Bus sit at a lower level than other products like, NServiceBus, MassTransit, or Brighter, which can work on top of RabbitMQ and Azure Service Bus.</span></span> <span data-ttu-id="502be-165">Votre choix dépend du nombre de fonctionnalités au niveau de l’application et l’évolutivité de l’emploi que vous avez besoin pour votre application.</span><span class="sxs-lookup"><span data-stu-id="502be-165">Your choice depends on how many rich features at the application level and out-of-the-box scalability you need for your application.</span></span> <span data-ttu-id="502be-166">Pour l’implémentation de simplement un bus d’événements de la preuve de concept pour votre environnement de développement, comme nous l’avons fait dans l’exemple eShopOnContainers, une implémentation simple de RabbitMQ en cours d’exécution sur un conteneur Docker peut être suffisant.</span><span class="sxs-lookup"><span data-stu-id="502be-166">For implementing just a proof-of-concept event bus for your development environment, as we have done in the eShopOnContainers sample, a simple implementation on top of RabbitMQ running on a Docker container might be enough.</span></span>

<span data-ttu-id="502be-167">Toutefois, pour les applications stratégiques et les systèmes de production devant hyper évolutivité, vous pouvez évaluer Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="502be-167">However, for mission-critical and production systems that need hyper-scalability, you might want to evaluate Azure Service Bus.</span></span> <span data-ttu-id="502be-168">Pour les fonctionnalités qui facilitent le développement d’applications distribuées et des abstractions, nous vous recommandons d’évaluer autres bus de service commercial et open source, tels que NServiceBus, MassTransit et Brighter.</span><span class="sxs-lookup"><span data-stu-id="502be-168">For high-level abstractions and features that make the development of distributed applications easier, we recommend that you evaluate other commercial and open-source service buses, such as NServiceBus, MassTransit, and Brighter.</span></span> <span data-ttu-id="502be-169">Bien entendu, vous pouvez créer vos propres fonctionnalités de service bus sur les technologies de bas niveau telles que RabbitMQ et Docker.</span><span class="sxs-lookup"><span data-stu-id="502be-169">Of course, you can build your own service-bus features on top of lower-level technologies like RabbitMQ and Docker.</span></span> <span data-ttu-id="502be-170">Mais ce travail plomberie coûtent trop important pour une application d’entreprise.</span><span class="sxs-lookup"><span data-stu-id="502be-170">But that plumbing work might cost too much for a custom enterprise application.</span></span>

## <a name="resiliently-publishing-to-the-event-bus"></a><span data-ttu-id="502be-171">Élastique publication sur le bus d’événements</span><span class="sxs-lookup"><span data-stu-id="502be-171">Resiliently publishing to the event bus</span></span>

<span data-ttu-id="502be-172">Un lors de l’implémentation d’une architecture pilotée par événements entre plusieurs microservices consiste à mettre à jour atomiquement d’état dans le microservice d’origine lors de la publication élastique de son événement intégration connexes dans le bus d’événements, en fonction d’une certaine manière transactions.</span><span class="sxs-lookup"><span data-stu-id="502be-172">A challenge when implementing an event-driven architecture across multiple microservices is how to atomically update state in the original microservice while resiliently publishing its related integration event into the event bus, somehow based on transactions.</span></span> <span data-ttu-id="502be-173">Voici quelques manières d’effectuer cette opération, bien qu’il peut y avoir des approches supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="502be-173">The following are a few ways to accomplish this, although there could be additional approaches as well.</span></span>

-   <span data-ttu-id="502be-174">À l’aide d’une file transactionnelle (basé sur le DTC) tels que MSMQ.</span><span class="sxs-lookup"><span data-stu-id="502be-174">Using a transactional (DTC-based) queue like MSMQ.</span></span> <span data-ttu-id="502be-175">(Toutefois, il s’agit une approche héritée.)</span><span class="sxs-lookup"><span data-stu-id="502be-175">(However, this is a legacy approach.)</span></span>

-   <span data-ttu-id="502be-176">À l’aide de [d’exploration de données du journal des transactions](http://www.scoop.it/t/sql-server-transaction-log-mining).</span><span class="sxs-lookup"><span data-stu-id="502be-176">Using [transaction log mining](http://www.scoop.it/t/sql-server-transaction-log-mining).</span></span>

-   <span data-ttu-id="502be-177">À l’aide complète [événement approvisionnement](https://msdn.microsoft.com/en-us/library/dn589792.aspx) modèle.</span><span class="sxs-lookup"><span data-stu-id="502be-177">Using full [Event Sourcing](https://msdn.microsoft.com/en-us/library/dn589792.aspx) pattern.</span></span>

-   <span data-ttu-id="502be-178">À l’aide de la [modèle de boîte d’envoi](http://gistlabs.com/2014/05/the-outbox/): une table de base de données transactionnelle comme une file d’attente qui sera la base d’un composant créateur de l’événement qui serait créer l’événement et le publier.</span><span class="sxs-lookup"><span data-stu-id="502be-178">Using the [Outbox pattern](http://gistlabs.com/2014/05/the-outbox/): a transactional database table as a message queue that will be the base for an event-creator component that would create the event and publish it.</span></span>

<span data-ttu-id="502be-179">Rubriques supplémentaires à prendre en compte lors de l’utilisation d’une communication asynchrone sont l’idempotence de message et la déduplication de message.</span><span class="sxs-lookup"><span data-stu-id="502be-179">Additional topics to consider when using asynchronous communication are message idempotence and message deduplication.</span></span> <span data-ttu-id="502be-180">Ces rubriques sont traitées dans la section [mise en œuvre de la communication basée sur les événements entre microservices (événements d’intégration)](#implementing_event_based_comms_microserv) plus loin dans ce guide.</span><span class="sxs-lookup"><span data-stu-id="502be-180">These topics are covered in the section [Implementing event-based communication between microservices (integration events)](#implementing_event_based_comms_microserv) later in this guide.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="502be-181">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="502be-181">Additional resources</span></span>

-   <span data-ttu-id="502be-182">**Événement piloté par messagerie**
    [*http://soapatterns.org/design\_modèles/événement\_par\_de messagerie*](http://soapatterns.org/design_patterns/event_driven_messaging)</span><span class="sxs-lookup"><span data-stu-id="502be-182">**Event Driven Messaging**
[*http://soapatterns.org/design\_patterns/event\_driven\_messaging*](http://soapatterns.org/design_patterns/event_driven_messaging)</span></span>

-   <span data-ttu-id="502be-183">**Canal de publication/abonnement**
    [*http://www.enterpriseintegrationpatterns.com/patterns/messaging/PublishSubscribeChannel.html*](http://www.enterpriseintegrationpatterns.com/patterns/messaging/PublishSubscribeChannel.html)</span><span class="sxs-lookup"><span data-stu-id="502be-183">**Publish/Subscribe Channel**
[*http://www.enterpriseintegrationpatterns.com/patterns/messaging/PublishSubscribeChannel.html*](http://www.enterpriseintegrationpatterns.com/patterns/messaging/PublishSubscribeChannel.html)</span></span>

-   <span data-ttu-id="502be-184">**UDI Dahan. Clarification CQRS**
    [*http://udidahan.com/2009/12/09/clarified-cqrs/*](http://udidahan.com/2009/12/09/clarified-cqrs/)</span><span class="sxs-lookup"><span data-stu-id="502be-184">**Udi Dahan. Clarified CQRS**
[*http://udidahan.com/2009/12/09/clarified-cqrs/*](http://udidahan.com/2009/12/09/clarified-cqrs/)</span></span>

-   <span data-ttu-id="502be-185">**Commande et de répartition de responsabilité (CQRS) de la requête**
    [*https://docs.microsoft.com/azure/architecture/patterns/cqrs*](https://docs.microsoft.com/azure/architecture/patterns/cqrs)</span><span class="sxs-lookup"><span data-stu-id="502be-185">**Command and Query Responsibility Segregation (CQRS)**
[*https://docs.microsoft.com/azure/architecture/patterns/cqrs*](https://docs.microsoft.com/azure/architecture/patterns/cqrs)</span></span>

-   <span data-ttu-id="502be-186">**Communication entre délimitée contextes**
    [*https://msdn.microsoft.com/library/jj591572.aspx*](https://msdn.microsoft.com/library/jj591572.aspx)</span><span class="sxs-lookup"><span data-stu-id="502be-186">**Communicating Between Bounded Contexts**
[*https://msdn.microsoft.com/library/jj591572.aspx*](https://msdn.microsoft.com/library/jj591572.aspx)</span></span>

-   <span data-ttu-id="502be-187">**Cohérence éventuelle**
    [*https://en.wikipedia.org/wiki/Eventual\_la cohérence*](https://en.wikipedia.org/wiki/Eventual_consistency)</span><span class="sxs-lookup"><span data-stu-id="502be-187">**Eventual consistency**
[*https://en.wikipedia.org/wiki/Eventual\_consistency*](https://en.wikipedia.org/wiki/Eventual_consistency)</span></span>

-   <span data-ttu-id="502be-188">**Jimmy Bogard. Refactorisation vers résilience : L’évaluation de couplage**
    [*https://jimmybogard.com/refactoring-towards-resilience-evaluating-coupling/*](https://jimmybogard.com/refactoring-towards-resilience-evaluating-coupling/)</span><span class="sxs-lookup"><span data-stu-id="502be-188">**Jimmy Bogard. Refactoring Towards Resilience: Evaluating Coupling**
[*https://jimmybogard.com/refactoring-towards-resilience-evaluating-coupling/*](https://jimmybogard.com/refactoring-towards-resilience-evaluating-coupling/)</span></span>


>[!div class="step-by-step"]
<span data-ttu-id="502be-189">[Précédente] (communication-de-microservice-architecture.md) [suivant] (mettre à jour-microservice-apis.md)</span><span class="sxs-lookup"><span data-stu-id="502be-189">[Previous] (communication-in-microservice-architecture.md) [Next] (maintain-microservice-apis.md)</span></span>
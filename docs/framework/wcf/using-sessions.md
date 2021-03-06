---
title: Utilisation de sessions
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- csharp
- vb
helpviewer_keywords:
- sessions [WCF]
ms.assetid: 864ba12f-3331-4359-a359-6d6d387f1035
caps.latest.revision: 
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload:
- dotnet
ms.openlocfilehash: f5f6df22918dedf32738a8cb9d73af2e625923a4
ms.sourcegitcommit: c0dd436f6f8f44dc80dc43b07f6841a00b74b23f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="using-sessions"></a>Utilisation de sessions
Dans les applications [!INCLUDE[indigo1](../../../includes/indigo1-md.md)] , une *session* met en corrélation un groupe de messages dans une conversation. Les sessions[!INCLUDE[indigo2](../../../includes/indigo2-md.md)] sont différentes de l'objet session disponible dans des applications [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] , prennent en charge différents comportements et sont contrôlées de différentes façons. Cette rubrique décrit les fonctionnalités activées par les sessions dans les applications [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] et comment les utiliser.  
  
## <a name="sessions-in-windows-communication-foundation-applications"></a>Sessions dans les applications Windows Communication Foundation  
 Lorsqu'un contrat de service spécifie qu'il requiert une session, ce contrat requiert que les appels généraux (autrement dit, les échanges de messages sous-jacents qui prennent en charge les appels) doivent faire partie de la même conversation. Si un contrat spécifie qu'il autorise des sessions mais n'en requiert pas, les clients peuvent se connecter et établir ou non une session. Si la session se termine et qu'un message est envoyé via le même canal, une exception est levée.  
  
 Les sessions[!INCLUDE[indigo2](../../../includes/indigo2-md.md)] sont associées aux principales fonctionnalités conceptuelles suivantes :  
  
-   Elles sont explicitement initialisées et terminées par l'application appelante (le client WCF).  
  
-   Les messages remis pendant une session sont traités dans l'ordre dans lequel ils sont reçus.  
  
-   Les sessions corrèlent un groupe de messages dans une conversation. Différents types de corrélation sont possibles : Par exemple, un canal basé sur session peut corréler des messages sur la base d'une connexion réseau partagée, pendant qu'un autre canal basé sur session corrèle des messages sur la base d'une balise partagée dans le corps du message. Les fonctionnalités qui peuvent être dérivées de la session varient en fonction de la nature de la corrélation.  
  
-   Il n'existe pas de magasin de données général associé à une session [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] .  
  
 Si vous êtes familiarisé avec la classe <xref:System.Web.SessionState.HttpSessionState?displayProperty=nameWithType> des applications [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] et avec les fonctionnalités qu'elle fournit, vous remarquerez les différences suivantes entre ce type de session et les sessions [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] :  
  
-   Les sessions [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] sont systématiquement initialisées par le serveur.  
  
-   Les sessions[!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] sont implicitement non ordonnées.  
  
-   Les sessions[!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)] fournissent un mécanisme de stockage général des données sur l'ensemble des demandes.  
  
 Cette rubrique décrit :  
  
-   Le comportement d'exécution par défaut lors de l'utilisation de liaisons basées sur session dans la couche de modèle de service.  
  
-   Types de fonctionnalités que les liaisons [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] définies par le système et basées sur les sessions fournissent.  
  
-   Comment créer un contrat qui déclare une spécification de session.  
  
-   Comment comprendre et contrôler la création de la session , la fin de la session et la relation entre la session et l'instance de service.  
  
## <a name="default-execution-behavior-using-sessions"></a>Comportement d'exécution par défaut à l'aide de sessions  
 Une liaison qui tente d'initialiser une session est appelée une liaison *basée sur session* . Les contrats de service spécifient qu'ils requièrent, autorisent ou refusent des liaisons basées sur session en affectant l'une des valeurs d'énumération <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A?displayProperty=nameWithType> à la propriété <xref:System.ServiceModel.SessionMode?displayProperty=nameWithType> sur l'interface de contrat de service (ou classe). Par défaut, la valeur de cette propriété est <xref:System.ServiceModel.SessionMode.Allowed>, signifiant que si un client utilise une liaison basée sur session avec une implémentation du service [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] , le service établit et utilise la session fournie.  
  
 Lorsqu'un service [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] accepte une session cliente, les fonctionnalités suivantes sont activées par défaut :  
  
1.  Tous les appels entre un objet client [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] sont gérés par la même instance de service.  
  
2.  Différentes liaisons basées sur les sessions fournissent des fonctionnalités supplémentaires.  
  
## <a name="system-provided-session-types"></a>Types de sessions fournis par le système  
 Une liaison basée sur session prend en charge l'association par défaut d'une instance de service avec une session donnée. Toutefois, différentes liaisons basées sur session prennent en charge des fonctionnalités différentes en plus d'activer le contrôle d'instanciation basé sur session précédemment décrit.  
  
 [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] fournit les types de comportement d'application basé sur session suivants :  
  
-   <xref:System.ServiceModel.Channels.SecurityBindingElement?displayProperty=nameWithType> prend en charge des sessions basées sur sécurité, dans lesquelles les deux terminaisons de communication ont convenu d'une conversation sécurisée spécifique. [!INCLUDE[crdefault](../../../includes/crdefault-md.md)][Sécurisation des Services](../../../docs/framework/wcf/securing-services.md). Par exemple, la liaison <xref:System.ServiceModel.WSHttpBinding?displayProperty=nameWithType> qui contient la prise en charge des sessions de sécurité et des sessions fiables, utilise par défaut uniquement une session sécurisée qui chiffre et signe numériquement les messages.  
  
-   La liaison <xref:System.ServiceModel.NetTcpBinding?displayProperty=nameWithType> prend en charge les sessions basées sur TCP/IP pour garantir que tous les messages sont corrélés par la connexion au niveau du socket.  
  
-   L'élément <xref:System.ServiceModel.Channels.ReliableSessionBindingElement?displayProperty=nameWithType> qui implémente la spécification WS-ReliableMessaging, fournit la prise en charge des sessions fiables dans lesquelles les messages peuvent être configurés afin d'être remis dans l'ordre et exactement une fois, ce qui garantit que les messages sont reçus même lorsqu'ils passent par plusieurs nœuds pendant la conversation. [!INCLUDE[crdefault](../../../includes/crdefault-md.md)][Des Sessions fiables](../../../docs/framework/wcf/feature-details/reliable-sessions.md).  
  
-   La liaison <xref:System.ServiceModel.NetMsmqBinding?displayProperty=nameWithType> fournit des sessions de datagramme MSMQ. [!INCLUDE[crdefault](../../../includes/crdefault-md.md)][Les files d’attente dans WCF](../../../docs/framework/wcf/feature-details/queues-in-wcf.md).  
  
 La définition de la propriété <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A> ne spécifie pas le type de session que le contrat requiert mais uniquement qu'il requiert un type de session.  
  
## <a name="creating-a-contract-that-requires-a-session"></a>Création d'un contrat qui requiert une session  
 La création d'un contrat qui requiert une session déclare que le groupe d'opérations que le contrat de service déclare doit entièrement être exécuté dans la même session et que les messages doivent être remis dans l'ordre. Pour déterminer le niveau de support de session qu'un contrat de service requiert, attribuez la valeur de l'énumération <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A?displayProperty=nameWithType> à la propriété <xref:System.ServiceModel.SessionMode?displayProperty=nameWithType> sur votre interface de contrat de service ou classe, afin de spécifier si le contrat :  
  
-   Requiert une session.  
  
-   Permet à un client d'établir une session.  
  
-   Interdit une session.  
  
 Toutefois, la définition de la propriété <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A> ne spécifie pas le type de comportement basé sur session que le contrat requiert. Il demande à [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] de confirmer au moment de l'exécution que la liaison (créant le canal de communication) configurée pour le service établit, n'établit pas ou peut établir une session lors de l'implémentation d'un service. Une fois encore, la liaison peut satisfaire cette spécification avec tout type de comportement basé sur session choisi : sécurité, transport, fiable ou toute combinaison. Le comportement exact dépend de la valeur <xref:System.ServiceModel.SessionMode?displayProperty=nameWithType> sélectionnée. Si la liaison configurée du service n'est pas conforme à la valeur <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A>, une exception est levée. Les liaisons et les canaux créés prenant en charge ces sessions sont dits « basés sur session ».  
  
 Le contrat de service suivant spécifie que toutes les opérations de la `ICalculatorSession` doivent être échangées dans une session. Aucune des opérations ne retourne une valeur à l'appelant sauf la méthode `Equals` . Toutefois, la méthode `Equals` ne prend pas de paramètres et, par conséquent, peut retourner uniquement une valeur non nulle à l'intérieur d'une session dans laquelle les données sont déjà passées aux autres opérations. Ce contrat requiert qu'une session fonctionne correctement. Sans une session associée à un client spécifique, l'instance de service n'a aucun moyen de savoir quelles données ce client a envoyées précédemment.  
  
 [!code-csharp[S_Service_Session#1](../../../samples/snippets/csharp/VS_Snippets_CFX/s_service_session/cs/service.cs#1)]
 [!code-vb[S_Service_Session#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_service_session/vb/service.vb#1)]  
  
 Si un service autorise une session, une session est établie et utilisée si le client en initialise une ; sinon, aucune session n'est établie.  
  
## <a name="sessions-and-service-instances"></a>Sessions et instances de service  
 Si vous utilisez le comportement d'instanciation par défaut de [!INCLUDE[indigo2](../../../includes/indigo2-md.md)], les appels généraux entre un objet client [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] sont gérés par la même instance de service. Par conséquent, au niveau de l'application, vous pouvez considérer une session comme un moyen d'activer un comportement d'application similaire au comportement d'appel local. Par exemple, lorsque vous créez un objet local :  
  
-   Un constructeur est appelé.  
  
-   Tous les appels effectués ultérieurement à la référence d'objet client [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] sont traités par la même instance d'objet.  
  
-   Un destructeur est appelé lorsque la référence d'objet est détruite.  
  
 Les sessions activent un comportement semblable entre les clients et les services tant que le comportement de l'instance de service par défaut est utilisé. Si un contrat de service requiert ou prend en charge des sessions, une ou plusieurs opérations de contrat peuvent être marquées comme initialisant ou terminant une session, par l'attribution des propriétés <xref:System.ServiceModel.OperationContractAttribute.IsInitiating%2A> et <xref:System.ServiceModel.OperationContractAttribute.IsTerminating%2A> .  
  
 Les*opérations d'initialisation* sont celles qui doivent être appelées comme première opération d'une nouvelle session. Vous devez appeler au moins une opération d'initialisation avant de pouvoir appeler d'autres opérations. Vous pouvez par conséquent créer un type de constructeur de session pour votre service en déclarant des opérations d'initialisation conçues pour prendre l'entrée des clients appropriés au début de l'instance de service. (Toutefois, l'état est associé à la session et pas à l'objet de service.)  
  
 Les*opérations de fin*, inversement, doivent être appelées comme dernier message dans une session existante. Par défaut, [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] recycle l'objet de service et son contexte après la fermeture de la session à laquelle le service a été associé. Vous pouvez, par conséquent, créer un type de destructeur en déclarant les opérations de fin conçues pour exécuter une fonction appropriée à la fin de l'instance de service.  
  
> [!NOTE]
>  Bien que le comportement par défaut présente une ressemblance avec les constructeurs et les destructeurs locaux, il ne s'agit que d'une ressemblance. Toute opération du service [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] peut être une opération d'initialisation ou de fin, ou les deux à la fois. De plus, par défaut, les opérations d'initialisation peuvent être appelées un nombre illimité de fois, dans tout ordre, après l'appel de la première ; aucune session supplémentaire n'est créée une fois que la session est établie et associée à une instance, à moins que vous ne contrôliez explicitement la durée de vie de l'instance de service (en manipulant l'objet <xref:System.ServiceModel.InstanceContext?displayProperty=nameWithType>). Enfin, l'état est associé à la session et pas l'objet de service.  
  
 Par exemple, le contrat `ICalculatorSession` utilisé dans l'exemple précédent requiert que l'objet client [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] appelle en premier l'opération `Clear` avant toute autre opération et que la session avec l'objet client [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] se termine lorsqu'il appelle l'opération `Equals` . L'exemple de code suivant montre un contrat qui applique ces spécifications. `Clear` doit être appelé en premier pour initialiser une session, et cette session se termine lorsque `Equals` est appelé.  
  
 [!code-csharp[SCA.IsInitiatingIsTerminating#1](../../../samples/snippets/csharp/VS_Snippets_CFX/sca.isinitiatingisterminating/cs/service.cs#1)]
 [!code-vb[SCA.IsInitiatingIsTerminating#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/sca.isinitiatingisterminating/vb/service.vb#1)]  
  
 Les services ne démarrent pas de sessions avec les clients. Dans les applications clientes [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] , il existe une relation directe entre la durée de vie du canal basé sur session et la durée de vie de la session elle-même. En tant que tels, les clients créent de nouvelles sessions en créant de nouveaux canaux basés sur des sessions et détruisent les sessions existantes en fermant correctement lesdits canaux. Un client démarre une session avec un point de terminaison de service en appelant l'un des éléments suivants :  
  
-   <xref:System.ServiceModel.ICommunicationObject.Open%2A?displayProperty=nameWithType> sur le canal retourné par un appel à <xref:System.ServiceModel.ChannelFactory%601.CreateChannel%2A?displayProperty=nameWithType>.  
  
-   <xref:System.ServiceModel.ClientBase%601.Open%2A?displayProperty=nameWithType>sur le [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] objet client généré par le [ServiceModel Metadata Utility Tool (Svcutil.exe)](../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md).  
  
-   Une opération d'initialisation sur un type d'objet client [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] (par défaut, toutes les opérations s'initialisent). Lorsque la première opération est appelée, l'objet client [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] ouvre automatiquement le canal et initialise une session.  
  
 Habituellement, un client termine une session avec un point de terminaison de service en appelant l'un des éléments suivants :  
  
-   <xref:System.ServiceModel.ICommunicationObject.Close%2A?displayProperty=nameWithType> sur le canal retourné par un appel à <xref:System.ServiceModel.ChannelFactory%601.CreateChannel%2A?displayProperty=nameWithType>.  
  
-   <xref:System.ServiceModel.ClientBase%601.Close%2A?displayProperty=nameWithType> sur l'objet client [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] généré par Svcutil.exe.  
  
-   Une opération terminant sur un type d'objet client [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] (par défaut, aucune opération ne termine ; le contrat doit spécifier explicitement une opération de fin). Lorsque la première opération est appelée, l'objet client [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] ouvre automatiquement le canal et initialise une session.  
  
 Pour obtenir des exemples, consultez [How to: Create a Service That Requires Sessions](../../../docs/framework/wcf/feature-details/how-to-create-a-service-that-requires-sessions.md) ainsi que [Default Service Behavior](../../../docs/framework/wcf/samples/default-service-behavior.md) et [Instancing](../../../docs/framework/wcf/samples/instancing.md) .  
  
 [!INCLUDE[crabout](../../../includes/crabout-md.md)]les clients et les sessions, consultez [les Services de l’accès à l’aide d’un Client WCF](../../../docs/framework/wcf/feature-details/accessing-services-using-a-client.md).  
  
## <a name="sessions-interact-with-instancecontext-settings"></a>Sessions interagissant avec les paramètres InstanceContext  
 Il y a une interaction entre l'énumération <xref:System.ServiceModel.SessionMode> dans un contrat et la propriété <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=nameWithType> qui contrôle l'association entre canaux et objets de service spécifiques. [!INCLUDE[crdefault](../../../includes/crdefault-md.md)][Sessions, instanciation et accès concurrentiel](../../../docs/framework/wcf/feature-details/sessions-instancing-and-concurrency.md).  
  
### <a name="sharing-instancecontext-objects"></a>Partage d'objets InstanceContext  
 Vous pouvez également contrôler quel canal ou appel basé sur session est associé à tel ou tel objet <xref:System.ServiceModel.InstanceContext> en effectuant vous-même l'association. Pour obtenir un exemple complet, consultez [InstanceContextSharing](http://msdn.microsoft.com/library/4a6a46d7-b7d7-4bb5-a0dd-03ffa3cbc230).  
  
## <a name="sessions-and-streaming"></a>Sessions et diffusion en continu  
 Lorsque vous avez une grande quantité de données à transférer, le mode de transfert par diffusion en continu dans [!INCLUDE[indigo2](../../../includes/indigo2-md.md)] est une alternative possible au comportement par défaut de mise en mémoire tampon et de traitement des messages en mémoire dans leur intégralité. Vous pouvez obtenir un comportement inattendu lors de la diffusion en continu des appels avec une liaison basée sur session. Tous les appels en streaming passent par un canal unique (le canal de datagramme) qui ne prend pas en charge les sessions même si la liaison utilisée est configurée pour utiliser des sessions. Si plusieurs clients effectuent des appels de diffusion en continu vers le même objet de service sur une liaison basée sur session, et si le mode d'accès concurrentiel de l'objet de service est configuré comme unique et son mode de contexte d'instance a la valeur `PerSession`, les appels généraux doivent traverser le canal de datagramme et un seul appel à la fois est traité. Un ou plusieurs clients peuvent ainsi être temporisés. Vous pouvez contourner ce problème en attribuant au `InstanceContextMode` de l'objet de service la valeur `PerCall` , ou au mode d'accès concurrentiel la valeur Multiple.  
  
> [!NOTE]
>  MaxConcurrentSessions n'a aucun effet dans ce cas car une seule « session » est disponible.  
  
## <a name="see-also"></a>Voir aussi  
 <xref:System.ServiceModel.OperationContractAttribute.IsInitiating%2A>  
 <xref:System.ServiceModel.OperationContractAttribute.IsTerminating%2A>

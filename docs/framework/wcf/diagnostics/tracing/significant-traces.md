---
title: Suivis principaux
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: 40a1770e-3b09-4142-b0dd-f9ef73642074
caps.latest.revision: "4"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload: dotnet
ms.openlocfilehash: 6641fe633585caf6cbf068a804430f9171e5ece0
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/22/2017
---
# <a name="significant-traces"></a>Suivis principaux
Cette rubrique répertorie quelques-uns des suivis principaux émis par [!INCLUDE[indigo1](../../../../../includes/indigo1-md.md)].  
  
## <a name="significant-traces"></a>Suivis principaux  
  
|Suivi|Description|  
|-----------|-----------------|  
|Suivi du journal des messages|Le suivi est émis lorsqu'un message [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] est enregistré par la fonctionnalité de journalisation des messages lorsque la source du suivi `System.ServiceModel.MessageLogging` est activée. Un clic sur ce suivi permet d'afficher le message. Il existe quatre points d'enregistrement configurables pour un message : `ServiceLevelSendRequest`, `TransportSend`, `TransportReceive`, `ServiceLevelReceiveRequest`, également indiqués par l'attribut Message Source dans le suivi du journal des messages.|  
|Suivi de message reçu|Ce suivi est émis lorsqu'un message [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] est reçu si la source de suivi `System.ServiceModel` est activée au niveau des informations ou des détails d'informations. Ce suivi est nécessaire pour consulter la flèche de corrélation du message dans la vue du graphique d'activité.|  
|Suivi de message envoyé|Ce suivi est émis lorsqu'un message [!INCLUDE[indigo2](../../../../../includes/indigo2-md.md)] est envoyé si la source du suivi `System.ServiceModel` est activée au niveau des informations ou des commentaires. Ce suivi est nécessaire pour consulter la flèche de corrélation du message dans la vue du graphique d'activité.|  
|Obtenir ChannelEndpointElement|Ce suivi est émis dans la fabrication de canal Construct, au niveau des informations. Il fournit une description du point de terminaison auquel le client parle (adresse distante, liaison, nom de contrat).|  
|Obtenir ServiceElement|Ce suivi est émis dans l'hôte du service Construct, au niveau des informations. Elle fournit une description du contrat de service et de la liaison.|  
|Créer SocketConnection|Ce suivi est émis dans la première action Process exécutée par le client et dans l'activité Receive bytes du service. Il fournit les adresses IP locales et distantes. Il est émis au niveau Informations.|

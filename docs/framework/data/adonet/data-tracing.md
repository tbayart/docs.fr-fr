---
title: "Traçage de données dans ADO.NET"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-ado
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: a6a752a5-d2a9-4335-a382-b58690ccb79f
caps.latest.revision: "9"
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.workload: dotnet
ms.openlocfilehash: df958982739c7ab2fd7aba42918b919c25d86829
ms.sourcegitcommit: ed26cfef4e18f6d93ab822d8c29f902cff3519d1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2018
---
# <a name="data-tracing-in-adonet"></a>Traçage de données dans ADO.NET
ADO.NET introduit une nouvelle fonctionnalité intégrée de traçage de données prise en charge par les fournisseurs de données .NET pour [!INCLUDE[ssNoVersion](../../../../includes/ssnoversion-md.md)], Oracle, OLE DB et ODBC, de même que l'objet <xref:System.Data.DataSet> ADO.NET et les protocoles réseau [!INCLUDE[ssNoVersion](../../../../includes/ssnoversion-md.md)].  
  
 Le traçage d'appels API d'accès aux données peut aider à diagnostiquer les problèmes suivants :  
  
-   discordance de schéma entre le programme client et la base de données ;  
  
-   problèmes d'indisponibilité de base de données ou de bibliothèque réseau ;  
  
-   SQL incorrect qu'il soit en codage dur ou généré par une application ;  
  
-   logique de programmation incorrecte ;  
  
-   problèmes résultant de l'interaction entre plusieurs composants ADO.NET ou entre ADO.NET et vos propres composants.  
  
 Pour prendre en charge plusieurs technologies de traçage, le traçage est extensible, de sorte qu'un développeur peut tracer un problème à tout niveau de la pile d'applications. Bien que le traçage ne soit pas une fonctionnalité purement ADO.NET, les fournisseurs Microsoft tirent parti des API de suivi et d’instrumentation généralisées.  
  
 Pour plus d’informations sur la configuration et la configuration du traçage managé dans ADO.NET, consultez [accès aux données de suivi](http://msdn.microsoft.com/library/hh880086.aspx).  
  
## <a name="accessing-diagnostic-information-in-the-extended-events-log"></a>Accès aux informations de diagnostic dans le journal des événements étendus  
 Dans le [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] fournisseur de données pour [!INCLUDE[ssNoVersion](../../../../includes/ssnoversion-md.md)], suivi d’accès aux données ([suivi d’accès aux données](http://msdn.microsoft.com/library/hh880086.aspx)) a été mis à jour pour le rendre plus facile à plus facile à mettre en corrélation des événements clients avec les informations de diagnostic, tels que les échecs de connexion à partir en anneau mémoire tampon et l’application performances informations de connectivité du serveur dans le journal des événements étendus. Pour plus d’informations sur la lecture du journal des événements étendus, consultez [afficher les données de Session événements](http://msdn.microsoft.com/library/hh710068\(SQL.110\).aspx).  
  
 Pour les opérations de connexion, ADO.NET envoie un ID de connexion client. Si la connexion échoue, vous pouvez accéder à la mémoire tampon en anneau de connectivité ([résolution des problèmes de connectivité dans SQL Server 2008 avec la mémoire tampon en anneau de connectivité](http://go.microsoft.com/fwlink/?LinkId=207752)) et recherchez le `ClientConnectionID` champ et obtenir des informations de diagnostic sur la Échec de connexion. Les identificateurs de connexion client sont enregistrés dans la mémoire tampon en anneau uniquement si une erreur se produit. (Si une connexion échoue avant l'envoi du paquet de préouverture de session, aucun ID de connexion client n'est généré.) L'ID de connexion client est un GUID de 16 octets. Vous pouvez également rechercher l'ID de connexion client dans la sortie cible d'événements étendus, si l'action `client_connection_id` est ajoutée aux événements dans une session d'événements étendus. Vous pouvez activer le traçage d'accès aux données, réexécuter la commande de connexion et observer le champ `ClientConnectionID` dans la trace d'accès aux données, si vous avez besoin d'obtenir une aide supplémentaire pour le diagnostic du pilote client.  
  
 Vous pouvez obtenir l'ID de connexion client par programme à l'aide de la propriété `SqlConnection.ClientConnectionID`.  
  
 `ClientConnectionID` est disponible pour un objet <xref:System.Data.SqlClient.SqlConnection> qui établit une connexion. Si une tentative de connexion échoue, `ClientConnectionID` peut être disponible via `SqlException.ToString`.  
  
 ADO.NET envoie également un ID d'activité propre au thread L'ID d'activité est capturé dans les sessions des événements étendus si les sessions sont démarrées avec l'option TRACK_CAUSAILITY activée. Pour les problèmes de performances avec une connexion active, vous pouvez obtenir l'ID d'activité de la trace d'accès aux données du client (champ `ActivityID`), puis localiser l'ID d'activité dans la sortie d'événements étendus. L'ID d'activité dans des événements étendus est un GUID de 16 octets (différent du GUID de l'ID de connexion client) ajouté à un numéro séquentiel à quatre octets. Le numéro séquentiel représente l'ordre d'une demande dans un thread et indique le classement relatif des instructions par lots et RPC pour le thread. `ActivityID` est actuellement éventuellement envoyé pour les instructions par lots SQL et les demandes RPC lorsque le suivi de l'accès aux données est activé et le 18e bit dans le mot de configuration de suivi d'accès aux données est activé.  
  
 Voici un exemple qui utilise [!INCLUDE[tsql](../../../../includes/tsql-md.md)] pour démarrer une session d'événements étendus qui sera enregistrée dans une mémoire tampon en anneau et indique l'ID d'activité envoyé à partir d'un client sur le RPC et les opérations par lots.  
  
```  
create event session MySession on server   
add event connectivity_ring_buffer_recorded,   
add event sql_statement_starting (action (client_connection_id)),   
add event sql_statement_completed (action (client_connection_id)),   
add event rpc_starting (action (client_connection_id)),   
add event rpc_completed (action (client_connection_id))  
add target ring_buffer with (track_causality=on)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Traçage réseau dans .NET Framework](../../../../docs/framework/network-programming/network-tracing.md)  
 [Suivi et instrumentation d’applications](../../../../docs/framework/debug-trace-profile/tracing-and-instrumenting-applications.md)  
 [Fournisseurs managés ADO.NET et centre de développement DataSet](http://go.microsoft.com/fwlink/?LinkId=217917)

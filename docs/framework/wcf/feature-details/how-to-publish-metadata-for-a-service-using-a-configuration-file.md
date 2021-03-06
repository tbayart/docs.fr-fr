---
title: "Comment : publier les métadonnées d'un service à l'aide d'un fichier de configuration"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: f061443f-92df-4824-b36a-609c4cd14a17
caps.latest.revision: "24"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.workload: dotnet
ms.openlocfilehash: 42f70cd34f65d5393d79b8ace4f9eb704f309d0f
ms.sourcegitcommit: 16186c34a957fdd52e5db7294f291f7530ac9d24
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/22/2017
---
# <a name="how-to-publish-metadata-for-a-service-using-a-configuration-file"></a>Comment : publier les métadonnées d'un service à l'aide d'un fichier de configuration
C'est l'une des deux rubriques de procédure qui traitent de la publication des métadonnées pour un service [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)]. Il y a deux façons de spécifier comment un service doit publier des métadonnées : à l'aide d'un fichier de configuration et à l'aide du code. Cette rubrique montre comment publier des métadonnées pour un service à l'aide d'un fichier de configuration.  
  
> [!CAUTION]
>  Cette rubrique indique comment publier des métadonnées de manière non sécurisée. Tout client peut récupérer les métadonnées du service. Si vous avez besoin de votre service pour publier des métadonnées de manière sécurisée, consultez [point de terminaison de métadonnées sécurisé personnalisée](../../../../docs/framework/wcf/samples/custom-secure-metadata-endpoint.md).  
  
 [!INCLUDE[crabout](../../../../includes/crabout-md.md)]publication de métadonnées dans le code, consultez [Comment : publier des métadonnées pour un Code à l’aide du Service](../../../../docs/framework/wcf/feature-details/how-to-publish-metadata-for-a-service-using-code.md). La publication des métadonnées permet aux clients de récupérer les métadonnées via une requête WS-Transfer GET ou une requête HTTP/GET à l'aide de la chaîne de requête `?wsdl`. Pour être sûr que le code fonctionne, créez un service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] de base. Pour plus de simplicité, un service auto-hébergé de base est fourni dans le code suivant.  
  
```csharp  
using System;  
using System.Runtime.Serialization;  
using System.ServiceModel;  
using System.ServiceModel.Description;  
  
namespace Metadata.Samples  
{  
    [ServiceContract]  
    public interface ISimpleService  
    {  
        [OperationContract]  
        string SimpleMethod(string msg);  
    }  
  
    class SimpleService : ISimpleService  
    {  
        public string SimpleMethod(string msg)  
        {  
            Console.WriteLine("The caller passed in " + msg);  
            return "Hello " + msg;  
        }  
    }  
  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            ServiceHost host = new ServiceHost(typeof(SimpleService),  
                new Uri("http://localhost:8001/MetadataSample"));   
            try  
            {  
                // Open the service host to accept incoming calls  
                host.Open();  
  
                // The service can now be accessed.  
                Console.WriteLine("The service is ready.");  
                Console.WriteLine("Press <ENTER> to terminate service.");  
                Console.WriteLine();  
                Console.ReadLine();  
  
                // Close the ServiceHostBase to shutdown the service.  
                host.Close();  
            }  
            catch (CommunicationException commProblem)  
            {  
                Console.WriteLine("There was a communication problem. " + commProblem.Message);  
                Console.Read();  
            }  
        }  
    }  
}  
```  
  
 Ce service est un service auto-hébergé, configuré à l'aide d'un fichier de configuration. Le fichier de configuration suivant sert de point de départ.  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service name="Metadata.Example.SimpleService">  
        <endpoint address=""  
                  binding="basicHttpBinding"  
                  contract="Metadata.Example.ISimpleService" />  
      </service>  
    </services>  
    <behaviors>  
  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
### <a name="to-publish-metadata-for-a-wcf-service-using-an-application-configuration-file"></a>Pour publier les métadonnées d'un service WCF à l'aide d'un fichier de configuration d'application  
  
1.  Dans le fichier App.config, après la fermeture de l'élément `</services>`, créez un élément `<behaviors>`.  
  
  
  
2.  Dans l'élément `<behaviors>`, ajoutez un nouvel élément `<serviceBehaviors>`.  
  
  
  
3.  Ajoutez un élément `<behavior>` à l'élément `<serviceBehaviors>` et spécifiez une valeur pour l'attribut `name` de l'élément `<behavior>`.  
  
  
  
4.  Ajoutez un élément `<serviceMetadata>` à l'élément `<behavior>`. Affectez à l'attribut `httpGetEnabled` la valeur `true` et à l'attribut `policyVersion` la valeur Policy15. `httpGetEnabled` permet au service de répondre aux demandes de métadonnées faites par une demande HTTP GET. `policyVersion` indique au service de se conformer à WS-Policy 1.5 lors de la génération des métadonnées.  
  
  
  
5.  Ajoutez un attribut `behaviorConfiguration` à l'élément `<service>` et spécifiez l'attribut `name` de l'élément `<behavior>` ajouté à l'étape 1, comme illustré dans l'exemple de code suivant.  
  
    ```xml  
    <services>  
      <service  
          name="Metadata.Example.SimpleService"  
          behaviorConfiguration="SimpleServiceBehavior">  
        ...  
      </service>  
    </services>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="SimpleServiceBehavior">  
          <serviceMetadata httpGetEnabled="True" policyVersion="Policy15" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    ```  
  
6.  Ajoutez un ou plusieurs éléments `<endpoint>` et attribuez au contrat la valeur `IMetadataExchange`, comme illustré dans l'exemple de code suivant.  
  
    ```xml  
    <services>  
      <service  
          name="Metadata.Example.SimpleService"  
          behaviorConfiguration="SimpleServiceBehavior">  
  
        <endpoint address=""  
                  binding="wsHttpBinding"  
                  contract="Metadata.Example.ISimpleService" />  
  
        <endpoint address="mex"  
                  binding="mexHttpBinding"  
                  contract="IMetadataExchange" />  
      </service>  
    </services>  
    ```  
  
7.  Pour les points de terminaison de métadonnées ajoutés à l'étape précédente, affectez à l'attribut `binding` l'une des valeurs suivantes :  
  
    -   `mexHttpBinding` pour la publication HTTP.  
  
    -   `mexHttpsBinding` pour la publication HTTPS.  
  
    -   `mexNamedPipeBinding` pour la publication de canal nommé.  
  
    -   `mexTcpBinding` pour la publication TCP.  
  
8.  Pour les points de terminaison de métadonnées ajoutés à l'étape précédente, attribuez à l'adresse la valeur suivante :  
  
    -   Une chaîne vide pour utiliser l'adresse de base de l'application hôte comme point de publication si l'adresse de base est la même que la liaison de métadonnées.  
  
    -   Une adresse relative si l'application hôte a une adresse de base.  
  
    -   Une adresse absolue.  
  
9. Créez et exécutez l'application console.  
  
10. Utilisez Internet Explorer pour naviguer jusqu'à l'adresse de base du service (http://localhost:8001/MetadataSample dans cet exemple) et vérifiez que la publication des métadonnées est activée. Dans la négative, un message s'affiche en haut de la page résultante : "La publication des métadonnées pour ce service est actuellement désactivée".  
  
### <a name="to-use-default-endpoints"></a>Pour utiliser les points de terminaison par défaut  
  
1.  Pour configurer des métadonnées sur un service qui utilise les points de terminaison par défaut, spécifiez <xref:System.ServiceModel.Description.ServiceMetadataBehavior> dans le fichier de configuration, comme dans l'exemple précédent, mais ne spécifiez aucun point de terminaison. Le fichier de configuration se présenterait alors comme suit.  
  
    ```xml  
    <configuration>  
      <system.serviceModel>  
        <behaviors>  
          <serviceBehaviors>  
            <behavior name="SimpleServiceBehavior">  
              <serviceMetadata httpGetEnabled="True" policyVersion="Policy12" />  
            </behavior>  
          </serviceBehaviors>  
        </behaviors>  
  
      </system.serviceModel>  
    </configuration>  
    ```  
  
     Étant donné que le service a un <xref:System.ServiceModel.Description.ServiceMetadataBehavior> avec le `httpGetEnabled` ayant la valeur `true`, la publication des métadonnées est activée pour le service, et comme aucun point de terminaison n'a été ajouté explicitement, le runtime ajoute les points de terminaison par défaut. [!INCLUDE[crabout](../../../../includes/crabout-md.md)]points de terminaison par défaut, les liaisons et comportements, consultez [Configuration simplifiée](../../../../docs/framework/wcf/simplified-configuration.md) et [simplifié la Configuration des Services WCF](../../../../docs/framework/wcf/samples/simplified-configuration-for-wcf-services.md).  
  
## <a name="example"></a>Exemple  
 L'exemple de code suivant affiche l'implémentation d'un service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] de base et le fichier de configuration qui publie les métadonnées pour le service.  
  
```csharp  
using System;  
using System.Runtime.Serialization;  
using System.ServiceModel;  
using System.ServiceModel.Description;  
  
namespace Metadata.Samples  
{  
    [ServiceContract]  
    public interface ISimpleService  
    {  
        [OperationContract]  
        string SimpleMethod(string msg);  
    }  
  
    class SimpleService : ISimpleService  
    {  
        public string SimpleMethod(string msg)  
        {  
            Console.WriteLine("The caller passed in " + msg);  
            return "Hello " + msg;  
        }  
    }  
  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            ServiceHost host = new ServiceHost(typeof(SimpleService),  
                new Uri("http://localhost:8001/MetadataSample"));   
            try  
            {  
                // Open the service host to accept incoming calls  
                host.Open();  
  
                // The service can now be accessed.  
                Console.WriteLine("The service is ready.");  
                Console.WriteLine("Press <ENTER> to terminate service.");  
                Console.WriteLine();  
                Console.ReadLine();  
  
                // Close the ServiceHostBase to shutdown the service.  
                host.Close();  
            }  
            catch (CommunicationException commProblem)  
            {  
                Console.WriteLine("There was a communication problem. " + commProblem.Message);  
                Console.Read();  
            }  
        }  
    }  
}  
```  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="SimpleServiceBehavior">  
          <serviceMetadata httpGetEnabled="True" policyVersion="Policy12" />  
          <serviceDebug includeExceptionDetailInFaults="False" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>Voir aussi  
 <xref:System.ServiceModel.Description.ServiceMetadataBehavior>  
 [Guide pratique pour héberger un service WCF dans une application managée](../../../../docs/framework/wcf/how-to-host-a-wcf-service-in-a-managed-application.md)  
 [Auto-hébergement](../../../../docs/framework/wcf/samples/self-host.md)  
 [Vue d’ensemble de l’architecture de métadonnées](../../../../docs/framework/wcf/feature-details/metadata-architecture-overview.md)  
 [Utilisation des métadonnées](../../../../docs/framework/wcf/feature-details/using-metadata.md)  
 [Guide pratique pour publier les métadonnées d’un service à l’aide de code](../../../../docs/framework/wcf/feature-details/how-to-publish-metadata-for-a-service-using-code.md)

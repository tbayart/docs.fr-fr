---
title: "IXmlSerializable des services Web, exemple de technologie | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 0202d3f1-a50b-427d-a5bb-79208b8f1c22
caps.latest.revision: 6
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 6
---
# IXmlSerializable des services Web, exemple de technologie
[Télécharger l'exemple](http://download.microsoft.com/download/4/7/B/47B2164C-E780-4B10-8DE4-2CB5B886E0A6/Technologies/Serialization/Xml%20Serialization/IXmlSerializable.zip.exe)  
  
 Cet exemple montre comment utiliser <xref:System.Xml.Serialization.IXmlSerializable> pour contrôler la sérialisation de types personnalisés dans les services Web ASP.NET.  
  
### Pour générer l'exemple à l'aide de Visual Studio  
  
1.  Ouvrez [!INCLUDE[vsprvslong](../../../includes/vsprvslong-md.md)] et sélectionnez **Nouveau site Web** dans le menu **Fichier**.  
  
2.  Dans le volet gauche de la boîte de dialogue **Nouveau site Web**, sélectionnez le langage de programmation de votre choix, puis dans le volet droit, sélectionnez **Service Web ASP.NET**.  
  
3.  Tapez **IXmlSerializable** comme nom du nouveau service Web.  
  
4.  Dans la fenêtre Explorateur de solutions, cliquez avec le bouton droit sur l'icône de Service.asmx et sélectionnez **Supprimer** ; répétez cette étape pour le fichier code\-behind Service.asmx.  
  
5.  Cliquez avec le bouton droit sur le répertoire de projet et sélectionnez **Ajouter un élément existant**.Dans la boîte de dialogue, accédez au sous\-répertoire Service du répertoire spécifique au langage.  
  
6.  Sélectionnez Service.asmx, puis répétez cette étape pour le fichier code\-behind Service.asmx.  
  
7.  Ouvrez l'[!INCLUDE[fileExplorer](../../../includes/fileexplorer-md.md)] et accédez au répertoire qui contient le répertoire IXmlSerializable créé à l'étape 3.  
  
8.  Cliquez avec le bouton droit sur l'icône du répertoire IXmlSerializable et sélectionnez **Partage et sécurité**.  
  
9. Sous l'onglet Partage Web, sélectionnez **Partager ce dossier** et vérifiez les paramètres par défaut, y compris le nom IXmlSerializable.  
  
10. Cliquez sur **OK**.  
  
### Pour exécuter l'exemple  
  
1.  Ouvrez une fenêtre de navigateur et sélectionnez sa barre d'adresses.  
  
2.  Tapez **http:\/\/localhost\/IXmlSerializable\/Service.asmx**.  
  
## Voir aussi  
 <xref:System.Xml.Serialization.IXmlSerializable>   
 <xref:System.Xml.Serialization>   
 <xref:System.Xml.XmlConvert>   
 <xref:System.Xml.XmlQualifiedName>   
 <xref:System.Xml.XmlReader>   
 <xref:System.Xml.Schema.XmlSchema>   
 <xref:System.Xml.Schema.XmlSchemaSet>   
 <xref:System.Xml.XmlUrlResolver>   
 <xref:System.Xml.XmlWriter>
---
title: Prérequis pour l’API de création de rapports Azure Active Directory | Microsoft Docs
description: En savoir plus sur la configuration requise pour accéder à l’API de création de rapports Azure AD
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 03/04/2020
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: f25f13c60a60e1a397e4c63443ee786a9acdf6d1
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78273800"
---
# <a name="prerequisites-to-access-the-azure-active-directory-reporting-api"></a>Prérequis pour accéder à l’API de création de rapports Azure Active Directory

Les [API de création de rapports Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) vous fournissent un accès par programmation aux données par le biais d’un ensemble d’API REST. Vous pouvez appeler ces API à partir des outils et langages de programmation.

L’API de création de rapports utilise [OAuth](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) pour autoriser l’accès aux API web.

Pour préparer votre accès à l’API de création de rapports, vous avez besoin d’effectuer ce qui suit :

1. [Attribuer des rôles](#assign-roles)
2. [Inscrire une application](#register-an-application)
3. [Accorder des autorisations](#grant-permissions)
4. [Rassembler les paramètres de configuration](#gather-configuration-settings)

## <a name="assign-roles"></a>Attribuer des rôles

Pour accéder aux données de rapports via l’API, vous devez avoir un des rôles suivants :

- Lecteur de sécurité

- Security Administrator

- Administrateur général


## <a name="register-an-application"></a>Inscrire une application

L’inscription est nécessaire, même si vous accédez à l’API de création de rapports à l’aide d’un script. Vous obtenez ainsi un **ID d’application**, qui est requis pour les appels d’autorisation et qui permet à votre code de recevoir des jetons.

Pour configurer votre annuaire et lui permettre d’accéder à l’API de création de rapports Azure AD, vous devez vous connecter au [portail Azure](https://portal.azure.com) avec un compte d’administrateur Azure, également membre du rôle d’annuaire **Administrateur général** dans votre locataire Azure AD.

> [!IMPORTANT]
> Les applications qui s’exécutent sous les informations d’identification pourvues de privilèges d’administrateur peuvent être très puissantes. Veillez donc à stocker les informations d’identification d’ID et du secret de l’application dans un emplacement sécurisé.
> 

**Pour inscrire une application Azure AD :**

1. Dans le panneau de navigation gauche du [portail Azure](https://portal.azure.com), sélectionnez **Azure Active Directory**.
   
    ![Inscription de l’application](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Dans la page **Azure Active Directory**, sélectionnez **Inscriptions des applications**.

    ![Inscription de l’application](./media/howto-configure-prerequisites-for-reporting-api/02.png) 

3. Dans la page **Inscriptions d’applications**, sélectionnez **Nouvelle inscription**.

    ![Inscription de l’application](./media/howto-configure-prerequisites-for-reporting-api/03.png)

4. Page **Inscription d’une application** :

    ![Inscription de l’application](./media/howto-configure-prerequisites-for-reporting-api/04.png)

    a. Dans la zone de texte **Nom**, tapez `Reporting API application`.

    b. Pour le **Type de comptes pris en charge**, sélectionnez **Comptes dans cet annuaire organisationnel uniquement**.

    c. Dans **URL de redirection**, sélectionnez la zone de texte **Web**, tapez `https://localhost`.

    d. Sélectionnez **Inscription**. 


## <a name="grant-permissions"></a>Accorder des autorisations 

En fonction de l’API à laquelle vous voulez accéder, vous devez accorder les autorisations suivantes à votre application :  

| API | Autorisation |
| --- | --- |
| Microsoft Azure Active Directory | Lire les données du répertoire |
| Microsoft Graph | Lire toutes les données du journal d’audit |


![Inscription de l’application](./media/howto-configure-prerequisites-for-reporting-api/36.png)

La section suivante répertorie les étapes pour les deux API. Si vous ne voulez pas accéder à l’une des API, vous pouvez ignorer les étapes qui s’y rapportent.

**Pour accorder des autorisations d’utiliser les API à votre application :**


1. Sélectionnez **Autorisations d’API**, puis **Ajouter une autorisation**. 

    ![Inscription de l’application](./media/howto-configure-prerequisites-for-reporting-api/05.png)

2. Dans la page **Demander des autorisations d’API**, localisez **Prendre en charge l’API héritée** **Azure Active Directory Graph**. 

    ![Inscription de l’application](./media/howto-configure-prerequisites-for-reporting-api/06.png)

3. Dans la page **Autorisations requises**, sélectionnez **Autorisations de l’application**, développez la case à cocher **Annuaire** **Annuaire.ReadAll**.  Sélectionnez **Ajouter des autorisations**.

    ![Inscription de l’application](./media/howto-configure-prerequisites-for-reporting-api/07.png)

4. Dans la page **Création de rapports d’application API - Autorisations de l’API**, sélectionnez **Accorder le consentement administrateur**. 

    ![Inscription de l’application](./media/howto-configure-prerequisites-for-reporting-api/08.png)

5. Remarque : **Microsoft Graph** est ajouté par défaut lors de l’inscription de l’API.

    ![Inscription de l’application](./media/howto-configure-prerequisites-for-reporting-api/15.png)

## <a name="gather-configuration-settings"></a>Rassembler les paramètres de configuration 

Cette section vous montre comment obtenir les paramètres suivants à partir de votre annuaire :

- Nom de domaine
- ID client
- Clé secrète client

Ces valeurs sont nécessaires lors de la configuration des appels à l’API de création de rapports. 

### <a name="get-your-domain-name"></a>Obtenir votre nom de domaine

**Pour obtenir votre nom de domaine :**

1. Dans le panneau de navigation gauche du [portail Azure](https://portal.azure.com), sélectionnez **Azure Active Directory**.
   
    ![Inscription de l’application](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Dans la page **Azure Active Directory**, sélectionnez **Noms de domaine personnalisés**.

    ![Inscription de l’application](./media/howto-configure-prerequisites-for-reporting-api/09.png) 

3. Copiez votre nom de domaine à partir de la liste des domaines.


### <a name="get-your-applications-client-id"></a>Obtenir l’ID client de l’application

**Pour obtenir l’ID client de votre application :**

1. Dans le volet de navigation gauche du [Portail Azure](https://portal.azure.com), cliquez sur **Azure Active Directory**.
   
    ![Inscription de l’application](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Sélectionnez votre application dans la page **Inscriptions des applications**.

3. Dans la page de l’application, accédez à **ID d’application** et sélectionnez **Cliquer pour copier**.

    ![Inscription de l’application](./media/howto-configure-prerequisites-for-reporting-api/11.png) 


### <a name="get-your-applications-client-secret"></a>Obtenir la clé secrète client de l’application
 Évitez les erreurs lors de la tentative d’accès aux journaux d’audit ou à de connexion à l’aide de l’API.

**Pour obtenir la clé secrète client de l’application :**

1. Dans le volet de navigation gauche du [Portail Azure](https://portal.azure.com), cliquez sur **Azure Active Directory**.
   
    ![Inscription de l’application](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2.  Sélectionnez votre application dans la page **Inscriptions des applications**.

3.  Sélectionnez **Certificats et secrets** dans la page **Application API**, dans la section **Secrets client**, cliquez sur **+Nouvelle clé secrète client**. 

    ![Inscription de l’application](./media/howto-configure-prerequisites-for-reporting-api/12.png)

5. Dans la page **Ajouter un secret client**, ajoutez :

    a. Dans la zone de texte **Description**, tapez `Reporting API`.

    b. Dans **Expire le**, sélectionnez **Dans 2 ans**.

    c. Cliquez sur **Enregistrer**.

    d. Copiez la valeur de la clé.

## <a name="troubleshoot-errors-in-the-reporting-api"></a>Résoudre les erreurs dans l'API de création de rapports

Cette section liste les messages d’erreur couramment rencontrés en accédant aux rapports d’activité à l’aide de l’API Microsoft Graph. Elle indique également les étapes à suivre pour les résoudre.


### <a name="error-failed-to-get-user-roles-from-microsoft-graph"></a>Erreur : Impossible d’obtenir les rôles d’utilisateur à partir de Microsoft Graph

 Connectez-vous à votre compte à l’aide des deux boutons de connexion dans l’interface utilisateur de l’Afficheur Graph pour éviter d’obtenir une erreur lors de la tentative de connexion à l’aide de l’Afficheur Graph. 

![Afficheur Graph](./media/troubleshoot-graph-api/graph-explorer.png)

### <a name="error-failed-to-do-premium-license-check-from-microsoft-graph"></a>Erreur : Impossible de vérifier la licence premium à partir de Microsoft Graph 

Si vous rencontrez ce message d’erreur lorsque vous tentez d’accéder aux connexions à l’aide de l’Afficheur Graph, choisissez **Modifier les autorisations** sous votre compte, dans la barre de navigation gauche, puis sélectionnez **Tasks.ReadWrite** et **Directory.Read.All**. 

![Modifier l’interface utilisateur des autorisations](./media/troubleshoot-graph-api/modify-permissions.png)


### <a name="error-tenant-is-not-b2c-or-tenant-doesnt-have-premium-license"></a>Erreur : Le locataires n’est pas B2C ou le locataire n'a pas de licence premium

L’accès aux rapports de connexion nécessite une licence Azure Active Directory Premium 1 (P1). Si vous voyez ce message d’erreur lorsque vous accédez aux connexions, vérifiez que votre locataire dispose bien d’une licence Azure AD P1.

### <a name="error-the-allowed-roles-does-not-include-user"></a>Erreur : Les rôles autorisés n’incluent pas Utilisateur. 

 Évitez les erreurs lors de la tentative d’accès aux journaux d’audit ou à de connexion à l’aide de l’API. Assurez-vous que votre compte fait partie du rôle **Lecteur de sécurité** ou **Lecteur de rapports** dans votre locataire Azure Active Directory.

### <a name="error-application-missing-aad-read-directory-data-permission"></a>Erreur : L'application ne dispose pas de l'autorisation AAD « Lire les données de l'annuaire » 

### <a name="error-application-missing-microsoft-api-read-all-audit-log-data-permission"></a>Erreur : L’application ne dispose pas de l’autorisation de l’API Microsoft Graph « Lire toutes les données du journal d’audit »

Pour vérifier que votre application est exécutée avec le bon ensemble d’autorisations, suivez les étapes décrites dans les [Prérequis pour accéder à l’API de création de rapports Azure Active Directory](howto-configure-prerequisites-for-reporting-api.md). 

## <a name="next-steps"></a>Étapes suivantes

* [Obtenir des données à l’aide de l’API de création de rapports Azure Active Directory avec des certificats](tutorial-access-api-with-certificates.md)
* [Informations de référence sur l’API d’audit](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Informations de référence sur l’API de création de rapports relatifs à l’activité de connexion](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)

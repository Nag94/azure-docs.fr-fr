---
title: Préparer des machines virtuelles Hyper-V pour les évaluer et les migrer avec Azure Migrate
description: Découvrez comment préparer l’évaluation/la migration des machines virtuelles Hyper-V avec Azure Migrate.
ms.topic: tutorial
ms.date: 01/01/2020
ms.custom: mvc
ms.openlocfilehash: 1d327f558806e0205540c183c56b92ba31e33cb7
ms.sourcegitcommit: f0f73c51441aeb04a5c21a6e3205b7f520f8b0e1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77031218"
---
# <a name="prepare-for-assessment-and-migration-of-hyper-v-vms-to-azure"></a>Préparer l’évaluation et la migration de machines virtuelles Hyper-V vers Azure

Cet article explique comment préparer l’évaluation et la migration de machines virtuelles Hyper-V locales vers Azure avec [Azure Migrate](migrate-services-overview.md).

[Azure Migrate](migrate-overview.md) fournit un hub d’outils qui vous permettent de découvrir, d’évaluer et de migrer des applications, une infrastructure et des charges de travail vers Microsoft Azure. Le hub comprend des outils Azure Migrate et des offres d’ISV (fournisseurs de logiciels indépendants) tiers.

Ce tutoriel est le premier d’une série qui montre comment évaluer et migrer des machines virtuelles Hyper-V vers Azure. Dans ce tutoriel, vous allez apprendre à :

> [!div class="checklist"]
> * Préparez Azure. Configurez des autorisations pour permettre à votre compte et vos ressources Azure de fonctionner avec Azure Migrate.
> * Préparer les machines virtuelles et les hôtes Hyper-V locaux pour l’évaluation des serveurs Vous pouvez effectuer la préparation à l’aide d’un script de configuration ou manuellement.
> * Préparez le déploiement de l’appliance Azure Migrate. L’appliance est utilisée pour détecter et évaluer les machines virtuelles locales.
> * Préparer les machines virtuelles et les hôtes Hyper-V locaux pour la migration des serveurs


> [!NOTE]
> Les tutoriels vous montrent le chemin de déploiement le plus simple pour un scénario donné afin que vous puissiez configurer rapidement une preuve de concept. Ils utilisent des options par défaut, le cas échéant, et ne montrent pas tous les paramètres et chemins possibles. Pour obtenir des instructions détaillées, passez en revue les procédures d’évaluation et de migration d’Hyper-V.


Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/pricing/free-trial/) avant de commencer.


## <a name="prepare-azure"></a>Préparer Azure

### <a name="azure-permissions"></a>Autorisations Azure

Vous devez définir des autorisations pour le déploiement Azure Migrate.

**Tâche** | **autorisations**
--- | ---
**Créer un projet Azure Migrate** | Votre compte Azure doit être autorisé à créer un projet.
**Inscrire l’appliance Azure Migrate** | Azure Migrate utilise une appliance Azure Migrate légère pour découvrir et évaluer les machines virtuelles Hyper-V à l’aide d’Azure Migrate Server Assessment. Cette appliance effectue la découverte des machines virtuelles et envoie les métadonnées et les données de performances des machines virtuelles à Azure Migrate.<br/><br/>Lors de l’inscription de l’appliance, les fournisseurs de ressources suivants sont inscrits dans l’abonnement choisi dans l’appliance : Microsoft.OffAzure, Microsoft.Migrate et Microsoft.KeyVault. L’inscription d’un fournisseur de ressources configure votre abonnement pour travailler avec le fournisseur de ressources. Pour inscrire les fournisseurs de ressources, vous avez besoin d’un rôle Contributeur ou Propriétaire sur l’abonnement.<br/><br/> Dans le cadre de l’intégration, Azure Migrate crée une application Azure Active Directory (Azure AD) :<br/> \- L’application Azure AD est utilisée pour la communication (authentification et autorisation) entre les agents exécutés sur l’appliance et leurs services respectifs exécutés sur Azure. Cette application n’a pas les privilèges requis pour effectuer des appels ARM ou des accès RBAC sur une ressource.



### <a name="assign-permissions-to-create-project"></a>Attribuer des autorisations pour créer un projet

Vérifiez que vous disposez des autorisations nécessaires pour créer un projet Azure Migrate.

1. Dans le portail Azure, ouvrez l’abonnement, puis sélectionnez **Contrôle d’accès (IAM)** .
2. Dans **Vérifier l’accès**, recherchez le compte approprié, puis cliquez dessus pour voir les autorisations correspondantes.
3. Vous devez disposer des autorisations de **Contributeur** ou de **Propriétaire**.
    - Si vous venez de créer un compte Azure gratuit, vous êtes le propriétaire de votre abonnement.
    - Si vous n’êtes pas le propriétaire de l’abonnement, demandez au propriétaire de vous attribuer le rôle.


### <a name="assign-permissions-to-register-the-appliance"></a>Affecter des autorisations pour inscrire l’appliance

Vous pouvez attribuer des autorisations pour permettre à Azure Migrate de créer l’application Azure AD pendant l’inscription de l’appliance, à l’aide de l’une des méthodes suivantes :

- L’administrateur général ou le locataire peuvent accorder des autorisations aux utilisateurs du locataire pour créer et inscrire des applications Azure AD.
- L’administrateur général ou le locataire peuvent attribuer au compte le rôle Développeur d’applications (qui dispose des autorisations appropriées).

> [!NOTE]
> - L’application n’a aucune autre autorisation d’accès sur l’abonnement que celles décrites ci-dessus.
> - Vous avez uniquement besoin de ces autorisations pour inscrire une nouvelle appliance. Vous pouvez supprimer les autorisations, une fois l’appliance configurée.


#### <a name="grant-account-permissions"></a>Accorder des autorisations au compte

L’administrateur général/locataire peut accorder des autorisations comme suit :

1. Dans Azure AD, l’administrateur général/locataire doit accéder à **Azure Active Directory** > **Utilisateurs** > **Paramètres utilisateur**.
2. L’administrateur doit affecter la valeur **Oui** à **Inscriptions des applications**.

    ![Autorisations Azure AD](./media/tutorial-prepare-hyper-v/aad.png)

> [!NOTE]
> Il s’agit d’un paramètre par défaut qui n’est pas sensible. [Plus d’informations](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance)



#### <a name="assign-application-developer-role"></a>Attribuer le rôle Développeur d’applications

L’administrateur général ou le locataire peuvent attribuer à un compte le rôle Développeur d’applications. [Plus d’informations](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal)


## <a name="prepare-hyper-v-for-assessment"></a>Préparer Hyper-V pour l’évaluation

Vous pouvez préparer Hyper-V pour l’évaluation de machines virtuelles manuellement ou à l’aide d’un script de configuration. Voici les préparatifs à effectuer, soit avec le script, soit [manuellement](#prepare-hyper-v-manually).

- [Vérifiez](migrate-support-matrix-hyper-v.md#hyper-v-host-requirements) les paramètres d’hôte Hyper-V et assurez-vous que les [ports requis](migrate-support-matrix-hyper-v.md#port-access) sont ouverts sur les hôtes Hyper-V.
- Configurez la communication à distance PowerShell sur chaque hôte, afin que l’appliance Azure Migrate puisse exécuter des commandes PowerShell sur l’hôte, via une connexion WinRM.
- Déléguez les informations d’identification si les disques de machine virtuelle se trouvent sur des partages SMB distants.
- Configurez un compte que l’appliance utilisera pour détecter les machines virtuelles sur les hôtes Hyper-V.
- Configurez les Services d’intégration Hyper-V sur chaque machine virtuelle que vous souhaitez découvrir et évaluer.



## <a name="prepare-with-a-script"></a>Préparer avec un script

Le script effectue les opérations suivantes :

- Vérifie que vous exécutez le script sur une version de PowerShell prise en charge.
- Vérifie que vous (l’utilisateur exécutant le script) disposez de privilèges Administrateur sur l’hôte Hyper-V.
- Vous permet de créer un compte d’utilisateur (non d’administrateur) local utilisé par le service Azure Migrate pour communiquer avec l’hôte Hyper-V. Ce compte d’utilisateur est ajouté aux groupes suivants sur l’ordinateur hôte :
    - Utilisateurs de gestion à distance
    - Administrateurs Hyper-V
    - Utilisateurs de l’Analyseur de performances
- Vérifie que l’hôte exécute une version prise en charge d’Hyper-V et le rôle Hyper-V.
- Active le service WinRM et ouvre les ports 5985 (HTTP) et 5986 (HTTPs) sur l’ordinateur hôte (nécessaire pour la collecte de métadonnées).
- Active la communication à distance PowerShell sur l’hôte.
- Vérifie que le service d’intégration Hyper-V est activé sur toutes les machines virtuelles gérées par l’hôte.
- Active CredSSP sur l’hôte si nécessaire.

Exécutez le script comme suit :

1. Assurez-vous que PowerShell version 4.0 ou ultérieure est installé sur l’hôte Hyper-V.
2. Téléchargez le script à partir du [Centre de téléchargement Microsoft](https://aka.ms/migrate/script/hyperv). Le script est signé par chiffrement par Microsoft.
3. Validez l’intégrité du script à l’aide de fichiers de hachage MD5 ou SHA256. Les valeurs de code de hachage sont indiquées ci-dessous. Pour générer le hachage pour le script, exécutez la commande suivante :
    ```
    C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]
    ```
    Exemple d’utilisation :
    ```
    C:\>CertUtil -HashFile C:\Users\Administrators\Desktop\ MicrosoftAzureMigrate-Hyper-V.ps1
    SHA256
    ```

4.  Après avoir validé l’intégrité du script, exécutez-le sur chaque hôte Hyper-V à l’aide de la commande PowerShell suivante :
    ```
    PS C:\Users\Administrators\Desktop> MicrosoftAzureMigrate-Hyper-V.ps1
    ```

### <a name="hashtag-values"></a>Valeurs de code de hachage

Les valeurs de hachage sont les suivantes :

| **Code de hachage** | **Valeur** |
| --- | --- |
| **MD5** | 0ef418f31915d01f896ac42a80dc414e |
| **SHA256** | 0ad60e7299925eff4d1ae9f1c7db485dc9316ef45b0964148a3c07c80761ade2 |


## <a name="prepare-hyper-v-manually"></a>Préparer Hyper-V manuellement

Appliquez les procédures de cette section pour préparer Hyper-V manuellement plutôt que par le biais d’un script.

### <a name="verify-powershell-version"></a>Vérifier la version de PowerShell

Assurez-vous que PowerShell version 4.0 ou ultérieure est installé sur l’hôte Hyper-V.



### <a name="set-up-an-account-for-vm-discovery"></a>Configurer un compte pour la découverte de machine virtuelle

Azure Migrate a besoin d’autorisations pour découvrir les machines virtuelles locales.

- Configurez un compte d’utilisateur local ou de domaine avec des autorisations d’administrateur sur le cluster ou les hôtes Hyper-V.

    - Vous avez besoin d’un compte unique pour tous les hôtes et clusters que vous souhaitez inclure dans la découverte.
    - Il peut s’agir d’un compte local ou de domaine. Nous vous recommandons de faire en sorte qu’il dispose d’autorisations d’administrateur sur les hôtes Hyper-V ou les clusters.
    - Autrement, si vous ne souhaitez pas affecter d’autorisations d’administrateur, les autorisations suivantes sont nécessaires :
        - Utilisateurs de gestion à distance
        - Administrateurs Hyper-V
        - Utilisateurs de l’Analyseur de performances

### <a name="verify-hyper-v-host-settings"></a>Vérifier les paramètres de l’hôte Hyper-V

1. Vérifiez la [configuration requise de l’hôte Hyper-V](migrate-support-matrix-hyper-v.md#hyper-v-host-requirements) pour l’évaluation du serveur.
2. Vérifiez que les [ports requis](migrate-support-matrix-hyper-v.md#port-access) sont ouverts sur les hôtes Hyper-V.

### <a name="enable-powershell-remoting-on-hosts"></a>Activer la communication à distance PowerShell sur les hôtes

Configurez la communication à distance PowerShell sur chaque hôte, comme suit :

1. Sur chaque hôte, ouvrez une console PowerShell en tant qu’administrateur.
2. Exécutez cette commande :

    ```
    Enable-PSRemoting -force
    ```
### <a name="enable-integration-services-on-vms"></a>Activer les services d’intégration sur les machines virtuelles

Les services d’intégration doivent être activés sur chaque machine virtuelle afin qu’Azure Migrate puisse capturer les informations du système d’exploitation sur la machine virtuelle.

Activez les [services d’intégration Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) sur chaque machine virtuelle que vous souhaitez découvrir et évaluer.


### <a name="enable-credssp-on-hosts"></a>Activer CredSSP sur les hôtes

Si l’hôte comprend des machines virtuelles dotées de disques situés sur des partages SMB, effectuez cette étape sur l’hôte.

- Vous pouvez exécuter cette commande à distance sur tous les hôtes Hyper-V.
- Si vous ajoutez de nouveaux nœuds hôtes sur un cluster, ils sont automatiquement ajoutés pour la découverte, mais vous devez activer manuellement CredSSP sur les nouveaux nœuds si nécessaire.

Pour opérer l’activation, procédez comme suit :

1. Identifiez les hôtes Hyper-V exécutant des machines virtuelles Hyper-V avec des disques sur des partages SMB.
2. Exécutez la commande suivante sur chaque hôte Hyper-V identifié :

    ```
    Enable-WSManCredSSP -Role Server -Force
    ```

Quand vous configurez l’appliance, vous terminez la configuration de CredSSP en [l’activant sur l’appliance](tutorial-assess-hyper-v.md#delegate-credentials-for-smb-vhds). Cela est décrit dans le didacticiel suivant de cette série.


## <a name="prepare-for-appliance-deployment"></a>Préparer le déploiement de l’appliance

Avant de configurer l’appliance Azure Migrate et de commencer l’évaluation dans le prochain tutoriel, préparez le déploiement de l’appliance.

1. [Vérifiez](migrate-appliance.md#appliance---hyper-v) la configuration requise de l’appliance.
2. [Passez en revue](migrate-appliance.md#url-access) les URL Azure auxquelles l’appliance doit accéder.
3. Passez en revue les données que l’appliance va collecter pendant la découverte et l’évaluation.
4. [Notez](migrate-appliance.md#collected-data---hyper-v) les conditions d’accès aux ports pour l’appliance.


## <a name="prepare-for-hyper-v-migration"></a>Préparer la migration Hyper-V

1. [Passez en revue](migrate-support-matrix-hyper-v-migration.md#hyper-v-hosts) la configuration requise de l’hôte Hyper-V pour la migration et les URL Azure auxquelles les hôtes et les clusters Hyper-V ont besoin d’accéder pour la migration de machine virtuelle.
2. [Passez en revue](migrate-support-matrix-hyper-v-migration.md#hyper-v-vms) la configuration requise des machines virtuelles Hyper-V que vous souhaitez migrer vers Azure.


## <a name="next-steps"></a>Étapes suivantes

Dans ce tutoriel, vous allez :

> [!div class="checklist"]
> * Configuré les autorisations de compte Azure.
> * Préparé les machines virtuelles et hôtes Hyper-V à l’évaluation et à la migration.
> * Préparé le déploiement de l’appliance Azure Migrate.

Passez au tutoriel suivant pour créer un projet Azure Migrate, déployer l’appliance, et détecter et évaluer les machines virtuelles Hyper-V à migrer vers Azure.

> [!div class="nextstepaction"]
> [Évaluer les machines virtuelles Hyper-V](./tutorial-assess-hyper-v.md)

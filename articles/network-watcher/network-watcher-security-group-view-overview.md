---
title: Présentation de l’affichage des groupes de sécurité dans Azure Network Watcher | Microsoft Docs
description: Cette page fournit une vue d’ensemble de la fonctionnalité d’affichage des groupes de sécurité de Network Watcher.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: damendo
ms.openlocfilehash: 18619cc0f7a21a39d0802575c484c68c5fa1a624
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76840755"
---
# <a name="introduction-to-effective-security-rules-view-in-azure-network-watcher"></a>Présentation de la vue Obtenir les règles de sécurité efficaces dans Azure Network Watcher

Les groupes de sécurité réseau sont associés à un niveau de sous-réseau ou à un niveau de carte réseau. Lorsqu’il est associé à un niveau de sous-réseau, il s’applique à toutes les instances de machine virtuelle du sous-réseau. La vue Obtenir les règles de sécurité efficaces retourne l’ensemble des règles et des groupes de sécurité réseau configurés qui sont associés à un niveau de sous-réseau et de carte réseau pour une machine virtuelle fournissant des informations sur la configuration. En outre, les règles de sécurité effectives sont renvoyées pour chacune des cartes réseau d’une machine virtuelle. La vue Obtenir les règles de sécurité efficaces vous permet d’évaluer les vulnérabilités réseau d’une machine virtuelle comme les ports ouverts. Vous pouvez également vérifier si votre Groupe de sécurité réseau fonctionne comme prévu en [comparant les règles de sécurité approuvées et configurées](network-watcher-nsg-auditing-powershell.md).

Un cas d’utilisation plus poussée concerne l’audit et la conformité de la sécurité. Vous pouvez définir un ensemble normatif de règles de sécurité comme modèle pour la gouvernance de la sécurité de votre organisation. Un audit de conformité périodique peut être implémenté de façon programmatique en comparant les règles normatives avec les règles effectives pour toutes les machines virtuelles de votre réseau.

Sur le portail, à chaque interface réseau correspondent des règles qui sont regroupées selon qu’elles s’appliquent au trafic entrant ou au trafic sortant. Cela permet de savoir facilement quelles sont les règles appliquées à une machine virtuelle. Un bouton de téléchargement est fourni pour télécharger simplement toutes les règles de sécurité, quel que soit l’onglet, dans un fichier CSV.

![affichage des groupes de sécurité][1]

Les règles peuvent être sélectionnées et un nouveau panneau s’ouvre pour afficher le groupe de sécurité réseau et les préfixes source et de destination. Dans ce panneau, vous pouvez naviguer directement vers la ressource de groupe de sécurité réseau.

![exploration][2]

### <a name="next-steps"></a>Étapes suivantes

Découvrez comment effectuer un audit de vos paramètres de groupe de sécurité réseau en consultant [Automate NSG auditing with Azure Network Watcher Security group view](network-watcher-nsg-auditing-powershell.md) (Automatiser l’audit des groupes de sécurité réseau avec l’affichage des groupes de sécurité réseau Azure Network Watcher)

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png










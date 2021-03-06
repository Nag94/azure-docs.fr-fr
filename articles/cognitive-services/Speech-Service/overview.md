---
title: Qu’est-ce que le service de reconnaissance vocale ?
titleSuffix: Azure Cognitive Services
description: Le service Speech rassemble la reconnaissance vocale, la synthèse vocale et la traduction vocale dans un seul abonnement Azure. Ajoutez les services Speech à vos applications, outils et appareils avec le SDK Speech, le SDK Speech Devices ou les API REST.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 02/10/2020
ms.author: dapine
ms.openlocfilehash: 7ddfae430e6aa4ec9549e40c937e5edcfd927f6d
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77119928"
---
# <a name="what-is-the-speech-service"></a>Qu’est-ce que le service de reconnaissance vocale ?

Le service Speech réunit la reconnaissance vocale, la synthèse vocale et la traduction vocale dans un même abonnement Azure. Vous pouvez aisément activer vos applications, outils et appareils pour les services Speech avec le [Kit de développement logiciel (SDK) Speech](speech-sdk-reference.md), le [Kit de développement logiciel (SDK) Speech Devices](https://aka.ms/sdsdk-quickstart) ou des [API REST](rest-apis.md).

> [!IMPORTANT]
> Le service Speech a remplacé l’API Reconnaissance vocale Bing, Traduction de conversation Translator Speech et Custom Speech. Pour obtenir des instructions de migration, voir _Guides pratiques > Migration_.

Ces fonctionnalités constituent le service Speech. Pour en savoir plus sur les cas d’utilisation courants de chaque fonctionnalité, utilisez les liens figurant dans ce tableau, ou parcourez la référence de l’API.

| Service | Fonctionnalité | Description | Kit SDK | REST |
| ------- | ------- | ----------- | --- | ---- |
| [Reconnaissance vocale](speech-to-text.md) | Reconnaissance vocale | La reconnaissance vocale transcrit en temps réel des flux audio en texte que vos applications, outils ou appareils peuvent utiliser ou afficher. Utilisez la reconnaissance vocale avec [LUIS (Language Understanding Intelligent Service)](https://docs.microsoft.com/azure/cognitive-services/luis/) pour déduire les intentions de l’utilisateur à partir des transcriptions et agir sur des commandes vocales. | [Oui](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Oui](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Transcription par lot](batch-transcription.md) | La transcription par lot permet de transcrire une reconnaissance vocale asynchrone de gros volumes de données. Il s’agit d’un service basé sur REST qui utilise le même point de terminaison que la personnalisation et la gestion des modèles. | Non | [Oui](https://westus.cris.ai/swagger/ui/index) |
| | [Conversation multi-appareil](multi-device-conversation.md) | Connecter plusieurs appareils ou clients dans une conversation pour envoyer des messages vocaux ou textuels, avec une prise en charge simple de la transcription et de la traduction| Oui | Non |
| | [Transcription de conversation](conversation-transcription-service.md) | Permet la reconnaissance vocale en temps réel, l’identification de l’orateur et la structuration (diarisation). Il est parfait pour la transcription de rencontres en personne, avec possibilité de distinguer les orateurs. | Oui | Non |
| | [Créer des modèles vocaux personnalisés](#customize-your-speech-experience) | Si vous utilisez la reconnaissance vocale pour la reconnaissance et la transcription dans un environnement unique, vous pouvez créer et former des modèles de prononciation, de langue et acoustiques personnalisés pour prendre en compte un bruit ambiant ou le vocabulaire spécifique d’un secteur. | Non | [Oui](https://westus.cris.ai/swagger/ui/index) |
| [Synthèse vocale](text-to-speech.md) | Synthèse vocale | La synthèse vocale convertit le texte d’entrée en parole synthétisée quasi humaine avec le [langage SSML (Speech Synthesis Markup Language)](speech-synthesis-markup.md). Faites votre choix parmi les voix standard et les voix neuronales (voir [Prise en charge linguistique](language-support.md)). | [Oui](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Oui](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Créer des voix personnalisées](#customize-your-speech-experience) | Créez des polices de voix personnalisées propres à vos marques ou produits. | Non | [Oui](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Traduction vocale](speech-translation.md) | Traduction vocale | La traduction vocale permet à vos applications, outils et appareils d’effectuer de la traduction multilingue en temps réel de la parole. Utilisez ce service pour la traduction de voix en voix et de voix en texte. | [Oui](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | Non |
| [Assistants vocaux](voice-assistants.md) | Assistants vocaux | Les assistants vocaux qui utilisent le service Speech permettent aux développeurs de créer des interfaces conversationnelles naturelles pour leurs applications et leurs expériences. Le service d’assistant vocal permet une interaction rapide et fiable entre un appareil et une implémentation d’assistant qui utilise le canal Direct Line Speech de Bot Framework ou le service intégré Custom Commands (préversion) pour réaliser la tâche. | [Oui](voice-assistants.md) | Non |

## <a name="try-the-speech-service"></a>Essayer le service Speech

Nous proposons des démarrages rapides pour la plupart des langages de programmation populaires, conçus pour que votre code soit opérationnel en moins de 10 minutes. Ce tableau répertorie les démarrages rapides les plus populaires pour chaque fonctionnalité. Utilisez le volet de navigation de gauche pour explorer les langages et plateformes supplémentaires.

| Reconnaissance vocale (SDK) | Synthèse vocale (SDK) | Traduction (SDK) |
| -------------------- | -------------------- | ----------------- |
| [Reconnaître la parole à partir d’un fichier audio](quickstarts/speech-to-text-from-file.md) | [Synthétiser la parole vers un fichier audio](quickstarts/text-to-speech-audio-file.md) | [Traduire la reconnaissance vocale](quickstarts/translate-speech-to-text.md) |
| [Reconnaître la parole avec un micro](quickstarts/speech-to-text-from-microphone.md) | [Synthétiser la parole vers un haut-parleur](quickstarts/text-to-speech.md) | [Traduire la parole en plusieurs langues cibles](quickstarts/translate-speech-to-text-multiple-languages.md) |
| [Reconnaître la parole stockée dans un stockage de blobs](quickstarts/from-blob.md) | [Asynchroniser la synthèse pour l’audio long ](quickstarts/text-to-speech/async-synthesis-long-form-audio.md) | [Traduire la parole en parole](quickstarts/translate-speech-to-speech.md) |

> [!NOTE]
> La reconnaissance vocale et la synthèse vocale ont également des points de terminaison REST et des démarrages rapides associés.

Une fois que vous aurez eu l’occasion d’utiliser le service Speech, essayez notre tutoriel qui vous apprendra à reconnaître les intentions d’un discours à l’aide du SDK Speech et de LUIS.

- [Tutoriel : Effectuer une reconnaissance des intentions du discours à l’aide du Kit de développement logiciel (SDK) Speech et de LUIS, C#](how-to-recognize-intents-from-speech-csharp.md)
- [Tutoriel : Activer les fonctions vocales sur votre bot avec le SDK Speech, C#](tutorial-voice-enable-your-bot-speech-sdk.md)
- [Tutoriel : Créer une application Flask pour la traduction de texte, l’analyse de sentiments et la synthèse vocale de texte traduit, REST](https://docs.microsoft.com/azure/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis?toc=%2fazure%2fcognitive-services%2fspeech-service%2ftoc.json&bc=%2fazure%2fcognitive-services%2fspeech-service%2fbreadcrumb%2ftoc.json&toc=%2Fen-us%2Fazure%2Fcognitive-services%2Fspeech-service%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)

## <a name="get-sample-code"></a>Obtenir un exemple de code

Un exemple de code est disponible sur GitHub pour le service Speech. Ces exemples couvrent des scénarios courants tels que la lecture du signal audio d’un fichier ou d’un flux, la reconnaissance continue et ponctuelle, et l’utilisation de modèles personnalisés. Pour voir les exemples SDK et REST, suivez ces liens :

- [Exemples de reconnaissance vocale, de synthèse vocale et de traduction vocale (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Exemples de transcription par lot (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
- [Exemples de synthèse vocale (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)
- [Exemples d’assistant vocal (Kit de développement logiciel [SDK])](https://aka.ms/csspeech/samples)

## <a name="customize-your-speech-experience"></a>Personnaliser votre expérience de reconnaissance vocale

Le service Speech fonctionne bien avec les modèles intégrés. Cependant, vous pouvez personnaliser et optimiser l’expérience pour votre produit ou environnement. Les options de personnalisation vont de l’ajustement du modèle acoustique aux polices de voix uniques pour votre marque.

| Speech Service | Plateforme | Description |
| -------------- | -------- | ----------- |
| Reconnaissance vocale | [Discours personnalisé](https://aka.ms/customspeech) | Personnalisez les modèles de reconnaissance vocale en fonction de vos besoins et des données disponibles. Éliminez les obstacles à la reconnaissance vocale, tels que le style d’élocution, le bruit de fond et le vocabulaire. |
| Synthèse vocale | [Custom Voice](https://aka.ms/customvoice) | Créez une voix unique et reconnaissable pour vos applications de synthèse vocale, avec vos données vocales disponibles. Vous pouvez ensuite affiner le réglage des sorties vocales en ajustant tout un ensemble de paramètres vocaux. |

## <a name="reference-docs"></a>Documents de référence

- [Kit de développement logiciel (SDK) de reconnaissance vocale](speech-sdk-reference.md)
- [SDK Speech Devices](speech-devices-sdk.md)
- [API REST : Reconnaissance vocale](rest-speech-to-text.md)
- [API REST : Synthèse vocale](rest-text-to-speech.md)
- [API REST : Transcription et personnalisation par lot](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Obtenir gratuitement une clé d’abonnement au service Speech](get-started.md)

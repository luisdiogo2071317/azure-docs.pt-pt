---
title: Sobre o reconhecimento de voz para texto | Microsoft Docs
description: Uma descrição geral das funcionalidades do reconhecimento de voz a API de texto.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 2eee1c6f9158f128ed5ffe575f8f498f1d3eb5e9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355688"
---
# <a name="about-the-speech-to-text-api"></a>Sobre o reconhecimento de voz para texto API

O **reconhecimento de voz para texto** API *transcribes* fluxos de áudio texto que a aplicação pode apresentar ao utilizador ou agir como comando de entrada. As APIs pode ser utilizadas com uma biblioteca de cliente SDK (para idiomas e plataformas suportadas) ou uma API REST.

O **reconhecimento de voz para texto** API oferece as seguintes funcionalidades:

- Avançadas tecnologia de reconhecimento de voz da Microsoft — a mesma utilizado pelo Cortana, Office e outros produtos Microsoft.

- Reconhecimento contínuo em tempo real. **Reconhecimento de voz para texto** permite aos utilizadores transcribe áudio texto em tempo real. Também suporta receber resultados intermédios das palavras que tenham sido reconhecidos até ao momento. O serviço reconhece automaticamente ao fim de reconhecimento de voz. Os utilizadores também podem escolher opções de formatação adicionais, incluindo maiúsculas/minúsculas e pontuação, profanity máscara e normalização de texto.

- Otimizado **reconhecimento de voz para texto** resultados para conversação interativa, e cenários de ditado. 

- Suporte para vários idiomas ditas no dialetos vários. Para a lista completa dos idiomas suportados em cada modo de reconhecimento, consulte [idiomas suportados](supported-languages.md#speech-to-text).

- Idioma personalizado e acústica modelos, pelo que pode personalizar a aplicação a forma dos seus utilizadores de falando, utilizando um ambiente e vocabulário especializado.

- Compreensão de linguagem natural. Através da integração com [compreensão de idiomas](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS), pode derivar pendentes e as entidades de reconhecimento de voz. Os utilizadores não têm saber vocabulário da sua aplicação, mas podem descrevem o que pretendam para as suas próprias palavras.

## <a name="api-capabilities"></a>Capacidades de API

Algumas funcionalidades do **reconhecimento de voz para texto** não estão disponíveis através de REST API. A tabela seguinte resume as capacidades de cada método de aceder à API.

| Caso de utilização | REST | SDKs |
|-----|-----|-----|----|
| Transcribe um utterance curto, tal como um comando (comprimento < 15 s); nenhuma provisórias resultados | Sim | Sim |
| Transcribe um utterance mais longo (> 15 s) | Não | Sim |
| Transcribe áudio transmissão em fluxo com resultados provisórias opcionais | Não | Sim |
| Compreender pendentes de orador através de LUIS | Não\* | Sim |

\* *Pendentes LUIS e entidades podem derivar através de uma subscrição de LUIS separada. Com esta subscrição, o SDK pode chamar LUIS para si e resultados de entidade e intenções, bem como transcriptions de reconhecimento de voz. Com a API REST, pode chamar LUIS para derivar de entidades com a sua subscrição LUIS e pendentes.*

## <a name="next-steps"></a>Passos Seguintes

* [Obter a sua subscrição de avaliação de reconhecimento de voz](https://azure.microsoft.com/try/cognitive-services/)
* [Ver como reconhecer voz em c#](quickstart-csharp-windows.md)

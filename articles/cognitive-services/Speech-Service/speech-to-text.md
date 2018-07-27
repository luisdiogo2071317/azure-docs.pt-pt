---
title: Sobre a conversão de voz em texto
description: Uma descrição geral das funcionalidades de voz para a API de texto.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 464eb9bde689aa947a5b1e2900ae0fc4670fed6e
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285481"
---
# <a name="about-the-speech-to-text-api"></a>Sobre a conversão de voz em texto API

O **conversão de voz em texto** API *converte* fluxos de áudio em texto que seu aplicativo pode apresentar ao utilizador ou agir como comando de entrada. As APIs podem ser utilizadas com uma biblioteca de cliente do SDK (para as plataformas suportadas e idiomas) ou uma API REST.

O **conversão de voz em texto** API oferece as seguintes funcionalidades:

- Advanced tecnologia de reconhecimento de voz da Microsoft — o mesmo usado pelo Cortana, Office e outros produtos da Microsoft.

- Reconhecimento contínuo em tempo real. **Conversão de voz em texto** permite que os usuários a transcrição de áudio em texto em tempo real. Também suporta a receber os resultados intermediários das palavras que tem sido reconhecidos até agora. O serviço reconhece automaticamente o final de voz. Os utilizadores também podem escolher opções de formatação adicionais, incluindo capitalização e pontuação, linguagem inapropriada máscara e normalização do texto inversa.

- Otimizado **conversão de voz em texto** resultados para conversação interativo e cenários de ditado. 

- Suporte para vários idiomas e dialetos. Para obter a lista completa das linguagens suportadas em cada modo de reconhecimento, consulte [idiomas suportados](supported-languages.md#speech-to-text).

- Personalizado modelos acústicos e de linguagem, pelo que pode personalizar a aplicação do vocabulário de domínio especializadas dos seus utilizadores, falando em ambiente e a forma de falar.

- Compreensão de linguagem natural. Através da integração com [compreensão de idiomas](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS), pode derivar intenções e entidades de voz. Os utilizadores não precisam conhecer o vocabulário da sua aplicação, mas podem descrever o que eles querem em suas próprias palavras.

## <a name="api-capabilities"></a>Capacidades de API

Alguns recursos do **conversão de voz em texto** API não estão disponíveis através de REST. A tabela seguinte resume as capacidades de cada método de acessar a API.

| Caso de utilização | REST | SDKs |
|-----|-----|-----|----|
| Transcrição de uma expressão de curto, tal como um comando (comprimento < 15 s); Não existem resultados intermediárias | Sim | Sim |
| Transcrição de uma expressão maior (> 15 s) | Não | Sim |
| Transcrição de áudio de transmissão em fluxo com resultados provisórias opcionais | Não | Sim |
| Compreender os objetivos de orador por meio do LUIS | Não\* | Sim |

\* *LUIS intenções e entidades podem ser obtidas através de uma subscrição separada do LUIS. Com esta subscrição, o SDK pode chamar o LUIS para e fornecer resultados de entidade e propósito, bem como as transcrições de voz. Com a API REST, pode chamar o LUIS para derivar intenções e entidades com a sua subscrição do LUIS.*

## <a name="next-steps"></a>Passos Seguintes

* [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Veja como a reconhecer a conversão de voz em c#](quickstart-csharp-dotnet-windows.md)

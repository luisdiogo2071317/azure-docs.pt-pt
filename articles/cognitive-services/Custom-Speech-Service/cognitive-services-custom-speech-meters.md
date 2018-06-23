---
title: Personalizado medidores do serviço de reconhecimento de voz e quotas no Azure | Microsoft Docs
description: Informações sobre medidores e quotas do serviço de reconhecimento de voz personalizadas no Azure.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/08/2017
ms.author: panosper
ms.openlocfilehash: d2225dec818c600febfad2f9ebc42594f6ac09ac
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351470"
---
# <a name="custom-speech-service-meters-and-quotas"></a>Personalizado medidores do serviço de reconhecimento de voz e quotas

Com o serviço de reconhecimento de voz de personalizado baseado na nuvem, pode personalizar os modelos de reconhecimento de voz para reconhecimento de voz para texto transcription.

Para começar a utilizar o serviço de reconhecimento de voz personalizadas, vá para o [portal do serviço de reconhecimento de voz personalizadas](https://cris.ai).

Para o atual preços medidores, vá para o [serviços cognitivos preços do serviço de reconhecimento de voz personalizadas](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/) página.

## <a name="tiers-explained"></a>Camadas explicadas
Para testar e protótipo apenas, iremos propor utilizar F0 escalão gratuito. Para sistemas de produção, iremos propor utilizar o escalão de S2. Utilizando a camada de S2 pode dimensionar a sua implementação para o número de unidades de escala (SUs) que o seu cenário exigir.

> [!NOTE]
> *Não é possível* migrar entre a camada de F0 e a camada de S2.
>

## <a name="meters-explained"></a>Medidores explicados

### <a name="scale-out"></a>Ampliar
Aumentar horizontalmente é uma funcionalidade nova que tenha lançada com o novo modelo de preços. Ao utilizar aumentar horizontalmente, pode controlar o número de pedidos simultâneos que pode processar o seu modelo.

Pode definir pedidos em simultâneo utilizando a medida SU no **criar o modelo de implementação** vista. Para obter mais informações, consulte [criar um ponto final de reconhecimento de voz para texto personalizado](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-endpoint.md). Consoante a quantidade de tráfego que envisage a consumir o modelo, pode escolher um número adequado de SUs. 

> [!NOTE]
> Cada unidade de escala garante que os 5 pedidos em simultâneo. Pode comprar 1 ou mais SUs, conforme apropriado. Porque o número de SUs aumenta em incrementos de 1, o número de pedidos simultâneos garantido aumentam a incrementos de 5.
>

### <a name="log-management"></a>Gestão do registo
Pode optar por desactivar rastreios de áudio para um modelo recentemente implementado um custo adicional. Serviço de reconhecimento de voz personalizadas não regista os pedidos de áudio ou as transcrições esse modelo.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre como utilizar o serviço de reconhecimento de voz personalizadas, vá para o [portal do serviço de reconhecimento de voz personalizadas](https://cris.ai).

* [Introdução](cognitive-services-custom-speech-get-started.md)
* [FAQ](cognitive-services-custom-speech-faq.md)
* [Glossário](cognitive-services-custom-speech-glossary.md)
 
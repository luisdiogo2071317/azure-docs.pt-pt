---
title: Medidores do serviço de voz personalizada e de quotas no Azure | Documentos da Microsoft
description: Informações sobre medidores e quotas de serviço de voz personalizada no Azure.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 07/08/2017
ms.author: panosper
ms.openlocfilehash: eb5a9e4a3a27a2a8c044749b8b4df0f198583bde
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55223668"
---
# <a name="custom-speech-service-meters-and-quotas"></a>Quotas e medidores do serviço de voz personalizada

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Com o serviço de voz de personalizada com base na cloud, pode personalizar modelos de voz para transcrição de voz em texto.

Para começar a utilizar o serviço de voz personalizada, vá para o [portal do serviço de voz personalizada](https://cris.ai).

Para a atual preços medidores, vá para o [preços dos serviços cognitivos para o serviço de voz personalizada](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/) página.

## <a name="tiers-explained"></a>Escalões explicados
Para teste e apenas o protótipo, propomos que utilize o escalão gratuito do F0. Para sistemas de produção, propomos de utilizar o escalão S2. Ao utilizar o escalão S2 pode dimensionar a sua implementação para o número de unidades de escala (SUs) que requer o seu cenário.

> [!NOTE]
> *Não é possível* migrar entre a camada de F0 e o escalão S2.
>

## <a name="meters-explained"></a>Medidores explicados

### <a name="scale-out"></a>Ampliar
Aumentar horizontalmente é um novo recurso que lançámos com o novo modelo de preços. Ao utilizar aumentar horizontalmente, pode controlar o número de pedidos simultâneos que consegue processar o seu modelo.

Pode definir pedidos em simultâneo com a medida SU no **criar o modelo de implementação** vista. Para obter mais informações, consulte [criar um ponto de final de voz em texto personalizado](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-endpoint.md). Consoante a quantidade de tráfego que prever o consumo de modelo, pode escolher um número adequado de SUs. 

> [!NOTE]
> Cada unidade de escala garante a 5 pedidos em simultâneo. Pode comprar 1 ou mais SUs, conforme apropriado. Uma vez que aumenta o número de SUs em incrementos de 1, o número de pedidos simultâneos é garantido para aumentar em incrementos de 5.
>

### <a name="log-management"></a>Gestão de registos
Pode optar por desativar rastreios de áudio para um modelo recentemente implementado a um custo adicional. Serviço de voz personalizada não regista os pedidos de áudio ou as transcrições desse modelo.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre como utilizar o serviço de voz personalizada, vá para o [portal do serviço de voz personalizada](https://cris.ai).

* [Introdução](cognitive-services-custom-speech-get-started.md)
* [FAQ](cognitive-services-custom-speech-faq.md)
* [Glossário](cognitive-services-custom-speech-glossary.md)
 

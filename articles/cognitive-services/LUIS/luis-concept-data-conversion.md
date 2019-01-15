---
title: Conversão de dados
titleSuffix: Language Understanding - Azure Cognitive Services
description: Saiba como expressões podem ser alterados antes de previsões de indisponibilidade na compreensão de idiomas (LUIS)
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: diberry
ms.openlocfilehash: 46ad8beb81f42fa19b832c63355488f31da023f8
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/14/2019
ms.locfileid: "54259381"
---
# <a name="convert-data-format-of-utterances"></a>Converter o formato de dados de expressões
LUIS utiliza o serviço de voz de serviços cognitivos para converter expressões com expressões faladas em expressões de texto antes de predição. 

## <a name="speech-to-intent-conversion-concepts"></a>Conversão de voz em conceitos de conversão de intenção
Conversão de voz para texto em LUIS permite-lhe enviar expressões faladas com um ponto de extremidade e receber uma resposta de predição do LUIS. O processo é uma integração entre o [voz](https://docs.microsoft.com/azure/cognitive-services/Speech) serviço com os LUIS. 

### <a name="key-requirements"></a>Requisitos chave
Não é necessário criar uma **API de voz do Bing** chave para essa integração. R **compreensão de idiomas** chave criada no portal do Azure funciona para essa integração. Não utilize a chave de arranque do LUIS, ele não funcionará para essa integração.

### <a name="new-endpoint"></a>Novo ponto final 
Esta integração acarreta um novo ponto final e [preços](luis-boundaries.md#key-limits) modelo. O ponto final, através do [SDK de voz](https://github.com/Azure-Samples/cognitive-services-speech-sdk), é capaz de receber ambos falado e expressões de texto, permitindo que utilizá-lo como um único ponto final. 

### <a name="quota-usage"></a>Utilização da quota
Ver [limites de chaves](luis-boundaries.md#key-limits) para obter informações. 

### <a name="data-retention"></a>Retenção de dados
Os dados enviados para o ponto final, através do SDK de voz, independentemente se for voz ou de texto, só são utilizados para melhorar o seu modelo de voz. Não é utilizado para além de seu modelo para melhorar a conversão de voz ou LUIS numa capacidade geral. Quando a aplicação do LUIS é eliminada, os dados mantidos também são eliminados.

<!-- TBD: Machine translation conversion concepts -->

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Utilize a voz para texto](luis-tutorial-speech-to-intent.md)


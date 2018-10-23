---
title: Conceitos de conversão de dados do LUIS - compreensão de idiomas
titleSuffix: Azure Cognitive Services
description: Saiba como expressões podem ser alterados antes de previsões de indisponibilidade na compreensão de idiomas (LUIS)
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 9324f7b4f7bed844f16d17b8960878892be4b165
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638397"
---
# <a name="data-conversion-concepts-in-luis"></a>Conceitos de conversão de dados do LUIS
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


---
title: O que é uma pontuação de BLEU? -Tradutor personalizado
titleSuffix: Azure Cognitive Services
description: BLEU é uma medida das diferenças entre uma tradução automática e um ou mais traduções de referência criada por humanos da mesma sentença de origem. O algoritmo BLEU compara frases consecutivos da tradução automática com as frases consecutivas que se encontra na tradução de referência e conta o número de correspondências, de forma ponderada.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: custom-translator
ms.topic: article
ms.date: 11/13/2018
ms.author: v-rada
ms.openlocfilehash: b0fd9777b8c830a06195dbc22f0bb9081ff9753a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55222019"
---
# <a name="what-is-a-bleu-score"></a>O que é uma pontuação de BLEU?

[BLEU (bilingue avaliação Understudy)](https://en.wikipedia.org/wiki/BLEU) é uma medida das diferenças entre uma tradução automática e um ou mais traduções de referência criada por humanos da mesma sentença de origem.

## <a name="scoring-process"></a>Processo de classificação

O algoritmo BLEU compara frases consecutivos da tradução automática com as frases consecutivas que se encontra na tradução de referência e conta o número de correspondências, de forma ponderada. Estes correspondências são independentes de posição. Um maior grau de correspondência indica um maior grau de semelhança com a tradução de referência e a pontuação mais alta. Inteligibilidade da e correção gramatical não são levados em consideração.

## <a name="how-bleu-works"></a>Como funciona a BLEU?

Força do BLEU é que ele correlaciona bem com bom senso humano por erros de bom senso de frase individualmente uma média ao longo de um corpo de teste, em vez de tentar planejar o senso de humano exato para todas as frases.

É uma discussão mais ampla das pontuações BLEU [aqui](https://youtu.be/-UqDljMymMg).

Resultados BLEU dependem vivamente o alcance do seu domínio, a consistência dos dados de teste com a formação e a otimização de dados e a quantidade de dados tiver disponíveis para preparar. Se seus modelos foram treinados num domínio estreito e seus dados de preparação são consistentes com os seus dados de teste, pode esperar uma pontuação de BLEU elevada.

>[!NOTE]
>Uma comparação entre as pontuações BLEU só está justificáveis quando BLEU resultados são comparados com o mesmo conjunto de teste, o mesmo par de idioma e o mesmo motor de MT. Uma pontuação de BLEU a partir de um conjunto de teste diferentes está vinculada a ser diferente.

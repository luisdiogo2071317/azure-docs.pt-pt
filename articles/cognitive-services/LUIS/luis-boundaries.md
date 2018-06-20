---
title: Limites de compreender (LUIS) de idioma | Microsoft Docs
titleSuffix: Azure
description: Este artigo contém limites conhecidos do LUIS.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 7f46e55e11c4eb68b515a743b0f51392ffc1269e
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266809"
---
# <a name="luis-boundaries"></a>Limites de LUIS
LUIS tem várias áreas de limites. O primeiro é o [limites de modelo](#model-boundaries), que controla o pendentes, as entidades e funcionalidades no LUIS. A área do segundo [limites de quota](#key-limits) com base no tipo de chave. É uma terceira área de limites a [combinação de teclado](#keyboard-controls) para controlar o Web site LUIS. Uma quarta área é o [mapeamento de região do mundo](luis-reference-regions.md) entre LUIS criação de Web site e o LUIS [endpoint](luis-glossary.md#endpoint) APIs. 


## <a name="model-boundaries"></a>Limites de modelo

|Área|Limite|
|--|:--|--|
| [Nome da aplicação][luis-get-started-create-app] | * Máximo de carateres predefinido |
| [Teste do batch][batch-testing]| conjuntos de dados de 10, 1000 utterances por conjunto de dados|
| **[Compostos](./luis-concept-entity-types.md)|100 com até 10 subordinados |
| Lista explícita | 50 por aplicação|
| **[Hierárquica](./luis-concept-entity-types.md) |100 com até 10 subordinados |
| [Pendentes][intents]|500 por aplicação<br>[Baseada em distribuição](https://github.com/Microsoft/botbuilder-tools/tree/master/Dispatch) aplicação possui origens de emissão 500 correspondente|
| [Lista de entidades](./luis-concept-entity-types.md) | Principal: Step-by 50, subordinado: 20.000 itens. Nome canónico é * predefinido máximo de carateres. Sinónimos não tem nenhuma restrição de comprimento. |
| [Padrões](luis-concept-patterns.md)|500 padrões por aplicação.<br>Comprimento máximo de padrão é de 400 carateres.<br>3 entidades Pattern.any por padrão<br>Máximo de 2 textos opcionais aninhados padrão|
| [Pattern.any](./luis-concept-entity-types.md)|100 por aplicação, 3 entidades pattern.any por padrão |
| [Lista de expressão][phrase-list]|10 frase listas, 5000 itens por lista|
| [Entidades prebuilt](./Pre-builtEntities.md) | Sem limite|
| [Entidades de expressão regular](./luis-concept-entity-types.md)|20 entidades<br>máximo de 500 carateres. Por padrão de entidade de expressão regular|
| [Funções](luis-concept-roles.md)|300 funções por aplicação. 10 funções por entidade|
| **[Simples](./luis-concept-entity-types.md)| 100 entidades|
| [Utterance][utterances] | 500 carateres|
| [Utterances][utterances] | 15 000 por aplicação|
| [Nome da versão][luis-how-to-manage-versions] | 10 carateres de restringida ao período e alfanuméricos (.) |

* Máximo de carateres predefinido é de 50 carateres. 

* * A contagem total de entidades simples, hierárquicas e compostas não pode exceder os 100. A contagem total de entidades hierárquicas, entidades compostas, entidades simples e entidades de elementos subordinados hierárquica não pode exceder 330. 

## <a name="intent-and-entity-naming"></a>Objetivo e atribuição de nomes de entidade
Não utilize os seguintes carateres em nomes de intenção e entidade:

|Caráter|Nome|
|--|--|
|`{`|Chaveta de abertura esquerdo|
|`}`|À direita chaveta de abertura|
|`[`|Reto esquerdo|
|`]`|Parêntesis Reto direito|
|`\`|barra invertida|

## <a name="key-limits"></a>Limites de chaves
A chave de criação tem limites diferentes para a criação e o ponto final. A chave de subscrição de serviço LUIS só é válida para consultas de ponto final.

|Chave|Criação de conteúdos|Ponto Final|Objetivo|
|--|--|--|--|
|Criação/arranque|milhões de 1/mês, 5/segundo|1 thousand/mês, 5/segundo|A aplicação de LUIS de criação|
|[Subscrição] [ pricing] escalão gratuito - F0 - |inválido|10 thousand/mês, 5/segundo|Consultar o ponto final LUIS|
|[Subscrição] [ pricing] - S0 - o escalão básico|inválido|50 por segundo|Consultar o ponto final LUIS|
|[Integração de análise de dados de sentimento](publishapp.md#enable-sentiment-analysis)|inválido|Sem qualquer encargo|Adicionar informações de sentimento, incluindo a extração de dados de expressão de chave |
|Integração de reconhecimento de voz|inválido|Pedidos de ponto final de milhares $5.50 EUR/1|Converter utterance ditas utterance de texto e devolver LUIS resultados|

## <a name="keyboard-controls"></a>Controlos de teclado

|Introdução por teclado | Descrição | 
|--|--|
|Controlo + I|Alterna entre tokens e entidades na lista de utterances|

## <a name="website-sign-in-time-period"></a>Web site, inicie sessão no período de tempo

O acesso de início de sessão destina-se **60 minutos**. Após este período de tempo, irá obter este erro. Terá de iniciar sessão novamente.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app
[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-test#batch-testing
[intents]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-intent
[phrase-list]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-utterance
[luis-how-to-manage-versions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-manage-versions
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/

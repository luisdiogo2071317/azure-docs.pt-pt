---
title: Limites das linguagens de compreensão (LUIS) | Documentos da Microsoft
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
ms.openlocfilehash: c1f49f88cb4772da636bd1396b09e5efd357fb60
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344126"
---
# <a name="luis-boundaries"></a>Limites de LUIS
LUIS tem várias áreas de limites. A primeira é o [limites de modelo](#model-boundaries), que controla o intenções, entidades e recursos do LUIS. É a segunda área [limites de quota](#key-limits) com base no tipo de chave. É uma terceira área dos limites da [combinação de teclado](#keyboard-controls) para controlar o Web site do LUIS. É uma área de quarta a [mapeamento de região do mundo](luis-reference-regions.md) entre o LUIS criação de Web site e o LUIS [endpoint](luis-glossary.md#endpoint) APIs. 


## <a name="model-boundaries"></a>Limites do modelo

|Área|Limite|
|--|:--|--|
| [Nome da aplicação][luis-get-started-create-app] | * Máximo de carateres de predefinido |
| [Teste de batch][batch-testing]| conjuntos de dados de 10, expressões de 1000 por conjunto de dados|
| **[Composição](./luis-concept-entity-types.md)|100 com até 10 crianças |
| Lista explícita | 50 por aplicação|
| **[Hierárquica](./luis-concept-entity-types.md) |100 com até 10 crianças |
| [Objetivos][intents]|500 por aplicação<br>[Com base na expedição](https://github.com/Microsoft/botbuilder-tools/tree/master/Dispatch) aplicativo possui origens de expedição 500 correspondente|
| [Lista de entidades](./luis-concept-entity-types.md) | Principal: Step-by 50, subordinado: 20 000 itens. É o nome canônico * predefinido máx. de caráter. Sinónimos não tem nenhuma restrição de comprimento. |
| [Padrões de](luis-concept-patterns.md)|500 padrões por aplicação.<br>Comprimento máximo do padrão é de 400 caracteres.<br>3 Pattern.any entidades, por padrão<br>Máximo de 2 textos de opcionais aninhados no padrão|
| [Pattern.any](./luis-concept-entity-types.md)|100 por aplicação, 3 pattern.any entidades, por padrão |
| [Lista de frase][phrase-list]|10 de frase de listas, itens de 5000 por lista|
| [Entidades pré-concebidas](./luis-prebuilt-entities.md) | sem limite|
| [Entidades de expressão regular](./luis-concept-entity-types.md)|20 entidades<br>máximo de 500 carateres. Por padrão de entidade de expressão regular|
| [Funções](luis-concept-roles.md)|300 funções por aplicação. 10 funções por entidade|
| **[Simples](./luis-concept-entity-types.md)| 100 entidades|
| [Expressão][utterances] | 500 carateres|
| [Expressões com][utterances] | 15 000 por aplicação|
| [Nome da versão][luis-how-to-manage-versions] | 10 caracteres restringidas de alfanuméricos e período (.) |

* Máximo de carateres de predefinido é 50 carateres. 

* * A contagem total de entidades simples, hierárquicas e compostas não pode exceder os 100. A contagem total de entidades hierárquicas, entidades compostas, entidades simples e entidades de filhos hierárquica não pode exceder 330. 

## <a name="intent-and-entity-naming"></a>Objetivo e a atribuição de nomes de entidade
Não utilize os seguintes carateres em nomes de intenção e entidade:

|Caráter|Nome|
|--|--|
|`{`|Chaveta esquerda|
|`}`|Chaveta direita|
|`[`|Parênteses Reto de abertura|
|`]`|Reto|
|`\`|Barra invertida|

## <a name="key-limits"></a>Limites de chaves
A chave de criação tem limites diferentes para a criação e o ponto final. A chave de ponto final de serviço do LUIS só é válida para consultas de ponto final.

|Chave|Criação de conteúdos|Ponto Final|Objetivo|
|--|--|--|--|
|Criação/Starter|1 milhão/mês, 5/segundo|1 mil/mês, 5/segundo|Criação da sua aplicação LUIS|
|[Subscrição] [ pricing] escalão gratuito de - F0 - |inválido|10 mil/mês, 5/segundo|Consultar o ponto de final do LUIS|
|[Subscrição] [ pricing] - S0 - de escalão básico|inválido|50/segundo|Consultar o ponto de final do LUIS|
|[Integração de análise de sentimentos](luis-how-to-publish-app.md#enable-sentiment-analysis)|inválido|sem custos|Adicionar informações de sentimentos, incluindo a extração de dados de expressões-chave |
|Integração de voz|inválido|Pedidos de ponto final de 5.50 US $/ 1 mil|Converter a expressão falada em expressão de texto e devolvem os resultados de LUIS|

## <a name="keyboard-controls"></a>Controles de teclado

|Entrada de teclado | Descrição | 
|--|--|
|Controle + E|Alterna entre tokens e de entidades na lista de expressões|

## <a name="website-sign-in-time-period"></a>Web site, inicie sessão no período de tempo

O acesso de início de sessão destina-se **60 minutos**. Após este período de tempo, irá obter este erro. Tem de iniciar sessão novamente.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app
[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-test#batch-testing
[intents]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-intent
[phrase-list]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-utterance
[luis-how-to-manage-versions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-manage-versions
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/

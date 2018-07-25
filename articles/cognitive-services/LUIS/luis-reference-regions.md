---
title: Regiões do Language Understanding (LUIS) | Documentos da Microsoft
titleSuffix: Azure
description: Este artigo contém listas de regiões do LUIS, para o Web site LUIS, as subscrições do Azure e regiões do mundo.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/19/2018
ms.author: diberry
ms.openlocfilehash: 1f6090bf1ac588585a16f93d2ac091e8950ca45f
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39238935"
---
# <a name="regions-and-keys"></a>Regiões e as chaves

A região em que publique a sua aplicação LUIS corresponde à região ou localização que especificar no portal do Azure ao criar uma chave de ponto final do Azure LUIS. Quando [publicar uma aplicação](./luis-how-to-publish-app.md), LUIS gera automaticamente um URL de ponto final para a região associada à chave. Para publicar uma aplicação do LUIS mais do que uma região, tem pelo menos uma chave por região. 

## <a name="luis-website"></a>Web site do LUIS
Existem três Web sites do LUIS, com base na região. Tem de criar e publicar na mesma região. 

|LUIS|Região|
|--|--|
|[www.Luis.ai][www.luis.ai]|As<br>não Europa<br>não Austrália|
|[au.Luis.ai][au.luis.ai]|Austrália|
|[eu.Luis.ai][eu.luis.ai]|Europa|


## <a name="publishing-regions"></a>Regiões de publicação

Aplicações de LUIS criadas em https://www.luis.ai pode ser publicado para todos os pontos finais, exceto o [Europeia](#publishing-to-europe) e [australiana](#publishing-to-australia) regiões. 

Só pode ser publicada a aplicação de região de criação para odpovídající publicar região. Se a aplicação está atualmente na região errada de criação, exportar a aplicação e importe-o para a região de criação correta para a sua região de publicação. 

 Região global | Região de criação | Publicar & consulta de região   |   Web site do LUIS | Formato de URL de ponto final   |
|-----|------|------|------|------|
| Ásia | EUA Oeste| Ásia Oriental     | [www.Luis.ai][www.luis.ai] |  https://eastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Ásia | EUA Oeste| Sudeste Asiático     | [www.Luis.ai][www.luis.ai] |   https://southeastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Austrália](#publishing-to-australia) | Leste da Austrália| Leste da Austrália     |   [au.Luis.ai][au.luis.ai] | https://australiaeast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Europa](#publishing-to-europe)| Europa Ocidental| Europa do Norte     | [eu.Luis.ai][eu.luis.ai]|  https://northeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| *[Europa](#publishing-to-europe) | Europa Ocidental| Europa Ocidental     | [eu.Luis.ai][eu.luis.ai]|  https://westeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| América do Norte | EUA Oeste | EUA Leste      |[www.Luis.ai][www.luis.ai] |   https://eastus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| América do Norte | EUA Oeste | EUA Leste 2     | [www.Luis.ai][www.luis.ai] |  https://eastus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| América do Norte | EUA Oeste | EUA Centro-Sul     | [www.Luis.ai][www.luis.ai] |  https://southcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| América do Norte | EUA Oeste | EUA Centro-Oeste     |[www.Luis.ai][www.luis.ai] |  https://westcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| América do Norte | EUA Oeste | EUA Oeste |  [www.Luis.ai][www.luis.ai] | https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| América do Norte | EUA Oeste | EUA Oeste 2    | [www.Luis.ai][www.luis.ai] |  https://westus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| América do Sul | EUA Oeste | Sul do Brasil     | [www.Luis.ai][www.luis.ai] |  https://brazilsouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |

## <a name="publishing-to-europe"></a>Publicação para a Europa

Para publicar as regiões da Europa, crie LUIS aplicações à https://eu.luis.ai apenas. Se tentar publicar em qualquer lugar que outra com uma chave na região da Europa, LUIS exibe uma mensagem de aviso. Em alternativa, utilize https://eu.luis.ai. Aplicações de LUIS criadas às [ https://eu.luis.ai ] [ eu.luis.ai] não migram para outras regiões. Exportar e, em seguida, importe a aplicação do LUIS para migrá-la.

## <a name="publishing-to-australia"></a>Publicação da Austrália

Para publicar as regiões australianas, crie LUIS aplicações em https://au.luis.ai apenas. Se tentar publicar em qualquer lugar que outra com uma chave na região australiano, LUIS exibe uma mensagem de aviso. Em alternativa, utilize https://au.luis.ai. Aplicações de LUIS criadas às [ https://au.luis.ai ] [ au.luis.ai] não migram para outras regiões. Exportar e, em seguida, importe a aplicação do LUIS para migrá-la.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência de entidades pré-concebidas](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai
 [au.luis.ai]: https://au.luis.ai
 [eu.luis.ai]: https://eu.luis.ai
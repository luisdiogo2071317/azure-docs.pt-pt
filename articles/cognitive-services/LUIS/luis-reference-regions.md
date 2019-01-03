---
title: Regiões de publicação e pontos finais
titleSuffix: Azure Cognitive Services
description: A região em que publique a sua aplicação LUIS corresponde à região ou localização que especificar no portal do Azure ao criar uma chave de ponto final do Azure LUIS. Quando publica uma aplicação, o LUIS gera automaticamente um URL de ponto final para a região associada à chave.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 995300fe2a82fb41078d1c66435f0fb006d5f8cf
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53603696"
---
# <a name="authoring-and-publishing-regions-and-the-associated-keys"></a>Criação e publicação de regiões e as chaves associadas

A região em que publique a sua aplicação LUIS corresponde à região ou localização que especificar no portal do Azure ao criar uma chave de ponto final do Azure LUIS. Quando [publicar uma aplicação](./luis-how-to-publish-app.md), LUIS gera automaticamente um URL de ponto final para a região associada à chave. Para publicar uma aplicação do LUIS mais do que uma região, tem pelo menos uma chave por região. 

## <a name="luis-website"></a>Web site do LUIS
Existem três Web sites do LUIS, com base na região. Tem de criar e publicar na mesma região. 

|LUIS|Região|
|--|--|
|[www.Luis.ai][www.luis.ai]|As<br>não Europa<br>não Austrália|
|[au.Luis.ai][au.luis.ai]|Austrália|
|[eu.Luis.ai][eu.luis.ai]|Europa|

## <a name="regions-and-azure-resources"></a>Regiões e recursos do Azure
A aplicação for publicada para todas as regiões associadas com os recursos de LUIS, adicionados no portal do LUIS. Por exemplo, para uma aplicação criada no [www.luis.ai][www.luis.ai], se criar um recurso de LUIS na **westus** e adicioná-lo para a aplicação como um recurso, a aplicação for publicada nessa região. 

## <a name="public-apps"></a>Aplicações públicas
Uma aplicação pública está publicada em todas as regiões, para que um utilizador com uma chave de recurso com base na região do LUIS pode aceder à aplicação em qualquer região é associada à sua chave de recurso.

## <a name="publishing-regions"></a>Regiões de publicação

Aplicações de LUIS criadas em https://www.luis.ai pode ser publicado para todos os pontos finais, exceto o [Europeia](#publishing-to-europe) e [australiana](#publishing-to-australia) regiões. 

Só pode ser publicada a aplicação de região de criação para odpovídající publicar região. Se a aplicação está atualmente na região errada de criação, exportar a aplicação e importe-o para a região de criação correta para a sua região de publicação. 

 Região global | Região de criação<br>`API region name` | Publicar & consulta de região<br>`API region name`   |   Web site do LUIS | Formato de URL de ponto final   |
|-----|------|------|------|------|
| Ásia | EUA Oeste<br>`westus`| Ásia Oriental<br>`eastasia`     | [www.Luis.ai][www.luis.ai] |  https://eastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Ásia | EUA Oeste<br>`westus`| Sudeste Asiático<br>`souteastasia`     | [www.Luis.ai][www.luis.ai] |   https://southeastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Austrália](#publishing-to-australia) | Leste da Austrália<br>`australiaeast`| Leste da Austrália<br>`australiaeast`     |   [au.Luis.ai][au.luis.ai] | https://australiaeast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Europa](#publishing-to-europe)| Europa Ocidental<br>`westeurope`| Europa do Norte<br>`northeurope`     | [eu.Luis.ai][eu.luis.ai]|  https://northeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| *[Europa](#publishing-to-europe) | Europa Ocidental<br>`westeurope`| Europa Ocidental<br>`westeurope`     | [eu.Luis.ai][eu.luis.ai]|  https://westeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| América do Norte | EUA Oeste<br>`westus` | EUA Leste<br>`eastus`      |[www.Luis.ai][www.luis.ai] |   https://eastus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| América do Norte | EUA Oeste<br>`westus` | EUA Leste 2<br>`eastus2`     | [www.Luis.ai][www.luis.ai] |  https://eastus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| América do Norte | EUA Oeste<br>`westus` | EUA Centro-Sul<br>`southcentralus`     | [www.Luis.ai][www.luis.ai] |  https://southcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| América do Norte | EUA Oeste<br>`westus` | EUA Centro-Oeste<br>`westcentralus`     |[www.Luis.ai][www.luis.ai] |  https://westcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| América do Norte | EUA Oeste<br>`westus` | EUA Oeste<br>`westus`  |  [www.Luis.ai][www.luis.ai] | https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| América do Norte | EUA Oeste<br>`westus` | EUA Oeste 2<br>`westus2`    | [www.Luis.ai][www.luis.ai] |  https://westus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| América do Sul | EUA Oeste<br>`westus` | Sul do Brasil<br>`brazilsouth`     | [www.Luis.ai][www.luis.ai] |  https://brazilsouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |

## <a name="publishing-to-europe"></a>Publicação para a Europa

Para publicar as regiões da Europa, crie LUIS aplicações à https://eu.luis.ai apenas. Se tentar publicar em qualquer lugar que outra com uma chave na região da Europa, LUIS exibe uma mensagem de aviso. Em alternativa, utilize https://eu.luis.ai. Aplicações de LUIS criadas às [ https://eu.luis.ai ] [ eu.luis.ai] não migram para outras regiões. Exportar e, em seguida, importe a aplicação do LUIS para migrá-la.

## <a name="publishing-to-australia"></a>Publicação da Austrália

Para publicar as regiões australianas, crie LUIS aplicações em https://au.luis.ai apenas. Se tentar publicar em qualquer lugar que outra com uma chave na região australiano, LUIS exibe uma mensagem de aviso. Em alternativa, utilize https://au.luis.ai. Aplicações de LUIS criadas às [ https://au.luis.ai ] [ au.luis.ai] não migram para outras regiões. Exportar e, em seguida, importe a aplicação do LUIS para migrá-la.

## <a name="endpoints"></a>Pontos Finais

Atualmente, o LUIS tem 2 pontos de extremidade: um para a criação e outro para análise de texto.

|Objetivo|do IdP|
|--|--|
|Criação de conteúdos|`https://{region}.api.cognitive.microsoft.com/luis/api/v2.0/apps/{appID}/`|
|Análise de texto (predição de consulta)|`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q={q}[&timezoneOffset][&verbose][&spellCheck][&staging][&bing-spell-check-subscription-key][&log]`|

A tabela seguinte explica os parâmetros, marcados com chavetas `{}`, na tabela anterior.

|Parâmetro|Objetivo|
|--|--|
|Região|Região do Azure - criação e publicação têm diferentes regiões|
|appID|ID da aplicação LUIS utilizado na rota de URL e encontrado no dashboard de aplicações|
|p|texto de expressão enviado a partir do aplicativo de cliente, como o bot de bate-papo|


## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência de entidades pré-concebidas](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai
 [au.luis.ai]: https://au.luis.ai
 [eu.luis.ai]: https://eu.luis.ai
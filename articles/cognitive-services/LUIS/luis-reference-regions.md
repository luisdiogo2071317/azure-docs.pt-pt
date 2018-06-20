---
title: Regiões do idioma compreender (LUIS) | Microsoft Docs
titleSuffix: Azure
description: Este artigo contém apresenta uma lista das regiões do LUIS para o Web site LUIS, as subscrições do Azure e regiões do mundo.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/19/2018
ms.author: v-geberr
ms.openlocfilehash: 86a20770178707f72cf2991ca08b6b98eaeaf0cf
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36237291"
---
# <a name="regions-and-keys"></a>Regiões e chaves

A região na qual pode publica a aplicação de LUIS corresponde à região ou localização que especificar no portal do Azure quando cria uma chave de ponto final LUIS do Azure. Quando lhe [publicar uma aplicação](./PublishApp.md), LUIS gera automaticamente um URL de ponto final para a região associado à chave. Para publicar uma aplicação LUIS mais de uma região, é necessário, pelo menos, uma chave por região. 

## <a name="luis-website"></a>Web site de LUIS
Existem três LUIS os Web sites, com base na região. Tem de criar e publicar na mesma região. 

|LUIS|Região|
|--|--|
|[www.Luis.ai][www.luis.ai]|As<br>não Europa<br>não Austrália|
|[au.Luis.ai][au.luis.ai]|Austrália|
|[eu.Luis.ai][eu.luis.ai]|Europa|


## <a name="publishing-regions"></a>Publicação de regiões

Aplicações de LUIS criadas no https://www.luis.ai pode ser publicado para todos os pontos finais, exceto o [Europeia](#publishing-to-europe) e [australiana](#publishing-to-australia) regiões. 

A aplicação de região de criação só pode ser publicada para uma correspondente publicar região. Se a aplicação está atualmente a região errada de criação, exportar a aplicação e importe-o para a região de criação correta para a sua região publicação. 

 Região global | Criação de região | Publicar & consultar região   |   Web site de LUIS | Formato de URL de ponto final   |
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

## <a name="publishing-to-europe"></a>Publicação Europa

Para publicar as regiões da Europa, criar LUIS aplicações em https://eu.luis.ai apenas. Se tentar publicar em qualquer local ou utilizar uma chave na região Europa, LUIS apresenta uma mensagem de aviso. Em alternativa, utilize https://eu.luis.ai. Aplicações de LUIS criadas em [ https://eu.luis.ai ] [ eu.luis.ai] não migra automaticamente a outras regiões. Exportar e, em seguida, importar a aplicação de LUIS para a migração.

## <a name="publishing-to-australia"></a>Publicação da Austrália

Para publicar as regiões australiana, criar LUIS aplicações em https://au.luis.ai apenas. Se tentar publicar em qualquer local ou utilizar uma chave na região australiana, LUIS apresenta uma mensagem de aviso. Em alternativa, utilize https://au.luis.ai. Aplicações de LUIS criadas em [ https://au.luis.ai ] [ au.luis.ai] não migra automaticamente a outras regiões. Exportar e, em seguida, importar a aplicação de LUIS para a migração.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência de entidades prebuilt](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai
 [au.luis.ai]: https://au.luis.ai
 [eu.luis.ai]: https://eu.luis.ai
---
title: Região de ponto de extremidade, node. js
titleSuffix: Language Understanding - Azure Cognitive Services
description: Com o node. js, encontre publicar região com a chave de ponto final e o aplicativo ID para LUIS.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 6bcea43fb21cae6f000f3522b498dfc6c0f6d9a7
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/10/2018
ms.locfileid: "53166222"
---
# <a name="find-endpoint-region-with-nodejs"></a>Localizar a região de ponto final com o node. js
Se tiver o LUIS ID da aplicação e o ID de subscrição do LUIS, pode encontrar a qual a região a utilizar para consultas de ponto final.

> [!NOTE] 
> A solução completa do node. js está disponível a partir da [ **LUIS-Samples** repositório do GitHub](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/nodejs/).

## <a name="luis-endpoint-query-strategy"></a>Estratégia de consulta de ponto final do LUIS
Cada consulta de ponto final do LUIS requer:

* Uma chave de ponto final
* Um ID de aplicação
* Uma região

Se a consulta de ponto final do LUIS utiliza o ID de chave e aplicação de ponto final correto, mas a região errada, o código de resposta é 401. O pedido 401 não é contabilizado para a quota de subscrição. Tornar este pedido numa estratégia para consultar todas as regiões para localizar a região correta. A região correta é a solicitação apenas que retorna um código de estado 2xx. 

|Código de resposta|Parâmetros|
|--|--|
|2xx|chave de ponto final correto<br>ID de aplicação correta<br>região de anfitrião correto|
|401|chave de ponto final correto<br>ID de aplicação correta<br>_incorreto_ região de anfitrião|

## <a name="nodejs-code-to-find-region"></a>Código de node. js para localizar a região
O aplicativo de console usa o ID da aplicação LUIS e a chave de ponto final e devolve todas as regiões associadas ele. Atualmente, é criada uma chave de ponto final por região por isso, apenas numa região deve retornar.

Inclua as dependências do NPM:

[!code-javascript[Add the dependencies](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=5-6 "Add the dependencies")]

Adicione constantes. Substitua os valores das variáveis para `subscriptionKey` e `appId` pelos seus próprios valores.  

[!code-javascript[Add constants](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=8-25 "Add constants")]

Adicionar `searchRegions` função para localizar a região. Todas as regiões incorretas devolvem 401, que é detetado e ignorado.

[!code-javascript[Find region](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=27-37 "Find region")]

Chamar o `searchRegions` de função e retornar única região:

[!code-javascript[Call the function](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=39-43 "Call the function")]

Quando o aplicativo é executado, o terminal mostra a região para o ID de aplicação.

![Captura de ecrã da aplicação de consola que mostra a região de LUIS](./media/find-region-nodejs/console.png)


## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o LUIS [regiões](luis-reference-regions.md).

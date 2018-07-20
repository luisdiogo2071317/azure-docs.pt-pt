---
title: Localizar a região de LUIS com node. js em limites de compreensão de idiomas (LUIS) | Documentos da Microsoft
description: Por meio de programação find publicar região com a chave de ponto final e o aplicativo ID para LUIS.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/01/2018
ms.author: v-geberr
ms.openlocfilehash: 6d85e6007b3e85a1b55997541e721ad57c22dddf
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/19/2018
ms.locfileid: "39163085"
---
# <a name="region-can-be-determined-from-api-call"></a>Pode ser determinada a região da chamada de API 
Se tiver o LUIS ID da aplicação e o ID de subscrição do LUIS, pode encontrar a qual a região a utilizar para consultas de ponto final.

> [!NOTE] 
> A solução completa do node. js está disponível a partir da [ **LUIS-Samples** repositório do Github](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/nodejs/).

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

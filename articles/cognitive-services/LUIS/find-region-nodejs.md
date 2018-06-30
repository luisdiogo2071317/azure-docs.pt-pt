---
title: Localizar região LUIS com o Node.js em limites de compreensão de idiomas (LUIS) | Microsoft Docs
description: Através de programação localizar publicar região com a chave de ponto final e aplicação ID para LUIS.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/01/2018
ms.author: v-geberr
ms.openlocfilehash: 6d85e6007b3e85a1b55997541e721ad57c22dddf
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37111750"
---
# <a name="region-can-be-determined-from-api-call"></a>É possível determinar a região da chamada de API 
Se tiver LUIS ID da aplicação e o ID de subscrição LUIS, pode encontrar a que região a utilizar para consultas de ponto final.

> [!NOTE] 
> A solução completa do Node.js está disponível a partir de [ **LUIS-Samples** repositório do Github](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/nodejs/).

## <a name="luis-endpoint-query-strategy"></a>Estratégia de consulta de ponto final de LUIS
Cada consulta de ponto final LUIS requer:

* Uma chave de ponto final
* Um ID de aplicação
* Uma região

Se a consulta de ponto final de LUIS utiliza o ID de chave e aplicação de ponto final correto, mas a região errada, o código de resposta é 401. O pedido 401 não é contabilizadas para a quota de subscrição. Ative este pedido para uma estratégia para consultar todas as regiões para localizar a região correta. A região correta é o pedido apenas que devolve um código de estado 2xx. 

|Código de resposta|Parâmetros|
|--|--|
|2xx|chave de ponto final correto<br>Corrija o ID da aplicação<br>região do anfitrião correto|
|401|chave de ponto final correto<br>Corrija o ID da aplicação<br>_incorreto_ região de anfitrião|

## <a name="nodejs-code-to-find-region"></a>Código node.js para localizar região
A aplicação de consola utiliza o ID da aplicação LUIS e a chave de ponto final e devolve todas as regiões associadas. Atualmente, é criada uma chave de ponto final por região, por isso, apenas uma região deve ser devolvido.

Inclua as dependências NPM:

[!code-javascript[Add the dependencies](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=5-6 "Add the dependencies")]

Adicione constantes. Substitua os valores das variáveis para `subscriptionKey` e `appId` com os seus próprios valores.  

[!code-javascript[Add constants](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=8-25 "Add constants")]

Adicionar `searchRegions` função para localizar a região. Todas as regiões incorretas devolvem 401, que é detetada e ignorado.

[!code-javascript[Find region](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=27-37 "Find region")]

Chamar o `searchRegions` funcionar e regresse única região:

[!code-javascript[Call the function](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=39-43 "Call the function")]

Quando a aplicação é executada, o terminal mostra a região para o ID de aplicação.

![Captura de ecrã da aplicação de consola que mostra a região LUIS](./media/find-region-nodejs/console.png)


## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre LUIS [regiões](luis-reference-regions.md).

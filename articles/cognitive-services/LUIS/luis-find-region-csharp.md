---
title: Localizar a região de LUIS com c# nos limites de compreensão de idiomas (LUIS) | Documentos da Microsoft
description: Por meio de programação find publicar região com a chave de ponto final e o aplicativo ID para LUIS.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/31/2018
ms.author: diberry
ms.openlocfilehash: 6f2688e492dbba718dc55825de164c189a9b46c9
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225113"
---
# <a name="region-can-be-determined-from-api-call"></a>Pode ser determinada a região da chamada de API 
Se tiver o LUIS ID da aplicação e o ID de subscrição do LUIS, pode encontrar a qual a região a utilizar para consultas de ponto final.

> [!NOTE] 
> A solução completa do c# está disponível a partir da [ **LUIS-Samples** repositório do Github](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/csharp/).

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

## <a name="c-class-code-to-find-region"></a>Código da classe c# para localizar a região
O aplicativo de console usa o ID da aplicação LUIS e a chave de ponto final e devolve todas as regiões associadas ele. Atualmente, é criada uma chave de ponto final por região por isso, apenas numa região deve retornar.

Inclua as dependências de biblioteca do .net:

[!code-csharp[Add the dependencies](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=1-6 "Add the dependencies")]

Adicione esta classe de LUIS personalizada criada para localizar a região. Substitua os valores das variáveis para `luisAppId` e `luisSubscriptionKey` pelos seus próprios valores. Todas as regiões que retornam 401 serão escritas para a consola de depuração. 

[!code-csharp[Add the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=10-83 "Add the LUIS class")]

Este é um exemplo de chamar a classe de LUIS personalizada no método Main do aplicativo de console:

[!code-csharp[Call the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=85-101 "Call the LUIS class")]

Quando o aplicativo é executado, a consola mostra a região para o ID de aplicação.

![Captura de ecrã da aplicação de consola que mostra a região de LUIS](./media/find-region-csharp/console.png)

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o LUIS [regiões](luis-reference-regions.md).

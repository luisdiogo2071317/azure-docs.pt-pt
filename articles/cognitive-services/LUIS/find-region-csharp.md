---
title: Localizar região LUIS com c# nos limites de compreensão de idiomas (LUIS) | Microsoft Docs
description: Através de programação localizar publicar região com a chave de ponto final e aplicação ID para LUIS.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/31/2018
ms.author: v-geberr
ms.openlocfilehash: f0df14736e0ed47957999e3aa7c6a22b0b0c0a35
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110416"
---
# <a name="region-can-be-determined-from-api-call"></a>É possível determinar a região da chamada de API 
Se tiver LUIS ID da aplicação e o ID de subscrição LUIS, pode encontrar a que região a utilizar para consultas de ponto final.

> [!NOTE] 
> A solução completa do c# está disponível a partir de [ **LUIS-Samples** repositório do Github](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/csharp/).

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

## <a name="c-class-code-to-find-region"></a>Código à classe c# para localizar região
A aplicação de consola utiliza o ID da aplicação LUIS e a chave de ponto final e devolve todas as regiões associadas. Atualmente, é criada uma chave de ponto final por região, por isso, apenas uma região deve ser devolvido.

Inclua as dependências da biblioteca de .net:

[!code-csharp[Add the dependencies](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=1-6 "Add the dependencies")]

Adicione esta classe LUIS personalizada criada de modo a encontrar a região. Substitua os valores das variáveis para `luisAppId` e `luisSubscriptionKey` com os seus próprios valores. Todas as regiões que devolvem 401 serão escritas para a consola de depuração. 

[!code-csharp[Add the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=10-83 "Add the LUIS class")]

Este é um exemplo da chamada de classe de LUIS personalizada no método principal da aplicação de consola:

[!code-csharp[Call the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=85-101 "Call the LUIS class")]

Quando a aplicação é executada, a consola mostra a região para o ID de aplicação.

![Captura de ecrã da aplicação de consola que mostra a região LUIS](./media/find-region-csharp/console.png)

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre LUIS [regiões](luis-reference-regions.md).

---
title: Códigos de resposta de HTTP da API de compreensão (LUIS) do idioma - Azure | Documentos da Microsoft
titleSuffix: Azure
description: Compreender quais códigos de resposta HTTP devolvidos pelas APIs de ponto final e de criação de LUIS
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/16/2018
ms.author: diberry
ms.openlocfilehash: f005c7f13cd05ce3ca6ce494c4a2670106cb3637
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900986"
---
# <a name="luis-api-http-response-codes"></a>Códigos de resposta HTTP da API de LUIS
O [criação](https://aka.ms/luis-authoring-apis) e [endpoint](https://aka.ms/luis-endpoint-apis) APIs devolvem os códigos de resposta HTTP. Mensagens de resposta inclui informações específicas a um pedido, o código de estado de resposta HTTP é geral. 

## <a name="common-status-codes"></a>Códigos de status comuns
A tabela seguinte lista algumas dos códigos de estado de resposta HTTP mais comuns para o [criação](https://aka.ms/luis-authoring-apis) e [endpoint](https://aka.ms/luis-endpoint-apis) APIs:

|Código|API|Explicação|
|:--|--|--|
|400|Criação de ponto final|parâmetros do pedido estão incorretos, que significa que os parâmetros necessários estão em falta, com formato incorreto ou é demasiado grande|
|400|Criação de ponto final|corpo do pedido está incorreto, que significa que o JSON é demasiado grande, com formato incorreto ou em falta|
|401|Criação de conteúdos|Utilizar chave de subscrição de ponto final, em vez de chave de criação|
|401|Criação de ponto final|chave inválido, um formato incorreto ou está vazio|
|401|Criação de ponto final| a chave não corresponde à região|
|401|Criação de conteúdos|não for o proprietário ou funcionário|
|401|Criação de conteúdos|ordem inválido de chamadas de API|
|403|Criação de ponto final|limite de quota de chave mensal total excedido|
|409|Ponto Final|aplicação ainda está a carregar|
|410|Ponto Final|aplicação tem de ser reestruturar e replicado|
|414|Ponto Final|consulta excede o limite máximo de carateres|
|429|Criação de ponto final|Limite de taxa foi excedido (pedidos/segundo)|
---
title: Códigos de resposta de HTTP de API de conhecimento (LUIS) do idioma - Azure | Microsoft Docs
titleSuffix: Azure
description: Compreender os códigos de resposta HTTP são devolvidos da criação de LUIS e APIs de ponto final
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 04/16/2018
ms.author: v-geberr
ms.openlocfilehash: 9c7381d9dc2ecf302c85c6b4f1f24b24a28d9ebe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352705"
---
# <a name="luis-api-http-response-codes"></a>Códigos de resposta de HTTP de API LUIS
O [criação](https://aka.ms/luis-authoring-apis) e [endpoint](https://aka.ms/luis-endpoint-apis) APIs devolvem códigos de resposta HTTP. Apesar de mensagens de resposta incluem as informações específicas a um pedido, o código de estado de resposta HTTP é geral. 

## <a name="common-status-codes"></a>Códigos de estado comuns
A tabela seguinte lista algumas dos códigos de estado de resposta HTTP mais comuns para o [criação](https://aka.ms/luis-authoring-apis) e [endpoint](https://aka.ms/luis-endpoint-apis) APIs:

|Código|API|Explicação|
|:--|--|--|
|400|Criação de ponto final|parâmetros do pedido estão incorretos, o que significa que os parâmetros necessários estão em falta, um formato incorreto ou é demasiado grande|
|400|Criação de ponto final|corpo do pedido está incorreto, o que significa que JSON está em falta, incorreto ou é demasiado grande|
|401|Criação de conteúdos|utilizada a chave de subscrição de ponto final, em vez da chave de criação|
|401|Criação de ponto final|chave inválido, com formato incorreto ou está vazio|
|401|Criação de ponto final| chave não corresponde à região|
|401|Criação de conteúdos|Não é o proprietário ou colaborador|
|401|Criação de conteúdos|ordem inválido de chamadas à API|
|403|Criação de ponto final|limite de quota chave mensal total excedido|
|409|Ponto Final|aplicação ainda está a carregar|
|410|Ponto Final|aplicação tem de ser retrained e republicadas|
|414|Ponto Final|consulta excede o limite máximo de carateres|
|429|Criação de ponto final|Excedido o limite de velocidade (pedidos por segundo)|
---
title: Códigos de resposta de HTTP da API - QnA Maker
titleSuffix: Azure Cognitive Services
description: Compreenda quais códigos de resposta HTTP são devolvidos das APIs do QnA Maker. Isto ajudará a resolver quaisquer erros.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 10/09/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: d41f434b4e90c3f3850b8cb2e2d5c09fd23fc2cf
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53085439"
---
# <a name="qna-maker-api-http-response-codes"></a>Códigos de resposta de HTTP de API do QnA Maker
O [gestão](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff) e predição APIs retornam códigos de resposta HTTP. Mensagens de resposta inclui informações específicas a um pedido, o código de estado de resposta HTTP é geral. 

### <a name="management"></a>Gestão

|Código|Explicação|
|:--|--|
|2xx|Êxito|
|400|parâmetros do pedido estão incorretos, que significa que os parâmetros necessários estão em falta, com formato incorreto ou é demasiado grande|
|400|corpo do pedido está incorreto, que significa que o JSON é demasiado grande, com formato incorreto ou em falta|
|401|Chave inválida|
|403|Proibido - não tem permissões corretas|
|404|Não existe KB|
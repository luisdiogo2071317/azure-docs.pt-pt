---
title: Códigos de resposta de HTTP da API - QnA Maker
titleSuffix: Azure Cognitive Services
description: Compreender quais códigos de resposta HTTP são devolvidos das APIs do QnA Maker
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 10/09/2018
ms.author: tulasim
ms.openlocfilehash: 96782d32817e4989c02e0ed098d7772c80aa26c8
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079684"
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
---
title: Aceda ao vivo | Documentos da Microsoft
description: A API do Live ir inicia a oferta em direto de listagem do processo.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: reference
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: c7d643f0c7885e64636a107d22ce332b1ba9371c
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810275"
---
<a name="go-live"></a>Go Live
=======

Esta API é iniciado o processo do envio de uma aplicação para produção. Esta operação é normalmente de execução demorada. Esta chamada usa a lista de correio eletrónico de notificação do [publicar](./cloud-partner-portal-api-publish-offer.md) operação de API.

 `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/golive?api-version=2017-10-31` 

<a name="uri-parameters"></a>Parâmetros do URI
--------------

|  **Nome**      |   **Descrição**                                                           | **Tipo de dados** |
|  --------      |   ---------------                                                           | ------------- |
| publisherId    | Identificador de publicador para a oferta recuperar, por exemplo `contoso`       |  Cadeia       |
| offerId        | Identificador da oferta para recuperar da oferta                                   |  Cadeia       |
| versão de API    | Versão mais recente da API                                                   |  Date         |
|  |  |  |


<a name="header"></a>Cabeçalho
------

|  **Nome**       |     **Valor**       |
|  ---------      |     ----------      |
| Content-Type    | `application/json`  |
| Autorização   | `Bearer YOUR_TOKEN` |
|  |  |


<a name="body-example"></a>Exemplo de corpo
------------

### <a name="response"></a>Resposta

`Operation-Location: https://cloudpartner.azure.com/api/publishers/contoso/offers/contoso-virtualmachineoffer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8`


### <a name="response-header"></a>Cabeçalho de Resposta

|  **Nome**             |      **Valor**                                                            |
|  --------             |      ----------                                                           |
| Operação de localização    |  URL para a consulta para determinar o estado atual da operação            |
|  |  |


### <a name="response-status-codes"></a>Códigos de estado de resposta

| **Código** |  * * Descrição * *                                                                        |
| -------- |  ----------------                                                                        |
|  202     | `Accepted` -O pedido foi aceite a com êxito. A resposta contém uma localização para controlar o estado da operação. |
|  400     | `Bad/Malformed request` -Informações de erro adicionais são encontradas no corpo da resposta. |
|  404     |  `Not found` -A entidade especificada não existe.                                       |
|  |  |

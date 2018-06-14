---
title: Erro de funções do Azure processamento orientações | Microsoft Docs
description: Fornece orientações gerais para processamento de erros que ocorrem no quando executar as suas funções e ligações para tópicos de erros específicos de enlace.
services: functions
cloud: ''
documentationcenter: ''
author: ggailey777
manager: cfowler
ms.assetid: ''
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 02/01/2018
ms.author: glenga; cfowler
ms.openlocfilehash: 82cdc62b3070811186583fdf1ce5e6ce421ebc34
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
ms.locfileid: "29118490"
---
# <a name="azure-functions-error-handling"></a>Processamento de erros de funções do Azure

Este tópico fornece orientações gerais para processamento de erros que ocorrem quando executar as suas funções. Também fornece ligações para tópicos que descrevem os erros de enlace específicas que podem ocorrer. 

## <a name="handing-errors-in-functions"></a>Erros nas funções de manipulação
[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

 
## <a name="binding-error-codes"></a>Códigos de erro de enlace

Quando a integração com serviços do Azure, pode ter erros gerados provenientes das APIs dos serviços subjacentes. Ligações para o erro código documentação para estes serviços podem ser encontrados no **exceções e códigos de retorno** secção das seguinte acionador e tópicos de referência de enlace:

+ [BD do Cosmos para o Azure](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [Armazenamento de blobs](functions-bindings-storage-blob.md#exceptions-and-return-codes)

+ [Hubs de Eventos](functions-bindings-event-hubs.md#exceptions-and-return-codes)

+ [Hubs de Notificação](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Armazenamento de filas](functions-bindings-storage-queue.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus.md#exceptions-and-return-codes)

+ [Armazenamento de tabelas](functions-bindings-storage-table.md#exceptions-and-return-codes)

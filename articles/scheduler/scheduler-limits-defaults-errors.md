---
title: Limites do Scheduler e padrões
description: Limites do Scheduler e padrões
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: ''
ms.assetid: 88f4a3e9-6dbd-4943-8543-f0649d423061
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 4be0695402b66fdb2a027bfbada0e0b26e02d36f
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44378725"
---
# <a name="scheduler-limits-and-defaults"></a>Limites do Scheduler e padrões
## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>As Quotas do Scheduler, limites, predefinições e limitações
[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a>O cabeçalho x-ms-request-id
Cada pedido efetuado para o serviço de agendador retorna um cabeçalho de resposta denominado **x-ms-request-id**. Este cabeçalho contém um valor indefinido, que identifica exclusivamente o pedido.

Se um pedido está a falhar consistentemente e tiver verificado que o pedido é corretamente formulado, pode usar este valor para reportar o erro para a Microsoft. No relatório incluem o valor de x-ms-request-id, o tempo aproximado que o pedido foi efectuado, o identificador de subscrição, a coleção de tarefas, e/ou tarefa e o tipo de operação que tentou o pedido.

## <a name="see-also"></a>Consultar Também
 [O que é o Scheduler?](scheduler-intro.md)

 [Conceitos, terminologia e hierarquia de entidades do Azure Scheduler](scheduler-concepts-terms.md)

 [Começar a utilizar o Scheduler no portal do Azure](scheduler-get-started-portal.md)

 [Planos e faturação no Azure Scheduler](scheduler-plans-billing.md)

 [Referência da API REST do Azure Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Referência de cmdlets do PowerShell do Azure Scheduler](scheduler-powershell-reference.md)

 [Elevada disponibilidade e fiabilidade do Azure Scheduler](scheduler-high-availability-reliability.md)

 [Autenticação de saída do Azure Scheduler](scheduler-outbound-authentication.md)


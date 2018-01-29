---
title: Entidades de mensagens de suspender do Service Bus do Azure | Microsoft Docs
description: Suspender e reativar as entidades de mensagens do Service Bus do Azure.
services: service-bus-messaging
documentationcenter: 
author: clemensv
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: sethm
ms.openlocfilehash: 1984b113f695107f8d4d80e5bbf25c7dc39d13f6
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="suspend-and-reactivate-messaging-entities-disable"></a>Suspender e reativar as entidades de mensagens (desativar)

Filas, tópicos e subscrições podem temporariamente suspensa. Suspensão coloca a entidade no estado desativado em que todas as mensagens são mantidas no armazenamento. No entanto, não podem ser removidas ou adicionadas mensagens e as operações de protocolo respetivos produzem erros.

Suspender uma entidade é geralmente feito para urgentes motivos administrativos. Um cenário é ter implementado um recetor defeituoso que recebe mensagens de fila, ocorre uma falha de processamento e ainda incorretamente conclui as mensagens e remove-os. Se esse comportamento é diagnosticado, a fila pode ser desativada para recebe até código corrigido é implementado e ainda pode ser impedida perda de dados causada pelo código da defeituoso.

Um suspensão ou reativação pode ser efetuada pelo utilizador ou pelo sistema. O sistema suspende apenas entidades devido a razões administrativos grave como atingir a limite de gastos de subscrição. Entidades de sistema-desativado não podem ser reativadas pelo utilizador, mas são restauradas quando a causa da suspensão foi resolvida.

No portal, o **propriedades** secção para a respetiva entidade permite alterar o estado; a seguinte captura de ecrã mostra o botão de alternar para uma fila:

![][1]

O portal apenas permite desativar completamente filas. Também pode desativar o envio e receção operações separadamente utilizando o Service Bus [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) APIs no .NET Framework SDK ou com um modelo Azure Resource Manager através da CLI do Azure ou do Azure PowerShell.

## <a name="suspension-states"></a>Estados de suspensão

Os Estados que podem ser definidos para uma fila são:

-   **Active Directory**: A fila está ativa.
-   **Desativado**: A fila é suspenso.
-   **SendDisabled**: fila parcialmente estiver suspenso, com receive que está a ser permitida.
-   **ReceiveDisabled**: fila parcialmente estiver suspenso, com o envio a ser permitido.

Para as subscrições e tópicos, apenas **Active Directory** e **desativado** pode ser definido.

O [EntityStatus](/dotnet/api/microsoft.servicebus.messaging.entitystatus) enumeração também define um conjunto de Estados de transição que só pode ser definido pelo sistema. O comando do PowerShell para desativar uma fila é mostrado no exemplo seguinte. O comando de reativação é equivalente, definição `Status` para **Active Directory**.

```powershell
$q = Get-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue

$q.Status = "Disabled"

Set-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue -QueueObj $q
```

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre mensagens do Service Bus, consulte os tópicos seguintes:

* [Noções básicas sobre o Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Filas, tópicos e subscrições do Service Bus](service-bus-queues-topics-subscriptions.md)
* [Introdução às filas do Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Como utilizar os tópicos e as subscrições do Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/entity-suspend/queue-disable.png


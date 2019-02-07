---
author: mattbriggs
ms.service: azure-stack
ms.topic: include
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/26/2018
ms.openlocfilehash: 5cd64b806392162fd3bee14ddaf607385ac05264
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/06/2019
ms.locfileid: "55807202"
---
Em fluxos de trabalho de validação, **agendamento** um teste utiliza os parâmetros comuns de nível de fluxo de trabalho que especificou durante a criação de fluxo de trabalho (consulte [parâmetros comuns do fluxo de trabalho para a validação de pilha do Azure como um serviço](../azure-stack-vaas-parameters.md)). Se qualquer um dos valores de parâmetro de teste se torne inválido, deve resupply-los com as instruções na [modifique os parâmetros de fluxo de trabalho](../azure-stack-vaas-monitor-test.md#change-workflow-parameters).

> [!NOTE]
> Um teste de validação de agendamento sobre uma instância existente, irá criar uma nova instância em vez da instância antiga no portal. Registos para a instância antiga serão mantidos, mas não estão acessíveis a partir do portal.  
Quando um teste tiver sido concluída com êxito, o **agenda** ação torna-se desativada.

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-agent](azure-stack-vaas-workflow-step_select-agent.md)]

1. Selecione **agenda** no menu de contexto para abrir um prompt para agendar a instância de teste.

1. Reveja os parâmetros de teste e, em seguida, selecione **submeter** para agendar o teste para execução.
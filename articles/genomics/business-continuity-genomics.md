---
title: Continuidade do negócio - Microsoft Genomics | Microsoft Docs
titleSuffix: Azure
description: Saiba como o Microsoft Genomics suporta continuidade do negócio
keywords: continuidade do negócio, recuperação após desastre
services: microsoft-genomics
author: grhuynh
manager: jhubbard
editor: jasonwhowell
ms.author: grhuynh
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: article
ms.date: 04/06/2018
ms.openlocfilehash: cb3825cb89aff386c4f7c3f3b0d771d73fe637b1
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
ms.locfileid: "31427167"
---
# <a name="overview-of-business-continuity-with-microsoft-genomics"></a>Descrição geral da continuidade do negócio com o Microsoft Genomics
Esta descrição geral descreve as funcionalidades que Microsoft Genomics fornece continuidade do negócio e recuperação de desastre. Saiba mais sobre as opções para recuperar a partir de eventos acontece, por exemplo, uma falha de região do Azure, que podem provocar a perda de dados. 


## <a name="microsoft-genomics-features-that-support-business-continuity"></a>Microsoft Genomics funcionalidades de continuidade do negócio que suporte 
Apesar de raros, um centro de dados do Azure pode ter uma falha, o que pode causar uma interrupção de negócios poderá último alguns minutos a algumas horas. Quando ocorre uma falha, todas as tarefas atualmente em execução no Centro de dados falharão e o serviço Microsoft Genomics será automaticamente, volte a submeter as tarefas para uma região secundária. 

* Uma das alternativas consiste em aguardar que o Centro de dados fique novamente online quando a interrupção do Centro de dados através de. Se a interrupção estiver curta, o serviço Microsoft Genomics irá detetar automaticamente as tarefas falhadas e o fluxo de trabalho será reiniciado automaticamente.

* Outra opção consiste em proativamente submeter o fluxo de trabalho noutra região de dados. Microsoft Genomics implementa várias instâncias [regiões do Azure](https://azure.microsoft.com/regions/services/), e cada instância de específico da região é independente. Se uma das instâncias Microsoft Genomics ocorrer uma falha do local de região, noutras regiões executar instâncias Microsoft Genomics irão continuar a processar as tarefas. Esta transferência para uma região alternativa está sob o controlo do utilizador e está disponível em qualquer altura.


### <a name="manually-failover-microsoft-genomics-workflows-to-another-region"></a>Manualmente ativação pós-falha Microsoft Genomics fluxos de trabalho noutra região
No caso de uma falha do Centro de dados regionais, pode optar por submeter Microsoft Genomics fluxos de trabalho numa região secundária, com base nos seus requisitos de continuidade de comerciais e soberania dos dados individuais. Para manualmente ativação pós-falha Microsoft Genomics fluxos de trabalho, teria de utilizar um específico da região diferente. Genomics conta e submeter a tarefa com adequado específico da região Genomics e as credenciais da conta de armazenamento.

Especificamente, é necessário:
* Crie uma conta de Genomics na região secundária, no portal do Azure. 
* Migrar os dados de entrada para uma conta do storage na região secundária e configurar uma pasta de saída na região secundária.
* O fluxo de trabalho na região secundária de envio.

Quando a região original é restaurada, o serviço Microsoft Genomics não migra os dados da região secundária para a região original. Pode optar por mover a entrada e fazer uma cópia de ficheiros de saída a partir da região secundária a região original.  Se optar por mover os seus dados, trata-se fora do serviço de Genomics e todos os custos relacionados com o movimento de dados será da responsabilidade do cliente. 

### <a name="preparing-for-a-possible-region-specific-outage"></a>A preparar a uma falha de específico da região possíveis
Se estiver preocupados com a recuperação mais rápida no caso de uma falha do Centro de dados, existem alguns passos que pode tomar para reduzir o tempo que demora para que possa reenviar manualmente os seus fluxos de trabalho do Microsoft Genomics para uma região secundária:

* Identificar uma região secundária adequada e ativamente profissional para criar uma conta de Genomics nessa região
* Duplicar os dados na região primária e secundária, para que os seus dados são imediatamente disponíveis na região secundária. Isto pode ser efectuada manualmente ou utilizando o [armazenamento georredundante](https://docs.microsoft.com/azure/storage/common/storage-redundancy) funcionalidade disponível no armazenamento do Azure. 

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu sobre as opções de continuidade do negócio e recuperação após desastre ao utilizar o serviço Microsoft Genomics. Para obter mais informações sobre a continuidade do negócio e recuperação de desastres no Azure no geral, consulte [orientações técnica de resiliência do Azure.](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region) 
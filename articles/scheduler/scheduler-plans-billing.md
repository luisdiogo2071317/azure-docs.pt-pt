---
title: Planos e faturação no agendador do Azure
description: Planos e faturação no agendador do Azure
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: ''
ms.assetid: 13a2be8c-dc14-46cc-ab7d-5075bfd4d724
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 03f335634b7ce1fe4aa6251d6ec21922ed9b84c8
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37887492"
---
# <a name="plans-and-billing-in-azure-scheduler"></a>Planos e faturação no agendador do Azure
## <a name="job-collection-plans"></a>Planos de recolha de tarefa
Coleções de tarefas são a entidade faturável no agendador do Azure. Coleções de tarefas contém um número de tarefas e são fornecidos em três planos – Standard, P10 Premium e P20 Premium – que são descritos abaixo.

| **Plano de recolha de tarefa** | **N. º máximo de tarefas por coleção de tarefas** | **Periodicidade máxima** | **Coleções de tarefas de máx. por subscrição** | **Limites** |
|:--- |:--- |:--- |:--- |:--- |
| **Standard** |50 tarefas por coleção de tarefas |Uma vez por minuto. Não é possível executar tarefas com mais frequência do que uma vez um minuto |Uma subscrição é permitida até 100 coleções de tarefas padrão |Acesso ao conjunto completo de recursos do agendador |
| **P10 Premium** |50 tarefas por coleção de tarefas |Uma vez por minuto. Não é possível executar tarefas com mais frequência do que uma vez um minuto |Uma subscrição é permitida até 10 000 coleções de tarefas P10 Premium. <a href="mailto:wapteams@microsoft.com">Contacte-nos</a> para obter mais informações. |Acesso ao conjunto completo de recursos do agendador |
| **P20 Premium** |tarefas de 1000 por coleção de tarefas |Uma vez por minuto. Não é possível executar tarefas com mais frequência do que uma vez um minuto |Uma subscrição é permitida até 10 000 coleções de tarefas P20 Premium. <a href="mailto:wapteams@microsoft.com">Contacte-nos</a> para obter mais informações. |Acesso ao conjunto completo de recursos do agendador |

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>As atualizações e mudanças para versões anteriores dos planos de coleção de tarefa
Pode atualizar ou mudar a versão de um plano de recolha de tarefa em qualquer altura entre os planos Standard, P10 Premium e P20 Premium.

## <a name="billing-and-azure-plans"></a>Planos de faturação e o Azure
Se tiver mais de 100 coleções de tarefas padrão (10 unidades de faturas padrão), em seguida, é um resultado melhor ter todas as coleções de tarefas no plano premium.

Se tiver uma coleção de tarefas padrão e a coleção de tarefas um premium, é faturada uma cobrança unidade standard *e* uma unidade de faturação premium. As contas de serviço de Scheduler com base no número de coleções de tarefas ativas que estão configuradas para standard ou premium; Isso é explicado mais nas próximas duas secções.

## <a name="standard-billable-units"></a>Unidades Faturáveis padrão
Uma unidade a cobrar padrão pode incluir até 10 coleções de tarefas padrão. Como uma coleção de tarefas padrão pode ter até 50 tarefas por coleção de tarefas, uma unidade de faturação padrão permite que uma subscrição para ter até 500 tarefas – até quase 22 milhões de execuções de tarefas por mês.

Se tiver entre coleções de tarefas padrão de 1 e 10, será cobrado para uma unidade de faturação padrão. Se tiver entre 11 e coleções de tarefas padrão 20, será cobrado para duas unidades de faturas padrão. Se tiver entre 21 e coleções de tarefas padrão 30, é cobrado por três unidades de faturas padrão e assim por diante.

## <a name="p10-premium-billable-units"></a>P10 Unidades Faturáveis Premium
Uma unidade a cobrar premium P10 pode incluir até 10 000 coleções de tarefas de premium P10. Como uma coleção de tarefas P10 premium pode ter até 50 tarefas por coleção de tarefas, uma unidade de faturação premium permite que uma subscrição para ter até 500 000 tarefas – até quase de 22 mil milhões de execuções de tarefas por mês.

Se tiver entre 1 e 10 000 coleções de tarefas premium, pagará por unidade de faturação um P10 premium. Se tiver entre 10,001 e coleções de tarefas premium 20.000, pagará por 2 unidades faturas de premium P10 e assim por diante.

Portanto, coleções de tarefas P10 premium têm a mesma funcionalidade que as coleções de tarefas padrão, mas fornecem uma quebra de preço no caso de seu aplicativo requer muitas coleções de tarefas.

## <a name="p20-premium-billable-units"></a>P20 Unidades Faturáveis Premium
Uma unidade a cobrar P20 premium pode incluir até 5 000 coleções de tarefas de premium P20. Como uma coleção de tarefas do P20 premium pode ter até 1.000 tarefas por coleção de tarefas, uma unidade de faturação premium permite que uma subscrição para ter até 5,000,000 tarefas – até quase 220 bilhões de execuções de tarefas por mês.

Coleções de tarefas do P20 premium fornecem as mesmas capacidades do coleções de tarefas P10 premium, mas também suporta um maior tarefas de número por coleção de tarefas e um maior número total de tarefas geral que premium P10, permitindo que tem mais escalabilidade.

## <a name="billing-and-active-status"></a>Estado de faturação e Active Directory
Coleções de tarefas são sempre ativas, a menos que a sua subscrição completa tornou-se em algum Estado temporário desativado devido a problemas de faturação. É a única forma de garantir que uma coleção de tarefas não é faturada ao eliminar a coleção de tarefas.

Embora pode desativar todas as tarefas dentro de uma coleção de tarefas numa única operação, ele não altera o estado de faturação da coleção de tarefas – a coleção de tarefas irá *ainda* ser cobrado. Da mesma forma, as coleções de tarefas vazio são consideradas Active Directory e serão cobradas.

## <a name="pricing"></a>Preços
Para detalhes de preços, consulte [preços do Scheduler](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="see-also"></a>Consultar Também
 [O que é o Scheduler?](scheduler-intro.md)

 [Conceitos, terminologia e hierarquia de entidades do Azure Scheduler](scheduler-concepts-terms.md)

 [Começar a utilizar o Scheduler no portal do Azure](scheduler-get-started-portal.md)

 [Referência da API REST do Azure Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Referência de cmdlets do PowerShell do Azure Scheduler](scheduler-powershell-reference.md)

 [Elevada disponibilidade e fiabilidade do Azure Scheduler](scheduler-high-availability-reliability.md)

 [Limites, predefinições e códigos de erro do Azure Scheduler](scheduler-limits-defaults-errors.md)

 [Autenticação de saída do Azure Scheduler](scheduler-outbound-authentication.md)


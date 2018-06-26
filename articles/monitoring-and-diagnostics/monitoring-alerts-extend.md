---
title: Expandir alertas (cópia) análise de registos para alertas do Azure - descrição geral
description: Descrição geral do processo para copiar alertas de análise de registos no portal do OMS para alertas do Azure, com detalhes endereçamento preocupações de cliente comuns.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 6484142eafa8388117c1e96ab31eefeab188e488
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750277"
---
# <a name="extend-log-analytics-alerts-to-azure-alerts"></a>Expandir a alertas de análise de registos para alertas do Azure
Até recentemente, Log Analytics do Azure incluída as suas próprias funcionalidades de alerta, o que foi proativamente notificá-lo de condições com base nos dados de análise de registos. Gerido regras de alertas no portal do Microsoft Operations Management Suite. A nova experiência de alertas tem agora integrado alertas em vários serviços no Microsoft Azure. Isto está disponível como **alertas** no Monitor do Azure no portal do Azure e suporta a alertas de atividade registos, métricas e registos de análise de registos e Azure Application Insights. 

## <a name="benefits-of-extending-your-alerts"></a>Vantagens de expandir os alertas
Existem várias vantagens de criar e gerir alertas no portal do Azure, tais como:

- Ao contrário no portal do Operations Management Suite, onde apenas 250 alertas foi criadas e visualizadas, alertas do Azure não tem nenhum desse limitação.
- Alertas do Azure, pode gerir, enumerar e ver todos os tipos de alerta. Anteriormente, fazê-lo foi apenas para os alertas de análise de registos.
- Pode limitar o acesso aos utilizadores apenas a monitorização e alertas, utilizando o [função Monitor de Azure](monitoring-roles-permissions-security.md).
- Nos alertas do Azure, pode utilizar [grupos ação](monitoring-action-groups.md). Isto permite-lhe ter mais de uma ação para cada alerta. Pode SMS, enviar uma chamada de voz, invocar um runbook de automatização do Azure, invocar um webhook e configurar um conector de gestão de serviço de TI (ITSM). 

## <a name="process-of-extending-your-alerts"></a>Processo de alargar os alertas
O processo de mover os alertas de análise de registos para alertas do Azure não envolve a alterar a definição de alerta, a consulta ou a configuração de qualquer forma. A única alteração necessária é que no Azure, pode efetuar todas as ações através da utilização de um grupo de ação. Se os grupos de ação já estão associados com o alerta, que sejam incluídos quando expandido no Azure.

> [!NOTE]
> Microsoft irá expandir automaticamente alertas criados na análise de registos para alertas do Azure, a partir no 14 de Maio de 2018, uma série periódica até concluído. Se tiver quaisquer problemas ao criar [grupos ação](monitoring-action-groups.md), utilize [estes passos de remediação](monitoring-alerts-extend-tool.md#troubleshooting) obter grupos de ação criados automaticamente. Pode utilizar estes passos até 5 de Julho de 2018. 
> 

Quando agendar alertas na área de trabalho do Log Analytics ser expandido para o Azure, podem continuar a funcionar e não em que qualquer forma comprometer a configuração. Quando agendada, os alertas poderão não estar disponíveis para modificação temporariamente, mas pode continuar a criar novos alertas do Azure, durante este período. Se tentar editar ou criar alertas a partir do portal do Operations Management Suite, terá a opção para continuar a criá-los a partir da sua área de trabalho de análise de registos. Também pode optar por criá-los a partir dos alertas do Azure no portal do Azure.

 ![Captura de ecrã da opção para criar alertas a partir de análise de registos ou alertas do Azure](./media/monitor-alerts-extend/ScheduledDirection.png)

> [!NOTE]
> Expandir alertas de análise de registos para o Azure não pagar a sua conta. Utilização de alertas do Azure para a consulta com base em alertas de análise de registos não é faturada quando utilizada dentro dos limites e condições indicado no [Monitor Azure preços política](https://azure.microsoft.com/pricing/details/monitor/).  


### <a name="how-to-extend-your-alerts-voluntarily"></a>Como expandir os alertas voluntariamente
Para expandir os alertas para alertas do Azure, pode utilizar um assistente disponível no portal do Operations Management Suite ou, para fazer através de programação, por isso, ao utilizar uma API. Para obter mais informações, consulte [alargar alertas para o Azure utilizando o portal do Operations Management Suite e API](monitoring-alerts-extend-tool.md).

## <a name="experience-after-extending-your-alerts"></a>Após expandir os alertas de experiência
Depois dos alertas são expandidos para alertas do Azure, podem continuar a estar disponíveis no portal do Operations Management Suite para a gestão de não diferente antes.

![Portal de captura de ecrã do Operations Management Suite, com alertas listados](./media/monitor-alerts-extend/PostExtendList.png)

Quando tenta editar um alerta existente ou criar um novo alerta no portal do Operations Management Suite, será automaticamente redirecionado para alertas do Azure.  

> [!NOTE]
> Certifique-se de que as permissões atribuídas a indivíduos que tem de adicionar ou editar alertas atribuídas corretamente no Azure. Para compreender que tem de conceder de permissões, consulte [permissões para utilizar a monitorização do Azure e alertas](monitoring-roles-permissions-security.md).  
> 

Pode continuar a criar alertas a partir de [API de análise do registo](../log-analytics/log-analytics-api-alerts.md) e [modelo de recursos de análise do registo](../monitoring/monitoring-solutions-resources-searches-alerts.md). Tem de incluir grupos da ação quando fazê-lo.

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre as ferramentas para [iniciar expandem alertas a partir de análise de registos no Azure](monitoring-alerts-extend-tool.md).
* Saiba mais sobre o [experiência de alertas do Azure](monitoring-overview-unified-alerts.md).
* Saiba como criar [registar alertas nos alertas do Azure](monitor-alerts-unified-log.md).

---
title: Expandir alertas (cópia) análise de registos para alertas do Azure - descrição geral | Microsoft Docs
description: Descrição geral do processo para copiar alertas de análise de registos no portal do OMS para alertas do Azure, com detalhes endereçamento preocupações de cliente comuns.
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: vinagara
ms.openlocfilehash: 296bdc6939ef8ab67656e5805cd603b50bce211e
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763158"
---
# <a name="extend-log-analytics-alerts-to-azure-alerts"></a>Expandir a alertas de análise de registos para alertas do Azure
Até recentemente, Log Analytics do Azure incluída as suas próprias funcionalidades de alerta, o que foi proativamente notificá-lo de condições com base nos dados de análise de registos.  Gestão de regras de alertas que foram executadas no [portal do Microsoft operação Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). A nova experiência de alertas tem agora a alerta experiência integrada em vários serviços no Microsoft Azure. A nova experiência está disponível como **alertas** no Monitor do Azure no portal do Azure e suporta a alertas de atividade registos, métricas e registos de análise de registos e o Application Insights. 

## <a name="benefits-of-extending-your-alerts"></a>Vantagens de expandir os alertas
Existem várias vantagens de criar e gerir alertas na porta do Azure, tais como:

- Ao contrário no portal do OMS, onde apenas 250 alertas foi criadas e visualizadas; nos alertas do Azure esta limitação não está presente
- De alertas do Azure, todos os tipos de alerta podem ser geridos, enumerar e visualizados; Análise de registos não apenas alertas estava anteriormente as maiúsculas e minúsculas
- Controlar o acesso aos utilizadores para que apenas a monitorização e alertas, utilizando [função de Monitor do Azure](monitoring-roles-permissions-security.md)
- Utilizam alertas do Azure [ação grupos](monitoring-action-groups.md), que permite-lhe ter mais de uma ação para cada alerta, incluindo SMS, enviar uma chamada de voz, invocar um Runbook da automatização, invocar um Webhook, configure um conector ITSM e muito mais. 

## <a name="process-of-extending-your-alerts"></a>Processo de alargar os alertas
O processo de mover os alertas de análise de registos para alertas do Azure, **não** envolvem alterar a definição de alerta, a consulta ou a configuração de qualquer forma. A única alteração necessária é que no Azure, todas as ações como notificação por correio eletrónico, uma chamada webhook, executar um runbook de automatização ou ligar à sua ferramenta ITSM são efetuadas através de um grupo de ação. Se os grupos de ação já estão associados com o alerta - estarão incluídas quando expandido no Azure.

> [!NOTE]
> Microsoft automaticamente irá expandir alertas criados na análise de registos para alertas do Azure ao iniciar no **14 de Maio de 2018** numa série periódica até concluído. Se existirem problemas na criação de [ação grupos](monitoring-action-groups.md), utilizador pode utilizar [passos de remediação apresentados](monitoring-alerts-extend-tool.md#troubleshooting) até **5 de Julho de 2018** obter grupos de ação criados automaticamente. 
> 

Quando se encontram agendadas alertas na área de trabalho de análise de registos para expandir para o Azure, irá continuar a funcionar e será **não** de qualquer forma comprometer a configuração. Quando agendada, os alertas podem não estar disponíveis para edição/modificação temporariamente; mas novos alertas do Azure podem continuar a ser criado durante este período. Se tentar editar ou criar alertas a partir do portal do OMS, terá a opção para continuar a criação da sua área de trabalho de análise de registos ou a partir de alertas do Azure no portal do Azure.

 ![Durante o período agendado, a ação do utilizador em alertas redirecionado para o Azure](./media/monitor-alerts-extend/ScheduledDirection.png)

> [!NOTE]
> expandir a alertas de análise de registos para o Azure não pagar a sua conta e a utilização de alertas do Azure para alertas de análise de registos de baseadas em consultas serão não cobradas quando utilizada dentro dos limites e condições indicado na [Monitor Azure preços da política](https://azure.microsoft.com/pricing/details/monitor/)  

Possam desfrutar as vantagens de expandir alertas antes desta data, voluntariamente aceitar ao movê-los para alertas do Azure.

### <a name="how-to-voluntarily-extend-your-alerts"></a>Como expandir voluntariamente os alertas
Para expandir os alertas para alertas do Azure, incluímos dois métodos para concluir esta tarefa na sua área de trabalho.  Isto é possível a partir de um assistente disponível no portal do OMS ou através de programação utilizando uma nova API.  Para obter mais informações, consulte [alargar alertas para o Azure utilizando o portal do OMS e API](monitoring-alerts-extend-tool.md).

## <a name="experience-after-extending-your-alerts"></a>Após expandir os alertas de experiência
Depois dos alertas são expandidos para alertas do Azure, continuarão a estar disponíveis no portal do OMS de gestão não diferente antes.<br><br> ![Portal do OMS listagem alertas após ser expandido para o Azure](./media/monitor-alerts-extend/PostExtendList.png)

Quando tenta editar um alerta existente ou criar um novo alerta no portal do OMS, será automaticamente redirecionado para alertas do Azure.  

> [!NOTE]
> É necessário garantir que as permissões atribuídas a indivíduos que tem de adicionar ou editar alertas atribuídas corretamente no Azure.  Reveja, [permissões para utilizar a monitorização do Azure e alertas](monitoring-roles-permissions-security.md) para compreender que permissões tem de conceder.  
> 

Criação irá continuar a trabalhar a partir de um alerta a [API de análise do registo](../log-analytics/log-analytics-api-alerts.md) e [modelo de recursos de análise do registo](../monitoring/monitoring-solutions-resources-searches-alerts.md), com apenas um menor alterar esse tem de ser aplicadas - grupos de ação tem de ser incluídos.

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre as ferramentas para [iniciar expandem alertas a partir de análise de registos no Azure](monitoring-alerts-extend-tool.md)
* Saiba mais sobre as novas [experiência de alertas do Azure](monitoring-overview-unified-alerts.md).
* Saiba como criar [registar alertas nos alertas do Azure](monitor-alerts-unified-log.md).

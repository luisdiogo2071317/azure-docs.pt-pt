---
title: Expandir alertas (cópia) a partir do portal do OMS no Azure - descrição geral | Microsoft Docs
description: Descrição geral do processo de alertas de cópia do portal do OMS para alertas do Azure, os detalhes em torno preocupações de cliente comuns.
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
ms.date: 03/16/2018
ms.author: vinagara
ms.openlocfilehash: 356c1343443b33e565c65ef0693b8d8455ff1d1b
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="extend-copy-alerts-from-oms-portal-into-azure"></a>Expandir alertas (cópia) a partir do portal do OMS no Azure
O portal do Operations Management Suite (OMS) mostra apenas os alertas de análise de registos.  A nova experiência de alertas tem agora a alerta experiência integrada em vários serviços e partes no Microsoft Azure. A nova experiência disponível como **alertas** no Monitor do Azure do Azure portal contém alertas de registo de atividade, métricas alertas e alertas de registo para análise de registos e o Application Insights. 


Mas para alguns utilizadores, a utilização de análise de registos e a funcionalidade allied, como alertas, foi submetido [portal do Microsoft operação Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). E, por conseguinte, para permitir-lhe gerir facilmente os seus outros recursos do Azure, juntamente com a sua utilização de análise de registos - sistematicamente Microsoft tem sido garantir que as capacidades do portal do OMS também estão disponíveis no portal do Azure. Em que regard, alertas do Azure já permite aos utilizadores gerir alertas baseadas em consulta para análise de registos, para obter mais informações, consulte [registar alertas de alertas do Azure](monitor-alerts-unified-log.md). Alertas no Monitor do Azure, os alertas criados no portal do OMS já estão listadas na área de trabalho de análise de registo adequado. Mas qualquer edição ou alterar esses alertas criados no portal do OMS, exigir que o utilizador deixe do Azure e utilizar o portal do OMS; em seguida, volte para o Azure, se são necessárias para gerir a qualquer outro serviço. Para reduzir este hardship, Microsoft agora é permitir que os utilizadores expandir os respetivos alertas a partir do portal do OMS no Azure.

## <a name="benefits-of-extending-your-alerts"></a>Vantagens de expandir os alertas
Para além do benefício acumulado no não ter de navegar fora do portal do Azure, existem outras vantagens salientes expandir alertas a partir do portal do OMS no Azure

- Ao contrário no portal do OMS, onde apenas 250 alertas foi criadas e visualizadas; nos alertas do Azure esta limitação não está presente
- De alertas do Azure, todos os tipos de alerta podem ser geridos, enumerar e visualizados; Análise de registos não apenas alertas como é o caso com o portal do OMS
- Utilizam alertas do Azure [ação grupos](monitoring-action-groups.md), que permitem-lhe ter mais de uma ação para cada alerta, incluindo SMS, chamada de voz, o Runbook de automatização, Webhook, ITSM conector e muito mais. Enquanto os alertas de análise de registos são limitadas em ambos os o número, bem como tipo de possíveis de ações

## <a name="process-of-extending-your-alerts"></a>Processo de alargar os alertas
O processo de alargar alertas a partir do portal do OMS no Azure, **não** envolvem alterar a definição de alerta, a consulta ou a configuração de qualquer forma. A única alteração necessária é que, no Azure, todas as ações como notificação por correio eletrónico, chamada webhook, executar o runbook de automatização ou ligar à ferramenta ITSM são efetuadas através do grupo de ação. Por conseguinte, se o grupo de medidas adequadas está associado com o alerta - estes irão tornar-se expandido no Azure.

Uma vez que o processo de alargar nondestructive e não interruptive, a Microsoft irá expandir alertas criados automaticamente no portal do OMS para alertas do Azure - começando **23 de Abril de 2018**. Deste dia, a Microsoft irá começar agendar a expandir os alertas no Azure e efetuar gradualmente todos os alertas presentes no portal do OMS, gerido a partir do portal do Azure. 

Quando alertas na área de trabalho do Log Analytics obterem agendadas para alargar no Azure, irá continuar a funcionar e será **não** de qualquer forma comprometer a monitorização. Quando agendada, os alertas podem não estar disponíveis para edição/modificação temporariamente; mas novos alertas do Azure podem continuar a ser criada no momento breve. Neste breve período, se qualquer edição ou a criação do alerta é efetuada no portal do OMS, os utilizadores têm a opção para continuar para Log Analytics do Azure ou alertas do Azure.

 ![Durante o período agendado, a ação do utilizador em alertas redirecionado para o Azure](./media/monitor-alerts-extend/ScheduledDirection.png)

> [!NOTE]
> Não é cobrada a expandir alertas a partir do portal do OMS no Azure e utilização do Azure alerta para consulta com base em alertas de análise de registos serão não cobradas, quando utilizada dentro de limites e condições indicadas na [Monitor Azure preços da política](https://azure.microsoft.com/en-us/pricing/details/monitor/)  

Os utilizadores possam desfrutar as vantagens de expandir alertas antes desta data; ao aceitar voluntariamente para tornar os alertas geríveis no Azure.

### <a name="how-to-voluntarily-extending-your-alerts"></a>Como expandir voluntariamente os alertas
Para ativar utilizadores OMS uma passagem fácil para os alertas do Azure, a Microsoft criou ferramentas para permitir a expandir os alertas. Os clientes portais do Microsoft OMS podem expandir os respetivos alertas para o Azure a partir de um assistente no portal do OMS (ou) por uma abordagem programática através de uma nova API. Para obter mais informações, consulte [alargar alertas para o Azure utilizando o portal do OMS e API](monitoring-alerts-extend-tool.md).


## <a name="usage-after-extending-your-alerts"></a>Utilização após expandir os alertas
Conforme indicado, irão ser expandidas em alertas criados no Microsoft operação Management Suite para alertas do Azure; Depois, que pode geri-los a partir do Azure. Alertas irão continuar a ser apresentado no portal do OMS - secção de definição de alerta. Conforme ilustrado abaixo:

 ![Portal do OMS listagem alertas após ser expandido para o Azure](./media/monitor-alerts-extend/PostExtendList.png)

Para todas as operações de alertas, como a edição ou criação efetuada no portal do OMS, os utilizadores serão direcionados transparente para alertas do Azure. Alerta de criação vai continuar do existente [API de análise do registo](../log-analytics/log-analytics-api-alerts.md) como anterior, com alteração apenas secundária que depois de alertas são expandidos no Azure - grupos de ação iriam têm de ser associada na agenda.

## <a name="next-steps"></a>Passos Seguintes

* Saiba das ferramentas para [iniciar expandir os alertas do OMS no Azure](monitoring-alerts-extend-tool.md)
* Saiba mais sobre as novas [experiência de alertas do Azure](monitoring-overview-unified-alerts.md).
* Saiba mais sobre [registar alertas nos alertas do Azure](monitor-alerts-unified-log.md).

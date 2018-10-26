---
title: Alertas de registo de atividades no Azure Monitor
description: Ser notificado através de SMS, webhook, SMS, e-mail e além disso, quando determinados eventos ocorrem no registo de atividades.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 6f007ca3aacb338c14bf481ee58407596c8290ad
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50091628"
---
# <a name="alerts-on-activity-log"></a>Alertas de registo de atividades 

## <a name="overview"></a>Descrição geral
Alertas de registo de atividade são alertas que ativar a ocorrência de um novo log de eventos de atividade que coincida com as condições especificadas no alerta. Eles são recursos do Azure, pelo que podem ser criadas com um modelo Azure Resource Manager. Eles também podem ser criados, atualizados ou eliminados no portal do Azure. Este artigo apresenta os conceitos por trás de alertas de registo de atividade. Em seguida, mostra como utilizar o portal do Azure para configurar um alerta sobre eventos de registo de atividade. Para obter mais informações sobre a utilização, consulte [criar e gerir alertas de registo de atividade](alert-activity-log.md).

> [!NOTE]
> Alertas **não pode** criado para eventos na categoria de alerta de registo de atividades

Normalmente, criar atividade de alertas de registo para receber notificações quando:

* Operações específicas ocorrerem nos recursos da sua subscrição do Azure, com freqüência no âmbito de recursos ou grupos de recursos específico. Por exemplo, poderá ser notificado quando qualquer máquina virtual na myProductionResourceGroup é eliminada. Em alternativa, pode querer ser notificado se quaisquer novas funções são atribuídas a um utilizador na sua subscrição.
* Ocorre um evento de estado de funcionamento do serviço. Eventos de estado de funcionamento do serviço incluem a notificação de incidentes e eventos de manutenção que se aplicam a recursos na sua subscrição.

Uma analogia simple para condições de noções básicas sobre em que as regras de alerta podem ser criadas no registo de atividades, é explorar ou filtrar eventos via [registo de atividades no Portal do Azure](monitoring-overview-activity-logs.md#query-the-activity-log-in-the-azure-portal). No Azure Monitor - registo de atividades, um pode filtrar ou localizar evento necessários e, em seguida, criar um alerta com o **Adicionar alerta de registo de atividade** botão.

Em ambos os casos, um alerta de registo de atividade monitoriza apenas para os eventos na subscrição na qual o alerta é criado.

Pode configurar um alerta de registo de atividade com base em qualquer propriedade de nível superior no objeto JSON para o evento do registo de atividades. Para obter mais informações, consulte [descrição geral do registo de atividades do Azure](./monitoring-overview-activity-logs.md#categories-in-the-activity-log). Para saber mais sobre eventos de estado de funcionamento do serviço, veja [receber alertas de registo de atividade nas notificações do serviço](./monitoring-activity-log-alerts-on-service-notifications.md). 

Alertas de registo de atividade tem algumas opções comuns:

- **Categoria**: administrativo, serviço de estado de funcionamento, dimensionamento automático, segurança, política e recomendação. 
- **Âmbito**: O recurso individual ou um conjunto de recursos para o qual o alerta no log de atividade está definido. Âmbito para um alerta de registo de atividades pode ser definido em vários níveis:
    - Nível de recurso: por exemplo, para uma máquina virtual específica
    - Nível de grupo de recursos: por exemplo, todas as máquinas virtuais no grupo de recursos específico
    - Nível de assinatura: por exemplo, todas as máquinas virtuais numa subscrição (ou) a todos os recursos numa subscrição
- **Grupo de recursos**: por predefinição, a regra de alerta é salvo no mesmo grupo de recursos que o destino definido no âmbito. O usuário também pode definir o grupo de recursos em que a regra de alerta deve ser armazenada.
- **Tipo de recurso**: Gestor de recursos definidos pelo espaço de nomes para o destino do alerta.

- **Nome da operação**: nome da operação de controlo de acesso de The Resource Manager Role-Based.
- **Nível**: O nível de gravidade do evento (verboso, informativo, aviso, erro ou crítico).
- **Estado**: O estado do evento, geralmente inicia, falhou ou foi concluída com êxito.
- **Evento iniciado por**: também conhecido como o "autor da chamada." O endereço de e-mail ou o identificador do Azure Active Directory do utilizador que executou a operação.

> [!NOTE]
> Numa subscrição de até 100 regras de alerta pode ser criado para uma atividade do âmbito em qualquer um de: um único recurso, todos os recursos no recurso de grupo (ou) ao nível da subscrição completa.

Quando um alerta de registo de atividade é ativado, ele usa um grupo de ação para gerar as ações ou notificações. Um grupo de ação é um conjunto reutilizável de recetores de notificação, como endereços de e-mail, números de telefone de URLs de webhook, nem o SMS. Os recetores podem ser referenciados a partir de vários alertas para centralizar e seus canais de notificação de grupo. Quando define o alerta de registo de atividades, tem duas opções. Pode:

* Utilize um grupo de ação existente no seu alerta de registo de atividades.
* Crie um novo grupo de ação.

Para saber mais sobre os grupos de ação, veja [criar e gerir grupos de ação no portal do Azure](monitoring-action-groups.md).


## <a name="next-steps"></a>Passos Seguintes
- Obter um [descrição geral dos alertas](monitoring-overview-alerts.md).
- Saiba mais sobre [criar e modificar alertas do registo de atividade](alert-activity-log.md).
- Reveja os [esquema de webhook de alerta de registo de atividades](monitoring-activity-log-alerts-webhook.md).
- Saiba mais sobre [notificações de estado de funcionamento de serviço](monitoring-service-notifications.md).


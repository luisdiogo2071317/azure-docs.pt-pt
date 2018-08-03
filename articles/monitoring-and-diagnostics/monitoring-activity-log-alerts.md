---
title: Criar alertas de registo de atividades clássica
description: Ser notificado por SMS, e-mail e webhook quando ocorrem determinados eventos no registo de atividades.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/18/2017
ms.author: johnkem
ms.component: alerts
ms.openlocfilehash: 120fd3552ad36b3d19179f39ca95ce2b3ee2c2e6
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426623"
---
# <a name="create-activity-log-alerts-classic"></a>Criar atividade de registo de alertas (clássico)

## <a name="overview"></a>Descrição geral
Alertas de registo de atividade são alertas que ativar a ocorrência de um novo log de eventos de atividade que coincida com as condições especificadas no alerta. Eles são recursos do Azure, pelo que podem ser criadas com um modelo Azure Resource Manager. Eles também podem ser criados, atualizados ou eliminados no portal do Azure. Este artigo apresenta os conceitos por trás de alertas de registo de atividade. Em seguida, mostra como utilizar o portal do Azure para configurar um alerta sobre eventos de registo de atividade.

> [!NOTE]

>  A nova [alertas](monitoring-overview-unified-alerts.md) experiência substituiu este procedimento. Este artigo é fornecido como referência para a experiência anterior. [Saiba mais](monitoring-activity-log-alerts-new-experience.md).

Normalmente, criar atividade de alertas de registo para receber notificações quando:

* Ocorrerem alterações específicas nos recursos da sua subscrição do Azure, com freqüência no âmbito de recursos ou grupos de recursos específico. Por exemplo, poderá ser notificado quando qualquer máquina virtual na myProductionResourceGroup é eliminada. Em alternativa, pode querer ser notificado se quaisquer novas funções são atribuídas a um utilizador na sua subscrição.
* Ocorre um evento de estado de funcionamento do serviço. Eventos de estado de funcionamento do serviço incluem a notificação de incidentes e eventos de manutenção que se aplicam a recursos na sua subscrição.

Em ambos os casos, um alerta de registo de atividade monitoriza apenas para os eventos na subscrição na qual o alerta é criado.

Pode configurar um alerta de registo de atividade com base em qualquer propriedade de nível superior no objeto JSON para o evento do registo de atividades. No entanto, o portal mostra as opções mais comuns:

- **Categoria**: administrativo, serviço de estado de funcionamento, o dimensionamento automático e recomendação. Para obter mais informações, consulte [descrição geral do registo de atividades do Azure](./monitoring-overview-activity-logs.md#categories-in-the-activity-log). Para saber mais sobre eventos de estado de funcionamento do serviço, veja [receber alertas de registo de atividade nas notificações do serviço](./monitoring-activity-log-alerts-on-service-notifications.md).
- **Grupo de recursos**
- **Recurso**
- **Tipo de recurso**
- **Nome da operação**: nome da operação de controlo de acesso de The Resource Manager Role-Based.
- **Nível**: O nível de gravidade do evento (verboso, informativo, aviso, erro ou crítico).
- **Estado**: O estado do evento, geralmente inicia, falhou ou foi concluída com êxito.
- **Evento iniciado por**: também conhecido como o "autor da chamada." O endereço de e-mail ou o identificador do Azure Active Directory do utilizador que executou a operação.

> [!NOTE]
> Quando a categoria é "administrativa", tem de especificar, pelo menos, um dos critérios anteriores no seu alerta. Não pode criar um alerta que é ativado sempre que for criado um evento nos registos de atividade.

Quando um alerta de registo de atividade é ativado, ele usa um grupo de ação para gerar as ações ou notificações. Um grupo de ação é um conjunto reutilizável de recetores de notificação, como endereços de e-mail, números de telefone de URLs de webhook, nem o SMS. Os recetores podem ser referenciados a partir de vários alertas para centralizar e seus canais de notificação de grupo. Quando define o alerta de registo de atividades, tem duas opções. Pode:

* Utilize um grupo de ação existente no seu alerta de registo de atividades.
* Crie um novo grupo de ação.

Para saber mais sobre os grupos de ação, veja [criar e gerir grupos de ação no portal do Azure](monitoring-action-groups.md).

Para saber mais sobre as notificações de estado de funcionamento do serviço, veja [receber alertas de registo de atividade nas notificações de estado de funcionamento do serviço](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="create-an-alert-classic-on-an-activity-log-event-with-a-new-action-group-by-using-the-azure-portal"></a>Criar um alerta (clássico) no evento do registo de atividades com um novo grupo de ação com o portal do Azure
1. Na [portal](https://portal.azure.com), selecione **Monitor**.

    ![O serviço de "Monitor"](./media/monitoring-activity-log-alerts/home-monitor.png)
1. Na **registo de atividades** secção, selecione **alertas (clássico)**.

    ![O separador "Alertas"](./media/monitoring-activity-log-alerts/alerts-blades.png)
1. Selecione **Adicionar alerta de registo de atividade**e preencha os campos.

1. Introduza um nome na **nome de alerta de registo de atividades** caixa e selecione um **Descrição**.

    ![O comando "Adicionar alerta de registo de atividade"](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

1. O **subscrição** caixa autofills com a sua subscrição atual. Esta subscrição é aquela na qual o grupo de ação é guardado. O recurso de alerta é implementado para esta subscrição e monitoriza os eventos de registo de atividade do mesmo.

    ![A caixa de diálogo "Adicionar alerta de registo de atividade"](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

1. Selecione o **grupo de recursos** no qual o recurso de alerta é criado. Não é o grupo de recursos que é monitorizado pelo alerta. Em vez disso, é o grupo de recursos onde está localizado o recurso de alerta.

1. Opcionalmente, selecione um **categoria de evento** para modificar os filtros adicionais que são apresentados. Para eventos administrativos, os filtros incluem **grupo de recursos**, **recurso**, **tipo de recurso**, **nome da operação**, **Nível**, **estado**, e **evento iniciado por**. Estes valores identificam os eventos que este alerta deve monitorizar.

    >[!NOTE]
    >Tem de especificar, pelo menos, um dos critérios anteriores no seu alerta. Não pode criar um alerta que é ativado sempre que for criado um evento nos registos de atividade.
    >
    >

1. Introduza um nome na **nome do grupo de ação** caixa e introduza um nome na **nome abreviado** caixa. O nome abreviado é utilizado em vez de um nome de grupo de ação completo quando as notificações são enviadas através deste grupo.

1.  Defina uma lista de ações ao fornecer a ação:

    a. **Nome**: introduza o nome, o alias ou o identificador da ação.

    b. **Tipo de ação**: selecione SMS, e-mail ou webhook.

    c. **Detalhes**: com base no tipo de ação, introduza um número de telefone, endereço de e-mail ou webhook URI.

1.  Selecione **OK** para criar o alerta.

O alerta demora alguns minutos para propagar totalmente e, em seguida, fique ativa. Aciona quando novos eventos correspondem a critérios do alerta.

Para obter mais informações, consulte [entender o esquema de webhook usado nos alertas de registo de atividade](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>O grupo de ações definido nessas etapas é reutilizável que um grupo de ação existente para todas as definições de alerta futuras.
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-by-using-the-azure-portal"></a>Criar um alerta no evento do registo de atividades para um grupo de ação existente com o portal do Azure
1. Siga os passos 1 a 7 na secção anterior para criar o alerta de registo de atividades.

1. Sob **notificar através de**, selecione a **existente** botões do grupo de ação. Selecione um grupo de ação existente na lista.

1. Selecione **OK** para criar o alerta.

O alerta demora alguns minutos para propagar totalmente e, em seguida, fique ativa. Aciona quando novos eventos correspondem a critérios do alerta.

## <a name="manage-your-alerts"></a>Gerir os alertas

Depois de criar um alerta, é visível na seção de alertas do painel Monitor. Selecione o alerta que pretende gerir para:

* Editá-lo.
* Elimine-o.
* Desativar ou ativá-lo, se quiser temporariamente parar ou retomar a receção de notificações para o alerta.

## <a name="next-steps"></a>Passos Seguintes
- Obter um [descrição geral dos alertas](monitoring-overview-alerts.md).
- Saiba mais sobre [limitação de velocidade de notificação](monitoring-alerts-rate-limiting.md).
- Reveja os [esquema de webhook de alerta de registo de atividades](monitoring-activity-log-alerts-webhook.md).
- Saiba mais sobre [grupos de ação](monitoring-action-groups.md).  
- Saiba mais sobre [notificações de estado de funcionamento de serviço](monitoring-service-notifications.md).
- Criar uma [alerta de registo de atividades para monitorizar todas as operações de motor de dimensionamento automático na sua subscrição](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).
- Criar uma [alerta de registo de atividades para monitorizar todas as operações de no/dimensionamento-Escalamento horizontal de dimensionamento automático com falhas na sua subscrição](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).

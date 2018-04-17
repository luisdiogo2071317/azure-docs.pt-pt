---
title: Criar alertas de registo de atividade e geri-los utilizando a nova experiência de alertas (pré-visualização) no Monitor do Azure | Microsoft Docs
description: Este artigo fornece informações sobre como criar alertas de registo de atividade a partir do separador de alertas (pré-visualização) no Monitor do Azure. Este artigo fornece detalhes sobre a nova experiência de utilizador para esta funcionalidade.
author: JYOTHIRMAISURI
manager: vvithal
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: aabc0e57-78cd-44dd-a8d1-af5e1e567360
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2018
ms.author: v-jysur
ms.custom: ''
ms.openlocfilehash: 740edfd158d56fcb224cf93d1720a2330779bfd6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="create-activity-log-alerts-using-the-new-alerts-preview-experience"></a>Criar registo de atividade experiência de alertas com os novos alertas (pré-visualização)

Alertas de registo de atividade são os alertas que obterem ativados quando ocorre a um novo registo de eventos de atividade que satisfaça as condições especificadas no alerta.

Estes alertas são para recursos do Azure, podem ser criados utilizando um modelo Azure Resource Manager. Também podem ser criados, atualizar ou eliminados no portal do Azure. Este artigo apresenta os conceitos atrás de alertas de registo de atividade. Em seguida, mostra como utilizar o portal do Azure para configurar um alerta sobre eventos de registo de atividade utilizando a nova experiência em [alertas do Azure (pré-visualização)](monitoring-overview-unified-alerts.md).

Normalmente, criar atividade alertas de registo para receber notificações quando ocorrem alterações específicas nos recursos na sua subscrição do Azure, muitas vezes, no âmbito de grupos de recurso específico ou recurso. Por exemplo, poderá ser notificado quando qualquer máquina virtual (grupo de recursos de exemplo) **myProductionResourceGroup** é eliminada, ou poderá pretender ser notificado se quaisquer novas funções são atribuídas a um utilizador na sua subscrição.

Pode configurar um alerta de registo de atividade com base em qualquer propriedade de nível superior no objeto JSON para um registo de eventos de atividade. No entanto, o portal mostra as opções mais comuns, conforme detalhado nas seguintes secções:

>[!NOTE]

> Quando a categoria "administrativa", tem de especificar, pelo menos, um dos critérios de anteriores o alerta. Não pode criar um alerta que activa sempre que é criado um evento nos registos de atividade.
>

Quando um alerta de registo de atividade é ativado, utiliza um grupo de ação para gerar as ações ou notificações. Um grupo de ação é um conjunto reutilizável de recetores de notificação, tais como endereços de correio eletrónico, números de telefone de URLs de webhook ou SMS. Os recetores podem ser referenciados a partir de vários alertas para centralizar e os canais de notificação de grupo. Quando definir o alerta de registo de atividade, tem duas opções. Pode:

* Utilize um grupo de ação existente no seu alerta de registo de atividade.
* Crie um novo grupo de ação.

Para mais informações sobre grupos de ação, consulte o artigo [criar e gerir grupos de ação no portal do Azure](monitoring-action-groups.md).

Para obter mais informações sobre notificações de estado de funcionamento do serviço, consulte o artigo [receber alertas de registo de atividade em notificações do Estado de funcionamento do serviço](monitoring-activity-log-alerts-on-service-notifications.md).


## <a name="whats-new-in-alerts-preview-for-activity-logs"></a>Novidades na pré-visualização de alertas para os registos de atividade?

[Alertas do Azure (pré-visualização)](monitoring-overview-unified-alerts.md) fornece agora a experiência de utilizador para os alertas de registo de atividade. Com o [avançada experiência de utilizador para alertas](monitoring-overview-unified-alerts.md), agora, pode:

- [Criar](#create-an-alert-rule-for-an-activity-log) e [gerir](#view-and-manage-activity-log-alert-rules) a atividade iniciar regras de alertas, a partir de **Monitor** > **alertas (pré-visualização)** painel. Saiba mais sobre [registos de atividade](monitoring-overview-activity-logs.md).

- **Novas opções para o destino de alertas**: ao criar uma regra de alerta de registo de atividade, agora, pode selecionar um recurso de destino ou um grupo de recursos ou uma subscrição.


## <a name="create-an-alert-rule-for-an-activity-log"></a>Criar uma regra de alerta para um registo de atividade

> [!NOTE]

>  Ao criar as regras de alerta, certifique-se do seguinte:

> - Subscrição no âmbito não é diferente da subscrição em que o alerta é criado.
- Os critérios têm de ser nível/Estado/chamador / grupo de recursos / id de recurso / tipo de recurso / categoria de evento no qual o alerta está configurado.
- Não condição "anyOf" ou condições aninhadas na configuração de alertas JSON (basicamente, tudo apenas um é permitido com nenhuma tudo/anyOf adicional).


Utilize o seguinte procedimento:

1. No portal do Azure, selecione **Monitor** > **alertas (pré-visualização).**
2. Clique em **nova regra de alerta** na parte superior do **alertas (pré-visualização)** janela.

     ![nova regra de alerta](./media/monitoring-activity-log-alerts-new-experience/create-new-alert-rule.png)

     O **criar regra** surge a janela.

      ![novas opções de regra de alerta](./media/monitoring-activity-log-alerts-new-experience/create-new-alert-rule-options.png)

3. **Em definir alertas condição,** forneça as seguintes informações e clique em **feito**.

    - **Destino de alerta:** para ver e selecionar o destino para o novo alerta, utilize **filtrar por subscrição** / **filtrar por tipo de recurso** e selecione o recurso ou grupo de recursos do lista apresentada.

    > [!NOTE]

    > pode selecionar um recurso, grupo de recursos ou uma subscrição completa.

    **Vista de exemplo de alerta de destino**

     ![Selecione o destino](./media/monitoring-activity-log-alerts-new-experience/select-target.png)

    - Em **critérios de destino**, clique em **adicionar critérios** selecione o tipo de sinal como **registo de atividade**.

    - Selecione o sinal a partir da lista apresentada.

    Pode selecionar a linha cronológica de histórico de registo e a lógica de alerta correspondente para o sinal de destino:

    **Adicionar o ecrã de critérios**

    ![adicionar critérios](./media/monitoring-activity-log-alerts-new-experience/add-criteria.png)

    **Tempo de histórico**: ao longo das últimas 12/6/24 horas, ao longo da última semana.

    **Alerta lógica**:

     - **Nível do evento**-o nível de gravidade do evento. **Verboso, informativa, aviso, erro**, ou **críticos**.
     - **Estado**: O estado do evento. **Foi iniciada, falha**, ou **foi concluída com êxito**.
     - **Evento iniciadas pelo**: também conhecido como o autor da chamada; O endereço de e-mail ou o identificador do Azure Active Directory do utilizador que executou a operação.

        **Gráfico de sinal de exemplo com alerta lógica aplicada** :

        ![ critérios selecionados](./media/monitoring-activity-log-alerts-new-experience/criteria-selected.png)

4. Em **definir os detalhes de regras de alerta**, forneça os detalhes seguintes:

    - **Nome da regra de alerta** – nome da nova regra de alerta
    - **Descrição** – uma descrição para a nova regra de alerta
    - **Guardar o alerta para o grupo de recursos** – selecione o grupo de recursos, onde pretende guardar esta nova regra.

5. Em **grupo ação**, no menu pendente, especifique o grupo de ação que pretende atribuir a esta regra de alerta de novo. Em alternativa, [criar um novo grupo de ação](monitoring-action-groups.md) e atribuir a nova regra. Para criar um novo grupo, clique em **+ novo grupo**.

6. Para ativar as regras depois de a criar, clique em **Sim** para **ativar regra após a criação** opção.
7. Clique em **criar regra de alerta**.

    É criada a nova regra de alerta para o registo de atividade e é apresentada uma mensagem de confirmação na parte superior direito da janela.

    ![ regra de alerta criada](./media/monitoring-activity-log-alerts-new-experience/alert-created.png)

    Pode ativar, desativar, editar ou eliminar uma regra. [Saiba mais](#view-and-manage-activity-log-alert-rules) sobre a gestão de regras de registo de atividade.

## <a name="view-and-manage-activity-log-alert-rules"></a>Ver e gerir regras de alerta de registo de atividade

1. No portal do Azure, clique em **Monitor** > **alertas (pré-visualização)** e clique em **gerir regras** na parte superior esquerdo da janela do.

    ![ Gerir regras de alertas](./media/monitoring-activity-log-alerts-new-experience/manage-alert-rules.png)

    É apresentada a lista de regras disponíveis.

2. Procure a regra de registo de atividade para modificar.

    ![ Procurar regras de alerta de registo de atividade](./media/monitoring-activity-log-alerts-new-experience/searth-activity-log-rule-to-edit.png)

    Pode utilizar os filtros disponíveis - **subscrição**, **grupo de recursos**, ou **recursos** para localizar a regra de atividade que pretende editar.

    > [!NOTE]

    > Só é possível editar **Descrição** , **destino critérios** e **grupos ação**.

3.  Selecione a regra e faça duplo clique para editar as opções de regra. Efetue as alterações necessárias e, em seguida, clique em **guardar**.

    ![ Gerir regras de alertas](./media/monitoring-activity-log-alerts-new-experience/activity-log-rule-edit-page.png)

4.  Pode desativar, ativar ou eliminar uma regra. Selecione a opção adequada na parte superior da janela, depois de selecionar a regra, conforme detalhado no passo 2.


## <a name="next-steps"></a>Passos Seguintes

- [Alertas de registo de atividade de arquivo](monitoring-archive-activity-log.md)
- [Registos de atividade de fluxo para os Event hubs](monitoring-stream-activity-logs-event-hubs.md)
- [Migrar para registos de atividade](monitoring-migrate-management-alerts.md)

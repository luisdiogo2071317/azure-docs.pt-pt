---
title: Monitorizar os runbooks de automatização do Azure com alertas de registos de atividade
description: Este artigo orienta-o através de runbooks de automatização do Azure com o registo de atividade de monitorização
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 05/17/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: a0bd291af98477308cda898580fd52e33b1e6bbd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="monitoring-runbooks-with-azure-activity-log-alerts"></a>Monitorização de runbooks com alertas de registo de atividade do Azure

Neste artigo, irá aprender a criar alertas com base no estado de conclusão de runbooks.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Azure https://portal.azure.com

## <a name="create-alert"></a>Criar alerta

Alertas permitem-lhe definir uma condição para monitorizar e uma ação a tomar quando esse condição for satisfeita.

No portal do Azure, navegue para **todos os serviços** e selecione **Monitor**. Na página do Monitor, selecione **alertas** e clique em **+ nova regra de alerta**.

### <a name="define-the-alert-condition"></a>Definir a condição do alerta

1. Em **1. Definir a condição de alerta**, clique em **+ destino selecione**. Escolha a sua subscrição e, em **filtrar por tipo de recurso**, selecione **as contas de automatização**. Escolha a sua conta de automatização e clique em **feito**.

   ![Selecione um recurso para o alerta](./media/automation-alert-activity-log/select-resource.png)

### <a name="configure-alert-criteria"></a>Configurar os critérios de alerta

1. Clique em **+ adicionar critérios**. Selecione **métricas** para o **assinalar o tipo**e escolha **Total de tarefas** da tabela.

1. O **lógica de sinal configurar** página é onde é possível definir a lógica de que o alerta é acionado. Sob o gráfico histórico são apresentados com duas dimensões, **nome do Runbook** e **estado**. As dimensões são diferentes propriedades para uma métrica que podem ser utilizadas para filtrar os resultados. Para **nome do Runbook**, selecione o runbook que pretende alerta sobre ou deixe em branco alerta em todos os runbooks. Para **estado**, selecione um Estado da lista pendente que pretende monitorizar. Os valores de nome e o estado de runbook que aparecem na lista pendente são apenas para as tarefas que têm executou da semana passada.

   Se pretender alerta sobre um Estado ou um runbook que não seja apresentado na lista pendente, clique em de **\+** junto da dimensão. Esta ação abre uma caixa de diálogo permite-lhe introduzir um valor personalizado, o que não tem emitidos para essa dimensão recentemente. Se introduzir um valor que não existe para uma propriedade não será acionado o alerta.

1. Em **alerta lógica**, definir a condição e o limiar para o alerta. É apresentada uma pré-visualização da sua condição definida por baixo.

1. Em **Evaluated com base no** selecione o período de tempo para a consulta e a frequência com que pretende que essa consulta foi executada. Por exemplo, se escolher **nos últimos 5 minutos** para **período** e **cada 1 minuto** para **frequência**, procura o alerta de número de runbooks que cumpre os critérios de durante os últimos 5 minutos. Esta consulta é executada a cada minuto e assim que os critérios de alerta que definiu já não se encontrar numa janela de 5 minutos, o alerta é resolvido em si. Quando terminar, clique em **feito**.

   ![Selecione um recurso para o alerta](./media/automation-alert-activity-log/configure-signal-logic.png)

### <a name="define-alert-details"></a>Definir os detalhes do alerta

1. Em **2. Definir os detalhes do alerta**, o alerta de dar um nome amigável e uma descrição. Definir o **gravidade** para corresponder a condição de alerta. Existem cinco gravidades entre 0 e 5. Os alertas são tratados independente mesmo a gravidade, pode fazer corresponder a gravidade para corresponder a lógica de negócio.

1. Na parte inferior da secção é um botão que permite-lhe ativar a regra após a conclusão. As regras são ativadas por predefinição durante a criação. Se selecionar não, pode criar o alerta e é criado num **desativado** estado. Do **regras** página no Monitor do Azure, pode selecioná-lo e clique em **ativar** para ativar o alerta quando estiver preparado.

### <a name="define-the-action-to-take"></a>Definir a ação a tomar

1. Em **3. Definir ação grupo**, clique em **+ novo grupo de ação**. Um grupo de ação é um grupo de ações que podem ser utilizadas em vários alertas. Estes podem incluir, mas não estão limitados a e-mail notificações, runbooks, webhooks e muitos mais. Para mais informações sobre grupos de ação, consulte o artigo [criar e gerir grupos de ação](../monitoring-and-diagnostics/monitoring-action-groups.md)

1. No **nome do grupo de ação** caixa, atribua-lhe um nome amigável e o nome abreviado. O nome abreviado é utilizado em vez de um nome de grupo ação completa, quando as notificações são enviadas através deste grupo.

1. No **ações** secção em **tipo de ação**, selecione **E-Mail/SMS/Push/voz**.

1. No **E-Mail/SMS/Push/voz** página, atribua um nome. Verifique o **E-Mail** caixa de verificação e introduza um endereço de correio electrónico válido a ser utilizado.

   ![Configurar o grupo de ação de correio eletrónico](./media/automation-alert-activity-log/add-action-group.png)

1. Clique em **OK** no **E-Mail/SMS/Push/voz** página para fechá-lo e clique em **OK** para fechar o **adicionar grupo de ação** página. O nome especificado nesta página é guardado como o **nome da ação**.

1. Quando terminar, clique em **Guardar**. Esta ação cria a regra que o alerta quando um runbook foi concluído com um determinado Estado.

> [!NOTE]
> Ao adicionar um endereço de correio eletrónico a um grupo de ação, um e-mail de notificação é enviado a indicar que o endereço foi adicionado a um grupo de ação.

## <a name="notification"></a>Notificação

Quando os critérios de alerta são cumpridos, o grupo de ação é executada a ação definida. No exemplo neste artigo, é enviada uma mensagem de e-mail. A imagem seguinte é um exemplo de uma mensagem de e-mail que recebe depois do alerta é acionado:

![Alerta de e-mail](./media/automation-alert-activity-log/alert-email.png)

Depois da métrica já não se encontra fora do limiar definido, o alerta está desativado e o grupo de ação é executada a ação definida. Se for selecionado um tipo de ação de correio eletrónico, um e-mail de resolução é enviado a indicar que foi resolvido.

## <a name="next-steps"></a>Passos Seguintes

Continue para o seguinte artigo para obter informações sobre outras formas que pode integrar alertings na sua conta de automatização.

> [!div class="nextstepaction"]
> [Utilize um alerta para acionar um runbook de automatização do Azure](automation-create-alert-triggered-runbook.md)
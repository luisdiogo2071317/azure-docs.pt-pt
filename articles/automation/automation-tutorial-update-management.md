---
title: Gerir atualizações e correções para as VMs Windows do Azure
description: Este artigo apresenta uma descrição geral de como utilizar a Automatização do Azure - Gestão de atualizações para gerir atualizações e correções para as suas VMs Windows do Azure.
services: automation
author: zjalexander
ms.service: automation
ms.component: update-management
ms.topic: tutorial
ms.date: 02/28/2018
ms.author: zachal
ms.custom: mvc
ms.openlocfilehash: 84ec2a5852e6aaeb4b9fe6ef11924209d03fb54b
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
---
# <a name="manage-windows-updates-with-azure-automation"></a>Gerir atualizações do Windows com a Automatização do Azure

A gestão de atualizações permite gerir atualizações e correções para as suas máquinas virtuais.
Neste tutorial, irá aprender a avaliar rapidamente o estado das atualizações disponíveis, agendar a instalação de atualizações necessárias, rever os resultados de implementação e criar um alerta para verificar se as atualizações são aplicadas com êxito.

Para obter informações sobre preços, veja [Preços de Automatização para Gestão de atualizações](https://azure.microsoft.com/pricing/details/automation/)

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Carregar uma VM para gestão de atualizações
> * Ver avaliações de atualizações
> * Configurar alertas
> * Agendar uma implementação de atualizações
> * Ver os resultados de uma implementação

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa de:

* Uma subscrição do Azure. Se ainda não tiver uma, pode [ativar o crédito do Azure mensal para subscritores do Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ou inscrever-se numa [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Um [conta de Automatização](automation-offering-get-started.md) para reter os runbooks de observador e ação e a Tarefa de Observador.
* Uma [máquina virtual](../virtual-machines/windows/quick-create-portal.md) para carregar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em https://portal.azure.com.

## <a name="enable-update-management"></a>Ativar a Gestão de atualizações

Primeiro, terá de ativar a Gestão de atualizações na sua VM para este tutorial.

1. No portal do Azure, no menu da esquerda, selecione **Máquinas virtuais** e selecione uma VM da lista.
2. Na página da VM, clique em **Gestão de atualizações**, na secção **Operações**. A página **Ativar Gestão de Atualizações** abre.

A validação é executada para determinar se a Gestão de atualizações está ativada para esta VM. Esta validação inclui a verificação da existência de uma área de trabalho do Log Analytics e da conta de Automatização ligada, e se a Solução de gestão de atualizações está na área de trabalho.

A área de trabalho do [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) serve para recolher dados gerados pelas funcionalidades e serviços, tais como a Gestão de atualizações. A área de trabalho fornece uma localização única para rever e analisar dados de várias origens.

O processo de validação verifica ainda se a VM está aprovisionada com o Microsoft Monitoring Agent (MMA) e a função de trabalho de runbook híbrida de Automatização.
Este agente serve para comunicar com a Automatização do Azure e obter informações sobre o estado de atualização. O agente requer que a porta 443 esteja aberta para comunicar com o serviço de Automatização do Azure e para transferir atualizações.

Se for detetada a falta de qualquer um dos seguintes pré-requisitos durante a inclusão, estes serão adicionados automaticamente:

* Área de trabalho do [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)
* [Conta de Automatização](./automation-offering-get-started.md)
* Uma [Função de trabalho de runbook híbrida](./automation-hybrid-runbook-worker.md) está ativada na VM

O ecrã **Gestão de Atualizações** é apresentado. Configure a localização, a área de trabalho do Log Analytics e a conta de Automatização a utilizar e clique em **Ativar**. Se os campos estiverem desativados, significa que outra solução de automatização está ativada para a VM e terá de ser utilizada a mesmo área de trabalho e conta de Automatização.

![Janela de ativação da solução de Gestão de atualizações](./media/automation-tutorial-update-management/manageupdates-update-enable.png)

A ativação da solução pode demorar alguns minutos. Durante este período, não deve fechar a janela do browser.
Após a ativação da solução, as informações sobre atualizações em falta na VM são transmitidas ao Log Analytics.
Pode demorar entre 30 minutos e 6 horas até que os dados fiquem disponíveis para análise.

## <a name="view-update-assessment"></a>Ver avaliação de atualizações

Depois de **Gestão de atualizações** ser ativada, o ecrã **Gestão de atualizações** aparece.
Se existirem atualizações em falta, poderá ver uma lista de atualizações em falta no separador **Atualizações em falta**.

Selecione a **LIGAÇÃO PARA INFORMAÇÕES** sobre a atualização para abrir o artigo de suporte para a atualização numa nova janela. Aqui, pode obter informações importantes sobre a atualização.

![Ver o estado de atualização](./media/automation-tutorial-update-management/manageupdates-view-status-win.png)

Se clicar em qualquer outro local na atualização, abre a janela **Pesquisa de Registos** para a atualização selecionada. A consulta para a pesquisa de registos está predefinida para essa atualização específica. Pode modificar esta consulta ou criar a sua própria consulta para ver informações detalhadas sobre as atualizações implementadas ou em falta no seu ambiente.

![Ver o estado de atualização](./media/automation-tutorial-update-management/logsearch.png)

## <a name="configure-alerting"></a>Configurar alertas

Neste passo, vai configurar um alerta para informá-lo quando as atualizações foram implementadas com êxito. O alerta que criar baseia-se numa consulta do Log Analytics. É possível escrever qualquer consulta personalizada para alertas adicionais, de forma a cobrir muitos cenários diferentes. No portal do Azure, navegue até **Monitor** e clique em **Criar Alerta**. Esta ação abre a página **Criar regra**.

Em **1. Definir condição do alerta**, clique em **+ Selecionar destino**. Em **Filtrar por tipo de recurso**, selecione **Log Analytics**. Escolha a sua área de trabalho do Log Analytics e clique em **Concluído**.

![criar alerta](./media/automation-tutorial-update-management/create-alert.png)

Clique no botão **+ Adicionar critérios** para abrir a página **Configurar lógica de sinal**. Escolha **Pesquisa de registos personalizados** na tabela. Introduza a seguinte consulta na caixa de texto **Consulta de pesquisa**. Esta consulta devolve os computadores e o nome da atualização executada que foi concluída no período de tempo especificado.

```loganalytics
UpdateRunProgress
| where InstallationStatus == 'Succeeded'
| where TimeGenerated > now(-10m)
| summarize by UpdateRunName, Computer
```

Introduza **1** como o **Limiar** para a Lógica de alerta. Quando terminar, clique em **Concluído**.

![Configurar lógica de sinal](./media/automation-tutorial-update-management/signal-logic.png)

Em **2. Definir detalhes do alerta**, atribua ao alerta um nome amigável e uma descrição. Defina a **Gravidade** como **Informativo (Gravidade 2)**, uma vez que o alerta é para uma execução bem sucedida.

![Configurar lógica de sinal](./media/automation-tutorial-update-management/define-alert-details.png)

Em **3. Definir grupo de ação**, clique em **+ Novo grupo de ação**. Um grupo de ação é um grupo de ações que podem ser utilizadas em vários alertas. Podem incluir, mas não estão limitados a notificações por e-mail, runbooks, webhooks e muitas mais. Para saber mais sobre grupos de ação, veja [Criar e gerir grupos de ação](../monitoring-and-diagnostics/monitoring-action-groups.md)

Na caixa **Nome do grupo de ação**, atribua um nome amigável e um nome abreviado ao grupo de ação. O nome abreviado é utilizado em vez de um nome de grupo de ação completo quando as notificações são enviadas através deste grupo.

Em **Ações**, atribua à ação um nome amigável como **Notificações por E-Mail** e, em **TIPO DE AÇÃO**, selecione **E-mail/SMS/Push/Voz**. Em **DETALHES**, selecione **Editar detalhes**.

Na página **E-mail/SMS/Push/Voz**, atribua um nome. Assinale a caixa **E-mail** e introduza um endereço de e-mail válido a utilizar.

![Configurar o grupo de ação de e-mail](./media/automation-tutorial-update-management/configure-email-action-group.png)

Clique em **OK** na página **E-mail/SMS/Push/Voz** para fechá-la e clique em **OK** para fechar a página **Adicionar grupo de ação**.

Pode personalizar o assunto do e-mail enviado ao clicar em **Assunto do e-mail**, em **Personalizar Ações** na página **Criar regra**. Quando terminar, clique em **Criar regra de alerta**. Esta ação cria a regra que o alerta quando a implementação de uma atualização for concluída com êxito e que máquinas faziam parte da execução dessa implementação de atualização.

## <a name="schedule-an-update-deployment"></a>Agendar uma implementação de atualizações

Agora que os alertas estão configurados, agende uma implementação que siga o agendamento e o período de administração da sua versão para instalar atualizações.
Pode escolher quais os tipos de atualização a incluir na implementação.
Por exemplo, pode incluir atualizações de segurança ou críticas e excluir update rollups.

> [!WARNING]
> Quando as atualizações requerem um reinício, a VM é reiniciada automaticamente.

Para agendar uma nova implementação de atualização para a VM, navegue de volta até **Gestão de atualizações** e selecione **Agendar a implementação da atualização** na parte superior do ecrã.

No ecrã **Nova implementação de atualização**, especifique as seguintes informações:

* **Nome** - Indique um nome exclusivo para a implementação da atualização.

* **Sistema operativo** - Escolha o SO de destino para a implementação da atualização.

* **Classificação da atualização** - Selecione os tipos de software que a implementação da atualização incluiu na implementação. Para este tutorial, deixe todos os tipos selecionados.

  Os tipos de classificação são:

   |SO  |Tipo  |
   |---------|---------|
   |Windows     | Atualizações críticas</br>Atualizações de segurança</br>Update rollups</br>Pacotes de funcionalidades</br>Service packs</br>Atualizações de definições</br>Ferramentas</br>Atualizações        |
   |Linux     | Atualizações críticas e de segurança</br>Outras atualizações       |

   Para obter uma descrição dos tipos de classificação, veja [Classificações de atualizações](automation-update-management.md#update-classifications).

* **Definições da agenda** - Esta ação abre a página Definições da Agenda. A hora de início predefinida é 30 minutos depois da hora atual. É possível definir qualquer hora a partir de 10 minutos no futuro.

   Também pode especificar se a implementação ocorre uma vez ou configurar um agendamento periódico.
   Selecione **Uma vez** em **Periodicidade**. Deixe a predefinição em 1 dia e clique em **OK**. Esta ação configura uma agenda periódica.

* **Janela de manutenção (minutos)** - Deixe este valor com o valor predefinido. Pode especificar o período de tempo no qual pretende que a implementação da atualização ocorra. Esta definição ajuda a garantir que as alterações são realizadas nos seus períodos de administração definidos.

![Ecrã de Definições de Agendamento de Atualizações](./media/automation-tutorial-update-management/manageupdates-schedule-win.png)

Depois de concluir a configuração da agenda, clique no botão **Criar**. Volta ao dashboard de estado. Selecione **Implementações de atualização agendadas** para mostrar a agenda de implementação que criou.

## <a name="view-results-of-an-update-deployment"></a>Ver resultados de uma implementação de atualização

Após o início da implementação agendada, pode ver o estado dessa implementação no separador **Implementações de atualização** no ecrã **Gestão de atualizações**.
O estado é apresentado como **Em curso** quando está em execução.
Depois de concluir, se for bem sucedida, muda para **Com êxito**.
Quando existirem falhas numa ou mais atualizações na implementação, o estado é **Falha parcial**.
Clique na implementação da atualização concluída para ver o dashboard relativo a essa implementação de atualização.

![Dashboard de estado de Implementação de Atualização para uma implementação específica](./media/automation-tutorial-update-management/manageupdates-view-results.png)

No mosaico **Resultados da atualização**, um resumo indica o número total de atualizações e os resultados de implementação da VM.
A tabela à direita mostra uma divisão detalhada de cada atualização e os resultados da instalação.
A lista seguinte mostra os valores disponíveis:

* **Não tentado** - a atualização não foi instalada porque não havia tempo suficiente disponível com base na duração da janela de manutenção definida.
* **Com êxito** - a atualização foi executada com êxito.
* **Falhou** - a atualização falhou.

Clique em **Todos os registos** para ver todas as entradas de registo que a implementação criou.

Clique no mosaico **Saída** para ver o fluxo de tarefas do runbook responsável pela gestão da implementação da atualização na VM de destino.

Clique em **Erros** para ver informações detalhadas sobre os erros da implementação.

Assim que a implementação da atualização for concluída com êxito, é enviado um e-mail semelhante à imagem seguinte para indicar o êxito da implementação.

![Configurar o grupo de ação de e-mail](./media/automation-tutorial-update-management/email-notification.png)

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Carregar uma VM para gestão de atualizações
> * Ver avaliações de atualizações
> * Configurar alertas
> * Agendar uma implementação de atualizações
> * Ver os resultados de uma implementação

Avance para a descrição geral da solução de Gestão de Atualizações.

> [!div class="nextstepaction"]
> [Solução de Gestão de Atualizações](../operations-management-suite/oms-solution-update-management.md?toc=%2fazure%2fautomation%2ftoc.json)

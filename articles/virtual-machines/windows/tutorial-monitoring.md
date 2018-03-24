---
title: Monitorização do Azure e a atualização e máquinas virtuais do Windows | Microsoft Docs
description: Tutorial - monitorizar e atualizar uma Máquina Virtual do Windows com o Azure PowerShell
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/04/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 94151008f0aba6020786e65c60cec66285f310c4
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="monitor-and-update-a-windows-virtual-machine-with-azure-powershell"></a>Monitorizar e atualizar a Máquina Virtual do Windows com o Azure PowerShell

Monitorização do Azure utiliza o agentes para recolher dados de arranque e de desempenho de VMs do Azure, armazenar estes dados no armazenamento do Azure e tornam acessível através do portal, o módulo Azure PowerShell e a CLI do Azure. Gestão de atualizações permite-lhe gerir as atualizações e correções de erros para as suas VMs do Windows Azure.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Ativar o diagnóstico de arranque numa VM
> * Ver diagnósticos de arranque
> * Veja as métricas anfitrião VM
> * Instalar a extensão de diagnóstico
> * Ver métricas da VM
> * Criar um alerta
> * Gerir atualizações do Windows
> * Configurar a monitorização avançada

Este tutorial requer a versão do módulo 3.6 ou posterior do Azure PowerShell. Executar ` Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps).

Para concluir o exemplo neste tutorial, tem de ter uma máquina virtual existente. Se for preciso, este [script de exemplo](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) pode criar um para si. Ao trabalhar através do tutorial, substitua o grupo de recursos, o nome da VM e a localização em que o necessário.

## <a name="view-boot-diagnostics"></a>Ver diagnósticos de arranque

Como máquinas virtuais do Windows efetuar o arranque, o agente de diagnóstico de arranque captura a saída de ecrã que pode ser utilizada para fins de resolução de problemas. Esta capacidade está ativada por predefinição. As capturas de ecrã capturadas estão armazenadas numa conta de armazenamento do Azure, que também é criada por predefinição. 

Pode obter os dados de diagnóstico de arranque com o [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) comando. No exemplo seguinte, diagnóstico de arranque é transferido para a raiz do * c:\* unidade. 

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>Ver métricas de anfitrião

Uma VM do Windows tem uma VM de anfitrião dedicada no Azure que interage com. As métricas são automaticamente recolhidas para o anfitrião e podem ser visualizadas no portal do Azure.

1. No portal do Azure, clique em **Grupos de Recursos**, selecione **myResourceGroup** e, em seguida, selecione **myVM** na lista de recursos.
2. Clique em **métricas** no painel de VM e, em seguida, selecione qualquer uma das métricas de anfitrião em **as métricas disponíveis** para ver como a VM do anfitrião está a efetuar.

    ![Ver métricas de anfitrião](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>Instalar a extensão de diagnóstico

As métricas de anfitrião básica estão disponíveis, mas mais granular e métricas específicas de VM, que tem de instalar a extensão de diagnóstico do Azure na VM. A extensão de diagnóstico do Azure permite que sejam obtidos dados de monitorização e diagnóstico adicionais da VM. Pode ver estas métricas de desempenho e criar alertas com base no desempenho da VM. A extensão de diagnóstico é instalada através do portal do Azure da seguinte forma:

1. No portal do Azure, clique em **Grupos de Recursos**, selecione **myResourceGroup** e, em seguida, selecione **myVM** na lista de recursos.
2. Clique em **Definições de diagnóstico**. A lista mostra que os *Diagnósticos de arranque* já foram ativados na secção anterior. Clique na caixa de verificação para *Métricas básicas*.
3. Clique em de **ativar a monitorização de ao nível do convidado** botão.

    ![Ver métricas de diagnóstico](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>Ver métricas da VM

Pode ver as métricas da VM da mesma forma que vê as métricas da VM de anfitrião:

1. No portal do Azure, clique em **Grupos de Recursos**, selecione **myResourceGroup** e, em seguida, selecione **myVM** na lista de recursos.
2. Para ver o desempenho da VM, clique em **Métricas** no painel da VM e, em seguida, selecione qualquer uma das métricas de diagnóstico em **Métricas disponíveis**.

    ![Ver métricas da VM](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a>Criar alertas

Pode criar alertas com base em métricas de desempenho específicas. Podem ser utilizados alertas para notificá-lo quando a utilização média da CPU excede um determinado limiar ou o espaço livre em disco disponível diminuir até uma determinada quantidade, por exemplo. Os alertas são apresentados no portal do Azure ou podem ser enviados por e-mail. Também pode acionar runbooks da Automatização do Azure ou do Azure Logic Apps em resposta aos alertas gerados.

O exemplo seguinte cria um alerta para a utilização média da CPU.

1. No portal do Azure, clique em **Grupos de Recursos**, selecione **myResourceGroup** e, em seguida, selecione **myVM** na lista de recursos.
2. Clique em **Regras de alerta** no painel da VM e, em seguida, clique em **Adicionar alerta de métrica** na parte superior do painel de alertas.
4. Forneça um **Nome** para o alerta, como *myAlertRule*
5. Para acionar um alerta quando a percentagem da CPU excede 1.0 para cinco minutos, mantenha todas as outras predefinições selecionadas.
6. Opcionalmente, selecione a caixa para *Proprietários, contribuidores e leitores do e-mail* para enviar uma notificação por e-mail. A ação predefinida é apresentar uma notificação no portal.
7. Clique no botão **OK**.

## <a name="manage-windows-updates"></a>Gerir atualizações do Windows

Gestão de atualizações permite-lhe gerir as atualizações e correções de erros para as suas VMs do Windows Azure.
Diretamente a partir da VM, pode avaliar rapidamente o estado das atualizações disponíveis, agendar a instalação de atualizações necessárias e rever os resultados de implementação para verificar se as atualizações foram aplicadas com êxito à VM.

Para obter informações sobre preços, veja [Preços de Automatização para Gestão de atualizações](https://azure.microsoft.com/pricing/details/automation/)

### <a name="enable-update-management"></a>Ativar a Gestão de atualizações

Ative a gestão de atualização para a VM:
 
1. No lado esquerdo do ecrã, selecione **Máquinas virtuais**.
2. Na lista, selecione uma VM.
3. No ecrã da VM, na secção **Operações**, clique em **Gestão de atualizações**. É aberto o ecrã **Ativar Gestão de Atualizações**.

A validação é executada para determinar se a Gestão de atualizações está ativada para esta VM. A validação inclui a verificação da existência de uma área de trabalho do Log Analytics e da conta de Automatização ligada, e se a solução está na área de trabalho.

A área de trabalho do Log Analytics serve para recolher dados gerados pelas funcionalidades e serviços, tais como a Gestão de atualizações. A área de trabalho fornece uma localização única para rever e analisar dados de várias origens. Para executar ações adicionais em VMs que necessitam de atualizações, a Automatização do Azure permite executar scripts nas VMs, tais como transferir e aplicar atualizações.

O processo de validação também verifica se a VM está aprovisionada com o Microsoft Monitoring Agent (MMA) e a função de trabalho híbrida. Este agente serve para comunicar com a VM e obter informações sobre o estado de atualização. 

Se estes pré-requisitos não forem cumpridos, é apresentada uma faixa que lhe dá a opção de ativar a solução.

![Faixa de configuração de carregamento da Gestão de Atualizações](./media/tutorial-monitoring/manageupdates-onboard-solution-banner.png)

Clique na faixa para ativar a solução. Se for detetada a falta de qualquer um dos pré-requisitos que se seguem após a validação, estes serão adicionados automaticamente:

* Área de trabalho do [Log Analytics](../../log-analytics/log-analytics-overview.md)
* [Automatização](../../automation/automation-offering-get-started.md)
* Uma [Função de trabalho de runbook híbrida](../../automation/automation-hybrid-runbook-worker.md) está ativada na VM

É aberto o ecrã **Ativar Gestão de Atualizações**. Configure as definições e clique em **Ativar**.

![Ativar a solução de Gestão de atualizações](./media/tutorial-monitoring/manageupdates-update-enable.png)

Ativar a solução pode demorar até 15 minutos e, durante este tempo, não deve fechar a janela do browser. Após a ativação da solução, as informações sobre atualizações em falta na VM são transmitidas ao Log Analytics.
Pode demorar entre 30 minutos e 6 horas até que os dados fiquem disponíveis para análise.

### <a name="view-update-assessment"></a>Ver avaliação de atualizações

Depois de **Gestão de atualizações** ser ativada, o ecrã **Gestão de atualizações** aparece. Pode ver uma lista de atualizações em falta no separador **Atualizações em falta**.

 ![Ver o estado de atualização](./media/tutorial-monitoring/manageupdates-view-status-win.png)

### <a name="schedule-an-update-deployment"></a>Agendar uma implementação de atualizações

Para instalar atualizações, agende uma implementação que siga o seu agendamento e o período de administração da versão.
Pode escolher quais os tipos de atualização a incluir na implementação. Por exemplo, pode incluir atualizações de segurança ou críticas e excluir update rollups.

Para agendar uma nova Implementação de Atualização para a VM, clique em **Agendar a implementação da atualização** na parte superior do ecrã **Gestão de atualizações**. No ecrã **Nova implementação de atualização**, especifique as seguintes informações:

* **Nome** - Indique um nome exclusivo para identificar a implementação de atualizações.
* **Classificação da atualização** - Selecione os tipos de software que a implementação da atualização incluiu na implementação. Os tipos de classificação são:
  * Atualizações críticas
  * Atualizações de segurança
  * Update rollups
  * Pacotes de funcionalidades
  * Service packs
  * Atualizações de definições
  * Ferramentas
  * Atualizações

* **Definições da agenda** - Pode aceitar a data e hora predefinidas, que é 30 minutos após a hora atual, ou especificar uma hora diferente.
  Também pode especificar se a implementação ocorre uma vez ou configurar um agendamento periódico. Clique na opção Periódico, em Periodicidade, para configurar um agendamento periódico.

  ![Ecrã de Definições de Agendamento de Atualizações](./media/tutorial-monitoring/manageupdates-schedule-win.png)

* **Janela de manutenção (minutos)** - Especifique o período de tempo no qual pretende que a implementação da atualização ocorra.  Isto ajuda a garantir que as alterações são realizadas nos seus períodos de administração definidos.

Depois de concluir a configuração do agendamento, clique no botão **Criar** e regressará ao dashboard de estado.
Tenha em atenção que a tabela **Agendada** mostra o agendamento da implementação que criou.

> [!WARNING]
> Para as atualizações que requerem um reinício, a VM é reiniciada automaticamente.

### <a name="view-results-of-an-update-deployment"></a>Ver resultados de uma implementação de atualização

Após o início da implementação agendada, pode ver o estado dessa implementação no separador **Implementações de atualizações** no ecrã **Gestão de atualizações**.
Se estiver em execução, o respetivo estado é apresentado como **Em curso**. Depois de concluir, se for bem sucedida, muda para **Com êxito**.
Se existir uma falha numa ou mais atualizações na implementação, o estado é **Falha parcial**.
Clique na implementação da atualização concluída para ver o dashboard relativo a essa implementação de atualização.

   ![Dashboard de estado de Implementação de Atualização para uma implementação específica](./media/tutorial-monitoring/manageupdates-view-results.png)

No mosaico **Resultados da atualização** encontra-se um resumo do número total de atualizações e os resultados de implementação da VM.
Na tabela à direita encontra-se uma divisão detalhada de cada atualização e os resultados da instalação, que podem ter um dos seguintes valores:

* **Não tentado** - a atualização não foi instalada porque não havia tempo suficiente disponível com base na duração da janela de manutenção definida.
* **Com êxito** - a atualização foi executada com êxito.
* **Falhou** - a atualização falhou.

Clique em **Todos os registos** para ver todas as entradas de registo que a implementação criou.

Clique no mosaico **Saída** para ver o fluxo de tarefas do runbook responsável pela gestão da implementação da atualização na VM de destino.

Clique em **Erros** para ver informações detalhadas sobre os erros da implementação.

## <a name="advanced-monitoring"></a>Monitorização avançada 

Pode efetuar uma monitorização mais avançada da VM através do [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). Se ainda não o tiver feito, pode inscrever-se numa [avaliação gratuita](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) do Operations Management Suite.

Quando tiver acesso ao portal do OMS, pode encontrar a chave e o identificador da área de trabalho no painel Definições. Utilize o [conjunto AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) comando para adicionar a extensão do OMS à VM. Atualizar os valores das variáveis no abaixo exemplo para refletir a chave de área de trabalho do OMS e área de trabalho ID.  

```powershell
$omsId = "<Replace with your OMS Id>"
$omsKey = "<Replace with your OMS key>"

Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
  -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
  -VMName myVM `
  -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
  -ExtensionType "MicrosoftMonitoringAgent" `
  -TypeHandlerVersion 1.0 `
  -Settings @{"workspaceId" = $omsId} `
  -ProtectedSettings @{"workspaceKey" = $omsKey} `
  -Location eastus
```

Após alguns minutos, deverá ver a nova VM, na área de trabalho OMS. 

![Painel do OMS](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, configurou e rever as VMs com o Centro de segurança do Azure. Aprendeu a:

> [!div class="checklist"]
> * Criar uma rede virtual
> * Criar um grupo de recursos e a VM 
> * Ativar diagnósticos de arranque na VM
> * Ver diagnósticos de arranque
> * Ver métricas de anfitrião
> * Instalar a extensão de diagnóstico
> * Ver métricas da VM
> * Criar um alerta
> * Gerir atualizações do Windows
> * Configurar a monitorização avançada

Avançar para o próximo tutorial para saber mais acerca do Centro de segurança do Azure.

> [!div class="nextstepaction"]
> [Gerir a segurança de VMs](./tutorial-azure-security.md)
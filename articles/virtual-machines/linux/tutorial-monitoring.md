---
title: "Monitorizar e atualizar máquinas virtuais do Linux no Azure | Microsoft Docs"
description: "Saiba como monitorizar diagnósticos de arranque e métricas de desempenho, e gerir atualizações de pacotes numa máquina virtual do Linux no Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 230ce6a6b33e63bcced5f520b57b63ef4ed05448
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="how-to-monitor-and-update-a-linux-virtual-machine-in-azure"></a>Como monitorizar e atualizar uma máquina virtual do Linux no Azure

Para garantir que as máquinas virtuais (VMs) no Azure estão a ser executadas corretamente, pode consultar diagnósticos de arranque e métricas de desempenho, bem como gerir as atualizações de pacotes. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Ativar diagnósticos de arranque na VM
> * Ver diagnósticos de arranque
> * Ver métricas de anfitrião
> * Ativar a extensão de diagnóstico na VM
> * Ver métricas da VM
> * Criar alertas com base nas métricas de diagnóstico
> * Gerir atualizações de pacotes
> * Configurar a monitorização avançada


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este tutorial requer a execução da versão 2.0.4 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-vm"></a>Criar VM

Para ver os diagnósticos e as métricas em ação, precisa de uma VM. Primeiro, crie um grupo de recursos com [az group create](/cli/azure/group#az_group_create). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroupMonitor* na localização *eastus*.

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

Agora, crie uma VM com [az vm create](https://docs.microsoft.com/cli/azure/vm#az_vm_create). O exemplo seguinte cria uma VM com o nome *myVM*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a>Ativar diagnósticos de arranque

À medida que as VMs do Linux arrancam, a extensão de diagnóstico de arranque captura a saída de arranque e guarda-a no armazenamento do Azure. Estes dados podem ser utilizados para resolver problemas de arranque das VMs. Os diagnósticos de arranque não são ativados automaticamente ao criar uma VM do Linux através da CLI do Azure.

Antes de ativar os diagnósticos de arranque, tem de ser criada uma conta de armazenamento para guardar os registos de arranque. As contas de armazenamento têm de ter um nome globalmente exclusivo, ter entre 3 e 24 carateres e conter apenas números e letras minúsculas. Crie uma conta de armazenamento com o comando [az storage account create](/cli/azure/storage/account#az_storage_account_create). Neste exemplo, é utilizada uma cadeia aleatória para criar um nome de conta de armazenamento exclusivo. 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

Ao ativar os diagnósticos de arranque, é necessário o URI para o contentor de armazenamento de blobs. O comando seguinte consulta a conta de armazenamento para devolver este URI. O valor do URI é armazenado no nome de variável *bloburi*, utilizado no próximo passo.

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

Ative agora os diagnósticos de arranque com [az vm boot-diagnostics enable](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_enable). O valor `--storage` é o URI do blob recolhido no passo anterior.

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a>Ver diagnósticos de arranque

Quando os diagnósticos de arranque estão ativados, sempre que parar e iniciar a VM, as informações sobre o processo de arranque são escritas num ficheiro de registo. Neste exemplo, desaloque primeiro a VM com o comando [az vm deallocate](/cli/azure/vm#az_vm_deallocate) da seguinte forma:

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Agora, inicie a VM com o comando [az vm start]( /cli/azure/vm#az_vm_stop) da seguinte forma:

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

Pode obter os dados de diagnósticos de arranque para *myVM* com o comando [az vm boot-diagnostics get-boot-log](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_get_boot_log) da seguinte forma:

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a>Ver métricas de anfitrião

Uma VM do Linux tem um anfitrião dedicado no Azure com o qual interage. As métricas são recolhidas automaticamente para o anfitrião e podem ser visualizadas no portal do Azure da seguinte forma:

1. No portal do Azure, clique em **Grupos de Recursos**, selecione **myResourceGroupMonitor** e, em seguida, selecione **myVM** na lista de recursos.
1. Para ver o desempenho da VM de anfitrião, clique em **Métricas** no painel da VM e, em seguida, selecione qualquer uma das métricas *[Anfitrião]* em **Métricas disponíveis**.

    ![Ver métricas de anfitrião](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a>Instalar a extensão de diagnóstico

> [!IMPORTANT]
> Este documento descreve a versão 2.3 da Extensão de Diagnóstico do Linux, que foi preterida. A versão 2.3 será suportada até 30 de junho de 2018.
>
> Pode ativar a versão 3.0 da Extensão de Diagnóstico do Linux. Para obter mais informações, veja [a documentação ](./diagnostic-extension.md).

As métricas de anfitrião básicas estão disponíveis, mas para ver métricas mais granulares e específicas da VM, tem de instalar a extensão de diagnóstico do Azure na VM. A extensão de diagnóstico do Azure permite que sejam obtidos dados de monitorização e diagnóstico adicionais da VM. Pode ver estas métricas de desempenho e criar alertas com base no desempenho da VM. A extensão de diagnóstico é instalada através do portal do Azure da seguinte forma:

1. No portal do Azure, clique em **Grupos de Recursos**, selecione **myResourceGroup** e, em seguida, selecione **myVM** na lista de recursos.
1. Clique em **Definições de diagnóstico**. A lista mostra que os *Diagnósticos de arranque* já foram ativados na secção anterior. Clique na caixa de verificação para *Métricas básicas*.
1. Na secção *Conta de armazenamento*, procure e selecione a conta *mydiagdata[1234]* criada na secção anterior.
1. Clique no botão **Guardar**.

    ![Ver métricas de diagnóstico](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a>Ver métricas da VM

Pode ver as métricas da VM da mesma forma que vê as métricas da VM de anfitrião:

1. No portal do Azure, clique em **Grupos de Recursos**, selecione **myResourceGroup** e, em seguida, selecione **myVM** na lista de recursos.
1. Para ver o desempenho da VM, clique em **Métricas** no painel da VM e, em seguida, selecione qualquer uma das métricas de diagnóstico em **Métricas disponíveis**.

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

## <a name="manage-package-updates"></a>Gerir atualizações de pacotes

Através da Gestão de atualizações, pode gerir as atualizações de pacotes e as correções para as VMs do Linux do Azure. Diretamente a partir da VM, pode avaliar rapidamente o estado das atualizações disponíveis, agendar a instalação de atualizações necessárias e rever os resultados de implementação para verificar se as atualizações foram aplicadas com êxito à VM.

Para obter informações sobre preços, veja [Preços de Automatização para Gestão de atualizações](https://azure.microsoft.com/pricing/details/automation/)

### <a name="enable-update-management-preview"></a>Ativar a Gestão de atualizações (Pré-visualização)

Ativar a Gestão de atualizações para a VM

1. No lado esquerdo do ecrã, selecione **Máquinas virtuais**.
1. Na lista, selecione uma VM.
1. No ecrã da VM, na secção **Operações**, clique em **Gestão de atualizações**. É aberto o ecrã **Ativar Gestão de atualizações**.

A validação é executada para determinar se a Gestão de atualizações está ativada para esta VM. A validação inclui a verificação da existência de uma área de trabalho do Log Analytics e da conta de Automatização ligada, e se a solução está na área de trabalho.

A área de trabalho do Log Analytics serve para recolher dados gerados pelas funcionalidades e serviços, tais como a Gestão de atualizações. A área de trabalho fornece uma localização única para rever e analisar dados de várias origens. Para executar ações adicionais em VMs que necessitam de atualizações, a Automatização do Azure permite executar scripts nas VMs, tais como transferir e aplicar atualizações.

O processo de validação também verifica se a VM está aprovisionada com o Microsoft Monitoring Agent (MMA) e a função de trabalho híbrida. Este agente serve para comunicar com a VM e obter informações sobre o estado de atualização. 

Se estes pré-requisitos não forem cumpridos, é apresentada uma faixa que lhe dá a opção de ativar a solução.

![Faixa de configuração de carregamento da Gestão de Atualizações](./media/tutorial-monitoring/manage-updates-onboard-solution-banner.png)

Clique na faixa para ativar a solução. Se for detetada a falta de qualquer um dos pré-requisitos que se seguem após a validação, estes serão adicionados automaticamente:

* Área de trabalho do [Log Analytics](../../log-analytics/log-analytics-overview.md)
* [Automatização](../../automation/automation-offering-get-started.md)
* Uma [Função de trabalho de runbook híbrida](../../automation/automation-hybrid-runbook-worker.md) está ativada na VM

É aberto o ecrã **Ativar Gestão de Atualizações**. Configure as definições e clique em **Ativar**.

![Ativar a solução de Gestão de atualizações](./media/tutorial-monitoring/manage-updates-update-enable.png)

Ativar a solução pode demorar até 15 minutos e, durante este tempo, não deve fechar a janela do browser. Após a ativação da solução, as informações sobre atualizações em falta no gestor de pacotes na VM são transmitidas ao Log Analytics.
Pode demorar entre 30 minutos e 6 horas até que os dados fiquem disponíveis para análise.

### <a name="view-update-assessment"></a>Ver avaliação de atualizações

Após a ativação da **Gestão de atualizações**, é apresentado o ecrã **Gestão de atualizações**. Pode ver uma lista de atualizações em falta no separador **Atualizações em falta**.

![Ver o estado de atualização](./media/tutorial-monitoring/manage-updates-view-status-linux.png)

### <a name="schedule-an-update-deployment"></a>Agendar uma implementação de atualizações

Para instalar atualizações, agende uma implementação que siga o seu agendamento e a janela de manutenção da versão.

Para agendar uma nova Implementação de Atualização para a VM, clique em **Agendar a implementação da atualização** na parte superior do ecrã **Gestão de atualizações**. No ecrã **Nova implementação de atualização**, especifique as seguintes informações:

* **Nome** - Indique um nome exclusivo para identificar a implementação de atualizações.
* **Atualizações a excluir** – selecione esta opção para introduzir os nomes de pacotes a excluir da atualização.
* **Definições da agenda** - Pode aceitar a data e hora predefinidas, que é 30 minutos após a hora atual, ou especificar uma hora diferente. Também pode especificar se a implementação ocorre uma vez ou configurar um agendamento periódico. Clique na opção Periódico, em Periodicidade, para configurar um agendamento periódico.

  ![Ecrã de Definições de Agendamento de Atualizações](./media/tutorial-monitoring/manage-updates-schedule-linux.png)

* **Janela de manutenção (minutos)** - Especifique o período de tempo no qual pretende que a implementação da atualização ocorra.  Isto ajuda a garantir que as alterações são realizadas nas janelas de manutenção definidas. 

Depois de concluir a configuração do agendamento, clique no botão **Criar** e regressará ao dashboard de estado.
Tenha em atenção que a tabela **Agendada** mostra o agendamento da implementação que criou.

> [!WARNING]
> A VM será reiniciada automaticamente após as atualizações serem instaladas se houver tempo suficiente na janela de manutenção.

A Gestão de atualizações utiliza o gestor de pacotes existente na VM para instalar pacotes.

### <a name="view-results-of-an-update-deployment"></a>Ver resultados de uma implementação de atualização

Após o início da implementação agendada, pode ver o estado dessa implementação no separador **Implementações de atualizações** no ecrã **Gestão de atualizações**.
Se estiver em execução, o respetivo estado é apresentado como **Em curso**. Depois de concluir, se for bem sucedida, muda para **Com êxito**.
Se existir uma falha numa ou mais atualizações na implementação, o estado é **Falha**.
Clique na implementação da atualização concluída para ver o dashboard relativo a essa implementação de atualização.

![Dashboard de estado de Implementação de Atualização para uma implementação específica](./media/tutorial-monitoring/manage-updates-view-results.png)

No mosaico **Resultados da atualização** encontra-se um resumo do número total de atualizações e os resultados de implementação da VM.
Na tabela à direita encontra-se uma divisão detalhada de cada atualização e os resultados da instalação, que podem ter um dos seguintes valores:

* **Não tentado** - a atualização não foi instalada porque não havia tempo suficiente disponível com base na duração da janela de manutenção definida.
* **Com êxito** – a atualização foi transferida e instalada com êxito na VM
* **Falhou** – falha ao transferir ou instalar a atualização na VM.

Clique em **Todos os registos** para ver todas as entradas de registo que a implementação criou.

Clique no mosaico **Saída** para ver o fluxo de tarefas do runbook responsável pela gestão da implementação da atualização na VM de destino.

Clique em **Erros** para ver informações detalhadas sobre os erros da implementação.

## <a name="advanced-monitoring"></a>Monitorização avançada 

Pode efetuar uma monitorização mais avançada da VM através do [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). Se ainda não o tiver feito, pode inscrever-se numa [avaliação gratuita](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) do Operations Management Suite.

Quando tiver acesso ao portal do OMS, pode encontrar a chave e o identificador da área de trabalho no painel Definições. Substitua <workspace-key> e <workspace-id> pelos valores da área de trabalho do OMS e, em seguida, pode utilizar **az vm extension set** para adicionar a extensão do OMS à VM:

```azurecli-interactive 
az vm extension set \
  --resource-group myResourceGroupMonitor \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.3 \
  --protected-settings '{"workspaceKey": "<workspace-key>"}' \
  --settings '{"workspaceId": "<workspace-id>"}'
```

No painel Pesquisa de Registos do portal do OMS, deverá ver *myVM*, tal como o que é mostrado na imagem seguinte:

![Painel do OMS](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, configurou, reviu e geriu atualizações para uma VM. Aprendeu a:

> [!div class="checklist"]
> * Ativar diagnósticos de arranque na VM
> * Ver diagnósticos de arranque
> * Ver métricas de anfitrião
> * Ativar a extensão de diagnóstico na VM
> * Ver métricas da VM
> * Criar alertas com base nas métricas de diagnóstico
> * Gerir atualizações de pacotes
> * Configurar a monitorização avançada

Avance para o próximo tutorial para saber mais sobre o Centro de Segurança do Azure.

> [!div class="nextstepaction"]
> [Gerir a segurança de VMs](./tutorial-azure-security.md)

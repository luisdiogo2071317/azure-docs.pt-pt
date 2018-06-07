---
title: Executar scripts de shell uma VM com Linux no Azure
description: Este tópico descreve como executar scripts dentro de uma máquina virtual de Linux do Azure utilizando o comando executado
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 05/02/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 6f21452ddc6c8a48392d24615e8dbcbba8b996c8
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661122"
---
# <a name="run-shell-scripts-in-your-linux-vm-with-run-command"></a>Executar scripts de shell na VM com Linux com o comando executado

Execute comandos permite-lhe executar scripts de shell dentro de uma VM com Linux do Azure, independentemente da conectividade de rede. Estes scripts podem ser utilizados para a máquina geral ou de gestão de aplicações e podem ser utilizados para diagnosticar e resolver problemas de acesso e da rede VM e rapidamente obter a VM para um bom estado.

## <a name="benefits"></a>Benefícios

Existem várias opções que podem ser utilizadas para aceder às suas máquinas virtuais. Pode executar scripts em máquinas virtuais, independentemente da conectividade de rede do comando de execução e está disponível por predefinição (sem instalação necessária). Execução do comando pode ser utilizado através do portal do Azure, [REST API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [CLI do Azure](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke), ou [PowerShell](/powershell/module/azurerm.compute/invoke-azurermvmruncommand).

Esta capacidade é útil em todos os cenários em que pretende executar um script witin uma máquinas virtuais e é uma das formas apenas para resolver problemas e remediar uma máquina virtual que não está ligada à rede devido a rede incorrecto ou utilizador administrativo configuração.

## <a name="restrictions"></a>Restrições

Seguem-se uma lista de restrições que estão presentes quando executar o comando.

* O resultado é limitado para a última 4096 bytes
* O tempo mínimo para executar um script cerca de 20 segundos
* Scripts são executados como utilizador elevados no Linux
* Pode executar um script num momento
* Não é possível cancelar um script em execução
* O tempo máximo que pode executar um script é 90 minutos, após o qual as TI será o tempo limite

## <a name="azure-cli"></a>CLI do Azure

Segue-se um exemplo utilizando o [az vm executar comando-](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) comando a executar um script de shell na VM com Linux do Azure.

```azurecli-interactive
az vm run-command invoke -g myResourceGroup -n myVm --command-id RunShellScript --scripts "sudo apt-get update && sudo apt-get install -y nginx"
```

## <a name="azure-portal"></a>Portal do Azure

Navegue para uma VM no [Azure](https://portal.azure.com) e selecione **executar comando** em **operações**. É-lhe apresentada uma lista dos comandos disponíveis para executar na VM.

![Executar a lista de comandos](./media/run-command/run-command-list.png)

Escolha um comando a executar. Alguns dos comandos podem ter parâmetros de entrada necessários ou opcionais. Para esses comandos os parâmetros são apresentados como campos de texto para lhe fornecer os valores de entrada. Para cada comando pode ver o script que está a ser executado, expandindo **Ver script**. **RunPowerShellScript** é diferente dos outros comandos, como permite-lhe fornecer os seus próprios scripts personalizados. 

> [!NOTE]
> Os comandos incorporados não são editáveis.

Depois do comando é escolhido, clique em **executar** para executar o script. O script é executado e concluído, devolve o resultado e quaisquer erros na janela de saída. A seguinte captura de ecrã mostra um exemplo da saída da execução de **ifconfig** comando.

![Execute a saída do comando de script](./media/run-command/run-command-script-output.png)

## <a name="available-commands"></a>Comandos disponíveis

Esta tabela mostra a lista de comandos disponíveis para VMs com Linux. O **RunShellScript** comando pode ser utilizado para executar qualquer script personalizado que pretende.

|**Nome**|**Descrição**|
|---|---|
|**RunShellScript**|Executa um script de shell do Linux.|
|**ifconfig**| Obter a configuração de todas as interfaces de rede.|

## <a name="limiting-access-to-run-command"></a>Limitar o acesso ao executar o comando

Listar os comandos de execução ou que mostra os detalhes de um comando requerem o `Microsoft.Compute/locations/runCommands/read` permissão, que incorporada [leitor](../../role-based-access-control/built-in-roles.md#reader) função e superior.

Executar um comando exige o `Microsoft.Compute/virtualMachines/runCommand/action` permissão, que o [contribuinte](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) função e superior.

Pode utilizar um do [incorporada](../../role-based-access-control/built-in-roles.md) funções ou crie um [personalizado](../../role-based-access-control/custom-roles.md) função para utilizar a executar o comando.

## <a name="next-steps"></a>Passos Seguintes

Ver, [executem scripts na sua VM com Linux](run-scripts-in-vm.md) para saber mais sobre outras formas de executar scripts e comandos remotamente a VM.

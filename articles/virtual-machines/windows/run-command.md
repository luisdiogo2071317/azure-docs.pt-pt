---
title: Executar PowerShell scripts numa VM Windows no Azure
description: Este tópico descreve como executar scripts PowerShell dentro de uma máquina virtual de Azure Windows utilizando o comando executado
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: ddbac24020110e32792286a1ac64070316cfb081
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36332719"
---
# <a name="run-powershell-scripts-in-your-windows-vm-with-run-command"></a>Executar PowerShell scripts numa VM do Windows com o comando executado

Execute utiliza comandos o agente da VM para executar scripts do PowerShell de shell dentro de uma VM do Windows Azure. Estes scripts podem ser utilizados para a máquina geral ou de gestão de aplicações e podem ser utilizados para diagnosticar e resolver problemas de acesso e da rede VM e rapidamente obter a VM para um bom estado.

## <a name="benefits"></a>Benefícios

Existem várias opções que podem ser utilizadas para aceder às suas máquinas virtuais. Execute comandos podem ser executados scripts as máquinas virtuais com remotamente o agente da VM. Execução do comando pode ser utilizado através do portal do Azure, [REST API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [CLI do Azure](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke), ou [PowerShell](/powershell/module/azurerm.compute/invoke-azurermvmruncommand).

Esta capacidade é útil em todos os cenários em que pretende executar um script das máquinas virtuais e é uma das formas apenas para resolver problemas e remediar uma máquina virtual que não tem o RDP ou abrir a porta SSH devido a rede incorrecto ou utilizador administrativo configuração.

## <a name="restrictions"></a>Restrições

As seguintes restrições aplicam-se ao utilizar a executar o comando:

* O resultado é limitado a última bytes 4096
* O tempo mínimo para executar um script é cerca de 20 segundos
* Scripts são executados como sistema no Windows
* Pode executar um script num momento
* Não é possível cancelar um script em execução
* O tempo máximo que pode executar um script é 90 minutos, após o qual as TI será o tempo limite

**PermissionsConfig-OrchestratorUsersGroup***GroupName***-OrchestratorUser***UserName***\-remote** 

## <a name="run-a-command"></a>Executar um comando

Navegue para uma VM no [Azure](https://portal.azure.com) e selecione **executar comando** em **operações**. É-lhe apresentada uma lista dos comandos disponíveis para executar na VM.

![Executar a lista de comandos](./media/run-command/run-command-list.png)

Escolha um comando a executar. Alguns dos comandos podem ter parâmetros de entrada necessários ou opcionais. Para esses comandos os parâmetros são apresentados como campos de texto para lhe fornecer os valores de entrada. Para cada comando pode ver o script que está a ser executado, expandindo **Ver script**. **RunPowerShellScript** é diferente dos outros comandos, como permite-lhe fornecer os seus próprios scripts personalizados.

> [!NOTE]
> Os comandos incorporados não são editáveis.

Depois do comando é escolhido, clique em **executar** para executar o script. O script é executado e concluído, devolve o resultado e quaisquer erros na janela de saída. A seguinte captura de ecrã mostra um exemplo da saída da execução de **RDPSettings** comando.

![Execute a saída do comando de script](./media/run-command/run-command-script-output.png)

## <a name="commands"></a>Comandos

Esta tabela mostra a lista de comandos disponíveis para as VMs do Windows. O **RunPowerShellScript** comando pode ser utilizado para executar qualquer script personalizado que pretende.

|**Nome**|**Descrição**|
|---|---|
|**RunPowerShellScript**|Executa um script do PowerShell|
|**EnableRemotePS**|Configura a máquina para ativar o PowerShell remoto.|
|**EnableAdminAccount**|Verifica se a conta de administrador local é desativada e, se assim for, ativa-o.|
|**IPConfig**| Mostra informações detalhadas para o IP de endereços, gateway predefinido e de máscara de sub-rede para cada adaptador vinculada a TCP/IP.|
|**RDPSettings**|Verifica as definições de registo e as definições de política de domínio. Sugere ações de política se a máquina fizer parte de um domínio ou modifica as definições para os valores predefinidos.|
|**ResetAccountPassword**| Repõe a palavra-passe da conta do administrador incorporado.|
|**ResetRDPCert**|Remove o certificado SSL associado para o serviço de escuta RDP e restaura a segurança de listerner RDP a predefinição. Utilize este script se vir algum problema com o certificado.|
|**SetRDPPort**|Define o predefinido ou o utilizador especificar o número de porta para ligações de ambiente de trabalho remoto. Ativa a regra de firewall para acesso de entrada para a porta.|

## <a name="powershell"></a>PowerShell

Segue-se um exemplo utilizando o [Invoke-AzureRmVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand) cmdlet para executar um script do PowerShell numa VM do Azure.

```azurepowershell-interactive
Invoke-AzureRmVMRunCommand -ResourceGroupName '<myResourceGroup>' -Name '<myVMName>' -CommandId 'RunPowerShellScript' -ScriptPath '<pathToScript>' -Parameter @{"arg1" = "var1";"arg2" = "var2"}
```

## <a name="limiting-access-to-run-command"></a>Limitar o acesso ao executar o comando

Listar os comandos de execução ou que mostra os detalhes de um comando requerem o `Microsoft.Compute/locations/runCommands/read` permissão, que incorporada [leitor](../../role-based-access-control/built-in-roles.md#reader) função e superior.

Executar um comando exige o `Microsoft.Compute/virtualMachines/runCommand/action` permissão, que o [contribuinte](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) função e superior.

Pode utilizar um do [incorporada](../../role-based-access-control/built-in-roles.md) funções ou crie um [personalizado](../../role-based-access-control/custom-roles.md) função para utilizar a executar o comando.

## <a name="next-steps"></a>Passos Seguintes

Ver, [executem scripts na sua VM do Windows](run-scripts-in-vm.md) para saber mais sobre outras formas de executar scripts e comandos remotamente a VM.
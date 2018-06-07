---
title: Executar scripts no VM Linux do Azure
description: Este tópico descreve como executar scripts dentro de uma máquina virtual
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 05/02/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 209b476e21a3a9ba08eb907a279c7c5f8faa1c17
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661123"
---
# <a name="run-scripts-in-your-linux-vm"></a>Executar scripts a VM com Linux

Automatizar tarefas ou resolver problemas, poderá ter de executar os comandos numa VM. O seguinte artigo fornece uma breve descrição geral das funcionalidades que estão disponíveis para executar scripts e comandos as suas VMs.

## <a name="custom-script-extension"></a>Extensão de Script Personalizado

O [extensão de Script personalizado](../extensions/custom-script-linux.md) é principalmente utilizado para a instalação de software e configuração de implementação do post.

* Transferir e executar scripts em máquinas virtuais do Azure.
* Pode ser executado utilizando os modelos Azure Resource Manager, CLI do Azure, REST API, PowerShell ou o portal do Azure.
* Ficheiros de script podem ser transferidos a partir do armazenamento do Azure ou o GitHub ou fornecidos do seu PC quando executar a partir do portal do Azure.
* Executar script do PowerShell nas máquinas do Windows e Bash script em máquinas Linux.
* Útil para configuração de implementação de post, a instalação de software e outros configuração ou a tarefas de gestão.

## <a name="run-command"></a>Execute o comando

O [executar comando](run-command.md) funcionalidade permite que a máquina virtual e a gestão de aplicações e a resolução de problemas com scripts e está disponível, mesmo quando a máquina não é de rede ligado.

* Execute scripts em máquinas virtuais do Azure.
* Pode ser executado utilizando [portal do Azure](run-command.md), [REST API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [CLI do Azure](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke), ou [PowerShell](/powershell/module/azurerm.compute/invoke-azurermvmruncommand)
* Executar um script e vista de saída e repita conforme necessário no portal do Azure rapidamente.
* Script pode ser digitado diretamente ou pode executar um dos scripts incorporados.
* Executar script do PowerShell nas máquinas do Windows e Bash script em máquinas Linux.
* Útil para a máquina virtual e gestão de aplicações e para executar scripts em máquinas virtuais que não são rede ligada.

## <a name="hybrid-runbook-worker"></a>Função de Trabalho de Runbook Híbrida

O [Runbook Worker híbrido](../../automation/automation-hybrid-runbook-worker.md) fornece gestão de máquina, a aplicação e ambiente geral com scripts personalizados do utilizador armazenados na conta de automatização.

* Execute scripts no Azure e não do Azure máquinas.
* Pode ser executada através do portal do Azure, da CLI do Azure, a REST API, PowerShell, o webhook.
* Scripts armazenadas e geridas numa conta de automatização.
* Executar runbooks do PowerShell, fluxo de trabalho do PowerShell, Python ou gráfico
* Sem limite de tempo no tempo de execução do script.
* Vários scripts podem ser executados em simultâneo.
* Saída do script completa é devolvida e armazenada.
* Histórico de tarefas disponível para 90 dias.
* Podem executar scripts como sistema Local ou com as credenciais fornecido pelo utilizador.
* Requer [instalação manual](../../automation/automation-windows-hrw-install.md)

## <a name="serial-console"></a>Consola de série

O [consola de série](serial-console.md) fornece acesso direto a uma VM, semelhante a ter um teclado ligado à VM.

* Execute os comandos em máquinas virtuais do Azure.
* Pode ser executado utilizando uma consola baseado em texto para a máquina no portal do Azure.
* Inicie sessão na máquina com uma conta de utilizador local.
* Útil quando for necessário acesso à máquina virtual, independentemente do Estado de rede ou sistema operativo da máquina.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as diferentes funcionalidades que estão disponíveis para executar scripts e comandos as suas VMs.

* [Extensão de Script Personalizado](../extensions/custom-script-linux.md)
* [Execute o comando](run-command.md)
* [Runbook Worker híbrido](../../automation/automation-hybrid-runbook-worker.md)
* [Consola de série](serial-console.md)
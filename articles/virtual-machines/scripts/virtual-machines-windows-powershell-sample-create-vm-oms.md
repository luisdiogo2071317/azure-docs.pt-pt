---
title: Exemplo de Script do Azure PowerShell – OMS | Microsoft Docs
description: Exemplo de Script do Azure PowerShell – OMS
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: dc6cda6490740488e60f380d99952fa3f8d0c3fb
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655967"
---
# <a name="create-an-operations-management-suite-monitored-vm-with-powershell"></a>Criar uma VM monitorizada pela Operations Management Suite com o PowerShell

Este script cria uma Máquina Virtual do Azure, instala o agente do Operations Management Suite (OMS) e inscreve o sistema com uma área de trabalho do OMS. Assim que o script tiver sido executado, a máquina virtual estará visível na consola do OMS. Além disso, terá de atualizar o ID e a chave da área de trabalho do OMS.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-monitor-oms/create-windows-vm-detailed-oms.ps1 "Create VM OMS")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, a VM e todos os recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar a implementação. Cada item na tabela liga a documentação específica do comando.

| Comando | Notas |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Cria a máquina virtual e liga-a à placa de rede, à rede virtual, à sub-rede e ao grupo de segurança de rede. Este comando também abre a porta 80 e define as credenciais administrativas. |
| [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) | Adicione uma extensão de VM à máquina virtual. Neste caso, é utilizada a extensão de agente do Operations Management Suite para instalar o agente do OMS e inscrever a VM numa área de trabalho do OMS. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Remove um grupo de recursos e todos os recursos contidos no grupo. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre o módulo do Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Pode ver exemplos adicionais de scripts do PowerShell da máquina virtual na [Documentação da VM Windows do Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

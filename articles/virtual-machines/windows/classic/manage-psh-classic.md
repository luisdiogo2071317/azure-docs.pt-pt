---
title: Gerir máquinas virtuais com o Azure PowerShell | Documentos da Microsoft
description: Saiba mais comandos que pode utilizar para automatizar tarefas no gerenciamento de suas máquinas virtuais.
services: virtual-machines-windows
documentationcenter: windows
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 7cdf9bd3-6578-4069-8a95-e8585f04a394
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/12/2016
ms.author: kasing
ms.openlocfilehash: b7fafa148417ba1667ec0277b414105f95e428ce
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2019
ms.locfileid: "53971789"
---
# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Gerir máquinas virtuais utilizando o Azure PowerShell
> [!IMPORTANT] 
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [Resource Manager e clássica](../../../resource-manager-deployment-model.md). Este artigo explica como utilizar o modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager. Para comandos comuns do PowerShell com o modelo do Resource Manager, consulte [aqui](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Muitas tarefas fizer por dia para gerir as VMs podem ser automatizadas com cmdlets do PowerShell do Azure. Este artigo dá-lhe comandos de exemplo para tarefas mais simples e links para artigos que mostram os comandos para tarefas mais complexas.

> [!NOTE]
> Se ainda não instalado e configurado o Azure PowerShell ainda, pode obter as instruções no artigo [como instalar e configurar o Azure PowerShell](/powershell/azure/overview).
> 
> 

## <a name="how-to-use-the-example-commands"></a>Como utilizar os comandos de exemplo
Terá de substituir alguns o texto nos comandos com o texto que é adequado para o seu ambiente. O < e > símbolos indicam o texto é necessário substituir. Quando substituir o texto, remover os símbolos, mas deixar as entre aspas duplas no lugar.

## <a name="get-a-vm"></a>Obtenha uma VM
Esta é uma tarefa de básica que usará muitas vezes. Usá-lo para obter informações sobre uma VM, executar tarefas numa VM ou obtenha de saída para armazenar numa variável.

Para obter informações sobre a VM, execute este comando, substituindo tudo no aspas, incluindo os < e > caracteres:

    Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Para armazenar a saída numa variável de $vm, execute:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Iniciar sessão numa VM baseada no Windows
Execute estes comandos:

> [!NOTE]
> Pode obter o nome do serviço máquina virtual e a cloud na exibição dos **Get-AzureVM** comando.
> 
> $svcName = `"<cloud service name>"` $vmName = `"<virtual machine name>"` $localPath = `"<drive and folder location to store the downloaded RDP file, example: c:\temp >"` $localFile = $localPath + "\" + $vmname +". rdp"get-AzureRemoteDesktopFile - ServiceName $svcName-nome $vmName - LocalPath $localFile-iniciar
> 
> 

## <a name="stop-a-vm"></a>Parar uma VM
Execute este comando:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

> [!IMPORTANT]
> Utilize este parâmetro para manter o IP virtual (VIP) do serviço cloud, caso seja a última VM referido serviço em nuvem. <br><br> Se utilizar o parâmetro StayProvisioned, ainda será cobrado para a VM.
> 
> 

## <a name="start-a-vm"></a>Iniciar uma VM
Execute este comando:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Anexar um disco de dados
Esta tarefa requer algumas etapas. Em primeiro lugar, utilize o ****Add-AzureDataDisk**** cmdlet para adicionar o disco para o objeto de $vm. Em seguida, utilize **Update-AzureVM** cmdlet para atualizar a configuração da VM.

Também vai ter de decidir entre anexar um disco novo ou um que contenha dados. Para um novo disco, o comando cria o ficheiro. vhd e anexa-lo.

Para anexar um disco novo, execute este comando:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Para anexar um disco de dados existente, execute este comando:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Para anexar discos de dados de um ficheiro. vhd existente no armazenamento de BLOBs, execute este comando:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Criar uma VM baseada no Windows
Para criar uma nova máquina virtual baseada no Windows no Azure, utilize as instruções em [utilizar o Azure PowerShell para criar e pré-configurar máquinas virtuais baseadas em Windows](create-powershell.md). Passos neste tópico, por meio da criação de um conjunto de comandos do Azure PowerShell cria uma VM baseada em Windows que pode ser pré-configurados:

* Com a associação de domínio do Active Directory.
* Com discos adicionais.
* Como membro de um existente com balanceamento de carga definido.
* Com um endereço IP estático.


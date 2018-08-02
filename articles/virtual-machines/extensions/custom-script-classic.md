---
title: Extensão de Script personalizado numa VM do Windows | Documentos da Microsoft
description: Automatizar tarefas de configuração de VM do Azure ao utilizar a extensão de Script personalizado para executar scripts do PowerShell numa VM remota do Windows
services: virtual-machines-windows
documentationcenter: ''
author: zroiy
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: ebb7340a-8f61-4d3c-a290-d7bf8de2d0bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: roiyz
ms.openlocfilehash: 5344471bb39219975198811f25d4074acb6250c0
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39414897"
---
# <a name="custom-script-extension-for-windows-using-the-classic-deployment-model"></a>Personalizado Script extensão para Windows usando o modelo de implementação clássica

> [!IMPORTANT] 
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [Resource Manager e clássica](../../azure-resource-manager/resource-manager-deployment-model.md). Este artigo explica como utilizar o modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager. Saiba como [executar estes passos com o modelo do Resource Manager](custom-script-windows.md).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../includes/virtual-machines-classic-portal.md)]

A extensão de Script personalizado transfere e executa scripts em máquinas virtuais do Azure. Esta extensão é útil para a configuração pós-implementação, instalação de software ou qualquer outra tarefa de gestão/configuração. Os scripts podem ser transferidos a partir do armazenamento do Azure ou do GitHub, ou fornecidos para o portal do Azure no runtime da extensão. A extensão de Script Personalizado é integrada em modelos do Azure Resource Manager, podendo também ser executada ao utilizar a CLI do Azure, PowerShell, portal do Azure ou API REST de Máquinas Virtuais do Azure.

Este documento fornece detalhes sobre como utilizar a extensão de Script personalizado com o módulo Azure PowerShell, modelos Azure Resource Manager e detalhes de resolução de problemas de etapas em sistemas Windows.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="operating-system"></a>Sistema Operativo

A extensão de Script personalizado para o Windows podem ser executado em Windows Server 2008 R2, 2012 e 2012 R2 e 2016 versões.

### <a name="script-location"></a>Localização do script

O script tem de ser armazenados no armazenamento do Azure ou qualquer outra localização acessível por meio de um URL válido.

### <a name="internet-connectivity"></a>Conectividade com a Internet

O Custom Script extensão para Windows requer que a máquina virtual de destino está ligada à internet. 

## <a name="extension-schema"></a>Esquema de extensão

O JSON seguinte mostra o esquema para a extensão de Script personalizado. A extensão requer uma localização do script (armazenamento do Azure ou noutra localização com o URL válido) e um comando a executar. Se utilizar o armazenamento do Azure como a origem do script, uma chave de conta e o nome da conta de armazenamento do Azure é necessária. Esses itens devem ser tratados como dados confidenciais e especificados na configuração de definição protegido extensões. Dados de definição de protegidos de extensão VM do Azure são encriptados e desencriptados apenas na máquina de virtual de destino.

```json
{
    "name": "config-app",
    "type": "Microsoft.ClassicCompute/virtualMachines/extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-01",
    "properties": {
        "publisher": "Microsoft.Compute",
        "extension": "CustomScriptExtension",
        "version": "1.8",
        "parameters": {
            "public": {
                "fileUris": "[myScriptLocation]"
            },
            "private": {
                "commandToExecute": "[myExecutionString]"
            }
        }
    }
}
```

### <a name="property-values"></a>Valores de propriedade

| Nome | Valor / exemplo |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| publicador | Microsoft.Compute |
| Extensão | CustomScriptExtension |
| typeHandlerVersion | 1.8 |
| fileUris (por exemplo) | https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1 |
| commandToExecute (por exemplo) | PowerShell - ExecutionPolicy irrestrito - configurar-música-app.ps1 de ficheiros |

## <a name="template-deployment"></a>Implementação de modelos

Extensões VM do Azure podem ser implementadas com modelos Azure Resource Manager. O esquema JSON detalhado na secção anterior pode ser utilizado num modelo do Azure Resource Manager para executar a extensão de Script personalizado durante uma implementação de modelo do Azure Resource Manager. Pode encontrar aqui, um modelo de exemplo que inclui a extensão de Script personalizado [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>Implementação do PowerShell

O `Set-AzureVMCustomScriptExtension` comando pode ser utilizado para adicionar a extensão de Script personalizado para uma máquina virtual existente. Para obter mais informações, consulte [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).

```powershell
# create vm object
$vm = Get-AzureVM -Name 2016clas -ServiceName 2016clas1313

# set extension
Set-AzureVMCustomScriptExtension -VM $vm -FileUri myFileUri -Run 'create-file.ps1'

# update vm
$vm | Update-AzureVM
```

## <a name="troubleshoot-and-support"></a>Resolução de problemas e suporte

### <a name="troubleshoot"></a>Resolução de problemas

Podem ser obtidos dados sobre o estado das implementações de extensão do portal do Azure e utilizando o módulo Azure PowerShell. Para ver o estado de implementação de extensões para uma determinada VM, execute o seguinte comando.

```powershell
Get-AzureVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

A execução da extensão de saída nos arquivos encontrados no diretório seguinte na máquina de virtual de destino a sessão iniciada.

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

O script propriamente dito é transferido para o diretório seguinte na máquina virtual de destino.

```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads
```

### <a name="support"></a>Suporte

Se precisar de mais ajuda a qualquer momento neste artigo, pode contactar os especialistas do Azure sobre o [fóruns do Azure do MSDN e Stack Overflow](https://azure.microsoft.com/support/forums/). Em alternativa, pode enviar um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione o suporte de Get. Para informações sobre como utilizar o suporte do Azure, leia os [FAQ do suporte Microsoft Azure](https://azure.microsoft.com/support/faq/).

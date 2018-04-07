---
title: Extensão de diagnóstico do Azure para Windows | Microsoft Docs
description: Monitorizar as VMs do Windows Azure utilizando a extensão de diagnóstico do Windows Azure
services: virtual-machines-windows
documentationcenter: ''
author: johnkemnetz
manager: ashwink
editor: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/06/2018
ms.author: johnkem
ms.openlocfilehash: a9621f6f93d8e14e53cfe05ca4a714e88b9d8572
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="windows-azure-diagnostics-extension"></a>Extensão de diagnóstico do Azure do Windows

## <a name="overview"></a>Descrição geral

A extensão de VM de diagnóstico do Windows Azure permite-lhe recolher dados de monitorização, tais como registos de evento e os contadores de desempenho da sua VM do Windows. Granularly pode especificar quais os dados que pretende recolher e onde pretende que os dados para aceder, tal como uma conta de armazenamento do Azure ou de um Hub de eventos do Azure. Também pode utilizar estes dados para criar gráficos no portal do Azure ou criar métricas alertas.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="operating-system"></a>Sistema operativo

A extensão de diagnóstico do Windows Azure pode ser executada no cliente do Windows 10, Windows Server 2008 R2, 2012, 2012 R2 e 2016.

### <a name="internet-connectivity"></a>Conectividade Internet

A extensão de diagnóstico do Windows Azure requer que a máquina virtual de destino está ligada à internet. 

## <a name="extension-schema"></a>Esquema de extensão

[Os valores de esquema e a propriedade de extensão de diagnóstico do Windows Azure são descritos neste documento.](../../monitoring-and-diagnostics/azure-diagnostics-schema-1dot3-and-later.md)

## <a name="template-deployment"></a>Implementação de modelos

Extensões VM do Azure podem ser implementadas com modelos Azure Resource Manager. O esquema JSON detalhado na secção anterior pode ser utilizado num modelo Azure Resource Manager para executar a extensão do Windows Azure Diagnostics durante uma implementação de modelo Azure Resource Manager. Consulte [monitorização de utilização e diagnóstico com modelos de VM do Windows e o Azure Resource Manager](extensions-diagnostics-template.md).

## <a name="azure-cli-deployment"></a>Implementação da CLI do Azure

A CLI do Azure pode ser utilizada para implementar a extensão de diagnóstico do Windows Azure para uma máquina virtual existente. Substitua as definições protegidas e propriedades de definições JSON válido do esquema de extensão acima. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name IaaSDiagnostics \
  --publisher Microsoft.Azure.Diagnostics \
  --version 1.9.0.0 --protected-settings protected-settings.json \
  --settings public-settings.json 
```

## <a name="powershell-deployment"></a>Implementação de PowerShell

O `Set-AzureRmVMDiagnosticsExtension` comando pode ser utilizado para adicionar a extensão de diagnóstico do Windows Azure para uma máquina virtual existente. Consulte também [utilize o PowerShell para ativar o diagnóstico do Azure numa máquina virtual com o Windows](ps-extensions-diagnostics.md).
```powershell
$vm_resourcegroup = "myvmresourcegroup"
$vm_name = "myvm"
$diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

Set-AzureRmVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup `
  -VMName $vm_name `
  -DiagnosticsConfigurationPath $diagnosticsconfig_path
```

## <a name="troubleshoot-and-support"></a>Resolver problemas e suporte

### <a name="troubleshoot"></a>Resolução de problemas

É possível obter dados sobre o estado das implementações de extensão do portal do Azure e utilizando a CLI do Azure. Para ver o estado de implementação das extensões para uma determinada VM, execute o seguinte comando utilizando a CLI do Azure.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

[Consulte este artigo](../../monitoring-and-diagnostics/azure-diagnostics-troubleshooting.md) para obter um guia de resolução de problemas mais abrangente para a extensão de diagnóstico do Windows Azure.

### <a name="support"></a>Suporte

Se precisar de mais ajuda, a qualquer altura neste artigo, pode contactar as especialistas do Azure no [fóruns do MSDN Azure e Stack Overflow](https://azure.microsoft.com/support/forums/). Em alternativa, pode ficheiro um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione o suporte de Get. Para informações sobre como utilizar o suporte do Azure, leia o [suporte do Microsoft Azure FAQ](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Próximos Passos
* [Saiba mais sobre a extensão de diagnóstico do Windows Azure](../../monitoring-and-diagnostics/azure-diagnostics.md)
* [Reveja as versões e esquema de extensão](../../monitoring-and-diagnostics/azure-diagnostics-schema.md)

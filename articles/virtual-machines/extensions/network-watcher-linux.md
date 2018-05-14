---
title: Extensão de máquina virtual de agente do observador de rede do Azure para Linux | Microsoft Docs
description: Implemente o agente do observador de rede na máquina de virtual com Linux utilizando uma extensão da máquina virtual.
services: virtual-machines-linux
documentationcenter: ''
author: gurudennis
manager: amku
editor: ''
tags: azure-resource-manager
ms.assetid: 5c81e94c-e127-4dd2-ae83-a236c4512345
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: db508e2311602a66a2c252ffaa842f8bfb4f670b
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a>Extensão da máquina virtual de agente do observador de rede para Linux

## <a name="overview"></a>Descrição geral

[Observador de rede do Azure](/azure/network-watcher/) é um serviço de monitorização, diagnóstico e análise de desempenho de rede que permite a monitorização de redes do Azure. A extensão de máquina virtual (VM) do agente do observador de rede é um requisito para algumas das funcionalidades de observador de rede em VMs do Azure, tais como capturar o tráfego de rede a pedido e outras funcionalidades avançadas.

Este artigo fornece detalhes sobre as plataformas suportadas e opções de implementação para a extensão de VM de agente do observador de rede do Linux. Instalação do agente não interromper, nem requer um reinício, da VM.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="operating-system"></a>Sistema operativo

A extensão de agente do observador de rede pode ser configurada para as distribuições de Linux seguintes:

| Distribuição | Versão |
|---|---|
| Ubuntu | 16.04 LTS, 14.04 LTS e 12.04 LTS |
| Debian | 7 e 8 |
| RedHat | 6 e 7 |
| Oracle Linux | 6.8 + e 7 |
| Servidor Linux Empresarial SUSE | 11 e 12 |
| OpenSUSE bissexto | 42.3 + |
| CentOS | 6.5 + e 7 |
| CoreOS | 899.17.0+ |

CoreOS não é suportada.

### <a name="internet-connectivity"></a>Conectividade Internet

Algumas das funcionalidades do agente do observador de rede necessita que uma VM está ligada à Internet. Sem a capacidade para estabelecer ligações de saída, algumas das funcionalidades do agente do observador de rede poderão avaria ou ficar indisponíveis. Para obter mais informações sobre a funcionalidade de observador de rede que requer que o agente, consulte o[documentação de observador de rede](/azure/network-watcher/).

## <a name="extension-schema"></a>Esquema de extensão

O JSON seguinte mostra o esquema para a extensão de agente do observador de rede. A extensão não necessita, ou suportar, quaisquer definições fornecido pelo utilizador. A extensão baseia-se na sua configuração predefinida.

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentLinux",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>Valores de propriedade

| Nome | Valor / exemplo |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Fabricante | Microsoft.Azure.NetworkWatcher |
| tipo | NetworkWatcherAgentLinux |
| typeHandlerVersion | 1.4 |

## <a name="template-deployment"></a>Implementação de modelos

É possível implementar extensões de VM do Azure com um modelo Azure Resource Manager. Para implementar a extensão de agente do observador de rede, utilize o esquema json anterior no modelo.

## <a name="azure-cli-10-deployment"></a>Implementação CLI 1.0 do Azure

O exemplo seguinte implementa a extensão de VM de agente do observador de rede VM existente implementada através do modelo de implementação clássica:

```azurecli
azure config mode asm
azure vm extension set myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="azure-cli-20-deployment"></a>Implementação de 2.0 da CLI do Azure

O exemplo seguinte implementa a extensão de VM de agente do observador de rede VM existente implementada através do Gestor de recursos:

```azurecli
az vm extension set --resource-group myResourceGroup1 --vm-name myVM1 --name NetworkWatcherAgentLinux --publisher Microsoft.Azure.NetworkWatcher --version 1.4
```

## <a name="troubleshooting-and-support"></a>Resolução de problemas e suporte

### <a name="troubleshooting"></a>Resolução de problemas

Pode obter dados sobre o estado das implementações de extensão com o portal do Azure ou a CLI do Azure.

O exemplo seguinte mostra o estado de implementação das extensões para uma VM implementada através do modelo de implementação clássica, utilizando a CLI do Azure 1.0:

```azurecli
azure config mode asm
azure vm extension get myVM1
```
Resultado da execução de extensão é registado para ficheiros encontrados no diretório seguinte:

`
/var/log/azure/Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentLinux/
`

O exemplo seguinte mostra o estado de implementação da extensão NetworkWatcherAgentLinux para uma VM implementada através do Resource Manager, utilizando o 2.0 CLI do Azure:

```azurecli
az vm extension show --name NetworkWatcherAgentLinux --resource-group myResourceGroup1 --vm-name myVM1
```

### <a name="support"></a>Suporte

Se precisar de mais ajuda, a qualquer altura neste artigo, pode consultar o [documentação de observador de rede](/azure/network-watcher/), ou contacte as especialistas do Azure no [fóruns do MSDN Azure e Stack Overflow](https://azure.microsoft.com/support/forums/). Em alternativa, pode ficheiro um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporte**. Para obter informações sobre a utilização do Azure suporta, consulte o [suporte do Microsoft Azure FAQ](https://azure.microsoft.com/support/faq/).

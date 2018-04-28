---
title: Exemplo de Script do PowerShell do Azure - encaminhar o tráfego para elevada disponibilidade de aplicações | Microsoft Docs
description: Exemplo de Script do PowerShell do Azure - encaminhar o tráfego para elevada disponibilidade de aplicações
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: georgewallace
tags: azure-infrastructure
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 24054625870cb073eec9769f50f370deb2828535
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="route-traffic-for-high-availability-of-applications"></a>Encaminhar o tráfego para elevada disponibilidade de aplicações

Este script cria um grupo de recursos, dois planos de serviço de aplicações, duas aplicações web, um perfil do Gestor de tráfego e dois pontos finais Gestor de tráfego. Gestor de tráfego direciona o tráfego para a aplicação de uma região como região primária e a região secundária quando a aplicação na região primária não está disponível. Antes de executar o script, tem de alterar os valores MyWebApp, MyWebAppL1 e MyWebAppL2 para valores exclusivos através do Azure. Depois de executar o script, pode aceder à aplicação na região primária com mywebapp.trafficmanager.net o URL.

Se for preciso, instale o Azure PowerShell com a instrução que se encontra no [Guia do Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) e, em seguida, execute `Connect-AzureRmAccount` para criar uma ligação ao Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.ps1 "Route traffic for high availability")]


Execute o seguinte comando para remover o grupo de recursos, a VM e todos os recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup1
Remove-AzureRmResourceGroup -Name myResourceGroup2
```


## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um grupo de recursos, uma aplicação Web, um perfil do gestor de tráfego e todos os recursos relacionados. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [New-AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Cria um plano do Serviço de Aplicações. Trata-se como um farm de servidores para a sua aplicação web do Azure. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Cria uma aplicação web do Azure dentro do plano do App Service. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/new-azurermresource) | Cria uma aplicação web do Azure dentro do plano do App Service. |
| [Novo AzureRmTrafficManagerProfile](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | Cria um perfil do Gestor de Tráfego do Azure. |
| [Novo AzureRmTrafficManagerEndpoint](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | Adiciona um ponto final de um perfil do Traffic Manager do Azure. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Exemplos de script do PowerShell redes adicionais podem ser encontrados no [documentação de descrição geral de funcionamento em rede do Azure](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).

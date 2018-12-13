---
title: Exemplo de Script do PowerShell do Azure - Dimensionar aplicação todo o mundo com o Gestor de tráfego | Documentos da Microsoft
description: Exemplo do Script do Azure PowerShell – Dimensionar uma aplicação Web para todo o mundo com uma arquitetura de elevada disponibilidade
services: app-service\web
documentationcenter: ''
author: syntaxc4
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: 470f0129-1efe-462c-a029-5c66e04158a8
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: f71b52d3e3eb7264fa3cf2d8a82c46ad6498c8a0
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53308231"
---
# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a>Dimensionar uma aplicação Web para todo o mundo com uma arquitetura de elevada disponibilidade

Neste cenário criará um grupo de recursos, dois planos do serviço de aplicações, duas aplicações Web, um perfil do gestor de tráfego e dois pontos finais do gestor de tráfego. Assim que o exercício estiver concluído, terá uma arquitetura de elevada disponibilidade, que permite fornecer a disponibilidade global da sua aplicação Web com base na latência de rede mais baixa.

Se for preciso, instale o Azure PowerShell com a instrução que se encontra no [Guia do Azure PowerShell](/powershell/azure/overview) e, em seguida, execute `Connect-AzureRmAccount` para criar uma ligação ao Azure.

## <a name="sample-script"></a>Script de exemplo

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/scale-geographic/scale-geographic.ps1 "Scale a web app worldwide with a high-availability architecture")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Depois de executar o script de exemplo, o seguinte comando pode ser utilizado para remover o grupo de recursos, a aplicação Web e todos os recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [New-AzureRMTrafficManagerProfile](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | Cria um perfil do Gestor de Tráfego. |
| [New-AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Cria um plano do Serviço de Aplicações. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Cria uma aplicação Web. |
| [New-AzureRMTrafficManagerEndpoint](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | Cria um ponto final num Perfil do Gestor de Tráfego. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o módulo do Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Pode ver os exemplos do Azure Powershell adicionais para as Aplicações Web do Serviço de Aplicações do Azure em [Exemplos do Azure PowerShell](../app-service-powershell-samples.md).

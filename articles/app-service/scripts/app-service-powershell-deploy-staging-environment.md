---
title: Script do Azure PowerShell de exemplo - criar aplicação e implementar código no bloco de teste | Documentos da Microsoft
description: Exemplo do Script do Azure PowerShell - Criar uma aplicação Web e implementar código num ambiente de teste
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: 27cf0680-c3a9-4a58-9f71-6dec09f6b874
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 51c51c9e64c14afb13c59e82c02a38d608812c76
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53313076"
---
# <a name="create-a-web-app-and-deploy-code-to-a-staging-environment"></a>Criar uma aplicação Web e implementar código para um ambiente de teste

Este script de exemplo cria uma aplicação Web no Serviço de Aplicações com um bloco de implementação adicional denominado "teste" e, em seguida, implementa uma aplicação de exemplo para o bloco "teste".

Se for preciso, instale o Azure PowerShell com a instrução que se encontra no [Guia do Azure PowerShell](/powershell/azure/overview) e, em seguida, execute `Connect-AzureRmAccount` para criar uma ligação ao Azure.

## <a name="sample-script"></a>Script de exemplo

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.ps1?highlight=1 "Create a web app and deploy code to a staging environment")]

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
| [New-AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Cria um plano do Serviço de Aplicações. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Cria uma aplicação Web. |
| [Set-AzureRmAppServicePlan](/powershell/module/azurerm.websites/set-azurermappserviceplan) | Modifica um plano do Serviço de Aplicações para alterar o escalão de preço. |
| [New-AzureRmWebAppSlot](/powershell/module/azurerm.websites/new-azurermwebappslot) | Cria um bloco de implementação para uma aplicação Web. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Modifica um recurso num grupo de recursos. |
| [Switch-AzureRmWebAppSlot](/powershell/module/azurerm.websites/switch-azurermwebappslot) | Alterna um bloco de implementação da aplicação Web para produção. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o módulo do Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Pode ver os exemplos do Azure Powershell adicionais para as Aplicações Web do Serviço de Aplicações do Azure em [Exemplos do Azure PowerShell](../app-service-powershell-samples.md).

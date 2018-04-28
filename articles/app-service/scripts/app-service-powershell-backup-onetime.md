---
title: Exemplo do Script do Azure PowerShell - Fazer cópia de segurança de uma aplicação Web | Microsoft Docs
description: Exemplo do Script do Azure PowerShell - Fazer cópia de segurança de uma aplicação Web
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: fc755f82-ca3e-4532-b251-690b699324d6
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 10/30/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: ed373131a2ca62ec79d9742545c2f18f6c1e382a
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="back-up-a-web-app"></a>Fazer cópia de segurança de uma aplicação Web

Este script de exemplo cria uma aplicação Web no Serviço de Aplicações com os respetivos recursos relacionados e, em seguida, cria uma cópia de segurança única para o mesmo. 

Se for preciso, instale o Azure PowerShell com a instrução que se encontra no [Guia do Azure PowerShell](/powershell/azure/overview) e, em seguida, execute `Connect-AzureRmAccount` para criar uma ligação ao Azure. 

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/app-service/backup-onetime/backup-onetime.ps1?highlight=1-5 "Back up a web app")]

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
| [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) | Cria uma Conta de armazenamento. |
| [New-AzureStorageContainer](/powershell/module/azure.storage/new-azurestoragecontainer) | Cria um contentor de armazenamento do Azure. |
| [New-AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) | Gera um token SAS para um contentor de armazenamento do Azure.  |
| [New-AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Cria um plano do Serviço de Aplicações. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Cria uma aplicação Web. |
| [New-AzureRmWebAppBackup](/powershell/module/azurerm.websites/new-azurermwebappbackup) | Cria uma cópia de segurança de uma aplicação Web. |
| [Get-AzureRmWebAppBackupList](/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | Obtém uma lista de cópias de segurança para uma aplicação Web. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre o módulo do Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Pode ver os exemplos do Azure Powershell adicionais para as Aplicações Web do Serviço de Aplicações do Azure em [Exemplos do Azure PowerShell](../app-service-powershell-samples.md).

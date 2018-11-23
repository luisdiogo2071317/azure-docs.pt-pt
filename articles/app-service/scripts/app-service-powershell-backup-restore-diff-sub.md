---
title: Exemplo de Script do PowerShell do Azure - restaurar uma aplicação web a partir de uma cópia de segurança noutra subscrição | Documentos da Microsoft
description: Exemplo de Script do PowerShell do Azure - restaurar uma aplicação web a partir de uma cópia de segurança noutra subscrição
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jpconnoc
editor: ''
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 11/21/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 7ed593e408b57246ef155ff8e36f054aacb2e063
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/22/2018
ms.locfileid: "52292177"
---
# <a name="restore-a-web-app-from-a-backup-in-another-subscription"></a>Restaurar uma aplicação web a partir de uma cópia de segurança noutra subscrição

Este script de exemplo obtém uma cópia de segurança concluída anteriormente a partir de uma aplicação web existente e restaurá-lo para uma aplicação web noutra subscrição. 

Se for preciso, instale o Azure PowerShell com a instrução que se encontra no [Guia do Azure PowerShell](/powershell/azure/overview) e, em seguida, execute `Connect-AzureRmAccount` para criar uma ligação ao Azure. 

## <a name="sample-script"></a>Script de exemplo

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-restore-diff-sub/backup-restore-diff-sub.ps1?highlight=1-6 "Restore a web app from a backup in another subscription")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Se já não precisar da aplicação web, utilize o seguinte comando para remover o grupo de recursos, a aplicação web e todos os recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroupName -Force
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) | Adiciona uma conta autenticada a utilizar para pedidos de cmdlet do Azure Resource Manager.  |
| [Get-AzureRmWebAppBackupList](/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | Obtém uma lista de cópias de segurança para uma aplicação Web. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Cria uma aplicação web |
| [Restauro-AzureRmWebAppBackup](/powershell/module/azurerm.websites/restore-azurermwebappbackup) | Restaura uma aplicação web a partir de uma cópia de segurança anteriormente concluída. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o módulo do Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Pode ver os exemplos do Azure Powershell adicionais para as Aplicações Web do Serviço de Aplicações do Azure em [Exemplos do Azure PowerShell](../app-service-powershell-samples.md).

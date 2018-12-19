---
title: Exemplo de Script do PowerShell do Azure - restaurar uma aplicação web a partir de uma cópia de segurança | Documentos da Microsoft
description: Exemplo de Script do PowerShell do Azure - restaurar uma aplicação web a partir de uma cópia de segurança
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 11/21/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: caee00130efdea253ced16d090eafeee22c16ac3
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2018
ms.locfileid: "53585236"
---
# <a name="restore-a-web-app-from-a-backup-using-azure-powershell"></a>Restaurar uma aplicação web a partir de uma cópia de segurança com o Azure PowerShell

Este script de exemplo obtém uma cópia de segurança concluída anteriormente a partir de uma aplicação web existente e restaurá-lo ao substituir seu conteúdo. 

Se for preciso, instale o Azure PowerShell com a instrução que se encontra no [Guia do Azure PowerShell](/powershell/azure/overview) e, em seguida, execute `Connect-AzureRmAccount` para criar uma ligação ao Azure. 

## <a name="sample-script"></a>Script de exemplo

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-restore/backup-restore.ps1?highlight=1-2 "Restore a web app from a backup")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Se já não precisar da aplicação web, utilize o seguinte comando para remover o grupo de recursos, a aplicação web e todos os recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroupName -Force
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [Get-AzureRmWebAppBackupList](/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | Obtém uma lista de cópias de segurança para uma aplicação Web. |
| [Restauro-AzureRmWebAppBackup](/powershell/module/azurerm.websites/restore-azurermwebappbackup) | Restaura uma aplicação web a partir de uma cópia de segurança anteriormente concluída. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o módulo do Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Pode ver os exemplos do Azure Powershell adicionais para as Aplicações Web do Serviço de Aplicações do Azure em [Exemplos do Azure PowerShell](../samples-powershell.md).

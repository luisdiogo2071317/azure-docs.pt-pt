---
title: Exemplo de Script do Azure PowerShell - Eliminar uma cópia de segurança agendada para uma aplicação Web | Microsoft Docs
description: Exemplo de Script do Azure PowerShell - Eliminar uma cópia de segurança agendada para uma aplicação Web
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: ebcadb49-755d-4202-a5eb-f211827a9168
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 10/30/2017
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 4a2abe05b6781358355cc264b66673671bc52557
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/10/2018
ms.locfileid: "53185652"
---
# <a name="delete-a-backup-for-a-web-using-azure-powershell"></a>Eliminar uma cópia de segurança para uma web com o Azure PowerShell

Este script de exemplo cria uma aplicação Web no Serviço de Aplicações com os respetivos recursos relacionados e, em seguida, cria uma cópia de segurança única para o mesmo. 

Para executar este script, precisa de uma cópia de segurança existente para uma aplicação Web. Para criar uma, veja [Realizar cópia de segurança de uma aplicação Web](app-service-powershell-backup-onetime.md) ou [Criar uma cópia de segurança agendada para uma aplicação Web](app-service-powershell-backup-scheduled.md).

## <a name="sample-script"></a>Script de exemplo

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-delete/backup-delete.ps1?highlight=1-2,11 "Delete a backup for a web app")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Depois de executar o script de exemplo, o seguinte comando pode ser utilizado para remover o grupo de recursos, a aplicação Web e todos os recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [Get-AzureRmWebAppBackupList](/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | Obtém uma lista de cópias de segurança para uma aplicação Web. |
| [Remove-AzureRmWebAppBackup](/powershell/module/azurerm.websites/remove-azurermwebappbackup) | Remove a cópia de segurança especificada de uma aplicação Web. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o módulo do Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Pode ver os exemplos do Azure Powershell adicionais para as Aplicações Web do Serviço de Aplicações do Azure em [Exemplos do Azure PowerShell](../app-service-powershell-samples.md).

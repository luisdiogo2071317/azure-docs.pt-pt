---
title: Script do Azure PowerShell-criar uma conta de API do Azure Cosmos DB para o MongoDB
description: Script do Azure PowerShell de exemplo – criar uma conta de API do Azure Cosmos DB para o MongoDB
services: cosmos-db
ms.service: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.custom: mvc
ms.devlang: PowerShell
ms.topic: sample
ms.date: 05/29/2018
ms.openlocfilehash: c12f2f86e56ac90fbea366e43959dd934c89d931
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/28/2018
ms.locfileid: "53811057"
---
# <a name="create-an-azure-cosmos-db-account-with-azure-cosmos-dbs-api-for-mongodb-using-powershell"></a>Criar uma conta do Azure Cosmos DB com API do Azure Cosmos DB para o MongoDB com o PowerShell

Este script do PowerShell de exemplo cria uma conta do Cosmos com API do Azure Cosmos DB para o MongoDB. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/cosmosdb/create-and-configure-mongodb-database/create-and-configure-mongodb-database.ps1?highlight=9,12-15,18,21-23,26-29,32-37 "Create an Azure Cosmos DB account")]

## <a name="clean-up-deployment"></a>Limpar a implementação

Depois de executar o script de exemplo, pode ser utilizado o seguinte comando para remover o grupo de recursos e todos os recursos associados ao mesmo.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [New-AzureRmResource](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresource?view=azurermps-3.8.0) | Cria um servidor lógico que aloja uma base de dados ou conjunto elástico. |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |
|||

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](https://docs.microsoft.com/powershell/).

Pode ver exemplos do script do Azure PowerShell Cosmos DB adicionais nos [scripts do PowerShell do Azure Cosmos DB](../powershell-samples.md).
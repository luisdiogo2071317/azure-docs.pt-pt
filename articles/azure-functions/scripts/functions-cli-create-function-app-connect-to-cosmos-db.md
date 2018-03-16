---
title: "Criar uma Função do Azure que se liga a um Azure Cosmos DB | Microsoft Docs"
description: "Exemplo do Script da CLI do Azure - Criar uma Função do Azure que se liga a um Azure Cosmos DB"
services: functions
documentationcenter: functions
author: ggailey777
manager: cfowler
editor: 
tags: functions
ms.assetid: 
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 01/22/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 092e0681b0491fc1c54c19e234aafdac6d428fd1
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a>Criar uma Função do Azure que se liga a um Azure Cosmos DB

Este script de exemplo das Funções do Azure cria uma aplicação de funções e liga a função a uma base de dados do Azure Cosmos DB. A definição da aplicação criada que contém a ligação pode ser utilizada com um [acionador ou enlace do Azure Cosmos DB](..\functions-bindings-cosmosdb.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se utilizar a CLI localmente, certifique-se de que a versão em execução é a 2.0 ou posterior. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de exemplo

Este exemplo cria uma aplicação de Funções do Azure e adiciona uma chave de acesso e um ponto final do Cosmos DB às definições da aplicação.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects to an Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos: cada comando na tabela está ligado a documentação específica do comando.

| Comando | Notas |
|---|---|
| [az login](https://docs.microsoft.com/cli/azure/reference-index#az_login) | Iniciar sessão no Azure. |
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Criar um grupo de recursos com localização |
| [az storage accounts create](https://docs.microsoft.com/cli/azure/storage/account) | Criar uma conta do Storage |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_create) | Criar uma nova aplicação de funções |
| [az cosmosdb create](https://docs.microsoft.com/cli/azure/cosmosdb#az_cosmosdb_create) | Criar base de dados cosmosdb |
| [az group delete](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Limpeza |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Pode ver exemplos do script da CLI das Funções do Azure adicionais na [Documentação das Funções do Azure](../functions-cli-samples.md).





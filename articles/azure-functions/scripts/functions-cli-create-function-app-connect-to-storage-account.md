---
title: "Criar uma Função do Azure que se liga ao Armazenamento do Azure | Microsoft Docs"
description: "Exemplo do Script da CLI do Azure - Criar uma Função do Azure que se liga a um Armazenamento do Azure"
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
ms.date: 04/20/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: cbe7bf95574ca7a77d666981691da05357ce9a0d
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="create-a-function-app-that-connects-to-an-azure-storage-account"></a>Criar uma aplicação de funções que se liga a uma conta de Armazenamento do Azure

Este script de exemplo das Funções do Azure cria uma aplicação de funções e liga a função a uma conta de Armazenamento do Azure. A definição da aplicação criada que contém a ligação pode ser utilizada com um [[acionador ou enlace de armazenamento](..\functions-bindings-storage-blob.md). 

[!INCLUDE [upgrade runtime](../../../includes/functions-cli-version-note.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se utilizar a CLI localmente, certifique-se de que a versão em execução é a 2.0 ou posterior. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de exemplo

Este exemplo cria uma aplicação de Funções do Azure e adiciona a cadeia de ligação de armazenamento a uma definição da aplicação.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]


## <a name="clean-up-deployment"></a>Limpar a implementação

Depois de executar o script de exemplo, execute o seguinte comando para remover o grupo de recursos e todos os recursos relacionados:

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az login](https://docs.microsoft.com/cli/azure/reference-index#az_login) | Iniciar sessão no Azure. |
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Criar um grupo de recursos com localização |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account) | Criar uma conta do Storage |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_create) | Criar uma nova aplicação de funções |
| [az group delete](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Limpeza |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Pode ver exemplos do script da CLI das Funções do Azure adicionais na [Documentação das Funções do Azure](../functions-cli-samples.md).

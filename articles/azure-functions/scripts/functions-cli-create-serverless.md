---
title: "Exemplo do Script da CLI do Azure - Criar uma Aplicação de Funções para execução sem servidor | Microsoft Docs"
description: "Exemplo do Script da CLI do Azure - Criar uma Aplicação de Funções para execução sem servidor"
services: functions
documentationcenter: functions
author: syntaxc4
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: 0e221db6-ee2d-4e16-9bf6-a456cd05b6e7
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 10/04/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: e868f07a8c24948655d81c7db1859d819b01c062
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="create-a-function-app-for-serverless-code-execution"></a>Criar uma aplicação de funções para execução de código sem servidor

Este script de exemplo das Funções do Azure cria uma aplicação de funções, que é um contentor para as suas funções. A aplicação de funções é criada com o [plano de consumo](../functions-scale.md#consumption-plan), que é ideal para cargas de trabalho sem servidor desencadeada por eventos.

[!INCLUDE [upgrade runtime](../../../includes/functions-cli-version-note.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão 2.0 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de exemplo

Este script cria uma aplicação das Funções do Azure com o [plano de consumo](../functions-scale.md#consumption-plan).

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-consumption/create-function-app-consumption.sh "Create an Azure Function on a consumption plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicação do script

Cada comando na tabela liga à documentação específica do comando. Este script utiliza os seguintes comandos:

| Comando | Notas |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az storage account create](/cli/azure/storage/account#az_storage_account_create) | Cria uma Conta de armazenamento do Azure. |
| [az functionapp create](/cli/azure/functionapp#az_functionapp_create) | Cria uma aplicação de funções. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](/cli/azure).

Pode ver exemplos do script da CLI das Funções do Azure adicionais na [Documentação das Funções do Azure](../functions-cli-samples.md).

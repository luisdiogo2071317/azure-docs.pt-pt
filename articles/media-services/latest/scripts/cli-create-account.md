---
title: Criar uma conta dos Serviços de Multimédia do Azure - CLI do Azure | Microsoft Docs
description: Utilize o script da CLI do Azure para criar uma conta dos Serviços de Multimédia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/25/2019
ms.author: juliako
ms.openlocfilehash: 41d9548aa33ee29c40179506ae824ab79b000859
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55213179"
---
# <a name="cli-example-create-an-azure-media-services-account"></a>Exemplo da CLI: Criar uma conta dos Azure Media Services

O script da CLI do Azure neste tópico mostra como criar uma conta dos Serviços de Multimédia do Azure. 

A conta de Media Services e todas as contas de armazenamento associado tem de ser na mesma subscrição do Azure. Recomenda-se para utilizar contas de armazenamento na mesma localização que a conta de Media Services.

[!INCLUDE [media-services-cli-instructions.md](../../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/media-services-create-account/Create-Account.sh "Create Account")]

## <a name="clean-up-deployment"></a>Limpar a implementação

Execute o seguinte comando para remover o grupo de recursos e todos os recursos associados ao mesmo.

```azurecli
az group delete --name amsResourceGroup
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Cria uma conta de armazenamento. |
| [az ams account create](https://docs.microsoft.com/cli/azure/ams/account?view=azure-cli-latest#az-ams-account-create) | Criar uma conta dos Serviços de Multimédia. |
| [az ams account sp create](https://docs.microsoft.com/cli/azure/ams/account/sp?view=azure-cli-latest#az-ams-account-sp-create) | Cria um principal de serviço com palavra-passe e configura o acesso a uma conta dos Serviços de Multimédia do Azure. 
| [az group delete](/cli/azure/group#az-group-delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |


## <a name="next-steps"></a>Passos Seguintes

Para obter mais exemplos, veja [Azure CLI samples](../cli-samples.md) (Exemplos de CLI do Azure).

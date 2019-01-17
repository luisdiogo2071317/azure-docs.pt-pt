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
ms.date: 01/15/2019
ms.author: juliako
ms.openlocfilehash: 2eeb47c2e0f96eca1ca9b852a2be6ca3102ba71e
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2019
ms.locfileid: "54353350"
---
# <a name="cli-example-create-an-azure-media-services-account"></a>Exemplo da CLI: Criar uma conta dos Azure Media Services

O script da CLI do Azure neste tópico mostra como criar uma conta dos Serviços de Multimédia do Azure. Conta de Media Services e a conta de armazenamento associado ao mesmo têm de ser parte do mesmo centro de dados e o mesmo grupo de recursos.

## <a name="prerequisites"></a>Pré-requisitos 

Instalar e utilizar a CLI localmente, este artigo requer a versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli). 

Atualmente, nem todos [dos serviços de multimédia v3 CLI](https://aka.ms/ams-v3-cli-ref) comandos trabalham no Azure Cloud Shell. Recomenda-se para utilizar a CLI localmente.

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

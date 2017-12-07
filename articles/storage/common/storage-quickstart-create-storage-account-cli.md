---
title: "Início Rápido do Azure – Criar uma conta de armazenamento com a CLI do Azure | Microsoft Docs"
description: Aprenda a criar rapidamente uma nova conta de armazenamento com a CLI do Azure.
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/28/2017
ms.author: tamram
ms.openlocfilehash: d0f9f487226218e6a8409c60edd7ef39ce760386
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2017
---
# <a name="create-a-storage-account-using-the-azure-cli"></a>Criar uma conta de armazenamento com a CLI do Azure

A CLI do Azure é utilizada para criar e gerir recursos do Azure a partir da linha de comandos ou em scripts. Este Início Rápido fornece detalhes sobre a utilização da CLI do Azure para criar uma conta de Armazenamento do Azure.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0.4 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="create-resource-group"></a>Criar grupo de recursos

Crie um grupo de recursos do Azure com o comando [az group create](/cli/azure/group#create). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos. Este exemplo cria um grupo de recursos com o nome *myResourceGroup* na região *eastus*.

```azurecli-interactive
az group create \
    --name myResourceGroup \
    --location eastus
```

Se não tiver a certeza de qual a região a especificar para o parâmetro `--location`, pode obter uma lista de regiões suportadas para a sua subscrição com o comando [az account list-locations](/cli/azure/account#list).

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

## <a name="create-a-general-purpose-standard-storage-account"></a>Criar uma conta de armazenamento padrão para fins gerais

Existem vários tipos de contas de armazenamento adequadas para cenários de utilização diferentes, em que cada um deles suporta um ou vários dos serviços de armazenamento (blobs, ficheiros, tabelas ou filas). A tabela seguinte fornece detalhes sobre os tipos de contas de armazenamento disponíveis.

|**Tipo de conta de armazenamento**|**Standard para fins gerais**|**Premium para fins gerais**|**Armazenamento de blobs, camadas de acesso frequente e esporádico**|
|-----|-----|-----|-----|
|**Serviços suportados**| Serviços Blob, Ficheiro, Tabela e Fila | Serviço Blob | Serviço Blob|
|**Tipos de blobs suportados**|Blobs de blocos, blobs de páginas e blobs de acréscimo | Blobs de páginas | Blobs de blocos e blobs de acréscimo|

Crie uma conta de armazenamento padrão para fins gerais com o comando [az storage account create](/cli/azure/storage/account#create).

```azurecli-interactive
az storage account create \
    --name mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --sku Standard_LRS \
    --encryption blob
```

## <a name="clean-up-resources"></a>Limpar recursos

Se já não precisa de nenhum dos recursos presentes no seu grupo de recursos, incluindo a conta de armazenamento que criou neste Início Rápido, elimine o grupo de recursos com o comando [az group delete](/cli/azure/group#delete).

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Neste Início Rápido, criou um grupo de recursos e uma conta de armazenamento padrão para fins gerais. Para saber como transferir dados de e para a sua conta de armazenamento, avance para o Início Rápido de armazenamento de Blobs.

> [!div class="nextstepaction"]
> [Transferir objetos de e para o armazenamento de Blobs do Azure com a CLI do Azure](../blobs/storage-quickstart-blobs-cli.md)
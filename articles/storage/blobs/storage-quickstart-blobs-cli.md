---
title: "Início Rápido do Azure – Transferir objetos de/para o armazenamento de Blobs do Azure com a CLI do Azure | Microsoft Docs"
description: Aprenda rapidamente a transferir objetos de/para o armazenamento de Blobs do Azure com a CLI do Azure
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
ms.date: 07/19/2017
ms.author: tamram
ms.openlocfilehash: a300294c83cb206e6211985c736e3ff01bb1ab43
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-the-azure-cli"></a>Transferir objetos de/para o armazenamento de Blobs do Azure com a CLI do Azure

A CLI do Azure é utilizada para criar e gerir recursos do Azure a partir da linha de comandos ou em scripts. Este Início Rápido fornece detalhes sobre a utilização da CLI do Azure para carregar e transferir dados de e para o armazenamento de Blobs do Azure.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0.4 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli).

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Criar um contentor

Os Blobs são sempre carregados para um contentor. Os contentores permitem organizar grupos de blobs, como organiza os ficheiros em diretórios no seu computador.

Crie um contentor para armazenar blobs com o comando [az storage container create](/cli/azure/storage/container#create).

```azurecli-interactive
az storage container create --name mystoragecontainer
```

## <a name="upload-a-blob"></a>Carregar um blob

O armazenamento de blobs suporta blobs de blocos, blobs de acréscimo e blobs de páginas. A maioria dos ficheiros guardados no armazenamento de Blobs é armazenada como blobs de blocos. Os blobs de acréscimo são utilizados quando é preciso adicionar dados a um blob existente sem modificar o conteúdo existente, como é o caso das operações de registo. Os blobs de páginas dão suporte aos ficheiros VHD de máquinas virtuais IaaS.

Neste exemplo, vamos carregar um blob para o contentor criado no último passo com o comando [az storage blob upload](/cli/azure/storage/blob#upload).

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/path/to/local/file
```

Esta operação cria o blob, caso este ainda não exista, ou substitui-o se o mesmo já existir. Carregue os ficheiros que quiser antes de continuar.

## <a name="list-the-blobs-in-a-container"></a>Listar os blobs num contentor

Liste os blobs no contentor com o comando [az storage blob list](/cli/azure/storage/blob#list).

```azurecli-interactive
az storage blob list \
    --container-name mystoragecontainer \
    --output table
```

## <a name="download-a-blob"></a>Transferir um blob

Utilize o comando [az storage blob download](/cli/azure/storage/blob#download) para transferir um blob carregado anteriormente.

```azurecli-interactive
az storage blob download \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/destination/path/for/file
```

## <a name="data-transfer-with-azcopy"></a>Transferência de dados com o AzCopy

O utilitário [AzCopy](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) é outra opção para a transferência de dados de scripts de elevado desempenho para o Armazenamento do Azure. Pode utilizar o AzCopy para transferir dados de e para o armazenamento de Blobs, Ficheiros e Tabelas.

Como exemplo rápido, eis o comando do AzCopy para carregar um ficheiro denominado *myfile.txt* para o contentor *mystoragecontainer*.

```bash
azcopy \
    --source /mnt/myfiles \
    --destination https://mystorageaccount.blob.core.windows.net/mystoragecontainer \
    --dest-key <storage-account-access-key> \
    --include "myfile.txt"
```

## <a name="clean-up-resources"></a>Limpar recursos

Se já não precisa de nenhum dos recursos presentes no seu grupo de recursos, incluindo a conta de armazenamento que criou neste Início Rápido, elimine o grupo de recursos com o comando [az group delete](/cli/azure/group#delete).

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Neste Início Rápido, aprendeu a transferir ficheiros entre o disco local e um contentor no armazenamento de Blobs do Azure. Para saber mais sobre a utilização de blobs no Armazenamento do Azure, avance para o tutorial para utilizar o armazenamento de Blobs do Azure.

> [!div class="nextstepaction"]
> [Procedimentos de operações de armazenamento de Blobs com a CLI do Azure](storage-how-to-use-blobs-cli.md)

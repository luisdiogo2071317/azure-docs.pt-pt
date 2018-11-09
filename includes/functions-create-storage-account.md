---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: cf8c6f07eb38487dd29624b15be3637536be92fc
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50134370"
---
## <a name="create-an-azure-storage-account"></a>Criar uma conta de Armazenamento do Azure

As Funções utilizam uma conta de Armazenamento do Microsoft Azure para fins gerais, para guardar as informações de estado das funções, entre outras. Crie uma conta de armazenamento para fins gerais no grupo de recursos que criou, ao utilizar o comando [az storage account create](/cli/azure/storage/account#create).

No comando seguinte, substitua o nome da conta de armazenamento globalmente único onde vir o marcador de posição `<storage_name>`. Os nomes das contas do Storage devem ter entre 3 e 24 carateres de comprimento e apenas podem conter números e letras minúsculas.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

Depois de a conta de armazenamento ter sido criada, a CLI do Azure mostra informações semelhantes às do exemplo seguinte:

```json
{
  "creationTime": "2017-04-15T17:14:39.320307+00:00",
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myresourcegroup/...",
  "kind": "Storage",
  "location": "westeurope",
  "name": "myfunctionappstorage",
  "primaryEndpoints": {
    "blob": "https://myfunctionappstorage.blob.core.windows.net/",
    "file": "https://myfunctionappstorage.file.core.windows.net/",
    "queue": "https://myfunctionappstorage.queue.core.windows.net/",
    "table": "https://myfunctionappstorage.table.core.windows.net/"
  },
     ....
    // Remaining output has been truncated for readability.
}
```
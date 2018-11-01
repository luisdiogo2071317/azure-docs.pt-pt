---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: d44865dc3189a7f9dc05106baf9f4d120e5e8bf6
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133559"
---
## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az_group_create). Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure, como aplicações Function App, bases de dados e contas de armazenamento, são implementados e geridos.

O exemplo seguinte cria um grupo de recursos com o nome `myResourceGroup`.  
Se não estiver a utilizar o Cloud Shell, inicie sessão primeiro com `az login`.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```
Geralmente, o grupo de recursos e os recursos são criados numa região perto de si. Para ver todas as localizações suportadas para os planos do Serviço de Aplicações, execute o comando [az appservice list-locations](/cli/azure/appservice#az-appservice-list-locations).
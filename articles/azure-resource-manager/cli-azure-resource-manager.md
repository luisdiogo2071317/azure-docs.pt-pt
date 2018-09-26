---
title: Gerir os recursos com a CLI do Azure | Documentos da Microsoft
description: Utilizar a Interface de linha de comandos (CLI do Azure) para gerir recursos do Azure e grupos
editor: ''
manager: timlt
documentationcenter: ''
author: tfitzmac
services: azure-resource-manager
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: conceptual
ms.date: 10/06/2017
ms.author: tomfitz
ms.openlocfilehash: dd111c33cbd348a05ed0f0c04f7325347612e54d
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47106917"
---
# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>Utilizar a CLI do Azure para gerir recursos do Azure e grupos de recursos

Neste artigo, irá aprender a gerir as suas soluções com a CLI do Azure e Azure Resource Manager. Se não estiver familiarizado com o Resource Manager, veja [descrição geral do Gestor de recursos](resource-group-overview.md). Este artigo se concentra em tarefas de gestão. Irá:

1. Criar um grupo de recursos
2. Adicionar um recurso para o grupo de recursos
3. Adicionar uma etiqueta para o recurso
4. Recursos de consulta com base em nomes ou valores de etiqueta
5. Aplicar e remover um bloqueio do recurso
6. Eliminar um grupo de recursos

Este artigo não mostra como implementar um modelo do Resource Manager para a sua subscrição. Para obter essas informações, consulte [implementar recursos com modelos do Resource Manager e a CLI do Azure](resource-group-template-deploy-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Para instalar e utilizar a CLI localmente, consulte [instalar a CLI do Azure](/cli/azure/install-azure-cli).

## <a name="set-subscription"></a>Conjunto de subscrição

Se tiver mais de uma subscrição, pode mudar para uma subscrição diferente. Em primeiro lugar, vamos ver todas as subscrições para a sua conta.

```azurecli-interactive
az account list
```

Devolve uma lista das suas subscrições ativadas e desativadas.

```json
[
  {
    "cloudName": "AzureCloud",
    "id": "<guid>",
    "isDefault": true,
    "name": "Example Subscription One",
    "registeredProviders": [],
    "state": "Enabled",
    "tenantId": "<guid>",
    "user": {
      "name": "example@contoso.org",
      "type": "user"
    }
  },
  ...
]
```

Tenha em atenção que uma subscrição está marcada como predefinição. Esta subscrição tem seu contexto atual para operações. Para mudar para uma subscrição diferente, forneça o nome de subscrição com o **conjunto de conta de az** comando.

```azurecli-interactive
az account set -s "Example Subscription Two"
```

Para mostrar o contexto da subscrição atual, utilize **show de conta de az** sem um parâmetro:

```azurecli-interactive
az account show
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Antes de implementar quaisquer recursos à sua subscrição, tem de criar um grupo de recursos que irá conter os recursos.

Para criar um grupo de recursos, utilize o comando **az group create**. O comando utiliza o **name** parâmetro para especificar um nome para o grupo de recursos e o **localização** parâmetro para especificar a localização do mesmo.

```azurecli-interactive
az group create --name TestRG1 --location "South Central US"
```

O resultado é o seguinte formato:

```json
{
  "id": "/subscriptions/<subscription-id>/resourceGroups/TestRG1",
  "location": "southcentralus",
  "managedBy": null,
  "name": "TestRG1",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

Se precisar de obter o grupo de recursos mais tarde, utilize o seguinte comando:

```azurecli-interactive
az group show --name TestRG1
```

Para obter todos os grupos de recursos na sua subscrição, utilize:

```azurecli-interactive
az group list
```

## <a name="add-resources-to-a-resource-group"></a>Adicionar recursos a um grupo de recursos

Para adicionar um recurso para o grupo de recursos, pode utilizar o **de criação do recurso de az** comando ou um comando específico para o tipo de recurso que está a criar (como **criar conta de armazenamento az**). Poderá considerar mais fácil de usar um comando que é específico para um tipo de recurso, pois inclui parâmetros para as propriedades que são necessários para o novo recurso. Para utilizar **de criação do recurso de az**, deve conhecer todas as propriedades para definir sem inseri-los.

No entanto, adicionar um recurso por meio de script pode causar confusão futura porque o novo recurso não existe num modelo do Resource Manager. Modelos permitem-lhe implementar de forma fiável e repetidamente a sua solução.

O comando seguinte cria uma conta de armazenamento. Em vez de utilizar o nome apresentado no exemplo, forneça um nome exclusivo para a conta de armazenamento. O nome tem de ter entre 3 e 24 carateres de comprimento e utilizar apenas números e letras minúsculas. Se utilizar o nome apresentado no exemplo, recebe um erro porque esse nome já está em utilização.

```azurecli-interactive
az storage account create -n myuniquestorage -g TestRG1 -l westus --sku Standard_LRS
```

Se precisar de obter este recurso mais tarde, utilize o seguinte comando:

```azurecli-interactive
az storage account show --name myuniquestorage --resource-group TestRG1
```

## <a name="add-a-tag"></a>Adicionar uma etiqueta

As etiquetas permitem-lhe organizar os recursos, de acordo com as propriedades diferentes. Por exemplo, pode ter vários recursos em diferentes grupos de recursos que pertencem ao mesmo departamento. Pode aplicar uma etiqueta de departamento e o valor a esses recursos para marcá-los como pertencentes da mesma categoria. Em alternativa, pode marcar se um recurso é utilizado num ambiente de produção ou teste. Neste artigo, aplicar marcas em apenas um recurso, mas no seu ambiente mais provável faz sentido para aplicar as etiquetas para todos os seus recursos.

O comando seguinte aplica-se duas etiquetas para a sua conta de armazenamento:

```azurecli-interactive
az resource tag --tags Dept=IT Environment=Test -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts"
```

As etiquetas são atualizadas como um único objeto. Para adicionar uma etiqueta a um recurso que já inclui etiquetas, primeiro recuperar as etiquetas existentes. Adicione a etiqueta nova para o objeto que contém as etiquetas existentes e volte a aplicar todas as etiquetas para o recurso.

```azurecli-interactive
jsonrtag=$(az resource show -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts"
```

## <a name="search-for-resources"></a>Procurar recursos

Utilize o **lista de recursos de az** comando para obter recursos para condições de pesquisa diferente.

* Para obter um recurso por nome, forneça o **nome** parâmetro:

  ```azurecli-interactive
  az resource list -n myuniquestorage
  ```

* Para obter todos os recursos num grupo de recursos, forneça o **grupo de recursos** parâmetro:

  ```azurecli-interactive
  az resource list --resource-group TestRG1
  ```

* Para obter todos os recursos com um nome de etiqueta e valor, forneça o **marca** parâmetro:

  ```azurecli-interactive
  az resource list --tag Dept=IT
  ```

* Para todos os recursos com um tipo de recurso em particular, forneça o **tipo de recurso** parâmetro:

  ```azurecli-interactive
  az resource list --resource-type "Microsoft.Storage/storageAccounts"
  ```

## <a name="get-resource-id"></a>Obter o ID de recurso

Muitos comandos demorar um ID de recurso como um parâmetro. Para obter o ID de um recurso e o arquivo numa variável, utilize:

```azurecli-interactive
webappID=$(az resource show -g exampleGroup -n exampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
```

## <a name="lock-a-resource"></a>Um recurso de bloqueio

Quando precisa certificar-se de que um recurso crítico é não acidentalmente eliminado ou modificado, aplica um bloqueio ao recurso. Pode especificar um uma **CanNotDelete** ou **só de leitura**.

Para criar ou eliminar bloqueios de gestão, tem de ter acesso ao `Microsoft.Authorization/*` ou `Microsoft.Authorization/locks/*` ações. Das funções incorporadas, apenas o proprietário e o administrador de acesso de utilizador são concedidos dessas ações.

Para aplicar um bloqueio, utilize o seguinte comando:

```azurecli-interactive
az lock create --lock-type CanNotDelete --resource-name myuniquestorage --resource-group TestRG1 --resource-type Microsoft.Storage/storageAccounts --name storagelock
```

Não é possível eliminar o recurso bloqueado no exemplo anterior, até que o bloqueio seja removido. Para remover um bloqueio, utilize:

```azurecli-interactive
az lock delete --name storagelock --resource-group TestRG1 --resource-type Microsoft.Storage/storageAccounts --resource-name myuniquestorage
```

Para obter mais informações sobre bloqueios de definição, consulte [bloquear recursos com o Azure Resource Manager](resource-group-lock-resources.md).

## <a name="remove-resources-or-resource-group"></a>Remover recursos ou grupo de recursos
Pode remover um recurso ou grupo de recursos. Quando remove um grupo de recursos, também remover todos os recursos dentro do grupo de recursos.

* Para eliminar um recurso do grupo de recursos, utilize o comando delete para o tipo de recurso que está a eliminar. O comando elimina o recurso, mas não elimina o grupo de recursos.

  ```azurecli-interactive
  az storage account delete -n myuniquestorage -g TestRG1
  ```

* Para eliminar um grupo de recursos e todos os respetivos recursos, utilize o comando **az group delete**.

  ```azurecli-interactive
  az group delete -n TestRG1
  ```

Para os dois comandos, é-lhe perguntado para confirmar que pretende remover o recurso ou grupo de recursos.

## <a name="next-steps"></a>Passos Seguintes
* Para saber mais sobre a criação de modelos do Resource Manager, veja [criação de modelos do Azure Resource Manager](resource-group-authoring-templates.md).
* Para saber mais sobre a implementação de modelos, veja [implementar uma aplicação com o modelo do Azure Resource Manager](resource-group-template-deploy-cli.md).
* Pode mover os recursos existentes para um novo grupo de recursos. Para obter exemplos, consulte [mover recursos para um novo grupo de recursos ou subscrição](resource-group-move-resources.md).
* Para obter documentação de orientação sobre como as empresas podem utilizar o Resource Manager para gerir subscrições de forma eficaz, consulte [Azure enterprise scaffold - prescriptive subscription governance (Andaime empresarial do Azure - governação de subscrições prescritivas)](/azure/architecture/cloud-adoption-guide/subscription-governance).
---
title: Criar várias instâncias de recursos com o Azure Resource Manager | Microsoft Docs
description: Saiba como criar um modelo do Azure Resource Manager para criar várias instâncias de recursos do Azure.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/18/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 1cfccdf644b1748a96f7638e574c66eace8d113a
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49456665"
---
# <a name="tutorial-create-multiple-resource-instances-using-resource-manager-templates"></a>Tutorial: Criar várias instâncias de recursos com modelos do Resource Manager

Saiba como iterar no seu modelo do Azure Resource Manager para criar várias instâncias de um recurso do Azure. No último tutorial, modificou um modelo existente para criar uma conta de Armazenamento do Microsoft Azure encriptada. Neste tutorial, modificará o mesmo modelo para criar três instâncias de contas de armazenamento.

> [!div class="checklist"]
> * Abrir um modelo de início rápido
> * Editar o modelo
> * Implementar o modelo

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este artigo, precisa de:

* [Visual Studio Code](https://code.visualstudio.com/) com [extensão Ferramentas do Resource Manager](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).

## <a name="open-a-quickstart-template"></a>Abrir um modelo de Início Rápido

O modelo utilizado neste início rápido chama-se [Criar uma conta de armazenamento padrão](https://azure.microsoft.com/resources/templates/101-storage-account-create/). O modelo define um recurso de conta de Armazenamento do Azure.

1. No Visual Studio Code, selecione **Ficheiro**>**Abrir Ficheiro**.
2. em **Nome de ficheiro**, cole o seguinte URL:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Selecione **Abrir** para abrir o ficheiro.
4. Selecione **Ficheiro**>**Guardar Como** para guardar o ficheiro como **azuredeploy.json** no computador local.

## <a name="edit-the-template"></a>Editar o modelo

O objetivo deste tutorial é utilizar a iteração de recursos para criar três contas de armazenamento.  O modelo de exemplo apenas cria uma conta de armazenamento. 

No Visual Studio Code, efetue as seguintes quatro alterações:

![Criar várias instâncias do Azure Resource Manager](./media/resource-manager-tutorial-create-multiple-instances/resource-manager-template-create-multiple-instances.png)

1. Adicione um elemento `copy` à definição do recurso de conta de armazenamento. No elemento de cópia, especifique o número de iterações e um nome para este ciclo. O valor tem de ser um número inteiro positivo e não pode ser mais de 800.
2. A função `copyIndex()` devolve a iteração atual no ciclo. `copyIndex()` é baseado em zero. Para deslocar o valor de índice, pode passar um valor na função copyIndex(). Por exemplo, *copyIndex(1)*.
3. Elimine o elemento **variables**, dado que já não é utilizado.
4. Elimine o elemento **outputs**.

O modelo completo assemelha-se a:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "apiVersion": "2018-02-01",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": 3
      }
    }
  ]
}
```

Para obter mais informações sobre a criação de várias instâncias, veja [Implementar várias instâncias de um recurso ou propriedade nos Modelos do Azure Resource Manager](./resource-group-create-multiple.md)

## <a name="deploy-the-template"></a>Implementar o modelo

Veja a secção [Implementar o modelo](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#deploy-the-template) no guia de introdução do Visual Studio Code para o procedimento de implementação.

Para listar as três contas de armazenamento, omita o parâmetro --name:

# <a name="clitabcli"></a>[CLI](#tab/CLI)
```azurecli
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az storage account list --resource-group $resourceGroupName
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName
```

---

Compare os nomes de contas de armazenamento com a definição de nome no modelo.

## <a name="clean-up-resources"></a>Limpar recursos

Quando os recursos do Azure já não forem necessários, limpe os recursos implementados ao eliminar o grupo de recursos.

1. No portal do Azure, selecione **Grupo de recursos** no menu à esquerda.
2. Introduza o nome do grupo de recursos no campo **Filtrar por nome**.
3. Selecione o nome do grupo de recursos.  Verá um total de seis recursos no grupo de recursos.
4. Selecione **Eliminar grupo de recursos** no menu superior.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a criar várias instâncias de contas de armazenamento. Até agora, criou uma conta de armazenamento ou várias instâncias de contas de armazenamento. No próximo tutorial, vai desenvolver um modelo com vários recursos e vários tipos de recurso. Alguns dos recursos têm recursos dependentes.

> [!div class="nextstepaction"]
> [Criar recursos dependentes](./resource-manager-tutorial-create-templates-with-dependent-resources.md)

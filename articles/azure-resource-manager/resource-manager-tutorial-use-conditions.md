---
title: Condição de utilização dos modelos do Azure Resource Manager | Microsoft Docs
description: Saiba como implementar recursos do Azure com base em condições.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/02/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 216e474f519e57352b017dc3e6bcdd74d48b03de
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48238651"
---
# <a name="tutorial-use-condition-in-azure-resource-manager-templates"></a>Tutorial: Condição de utilização nos modelos do Azure Resource Manager

Saiba como implementar recursos do Azure com base em condições. 

O cenário utilizado neste tutorial é semelhante ao utilizado no [Tutorial: Criar modelos do Azure Resource Manager com recursos dependentes](./resource-manager-tutorial-create-templates-with-dependent-resources.md). Nesse tutorial, cria uma conta de armazenamento, uma máquina virtual, uma rede virtual e alguns outros recursos dependentes. Em vez de criar uma nova conta de armazenamento, permite que as pessoas optem entre criar uma nova conta de armazenamento e utilizar uma conta de armazenamento existente. Para alcançar este objetivo, tem de definir um parâmetro adicional. Se o valor do parâmetro for "new" (nova), é criada uma nova conta de armazenamento.

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Abrir um modelo de início rápido
> * Modificar o modelo
> * Implementar o modelo
> * Limpar recursos

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este artigo, precisa de:

* [Visual Studio Code](https://code.visualstudio.com/) com [extensão Ferramentas do Resource Manager](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)

## <a name="open-a-quickstart-template"></a>Abrir um modelo de Início Rápido

Os Modelos de Início Rápido do Azure são um repositório de modelos do Resource Manager. Em vez de criar um modelo do zero, pode encontrar um modelo de exemplo e personalizá-lo. O modelo utilizado neste tutorial é denominado [Implementar uma VM do Windows simples](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/).

1. No Visual Studio Code, selecione **Ficheiro**>**Abrir Ficheiro**.
2. em **Nome de ficheiro**, cole o seguinte URL:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Selecione **Abrir** para abrir o ficheiro.
4. Selecione **Ficheiro**>**Guardar Como** para guardar uma cópia do ficheiro no computador local, com o nome **azuredeploy.json**.

## <a name="modify-the-template"></a>Modificar o modelo

Introduza duas alterações ao modelo existente:

* Adicione um parâmetro utilizado para fornecer um nome de conta de armazenamento. Este parâmetro permite que o utilizador opte por especificar um nome de conta de armazenamento existente. Também pode ser utilizado como o novo nome da conta de armazenamento.
* Adicionar um novo parâmetro denominado **newOrExisting**. A implementação utiliza este parâmetro para determinar onde criar uma conta de armazenamento nova ou para utilizar uma conta de armazenamento existente.

1. Abra **azuredeploy. JSON** no Visual Studio Code.
2. Substitua as **variáveis ('storageAccountName')** pelos **parâmetros ('storageAccountName')** em todo o modelo.  Existem três aspetos de **variáveis ('storageAccountName')**.
3. Remova a definição de variável seguinte:

    ```json
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'sawinvm')]",
    ```
4. Adicione os dois parâmetros seguintes ao modelo:

    ```json
    "newOrExisting": {
      "type": "string"
    },
    "storageAccountName": {
      "type": "string"
    },
    ```
    A definição de parâmetros atualizada assemelha-se a:

    ![Condição de utilização do Resource Manager](./media/resource-manager-tutorial-use-conditions/resource-manager-tutorial-use-condition-template-parameters.png)

5. Adicione a linha seguinte ao início da definição de conta de armazenamento.

    ```json
    "condition": "[equals(parameters('newOrExisting'),'yes')]",
    ```

    A condição verifica o valor de um parâmetro denominado **newOrExisting**. Se o valor do parâmetro for **New** (novo), a implementação cria a conta de armazenamento.

    A definição de conta de armazenamento atualizada assemelha-se a:

    ![Condição de utilização do Resource Manager](./media/resource-manager-tutorial-use-conditions/resource-manager-tutorial-use-condition-template.png)

6. Guarde as alterações.

## <a name="deploy-the-template"></a>Implementar o modelo

Siga as instruções de [Implementar o modelo](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) para implementar o modelo.

Quando implementa o modelo com o Azure PowerShell, tem de especificar um parâmetro adicional:

```powershell
$resourceGroupName = "<Enter the resource group name>"
$storageAccountName = "Enter the storage account name>"
$location = "<Enter the Azure location>"
$vmAdmin = "<Enter the admin username>"
$vmPassword = "<Enter the password>"
$dnsLabelPrefix = "<Enter the prefix>"

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
$vmPW = ConvertTo-SecureString -String $vmPassword -AsPlainText -Force
New-AzureRmResourceGroupDeployment -Name mydeployment0710 -ResourceGroupName $resourceGroupName `
    -TemplateFile azuredeploy.json -adminUsername $vmAdmin -adminPassword $vmPW `
    -dnsLabelPrefix $dnsLabelPrefix -storageAccountName $storageAccountName -newOrExisting "new"
```

> [!NOTE]
> A implementação falhar se **newOrExisting** for **New** (Novo), mas a conta de armazenamento com o nome de conta de armazenamento especificado já existe.

Tente fazer outra implementação com **newOrExisting** definido como "existing" (existente) e especifique uma conta de armazenamento existente. Para criar uma conta do armazenamento antecipadamente, consulte [Criar uma conta de armazenamento](../storage/common/storage-quickstart-create-account.md).

## <a name="clean-up-resources"></a>Limpar recursos

Quando os recursos do Azure já não forem necessários, limpe os recursos implementados ao eliminar o grupo de recursos.

1. No portal do Azure, selecione **Grupo de recursos** no menu à esquerda.
2. Introduza o nome do grupo de recursos no campo **Filtrar por nome**.
3. Selecione o nome do grupo de recursos.  Verá um total de seis recursos no grupo de recursos.
4. Selecione **Eliminar grupo de recursos** no menu superior.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, vai desenvolver um modelo que permite que os utilizadores optem entre criar uma conta de armazenamento nova e utilizar uma conta de armazenamento existente. A máquina virtual criada neste tutorial requer um nome de utilizador de administrador e a palavra-passe. Em vez de transmitir a palavra-passe durante a implementação, pode armazenar previamente a palavra-passe com o Azure Key Vault e obtê-la durante a implementação. Para saber como obter segredos a partir do Azure Key Vault e utilizá-los na implementação do modelo, consulte:

> [!div class="nextstepaction"]
> [Integrar o Key Vault na implementação do modelo](./resource-manager-tutorial-use-key-vault.md)

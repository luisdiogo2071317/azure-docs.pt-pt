---
title: Criar modelos ligados do Azure Resource Manager | Microsoft Docs
description: Saiba como criar modelos ligados do Azure Resource Manager para criar máquinas virtuais
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 11/13/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: dfdad89d628fda476ecef1c43246ce3927927555
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52863504"
---
# <a name="tutorial-create-linked-azure-resource-manager-templates"></a>Tutorial: Criar modelos ligados do Azure Resource Manager

Saiba como criar modelos ligados do Azure Resource Manager. Com os modelos ligados, pode ter um modelo para chamar outro modelo. É ótimo para modelos de modulação. Neste tutorial, vai utilizar o mesmo modelo utilizado no [Tutorial: Criar várias instâncias de recursos com modelos do Resource Manager](./resource-manager-tutorial-create-multiple-instances.md), o qual cria uma máquina virtual, uma rede virtual e outro recurso dependente, incluindo uma conta de armazenamento. Vai separar o recurso de conta de armazenamento para um modelo ligado.

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Abrir um modelo de Início rápido
> * Criar o modelo ligado
> * Carregar o modelo ligado
> * Ligar ao modelo ligado
> * Configurar a dependência
> * Implementar o modelo

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este artigo, precisa de:

* [Visual Studio Code](https://code.visualstudio.com/) com [extensão Ferramentas do Resource Manager](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Para aumentar a segurança, utilize uma palavra-passe gerada para a conta de administrador da máquina virtual. Eis um exemplo para gerar uma palavra-passe:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    O Azure Key Vault foi criado para salvaguardar chaves criptográficos e outros segredos. Para obter mais informações, veja [Tutorial: Integrar o Azure Key Vault na implementação de modelos do Resource Manager](./resource-manager-tutorial-use-key-vault.md). Também recomendamos que atualize a palavra-passe a cada três meses.

## <a name="open-a-quickstart-template"></a>Abrir um modelo de Início Rápido

Os Modelos de Início Rápido do Azure são um repositório de modelos do Resource Manager. Em vez de criar um modelo do zero, pode encontrar um modelo de exemplo e personalizá-lo. O modelo utilizado neste tutorial é denominado [Implementar uma VM do Windows simples](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/). Este é o mesmo modelo utilizado no [Tutorial: Criar várias instâncias de recursos com modelos do Resource Manager](./resource-manager-tutorial-create-multiple-instances.md). Guarde duas cópias do mesmo modelo para serem utilizadas como:

* **O modelo principal**: crie todos os recursos exceto a conta de armazenamento.
* **O modelo ligado**: crie a conta de armazenamento.

1. No Visual Studio Code, selecione **Ficheiro**>**Abrir Ficheiro**.
2. em **Nome de ficheiro**, cole o seguinte URL:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Selecione **Abrir** para abrir o ficheiro.
4. Existem cinco recursos definidos pelo modelo:

    * `Microsoft.Storage/storageAccounts`. Veja a [referência do modelo](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts). 
    * `Microsoft.Network/publicIPAddresses`. Veja a [referência do modelo](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses). 
    * `Microsoft.Network/virtualNetworks`. Veja a [referência do modelo](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks). 
    * `Microsoft.Network/networkInterfaces`. Veja a [referência do modelo](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces). 
    * `Microsoft.Compute/virtualMachines`. Veja a [referência do modelo](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

    É útil ter alguma compreensão básica do modelo antes de personalizá-lo.
5. Selecione **Ficheiro**>**Guardar Como** para guardar uma cópia do ficheiro no computador local, com o nome **azuredeploy.json**.
6. Selecione **Ficheiro**>**Guardar Como** para criar outra cópia do ficheiro com o nome **linkedTemplate.json**.

## <a name="create-the-linked-template"></a>Criar o modelo ligado

O modelo ligado cria uma conta de armazenamento. O modelo ligado é quase idêntico ao modelo autónomo que cria uma conta de armazenamento. Neste tutorial, o modelo ligado tem de passar um valor de volta ao modelo principal. Este valor é definido no elemento `outputs`.

1. Abra linkedTemplate.json no Visual Studio Code, se não estiver aberto.
2. Efetue as seguintes alterações:

    * Remova todos os recursos exceto a conta de armazenamento. Deverá remover um total de quatro recursos.
    * Atualize o elemento **outputs**, para que este se assemelhe a:

        ```json
        "outputs": {
            "storageUri": {
                "type": "string",
                "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
              }
        }
        ```
        O **storageUri** é necessário para a definição do recurso de máquina virtual no modelo principal.  Passe o valor de volta ao modelo principal como um valor de resultado.
    * Remova os parâmetros que nunca foram utilizados. Estes parâmetros têm uma linha verde ondulada por baixo. Só terá um parâmetro restante, chamado **location**.
    * Remova o elemento **variables**. Já não são necessários neste tutorial.
    * Adicione um parâmetro denominado **storageAccountName**. O nome da conta de armazenamento é passado do modelo principal para o modelo ligado como um parâmetro.

    Quando tiver terminado, o modelo terá o seguinte aspeto:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountName":{
            "type": "string",
            "metadata": {
              "description": "Azure Storage account name."
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
            "name": "[parameters('storageAccountName')]",
            "apiVersion": "2016-01-01",
            "location": "[parameters('location')]",
            "sku": {
              "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
          }
        ],
        "outputs": {
            "storageUri": {
                "type": "string",
                "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
              }
        }
    }
    ```
3. Guarde as alterações.

## <a name="upload-the-linked-template"></a>Carregar o modelo ligado

Os modelos têm de ser acessíveis de onde executa a implementação. Esta localização pode ser uma conta de armazenamento do Azure, o Github ou o Dropbox. Se os modelos contiverem informações confidenciais, confirme que protege o acesso às mesmas. Neste tutorial, vai utilizar o método de implementação do Cloud Shell que utilizou no [Tutorial: Criar várias instâncias de recursos com modelos do Resource Manager](./resource-manager-tutorial-create-multiple-instances.md). O modelo principal (azuredeploy.json) é carregado para o shell. O modelo ligado (linkedTemplate.json) tem de ser partilhado em alguma localização.  Para reduzir as tarefas deste tutorial, o modelo ligado definido na secção anterior foi carregado para [uma conta de armazenamento do Azure](https://armtutorials.blob.core.windows.net/linkedtemplates/linkedStorageAccount.json).

## <a name="call-the-linked-template"></a>Chamar o modelo ligado

O modelo principal chama-se azuredeploy.json.

1. Abra azuredeploy.json no Visual Studio Code, se não estiver aberto.
2. Elimine a definição do recurso da conta de armazenamento do modelo.
3. Adicione o seguinte fragmento json ao local em que tinha a definição de conta de armazenamento:

    ```json
    {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri":"https://armtutorials.blob.core.windows.net/linkedtemplates/linkedStorageAccount.json"
          },
          "parameters": {
              "storageAccountName":{"value": "[variables('storageAccountName')]"},
              "location":{"value": "[parameters('location')]"}
          }
      }
    },
    ```

    Preste atenção a estes detalhes:

    * Um recurso `Microsoft.Resources/deployments` no modelo principal é utilizado para ligar a outro modelo.
    * O recurso `deployments` tem um nome chamado `linkedTemplate`. Este nome é utilizado para [configurar a dependência](#configure-dependency).  
    * Só pode utilizar o modo de implementação [Incremental](./deployment-modes.md) ao chamar modelos ligados.
    * `templateLink/uri` contém o URI do modelo ligado. O modelo ligado foi carregado para uma conta de armazenamento partilhada. Poderá atualizar o URI se carregar o modelo para outra localização na Internet.
    * Utilize `parameters` para passar os valores do modelo principal para o modelo ligado.
4. Guarde as alterações.

## <a name="configure-dependency"></a>Configurar a dependência

Como vimos no [Tutorial: Criar várias instâncias de recursos com o modelo do Resource Manager](./resource-manager-tutorial-create-multiple-instances.md), o recurso de máquina virtual depende da conta de armazenamento:

![Diagrama de dependência de modelos do Azure Resource Manager](./media/resource-manager-tutorial-create-linked-templates/resource-manager-template-visual-studio-code-dependency-diagram.png)

Uma vez que agora a conta de armazenamento está definida no modelo ligado, tem de atualizar os dois elementos seguintes do recurso `Microsoft.Compute/virtualMachines`.

* Reconfigure o elemento `dependOn`. A definição da conta de armazenamento é movida para o modelo ligado.
* Reconfigure o elemento `properties/diagnosticsProfile/bootDiagnostics/storageUri`. Em [Criar o modelo ligado](#create-the-linked-template), adicionou um valor de resultado:

    ```json
    "outputs": {
        "storageUri": {
            "type": "string",
            "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
            }
    }
    ```
    O modelo principal requer este valor.

1. Abra azuredeploy.json no Visual Studio Code, se não estiver aberto.
2. Expanda a definição do recurso de máquina virtual e atualize **dependsOn**, conforme mostrado na captura de ecrã seguinte:

    ![Dependência de configuração de modelos ligados do Azure Resource Manager ](./media/resource-manager-tutorial-create-linked-templates/resource-manager-template-linked-templates-configure-dependency.png)

    *linkedTemplate* é o nome do recurso de implementações.  
3. Atualize **properties/diagnosticsProfile/bootDiagnostics/storageUri**, conforme mostrado na anterior captura de ecrã.

Para obter mais informações, veja [Utilizar modelos ligados e aninhados ao implementar recursos do Azure](./resource-group-linked-templates.md)

## <a name="deploy-the-template"></a>Implementar o modelo

Veja a secção [Implementar o modelo](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) para obter o procedimento de implementação. Para aumentar a segurança, utilize uma palavra-passe gerada para a conta de administrador da máquina virtual. Veja [Pré-requisitos](#prerequisites).

## <a name="clean-up-resources"></a>Limpar recursos

Quando os recursos do Azure já não forem necessários, limpe os recursos implementados ao eliminar o grupo de recursos.

1. No portal do Azure, selecione **Grupo de recursos** no menu à esquerda.
2. Introduza o nome do grupo de recursos no campo **Filtrar por nome**.
3. Selecione o nome do grupo de recursos.  Verá um total de seis recursos no grupo de recursos.
4. Selecione **Eliminar grupo de recursos** no menu superior.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, desenvolveu e implementou um modelo ligado. Para saber como utilizar extensões de máquina virtual para efetuar tarefas pós-implementação, veja

> [!div class="nextstepaction"]
> [Implementar extensões de máquina virtual](./deployment-manager-tutorial.md)

---
title: Criar modelos do Azure Resource Manager com recursos dependentes | Microsoft Docs
description: Saiba como criar um modelo do Azure Resource Manager com vários recursos e como implementá-lo com o portal do Azure
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 09/07/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: fe6313c059a1dd1050240ead5f7ca8e3e1512aa6
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/01/2018
ms.locfileid: "47584518"
---
# <a name="tutorial-create-azure-resource-manager-templates-with-dependent-resources"></a>Tutorial: Criar modelos do Azure Resource Manager com recursos dependentes

Saiba como criar um modelo do Azure Resource Manager para implementar vários recursos.  Depois de criar o modelo, implemente o modelo com o Cloud Shell do portal do Azure.

Neste tutorial, vai criar uma conta de armazenamento, uma máquina virtual, uma rede virtual e mais alguns recursos dependentes. Alguns dos recursos não podem ser implementados enquanto existir outro recurso. Por exemplo, não é possível criar a máquina virtual enquanto as respetivas conta de armazenamento e interface de rede existirem. Defina esta relação fazendo com que um recurso seja dependente dos outros recursos. O Resource Manager avalia as dependências entre os recursos e implementa-os por ordem dependente. Quando os recursos não são dependentes entre si, o Resource Manager implementa-os em paralelo. Para obter mais informações, veja [Definir a ordem para implementar recursos nos Modelos do Azure Resource Manager](./resource-group-define-dependencies.md).

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Abrir um modelo de início rápido
> * Explorar o modelo
> * Implementar o modelo

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este artigo, precisa de:

* [Visual Studio Code](https://code.visualstudio.com/).
* Extensão das Ferramentas do Azure Resource Manager.  Veja [Instalar a extensão ](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)

## <a name="open-a-quickstart-template"></a>Abrir um modelo de Início Rápido

Os Modelos de Início Rápido do Azure são um repositório de modelos do Resource Manager. Em vez de criar um modelo do zero, pode encontrar um modelo de exemplo e personalizá-lo. O modelo utilizado neste tutorial é denominado [Implementar uma VM do Windows simples](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/).

1. No Visual Studio Code, selecione **Ficheiro**>**Abrir Ficheiro**.
2. em **Nome de ficheiro**, cole o seguinte URL:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Selecione **Abrir** para abrir o ficheiro.
4. Selecione **Ficheiro**>**Guardar Como** para guardar uma cópia do ficheiro no computador local, com o nome **azuredeploy.json**.

## <a name="explore-the-template"></a>Explorar o modelo

Ao explorar o modelo nesta secção, tente responder a estas perguntas:

- Número de recursos do Azure definidos neste modelo?
- Um dos recursos é uma conta de armazenamento do Azure.  A definição aparenta ser como a utilizada no último tutorial?
- Pode encontrar as referências de modelo para os recursos definidos neste modelo?
- Pode encontrar as dependências dos recursos?

1. No Visual Studio Code, feche os elementos até ver apenas os elementos de primeiro nível e os elementos de segundo nível dentro de **recursos**:

    ![Modelos do Azure Resource Manager no Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code.png)

    Existem cinco recursos definidos pelo modelo.
2. Expanda o primeiro recurso. É uma conta de armazenamento. A definição deve ser idêntica à usada no início do último tutorial.

    ![Definição da conta de armazenamento de modelos do Azure Resource Manager no Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-storage-account-definition.png)

3. Expanda o segundo recurso. O tipo de recurso é **Microsoft.Network/publicIPAddresses**. Para localizar a referência de modelo, navegue para [referência de modelo](https://docs.microsoft.com/azure/templates/), introduza **endereço ip público** ou **endereços ip públicos** no campo **Filtrar por título**. Compare a definição do recurso para a referência do modelo.

    ![Definição do endereço IP público de modelos do Azure Resource Manager no Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-public-ip-address-definition.png)
4. Repita o último passo para encontrar as referências de modelo para outros recursos definidos neste modelo.  Compare as definições do recurso para as referências.
5. Expanda o quarto recurso:

    ![dependson de modelos do Azure Resource Manager no Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependson.png)

    O elemento dependsOn permite-lhe definir um recurso como sendo dependente de um ou mais recursos. Neste exemplo, este recurso é um networkInterface.  Depende de dois outros recursos:

    * publicIPAddress
    * virtualNetwork

6. Expanda o quinto recurso. Este recurso é uma máquina virtual. Depende de dois outros recursos:

    * storageAccount
    * networkInterface

O diagrama seguinte ilustra os recursos e as informações de dependência para este modelo:

![Diagrama de dependência de modelos do Azure Resource Manager no Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependency-diagram.png)

Ao especificar as dependências, o Resource Manager implementa a solução de forma eficiente. Implementa a conta de armazenamento, o endereço IP público e a rede virtual em paralelo porque não têm dependências. Depois de o endereço IP público e a rede virtual serem implementados, a interface de rede é criada. Quando todos os outros recursos estiverem implementados, o Resource Manager implementa a máquina virtual.

## <a name="deploy-the-template"></a>Implementar o modelo

Existem muitos métodos para implementar modelos.  Neste tutorial, vai utilizar o Cloud Shell do portal do Azure.

1. Inicie sessão no [portal do Azure](https://portal.azure.com)
2. Selecione **Cloud Shell** no canto superior direito, conforme mostrado na imagem seguinte:

    ![Cloud Shell do portal do Azure](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell.png)
3. Selecione **PowerShell** no canto superior esquerdo do Cloud Shell.  Vai utilizar o PowerShell neste tutorial.
4. Selecione **Reiniciar**
5. Selecione **Carregar ficheiro** do Cloud Shell:

    ![Carregar ficheiro Cloud Shell do Portal do Azure](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-upload-file.png)
6. Selecione o ficheiro que guardou anteriormente no tutorial. O nome predefinido é **azuredeploy.json**.  Se tiver um ficheiro com o mesmo nome de ficheiro, o ficheiro antigo será substituído sem qualquer notificação.
7. No Cloud Shell, execute o seguinte comando para certificar-se de que o ficheiro foi carregado com êxito. 

    ```shell
    ls
    ```

    ![Ficheiro de lista de Cloud Shell do portal do Azure](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-list-file.png)

    O nome do ficheiro mostrado na captura de ecrã é azuredeploy.json.

8. No Cloud Shell, execute o seguinte comando para verificar o conteúdo do ficheiro JSON:

    ```shell
    cat azuredeploy.json
    ```
9. No Cloud Shell, execute os seguintes comandos do PowerShell:

    ```powershell
    $resourceGroupName = "<Enter the resource group name>"
    $location = "<Enter the Azure location>"
    $vmAdmin = "<Enter the admin username>"
    $vmPassword = "<Enter the password>"
    $dnsLabelPrefix = "<Enter the prefix>"

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    $vmPW = ConvertTo-SecureString -String $vmPassword -AsPlainText -Force
    New-AzureRmResourceGroupDeployment -Name mydeployment0710 -ResourceGroupName $resourceGroupName `
        -TemplateFile azuredeploy.json -adminUsername $vmAdmin -adminPassword $vmPW `
        -dnsLabelPrefix $dnsLabelPrefix
    ```
    Segue-se a captura de ecrã de uma implementação de exemplo:

    ![Modelo de implementação de Cloud Shell no portal do Azure](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-deploy-template.png)

    Na captura de ecrã, utilizam-se estes valores:

    * **$resourceGroupName**: myresourcegroup0710. 
    * **$location**: eastus2
    * **&lt;DeployName>**: mydeployment0710
    * **&lt;TemplateFile>**: azuredeploy.json
    * **Parâmetro de modelo**s:

        * **adminUsername**: JohnDole
        * **adminPassword**: Pass@word123
        * **dnsLabelPrefix**: myvm0710

10. Execute o seguinte comando do PowerShell para listar a máquina virtual recentemente criada:

    ```powershell
    Get-AzureRmVM -Name SimpleWinVM -ResourceGroupName <ResourceGroupName>
    ```

    O nome da máquina virtual está hard-coded como **SimpleWinVM** dentro do modelo.

## <a name="clean-up-resources"></a>Limpar recursos

Quando os recursos do Azure já não forem necessários, limpe os recursos implementados ao eliminar o grupo de recursos.

1. No portal do Azure, selecione **Grupo de recursos** no menu à esquerda.
2. Introduza o nome do grupo de recursos no campo **Filtrar por nome**.
3. Selecione o nome do grupo de recursos.  Verá um total de seis recursos no grupo de recursos.
4. Selecione **Eliminar grupo de recursos** no menu superior.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, vai desenvolver e implementar um modelo para criar uma máquina virtual, uma rede virtual e os recursos dependentes. Para saber como implementar recursos do Azure com base em condições, veja:


> [!div class="nextstepaction"]
> [Condições de utilização](./resource-manager-tutorial-use-conditions.md)


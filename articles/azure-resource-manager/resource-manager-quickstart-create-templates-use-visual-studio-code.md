---
title: Utilizar o Visual Studio Code para criar modelos do Azure Resource Manager | Microsoft Docs
description: Utilize o Visual Studio Code e a extensão Azure Resource Manager Tools para trabalhar em modelos do Resource Manager.
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
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: 092b6f2c3267a2c2cd2cc6304133134825bb7261
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50230156"
---
# <a name="quickstart-create-azure-resource-manager-templates-by-using-visual-studio-code"></a>Início Rápido: Criar modelos do Azure Resource Manager com o Visual Studio Code

Aprenda a utilizar o código do Visual Studio e a extensão das Ferramentas do Azure Resource Manager para criar e editar modelos do Azure Resource Manager. Pode criar modelos do Resource Manager no Visual Studio Code sem a extensão, mas esta proporciona opções de conclusão automática que simplificam o desenvolvimento dos modelos. Para compreender os conceitos associados à implementação e gestão das suas soluções do Azure, veja [Descrição geral do Azure Resource Manager](resource-group-overview.md).

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este artigo, precisa de:

- [Visual Studio Code](https://code.visualstudio.com/).
- Extensão das Ferramentas do Azure Resource Manager. Para instalar, siga estes passos:

    1. Abra o Visual Studio Code.
    2. Prima **CTRL+SHIFT+X** para abrir o painel Extensões
    3. Procure **Azure Resource Manager Tools** e, em seguida, selecione **Instalar**.
    4. Para concluir a instalação da extensão, selecione **Recarregar**.

## <a name="open-a-quickstart-template"></a>Abrir um modelo de Início Rápido

Em vez de criar um modelo de raiz, pode abrir um modelo dos [Modelos de Início Rápido do Azure](https://azure.microsoft.com/resources/templates/). Os Modelos de Início Rápido do Azure são um repositório de modelos do Resource Manager.

O modelo utilizado neste início rápido chama-se [Criar uma conta de armazenamento padrão](https://azure.microsoft.com/resources/templates/101-storage-account-create/). O modelo define um recurso de conta de Armazenamento do Azure.

1. No Visual Studio Code, selecione **Ficheiro**>**Abrir Ficheiro**.
2. em **Nome de ficheiro**, cole o seguinte URL:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Selecione **Abrir** para abrir o ficheiro.
4. Selecione **Ficheiro**>**Guardar Como** para guardar o ficheiro como **azuredeploy.json** no computador local.

## <a name="edit-the-template"></a>Editar o modelo

Para saber como editar um modelo com o Visual Studio Code, adicione mais um elemento para a secção `outputs`.

1. Adicione mais um resultado ao modelo exportado:

    ```json
    "storageUri": {
      "type": "string",
      "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
    }
    ```

    Quando concluir, o ecrã de resultado terá este aspeto:

    ```json
    "outputs": {
      "storageAccountName": {
        "type": "string",
        "value": "[variables('storageAccountName')]"
      },
      "storageUri": {
        "type": "string",
        "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
      }
    }
    ```

    Se tiver copiado e colado o código no Visual Studio Code, tente voltar a escrever o elemento **valor** para experimentar a capacidade IntelliSense da extensão das Ferramentas do Resource Manager.

    ![IntelliSense Visual Studio Code de modelo do Resource Manager](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/resource-manager-templates-visual-studio-code-intellisense.png)

2. Selecione **Ficheiro**>**Guardar** para guardar o ficheiro.

## <a name="deploy-the-template"></a>Implementar o modelo

Existem muitos métodos para implementar modelos.  Neste início rápido, vai utilizar o Cloud shell do Azure. O Cloud Shell suporta a CLI do Azure e o Azure PowerShell.

1. Iniciar sessão no [Cloud shell do Azure](https://shell.azure.com)

    ![CLI Cloud Shell do portal do Azure](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-choose-cli.png)
2. No canto superior esquerdo do Cloud Shell, aparece o **PowerShell** ou o **Bash**. Para utilizar a CLI, terá de abrir uma sessão do Bash. Para executar o PowerShell, terá de abrir uma sessão do PowerShell. Selecione a seta para baixo para alternar entre o Bash e o PowerShell. Veja a captura de ecrã anterior. É necessário reiniciar o Shell quando mudar.
3. Selecione **Carregar/transferir ficheiros** e, em seguida, selecione **Carregar**.

    # <a name="clitabcli"></a>[CLI](#tab/CLI)

    ![Carregar ficheiro Cloud Shell do Portal do Azure](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file.png)
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ![Carregar ficheiro Cloud Shell do Portal do Azure](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file-powershell.png)
    
    ---

    Tem de carregar o ficheiro de modelo antes de poder implementá-lo a partir do Shell.
5. Selecione o ficheiro que guardou na secção anterior. O nome predefinido é **azuredeploy.json**.
6. A partir do Cloud Shell, execute o comando **ls** para se certificar de que o ficheiro foi carregado com êxito. Pode também utilizar o comando **cat** para verificar o conteúdo do modelo. A imagem seguinte mostra como executar o comando a partir do Bash.  Utilize os mesmos comandos a partir de uma sessão do PowerShell.

    # <a name="clitabcli"></a>[CLI](#tab/CLI)

    ![Ficheiro de lista de Cloud Shell do portal do Azure](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file.png)
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ![Ficheiro de lista de Cloud Shell do portal do Azure](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file-powershell.png)
    
    ---
7. No Cloud Shell, execute os seguintes comandos. Selecione o separador para mostrar o código do PowerShell ou o código da CLI.

    # <a name="clitabcli"></a>[CLI](#tab/CLI)
    ```azurecli
    echo "Enter the Resource Group name:" &&
    read resourceGroupName &&
    echo "Enter the name for this deployment:" &&
    read deploymentName &&
    echo "Enter the location (i.e. centralus):" &&
    read location &&
    az group create --name $resourceGroupName --location $location &&
    az group deployment create --name $deploymentName --resource-group $resourceGroupName --template-file "azuredeploy.json"
    ```
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ```azurepowershell
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    $deploymentName = Read-Host -Prompt "Enter the name for this deployment"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmResourceGroupDeployment -Name $deploymentName -ResourceGroupName $resourceGroupName -TemplateFile "azuredeploy.json"
    ```
    
    ---

    Atualize o nome de ficheiro do modelo, se guardar o ficheiro com um nome diferente de **azuredeploy.json**.

    A captura de ecrã seguinte mostra uma implementação de exemplo:

    # <a name="clitabcli"></a>[CLI](#tab/CLI)

    ![Modelo de implementação de Cloud Shell no portal do Azure](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template.png)
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ![Modelo de implementação de Cloud Shell no portal do Azure](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template-powershell.png)
    
    ---

    O nome da conta de armazenamento e o URL de armazenamento na secção de resultados são destacados na captura de ecrã. Precisa do nome da conta de armazenamento no próximo passo.

7. Execute o seguinte comando do PowerShell ou da CLI para listar a conta de armazenamento acabada de criar:

    # <a name="clitabcli"></a>[CLI](#tab/CLI)
    ```azurecli
    echo "Enter the Resource Group name:" &&
    read resourceGroupName &&
    echo "Enter the Storage Account name:" &&
    read storageAccountName &&
    az storage account show --resource-group $resourceGroupName --name $storageAccountName
    ```
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ```azurepowershell
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    $storageAccountName = Read-Host -Prompt "Enter the Storage Account name"
    Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName
    ```
    
    ---

## <a name="clean-up-resources"></a>Limpar recursos

Quando os recursos do Azure já não forem necessários, limpe os recursos implementados ao eliminar o grupo de recursos.

1. No portal do Azure, selecione **Grupo de recursos** no menu à esquerda.
2. Introduza o nome do grupo de recursos no campo **Filtrar por nome**.
3. Selecione o nome do grupo de recursos.  Verá um total de seis recursos no grupo de recursos.
4. Selecione **Eliminar grupo de recursos** no menu superior.

## <a name="next-steps"></a>Passos seguintes

O foco principal deste início rápido é usar o Visual Studio Code para editar um modelo existente a partir de modelos de Início Rápido do Azure. Também aprendeu como implementar o modelo com a CLI ou o PowerShell a partir do Azure Cloud Shell. Os modelos de Início Rápido do Azure poderão não fornecer-lhe tudo o que precisa. O próximo tutorial mostra-lhe como localizar as informações de referência de modelo para que possa criar uma conta de Armazenamento do Azure encriptada.

> [!div class="nextstepaction"]
> [Criar conta de armazenamento encriptada](./resource-manager-tutorial-create-encrypted-storage-accounts.md)

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
ms.date: 10/09/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 50f1c81f08787181de2fe3a9f6fb97a96a2bd882
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114317"
---
# <a name="tutorial-create-azure-resource-manager-templates-with-dependent-resources"></a>Tutorial: Criar modelos do Azure Resource Manager com recursos dependentes

Saiba como criar um modelo do Azure Resource Manager para implementar vários recursos.  Depois de criar o modelo, implemente o modelo com o Cloud Shell do portal do Azure.

Neste tutorial, vai criar uma conta de armazenamento, uma máquina virtual, uma rede virtual e mais alguns recursos dependentes. Alguns dos recursos não podem ser implementados enquanto existir outro recurso. Por exemplo, não é possível criar a máquina virtual enquanto as respetivas conta de armazenamento e interface de rede existirem. Defina esta relação fazendo com que um recurso seja dependente dos outros recursos. O Resource Manager avalia as dependências entre os recursos e implementa-os por ordem dependente. Quando os recursos não são dependentes entre si, o Resource Manager implementa-os em paralelo. Para obter mais informações, veja [Definir a ordem para implementar recursos nos Modelos do Azure Resource Manager](./resource-group-define-dependencies.md).

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Preparar o Key Vault
> * Abrir um modelo de início rápido
> * Explorar o modelo
> * Editar o ficheiro de parâmetros
> * Implementar o modelo

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este artigo, precisa de:

* [Visual Studio Code](https://code.visualstudio.com/) com a extensão Ferramentas do Resource Manager.  Veja [Instalar a extensão ](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)

## <a name="prepare-key-vault"></a>Preparar o Key Vault

Para impedir um ataque de "password spray", é recomendada a utilização de uma palavra-passe gerada automaticamente para a conta de administrador da máquina virtual e utilizar o Key Vault para armazenar a palavra-passe. O procedimento seguinte cria um Key Vault e um segredo para armazenar a palavra-passe. Também configura as permissões necessárias para a implementação do modelo para aceder ao segredo armazenado no Key Vault. São necessárias políticas de acesso adicionais se o Key Vault estiver numa subscrição do Azure diferente. Para obter detalhes, veja [Utilizar o Azure Key Vault para transmitir valores de parâmetros seguros durante a implementação](./resource-manager-keyvault-parameter.md).

1. Inicie sessão no [Azure Cloud Shell](https://shell.azure.com).
2. Mude para o seu ambiente favorito, seja o **PowerShell** ou o **Bash** no canto superior esquerdo.
3. Execute o comando seguinte do Azure PowerShell ou da CLI do Azure.  

    ```azurecli-interactive
    keyVaultName='<your-unique-vault-name>'
    resourceGroupName='<your-resource-group-name>'
    location='Central US'
    userPrincipalName='<your-email-address-associated-with-your-subscription>'
    
    # Create a resource group
    az group create --name $resourceGroupName --location $location
    
    # Create a Key Vault
    keyVault=$(az keyvault create \
      --name $keyVaultName \
      --resource-group $resourceGroupName \
      --location $location \
      --enabled-for-template-deployment true)
    keyVaultId=$(echo $keyVault | jq -r '.id')
    az keyvault set-policy --upn $userPrincipalName --name $keyVaultName --secret-permissions set delete get list

    # Create a secret
    password=$(openssl rand -base64 32)
    az keyvault secret set --vault-name $keyVaultName --name 'vmAdminPassword' --value $password
    
    # Print the useful property values
    echo "You need the following values for the virtual machine deployment:"
    echo "Resource group name is: $resourceGroupName."
    echo "The admin password is: $password."
    echo "The Key Vault resource ID is: $keyVaultId."
    ```

    ```azurepowershell-interactive
    $keyVaultName = "<your-unique-vault-name>"
    $resourceGroupName="<your-resource-group-name>"
    $location='Central US'
    $userPrincipalName="<your-email-address-associated-with-your-subscription>"
    
    # Create a resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
        
    # Create a Key Vault
    $keyVault = New-AzureRmKeyVault `
      -VaultName $keyVaultName `
      -resourceGroupName $resourceGroupName `
      -Location $location `
      -EnabledForTemplateDeployment
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $userPrincipalName -PermissionsToSecrets set,delete,get,list
      
    # Create a secret
    $password = openssl rand -base64 32
    
    $secretValue = ConvertTo-SecureString $password -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name "vmAdminPassword" -SecretValue $secretValue
    
    # Print the useful property values
    echo "You need the following values for the virtual machine deployment:"
    echo "Resource group name is: $resourceGroupName."
    echo "The admin password is: $password."
    echo "The Key Vault resource ID is: " $keyVault.ResourceID
    ```
4. Anote os valores de saída. Vai precisar deles mais tarde no tutorial.

> [!NOTE]
> Cada serviço do Azure tem requisitos de palavra-passe específicos. Por exemplo, os requisitos da máquina virtual do Azure podem ser encontrados em Quais são os requisitos de palavra-passe quando criar uma VM?

## <a name="open-a-quickstart-template"></a>Abrir um modelo de Início Rápido

Os Modelos de Início Rápido do Azure são um repositório de modelos do Resource Manager. Em vez de criar um modelo do zero, pode encontrar um modelo de exemplo e personalizá-lo. O modelo utilizado neste tutorial é denominado [Implementar uma VM do Windows simples](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/).

1. No Visual Studio Code, selecione **Ficheiro**>**Abrir Ficheiro**.
2. em **Nome de ficheiro**, cole o seguinte URL:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Selecione **Abrir** para abrir o ficheiro.
4. Selecione **Ficheiro**>**Guardar Como** para guardar uma cópia do ficheiro no computador local, com o nome **azuredeploy.json**.
5. Repita os passos 1 a 4 para abrir **https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json** e, em seguida, guarde o ficheiro como **azuredeploy.parameters.json**.

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

## <a name="edit-the-parameters-file"></a>Editar o ficheiro de parâmetros

Não precisa de fazer quaisquer alterações ao ficheiro de modelo. No entanto, tem de modificar o ficheiro de parâmetros para recuperar a palavra-passe de administrador a partir do Key Vault.

1. Abra **azuredeploy.parameters.json** no Visual Studio Code, se não estiver aberto.
2. Atualize o parâmetro **adminPassword** para:

    ```json
    "adminPassword": {
        "reference": {
            "keyVault": {
            "id": "/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>"
            },
            "secretName": "vmAdminPassword"
        }
    },
    ```
    Substitua **id** pelo ID do recurso do Key Vault criado no último procedimento. É uma das saídas. 

    ![Integrar o cofre de chaves e o ficheiro de parâmetros de implementação de máquinas virtuais de modelos do Resource Manager](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)
3. Atribua valores para:

    - **adminUsername**: nome da conta de administrador da máquina virtual.
    - **dnsLabelPrefix**: dê um nome a dnsLablePrefix.
4. Guarde as alterações.

## <a name="deploy-the-template"></a>Implementar o modelo

Existem muitos métodos para implementar modelos.  Neste tutorial, vai utilizar o Cloud Shell do portal do Azure.

1. Inicie sessão no [Cloud Shell](https://shell.azure.com). Também pode iniciar sessão no [portal do Azure](https://portal.azure.com) e, em seguida, selecione **Cloud Shell** no canto superior direito, conforme mostrado na imagem seguinte:

    ![Cloud Shell do portal do Azure](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell.png)
2. Selecione **PowerShell** no canto superior esquerdo do Cloud Shell e, em seguida, selecione **Confirmar**.  Vai utilizar o PowerShell neste tutorial.
3. Selecione **Carregar ficheiro** do Cloud Shell:

    ![Carregar ficheiro Cloud Shell do Portal do Azure](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-upload-file.png)
4. Selecione os ficheiros que guardou anteriormente no tutorial. O nome predefinido é **azuredeploy.json** e **azuredeploy.paraemters.json**.  Se tiver ficheiros com os mesmos nomes de ficheiro, os ficheiros antigos serão substituídos sem qualquer notificação.
5. No Cloud Shell, execute o seguinte comando para certificar-se de que o ficheiro foi carregado com êxito. 

    ```bash
    ls
    ```

    ![Ficheiro de lista de Cloud Shell do portal do Azure](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-list-file.png)

    O nome do ficheiro mostrado na captura de ecrã é azuredeploy.json.

6. No Cloud Shell, execute o seguinte comando para verificar o conteúdo do ficheiro JSON:

    ```bash
    cat azuredeploy.json
    cat azuredeploy.parameters.json
    ```
7. No Cloud Shell, execute os seguintes comandos do PowerShell. O script de exemplo utiliza o mesmo grupo de recursos criado para o Key Vault. Utilizar o mesmo grupo de recursos facilita a limpeza dos recursos.

    ```powershell
    $resourceGroupName = "<Enter the resource group name>"
    $deploymentName = "<Enter a deployment name>"

    New-AzureRmResourceGroupDeployment -Name $deploymentName `
        -ResourceGroupName $resourceGroupName `
        -TemplateFile azuredeploy.json `
        -TemplateparameterFile azuredeploy.parameters.json
    ```
8. Execute o seguinte comando do PowerShell para listar a máquina virtual recentemente criada:

    ```powershell
    Get-AzureRmVM -Name SimpleWinVM -ResourceGroupName $resourceGroupName
    ```

    O nome da máquina virtual está hard-coded como **SimpleWinVM** dentro do modelo.

9. Inicie sessão na máquina virtual para testar as credenciais do administrador. 

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


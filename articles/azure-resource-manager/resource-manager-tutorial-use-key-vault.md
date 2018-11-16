---
title: Integrar o Azure Key Vault na implementação de modelos do Resource Manager | Microsoft Docs
description: Saiba como utilizar o Azure Key Vault para transmitir valores de parâmetros seguros durante a implementação de modelos do Resource Manager
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
ms.openlocfilehash: a3ca6422bf5335604e561b71db6c75a889a74586
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51615760"
---
# <a name="tutorial-integrate-azure-key-vault-in-resource-manager-template-deployment"></a>Tutorial: Integrar o Azure Key Vault na implementação de modelos do Resource Manager

Saiba como obter valores secretos do Azure Key Vault e transmiti-los como parâmetros durante a implementação do Resource Manager. O valor nunca está exposto porque só faz referência ao respetivo ID do Cofre de Chaves. Para obter mais informações, veja [Utilizar o Azure Key Vault para transmitir valores de parâmetros seguros durante a implementação](./resource-manager-keyvault-parameter.md)

No tutorial [Definir a ordem de implementação de recursos](./resource-manager-tutorial-create-templates-with-dependent-resources.md), cria uma máquina virtual, uma rede virtual e alguns outros recursos dependentes. Neste tutorial, vai personalizar o modelo para obter a palavra-passe de administrador de uma máquina virtual do Azure Key Vault.

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Preparar o Key Vault
> * Abrir um modelo de Início rápido
> * Editar o ficheiro de parâmetros
> * Implementar o modelo
> * Validar a implementação
> * Limpar recursos

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este artigo, precisa de:

* [Visual Studio Code](https://code.visualstudio.com/) com [extensão Ferramentas do Resource Manager](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Para aumentar a segurança, utilize uma palavra-passe gerada para a conta de administrador da máquina virtual. Eis um exemplo para gerar uma palavra-passe:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    O Azure Key Vault foi criado para salvaguardar chaves criptográficos e outros segredos. Para obter mais informações, veja [Tutorial: Integrar o Azure Key Vault na implementação de modelos do Resource Manager](./resource-manager-tutorial-use-key-vault.md). Também recomendamos que atualize a palavra-passe a cada três meses.

## <a name="prepare-the-key-vault"></a>Preparar o Key Vault

Nesta secção, vai utilizar um modelo do Resource Manager para criar um Cofre de Chaves e um segredo. Este modelo:

* Criar um Key Vault com a propriedade `enabledForTemplateDeployment` ativada. Esta propriedade tem de ser verdadeira antes de o processo de implementação do modelo poder aceder aos segredos definidos neste Cofre de Chaves.
* Adiciona um segredo ao Cofre de Chaves.  O segredo armazena a palavra-passe de administrador da máquina virtual.

Se, como o utilizador para implementar o modelo de máquina virtual, não for o proprietário ou o contribuidor do Cofre de Chaves, o Proprietário ou o Contribuidor do Cofre de Chaves deve conceder-lhe o acesso à permissão Microsoft.KeyVault/vaults/deploy/action para o Cofre de Chaves. Para obter mais informações, veja [Utilizar o Azure Key Vault para transmitir valores de parâmetros seguros durante a implementação](./resource-manager-keyvault-parameter.md)

O modelo precisa do ID de objeto de utilizador do Azure AD para configurar permissões. O procedimento seguinte obtém o ID de objeto (GUID) e gera a palavra-passe de administrador. Para impedir um ataque de "password spray", é recomendada a utilização de palavras-passe geradas.

1. Execute o comando seguinte do Azure PowerShell ou da CLI do Azure.  

    ```azurecli-interactive
    echo "Enter your email address that is associated with your Azure subscription):" &&
    read upn &&
    az ad user show --upn-or-object-id $upn --query "objectId" &&
    openssl rand -base64 32
    ```
    ```azurepowershell-interactive
    $upn = Read-Host -Prompt "Input your user principal name (email address) used to sign in to Azure"
    (Get-AzureADUser -ObjectId $upn).ObjectId
    openssl rand -base64 32
    ```
2. Anote o ID de objeto e a palavra-passe gerada. Vai precisar deles mais tarde.
3. Verifique se a palavra-passe gerada cumpre os requisitos de palavra-passe da máquina virtual. Cada serviço do Azure tem requisitos de palavra-passe específicos. Para obter os requisitos de palavra-passe de VM, veja [Quais são os requisitos de palavra-passe quando criar uma VM?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

Para criar um Cofre de Chaves:

1. Selecione a imagem seguinte para iniciar sessão no Azure e abrir um modelo. O modelo cria um Cofre de Chaves e um segredo.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Farmtutorials.blob.core.windows.net%2Fcreatekeyvault%2FCreateKeyVault.json"><img src="./media/resource-manager-tutorial-use-key-vault/deploy-to-azure.png" /></a>

2. Selecione ou introduza os seguintes valores.  Não selecione **Comprar** depois de introduzir os valores.

    ![Portal de implementação de integração de Cofres de Chaves de modelos do Resource Manager](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-key-vault-portal.png)

    * **Subscrição**: selecione uma subscrição do Azure.
    * **Grupo de recursos**: atribua um nome exclusivo. Anote este nome, uma vez que vai utilizar o mesmo grupo de recursos para implementar a máquina virtual na próxima sessão. Colocar o Cofre de Chaves e a máquina virtual no mesmo grupo de recursos facilita a limpeza do recurso no final do tutorial.
    * **Localização**: selecione uma localização.  A localização predefinida é **E.U.A. Central**.
    * **Nome do Cofre de Chaves**: atribua um nome exclusivo. 
    * **ID do inquilino**: a função de modelo obtém automaticamente o ID do inquilino.  Não altere o valor predefinido
    * **ID de utilizador do AD**: introduza o ID de objeto de utilizador do Azure AD obtido no último procedimento.
    * **Nome do Segredo**: o nome predefinido é **mAdminPassword**. Se alterar o nome do segredo aqui, tem de atualizá-lo quando implementar a máquina virtual.
    * **Valor do Segredo**: introduza o segredo.  O segredo é a palavra-passe utilizada para iniciar sessão na máquina virtual. É recomendado utilizar a palavra-passe gerada que criou no último procedimento.
    * **Aceito os termos e condições acima apresentados**: selecione.
3. Selecione **Editar parâmetros** na parte superior para ver o modelo.
4. Navegue para a linha 28 do ficheiro JSON do modelo. Esta é a definição do recurso do Cofre de Chaves.
5. Navegue para a linha 35:

    ```json
    "enabledForTemplateDeployment": true,
    ```
    `enabledForTemplateDeployment` é uma propriedade do Cofre de Chaves. Esta propriedade tem de ser verdadeira antes de poder obter os segredos a partir deste Cofre de Chaves durante a implementação.
6. Navegue para a linha 89. Esta é a definição do segredo do Cofre de Chaves.
7. Selecione **Eliminar** na parte inferior da página. Não efetue alterações.
8. Verifique se forneceu todos os valores, conforme mostrado na captura de ecrã anterior, e clique em **Comprar** na parte inferior da página.
9. Selecione o ícone de campainha (notificação) na parte superior da página para abrir o painel **Notificações**. Aguarde até o recurso ser implementado com êxito.
10. Selecione **Ir para o grupo de recursos** no painel **Notificações**. 
11. Selecione o nome do Cofre de Chaves para abri-lo.
12. Selecione **Políticas de acesso** no painel esquerdo. O seu nome (Active Directory) deve ser apresentado, caso contrário, não tem permissão para aceder ao cofre de chaves.
13. Selecione **Clicar para mostrar as políticas de acesso avançadas**. Verifique se a opção **Ativar o acesso ao Azure Resource Manager para implementação de modelos** está selecionada. Trata-se de outra condição para o Cofre de Chaves funcionar.

    ![Políticas de acesso de integração de Cofres de Chaves de modelos do Resource Manager](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-key-vault-access-policies.png)
14. Selecione **Propriedades** no painel esquerdo.
15. Faça uma cópia do **ID do Recurso**. Vai precisar deste ID quando implementar a máquina virtual.  O formato do ID do Recurso é:

    ```json
    /subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>
    ```

## <a name="open-a-quickstart-template"></a>Abrir um modelo de Início Rápido

Os Modelos de Início Rápido do Azure são um repositório de modelos do Resource Manager. Em vez de criar um modelo do zero, pode encontrar um modelo de exemplo e personalizá-lo. O modelo utilizado neste tutorial é denominado [Implementar uma VM do Windows simples](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/).

1. No Visual Studio Code, selecione **Ficheiro**>**Abrir Ficheiro**.
2. em **Nome de ficheiro**, cole o seguinte URL:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Selecione **Abrir** para abrir o ficheiro. Trata-se do mesmo cenário utilizado no [Tutorial: Criar modelos do Azure Resource Manager com recursos dependentes](./resource-manager-tutorial-create-templates-with-dependent-resources.md).
4. Existem cinco recursos definidos pelo modelo:

    * `Microsoft.Storage/storageAccounts`. Veja a [referência do modelo](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
    * `Microsoft.Network/publicIPAddresses`. Veja a [referência do modelo](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
    * `Microsoft.Network/virtualNetworks`. Veja a [referência do modelo](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
    * `Microsoft.Network/networkInterfaces`. Veja a [referência do modelo](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
    * `Microsoft.Compute/virtualMachines`. Veja a [referência do modelo](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

    É útil ter alguma compreensão básica do modelo antes de personalizá-lo.
5. Selecione **Ficheiro**>**Guardar Como** para guardar uma cópia do ficheiro no computador local, com o nome **azuredeploy.json**.
6. Repita os passos 1 a 4 para abrir o URL seguinte e, em seguida, guarde o ficheiro como **azuredeploy.parameters.json**.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json
    ```

## <a name="edit-the-parameters-file"></a>Editar o ficheiro de parâmetros

Não precisa de fazer quaisquer alterações ao ficheiro de modelo.

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
    Substitua **id** pelo ID do recurso do Cofre de Chaves criado no último procedimento.  

    ![Integrar o cofre de chaves e o ficheiro de parâmetros de implementação de máquinas virtuais de modelos do Resource Manager](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)
3. Atribua valores para:

    * **adminUsername**: nome da conta de administrador da máquina virtual.
    * **dnsLabelPrefix**: dê um nome a dnsLabelPrefix.
4. Guarde as alterações.

## <a name="deploy-the-template"></a>Implementar o modelo

Siga as instruções em [Implementar o modelo](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) para implementar o modelo. Tem de carregar **azuredeploy.json** e **azuredeploy.parameters.json** para o Cloud Shell e, em seguida, utilize o seguinte script do PowerShell para implementar o modelo:

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the resource group name of the Key Vault"
$deploymentName = Read-Host -Prompt "Enter the name for this deployment"
New-AzureRmResourceGroupDeployment -Name $deploymentName -ResourceGroupName $resourceGroupName `
    -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
```

Quando implementar o modelo, utilize o mesmo grupo de recursos do Cofre de Chaves. Facilita a limpeza dos recursos. Apenas tem de eliminar um grupo de recursos em vez de dois.

## <a name="valid-the-deployment"></a>Validar a implementação

Depois de ter implementado com êxito a máquina virtual, teste o início de sessão com a palavra-passe armazenada no Cofre de Chaves.

1. Abra o [Portal do Azure](https://portal.azure.com).
2. Selecione **Grupos de recursos**/**NomedoGrupodeRecursos>**/**simpleWinVM**
3. Selecione **ligar** na parte superior.
4. Selecione **Transferir ficheiro RDP** e, em seguida, siga as instruções para iniciar sessão na máquina virtual com a palavra-passe armazenada no Cofre de Chaves.

## <a name="clean-up-resources"></a>Limpar recursos

Quando os recursos do Azure já não forem necessários, limpe os recursos implementados ao eliminar o grupo de recursos.

1. No portal do Azure, selecione **Grupo de recursos** no menu à esquerda.
2. Introduza o nome do grupo de recursos no campo **Filtrar por nome**.
3. Selecione o nome do grupo de recursos.  Verá um total de seis recursos no grupo de recursos.
4. Selecione **Eliminar grupo de recursos** no menu superior.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, obteve um segredo do Azure Key Vault e utilizou-o na sua implementação do modelo.  Para saber como criar modelos ligados, veja:

> [!div class="nextstepaction"]
> [Criar modelos ligados](./resource-manager-tutorial-create-linked-templates.md)

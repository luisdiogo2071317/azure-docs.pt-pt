---
title: Utilizar o Visual Studio Code para criar modelos do Azure Resource Manager | Microsoft Docs
description: Utilize a extensão Azure Resource Manager Tools para trabalhar em modelos do Resource Manager.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 07/17/2018
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: d50b84ac61210fc89665341ae0c2de3fc4ce0c11
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "42023611"
---
# <a name="quickstart-create-azure-resource-manager-templates-by-using-visual-studio-code"></a>Início Rápido: como criar modelos do Azure Resource Manager com o Visual Studio Code

Saiba como criar modelos do Azure Resource Manager ao utilizar o Visual Studio Code e a extensão de Ferramentas do Azure Resource. Pode criar modelos do Resource Manager no Visual Studio Code sem a extensão, mas esta proporciona opções de conclusão automática que simplificam o desenvolvimento dos modelos. Para compreender os conceitos associados à implementação e gestão das suas soluções do Azure, veja [Descrição geral do Azure Resource Manager](resource-group-overview.md).

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

Para saber como editar um modelo com o Visual Studio Code, adicione mais um elemento para a secção de resultados.

1. No Visual Studio Code, adicione mais um resultado ao modelo exportado:

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

    Se tiver copiado e colado o código no Visual Studio Code, tente voltar a escrever o elemento **valor** para experimentar a capacidade intellisense da extensão das Ferramentas do Resource Manager.

    ![Intellisense Visual Studio Code de modelo do Resource Manager](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/resource-manager-templates-visual-studio-code-intellisense.png)

2. Selecione **Ficheiro**>**Guardar** para guardar o ficheiro.

## <a name="deploy-the-template"></a>Implementar o modelo

Existem muitos métodos para implementar modelos.  Neste início rápido, vai utilizar o Cloud Shell do portal do Azure. O Cloud Shell suporta a CLI do Azure e o Azure PowerShell. As instruções aqui apresentadas utilizam a CLI.

1. Inicie sessão no [portal do Azure](https://portal.azure.com)
2. Selecione **Cloud Shell** no canto superior direito, conforme mostrado na imagem seguinte:

    ![Cloud Shell do portal do Azure](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell.png)

3. Selecione a seta para baixo e, em seguida, selecione **Bash** para mudar do PowerShell para a CLI.

    ![CLI Cloud Shell do portal do Azure](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-choose-cli.png)
4. Selecione **Reiniciar** para reiniciar a shell.
5. Selecione **Carregar/transferir ficheiros** e, em seguida, selecione **Carregar**.

    ![Carregar ficheiro Cloud Shell do Portal do Azure](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file.png)
4. Selecione o ficheiro que guardou anteriormente no início rápido. O nome predefinido é **azuredeploy.json**.
5. A partir do Cloud Shell, execute o comando **ls** para se certificar de que o ficheiro foi carregado com êxito. Pode também utilizar o comando **cat** para verificar o conteúdo do modelo.

    ![Ficheiro de lista de Cloud Shell do portal do Azure](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file.png)
6. No Cloud Shell, execute os seguintes comandos:

    ```cli
    az group create --name <ResourceGroupName> --location <AzureLocation>

    az group deployment create --name <DeploymentName> --resource-group <ResourceGroupName> --template-file <TemplateFileName>
    ```
    Segue-se a captura de ecrã de uma implementação de exemplo:

    ![Modelo de implementação de Cloud Shell no portal do Azure](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template.png)

    Na captura de ecrã, utilizam-se estes valores:

    - **&lt;ResourceGroupName>**: myresourcegroup0709. O parâmetro tem duas ocorrências.  Certifique-se de que utiliza o mesmo valor.
    - **&lt;AzureLocation>**: eastus2
    - **&lt;DeployName>**: mydeployment0709
    - **&lt;TemplateFile>**: azuredeploy.json

    No resultado da captura de ecrã, o nome da conta de armazenamento é *3tqebj3slyfyestandardsa*. 

7. Execute o seguinte comando da CLI para listar a conta de armazenamento acabada de criar:

    ```cli
    az storage account show --resource-group <ResourceGroupName> --name <StorageAccountName>
    ```

## <a name="clean-up-resources"></a>Limpar recursos

Quando os recursos do Azure já não forem necessários, limpe os recursos implementados ao eliminar o grupo de recursos.

1. No portal do Azure, selecione **Grupo de recursos** no menu à esquerda.
2. Introduza o nome do grupo de recursos no campo **Filtrar por nome**.
3. Selecione o nome do grupo de recursos.  Verá um total de seis recursos no grupo de recursos.
4. Selecione **Eliminar grupo de recursos** no menu superior.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a criar um modelo a partir do Visual Studio Code e a implementar o modelo através do Cloud Shell do portal do Azure. No próximo tutorial, saiba mais sobre como desenvolver um modelo e utilizar a referência de modelos.

> [!div class="nextstepaction"]
> [Criar conta de Armazenamento encriptado](./resource-manager-tutorial-create-encrypted-storage-accounts.md)

---
title: Criar e implementar um modelo do Azure Resource Manager com o portal do Azure | Microsoft Docs
description: Saiba como criar o seu primeiro modelo do Azure Resource Manager com o portal do Azure e como implementá-lo.
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
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: 515ab6a4ee832c702cfe19f2aca9022f208b409d
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51612793"
---
# <a name="quickstart-create-and-deploy-azure-resource-manager-templates-by-using-the-azure-portal"></a>Início Rápido: Criar e implementar um modelo do Azure Resource Manager com o portal do Azure

Saiba como criar o seu primeiro modelo do Azure Resource Manager mediante a geração de um modelo no portal do Azure e aprenda o processo de edição e implementação do mesmo a partir do portal do Azure. Os modelos do Resource Manager são ficheiros JSON que definem os recursos que precisa de implementar para a sua solução. As instruções neste tutorial permitem criar uma conta de Armazenamento do Azure. Pode utilizar o mesmo processo para criar outros recursos do Azure.

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="generate-a-template-using-the-portal"></a>Gerar um modelo com o portal

Nesta secção, vai criar uma conta de armazenamento através do portal do Azure. Antes de implementar a conta de armazenamento, tem a opção de explorar o modelo gerado pelo portal com base nas suas configurações. Pode guardar o modelo e reutilizá-lo no futuro.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **Criar um recurso** > **Armazenamento** > **Conta de armazenamento – blob, ficheiro, tabela, fila**.

    ![Criar uma conta de armazenamento do Azure com o portal do Azure](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-portal.png)
3. Introduza as seguintes informações. 

    - **Grupo de recursos**: crie um novo grupo de recursos do Azure com o nome da sua preferência. Na captura de ecrã, o nome do grupo de recursos é *mystorage1016rg*.
    - **Nome**: dê um nome exclusivo à conta de armazenamento. Na captura de ecrã, o nome é *mystorage1016*.

    Pode utilizar os valores predefinidos para as restantes propriedades.

    ![Criar uma configuração de conta de armazenamento do Azure com o portal do Azure](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account.png)

    > [!NOTE]
    > Alguns dos modelos exportados requerem algumas edições antes de poder implementá-los.

4. Selecione **Rever + criar**, na parte inferior do ecrã. 
5. Selecione **Transferir um modelo para automatização**, na parte inferior do ecrã. O portal mostra o modelo gerado:

    ![Gerar um modelo a partir do portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template.png)

    O painel principal mostra o modelo. É um ficheiro JSON com quatro elementos de nível superior - `schema`, `contentVersion`, `parameters` e `resources`. Para obter mais informações, veja [Compreender a estrutura e a sintaxe de modelos do Azure Resource Manager](./resource-group-authoring-templates.md)

    Existem seis parâmetros definidos. Um desses parâmetros é denominado **storageAccountName**. A segunda parte realçada mostra como o utilizar no modelo. Na próxima secção, vai editar o modelo de modo a utilizar um nome gerado para a conta de armazenamento.

    No modelo, é definido um recurso do Azure. O tipo é [Microsoft.Storage/storageAccounts]. Veja como é que o recurso está definido e a estrutura da definição.
6. Selecione **Transferir**. Guarde **template.json** do pacote transferido para o seu computador. Na próxima secção, vai utilizar uma ferramenta de implementação de modelos para editar o modelo.
7. Selecione o separador **Parameter** (Parâmetro) para ver os valores que indicou para os parâmetros. Aponte-os, pois vai precisar dos mesmos na próxima secção, quando implementar o modelo.

    ![Gerar um modelo a partir do portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template-parameters.png)

    Com o modelo e os ficheiros de parâmetros, pode criar uma conta de armazenamento do Azure.

## <a name="edit-and-deploy-the-template"></a>Editar e implementar o modelo

O portal do Azure pode ser utilizado para fazer algumas edições básicas aos modelos. Neste início rápido, vai utilizar uma ferramenta do portal, denominada *Template Deployment*. Para editar um modelo mais complexo, considere utilizar o [Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md), que oferece funcionalidades de edição mais avançadas.

O Azure exige que cada serviço do Azure tenha um nome exclusivo. A implementação falha se introduzir um nome de conta de armazenamento que já existe. Para evitar este problema, pode utilizar uma chamada de função de modelo, `uniquestring()`, para gerar um nome de conta de armazenamento exclusivo.

1. No portal do Azure, selecione **Criar um recurso**.
2. Em **Pesquisar no Marketplace**, escreva **implementação de modelo** e prima **ENTER**.
3. Selecione **Implementação de modelo**.

    ![Biblioteca de modelos do Azure Resource Manager](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-library.png)
4. Selecione **Criar**.
5. Selecione **Crie o seu próprio modelo no editor**.
6. Selecione **Carregar ficheiro** e siga as instruções para carregar template.json, que transferiu na última secção.
7. Adicione uma variável, conforme mostrado na captura de ecrã seguinte:

    ```json
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
    ```
    ![Modelos do Azure Resource Manager](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-edit-storage-account-template-revised.png)

    São utilizadas duas funções aqui, `concat()` e `uniqueString()`.

8. Remova o parâmetro **storageAccountName** realçado na captura de ecrã anterior.
9. Atualize o elemento name do recurso **Microsoft.Storage/storageAccounts** para utilizar a variável recém-definida em vez do parâmetro:

    ```json
    "name": "[variables('storageAccountName')]",
    ```

    O modelo final deve ter o seguinte aspeto:

    ```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "location": {
                "type": "string"
            },
            "accountType": {
                "type": "string"
            },
            "kind": {
                "type": "string"
            },
            "accessTier": {
                "type": "string"
            },
            "supportsHttpsTrafficOnly": {
                "type": "bool"
            }
        },
        "variables": {
            "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
            {
                "name": "[variables('storageAccountName')]",
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2018-07-01",
                "location": "[parameters('location')]",
                "properties": {
                    "accessTier": "[parameters('accessTier')]",
                    "supportsHttpsTrafficOnly": "[parameters('supportsHttpsTrafficOnly')]"
                },
                "dependsOn": [],
                "sku": {
                    "name": "[parameters('accountType')]"
                },
                "kind": "[parameters('kind')]"
            }
        ],
        "outputs": {}
    }
    ```
7. Selecione **Guardar**.
8. Introduza os seguintes valores:

    - **Grupo de recursos**: atribua um nome exclusivo ao grupo de recursos.
    - **Localização**: selecione uma localização para o grupo de recursos.
    - **Localização**: selecione uma localização para a conta de armazenamento.  Pode utilizar a mesma localização do grupo de recursos.
    - **Tipo de Conta**: introduza **Standard_LRS** para este início rápido.
    - **Tipo**: introduza **StorageV2** para este início rápido.
    - **Camada de acesso**: introduza **Acesso frequente** para este início rápido.
    - **Tráfego HTTPS apenas ativado**.  Selecione **verdadeiro** para este início rápido.
    - **Aceito os termos e condições acima apresentados** (selecione)

    Segue-se uma captura de ecrã de uma implementação de exemplo:

    ![Implementação de modelos do Azure Resource Manager](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-deploy.png)

10. Selecione **Comprar**.
11. Selecione o ícone de campainha (notificações) na parte superior do ecrã para ver o estado de implementação. Aguarde até que a implementação esteja concluída.

    ![Notificação de implementação de modelos do Azure Resource Manager](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-notification.png)

12. Selecione **Ir para o grupo de recursos** no painel de notificação. Deverá ver um ecrã semelhante a:

    ![Grupo de recursos de implementação de modelos do Azure Resource Manager](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-deployment-resource-group.png)

    Pode ver que o estado de implementação foi bem-sucedido e que não existe apenas uma conta de armazenamento no grupo de recursos. O nome da conta de armazenamento é uma cadeia de carateres exclusiva gerada pelo modelo. Para saber mais sobre como utilizar contas de armazenamento do Azure, veja [Início Rápido: Carregar, transferir e listar blobs através do portal do Azure](../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="clean-up-resources"></a>Limpar recursos

Quando os recursos do Azure já não forem necessários, limpe os recursos implementados ao eliminar o grupo de recursos.

1. No portal do Azure, selecione **Grupo de recursos** no menu à esquerda.
2. Introduza o nome do grupo de recursos no campo **Filtrar por nome**.
3. Selecione o nome do grupo de recursos.  Deverá ver a conta de armazenamento no grupo de recursos.
4. Selecione **Eliminar grupo de recursos** no menu superior.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a gerar um modelo a partir do portal do Azure e a implementar o modelo através do portal. O modelo utilizado neste Início Rápido é um modelo simples com um recurso do Azure. Quando o modelo é complexo, é mais fácil utilizar o Visual Studio Code ou o Visual Studio para desenvolver o modelo. O próximo início rápido também lhe mostra como implementar modelos com o Azure PowerShell e a CLI (Interface de Linha de Comandos) do Azure.

> [!div class="nextstepaction"]
> [Criar modelos com o Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md)

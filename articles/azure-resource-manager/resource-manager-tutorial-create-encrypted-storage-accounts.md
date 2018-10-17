---
title: Criar um modelo do Azure Resource Manager para implementar uma conta de armazenamento encriptada | Microsoft Docs
description: Utilize o Visual Studio Code para criar um modelo para implementar uma conta de armazenamento encriptada.
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
ms.openlocfilehash: d48374d7919be3d141ea199e8238a220dbfe0332
ms.sourcegitcommit: 3150596c9d4a53d3650cc9254c107871ae0aab88
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47419530"
---
# <a name="tutorial-create-an-azure-resource-manager-template-for-deploying-an-encrypted-storage-account"></a>Tutorial: Criar um modelo do Azure Resource Manager para implementar uma conta de armazenamento encriptada

Saiba como localizar informações para concluir um modelo do Azure Resource Manager.

Neste tutorial, vai utilizar um modelo de base dos modelos de Início Rápido do Azure para criar uma conta de Armazenamento do Azure.  Com a documentação de referência do modelo, personalize o modelo de base para criar uma conta de armazenamento encriptada.

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Abrir um modelo de Início Rápido
> * Compreender o modelo
> * Editar o modelo
> * Implementar o modelo

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este artigo, precisa de:

* [Visual Studio Code](https://code.visualstudio.com/).
* Extensão das Ferramentas do Azure Resource Manager. Para instalar, veja [Instalar a extensão das Ferramentas do Resource Manager](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).

## <a name="open-a-quickstart-template"></a>Abrir um modelo de Início Rápido

O modelo utilizado neste início rápido chama-se [Criar uma conta de armazenamento padrão](https://azure.microsoft.com/resources/templates/101-storage-account-create/). O modelo define um recurso de conta de Armazenamento do Azure.

1. No Visual Studio Code, selecione **Ficheiro**>**Abrir Ficheiro**.
2. em **Nome de ficheiro**, cole o seguinte URL:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Selecione **Abrir** para abrir o ficheiro.
4. Selecione **Ficheiro**>**Guardar Como** para guardar o ficheiro como **azuredeploy.json** no computador local.

## <a name="understand-the-format"></a>Compreender o formato

No VS Code, feche o modelo até ao nível da raiz. Tem a estrutura mais simples com os seguintes elementos:

![Estrutura mais simples do modelo do Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-simplest-structure.png)

* **$schema**: especifique a localização do ficheiro de esquema JSON que descreve a versão da linguagem de modelos.
* **contentVersion**: especifique qualquer valor para este elemento, para documentar alterações significativas ao seu modelo.
* **parâmetros**: especifique os valores que são fornecidos quando a implementação é executada, para personalizar a implementação de recursos.
* **variáveis**: especifique os valores que são utilizados como fragmentos JSON no modelo, para simplificar as expressões de linguagem de modelos.
* **recursos**: especifique os tipos de recurso que são implementados ou atualizados num grupo de recursos.
* **saídas**: especifique os valores que são devolvidos após a implementação.

## <a name="use-parameters-in-template"></a>Utilizar parâmetros no modelo

Os parâmetros permitem-lhe personalizar a implementação, ao fornecer valores que são adaptados para um determinado ambiente. Utilize os parâmetros definidos no modelo ao definir valores para a conta de armazenamento.

![Parâmetros de modelo do Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-parameters.png)

Neste modelo, são definidos dois parâmetros. Observe que uma função de modelo é utilizada em location.defaultValue:

```json
"defaultValue": "[resourceGroup().location]",
```

A função resourceGroup() devolve um objeto que representa o grupo de recursos atual. Para obter uma lista de funções de modelo, veja o artigo [Funções de modelo do Azure Resource Manager](./resource-group-template-functions.md).

Para utilizar os parâmetros definidos no modelo:

```json
"location": "[parameters('location')]",
"name": "[parameters('storageAccountType')]"
```

## <a name="use-variables-in-template"></a>Utilizar variáveis no modelo

As variáveis permitem-lhe construir valores que podem ser utilizados em todo o modelo. As variáveis ajudam a reduzir a complexidade dos modelos.

![Variáveis de modelo do Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-variables.png)

Este modelo define uma variável *storageAccountName*. Na definição, são utilizadas duas funções de modelo:

- **concat()**: concatena cadeias. Para obter mais informações, veja[concat](./resource-group-template-functions-string.md#concat).
- **uniqueString()**: cria uma cadeia de hash determinista, com base nos valores fornecidos como parâmetros. Cada conta de armazenamento do Azure tem de ter um nome exclusivo em todo o Azure. Esta função fornece uma cadeia exclusiva. Para obter mais funções de cadeia, veja [Funções de cadeia](./resource-group-template-functions-string.md).

Para utilizar a variável definida no modelo:

```json
"name": "[variables('storageAccountName')]"
```

## <a name="edit-the-template"></a>Editar o modelo

O objetivo deste tutorial é definir um modelo para criar uma conta de armazenamento encriptada.  O modelo de exemplo apenas cria uma conta de armazenamento não encriptada básica. Para localizar a configuração relacionada com a encriptação, pode utilizar a referência do modelo da conta de Armazenamento do Azure.

1. Navegue até [Modelos do Azure](https://docs.microsoft.com/azure/templates/).
2. Em **Filtrar por título**, introduza **contas de armazenamento**.
3. Selecione **Referência/Referência de modelo/Armazenamento/Contas de Armazenamento** conforme mostrado na captura de ecrã seguinte:

    ![Conta de armazenamento de referência do modelo do Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-resources-reference-storage-accounts.png)

    resource-manager-template-resources-reference-storage-accounts
1. Explore as informações relacionadas com a encriptação.  
1. Dentro do elemento de propriedades da definição de recurso de conta de armazenamento, adicione o seguinte json:

    ```json
    "encryption": {
        "keySource": "Microsoft.Storage",
        "services": {
            "blob": {
                "enabled": true
            }
        }
    }
    ```
    Esta parte ativa a função de encriptação do serviço de armazenamento de blobs.

A partir do Visual Studio Code, modifique o modelo para que o elemento de recursos final se pareça com:

![Recursos de conta de armazenamento encriptada de modelo do Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-resources.png)

## <a name="deploy-the-template"></a>Implementar o modelo

Veja a secção [Implementar o modelo](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#deploy-the-template) no guia de introdução do Visual Studio Code para o procedimento de implementação.

A captura de ecrã seguinte mostra o comando da CLI para listar a conta de armazenamento recentemente criada, o que indica que a encriptação foi ativada para o armazenamento de blobs.

![Conta de armazenamento encriptada do Azure Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-account.png)

## <a name="clean-up-resources"></a>Limpar recursos

Quando os recursos do Azure já não forem necessários, limpe os recursos implementados ao eliminar o grupo de recursos.

1. No portal do Azure, selecione **Grupo de recursos** no menu à esquerda.
2. Introduza o nome do grupo de recursos no campo **Filtrar por nome**.
3. Selecione o nome do grupo de recursos.  Verá um total de seis recursos no grupo de recursos.
4. Selecione **Eliminar grupo de recursos** no menu superior.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a utilizar a referência de modelo para personalizar um modelo existente. Para saber mais sobre como criar várias instâncias de contas de armazenamento, veja:

> [!div class="nextstepaction"]
> [Criar múltiplas instâncias](./resource-manager-tutorial-create-multiple-instances.md)

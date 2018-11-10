---
title: Implementar extensões de máquina virtual com modelos do Azure Resource Manager | Microsoft Docs
description: Saiba como implementar extensões de máquina virtual com modelos do Azure Resource Manager
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/30/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: ca07f044a423a4ea1b5dee484c56f457c7d785de
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50239736"
---
# <a name="tutorial-deploy-virtual-machine-extensions-with-azure-resource-manager-templates"></a>Tutorial: Implementar extensões de máquina virtual com modelos do Azure Resource Manager

Aprenda a utilizar [extensões de máquina virtual do Azure](../virtual-machines/extensions/features-windows.md) para fazer tarefas de automatização e configuração de pós-implementação em VMs do Azure. Estão disponíveis muitas VMs diferentes para utilização com as VMs do Azure. Neste tutorial, vai implementar uma extensão de Script Personalizado a partir de um modelo do Resource Manager para executar um script do PowerShell numa VM do Windows.  O script instala o Servidor Web na VM.

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Preparar o script do Powershell
> * Abrir um modelo de Início rápido
> * Editar o modelo
> * Implementar o modelo
> * Verificar a implementação

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este artigo, precisa de:

* [Visual Studio Code](https://code.visualstudio.com/) com a extensão Ferramentas do Resource Manager.  Veja [Instalar a extensão](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Para aumentar a segurança, utilize uma palavra-passe gerada para a conta de administrador da máquina virtual. Eis um exemplo para gerar uma palavra-passe:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    O Azure Key Vault foi criado para salvaguardar chaves criptográficos e outros segredos. Para obter mais informações, veja [Tutorial: Integrar o Azure Key Vault na implementação de modelos do Resource Manager](./resource-manager-tutorial-use-key-vault.md). Também recomendamos que atualize a palavra-passe a cada três meses.

## <a name="prepare-a-powershell-script"></a>Preparar o script do Powershell

Um script do PowerShell com o seguinte conteúdo é partilhado de uma [conta de Armazenamento do Azure com acesso público](https://armtutorials.blob.core.windows.net/usescriptextensions/installWebServer.ps1):

```azurepowershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Se optar por publicar o ficheiro na sua própria localização, tem de atualizar o elemento [fileUri] no modelo mais tarde no tutorial.

## <a name="open-a-quickstart-template"></a>Abrir um modelo de Início Rápido

Os Modelos de Início Rápido do Azure são um repositório de modelos do Resource Manager. Em vez de criar um modelo do zero, pode encontrar um modelo de exemplo e personalizá-lo. O modelo utilizado neste tutorial é denominado [Implementar uma VM do Windows simples](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/).

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

## <a name="edit-the-template"></a>Editar o modelo

Adicione um recurso de extensão de máquina virtual ao modelo existente com o seguinte conteúdo:

```json
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "[concat(variables('vmName'),'/', 'InstallWebServer')]",
    "location": "[parameters('location')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/',variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.7",
        "autoUpgradeMinorVersion":true,
        "settings": {
            "fileUris": [
                "https://armtutorials.blob.core.windows.net/usescriptextensions/installWebServer.ps1"
            ],
            "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File installWebServer.ps1"
        }
    }
}
```

Veja a [referência de extensão](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines/extensions) se precisar de mais informações sobre esta definição do recurso. Seguem alguns elementos importantes:

* **nome**: uma vez que o recurso de extensão é um recurso subordinado do objeto de máquina virtual, o nome tem de ter o prefixo do nome da máquina virtual. Veja [Recursos subordinados](./resource-manager-templates-resources.md#child-resources).
* **dependsOn**: o recurso de extensão tem de ser criado após a criação da máquina virtual.
* **fileUris**: estas são as localizações onde são armazenados os ficheiros de script. Se optar por não utilizar o ficheiro apresentado, tem de atualizar os valores.
* **commandToExecute**: este é o comando para invocar o script.  

## <a name="deploy-the-template"></a>Implementar o modelo

Veja a secção [Implementar o modelo](./resource-manager-tutorial-create-multiple-instances.md#deploy-the-template) para obter o procedimento de implementação. É recomendado que utilize uma palavra-passe gerada para a conta de administrador da máquina virtual. Veja [Pré-requisitos](#prerequisites).

## <a name="verify-the-deployment"></a>Verificar a implementação

No portal, selecione a VM e na descrição geral da VM, utilize a opção **Clicar para copiar** à direita do endereço IP para copiá-lo e colá-lo num separador do browser. A página de boas-vindas do IIS predefinida abre e deverá ser semelhante ao seguinte:

![O Azure Resource Manager implementa o servidor Web dos IIS de script de cliente de extensões da vm](./media/resource-manager-tutorial-deploy-vm-extensions/resource-manager-template-deploy-extensions-customer-script-web-server.png)

## <a name="clean-up-resources"></a>Limpar recursos

Quando os recursos do Azure já não forem necessários, limpe os recursos implementados ao eliminar o grupo de recursos.

1. No portal do Azure, selecione **Grupo de recursos** no menu à esquerda.
2. Introduza o nome do grupo de recursos no campo **Filtrar por nome**.
3. Selecione o nome do grupo de recursos.  Verá um total de seis recursos no grupo de recursos.
4. Selecione **Eliminar grupo de recursos** no menu superior.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, implementou uma máquina virtual e uma extensão de máquina virtual. A extensão instalou o servidor web dos IIS na máquina virtual. Para saber como utilizar a extensão da Base de Dados SQL para importar um ficheiro BACPAC, veja:

> [!div class="nextstepaction"]
> [](./resource-manager-tutorial-deploy-vm-extensions.md)

---
title: Como configurar MSI numa VM do Azure utilizando um modelo
description: Instruções passo a passo para configurar uma identidade de serviço geridas (MSI) na VM do Azure, utilizando um modelo Azure Resource Manager.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 521c5a3c0ad55afa0b71628195be7782b0e43b67
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="configure-a-vm-managed-service-identity-by-using-a-template"></a>Configurar uma identidade de serviço geridas da VM utilizando o modelo

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter de credenciais no seu código. 

Neste artigo, irá aprender a realizar as seguintes operações de identidade de serviço gerida numa VM do Azure, utilizando o modelo de implementação Azure Resource Manager:

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade de serviço geridas, consulte o [secção Descrição geral](overview.md). **Certifique-se rever o [diferença entre um sistema atribuída e o utilizador atribuído identidade](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscrever-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.

## <a name="azure-resource-manager-templates"></a>Modelos do Azure Resource Manager

Como com o Azure portal e scripts, modelos Azure Resource Manager fornecem a capacidade de implementar recursos novos ou modificados definidos por um grupo de recursos do Azure. Várias opções estão disponíveis para edição do modelo e implementação, local e baseado no portal, incluindo:

   - Utilizar um [modelo personalizado no Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), que lhe permite criar um modelo a partir do zero ou baseá-num comuns existente ou [modelo de início rápido](https://azure.microsoft.com/documentation/templates/).
   - Efetuar a derivação de um grupo de recursos existente ao exportar um modelo a partir [a implementação original](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), ou a partir de [estado atual da implementação](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Utilizar uma local [editor de JSON (por exemplo, o VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md)e, em seguida, carregar e implementar utilizando o PowerShell ou a CLI.
   - Com o Visual Studio [projeto do grupo de recursos do Azure](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) para criar e implementar um modelo.  

Independentemente da opção que escolher, sintaxe do modelo é o mesmo durante a implementação inicial e reimplementação. Ativar um sistema ou o utilizador atribuído identidade numa VM nova ou existente é feito da mesma forma. Também, por predefinição, o Azure Resource Manager efetua uma [atualização incremental](../../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) das implementações.

## <a name="system-assigned-identity"></a>Atribuído a identidade de sistema

Nesta secção, irá ativar e desativar um sistema atribuído de identidade com um modelo Azure Resource Manager.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Ativar o sistema atribuído identidade durante a criação de uma VM do Azure ou numa VM existente

1. Se iniciar sessão localmente no Azure ou através do portal do Azure, utilize uma conta que está associada à subscrição do Azure que contém a VM. Certifique-se também que a sua conta pertencer a uma função que dá-lhe permissões de escrita na VM (por exemplo, a função de "Contribuinte de Máquina Virtual").

2. Depois de carregar o modelo para um editor, localize o `Microsoft.Compute/virtualMachines` recursos de interesse no `resources` secção. Seu poderá ter o aspeto ligeiramente diferente da captura de ecrã seguinte, consoante o editor que está a utilizar e, se estiver a editar um modelo para uma implementação nova ou existente.

   >[!NOTE] 
   > Este exemplo assume variáveis como `vmName`, `storageAccountName`, e `nicName` definiu no modelo.
   >

   ![Captura de ecrã do modelo - localize VM](../media/msi-qs-configure-template-windows-vm/template-file-before.png) 

3. Para ativar o sistema atribuído de identidade, adicione o `"identity"` propriedade com o mesmo nível, como o `"type": "Microsoft.Compute/virtualMachines"` propriedade. Utilize a seguinte sintaxe:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. (Opcional) Adicione a extensão do MSI da VM como um `resources` elemento. Este passo é opcional como pode utilizar o ponto de final para a identidade de serviço de metadados de instância do Azure (IMDS), obtenha tokens bem.  Utilize a seguinte sintaxe:

   >[!NOTE] 
   > O exemplo seguinte assume uma extensão de VM do Windows (`ManagedIdentityExtensionForWindows`) está a ser implementada. Também pode configurar para Linux ao utilizar `ManagedIdentityExtensionForLinux` em vez disso, para o `"name"` e `"type"` elementos.
   >

   ```JSON
   { 
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
       "apiVersion": "2016-03-30",
       "location": "[resourceGroup().location]",
       "dependsOn": [
           "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
       ],
       "properties": {
           "publisher": "Microsoft.ManagedIdentity",
           "type": "ManagedIdentityExtensionForWindows",
           "typeHandlerVersion": "1.0",
           "autoUpgradeMinorVersion": true,
           "settings": {
               "port": 50342
           },
           "protectedSettings": {}
       }
   }
   ```

5. Quando estiver pronto, o modelo deve ter um aspeto semelhante ao seguinte:

   ![Captura de ecrã do modelo após a atualização](../media/msi-qs-configure-template-windows-vm/template-file-after.png)

### <a name="disable-a-system-assigned-identity-from-an-azure-vm"></a>Desativar um sistema atribuído a identidade de uma VM do Azure

> [!NOTE]
> Desativar a identidade de serviço gerida de uma Máquina Virtual não é atualmente suportada. Entretanto, pode alternar entre a utilização do sistema atribuídas e identidades atribuídas do utilizador.

Se tiver uma VM que já não tem uma identidade de serviço geridas:

1. Se iniciar sessão localmente no Azure ou através do portal do Azure, utilize uma conta que está associada à subscrição do Azure que contém a VM. Certifique-se também que a sua conta pertencer a uma função que dá-lhe permissões de escrita na VM (por exemplo, a função de "Contribuinte de Máquina Virtual").

2. Alterar o tipo de identidade para `UserAssigned`.

## <a name="user-assigned-identity"></a>Utilizador atribuído identidade

Nesta secção, vai criar uma identidade de utilizador e a VM do Azure com um modelo Azure Resource Manager.

 ### <a name="create-and-assign-a-user-assigned-identity-to-an-azure-vm"></a>Criar e atribuir um utilizador atribuído a identidade para uma VM do Azure

1. Execute primeiro o passo da secção [ativar uma identidade de sistema atribuída durante a criação de uma VM do Azure ou numa VM existente](#enable-system-assigned-identity-during-creation-of-an-azure-vm-or-on-an-existing-vm)

2.  Na secção de variáveis que contém as variáveis de configuração para a VM do Azure, adicione uma entrada para um nome de identidade atribuído utilizador semelhante ao seguinte.  Esta ação irá criar o utilizador atribuído identidade durante o processo de criação de VM do Azure:
    
    > [!IMPORTANT]
    > Criar atribuído identidades com carateres especiais (ou seja, um caráter de sublinhado) no nome de utilizador não é atualmente suportado. Utilize apenas carateres alfanuméricos. Verifique novamente para as atualizações.  Para obter mais informações consulte [perguntas mais frequentes e problemas conhecidos](known-issues.md)

    ```json
    "variables": {
        "vmName": "[parameters('vmName')]",
        //other vm configuration variables...
        "identityName": "[concat(variables('vmName'), 'id')]"
    ```

3. Sob o `resources` elemento, adicione a seguinte entrada para criar uma identidade de utilizador atribuída:

    ```json
    {
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
        "name": "[variables('identityName')]",
        "apiVersion": "2015-08-31-PREVIEW",
        "location": "[resourceGroup().location]"
    },
    ```

4. Em seguida, sob o `resources` elemento adicione a seguinte entrada para atribuir as extensões de identidade gerido para a VM:

    ```json
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForLinux')]",
        "apiVersion": "2015-05-01-preview",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
        ],
        "properties": {
            "publisher": "Microsoft.ManagedIdentity",
            "type": "ManagedIdentityExtensionForLinux",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "port": 50342
            }
        }
    }
    ```
5. Em seguida, adicione a seguinte entrada para atribuir o utilizador atribuído identidade para a VM:

    ```json
    {
        "apiVersion": "2017-12-01",
        "type": "Microsoft.Compute/virtualMachines",
        "name": "[variables('vmName')]",
        "location": "[resourceGroup().location]",
        "identity": {
            "type": "userAssigned",
            "identityIds": [
                "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities/', variables('identityName'))]"
            ]
        },
    ```
6.  Quando tiver terminado, o modelo deve ter um aspeto semelhante ao seguinte:
    > [!NOTE]
    > O modelo não listar todas as variáveis necessárias para criar a VM.  `//other configuration variables...` é utilizado in the place of todas as variáveis de configuração necessárias com vista à, uma forma abreviada.

      ![Captura de ecrã da identidade de utilizador atribuído](../media/msi-qs-configure-template-windows-vm/template-user-assigned-identity.png)


## <a name="related-content"></a>Conteúdo relacionado

- Para obter uma perspetiva mais abrangente sobre MSI, leia o [descrição geral de identidade de serviço geridas](overview.md).


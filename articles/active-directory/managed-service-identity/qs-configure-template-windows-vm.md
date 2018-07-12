---
title: Como configurar o MSI numa VM do Azure ao utilizar um modelo
description: Instruções passo a passo para configurar uma identidade de serviço gerida (MSI) na VM do Azure, com um modelo Azure Resource Manager.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: d8490dcba35cfeabb3da589f3d079571d5e98d3b
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969209"
---
# <a name="configure-a-vm-managed-service-identity-by-using-a-template"></a>Configurar uma identidade de serviço gerido da VM com um modelo

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, aprenderá a efetuar as seguintes operações de identidade do serviço gerido na VM do Azure, com o modelo de implementação Azure Resource Manager:

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre um sistema atribuído e a identidade atribuída ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [Inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.

## <a name="azure-resource-manager-templates"></a>Modelos do Azure Resource Manager

Tal como acontece com o portal do Azure e criação de scripts, [do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) os modelos fornecem a capacidade de implementar recursos novos ou modificados definidos por um grupo de recursos do Azure. Várias opções estão disponíveis para edição de modelo e a implantação, local e baseado no portal, incluindo:

   - Utilizar um [modelo personalizado do Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), que permite-lhe criar um modelo a partir do zero ou baseá-la num comum existente ou [modelo de início rápido](https://azure.microsoft.com/documentation/templates/).
   - Derivar de um grupo de recursos existente ao exportar um modelo a partir de qualquer uma [implementação original](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), ou a partir do [estado atual da implantação](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Com um local [editor de JSON (por exemplo, o VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md)e, em seguida, carregar e implementar com o PowerShell ou a CLI.
   - Com o Visual Studio [projeto do grupo de recursos do Azure](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) para criar e implementar um modelo.  

Independentemente da opção escolhida, sintaxe do modelo é o mesmo durante a implementação inicial e a reimplementação. Ativar um sistema ou identidade numa VM nova ou existente atribuída ao utilizador é feito da mesma forma. Além disso, por padrão, o Azure Resource Manager faz uma [atualização incremental](../../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) para implementações.

## <a name="system-assigned-identity"></a>Sistema de identidade atribuído

Nesta secção, irá ativar e desativar um sistema de identidade com um modelo Azure Resource Manager atribuído.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Ativar o sistema de identidade atribuído durante a criação de uma VM do Azure ou numa VM existente

1. Se iniciar sessão localmente no Azure ou através do portal do Azure, utilize uma conta que está associada à subscrição do Azure que contém a VM. Certifique-se também que a sua conta pertencer a uma função que dá-lhe permissões de escrita na VM (por exemplo, a função de "Contribuinte de Máquina Virtual").

2. Depois de carregar o modelo num editor, localize a `Microsoft.Compute/virtualMachines` recursos de interesse no `resources` secção. Sua poderá parecer ligeiramente diferente da captura de ecrã seguinte, consoante o editor que estiver a utilizar e se estiver a editar um modelo para uma implementação novo ou existente.

   >[!NOTE] 
   > Este exemplo assume variáveis como `vmName`, `storageAccountName`, e `nicName` foram definidos no modelo.
   >

   ![Captura de ecrã do modelo - localize a VM](../media/msi-qs-configure-template-windows-vm/template-file-before.png) 

3. Para ativar o sistema de identidade atribuído, adicione a `"identity"` propriedade o mesmo nível, como o `"type": "Microsoft.Compute/virtualMachines"` propriedade. Utilize a seguinte sintaxe:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. (Opcional) Adicione a extensão MSI de VM como um `resources` elemento. Este passo é opcional, como pode usar o ponto de extremidade para a identidade de serviço de metadados de instância do Azure (IMDS), para obtenção de tokens também.  Utilize a seguinte sintaxe:

   >[!NOTE] 
   > O exemplo a seguir supõe uma extensão de VM do Windows (`ManagedIdentityExtensionForWindows`) está a ser implementado. Também pode configurar para Linux, utilizando `ManagedIdentityExtensionForLinux` em vez disso, para o `"name"` e `"type"` elementos.
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

5. Quando tiver terminado, o modelo deve ter um aspeto semelhante ao seguinte:

   ![Captura de ecrã do modelo após atualização](../media/msi-qs-configure-template-windows-vm/template-file-after.png)

### <a name="assign-a-role-the-vms-system-assigned-identity"></a>Atribuir uma função de identidade de sistema atribuído da VM

Depois de ativar uma identidade de sistema atribuída na sua VM, poderá ser útil para o conceder uma função como **leitor** acesso ao grupo de recursos no qual foi criado.

1. Se iniciar sessão localmente no Azure ou através do portal do Azure, utilize uma conta que está associada à subscrição do Azure que contém a VM. Além disso, certifique-se de que a sua conta pertencer a uma função que dá-lhe permissões de escrita na VM (por exemplo, a função de "Contribuinte de Máquina Virtual").
 
2. Carregar o modelo para um [editor](#azure-resource-manager-templates) e adicione as seguintes informações para dar a sua VM **leitor** acesso ao grupo de recursos no qual foi criado.  A estrutura do modelo pode variar consoante o editor e o modelo de implementação que escolher.
   
   Sob o `parameters` secção adicione o seguinte:

    ```JSON
    "builtInRoleType": {
          "type": "string",
          "defaultValue": "Reader"
        },
        "rbacGuid": {
          "type": "string"
        }
    ```

    Sob o `variables` secção adicione o seguinte:

    ```JSON
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
    ```

    Sob o `resources` secção adicione o seguinte:

    ```JSON
    {
        "apiVersion": "2017-09-01",
         "type": "Microsoft.Authorization/roleAssignments",
         "name": "[parameters('rbacGuid')]",
         "properties": {
                "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
                "principalId": "[reference(variables('vmResourceId'), '2017-12-01', 'Full').identity.principalId]",
                "scope": "[resourceGroup().id]"
          },
          "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
            ]
    }
    ```

### <a name="disable-a-system-assigned-identity-from-an-azure-vm"></a>Desativar um sistema de identidade atribuído a partir de uma VM do Azure

Se tiver uma VM que não precisa mais de uma identidade de serviço gerido:

1. Se iniciar sessão localmente no Azure ou através do portal do Azure, utilize uma conta que está associada à subscrição do Azure que contém a VM. Certifique-se também que a sua conta pertencer a uma função que dá-lhe permissões de escrita na VM (por exemplo, a função de "Contribuinte de Máquina Virtual").

2. Carregar o modelo para um [editor](#azure-resource-manager-templates) e localize a `Microsoft.Compute/virtualMachines` recursos de interesse no `resources` secção. Se tiver uma VM que tem apenas uma identidade de sistema atribuído, pode desativá-lo ao alterar o tipo de identidade para `None`.  Se a VM tem o sistema e as identidades de atribuída ao utilizador, remova `SystemAssigned` do tipo de identidade e mantenha `UserAssigned` juntamente com o `identityIds` matriz das identidades de utilizador atribuída.  O exemplo seguinte mostra como remover um sistema de identidade atribuído a partir de uma VM com nenhum utilizador identidades atribuída:
   
   ```JSON
    {
      "apiVersion": "2017-12-01",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "identity": { 
          "type": "None"
    }
   ```

## <a name="user-assigned-identity"></a>Identidade atribuída ao utilizador

Nesta secção, atribua uma identidade de utilizador atribuída a uma VM do Azure com o modelo Azure Resource Manager.

> [!Note]
> Para criar uma identidade de utilizador atribuída com um modelo de Gestor de recursos do Azure, veja [criar uma identidade atribuída ao utilizador](how-to-manage-ua-identity-arm.md#create-a-user-assigned-identity).

 ### <a name="assign-a-user-assigned-identity-to-an-azure-vm"></a>Atribuir um utilizador atribuído a identidade para uma VM do Azure

1. Sob o `resources` elemento, adicione a seguinte entrada para atribuir uma identidade de utilizador atribuída à sua VM.  Certifique-se de que substitua `<USERASSIGNEDIDENTITY>` com o nome da identidade atribuída ao utilizador que criou.
    ```json
    {
        "apiVersion": "2017-12-01",
        "type": "Microsoft.Compute/virtualMachines",
        "name": "[variables('vmName')]",
        "location": "[resourceGroup().location]",
        "identity": {
            "type": "userAssigned",
            "identityIds": [
                "[resourceID('Micrososft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
            ]
        },
    ```
    
2. (Opcional) Em seguida, sob o `resources` elemento, adicione a seguinte entrada para atribuir a extensão de identidade gerida para a VM. Este passo é opcional, como pode usar o ponto de extremidade para a identidade de serviço de metadados de instância do Azure (IMDS), para obtenção de tokens também. Utilize a seguinte sintaxe:
    ```json
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
        "apiVersion": "2015-05-01-preview",
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
            }
        }
    }
    ```
    
3.  Quando tiver terminado, o modelo deve ter um aspeto semelhante ao seguinte:

      ![Captura de ecrã da identidade atribuída ao utilizador](./media/qs-configure-template-windows-vm/qs-configure-template-windows-vm-ua-final.PNG)


## <a name="related-content"></a>Conteúdo relacionado

- Para obter uma perspectiva mais ampla sobre o MSI, leia os [descrição geral de identidade do serviço gerido](overview.md).


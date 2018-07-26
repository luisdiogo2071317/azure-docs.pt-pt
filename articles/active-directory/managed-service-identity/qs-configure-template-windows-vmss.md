---
title: Configurar a identidade do serviço gerido num conjunto com um modelo de dimensionamento de máquina virtual do Azure
description: Instruções passo a passo para configurar uma identidade do serviço gerido num VMSS do Azure, com um modelo Azure Resource Manager.
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
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: 562bf5e5239114a8dad16727089f94f378db82ff
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/26/2018
ms.locfileid: "39258869"
---
# <a name="configure-managed-service-identity-on-virtual-machine-scale-using-a-template"></a>Configurar a identidade do serviço gerido no dimensionamento de máquinas virtuais com um modelo

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, saiba como realizar as seguintes operações de identidade do serviço gerido num conjunto de dimensionamento de máquina virtual do Azure, utilizando o modelo de implementação Azure Resource Manager:
- Ativar e desativar a identidade do sistema atribuído num conjunto de dimensionamento de máquina virtual do Azure
- Adicionar e remover uma identidade de utilizador atribuída num conjunto de dimensionamento de máquina virtual do Azure

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre um sistema atribuído e a identidade atribuída ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar as operações de gestão neste artigo, a conta tem das atribuições de funções seguintes:
    - [Contribuinte de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) criar um dimensionamento de máquina virtual definido e ativar e remover sistema e/ou utilizador atribuídos identidade gerida a partir de um conjunto de dimensionamento de máquina virtual.
    - [Contribuidor de identidade de geridos](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) função para criar uma identidade atribuída ao utilizador.
    - [Gerido operador de identidade](/azure/role-based-access-control/built-in-roles#managed-identity-operator) função para atribuir e remover uma identidade de utilizador atribuída de e para um conjunto de dimensionamento de máquina virtual.

## <a name="azure-resource-manager-templates"></a>Modelos do Azure Resource Manager

Tal como acontece com o portal do Azure e criação de scripts, [do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) os modelos fornecem a capacidade de implementar recursos novos ou modificados definidos por um grupo de recursos do Azure. Várias opções estão disponíveis para edição de modelo e a implantação, local e baseado no portal, incluindo:

   - Utilizar um [modelo personalizado do Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), que permite-lhe criar um modelo a partir do zero ou baseá-la num comum existente ou [modelo de início rápido](https://azure.microsoft.com/documentation/templates/).
   - Derivar de um grupo de recursos existente ao exportar um modelo a partir de qualquer uma [implementação original](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), ou a partir do [estado atual da implantação](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Com um local [editor de JSON (por exemplo, o VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md)e, em seguida, carregar e implementar com o PowerShell ou a CLI.
   - Com o Visual Studio [projeto do grupo de recursos do Azure](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) para criar e implementar um modelo.  

Independentemente da opção escolhida, sintaxe do modelo é o mesmo durante a implementação inicial e a reimplementação. Ativar a identidade do serviço gerido numa VM nova ou existente é feito da mesma forma. Além disso, por padrão, o Azure Resource Manager faz uma [atualização incremental](../../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) para implementações.

## <a name="system-assigned-identity"></a>Sistema de identidade atribuído

Nesta secção, irá ativar e desativar o sistema de identidade com um modelo Azure Resource Manager atribuído.

### <a name="enable-system-assigned-identity-during-creation-the-creation-of-or-an-existing-azure-virtual-machine-scale-set"></a>Ativar sistema identidade atribuída durante a criação da criação de ou um conjunto de dimensionamento de máquina virtual do Azure existente

1. Carregar o modelo para um editor, localize a `Microsoft.Compute/virtualMachineScaleSets` recursos de interesse no `resources` secção. Sua poderá parecer ligeiramente diferente da captura de ecrã seguinte, consoante o editor que estiver a utilizar e se estiver a editar um modelo para uma implementação novo ou existente.
   
   ![Captura de ecrã do modelo - localize a VM](../managed-service-identity/media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-before-vmss.png) 

2. Para ativar a identidade do sistema atribuído, adicione a `"identity"` propriedade o mesmo nível, como o `"type": "Microsoft.Compute/virtualMachineScaleSets"` propriedade. Utilize a seguinte sintaxe:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

3. (Opcional) Adicionar conjunto de dimensionamento da máquina virtual de extensão de identidade do serviço gerido como um `extensionsProfile` elemento. Este passo é opcional, como pode usar a identidade de serviço de metadados de instância do Azure (IMDS), para obtenção de tokens também.  Utilize a seguinte sintaxe:

   >[!NOTE] 
   > O exemplo a seguir supõe a extensão do conjunto de dimensionamento de máquinas virtuais do Windows (`ManagedIdentityExtensionForWindows`) está a ser implementado. Também pode configurar para Linux, utilizando `ManagedIdentityExtensionForLinux` em vez disso, para o `"name"` e `"type"` elementos.
   >

   ```JSON
   "extensionProfile": {
        "extensions": [
            {
                "name": "MSIWindowsExtension",
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

4. Quando tiver terminado, o modelo deve ter um aspeto semelhante ao seguinte:

   ![Captura de ecrã do modelo após atualização](../managed-service-identity/media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-after-vmss.png) 

### <a name="disable-a-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Desativar uma identidade de sistema atribuída de um conjunto de dimensionamento de máquina virtual do Azure

Se tiver um conjunto de dimensionamento que já não precisa de uma identidade de serviço gerido:

1. Se iniciar sessão para o Azure localmente ou através do portal do Azure, utilize uma conta que está associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual.

2. Carregar o modelo para um [editor](#azure-resource-manager-templates) e localize a `Microsoft.Compute/virtualMachineScaleSets` recursos de interesse no `resources` secção. Se tiver um conjunto de dimensionamento de máquina virtual que tem apenas uma identidade de sistema atribuído, pode desativá-lo ao alterar o tipo de identidade para `None`.  Se o conjunto de dimensionamento de máquina virtual tem o sistema e as identidades de atribuída ao utilizador, remova `SystemAssigned` do tipo de identidade e mantenha `UserAssigned` juntamente com o `identityIds` matriz das identidades de utilizador atribuída.  O exemplo seguinte mostra como remover um sistema de identidade atribuído de um conjunto de dimensionamento com nenhum utilizador identidades atribuída:
   
   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2017-03-30",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "None"
        }

   }
   ```

## <a name="user-assigned-identity"></a>Identidade atribuída ao utilizador

Nesta secção, atribua uma identidade de utilizador atribuída a um VMSS do Azure com o modelo Azure Resource Manager.

> [!Note]
> Para criar uma identidade de utilizador atribuída com um modelo de Gestor de recursos do Azure, veja [criar uma identidade atribuída ao utilizador](how-to-manage-ua-identity-arm.md#create-a-user-assigned-identity).

### <a name="assign-a-user-assigned-identity-to-an-azure-vmss"></a>Atribuir um utilizador atribuído a identidade para um VMSS do Azure

1. Sob o `resources` elemento, adicione a seguinte entrada para atribuir uma identidade atribuída ao utilizador para o VMSS.  Certifique-se de que substitua `<USERASSIGNEDIDENTITY>` com o nome da identidade atribuída ao utilizador que criou.

   > [!Important]
   > O `<USERASSIGNEDIDENTITYNAME>` valor mostrado no exemplo a seguir deve ser armazenado numa variável.  Além disso, para a implementação suportada atualmente de atribuição de identidades de utilizador atribuída a uma máquina virtual num modelo do Resource Manager, a versão de api deve corresponder à versão no exemplo a seguir. 

    ```json
    {
        "name": "[variables('vmssName')]",
        "apiVersion": "2017-03-30",
        "location": "[parameters(Location')]",
        "identity": {
            "type": "userAssigned",
            "identityIds": [
                "[resourceID('Micrososft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITY>'))]"
            ]
        }

    }
    ```

2. (Opcional) Adicione a entrada seguinte sob o `extensionProfile` elemento para atribuir a extensão de identidade gerida para o VMSS. Este passo é opcional, como pode usar o ponto de extremidade para a identidade de serviço de metadados de instância do Azure (IMDS), para obtenção de tokens também. Utilize a seguinte sintaxe:
   
    ```JSON
       "extensionProfile": {
            "extensions": [
                {
                    "name": "MSIWindowsExtension",
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

3.  Quando tiver terminado, o modelo deve ter um aspeto semelhante ao seguinte:
   
      ![Captura de ecrã da identidade atribuída ao utilizador](./media/qs-configure-template-windows-vmss/qs-configure-template-windows-final.PNG)

### <a name="remove-user-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Remover a identidade atribuída ao utilizador um conjunto de dimensionamento de máquina virtual do Azure

Se tiver um conjunto de dimensionamento que já não precisa de uma identidade de serviço gerido:

1. Se iniciar sessão para o Azure localmente ou através do portal do Azure, utilize uma conta que está associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual.

2. Carregar o modelo para um [editor](#azure-resource-manager-templates) e localize a `Microsoft.Compute/virtualMachineScaleSets` recursos de interesse no `resources` secção. Se tiver um conjunto de dimensionamento de máquina virtual que tenha apenas a identidade atribuída ao utilizador, pode desativá-lo ao alterar o tipo de identidade para `None`.  Se o conjunto de dimensionamento de máquina virtual tem o sistema e o utilizador identidades atribuídas e gostaria de manter uma identidade de sistema atribuído, remova `UserAssigned` do tipo de identidade, juntamente com o `identityIds` matriz das identidades de utilizador atribuída.
    
   Para remover um uma identidade de utilizador único atribuído a partir de um conjunto de dimensionamento de máquina virtual, remova-a do `identityIds` matriz.
   
   O exemplo seguinte mostra como remover utilizador de todas as identidades atribuídas a partir de um conjunto de dimensionamento sem nenhum sistema de identidades atribuído:
   
   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2017-03-30",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "None"
        }

   }
   ```

## <a name="next-steps"></a>Passos Seguintes

- Para obter uma perspectiva mais ampla sobre a identidade do serviço gerido, leia os [descrição geral de identidade do serviço gerido](overview.md).


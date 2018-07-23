---
title: Configurar MSI num conjunto com um modelo de dimensionamento de máquina virtual do Azure
description: Instruções passo a passo para configurar uma identidade de serviço gerida (MSI) num VMSS do Azure, com um modelo Azure Resource Manager.
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
ms.openlocfilehash: 994a146f7ecf0d3b37139bd6fe5d3c17c6a7bf59
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186411"
---
# <a name="configure-a-vmss-managed-service-identity-by-using-a-template"></a>Configurar uma identidade do serviço gerido VMSS utilizando um modelo

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, aprenderá a efetuar as seguintes operações de identidade do serviço gerido num VMSS do Azure, utilizando o modelo de implementação Azure Resource Manager:
- Ativar e desativar o sistema de identidade no VMSS Azure atribuído
- Adicionar e remover um utilizador atribuído a identidade num VMSS do Azure

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre um sistema atribuído e a identidade atribuída ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar as operações de gestão neste artigo, a conta tem das atribuições de funções seguintes:
    - [Contribuinte de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para criar um conjunto de dimensionamento de máquinas virtuais e ativar e remover sistema atribuído a identidade gerida a partir de um conjunto de dimensionamento de máquina virtual.
    - [Contribuidor de identidade de geridos](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) função para criar uma identidade atribuída ao utilizador.
    - [Gerido operador de identidade](/azure/role-based-access-control/built-in-roles#managed-identity-operator) função para atribuir e remover uma identidade de utilizador atribuída de e para um conjunto de dimensionamento de máquina virtual.

## <a name="azure-resource-manager-templates"></a>Modelos do Azure Resource Manager

Tal como acontece com o portal do Azure e criação de scripts, [do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) os modelos fornecem a capacidade de implementar recursos novos ou modificados definidos por um grupo de recursos do Azure. Várias opções estão disponíveis para edição de modelo e a implantação, local e baseado no portal, incluindo:

   - Utilizar um [modelo personalizado do Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), que permite-lhe criar um modelo a partir do zero ou baseá-la num comum existente ou [modelo de início rápido](https://azure.microsoft.com/documentation/templates/).
   - Derivar de um grupo de recursos existente ao exportar um modelo a partir de qualquer uma [implementação original](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), ou a partir do [estado atual da implantação](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Com um local [editor de JSON (por exemplo, o VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md)e, em seguida, carregar e implementar com o PowerShell ou a CLI.
   - Com o Visual Studio [projeto do grupo de recursos do Azure](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) para criar e implementar um modelo.  

Independentemente da opção escolhida, sintaxe do modelo é o mesmo durante a implementação inicial e a reimplementação. Ativar o MSI numa VM nova ou existente é feito da mesma forma. Além disso, por padrão, o Azure Resource Manager faz uma [atualização incremental](../../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) para implementações.

## <a name="system-assigned-identity"></a>Sistema de identidade atribuído

Nesta secção, irá ativar e desativar o sistema de identidade com um modelo Azure Resource Manager atribuído.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vmss-or-an-existing-azure-vmss"></a>Ativar o sistema de identidade atribuído durante a criação de um VMSS do Azure ou um VMSS do Azure existente

1. Carregar o modelo para um editor, localize a `Microsoft.Compute/virtualMachineScaleSets` recursos de interesse no `resources` secção. Sua poderá parecer ligeiramente diferente da captura de ecrã seguinte, consoante o editor que estiver a utilizar e se estiver a editar um modelo para uma implementação novo ou existente.
   
   ![Captura de ecrã do modelo - localize a VM](../managed-service-identity/media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-before-vmss.png) 

2. Para ativar a identidade do sistema atribuído, adicione a `"identity"` propriedade o mesmo nível, como o `"type": "Microsoft.Compute/virtualMachineScaleSets"` propriedade. Utilize a seguinte sintaxe:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

3. (Opcional) Adicionar conjunto de dimensionamento da máquina virtual extensão MSI como um `extensionsProfile` elemento. Este passo é opcional, como pode usar a identidade de serviço de metadados de instância do Azure (IMDS), para obtenção de tokens também.  Utilize a seguinte sintaxe:

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

> [!NOTE]
> A desativar a identidade de serviço gerida de uma Máquina Virtual não é atualmente suportada. Entretanto, pode alternar entre a utilização do sistema atribuído e identidades de utilizador atribuída.

Se tiver um conjunto de dimensionamento que já não necessita de um sistema de identidade atribuído, mas ainda precisa de utilizador identidades atribuído:

- Carregar o modelo para um editor e altere o tipo de identidade para `'UserAssigned'`

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

## <a name="next-steps"></a>Passos Seguintes

- Para obter uma perspectiva mais ampla sobre o MSI, leia os [descrição geral de identidade do serviço gerido](overview.md).


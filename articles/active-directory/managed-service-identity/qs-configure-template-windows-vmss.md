---
title: Configurar MSI num conjunto utilizando um modelo de dimensionamento de máquina virtual do Azure
description: Instruções passo a passo para configurar uma identidade de serviço geridas (MSI) num VMSS do Azure, utilizando um modelo Azure Resource Manager.
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
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: b95f5bb2aa93fb29999994ccd83dc898f88f1072
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="configure-a-vmss-managed-service-identity-by-using-a-template"></a>Configurar uma identidade de serviço geridas VMSS utilizando um modelo

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter de credenciais no seu código. 

Neste artigo, irá aprender a efetuar as seguintes operações de identidade de serviço geridas num VMSS do Azure, utilizando o modelo de implementação Azure Resource Manager:
- Ativar e desativar o sistema atribuído a identidade de um VMSS do Azure
- Adicionar e remover um utilizador atribuído a identidade de um VMSS do Azure

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade de serviço geridas, consulte o [secção Descrição geral](overview.md). **Certifique-se rever o [diferença entre um sistema atribuída e o utilizador atribuído identidade](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscrever-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.

## <a name="azure-resource-manager-templates"></a>Modelos do Azure Resource Manager

Tal como acontece com o Azure portal e scripts, [do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) modelos fornecem a capacidade de implementar recursos novos ou modificados definidos por um grupo de recursos do Azure. Várias opções estão disponíveis para edição do modelo e implementação, local e baseado no portal, incluindo:

   - Utilizar um [modelo personalizado no Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), que lhe permite criar um modelo a partir do zero ou baseá-num comuns existente ou [modelo de início rápido](https://azure.microsoft.com/documentation/templates/).
   - Efetuar a derivação de um grupo de recursos existente ao exportar um modelo a partir [a implementação original](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), ou a partir de [estado atual da implementação](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Utilizar uma local [editor de JSON (por exemplo, o VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md)e, em seguida, carregar e implementar utilizando o PowerShell ou a CLI.
   - Com o Visual Studio [projeto do grupo de recursos do Azure](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) para criar e implementar um modelo.  

Independentemente da opção que escolher, sintaxe do modelo é o mesmo durante a implementação inicial e reimplementação. Ativar MSI numa VM nova ou existente é feito da mesma forma. Também, por predefinição, o Azure Resource Manager efetua uma [atualização incremental](../../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) das implementações.

## <a name="system-assigned-identity"></a>Atribuído a identidade de sistema

Nesta secção, irá ativar e desativar o sistema atribuído de identidade com um modelo Azure Resource Manager.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vmss-or-an-existing-azure-vmss"></a>Ativar o sistema atribuído identidade durante a criação de um VMSS do Azure ou um VMSS existente do Azure

1. Carregar o modelo para um editor, localize o `Microsoft.Compute/virtualMachineScaleSets` recursos de interesse no `resources` secção. Seu poderá ter o aspeto ligeiramente diferente da captura de ecrã seguinte, consoante o editor que está a utilizar e, se estiver a editar um modelo para uma implementação nova ou existente.
   
   ![Captura de ecrã do modelo - localize VM](../media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-before-vmss.png) 

2. Para ativar a identidade do sistema atribuído, adicione o `"identity"` propriedade com o mesmo nível, como o `"type": "Microsoft.Compute/virtualMachineScaleSets"` propriedade. Utilize a seguinte sintaxe:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

3. (Opcional) Adicionar o dimensionamento da máquina virtual Definir extensão do MSI como um `extensionsProfile` elemento. Este passo é opcional como pode utilizar a identidade de serviço de metadados de instância do Azure (IMDS), obtenha tokens bem.  Utilize a seguinte sintaxe:

   >[!NOTE] 
   > O exemplo seguinte parte do princípio de extensão de definir um dimensionamento de máquina virtual do Windows (`ManagedIdentityExtensionForWindows`) está a ser implementada. Também pode configurar para Linux ao utilizar `ManagedIdentityExtensionForLinux` em vez disso, para o `"name"` e `"type"` elementos.
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

4. Quando estiver pronto, o modelo deve ter um aspeto semelhante ao seguinte:

   ![Captura de ecrã do modelo após a atualização](../media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-after-vmss.png) 

### <a name="disable-a-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Desativar uma identidade de sistema atribuída de um conjunto de dimensionamento de máquina virtual do Azure

> [!NOTE]
> Desativar a identidade de serviço gerida de uma Máquina Virtual não é atualmente suportada. Entretanto, pode alternar entre a utilização do sistema atribuídas e identidades atribuídas do utilizador.

Se tiver um conjunto que já não é de dimensionamento de máquina virtual necessita de um sistema atribuído de identidade, mas ainda tem atribuído identidades de utilizador:

- Carregar o modelo para um editor e alterar o tipo de identidade para `'UserAssigned'`

## <a name="user-assigned-identity"></a>Utilizador atribuído identidade

Nesta secção, pode designar uma identidade de utilizador atribuída para um VMSS do Azure utilizando o modelo Azure Resource Manager.

> [!Note]
> Para criar uma identidade de utilizador atribuído através de um modelo de Gestor de recursos do Azure, consulte [criar uma identidade de utilizador atribuída](how-to-manage-ua-identity-arm.md#create-a-user-assigned-identity).

### <a name="assign-a-user-assigned-identity-to-an-azure-vmss"></a>Atribuir um utilizador atribuído identidade um VMSS do Azure

1. Sob o `resources` elemento, adicione a seguinte entrada para atribuir uma identidade de utilizador atribuída ao seu VMSS.  Não se esqueça de substituir `<USERASSIGNEDIDENTITY>` com o nome da identidade atribuída ao utilizador que criou.

    ```json
    {
        "name": "[variables('vmssName')]",
        "apiVersion": "2017-03-30",
        "location": "[parameters(Location')]",
        "identity": {
            "type": "userAssigned",
            "identityIds": [
                "[resourceID('Micrososft.ManagedIdentity/userAssignedIdentities/<USERASSIGNEDIDENTITY>)']"
            ]
        }

    }
    ```
2. (Opcional) Adicione a seguinte entrada sob o `extensionProfile` elemento para atribuir a extensão de identidade gerido a sua VMSS. Este passo é opcional como pode utilizar o ponto de final para a identidade de serviço de metadados de instância do Azure (IMDS), obtenha tokens bem. Utilize a seguinte sintaxe:
   
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
   
      ![Captura de ecrã da identidade de utilizador atribuído](./media/qs-configure-template-windows-vmss/qs-configure-template-windows-final.PNG)

## <a name="next-steps"></a>Passos Seguintes

- Para obter uma perspetiva mais abrangente sobre MSI, leia o [descrição geral de identidade de serviço geridas](overview.md).


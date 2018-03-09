---
title: "Configurar MSI num conjunto utilizando um modelo de dimensionamento de máquina virtual do Azure"
description: "Instruções passo a passo para configurar uma identidade de serviço geridas (MSI) num VMSS do Azure, utilizando um modelo Azure Resource Manager."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: 669348213e00cb338c69156f37779767c681a227
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/07/2018
---
# <a name="configure-a-vm-managed-service-identity-by-using-a-template"></a>Configurar uma identidade de serviço geridas da VM utilizando o modelo

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço geridas (MSI) fornece serviços do Azure com uma identidade gerido automaticamente no Azure Active Directory (Azure AD). Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter de credenciais no seu código. 

Neste artigo, irá aprender a ativar e remover MSI para um conjunto de dimensionamento da máquina virtual do Azure, utilizando um modelo de implementação Azure Resource Manager.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="enable-msi-during-creation-of-an-azure-virtual-machine-scale-set-or-an-existing-azure-virtual-machine-scale-set"></a>Ativar MSI durante a criação de um conjunto de dimensionamento da máquina virtual do Azure ou um conjunto de dimensionamento de máquina virtual do Azure existente

Como com o Azure portal e scripts, modelos Azure Resource Manager fornecem a capacidade de implementar recursos novos ou modificados definidos por um grupo de recursos do Azure. Várias opções estão disponíveis para edição do modelo e implementação, local e baseado no portal, incluindo:

   - Utilizar um [modelo personalizado no Azure Marketplace](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), que lhe permite criar um modelo a partir do zero ou baseá-num comuns existente ou [modelo de início rápido](https://azure.microsoft.com/documentation/templates/).
   - Efetuar a derivação de um grupo de recursos existente ao exportar um modelo a partir [a implementação original](../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), ou a partir de [estado atual da implementação](../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Utilizar uma local [editor de JSON (por exemplo, o VS Code)](../azure-resource-manager/resource-manager-create-first-template.md)e, em seguida, carregar e implementar utilizando o PowerShell ou a CLI.
   - Com o Visual Studio [projeto do grupo de recursos do Azure](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) para criar e implementar um modelo.  

Independentemente da opção que escolher, sintaxe do modelo é o mesmo durante a implementação inicial e reimplementação. Ativar o MSI um conjunto de dimensionamento de máquina virtual do Azure nova ou existente é feito da mesma forma. Também, por predefinição, o Azure Resource Manager efetua uma [atualização incremental](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) para implementações:

1. Se iniciar sessão Azure localmente ou através do portal do Azure, utilize uma conta que está associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual.

2. Depois de carregar o modelo para um editor, localize o `Microsoft.Compute/virtualMachineScaleSets` recursos de interesse no `resources` secção. Seu poderá ter o aspeto ligeiramente diferente da captura de ecrã seguinte, consoante o editor que está a utilizar e, se estiver a editar um modelo para uma implementação nova ou existente.
   
   ![Captura de ecrã do modelo - localize VM](./media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-before-vmss.png) 

3. Adicionar o `"identity"` propriedade com o mesmo nível, como o `"type": "Microsoft.Compute/virtualMachineScaleSets"` propriedade. Utilize a seguinte sintaxe:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. Em seguida, adicione o dimensionamento da máquina virtual Definir extensão do MSI como um `extensionsProfile` elemento. Utilize a seguinte sintaxe:

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

5. Quando estiver pronto, o modelo deve ter um aspeto semelhante ao seguinte:

   ![Captura de ecrã do modelo após a atualização](./media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-after-vmss.png) 

## <a name="remove-msi-from-an-azure-virtual-machine-scale-set"></a>Remover MSI a partir de um conjunto de dimensionamento da máquina virtual do Azure

Se tiver um conjunto de dimensionamento de máquina virtual que já não necessita de um MSI:

1. Se iniciar sessão Azure localmente ou através do portal do Azure, utilize uma conta que está associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual.

2. Remova os dois elementos que foram adicionados na secção anterior: o conjunto de dimensionamento de máquina virtual `"identity"` e `"extensionsProfile"` propriedades.

## <a name="next-steps"></a>Passos Seguintes

- Para obter uma perspetiva mais abrangente sobre MSI, leia o [descrição geral de identidade de serviço geridas](msi-overview.md).


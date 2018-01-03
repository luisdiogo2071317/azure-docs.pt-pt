---
title: "Como configurar um MSI atribuído por utilizador para uma VM do Azure através de um modelo do Azure"
description: "Passo pelo passo as instruções para configurar um utilizador atribuído geridos serviço de identidade (MSI) para uma VM do Azure, utilizando um modelo Azure Resource Manager."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: d97f0fa2d6c1c92aaa3d5c74dd6715de00d32438
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/22/2017
---
# <a name="configure-a-user-assigned-managed-service-identity-msi-for-a-vm-using-an-azure-template"></a>Configurar um utilizador atribuído geridos serviço de identidade (MSI) para uma VM, utilizando um modelo do Azure

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido no Azure Active Directory. Pode utilizar esta identidade para se autenticarem em serviços que suportam a autenticação do Azure AD, sem necessitar de credenciais no seu código. 

Neste artigo, irá aprender a ativar e remover um MSI atribuído por utilizador para uma VM do Azure, utilizando um modelo de implementação Azure Resource Manager.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Ativar MSI durante a criação de uma VM do Azure ou numa VM existente

Como com o Azure portal e scripts, modelos Azure Resource Manager fornecem a capacidade de implementar recursos novos ou modificados definidos por um grupo de recursos do Azure. Várias opções estão disponíveis para edição do modelo e implementação, local e baseado no portal, incluindo:

   - Utilizar um [modelo personalizado no Azure Marketplace](~/articles/azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), que lhe permite criar um modelo a partir do zero ou baseá-num comuns existente ou [modelo de início rápido](https://azure.microsoft.com/documentation/templates/).
   - Efetuar a derivação de um grupo de recursos existente ao exportar um modelo a partir [a implementação original](~/articles/azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), ou a partir de [estado atual da implementação](~/articles/azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Utilizar uma local [editor de JSON (por exemplo, o VS Code)](~/articles/azure-resource-manager/resource-manager-create-first-template.md)e, em seguida, carregar e implementar utilizando o PowerShell ou a CLI.
   - Com o Visual Studio [projeto do grupo de recursos do Azure](~/articles/azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) para criar e implementar um modelo.  

Independentemente da opção que escolher, sintaxe do modelo é o mesmo durante a implementação inicial e reimplementação. Criar e atribuir um MSI utilizador atribuído a uma VM nova ou existente é feito da mesma forma. Também, por predefinição, o Azure Resource Manager efetua uma [atualização incremental](~/articles/azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) para implementações:

1. Se iniciar sessão localmente no Azure ou através do portal do Azure, utilize uma conta que está associada à subscrição do Azure que contém o MSI e a VM. Certifique-se também que a sua conta pertencer a uma função que dá-lhe permissões de escrita na subscrição ou recursos (por exemplo, a função de "proprietário").

2. Depois de carregar o modelo para um editor, localize o `Microsoft.Compute/virtualMachines` recursos de interesse no `resources` secção. Seu poderá ter o aspeto ligeiramente diferente da captura de ecrã seguinte, consoante o editor que está a utilizar e, se estiver a editar um modelo para uma implementação nova ou existente.

   >[!NOTE] 
   > Este exemplo assume variáveis como `vmName`, `storageAccountName`, e `nicName` definiu no modelo.
   >

   ![Captura de ecrã do modelo - localize VM](~/articles/active-directory/media/msi-qs-configure-template-windows-vm/template-file-before.png) 

3. Adicionar o `"identity"` propriedade com o mesmo nível, como o `"type": "Microsoft.Compute/virtualMachines"` propriedade. Utilize a seguinte sintaxe:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. Em seguida, adicione a extensão do MSI da VM como um `resources` elemento. Utilize a seguinte sintaxe:

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

   ![Captura de ecrã do modelo após a atualização](~/articles/active-directory/media/msi-qs-configure-template-windows-vm/template-file-after.png) 

## <a name="remove-msi-from-an-azure-vm"></a>Remover MSI a partir de uma VM do Azure

Se tiver uma VM que já não necessita de um MSI:

1. Se iniciar sessão localmente no Azure ou através do portal do Azure, utilize uma conta que está associada à subscrição do Azure que contém a VM. Certifique-se também que a sua conta pertencer a uma função que dá-lhe permissões de escrita na VM (por exemplo, a função de "Contribuinte de Máquina Virtual").

2. Remova os dois elementos que foram adicionados na secção anterior: a VM `"identity"` propriedade e o `"Microsoft.Compute/virtualMachines/extensions"` recursos.

## <a name="related-content"></a>Conteúdo relacionado

- Para obter uma perspetiva mais abrangente sobre MSI, leia o [descrição geral de identidade de serviço geridas](msi-overview.md).


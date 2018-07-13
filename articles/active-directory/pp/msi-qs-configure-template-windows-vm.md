---
title: Como configurar um MSI atribuído ao utilizador para VM do Azure com um modelo do Azure
description: Passo a passo de instruções para configurar um utilizador atribuído Managed Service Identity (MSI) para a VM do Azure, com um modelo Azure Resource Manager.
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
ms.date: 12/22/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: e01e4c397e0d0a19280a32fc1e8341b57b47e4eb
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38610388"
---
# <a name="configure-a-user-assigned-managed-service-identity-msi-for-a-vm-using-an-azure-template"></a>Configurar um utilizador atribuído Managed Service Identity (MSI) para uma VM, com um modelo do Azure

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida no Azure Active Directory. Pode utilizar esta identidade para autenticar para serviços que suportam a autenticação do Azure AD, sem a necessidade de credenciais no seu código. 

Neste artigo, saiba como ativar e remover um MSI atribuído ao utilizador para VM do Azure, com um modelo de implementação Azure Resource Manager.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Ativar o MSI durante a criação de uma VM do Azure ou numa VM existente

Como com o Azure portal e criação de scripts, os modelos Azure Resource Manager fornecem a capacidade de implementar recursos novos ou modificados definidos por um grupo de recursos do Azure. Várias opções estão disponíveis para edição de modelo e a implantação, local e baseado no portal, incluindo:

   - Utilizar um [modelo personalizado do Azure Marketplace](~/articles/azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), que permite-lhe criar um modelo a partir do zero ou baseá-la num comum existente ou [modelo de início rápido](https://azure.microsoft.com/documentation/templates/).
   - Derivar de um grupo de recursos existente ao exportar um modelo a partir de qualquer uma [implementação original](~/articles/azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), ou a partir do [estado atual da implantação](~/articles/azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Com um local [editor de JSON (por exemplo, o VS Code)](~/articles/azure-resource-manager/resource-manager-create-first-template.md)e, em seguida, carregar e implementar com o PowerShell ou a CLI.
   - Com o Visual Studio [projeto do grupo de recursos do Azure](~/articles/azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) para criar e implementar um modelo.  

Independentemente da opção escolhida, sintaxe do modelo é o mesmo durante a implementação inicial e a reimplementação. Criar e atribuir um MSI atribuído ao utilizador a uma VM nova ou existente é feito da mesma forma. Além disso, por padrão, o Azure Resource Manager faz uma [atualização incremental](~/articles/azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) para implementações:

1. Se iniciar sessão localmente no Azure ou através do portal do Azure, utilize uma conta que está associada à subscrição do Azure que contém o MSI e a VM. Certifique-se também que a sua conta pertencer a uma função que dá-lhe permissões de escrita nos recursos (por exemplo, a função de "proprietário") ou de subscrição.

2. Depois de carregar o modelo num editor, localize a `Microsoft.Compute/virtualMachines` recursos de interesse no `resources` secção. Sua poderá parecer ligeiramente diferente da captura de ecrã seguinte, consoante o editor que estiver a utilizar e se estiver a editar um modelo para uma implementação novo ou existente.

   >[!NOTE] 
   > Este exemplo assume variáveis como `vmName`, `storageAccountName`, e `nicName` foram definidos no modelo.
   >

   ![Captura de ecrã do modelo - localize a VM](~/articles/active-directory/media/msi-qs-configure-template-windows-vm/template-file-before.png) 

3. Adicionar a `"identity"` propriedade o mesmo nível, como o `"type": "Microsoft.Compute/virtualMachines"` propriedade. Utilize a seguinte sintaxe:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. Em seguida, adicione a extensão MSI de VM como um `resources` elemento. Utilize a seguinte sintaxe:

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

   ![Captura de ecrã do modelo após atualização](~/articles/active-directory/media/msi-qs-configure-template-windows-vm/template-file-after.png) 

## <a name="remove-msi-from-an-azure-vm"></a>Remover MSI a partir de uma VM do Azure

Se tiver uma VM que não precisa mais de um MSI:

1. Se iniciar sessão localmente no Azure ou através do portal do Azure, utilize uma conta que está associada à subscrição do Azure que contém a VM. Certifique-se também que a sua conta pertencer a uma função que dá-lhe permissões de escrita na VM (por exemplo, a função de "Contribuinte de Máquina Virtual").

2. Remover os dois elementos que foram adicionados na seção anterior: a VM `"identity"` propriedade e o `"Microsoft.Compute/virtualMachines/extensions"` recursos.

## <a name="related-content"></a>Conteúdo relacionado

- Para obter uma perspectiva mais ampla sobre o MSI, leia os [descrição geral de identidade do serviço gerido](msi-overview.md).


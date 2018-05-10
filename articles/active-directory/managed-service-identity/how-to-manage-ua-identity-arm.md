---
title: Como criar e eliminar um utilizador atribuído a identidade de serviço geridas com o Azure Resource Manager
description: Passo a passo obter instruções sobre como criar e eliminar utilizador atribuído a identidade de serviço geridas através de recursos do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: e5c5ff74ee94f8df03ceb5b469ad635bd80d5a11
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="create-list-and-delete-a-user-assigned-identity-using-azure-resource-manager"></a>Criar, listar e eliminar uma identidade de utilizador atribuída com o Azure Resource Manager

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido no Azure Active Directory. Pode utilizar esta identidade para se autenticarem em serviços que suportam a autenticação do Azure AD, sem necessitar de credenciais no seu código. 

Neste artigo, vai criar um utilizador atribuído identidade gerida utilizando o Gestor de recursos do Azure.

Não é possível listar e eliminar um utilizador atribuído a identidade com um modelo Azure Resource Manager.  Consulte os artigos seguintes para criar e listar uma identidade de utilizador atribuída:

- [Atribuído a identidade de utilizador de lista](how-to-manage-ua-identity-cli.md#list-user-assigned-identities)
- [Eliminar a identidade do utilizador atribuído](how-to-manage-ua-identity-cli.md#delete-a-user-assigned-identity)
## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade de serviço geridas, consulte o [secção Descrição geral](overview.md). **Certifique-se rever o [diferença entre um sistema atribuída e o utilizador atribuído identidade](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscrever-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.

Se iniciar sessão localmente no Azure ou através do portal do Azure, utilize uma conta que está associada à subscrição do Azure que contém a VM. Certifique-se também que a sua conta pertencer a uma função que dá-lhe permissões de escrita na VM (por exemplo, a função de "Contribuinte de Máquina Virtual").

## <a name="template-creation-and-editing"></a>A criação de modelos e editar

Como com o Azure portal e scripts, modelos Azure Resource Manager fornecem a capacidade de implementar recursos novos ou modificados definidos por um grupo de recursos do Azure. Várias opções estão disponíveis para edição do modelo e implementação, local e baseado no portal, incluindo:

- Utilizar um [modelo personalizado no Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), que lhe permite criar um modelo a partir do zero ou baseá-num comuns existente ou [modelo de início rápido](https://azure.microsoft.com/documentation/templates/).
- Efetuar a derivação de um grupo de recursos existente ao exportar um modelo a partir [a implementação original](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), ou a partir de [estado atual da implementação](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
- Utilizar uma local [editor de JSON (por exemplo, o VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md)e, em seguida, carregar e implementar utilizando o PowerShell ou a CLI.
- Com o Visual Studio [projeto do grupo de recursos do Azure](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) para criar e implementar um modelo. 

## <a name="create-a-user-assigned-identity"></a>Criar um utilizador atribuído identidade 

Para criar um utilizador atribuído a identidade, utilize o modelo seguinte. Substitua o `<USER ASSIGNED IDENTITY NAME>` valor com os seus próprios valores:

> [!IMPORTANT]
> Criar identidades de utilizador atribuída só suporta alfanuméricos e hífenes (0-9 ou a-z ou A-Z ou -) carateres. Além disso, o nome deve ser limitado a 24 carateres para a atribuição a VM/VMSS funcione corretamente. Verifique novamente para as atualizações. Para obter mais informações, consulte [perguntas mais frequentes e problemas conhecidos](known-issues.md)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "resourceName": {
          "type": "string",
          "metadata": {
            "description": "<USER ASSIGNED IDENTITY NAME>"
          }
        }
  },
  "resources": [
    {
      "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
      "name": "[parameters('<USER ASSIGNED IDENTITY NAME>')]",
      "apiVersion": "2015-08-31-PREVIEW",
      "location": "[resourceGroup().location]"
    }
  ],
  "outputs": {
      "identityName": {
          "type": "string",
          "value": "[parameters('<USER ASSIGNED IDENTITY NAME>')]"
      }
  }
}
```
## <a name="related-content"></a>Conteúdo relacionado

Para obter informações sobre como atribuir uma identidade de utilizador atribuída a uma VM do Azure através de um Gestor de recursos do Azure modelo, consulte [configurar uma identidade de serviço geridas da VM utilizando um modelo](qs-configure-template-windows-vm.md).


 

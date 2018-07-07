---
title: Como criar e eliminar um utilizador atribuído a identidade do serviço gerido com o Azure Resource Manager
description: Instruções passo a passo obter instruções sobre como criar e eliminar o utilizador atribuído a identidade do serviço gerido com o recurso do Azure.
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
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 74e4c28130fb49a3cb81420913f71dfa3a7c00bf
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37903977"
---
# <a name="create-list-and-delete-a-user-assigned-identity-using-azure-resource-manager"></a>Criar, listar e eliminar uma identidade atribuída ao utilizador com o Azure Resource Manager

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida no Azure Active Directory. Pode utilizar esta identidade para autenticar para serviços que suportam a autenticação do Azure AD, sem a necessidade de credenciais no seu código. 

Neste artigo, vai criar um identidade gerida com um Azure Resource Manager atribuída ao utilizador.

Não é possível listar e eliminar um utilizador atribuído a identidade com um modelo Azure Resource Manager.  Veja os artigos seguintes para criar e listar uma identidade de utilizador atribuída:

- [Identidade atribuída ao utilizador lista](how-to-manage-ua-identity-cli.md#list-user-assigned-identities)
- [Eliminar a identidade atribuída ao utilizador](how-to-manage-ua-identity-cli.md#delete-a-user-assigned-identity)
## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre um sistema atribuído e a identidade atribuída ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [Inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.

Se iniciar sessão localmente no Azure ou através do portal do Azure, utilize uma conta que está associada à subscrição do Azure que contém a VM. Certifique-se também que a sua conta pertencer a uma função que dá-lhe permissões de escrita na VM (por exemplo, a função de "Contribuinte de Máquina Virtual").

## <a name="template-creation-and-editing"></a>Criação de modelos e a edição

Como com o Azure portal e criação de scripts, os modelos Azure Resource Manager fornecem a capacidade de implementar recursos novos ou modificados definidos por um grupo de recursos do Azure. Várias opções estão disponíveis para edição de modelo e a implantação, local e baseado no portal, incluindo:

- Utilizar um [modelo personalizado do Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), que permite-lhe criar um modelo a partir do zero ou baseá-la num comum existente ou [modelo de início rápido](https://azure.microsoft.com/documentation/templates/).
- Derivar de um grupo de recursos existente ao exportar um modelo a partir de qualquer uma [implementação original](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), ou a partir do [estado atual da implantação](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
- Com um local [editor de JSON (por exemplo, o VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md)e, em seguida, carregar e implementar com o PowerShell ou a CLI.
- Com o Visual Studio [projeto do grupo de recursos do Azure](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) para criar e implementar um modelo. 

## <a name="create-a-user-assigned-identity"></a>Criar um identidade atribuída ao utilizador 

Para criar um identidade atribuída ao utilizador, utilize o modelo seguinte. Substitua o `<USER ASSIGNED IDENTITY NAME>` valor pelos seus próprios valores:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

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

Para obter informações sobre como atribuir uma identidade de utilizador atribuída a VM do Azure com uma veja de modelo do Azure Resource Manager [configurar uma identidade de serviço gerido da VM com um modelo](qs-configure-template-windows-vm.md).


 

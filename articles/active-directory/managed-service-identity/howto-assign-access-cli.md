---
title: Como atribuir acesso MSI para um recurso do Azure, utilizando a CLI do Azure
description: Passo a passo instruções para atribuir um MSI um recurso, aceder a outro recurso, utilizando a CLI do Azure.
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
ms.date: 09/25/2017
ms.author: daveba
ms.openlocfilehash: 947e0140c7943954be5eb285bb7ec514b74e9022
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-azure-cli"></a>Atribuir acesso uma identidade de serviço geridas (MSI) a um recurso com a CLI do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Depois de configurar um recurso do Azure com um MSI, é possível conceder o acesso do MSI para outro recurso, tal como qualquer principal de segurança. Este exemplo mostra como enviar uma máquina virtual do Azure ou MSI para requerer acesso do conjunto de dimensionamento da máquina virtual a uma conta de armazenamento do Azure, utilizando a CLI do Azure.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Para executar os exemplos de script do CLI, tem três opções:

- Utilize [Shell de nuvem do Azure](../../cloud-shell/overview.md) do portal do Azure (consulte a secção seguinte).
- Utilize a Shell de nuvem do Azure incorporados através de "Tente-" botão, localizado no canto superior direito de cada bloco de código.
- [Instale a versão mais recente do CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 ou posterior) se preferir utilizar uma consola local do CLI. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Utilizar o RBAC para atribuir o acesso MSI a outro recurso

Depois de ativar a MSI um recurso do Azure, tal como um [máquina virtual do Azure](qs-configure-cli-windows-vm.md) ou [conjunto de dimensionamento da máquina virtual do Azure](qs-configure-cli-windows-vmss.md): 

1. Se estiver a utilizar a CLI do Azure numa consola local, primeiro inicie sessão no Azure com [início de sessão az](/cli/azure/reference-index#az_login). Utilize uma conta que está associada à subscrição do Azure na qual gostaria de implementar o conjunto de dimensionamento da máquina virtual ou de VM:

   ```azurecli-interactive
   az login
   ```

2. Neste exemplo, vamos dá ao acesso uma máquina virtual do Azure para uma conta de armazenamento. Primeiro vamos utilizar [lista de recursos de az](/cli/azure/resource/#az_resource_list) para obter o principal de serviço para a máquina virtual com o nome "myVM":

   ```azurecli-interactive
   spID=$(az resource list -n myVM --query [*].identity.principalId --out tsv)
   ```
   Para um conjunto de dimensionamento da máquina virtual do Azure, o comando é o mesmo, exceto aqui, pode obter o principal de serviço para o conjunto de dimensionamento de máquina virtual com o nome "DevTestVMSS":
   
   ```azurecli-interactive
   spID=$(az resource list -n DevTestVMSS --query [*].identity.principalId --out tsv)
   ```

3. Assim que tiver o ID de principal de serviço, utilize [de criação da atribuição de função az](/cli/azure/role/assignment#az_role_assignment_create) para dar a máquina virtual ou dimensionamento da máquina virtual definir acesso de "Leitor" para uma conta de armazenamento denominada "myStorageAcct":

   ```azurecli-interactive
   az role assignment create --assignee $spID --role 'Reader' --scope /subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

## <a name="troubleshooting"></a>Resolução de problemas

Se o MSI para o recurso não aparecer na lista de identidades disponíveis, certifique-se de que o MSI foi ativado corretamente. No nosso caso, iremos pode voltar para a máquina virtual do Azure ou conjunto de dimensionamento da máquina virtual a [portal do Azure](https://portal.azure.com) e:

- Observe a página "Configuration" e certifique-se MSI ativado = "Yes".
- Observe a página "Extensões" e certifique-se a extensão MSI implementada com êxito (**extensões** página não está disponível para um conjunto de dimensionamento da máquina virtual do Azure).

Se o estiver incorreto, terá de voltar a implementar o MSI no seu recurso ou resolver a falha de implementação.

## <a name="related-content"></a>Conteúdo relacionado

- Para obter uma descrição geral do MSI, consulte [descrição geral de identidade de serviço geridas](overview.md).
- Para ativar o MSI numa máquina virtual do Azure, consulte [configurar um Azure VM geridos serviço de identidade (MSI) utilizando a CLI do Azure](qs-configure-cli-windows-vm.md).
- Para ativar o MSI um conjunto de dimensionamento da máquina virtual do Azure, consulte o artigo [configurar um Azure Máquina Virtual escala definir geridos serviço de identidade (MSI) no portal do Azure](qs-configure-portal-windows-vmss.md)

Utilize a seguinte secção de comentários para fornecer comentários e ajudam-nos refinar e formam o nosso conteúdo.


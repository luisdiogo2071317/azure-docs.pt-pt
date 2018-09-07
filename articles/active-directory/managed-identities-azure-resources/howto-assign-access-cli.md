---
title: Como atribuir acesso MSI a um recurso do Azure, cli do Azure
description: Instruções passo a passo instruções para atribuir um MSI num recurso, o acesso a outro recurso, com a CLI do Azure.
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
ms.openlocfilehash: a5da06eac7f4680282aad305f57cb9ca1c9d5730
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028603"
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-azure-cli"></a>Atribuir um acesso de identidade de serviço gerida (MSI) a um recurso com a CLI do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Assim que tiver configurado um recurso do Azure com um MSI, pode dar o acesso MSI para outro recurso, tal como qualquer entidade de segurança. Este exemplo mostra como conceder a uma máquina virtual do Azure ou o acesso MSI do conjunto de dimensionamento de máquina virtual para uma conta de armazenamento do Azure, cli do Azure.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Para executar os exemplos de script da CLI, tem três opções:

- Uso [Azure Cloud Shell](../../cloud-shell/overview.md) do portal do Azure (consulte a secção seguinte).
- Utilize o embedded Azure Cloud Shell através do "Experimente-lo" botão do, localizado no canto superior direito de cada bloco de código.
- [Instalar a versão mais recente da CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 ou posterior) se preferir utilizar uma consola CLI local. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Utilizar o RBAC para atribuir o acesso MSI para outro recurso

Depois de ativar a MSI num recurso do Azure, tal como um [máquina virtual do Azure](qs-configure-cli-windows-vm.md) ou [conjunto de dimensionamento de máquina virtual do Azure](qs-configure-cli-windows-vmss.md): 

1. Se estiver a utilizar a CLI do Azure numa consola local, primeiro inicie sessão no Azure com [az login](/cli/azure/reference-index#az-login). Utilize uma conta que seja associada à subscrição do Azure sob a qual pretende implementar o conjunto de dimensionamento VM ou numa máquina virtual:

   ```azurecli-interactive
   az login
   ```

2. Neste exemplo, estamos oferecendo uma máquina virtual do Azure de acesso a uma conta de armazenamento. Primeiro, usamos [lista de recursos de az](/cli/azure/resource/#az-resource-list) para obter o principal de serviço para a máquina virtual com o nome "myVM":

   ```azurecli-interactive
   spID=$(az resource list -n myVM --query [*].identity.principalId --out tsv)
   ```
   Para obter um conjunto de dimensionamento de máquina virtual do Azure, o comando é o mesmo, exceto aqui, obtém o principal de serviço para o conjunto de dimensionamento de máquinas virtuais com o nome "DevTestVMSS":
   
   ```azurecli-interactive
   spID=$(az resource list -n DevTestVMSS --query [*].identity.principalId --out tsv)
   ```

3. Assim que tiver o ID de principal de serviço, utilize [criação da atribuição de função de az](/cli/azure/role/assignment#az-role-assignment-create) dar a máquina virtual ou o dimensionamento de máquinas virtuais, definir acesso de "Leitor de" para uma conta de armazenamento denominada "myStorageAcct":

   ```azurecli-interactive
   az role assignment create --assignee $spID --role 'Reader' --scope /subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

## <a name="troubleshooting"></a>Resolução de problemas

Se o MSI para o recurso não aparecer na lista de identidades disponíveis, certifique-se de que o MSI foi ativado corretamente. No nosso caso, podemos voltar para a máquina virtual do Azure ou o conjunto de dimensionamento de máquina virtual a [portal do Azure](https://portal.azure.com) e:

- Veja a página "Configuração" e certifique-se de MSI ativada = "Yes".
- Veja a página de "Extensões" e certifique-se a extensão MSI implementada com êxito (**extensões** página não está disponível para um conjunto de dimensionamento de máquina virtual do Azure).

Se uma for incorreta, terá de voltar a implementar o MSI no seu recurso, ou a falha de implementação de resolução de problemas.

## <a name="related-content"></a>Conteúdo relacionado

- Para uma descrição geral do MSI, consulte [descrição geral de identidade do serviço gerido](overview.md).
- Para ativar o MSI numa máquina virtual do Azure, consulte [configurar uma Azure VM Managed Service Identity (MSI) com a CLI do Azure](qs-configure-cli-windows-vm.md).
- Para ativar o MSI num conjunto de dimensionamento de máquina virtual do Azure, consulte [configurar um Azure Máquina Virtual dimensionamento definir Managed Service Identity (MSI) com o portal do Azure](qs-configure-portal-windows-vmss.md)

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo.


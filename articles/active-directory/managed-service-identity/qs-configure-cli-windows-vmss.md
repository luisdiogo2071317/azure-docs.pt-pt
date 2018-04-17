---
title: Configurar MSI num conjunto utilizando a CLI do Azure de dimensionamento de máquina virtual do Azure
description: Passo pelo passo as instruções para configurar uma identidade de serviço geridas (MSI) numa Azure Virtual Machine conjunto de dimensionamento, com a CLI do Azure.
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
ms.date: 02/15/2018
ms.author: daveba
ms.openlocfilehash: 9cdf5225f2d87fffa2290e3edd09d4ae829aee21
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-azure-cli"></a>Configurar uma máquina virtual gerida serviço de identidade (MSI) utilizando a CLI do Azure do conjunto de dimensionamento

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter de credenciais no seu código. 

Neste artigo, irá aprender como ativar e remover MSI para um conjunto utilizando a CLI do Azure de dimensionamento de máquina virtual do Azure.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Para executar os exemplos de script do CLI, tem três opções:

- Utilize [Shell de nuvem do Azure](../../cloud-shell/overview.md) do portal do Azure (consulte a secção seguinte).
- Utilize a Shell de nuvem do Azure incorporados através de "Tente-" botão, localizado no canto superior direito de cada bloco de código.
- [Instale a versão mais recente do CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 ou posterior) se preferir utilizar uma consola local do CLI. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="enable-msi-during-creation-of-an-azure-virtual-machine-scale-set"></a>Ativar MSI durante a criação de um conjunto de dimensionamento da máquina virtual do Azure

Para criar um dimensionamento da máquina virtual ativado por MSI definir:

1. Se estiver a utilizar a CLI do Azure numa consola local, primeiro inicie sessão no Azure com [início de sessão az](/cli/azure/reference-index#az_login). Utilize uma conta que está associada à subscrição do Azure na qual gostaria de implementar o conjunto de dimensionamento de máquina virtual:

   ```azurecli-interactive
   az login
   ```

2. Criar um [grupo de recursos](../../azure-resource-manager/resource-group-overview.md#terminology) para a implementação do seu conjunto de dimensionamento da máquina virtual e os respetivos recursos relacionados, utilizar e contenção [criar grupo az](/cli/azure/group/#az_group_create). Pode ignorar este passo se já tiver o grupo de recursos que pretende utilizar em vez disso:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Criar um conjunto utilizando de dimensionamento de máquina virtual [az vmss criar](/cli/azure/vmss/#az_vmss_create) . O exemplo seguinte cria um conjunto nomeado de dimensionamento de máquina virtual *myVMSS* com um MSI, conforme solicitado pelo `--assign-identity` parâmetro. O `--admin-username` e `--admin-password` parâmetros especificam a conta de nome e palavra-passe de utilizador administrativo para início de sessão na máquina virtual. Atualize estes valores conforme adequado para o seu ambiente: 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

## <a name="enable-msi-on-an-existing-azure-virtual-machine-scale-set"></a>Ativar o MSI num conjunto de dimensionamento de máquina virtual do Azure existente

Se precisar de ativar o MSI num conjunto de dimensionamento de máquina virtual do Azure existente:

1. Se estiver a utilizar a CLI do Azure numa consola local, primeiro inicie sessão no Azure com [início de sessão az](/cli/azure/reference-index#az_login). Utilize uma conta que está associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual.

   ```azurecli-interactive
   az login
   ```

2. Utilize [identidade de vmss az atribuir](/cli/azure/vmss/identity/#az_vmss_identity_assign) comando para adicionar um MSI para uma VM existente:

   ```azurecli-interactive
   az vmss identity assign -g myResourceGroup -n myVMSS
   ```

## <a name="remove-msi-from-an-azure-virtual-machine-scale-set"></a>Remover MSI a partir de um conjunto de dimensionamento da máquina virtual do Azure

Se tiver um conjunto de dimensionamento de máquina virtual que já não necessita de um MSI:

1. Se estiver a utilizar a CLI do Azure numa consola local, primeiro inicie sessão no Azure com [início de sessão az](/cli/azure/reference-index#az_login). Utilize uma conta que está associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual.

   ```azurecli-interactive
   az login
   ```

2. Utilize [remover a identidade de vmss az](/cli/azure/vmss/identity/#az_vmss_remove_identity) comando para remover o MSI:

   ```azurecli-interactive
   az vmss identity remove -g myResourceGroup -n myVMSS --identities readerID writerID
   ```

## <a name="next-steps"></a>Passos Seguintes

- [Descrição geral de identidade de serviço gerida](overview.md)
- Para o Azure completo início rápido de criação de conjunto de dimensionamento de máquina virtual, consulte: 

  - [Criar um conjunto de dimensionamento de Máquina Virtual com a CLI](../../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)

Utilize a seguinte secção de comentários para fornecer comentários e ajudam-nos refinar e formam o nosso conteúdo.

















---
title: Configurar a chave de cofre para Windows de VMs no Azure Resource Manager | Documentos da Microsoft
description: Como configurar o Cofre de chaves para utilização com uma máquina virtual do Azure Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 33a483e2-cfbc-4c62-a588-5d9fd52491e2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: kasing
ms.openlocfilehash: 599b16f633d9a0de5165bdf5cb3d7b82abca655b
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39597715"
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Configurar o Cofre de chaves para máquinas virtuais no Azure Resource Manager

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

Na pilha do Azure Resource Manager, os segredos/certificados são modelados como recursos que são fornecidos pelo fornecedor de recursos do Cofre de chaves. Para saber mais sobre o Key Vault, consulte [o que é o Azure Key Vault?](../../key-vault/key-vault-whatis.md)

> [!NOTE]
> 1. Para que o Cofre de chaves para serem utilizadas com as máquinas virtuais do Azure Resource Manager, o **EnabledForDeployment** propriedade no Cofre de chaves tem de ser definida como true. Pode fazê-lo em vários clientes.
> 2. O Cofre de chaves tem de ser criada na mesma subscrição e localização que a Máquina Virtual.
>
>

## <a name="use-powershell-to-set-up-key-vault"></a>Utilize o PowerShell para configurar o Cofre de chaves
Para criar um cofre de chaves com o PowerShell, veja [introdução ao Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).

Para obter cofres de chaves novo, pode utilizar este cmdlet do PowerShell:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Para obter cofres de chaves existentes, pode utilizar este cmdlet do PowerShell:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="use-cli-to-set-up-key-vault"></a>Utilizar a CLI para configurar o Cofre de chaves
Para criar um cofre de chaves utilizando a interface de linha de comandos (CLI), veja [gerir Key Vault com a CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).

Para a CLI, terá de criar o Cofre de chaves, antes de atribuir a política de implementação. Pode fazê-lo utilizando o comando seguinte:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Utilize modelos para configurar o Cofre de chaves
Apesar de utilizar um modelo, tem de definir o `enabledForDeployment` propriedade `true` para o recurso do Key Vault.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Para outras opções que pode configurar quando cria um cofre de chaves com os modelos, veja [criar um cofre de chaves](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).

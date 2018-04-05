---
title: Criar um instantâneo de um VHD no Azure | Microsoft Docs
description: Saiba como criar uma cópia de um VHD no Azure como uma cópia de segurança ou de resolução de problemas.
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 03/20/2018
ms.author: cynthn
ms.openlocfilehash: e5882b2ddc708544a7715da13c1f0d18384ce4e3
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="create-a-snapshot"></a>Criar um instantâneo 

Tire um instantâneo de um disco de SO ou dados na cópia de segurança ou para resolver problemas VM. Um instantâneo é uma cópia completa só de leitura de um VHD. 

## <a name="use-azure-cli"></a>Utilizar a CLI do Azure 

O exemplo seguinte requer o 2.0 do CLI do Azure instalado e registado na sua conta do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Os passos seguintes mostram como tirar um instantâneo, utilizando o `az snapshot create` comando com o `--source-disk` parâmetro. O exemplo seguinte parte do princípio de que não há uma VM chamada `myVM` no `myResourceGroup` grupo de recursos.

Obter o ID de disco.
```azure-cli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

Tirar um instantâneo com o nome *cópia de segurança osDisk*.

```azurecli-interactive
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDisk-backup
```

> [!NOTE]
> Se pretende armazenar o instantâneo no armazenamento resiliente para a zona, terá de criá-la numa região que suporte [zonas de disponibilidade](../../availability-zones/az-overview.md) e incluir o `--sku Standard_ZRS` parâmetro.

## <a name="use-azure-portal"></a>Utilizar o portal do Azure 

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. A partir do canto superior esquerdo, clique em **crie um recurso** e procure **instantâneo**.
3. No painel do instantâneo, clique em **criar**.
4. Introduza um **nome** para o instantâneo.
5. Selecione um [Grupo de recursos](../../azure-resource-manager/resource-group-overview.md#resource-groups) ou escreva o nome para um novo. 
6. Selecione um localização do datacenter do Azure.  
7. Para **disco de origem**, selecione o disco de gerido para instantâneo.
8. Selecione o **tipo de conta** utilizar para armazenar o instantâneo. Recomendamos **Standard_LRS** , a menos que o ficheiro necessário armazenados num disco elevado desempenho.
9. Clique em **Criar**.


## <a name="next-steps"></a>Passos seguintes

 Crie uma máquina virtual a partir de um instantâneo ao criar um disco gerido do instantâneo e, em seguida, anexar o disco novo gerido como disco do SO. Para obter mais informações, consulte o [criar uma VM a partir de um instantâneo](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) script.


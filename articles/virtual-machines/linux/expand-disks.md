---
title: Expanda os discos rígidos virtuais numa VM do Linux no Azure | Documentos da Microsoft
description: Saiba como expandir os discos rígidos virtuais numa VM do Linux com a CLI do Azure
services: virtual-machines-linux
documentationcenter: ''
author: roygara
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/13/2017
ms.author: rogarana
ms.openlocfilehash: 0c2d4d1413b6cfd0b5e457e720b59c6c7b575092
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46974549"
---
# <a name="how-to-expand-virtual-hard-disks-on-a-linux-vm-with-the-azure-cli"></a>Como expandir os discos rígidos virtuais numa VM do Linux com a CLI do Azure

O tamanho de disco rígido virtual padrão para o sistema operativo (SO) é, normalmente, 30 GB numa máquina virtual (VM) do Linux no Azure. Pode [adicionar discos de dados](add-disk.md) fornecer para o espaço de armazenamento adicional, mas também pode pretender expandir um disco de dados existente. Este artigo fornece detalhes sobre como expandir os discos geridos para uma VM do Linux com a CLI do Azure. 

> [!WARNING]
> Sempre Certifique-se de que volta dos seus dados antes de executar o disco redimensiona operações. Para obter mais informações, consulte [cópia de segurança de VMs do Linux no Azure](tutorial-backup-vms.md).

## <a name="expand-azure-managed-disk"></a>Expanda o disco gerido do Azure
Certifique-se de que tem a versão mais recente [CLI do Azure](/cli/azure/install-az-cli2) instalado e registado à utilização conta do Azure [início de sessão az](/cli/azure/reference-index#az_login).

Este artigo requer uma VM existente no Azure com pelo menos um disco de dados anexados e preparado. Se ainda não tiver uma VM que pode utilizar, veja [criar e preparar uma VM com discos de dados](tutorial-manage-disks.md#create-and-attach-disks).

Nos exemplos a seguir, substitua os nomes de parâmetros de exemplo pelos seus próprios valores. Os nomes de parâmetros de exemplo incluem *myResourceGroup* e *myVM*.

1. Não não possível efetuar operações em discos rígidos virtuais com a VM em execução. Desaloque a VM com [az vm deallocate](/cli/azure/vm#az_vm_deallocate). O exemplo seguinte desaloca a VM com o nome *myVM* no grupo de recursos com o nome *myResourceGroup*:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > A VM tem de ser desalocada para expandir o disco rígido virtual. `az vm stop` Não é libertar os recursos de computação. Para liberar recursos de computação, utilize `az vm deallocate`.

1. Ver uma lista de discos geridos num grupo de recursos com [lista de disco de az](/cli/azure/disk#az_disk_list). O exemplo seguinte mostra uma lista de discos geridos no grupo de recursos com o nome *myResourceGroup*:

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    Expanda o disco necessário com [atualização do disco az](/cli/azure/disk#az_disk_update). O exemplo seguinte expande o disco gerido com o nome *myDataDisk* ser *200*Gb de tamanho:

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > Ao expandir um disco gerido, o tamanho atualizado é mapeado para o tamanho de disco gerido mais próximo. Para uma tabela dos escalões e tamanhos de disco gerido disponíveis, consulte [Managed Disks descrição geral do Azure - preços e faturação](../windows/managed-disks-overview.md#pricing-and-billing).

1. Inicie a VM com [início de vm de az](/cli/azure/vm#az_vm_start). O exemplo seguinte inicia a VM com o nome *myVM* no grupo de recursos com o nome *myResourceGroup*:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```


## <a name="expand-disk-partition-and-filesystem"></a>Expandir a partição de disco e o sistema de ficheiros
Para utilizar o disco expandido, terá de expandir a partição e o sistema de ficheiros subjacente.

1. SSH para a VM com as credenciais apropriadas. Pode obter o endereço IP público da VM com [show de vm de az](/cli/azure/vm#az_vm_show):

    ```azurecli
    az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
    ```

1. Para utilizar o disco expandido, terá de expandir a partição e o sistema de ficheiros subjacente.

    a. Se já montar, desmonte o disco:

    ```bash
    sudo umount /dev/sdc1
    ```

    b. Utilize `parted` para ver informações de disco e redimensione a partição:

    ```bash
    sudo parted /dev/sdc
    ```

    Ver informações sobre o esquema de partição existente com `print`. O resultado é semelhante ao seguinte exemplo, o qual mostra que o disco subjacente está 215Gb de tamanho:

    ```bash
    GNU Parted 3.2
    Using /dev/sdc1
    Welcome to GNU Parted! Type 'help' to view a list of commands.
    (parted) print
    Model: Unknown Msft Virtual Disk (scsi)
    Disk /dev/sdc1: 215GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: loop
    Disk Flags:
    
    Number  Start  End    Size   File system  Flags
        1      0.00B  107GB  107GB  ext4
    ```

    c. Expanda a partição com `resizepart`. Introduza o número de partição *1*e um tamanho para a nova partição:

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [107GB]? 215GB
    ```

    d. Para sair, introduza: `quit`

1. Com a partição redimensionada, certifique-se a consistência da partição com `e2fsck`:

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

1. Agora, redimensione o sistema de ficheiros com `resize2fs`:

    ```bash
    sudo resize2fs /dev/sdc1
    ```

1. Monte a partição para a localização pretendida, tais como `/datadrive`:

    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```

1. Para verificar o disco do SO foi redimensionado, utilize `df -h`. O resultado de exemplo seguinte mostra a unidade de dados */desenvolvimento/sdc1*, agora é 200 GB:

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        197G   60M   187G   1% /datadrive
    ```

## <a name="next-steps"></a>Passos Seguintes
Se precisar de armazenamento adicional, também [adicionar discos de dados a uma VM do Linux](add-disk.md). Para obter mais informações sobre a encriptação de disco, consulte [encriptar discos numa VM do Linux com a CLI do Azure](encrypt-disks.md).

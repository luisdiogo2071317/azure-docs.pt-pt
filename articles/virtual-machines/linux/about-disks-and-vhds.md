---
title: Sobre não geridos (blobs de páginas) e geridas o armazenamento de discos para VMs de Linux do Microsoft Azure | Documentos da Microsoft
description: Saiba mais sobre as noções básicas do geridos (blobs de páginas) e não geridos armazenamento de discos para máquinas virtuais do Linux no Azure.
services: virtual-machines-linux,storage
author: roygara
ms.service: virtual-machines-linux
ms.tgt_pltfrm: linux
ms.topic: article
ms.date: 11/15/2017
ms.author: rogarana
ms.component: disks
ms.openlocfilehash: 53b44d47d9421f1b37aef7ca4d30c2c753b79740
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51218326"
---
# <a name="about-disks-storage-for-azure-linux-vms"></a>Sobre o armazenamento de discos para VMs Linux do Azure
Assim como qualquer outro computador, o máquinas virtuais do Azure utilizam discos como um local para armazenar um sistema operacional, aplicativos e dados. Todas as máquinas virtuais do Azure têm, pelo menos, dois discos – um disco de sistema operativo Linux e um disco temporário. O disco do sistema operativo é criado a partir de uma imagem e o disco do sistema operativo e a imagem são discos rígidos virtuais (VHDs) armazenados numa conta de armazenamento do Azure. Máquinas virtuais também podem ter um ou mais discos de dados, que também são armazenados como VHDs.

Neste artigo, iremos falar sobre as utilizações diferentes para os discos e, em seguida, discutir os diferentes tipos de discos, pode criar e utilizar. Este artigo também está disponível para [as máquinas virtuais do Windows](../windows/about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Discos utilizados por VMs

Vamos dar uma olhada em como os discos são utilizados pelas VMs.

## <a name="operating-system-disk"></a>Disco do sistema operativo

Cada máquina virtual possui um disco de sistema de operativo anexado. Ele está registado como uma unidade SATA e tem o nome /dev/sda por predefinição. Este disco tem a capacidade máxima de 2048 gigabytes (GB).

## <a name="temporary-disk"></a>Disco temporário

Cada VM contém um disco temporário. O disco temporário fornece armazenamento de curto prazo para aplicações e processos e destina-se para armazenar apenas os dados, tais como ficheiros de paginação ou de troca. Dados do disco temporário podem ser perdidos durante uma [evento de manutenção](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) ou quando [Reimplementar uma VM](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Durante a VM seja reiniciada padrão, devem manter os dados na unidade temporária. No entanto, há casos em que os dados podem não persistir, por exemplo, transferindo para um novo anfitrião. Da mesma forma, todos os dados na unidade temporária não devem ser dados que é essenciais para o sistema.

Em máquinas de virtuais do Linux, o disco é normalmente **/desenvolvimento/sdb** é formatado e montada **/mnt** pelo agente do Linux do Azure. O tamanho do disco temporário varia, com base no tamanho da máquina virtual. Para obter mais informações, consulte [tamanhos de máquinas de virtuais de Linux](../windows/sizes.md).

## <a name="data-disk"></a>Disco de dados

Um disco de dados é um VHD que está ligado a uma máquina virtual para armazenar dados de aplicação ou outros dados que precisa para manter. Discos de dados estão registados como unidades SCSI e são rotulados com uma letra que escolher. Cada disco de dados tem a capacidade máxima de 4095 GB. O tamanho da máquina virtual determina quantos discos de dados, pode anexar a ele e o tipo de armazenamento pode utilizar para alojar os discos.

> [!NOTE]
> Para obter mais informações sobre as capacidades de máquinas virtuais, consulte [tamanhos de máquinas de virtuais de Linux](./sizes.md).

Quando cria uma máquina virtual a partir de uma imagem, o Azure cria um disco do sistema operativo. Se utilizar uma imagem que inclua discos de dados, o Azure também cria os discos de dados ao criar a máquina virtual. Caso contrário, adicione discos de dados depois de criar a máquina virtual.

Pode adicionar discos de dados a uma máquina virtual em qualquer altura, ao **anexar** o disco à máquina virtual. Pode utilizar um VHD que carregou ou copiados para a sua conta de armazenamento ou que o Azure cria para. Anexar um disco de dados, associa o ficheiro VHD com a VM, colocando uma concessão no VHD para que não pode ser eliminada do armazenamento, enquanto ainda estiver a ser ligado.

[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="troubleshooting"></a>Resolução de problemas
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Passos Seguintes
* [Anexar um disco](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) para adicionar armazenamento adicional para a sua VM.
* [Criar um instantâneo](snapshot-copy-managed-disk.md).
* [Converter para discos geridos](convert-unmanaged-to-managed-disks.md).
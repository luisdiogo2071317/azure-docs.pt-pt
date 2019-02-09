---
title: Sobre não geridos (blobs de páginas) e geridas o armazenamento de discos para VMs Windows do Microsoft Azure | Documentos da Microsoft
description: Saiba mais sobre as noções básicas do geridos (blobs de páginas) e não geridos armazenamento de discos para máquinas de virtuais do Windows no Azure.
services: virtual-machines-windows,storage
author: roygara
ms.service: virtual-machines-windows
ms.tgt_pltfrm: windows
ms.topic: article
ms.date: 11/15/2017
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 0caa88939e263aa5d18460144893cdbce72f10ec
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2019
ms.locfileid: "55961538"
---
# <a name="about-disks-storage-for-azure-windows-vms"></a>Sobre o armazenamento de discos para VMs do Windows do Azure

Assim como qualquer outro computador, o máquinas virtuais do Azure utilizam discos como um local para armazenar um sistema operacional, aplicativos e dados. Todas as máquinas virtuais do Azure têm, pelo menos, dois discos – um disco de sistema operativo do Windows e um disco temporário. O disco do sistema operativo é criado a partir de uma imagem e o disco do sistema operativo e a imagem são discos rígidos virtuais (VHDs) armazenados numa conta de armazenamento do Azure. Máquinas virtuais também podem ter um ou mais discos de dados, que também são armazenados como VHDs. 

Neste artigo, iremos falar sobre as utilizações diferentes para os discos e, em seguida, discutir os diferentes tipos de discos, pode criar e utilizar. Este artigo também está disponível para [máquinas virtuais do Linux](../linux/about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Discos utilizados por VMs

Vamos dar uma olhada em como os discos são utilizados pelas VMs.

### <a name="operating-system-disk"></a>Disco do sistema operativo

Cada máquina virtual possui um disco de sistema de operativo anexado. É registado como uma unidade SATA e identificado como a unidade c: por predefinição. Este disco tem a capacidade máxima de 2048 gigabytes (GB).

### <a name="temporary-disk"></a>Disco temporário

Cada VM contém um disco temporário. O disco temporário fornece armazenamento de curto prazo para aplicações e processos e destina-se para armazenar apenas os dados, tais como ficheiros de paginação ou de troca. Dados do disco temporário podem ser perdidos durante uma [evento de manutenção](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) ou quando [Reimplementar uma VM](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Durante uma reinicialização bem sucedida padrão da VM, devem manter os dados na unidade temporária. No entanto, há casos em que os dados podem não persistir, por exemplo, transferindo para um novo anfitrião. Da mesma forma, todos os dados na unidade temporária não devem ser dados que é essenciais para o sistema. Ao conceber uma aplicação que utiliza uma unidade temporária como um cache de dados para melhorar o desempenho do aplicativo, partem do princípio em seu design que a cache de dados na unidade temporária é perdida durante a reinicialização e que a aplicação tem de tempo para reconstruir o cache de dados antes de um semelhantes foi atingido o desempenho.

O disco temporário é identificado como a unidade d: por predefinição e ele utilizado para armazenar Pagefile. sys. Para remapear este disco para uma letra de unidade diferente, veja [alterar a letra de unidade do disco temporário Windows](change-drive-letter.md). O tamanho do disco temporário varia, com base no tamanho da máquina virtual. Para obter mais informações, consulte [máquinas de virtuais de tamanhos para Windows](sizes.md).

Para obter mais informações sobre como o Azure utiliza o disco temporário, consulte [Noções básicas sobre a unidade temporária em máquinas virtuais do Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

### <a name="data-disk"></a>Disco de dados

Um disco de dados é um VHD que está ligado a uma máquina virtual para armazenar dados de aplicação ou outros dados que precisa para manter. Discos de dados estão registados como unidades SCSI e são rotulados com uma letra que escolher. Cada disco de dados tem a capacidade máxima de 4095 GB, os discos geridos têm a capacidade máxima de 32.767 GiB. O tamanho da máquina virtual determina quantos discos de dados, pode anexar a ele e o tipo de armazenamento pode utilizar para alojar os discos.

> [!NOTE]
> Para obter mais informações sobre as capacidades de máquinas virtuais, consulte [máquinas de virtuais de tamanhos para Windows](sizes.md).

Quando cria uma máquina virtual a partir de uma imagem, o Azure cria um disco do sistema operativo. Se utilizar uma imagem que inclua discos de dados, o Azure também cria os discos de dados ao criar a máquina virtual. Caso contrário, adicione discos de dados depois de criar a máquina virtual.

Pode adicionar discos de dados a uma máquina virtual em qualquer altura, ao **anexar** o disco à máquina virtual. Pode utilizar um VHD que carregou ou copiados para a sua conta de armazenamento ou utilizar um VHD vazio que o Azure cria para. Anexar um disco de dados, associa o ficheiro VHD com a VM, colocando uma concessão no VHD para que não pode ser eliminada do armazenamento, enquanto ainda estiver a ser ligado.

[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

Para tamanhos de pré-visualização, consulte nosso [FAQ](faq-for-disks.md#new-disk-sizes-managed-and-unmanaged) para saber em que regiões estão disponíveis.

## <a name="one-last-recommendation-use-trim-with-unmanaged-standard-disks"></a>Uma recomendação último: Utilizar TRIM com discos standard não geridos

Se utilizar discos não geridos-standard (HDD), deverá ativar a limitação. TRIM descartará blocos não utilizados no disco, portanto, é-lhe cobrada apenas para o armazenamento de que, na verdade, está a utilizar. Isso pode poupar nos custos se cria arquivos grandes e, em seguida, eliminá-los.

Pode executar este comando para verificar a definição de COMPACTAÇÃO. Abra um prompt de comando na sua VM do Windows e o tipo:

```
fsutil behavior query DisableDeleteNotify
```

Se o comando devolve 0, a limitação está ativada corretamente. Se ela retornar 1, execute o seguinte comando para ativar a limitação:

```
fsutil behavior set DisableDeleteNotify 0
```

> [!NOTE]
> Nota: Suporte para a libertação começa com o Windows Server 2012 / Windows 8 e superior, consulte [nova API permite que as aplicações enviar sugestões de "Cortar e Anular mapeamento" para mídias de armazenamento](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).
> 

<!-- Might want to match next-steps from overview of managed disks -->
## <a name="next-steps"></a>Passos Seguintes
* [Anexar um disco](attach-disk-portal.md) para adicionar armazenamento adicional para a sua VM.
* [Criar um instantâneo](snapshot-copy-managed-disk.md).
* [Converter para discos geridos](convert-unmanaged-to-managed-disks.md).



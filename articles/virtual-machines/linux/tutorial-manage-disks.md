---
title: Gerir discos do Azure com a CLI do Azure | Microsoft Docs
description: Tutorial - Gerir discos do Azure com a CLI do Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 1207ae8160739bcf27a651880dd58ea6893ebf37
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="manage-azure-disks-with-the-azure-cli"></a>Gerir discos do Azure com a CLI do Azure

As máquinas virtuais do Azure utilizam discos para armazenar o sistema operativo, as aplicações e os dados das VMs. Ao criar uma VM, é importante escolher um tamanho de disco e a configuração adequados para a carga de trabalho esperada. Este tutorial abrange a implementação e gestão de discos de VM. Vai obter informações sobre:

> [!div class="checklist"]
> * Discos de SO e discos temporários
> * Discos de dados
> * Discos Standard e Premium
> * Desempenho do disco
> * Expor e preparar os discos de dados
> * Redimensionar discos
> * Instantâneos de disco


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este tutorial requer a execução da versão 2.0.4 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="default-azure-disks"></a>Discos do Azure predefinidos

Quando uma máquina virtual do Azure é criada, dois discos são automaticamente expostos à máquina virtual. 

**Disco do sistema operativo** - os discos do sistema operativo podem ter o tamanho máximo de 1 terabyte, e alojam o sistema operativo das VMs. O disco do SO está identificado como */dev/sda* por predefinição. A configuração da colocação em cache do disco do SO está otimizada para desempenho do SO. Devido a esta configuração, o disco do SO **não deve** alojar aplicações nem dados. Para aplicações e dados, utilize discos de dados, que são descritos posteriormente neste artigo. 

**Disco temporário** - os discos temporários utilizam uma unidade de estado sólido que está localizada no mesmo anfitrião da VM do Azure. Os discos temporários são de elevado desempenho e servem para operações como o processamento de dados temporários. No entanto, se a VM for movida para um novo anfitrião, todos os dados armazenados num disco temporário são removidos. O tamanho do disco temporário é determinado pelo tamanho da VM. Os discos temporários estão identificados como */dev/sdb* e têm um ponto de montagem de */mnt*.

### <a name="temporary-disk-sizes"></a>Tamanhos dos discos temporários

| Tipo | Tamanho da VM | Tamanho máximo de disco temporário (GB) |
|----|----|----|
| [Fins gerais](sizes-general.md) | Séries A e D | 800 |
| [Com otimização de computação](sizes-compute.md) | Série F | 800 |
| [Com otimização de memória](../virtual-machines-windows-sizes-memory.md) | Séries D e G | 6144 |
| [Com otimização de armazenamento](../virtual-machines-windows-sizes-storage.md) | Série L | 5630 |
| [GPU](sizes-gpu.md) | Série N | 1440 |
| [Elevado desempenho](sizes-hpc.md) | Séries A e H | 2000 |

## <a name="azure-data-disks"></a>Discos de dados do Azure

Podem ser adicionados mais discos para instalar aplicações e armazenar dados. Os discos de dados devem ser utilizados em qualquer situação em que se pretenda armazenamento de dados durável e reativo. Cada disco de dados tem a capacidade máxima de 1 terabyte. O tamanho da máquina virtual determina quantos discos de dados podem ser expostos a uma VM. Para cada vCPU de VM, podem ser expostos dois discos de dados. 

### <a name="max-data-disks-per-vm"></a>Discos de dados máximos por VM

| Tipo | Tamanho da VM | Discos de dados máximos por VM |
|----|----|----|
| [Fins gerais](sizes-general.md) | Séries A e D | 32 |
| [Com otimização de computação](sizes-compute.md) | Série F | 32 |
| [Com otimização de memória](../virtual-machines-windows-sizes-memory.md) | Séries D e G | 64 |
| [Com otimização de armazenamento](../virtual-machines-windows-sizes-storage.md) | Série L | 64 |
| [GPU](sizes-gpu.md) | Série N | 48 |
| [Elevado desempenho](sizes-hpc.md) | Séries A e H | 32 |

## <a name="vm-disk-types"></a>Tipos de disco de VM

O Azure oferece dois tipos de disco.

### <a name="standard-disk"></a>Disco Standard

O Armazenamento Standard está protegido por HDDs e fornece armazenamento económico, mantendo o desempenho. Os discos Standard são ideais para uma carga de trabalho de desenvolvimento e teste económica.

### <a name="premium-disk"></a>Disco Premium

Os discos Premium são apoiados por um disco de elevado desempenho baseado em SSD e de baixa latência. São perfeitos para as VMs com carga de trabalho de produção. O Armazenamento Premium suporta VMs da série DS, série DSv2, série GS e série FS. Os discos Premium são fornecidos em três tipos (P10, P20, P30), e o tamanho do disco determina o tipo de disco. Ao selecionar um tamanho de disco, o valor é arredondado para o tipo seguinte. Por exemplo, se o tamanho do disco for inferior a 128 GB, o tipo de disco é P10. Se o tamanho do disco estiver entre 129 GB e 512 GB, o tamanho é P20. Para tudo o que estiver acima de 512 GB, o tamanho é P30.

### <a name="premium-disk-performance"></a>Desempenho do disco Premium

|Tipo de disco de armazenamento Premium | P10 | P20 | P30 |
| --- | --- | --- | --- |
| Tamanho do disco (arredondado) | 128 GB | 512 GB | 1.024 GB (1 TB) |
| IOPs Máx por disco | 500 | 2.300 | 5.000 |
Débito por disco | 100 MB/s | 150 MB/s | 200 MB/s |

Enquanto a tabela acima identifica o IOPS máximo por disco, um nível mais elevado de desempenho pode ser alcançado ao repartir vários discos de dados. Por exemplo, uma VM Standard_GS5 pode atingir o máximo de 80 000 IOPS. Para obter informações detalhadas sobre o IOPS máximo por VM, consulte [Tamanhos de VM do Linux](sizes.md).

## <a name="create-and-attach-disks"></a>Criar e expor discos

Os discos de dados podem ser criados e expostos no momento de criação da VM ou para uma VM existente.

### <a name="attach-disk-at-vm-creation"></a>Expor o disco durante a criação de VM

Crie um grupo de recursos com o comando [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create). 

```azurecli-interactive 
az group create --name myResourceGroupDisk --location eastus
```

Crie uma VM com o comando [z vm create]( /cli/azure/vm#az_vm_create). O argumento `--datadisk-sizes-gb` é utilizado para especificar que um disco adicional deve ser criado e exposto à máquina virtual. Para criar e expor mais do que um disco, utilize uma lista delimitada por espaços dos valores de tamanho de disco. No exemplo seguinte, é criada uma VM com dois discos de dados, ambos de 128 GB. Porque os tamanhos de disco são de 128 GB, estes discos são configurados como P10s, que fornecem o máximo de 500 IOPS por disco.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupDisk \
  --name myVM \
  --image UbuntuLTS \
  --size Standard_DS2_v2 \
  --data-disk-sizes-gb 128 128 \
  --generate-ssh-keys
```

### <a name="attach-disk-to-existing-vm"></a>Expor o disco a uma VM existente

Para criar e expor um disco novo a uma máquina virtual existente, utilize o comando [az vm disk attach](/cli/azure/vm/disk#az_vm_disk_attach). O exemplo seguinte cria um disco premium, de 128 gigabytes de tamanho, e expõe-o à VM criada no último passo.

```azurecli-interactive 
az vm disk attach --vm-name myVM --resource-group myResourceGroupDisk --disk myDataDisk --size-gb 128 --sku Premium_LRS --new 
```

## <a name="prepare-data-disks"></a>Preparar dados de discos

Depois de um disco ser exposto à máquina virtual, o sistema operativo tem de ser configurado para utilizar o disco. O exemplo seguinte mostra como configurar manualmente um disco. Este processo também pode ser automatizado através da inicialização da cloud, que é abordada num [tutorial posterior](./tutorial-automate-vm-deployment.md).

### <a name="manual-configuration"></a>Configuração manual

Crie uma ligação SSH com a máquina virtual. Substitua o endereço IP de exemplo pelo IP público da máquina virtual.

```azurecli-interactive 
ssh 52.174.34.95
```

Particione o disco com `fdisk`.

```bash
(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
```

Escreva um sistema de ficheiros na partição com o comando `mkfs`.

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Monte o disco novo para que fique acessível no sistema operativo.

```bash
sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
```

O disco pode agora ser acedido através do ponto de montagem *datadrive*, que pode ser verificado com o comando `df -h`. 

```bash
df -h
```

A saída mostra o novo disco montado em */datadrive*.

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        30G  1.4G   28G   5% /
/dev/sdb1       6.8G   16M  6.4G   1% /mnt
/dev/sdc1        50G   52M   47G   1% /datadrive
```

Para garantir que a unidade é remontada após uma reinicialização, tem de ser adicionada ao ficheiro *etc/fstab*. Para tal, obtenha o UUID do disco com o utilitário `blkid`.

```bash
sudo -i blkid
```

A saída apresenta o UUID da unidade, `/dev/sdc1` neste caso.

```bash
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

Adicione uma linha semelhante à seguinte ao ficheiro *etc/fstab*.

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  ext4    defaults,nofail   1  2
```

Agora que o disco foi configurado, feche a sessão SSH.

```bash
exit
```

## <a name="resize-vm-disk"></a>Redimensionar um disco de VM

Assim que uma VM tiver sido implementada, o tamanho do disco do sistema operativo ou de quaisquer discos de dados expostos pode ser aumentado. Aumentar o tamanho de um disco é vantajoso quando necessitar de mais espaço de armazenamento ou de um nível mais elevado de desempenho (P10 P20, P30). Tenha em atenção que não pode diminuir o tamanho dos discos.

Antes de aumentar o tamanho do disco, é necessário o Id ou nome do disco. Utilize o comando [az disk list](/cli/azure/disk#az_disk_list) para obter todos os discos num grupo de recursos. Tome nota do nome do disco que gostaria de redimensionar.

```azurecli-interactive 
az disk list -g myResourceGroupDisk --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table
```

A VM também tem de ser desalocada. Utilize o comando [az vm deallocate]( /cli/azure/vm#az_vm_deallocate) para parar e desalocar a VM.

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupDisk --name myVM
```

Utilize o comando [az disk update](/cli/azure/vm/disk#az_vm_disk_update) para redimensionar o disco. Este exemplo redimensiona um disco com o nome *myDataDisk* para 1 terabyte.

```azurecli-interactive 
az disk update --name myDataDisk --resource-group myResourceGroupDisk --size-gb 1023
```

Depois de a operação de redimensionamento estar concluída, inicie a VM.

```azurecli-interactive 
az vm start --resource-group myResourceGroupDisk --name myVM
```

Se tiver redimensionado o disco do sistema operativo, a partição é automaticamente expandida. Se tiver redimensionado um disco de dados, quaisquer partições atuais têm de ser expandidas no sistema operativo das VMs.

## <a name="snapshot-azure-disks"></a>Instantâneos de discos do Azure

Tirar um instantâneo de disco cria uma cópia só de leitura de um ponto no tempo do disco. Os instantâneos de VM do Azure são úteis para guardar rapidamente o estado de uma VM antes de efetuar alterações de configuração. Caso se conclua que as alterações da configuração são indesejadas, o estado da VM pode ser restaurado com o instantâneo. Quando uma VM tiver mais do que um disco, é tirado um instantâneo de cada disco independentemente dos outros. Para fazer cópias de segurança da aplicação, considere parar a VM antes de tirar instantâneos do disco. Em alternativa, utilize o [Serviço Azure Backup](/azure/backup/), que permite efetuar cópias de segurança automatizadas enquanto a VM está em execução.

### <a name="create-snapshot"></a>Criar instantâneo

Antes de criar um instantâneo do disco da máquina virtual, é necessário o Id ou nome do disco. Utilize o comando [az vm show](https://docs.microsoft.com/cli/azure/vm#az_vm_show) para obter o id do disco. Neste exemplo, o id do disco é armazenado numa variável, para que possa ser utilizado num passo posterior.

```azurecli-interactive 
osdiskid=$(az vm show -g myResourceGroupDisk -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

Agora que tem o id do disco da máquina virtual, o comando seguinte cria um instantâneo do disco.

```azurcli
az snapshot create -g myResourceGroupDisk --source "$osdiskid" --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a>Criar disco a partir de instantâneo

Este instantâneo pode então ser convertido num disco, que pode ser utilizado para recriar a máquina virtual.

```azurecli-interactive 
az disk create --resource-group myResourceGroupDisk --name mySnapshotDisk --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a>Restaurar a máquina virtual a partir de instantâneo

Para demonstrar a recuperação da máquina virtual, elimine a máquina virtual existente. 

```azurecli-interactive 
az vm delete --resource-group myResourceGroupDisk --name myVM
```

Crie uma máquina virtual nova a partir do disco do instantâneo.

```azurecli-interactive 
az vm create --resource-group myResourceGroupDisk --name myVM --attach-os-disk mySnapshotDisk --os-type linux
```

### <a name="reattach-data-disk"></a>Voltar a expor disco de dados

Todos os discos de dados têm de ser novamente expostos à máquina virtual.

Em primeiro lugar, localize o nome do disco de dados com o comando [az disk list](https://docs.microsoft.com/cli/azure/disk#az_disk_list). Este exemplo coloca o nome do disco numa variável designada *datadisk*, que é utilizada no próximo passo.

```azurecli-interactive 
datadisk=$(az disk list -g myResourceGroupDisk --query "[?contains(name,'myVM')].[name]" -o tsv)
```

Utilize o comando [az vm disk attach](https://docs.microsoft.com/cli/azure/vm/disk#az_vm_disk_attach) para expor o disco.

```azurecli-interactive 
az vm disk attach –g myResourceGroupDisk –-vm-name myVM –-disk $datadisk
```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu sobre os tópicos de discos de VM, como:

> [!div class="checklist"]
> * Discos de SO e discos temporários
> * Discos de dados
> * Discos Standard e Premium
> * Desempenho do disco
> * Expor e preparar os discos de dados
> * Redimensionar discos
> * Instantâneos de disco

Avance para o próximo tutorial para saber mais sobre como automatizar a configuração da VM.

> [!div class="nextstepaction"]
> [Automatizar a configuração da VM](./tutorial-automate-vm-deployment.md)

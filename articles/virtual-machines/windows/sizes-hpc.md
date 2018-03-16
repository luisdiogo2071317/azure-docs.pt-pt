---
title: Tamanhos de VM do Windows Azure - HPC | Microsoft Docs
description: "Lista os tamanhos diferentes disponíveis para Windows computação de alto desempenho máquinas virtuais no Azure. Apresenta informações sobre o número de vCPUs, discos de dados e NICs, bem como armazenamento débito e a rede de largura de banda para tamanhos de nesta série."
services: virtual-machines-windows
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/15/2018
ms.author: jonbeck
ms.openlocfilehash: 6f2c72689811d26f95a64fdf5f473606f3ea3f7d
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="high-performance-compute-vm-sizes"></a>Tamanhos de VM de computação de elevado desempenho

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


* **Sistema operativo** -Windows Server 2016, o Windows Server 2012 R2, Windows Server 2012

* **MPI** -MPI Microsoft (MS-MPI) 2012 R2 ou posterior, Intel MPI biblioteca 5. x

  Implementações de MPI suportadas utilizam a interface Microsoft Network Direct para comunicar entre instâncias. 

* **Espaço de endereços de rede RDMA** -o endereço espaço 172.16.0.0/16 de reservas de rede RDMA o no Azure. Para executar aplicações MPI no instâncias implementadas numa rede virtual do Azure, certifique-se de que o espaço de endereços de rede virtual não se sobreponha a rede RDMA.

* **Extensão da HpcVmDrivers VM** - em VMs com capacidade RDMA, adicione a extensão de HpcVmDrivers para instalar controladores de dispositivo de rede do Windows para conectividade RDMA. (Em determinadas implementações das instâncias A8 e A9, a extensão de HpcVmDrivers é adicionada automaticamente.) Para adicionar a extensão VM a uma VM, pode utilizar [Azure PowerShell](/powershell/azure/overview) cmdlets. 

  
  O seguinte comando instala a mais recente HpcVMDrivers extensão da versão 1.1 numa VM com capacidade RDMA existente com o nome *myVM* implementada no grupo de recursos com o nome *myResourceGroup* no *EUA oeste* região:

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  Para obter mais informações, consulte [extensões de Máquina Virtual e funcionalidades](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Pode também funcionar com as extensões para as VMs implementadas no [modelo de implementação clássica](classic/manage-extensions.md).


## <a name="using-hpc-pack"></a>Utilizar o pacote HPC

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), gratuita HPC cluster e a tarefa de solução de gestão da Microsoft, é uma opção para criar um cluster de cálculo no Azure para executar aplicações MPI baseados em Windows e outras cargas de trabalho HPC. Pacote HPC 2012 R2 e versões posteriores incluem um ambiente de tempo de execução para MS-MPI que utiliza a rede de Azure RDMA, quando implementados em VMs com capacidade RDMA.



## <a name="other-sizes"></a>Outros tamanhos de
- [Fins gerais](sizes-general.md)
- [Com otimização de computação](sizes-compute.md)
- [Com otimização de memória](../virtual-machines-windows-sizes-memory.md)
- [Com otimização de armazenamento](../virtual-machines-windows-sizes-storage.md)
- [Com otimização de GPU](sizes-gpu.md)

## <a name="next-steps"></a>Passos seguintes

- Para listas de verificação utilizar as instâncias intensivas de computação com o HPC Pack no Windows Server, consulte [configurar um cluster do Windows RDMA com o HPC Pack para executar aplicações MPI](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

- Para utilizar instâncias intensivas de computação quando executar aplicações de MPI no Batch do Azure, consulte [utilizar tarefas de várias instâncias para executar aplicações de Interface de passagem de mensagens (MPI) no Azure Batch](../../batch/batch-mpi.md).

- Saiba mais sobre como [unidades (ACU) de computação do Azure](acu.md) podem ajudar a comparar o desempenho de computação em SKUs do Azure.





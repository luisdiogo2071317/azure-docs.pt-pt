---
title: Tamanhos de VM do Windows Azure – HPC | Documentos da Microsoft
description: Lista os tamanhos diferentes disponíveis para Windows computação de alto desempenho máquinas virtuais no Azure. Lista as informações sobre o número de vCPUs, discos de dados e NICs, bem como armazenamento e débito de rede largura de banda para tamanhos nesta série.
services: virtual-machines-windows
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/06/2018
ms.author: jonbeck
ms.openlocfilehash: 31e81741d2a627888e478b3871bdbab4e6b6d6f5
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37902644"
---
# <a name="high-performance-compute-vm-sizes"></a>Tamanhos de VM de computação de elevado desempenho

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


* **Sistema operativo** -Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

* **MPI** -Microsoft MPI (MS-MPI) 2012 R2 ou posterior, Intel MPI biblioteca 5.x

  Implementações de MPI suportadas utilizam a interface direta de rede do Microsoft para comunicar entre instâncias. 

* **Espaço de endereços de rede RDMA** -rede o RDMA no Azure reserva-se o endereço espaço 172.16.0.0/16. Para executar aplicações MPI em instâncias implementadas numa rede virtual do Azure, certifique-se de que o espaço de endereços de rede virtual não se sobreponha a rede RDMA.

* **Extensão de HpcVmDrivers VM** - em VMs com capacidade RDMA, adicione a extensão de HpcVmDrivers para instalar controladores de dispositivo de rede Windows para a conectividade RDMA. (Em determinadas implementações das instâncias A8 e A9, a extensão de HpcVmDrivers é adicionada automaticamente.) Para adicionar a extensão de VM a uma VM, pode utilizar [do Azure PowerShell](/powershell/azure/overview) cmdlets. 

  
  O seguinte comando instala a extensão de HpcVMDrivers versão 1.1 mais recente numa VM com capacidade RDMA existente com o nome *myVM* implementados no grupo de recursos com o nome *myResourceGroup* no  *Oeste dos E.U.A.* região:

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  Para obter mais informações, consulte [extensões de Máquina Virtual e funcionalidades](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Também pode trabalhar com as extensões para as VMs implementadas no [modelo de implementação clássica](classic/manage-extensions.md).


## <a name="using-hpc-pack"></a>Com o HPC Pack

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), gratuita HPC cluster e a tarefa de solução de gestão da Microsoft, é uma opção de criar um cluster de computação no Azure para executar aplicações MPI baseados em Windows e outras cargas de trabalho HPC. HPC Pack 2012 R2 e versões posteriores incluem um ambiente de tempo de execução para MS-MPI, que utiliza a rede de RDMA do Azure quando implementadas em VMs com capacidade RDMA.



## <a name="other-sizes"></a>Outros tamanhos
- [Fins gerais](sizes-general.md)
- [Com otimização de computação](sizes-compute.md)
- [Com otimização de memória](../virtual-machines-windows-sizes-memory.md)
- [Com otimização de armazenamento](../virtual-machines-windows-sizes-storage.md)
- [Com otimização de GPU](sizes-gpu.md)
- [Gerações anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Passos Seguintes

- Para listas de verificação utilizar as instâncias de computação intensiva, com o HPC Pack no Windows Server, consulte [configurar um cluster Windows RDMA com HPC Pack para executar aplicações MPI](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

- Para utilizar instâncias de computação intensiva ao executar aplicações MPI com o Azure Batch, veja [utilizar tarefas de várias instâncias para executar aplicações Message Passing Interface (MPI) no Azure Batch](../../batch/batch-mpi.md).

- Saiba mais sobre como [computação do Azure (ACU) de unidades](acu.md) pode ajudá-lo a comparar o desempenho de computação nos SKUs do Azure.





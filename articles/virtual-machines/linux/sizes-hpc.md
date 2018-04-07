---
title: Tamanhos de VM Linux do Azure - HPC | Microsoft Docs
description: Lista os tamanhos diferentes disponíveis para Linux elevado desempenho informática máquinas virtuais no Azure. Apresenta informações sobre o número de vCPUs, discos de dados e NICs, bem como armazenamento débito e a rede de largura de banda para tamanhos de nesta série.
services: virtual-machines-linux
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/15/2018
ms.author: jonbeck
ms.openlocfilehash: a24cb03cd30b212650a36cd5ac40977de5eea11e
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="high-performance-compute-virtual-machine-sizes"></a>Tamanhos de máquinas virtuais de computação de elevado desempenho

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


### <a name="mpi"></a>MPI 

Versões de 5. x de Intel MPI só são suportadas. Versões posteriores (2017, 2018) da biblioteca de tempo de execução Intel MPI não são compatíveis com os controladores de Azure Linux RDMA.


### <a name="distributions"></a>Distribuições
 
Implemente uma VM de computação intensivas de uma das imagens no Azure Marketplace que suporte a conectividade RDMA:
  
* **Ubuntu** -Ubuntu Server 16.04 LTS. Configurar controladores RDMA na VM e registe o Intel para transferir Intel MPI:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

* **SUSE Linux Enterprise Server** -SLES 12 SP3 para HPC, SLES 12 SP3 para HPC (Premium), SLES 12 SP1 para HPC, SLES 12 SP1 para HPC (Premium). RDMA controladores estão instalados e pacotes de Intel MPI são distribuídas na VM. Instale MPI executando o seguinte comando:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
    
* **Com base em centOS HPC** -com base em CentOS 6.5 HPC ou uma versão posterior (para a série H, versão 7.1 ou posterior é recomendado). Controladores RDMA e Intel MPI 5.1 são instaladas na VM.  
 
  > [!NOTE]
  > Nas imagens de HPC com base em CentOS, atualizações de kernel estão desativadas no **yum** ficheiro de configuração. Isto acontece porque os controladores de Linux RDMA são distribuídos como um pacote RPM e atualizações de controladores poderão não funcionar se kernel é atualizado.
  > 
 
### <a name="cluster-configuration"></a>Configuração de cluster 
    
É necessária configuração de sistema adicionais para executar tarefas MPI em VMs em cluster. Por exemplo, num cluster de VMs, tem de estabelecer fidedignidade entre os nós de computação. Para as definições típicas, consulte [configurar um cluster de Linux RDMA para executar aplicações MPI](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

### <a name="network-topology-considerations"></a>Considerações de topologia de rede
* Em com capacidade RDMA VMs com Linux no Azure, Eth1 está reservado para o tráfego de rede RDMA. Não altere as definições de Eth1 ou quaisquer informações no ficheiro de configuração que faça referência a esta rede. Eth0 está reservado para regular tráfego de rede do Azure.

* O endereço espaço 172.16.0.0/16 de reservas de rede RDMA no Azure. 


## <a name="using-hpc-pack"></a>Utilizar o pacote HPC
[Pacote HPC](https://technet.microsoft.com/library/jj899572.aspx), gratuita HPC cluster e a tarefa de solução de gestão da Microsoft, é uma opção para utilizar as instâncias intensivas de computação com Linux. As versões mais recentes do suporte de HPC Pack várias distribuições em Linux para executar em nós de computação do implementado em VMs do Azure, gerido por um nó principal do Windows Server. Connosco de computação do Linux com capacidade RDMA com Intel MPI, HPC Pack pode agendar e executar Linux MPI aplicações que acedam à rede RDMA. Consulte [começar connosco de computação do Linux num cluster HPC Pack no Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="other-sizes"></a>Outros tamanhos de
- [Fins gerais](sizes-general.md)
- [Com otimização de computação](sizes-compute.md)
- [Com otimização de memória](sizes-memory.md)
- [Com otimização de armazenamento](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)


## <a name="next-steps"></a>Passos Seguintes

- Para começar a implementar e utilizar tamanhos de computação intensiva com o RDMA no Linux, consulte [configurar um cluster de Linux RDMA para executar aplicações MPI](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

- Saiba mais sobre como [unidades (ACU) de computação do Azure](acu.md) podem ajudar a comparar o desempenho de computação em SKUs do Azure.





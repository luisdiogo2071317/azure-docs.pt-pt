---
title: Tamanhos de VM do Linux do Azure – HPC | Documentos da Microsoft
description: Lista os tamanhos diferentes disponíveis para máquinas virtuais no Azure de computação de Linux ao alto desempenho. Lista as informações sobre o número de vCPUs, discos de dados e NICs, bem como armazenamento e débito de rede largura de banda para tamanhos nesta série.
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
ms.date: 07/06/2018
ms.author: jonbeck
ms.openlocfilehash: 748cb4612b2b5aed26ba8197cfad0782f2645e1e
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37902134"
---
# <a name="high-performance-compute-virtual-machine-sizes"></a>Tamanhos de máquinas virtuais de computação de elevado desempenho

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


### <a name="mpi"></a>MPI 

Apenas o Intel MPI 5.x versões são suportadas. Versões posteriores (2017, 2018) da biblioteca de tempo de execução do Intel MPI não são compatíveis com os controladores de RDMA do Linux do Azure.


### <a name="distributions"></a>Distribuições
 
Implemente uma VM de computação intensiva de uma das imagens no Azure Marketplace que suporta a conetividade RDMA:
  
* **Ubuntu** -Ubuntu Server 16.04 LTS. Configurar controladores RDMA na VM e registar com o Intel para transferir a MPI Intel:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

* **SUSE Linux Enterprise Server** -SLES 12 SP3 para HPC, SLES 12 SP3 para HPC (Premium), SLES 12 SP1 para HPC, SLES 12 SP1 para HPC (Premium). Controladores de RDMA são instalados e os pacotes de Intel MPI são distribuídos na VM. Instale o MPI executando o seguinte comando:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
    
* **Baseada em centOS HPC** -baseada em CentOS 6.5 HPC ou uma versão posterior (para a série H, versão 7.1 ou posterior é recomendado). Drivers RDMA e Intel MPI 5.1 são instalados na VM.  
 
  > [!NOTE]
  > Nas imagens de HPC baseada em CentOS, as atualizações de kernel estão desativadas no **yum** ficheiro de configuração. Isso ocorre porque os drivers de RDMA do Linux são distribuídos como um pacote RPM e atualizações de controladores poderão não funcionar se o kernel é atualizado.
  > 
 
### <a name="cluster-configuration"></a>Configuração de cluster 
    
Configuração de sistema adicionais é necessário para executar trabalhos de MPI em VMs em cluster. Por exemplo, num cluster de VMs, tem de estabelecer confiança entre os nós de computação. Para as definições típicas, consulte [configurar um cluster RDMA do Linux para executar aplicações MPI](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

### <a name="network-topology-considerations"></a>Considerações de topologia de rede
* Em VMs capacidade RDMA do Linux no Azure, Eth1 está reservado para o tráfego de rede RDMA. Não altere as definições de Eth1 ou todas as informações no ficheiro de configuração que faça referência a esta rede. Eth0 está reservado para regular tráfego de rede do Azure.

* A rede RDMA no Azure reserva-se o endereço espaço 172.16.0.0/16. 


## <a name="using-hpc-pack"></a>Com o HPC Pack
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), gratuita HPC cluster e a tarefa de solução de gestão da Microsoft, é uma opção para utilizar as instâncias de computação intensiva, com o Linux. As versões mais recentes do suporte do HPC Pack várias distribuições do Linux para serem executadas em nós de computação implementados em VMs do Azure, gerido por um nó principal do Windows Server. Connosco de computação do Linux com capacidade RDMA com o Intel MPI, o HPC Pack pode agendar e executar Linux MPI aplicações que acedam à rede RDMA. Ver [começar connosco de computação do Linux num cluster HPC Pack no Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="other-sizes"></a>Outros tamanhos
- [Fins gerais](sizes-general.md)
- [Com otimização de computação](sizes-compute.md)
- [Com otimização de memória](sizes-memory.md)
- [Com otimização de armazenamento](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)
- [Gerações anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Passos Seguintes

- Para começar a implementar e utilizar tamanhos de computação intensiva com RDMA no Linux, veja [configurar um cluster RDMA do Linux para executar aplicações MPI](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

- Saiba mais sobre como [computação do Azure (ACU) de unidades](acu.md) pode ajudá-lo a comparar o desempenho de computação nos SKUs do Azure.





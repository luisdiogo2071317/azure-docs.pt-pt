---
title: Opções de cluster Windows HPC Pack no Azure | Microsoft Docs
description: Saiba mais sobre as opções de com o Microsoft HPC Pack para criar e gerir um desempenho elevado do Windows computação em cluster (HPC) na nuvem do Azure
services: virtual-machines-windows,cloud-services,batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 02c5566d-2129-483c-9ecf-0d61030442d7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/14/2018
ms.author: danlep
ms.openlocfilehash: 1232228d5e2fcd05f41a096a933072dffcca2119
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="options-with-hpc-pack-to-create-and-manage-a-cluster-for-windows-hpc-workloads-in-azure"></a>Opções com o HPC Pack para criar e gerir um cluster para cargas de trabalho do Windows HPC no Azure
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Este artigo foca-se nas opções do Azure para criar clusters HPC Pack executar cargas de trabalho do Windows. Também existem opções para a criação de clusters HPC Pack para executar [cargas de trabalho Linux HPC](../linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="hpc-pack-cluster-in-azure-vms-and-vm-scale-sets"></a>Cluster HPC Pack em VMs do Azure e os conjuntos de dimensionamento VM
### <a name="azure-resource-manager-templates"></a>Modelos do Azure Resource Manager
* (GitHub) [Modelos de cluster HPC Pack 2016 atualização 1](https://github.com/MsHpcPack/HPCPack2016)
* (GitHub) [Modelos de cluster HPC Pack 2012 R2](https://github.com/MsHpcPack/HPCPack2012R2)
* (Início rápido) [Criar um cluster HPC Pack 2012 R2](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)
* (Início rápido) [Criar um cluster HPC Pack 2012 R2 com a imagem do nó de computação personalizada](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Imagens VM do Azure
Procurar [imagens pacote HPC no Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?page=1&search=%22HPC%20%20Pack%22) se pretender criar o seu próprio cluster HPC Pack no Azure.


### <a name="tutorials"></a>Tutoriais
* [Tutorial: Implementar um cluster HPC Pack 2016 no Azure](hpcpack-2016-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Gerir um cluster HPC Pack 2016 no Azure com o Azure Active Directory](hpcpack-cluster-active-directory.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)


## <a name="hpc-pack-2012-r2-cluster-deployment-in-the-classic-deployment-model"></a>Implementação de cluster HPC Pack 2012 R2 no modelo de implementação clássica
* [Criar um cluster HPC com o script de implementação do IaaS do HPC Pack](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Tutorial: Introdução com um cluster HPC Pack no Azure para executar cargas de trabalho do Excel e SOA](excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Gerir os nós de computação de um cluster HPC Pack no Azure](classic/hpcpack-cluster-node-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Aumentar e diminuir a recursos de computação do Azure num cluster HPC Pack](classic/hpcpack-cluster-node-autogrowshrink.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Configurar um cluster RDMA do Windows com HPC Pack para executar aplicações MPI](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)


## <a name="burst-to-azure-from-hpc-pack-2012-r2"></a>Impulsar para o Azure a partir do HPC Pack 2012 R2
* [Impulsar para instâncias de trabalho do Azure com o Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)
* [Aceder ao Azure Batch com o HPC Pack](https://technet.microsoft.com/library/mt612877.aspx)
* [Tutorial: Configurar um cluster de híbrido com o HPC Pack no Azure](../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)

## <a name="job-submission"></a>Submissão de tarefa

* [Submeter as tarefas para um cluster HPC Pack no Azure](hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)







---
title: Utilizar VMs de computação intensiva do Azure com o Batch | Documentos da Microsoft
description: Como tirar proveito dos tamanhos de VM com capacidade RDMA ou ativadas para GPU em conjuntos do Azure Batch
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2018
ms.author: danlep
ms.openlocfilehash: 6969f0c6a05ebf5b34fb746d2a83b884687ad710
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258260"
---
# <a name="use-rdma-capable-or-gpu-enabled-instances-in-batch-pools"></a>Utilizar com capacidade RDMA ou ativadas para GPU instâncias em conjuntos do Batch

Para executar determinadas tarefas do Batch, pode querer tirar partido de tamanhos de VM do Azure concebido para a computação em grande escala. Por exemplo, para executar várias instâncias [cargas de trabalho MPI](batch-mpi.md), pode escolher A8, A9, ou para acesso remoto direto memória (RDMA) de interface de tamanhos de série H, que tem uma rede. Estes tamanhos de ligar a uma rede InfiniBand para a comunicação entre nós, que pode acelerar aplicações MPI. Ou para aplicativos de CUDA, pode escolher tamanhos de série N que incluem placas (GPU unidade) de processamento de gráfico NVIDIA Tesla.

Este artigo fornece orientação e exemplos para usar alguns dos tamanhos de especializado do Azure em conjuntos do Batch. Para especificações e em segundo plano, consulte:

* Tamanhos de VM de computação de alto desempenho ([Linux](../virtual-machines/linux/sizes-hpc.md), [Windows](../virtual-machines/windows/sizes-hpc.md)) 

* Tamanhos de VM ativadas para GPU ([Linux](../virtual-machines/linux/sizes-gpu.md), [Windows](../virtual-machines/windows/sizes-gpu.md)) 


## <a name="subscription-and-account-limits"></a>Subscrição e limites de conta

* **Quotas e limites** – a [quota de núcleos por conta do Batch](batch-quota-limit.md#resource-quotas) pode limitar o número de nós de um determinado tamanho, pode adicionar a um conjunto do Batch. É mais provável atingir uma quota ao escolher com capacidade RDMA, ativadas para GPU ou outros tamanhos de VM com vários núcleos. 

  Além disso, a utilização de certas famílias VM na sua conta do Batch, por exemplo, a NCv2, NCv3 e ND, está limitada devido à capacidade limitada. Ao pedir um aumento de quota da predefinição de núcleos de 0, o uso dessas famílias só está disponível.  

  Se for necessário, [pedir um aumento de quota](batch-quota-limit.md#increase-a-quota) sem encargos.

* **Disponibilidade de região** - computação intensiva VMs poderão não estar disponíveis nas regiões onde criar as contas do Batch. Para verificar que um tamanho está disponível, consulte [produtos disponíveis por região](https://azure.microsoft.com/regions/services/).


## <a name="dependencies"></a>Dependências

As capacidades RDMA e GPU de tamanhos de computação intensiva são suportadas apenas em determinados sistemas operativos. Consoante o sistema operativo, poderá ter de instalar ou configurar o controlador adicional ou outro software. As tabelas seguintes resumem essas dependências. Consulte artigos ligados para obter detalhes. Para obter opções configurar os conjuntos do Batch, consulte mais adiante neste artigo.


### <a name="linux-pools---virtual-machine-configuration"></a>Conjuntos de Linux - configuração de Máquina Virtual

| Tamanho | Capacidade | Sistemas operativos | Software necessário | Definições do conjunto |
| -------- | -------- | ----- |  -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/linux/sizes-hpc.md#rdma-capable-instances) | RDMA | Ubuntu 16.04 LTS,<br/>SUSE Linux Enterprise Server 12 HPC, ou<br/>Baseada em centOS HPC<br/>(O azure Marketplace) | Intel MPI 5 | Ativar a comunicação entre nós, desabilite a execução de tarefa em simultâneo |
| [NC, a NCv2, NCv3, série ND *](../virtual-machines/linux/n-series-driver-setup.md) | NVIDIA Tesla GPU (varia por série) | Ubuntu 16.04 LTS,<br/>Red Hat Enterprise Linux 7.3 ou 7.4, ou<br/>CentOS 7.3 ou 7.4<br/>(O azure Marketplace) | Controladores de NVIDIA CUDA Toolkit | N/A | 
| [Série NV](../virtual-machines/linux/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | Ubuntu 16.04 LTS,<br/>Red Hat Enterprise Linux 7.3, ou<br/>CentOS 7.3<br/>(O azure Marketplace) | Controladores de GRID da NVIDIA | N/A |

* Conectividade RDMA em VMs de série N com capacidade RDMA pode exigir [configuração adicional](../virtual-machines/linux/n-series-driver-setup.md#rdma-network-connectivity) que varia consoante a distribuição.



### <a name="windows-pools---virtual-machine-configuration"></a>Agrupamentos do Windows - configuração de Máquina Virtual

| Tamanho | Capacidade | Sistemas operativos | Software necessário | Definições do conjunto |
| -------- | ------ | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2016, 2012 R2, ou<br/>2012 (do azure Marketplace) | Microsoft MPI 2012 R2 ou posterior, ou<br/> Intel MPI 5<br/><br/>Extensão de VM do Azure HpcVMDrivers | Ativar a comunicação entre nós, desabilite a execução de tarefa em simultâneo |
| [NC, a NCv2, NCv3, série ND *](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla GPU (varia por série) | Windows Server 2016 ou <br/>2012 R2 (do Azure Marketplace) | Controladores de NVIDIA Tesla ou drivers de CUDA Toolkit| N/A | 
| [Série NV](../virtual-machines/windows/n-series-driver-setup.md) | NVIDIA Tesla M60 GPU | Windows Server 2016 ou<br/>2012 R2 (do Azure Marketplace) | Controladores de GRID da NVIDIA | N/A |

* Conectividade RDMA em VMs de série N com capacidade RDMA é suportada no Windows Server 2016 ou Windows Server 2012 R2 (a partir do Azure Marketplace) com a extensão de HpcVMDrivers e Microsoft MPI ou do Intel MPI.

### <a name="windows-pools---cloud-services-configuration"></a>Agrupamentos do Windows - configuração de serviços Cloud

> [!NOTE]
> Tamanhos de série N não são suportados em conjuntos do Batch com a configuração de serviço em nuvem.
>

| Tamanho | Capacidade | Sistemas operativos | Software necessário | Definições do conjunto |
| -------- | ------- | -------- | -------- | ----- |
| [H16r, H16mr, A8, A9](../virtual-machines/windows/sizes-hpc.md#rdma-capable-instances) | RDMA | Windows Server 2016, 2012 R2, 2012, ou<br/>2008 R2 (família de SO convidado) | Microsoft MPI 2012 R2 ou posterior, ou<br/>Intel MPI 5<br/><br/>Extensão de VM do Azure HpcVMDrivers | Ativar a comunicação entre nós,<br/> desabilite a execução de tarefa em simultâneo |





## <a name="pool-configuration-options"></a>Opções de configuração de conjunto

Para configurar um tamanho de VM especializado para o seu conjunto do Batch, as APIs do Batch e as ferramentas fornecem várias opções para instalar o software necessário ou drivers, incluindo:

* [Iniciar tarefa](batch-api-basics.md#start-task) -carregar um pacote de instalação como um arquivo de recursos para uma conta de armazenamento do Azure na mesma região que a conta do Batch. Crie uma linha de comando de tarefa de início para instalar o arquivo de recursos automaticamente quando o conjunto é iniciado. Para obter mais informações, veja a [Documentação da API REST](/rest/api/batchservice/add-a-pool-to-an-account#bk_starttask).

  > [!NOTE] 
  > A tarefa de início deve ser executado com permissões elevadas (administrador), e ele deve esperar para o sucesso.
  >

* [Pacote de aplicação](batch-application-packages.md) - adicionar um pacote de instalação zipado para sua conta do Batch e configurar uma referência de pacote no conjunto. Esta definição carrega e unzips o pacote em todos os nós no conjunto. Se o pacote de um instalador, crie uma linha de comandos da tarefa de início para instalar automaticamente a aplicação em todos os nós do conjunto. Opcionalmente, instale o pacote quando uma tarefa está agendada para ser executada num nó.

* [Imagem do conjunto personalizado](batch-custom-images.md) - criar uma imagem de VM do Linux ou do Windows personalizada que contém drivers, software, ou outras definições necessárias para o tamanho da VM. 

* [Batch Shipyard](https://github.com/Azure/batch-shipyard) configura automaticamente a GPU e o RDMA para trabalhar transparentemente com cargas de trabalho em contentores no Azure Batch. Batch Shipyard é totalmente controlado por com ficheiros de configuração. Existem muitos receita configurações de exemplo disponíveis que permitem cargas de trabalho GPU e RDMA, tais como o [CNTK GPU receita](https://github.com/Azure/batch-shipyard/tree/master/recipes/CNTK-GPU-OpenMPI) que pré-configura os controladores de GPU em VMs de série N e carrega o software de ferramentas cognitivas da Microsoft como uma imagem do Docker.


## <a name="example-microsoft-mpi-on-an-a8-vm-pool"></a>Exemplo: Microsoft MPI num conjunto de A8 VM

Para executar aplicações MPI do Windows num conjunto de nós de A8 do Azure, terá de instalar uma implementação de MPI suportada. Eis os passos de exemplo para instalar [Microsoft MPI](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx) num Windows do agrupamento com um pacote de aplicação do Batch.

1. Transfira o [pacote de configuração](https://go.microsoft.com/FWLink/p/?LinkID=389556) (MSMpiSetup.exe) para a versão mais recente do Microsoft MPI.
2. Crie um ficheiro zip do pacote.
3. Carregar o pacote para a sua conta do Batch. Para obter os passos, consulte a [pacotes de aplicações](batch-application-packages.md) orientações. Especifique um id de aplicação, como *MSMPI*e uma versão como *8.1*. 
4. Utilizar as APIs do Batch ou o portal do Azure, crie um conjunto na configuração de serviços de nuvem com o número pretendido de nós e dimensionamento. A tabela seguinte mostra exemplos de definições para configurar a MPI no modo autônomo usando uma tarefa de início:

| Definição | Valor |
| ---- | ----- | 
| **Tipo de Imagem** | Serviços Cloud |
| **Família de SO** | Windows Server 2012 R2 (família de SO 4) |
| **Tamanho do nó** | A8 Standard |
| **Comunicação internós ativada** | Verdadeiro |
| **Máx. tarefas por nó** | 1 |
| **Referências do pacote de aplicação** | MSMPI |
| **Iniciar a tarefa ativada** | Verdadeiro<br>**Linha de comandos** - `"cmd /c %AZ_BATCH_APP_PACKAGE_MSMPI#8.1%\\MSMpiSetup.exe -unattend -force"`<br/>**Identidade do usuário** -utilizador automático de agrupamento, o administrador<br/>**Aguardar execução com êxito** – verdadeiro

## <a name="example-nvidia-tesla-drivers-on-nc-vm-pool"></a>Exemplo: Controladores de NVIDIA Tesla no pool de VMS do NC

Para executar aplicativos de CUDA num conjunto de nós do Linux NC, terá de instalar CUDA Toolkit 9.0 em nós. O Kit de ferramentas instala os controladores de NVIDIA Tesla GPU necessários. Seguem-se passos de exemplo para implementar uma imagem de Ubuntu 16.04 LTS personalizada com os controladores GPU:

1. Implemente uma VM da série NC do Azure a executar o Ubuntu 16.04 LTS. Por exemplo, crie a VM na região e.u.a. centro-Sul. Certifique-se de que criar a VM com um disco gerido.
2. Siga os passos para ligar à VM e [instalar controladores CUDA](../virtual-machines/linux/n-series-driver-setup.md).
3. Desaprovisionar o agente do Linux e, em seguida [capturar a imagem de VM do Linux](../virtual-machines/linux/capture-image.md).
4. Crie uma conta do Batch numa região que suporta VMs do NC.
5. Usando as APIs do Batch ou o portal do Azure, criar um conjunto [com a imagem personalizada](batch-custom-images.md) e com o número pretendido de nós e dimensionamento. A tabela seguinte mostra exemplos de definições de agrupamento para a imagem:

| Definição | Valor |
| ---- | ---- |
| **Tipo de Imagem** | Imagem personalizada |
| **Imagem personalizada** | Nome da imagem |
| **SKU do agente de nó** | batch.node.ubuntu 16.04 |
| **Tamanho do nó** | Padrão NC6 |



## <a name="next-steps"></a>Passos Seguintes

* Para executar trabalhos de MPI num conjunto do Batch do Azure, consulte a [Windows](batch-mpi.md) ou [Linux](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/) exemplos.

* Para obter exemplos de cargas de trabalho GPU no Batch, consulte a [Batch Shipyard](https://github.com/Azure/batch-shipyard/) receitas.
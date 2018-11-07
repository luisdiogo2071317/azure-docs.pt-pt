---
title: Configurar um cluster RDMA do Windows para executar aplicações MPI | Documentos da Microsoft
description: Saiba como criar um cluster Windows HPC Pack com tamanho H16r, H16mr, A8 ou A9 VMs para utilizar a rede de RDMA do Azure para executar aplicações MPI.
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,hpc-pack
ms.assetid: 7d9f5bc8-012f-48dd-b290-db81c7592215
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 03/06/2018
ms.author: danlep
ms.openlocfilehash: 52338cc21e46b544c2abb79cd7094615c837a2e8
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51233784"
---
# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-to-run-mpi-applications"></a>Configurar um cluster Windows RDMA com HPC Pack para executar aplicações MPI
Configurar um cluster RDMA do Windows no Azure com o [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) e [tamanhos de VM de HPC com capacidade RDMA](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#rdma-capable-instances) para executar aplicações paralelas de Message Passing Interface (MPI). Ao configurar nós com capacidade RDMA, com base no Windows Server num cluster HPC Pack, aplicações MPI comunicar-se com eficiência através de uma baixa latência, rede de alto débito do Azure que é baseado na tecnologia de acesso (RDMA) de memória direta remoto.

## <a name="hpc-pack-cluster-deployment-options"></a>Opções de implementação de cluster HPC Pack
Microsoft HPC Pack é uma ferramenta fornecido sem custos adicionais para criar o HPC clusters no local ou no Azure para executar o Windows ou Linux HPC aplicações. HPC Pack inclui um ambiente de tempo de execução para a implementação Microsoft de que a mensagem Passing Interface para Windows (MS-MPI). Quando utilizado com as instâncias com capacidade RDMA com um sistema de operativo do Windows Server suportado, o HPC Pack fornece uma opção eficiente para executar aplicações MPI do Windows que acessam a rede de RDMA do Azure. 

Este artigo apresenta dois cenários e ligações para orientações detalhadas para configurar um cluster RDMA do Windows com o Microsoft HPC Pack 2012 R2. 

* Cenário 1. Implementar instâncias de função de trabalho de computação intensiva (PaaS)
* Cenário 2. Implementar nós de computação de VMs de computação intensiva (IaaS)

## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Cenário 1: Implementar instâncias de função de trabalho de computação intensiva (PaaS)
A partir de um cluster HPC Pack existente, adicione recursos de computação extra nas instâncias de função de trabalho do Azure (nós do Azure) em execução num serviço cloud (PaaS). Esta funcionalidade, também chamada de "expansão para o Azure" do HPC Pack, suporta uma variedade de tamanhos para as instâncias de função de trabalho. Ao adicionar os nós do Azure, especifique um dos tamanhos com capacidade RDMA.

Seguem-se as considerações e os passos para expandir com capacidade RDMA para instâncias do Azure existente de um (normalmente, no local) cluster. Utilize os procedimentos semelhantes para adicionar instâncias de função de trabalho para um nó principal HPC Pack implementado na VM do Azure.

> [!NOTE]
> Para obter um tutorial para expansão para o Azure com o HPC Pack, consulte [configurar um cluster híbrido com o HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Tenha em atenção as considerações nas etapas a seguir se aplicam especificamente com capacidade RDMA nós do Azure.
> 
> 

![Expanda no Azure][burst]

### <a name="steps"></a>Passos
1. **Implementar e configurar um nó principal HPC Pack 2012 R2**
   
    Transferir o pacote de instalação do HPC Pack do [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922). Para requisitos e instruções para preparar uma implementação de rajada do Azure, consulte [períodos de rajada de instâncias de trabalho do Azure com o Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).
2. **Configurar um certificado de gestão na subscrição do Azure**
   
    Configure um certificado para proteger a ligação entre o nó principal e o Azure. Para opções e procedimentos, consulte [cenários para configurar o certificado de gestão do Azure para o HPC Pack](https://technet.microsoft.com/library/gg481759.aspx). Para implementações de teste, o HPC Pack instala um padrão Microsoft HPC Azure certificado de gestão pode carregar rapidamente a sua subscrição do Azure.
3. **Criar um novo serviço cloud e uma conta de armazenamento**
   
    Utilize o portal do Azure para criar um serviço em nuvem (clássico) e uma conta de armazenamento (clássico) para a implementação. Crie estes recursos numa região em que o tamanho de série H, A8 ou A9 que pretende utilizar está disponível. Ver [produtos do Azure por região](https://azure.microsoft.com/regions/services/).

4. **Criar um modelo de nó do Azure**
   
    Utilize o Assistente de modelo do nó de criação no Gestor de clusters HPC. Para obter os passos, consulte [criar um modelo de nó do Azure](https://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) no "Passos para implementar o Azure nós com o Microsoft HPC Pack".
   
    Para testes iniciais, sugerimos que configurar uma política de disponibilidade manual no modelo.
5. **Adicionar nós ao cluster**
   
    Utilize o nó Assistente para adicionar no Gestor de clusters HPC. Para obter mais informações, consulte [adicionar nós do Azure para o Cluster do Windows HPC](https://technet.microsoft.com/library/gg481758.aspx#BKMK_Add).
   
    Ao especificar o tamanho de nós, selecione um dos tamanhos de instância com capacidade RDMA.
   
   > [!NOTE]
   > Cada rajada para implementação do Azure com as instâncias de computação intensiva, o HPC Pack implementa automaticamente um mínimo de duas instâncias com capacidade RDMA (por exemplo, A8) como nós de proxy, além das instâncias de função de trabalho do Azure que especificar. Os nós de proxy utilizam núcleos que são alocados à subscrição e incorrer em custos, juntamente com as instâncias de função de trabalho do Azure.
   > 
   > 
6. **Iniciar (aprovisionar) os nós e colocá-los online para executar tarefas**
   
    Selecione os nós e utilize o **iniciar** ação no Gestor de clusters de HPC. Quando o aprovisionamento estiver concluído, selecione os nós e utilizar o **colocar Online** ação no Gestor de clusters de HPC. Os nós estão prontos para executar tarefas.
7. **Submeter tarefas ao cluster**
   
   Utilize ferramentas de submissão de tarefa HPC Pack para executar tarefas de cluster. Ver [pacote HPC da Microsoft: gestão de tarefas](https://technet.microsoft.com/library/jj899585.aspx).
8. **Parar (desaprovisionamento) os nós**
   
   Quando tiver terminado de tarefas em execução, colocar offline os nós e utilizar o **parar** ação no Gestor de clusters de HPC.

## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Cenário 2: Implementar nós de computação em VMs de computação intensiva (IaaS)
Neste cenário, vai implementar o nó principal do HPC Pack e nós de computação de cluster em VMs numa rede virtual do Azure. HPC Pack fornece vários [opções de implementação em VMs do Azure](../../windows/hpcpack-cluster-options.md), incluindo scripts de implantação automatizado e os modelos de início rápido do Azure. Por exemplo, as seguintes considerações e passos guiá-lo para utilizar o [script de implementação do HPC Pack IaaS](hpcpack-cluster-powershell-script.md) para automatizar a implantação de um cluster HPC Pack 2012 R2 no Azure.

![Cluster em VMs do Azure][iaas]

### <a name="steps"></a>Passos
1. **Criar um nó principal do cluster e VMs de nó de computação ao executar o script de implementação do HPC Pack IaaS num computador cliente**
   
    Transferir o pacote de Script de implementação do IaaS HPC Pack a partir da [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).
   
    Para preparar o computador cliente, crie o ficheiro de configuração de script e execute o script, consulte [criar um Cluster HPC com o script de implementação do HPC Pack IaaS](hpcpack-cluster-powershell-script.md). 
   
    Para considerações implementar com capacidade RDMA nós de computação, consulte [tamanhos de VM de computação de alto desempenho](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#rdma-capable-instances) e tenha em atenção o seguinte:
   
   * **Rede virtual**: Especifique uma nova rede virtual numa região em que o tamanho da série H, A8 ou A9 que pretende utilizar está disponível. Ver [produtos do Azure por região](https://azure.microsoft.com/regions/services/).

   * **Sistema operativo Windows Server**: para suportar a conetividade RDMA, especifique um sistema de operativo de servidor Windows compatível, como o Windows Server 2012 R2 para o nó de computação de VMs.
   * **Serviços cloud**: uma vez que o script utiliza o modelo de implementação clássico, as VMs do cluster são implementadas utilizando os serviços cloud do Azure (`ServiceName` definições no ficheiro de configuração). Recomendamos que implemente o seu nó principal num único serviço de nuvem e os nós de computação num serviço cloud diferente. 
   * **Siga o tamanho do nó**: para este cenário, considere um tamanho de, pelo menos, A4 (Extra grande) para o nó principal.
   * **Extensão de HpcVmDrivers**: O script de implementação instala o agente da VM do Azure e a extensão de HpcVmDrivers automaticamente quando implementar nós de computação de A8 ou A9 tamanho com um sistema operativo Windows Server. HpcVmDrivers instala controladores no nó de computação de VMs para que eles podem se conectar à rede RDMA. Em VMs de série H com capacidade RDMA, tem de instalar manualmente a extensão de HpcVmDrivers. Ver [tamanhos de VM de computação de alto desempenho](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
   * **Configuração de rede de cluster**: O script de implementação configura automaticamente o cluster HPC Pack em 5 de topologia (todos os nós na rede empresarial). Esta topologia é necessária para todas as implementações de cluster HPC Pack em VMs. Não altere a topologia de rede de cluster mais tarde.
1. **Colocar online nós de computação para executar tarefas**
   
    Selecione os nós e utilize o **colocar Online** ação no Gestor de clusters de HPC. Os nós estão prontos para executar tarefas.
3. **Submeter tarefas ao cluster**
   
    Ligar ao nó principal para submeter tarefas ou configurar um computador no local para fazer isso. Para obter informações, consulte [submeter tarefas para um HPC cluster do Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
4. **Colocar offline os nós e pare (desaloque)-los**
   
    Quando tiver terminado de tarefas em execução, levar os nós offline no Gestor de clusters de HPC. Em seguida, utilize ferramentas de gestão do Azure para encerrá-las.

## <a name="run-mpi-applications-on-the-cluster"></a>Executar aplicações MPI no cluster
### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Exemplo: Executar mpipingpong num cluster HPC Pack
Para verificar uma implantação do HPC Pack das instâncias com capacidade RDMA, execute o HPC Pack **mpipingpong** comando no cluster. **mpipingpong** envia pacotes de dados entre nós emparelhadas repetidamente para calcular as medições de latência e débito e estatísticas para a rede de aplicação com capacidade RDMA. Este exemplo mostra um padrão típico para executar um trabalho MPI (neste caso, **mpipingpong**) ao utilizar o cluster **mpiexec** comando.

Este exemplo assume que adicionou nós do Azure numa configuração de "explosão para o Azure" ([cenário 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-\(PaaS\) in this article). Se tiver implementado o HPC Pack num cluster de VMs do Azure, terá de modificar a sintaxe de comando para especificar um grupo de nó diferente e definir variáveis de ambiente adicionais para direcionar o tráfego de rede para a rede RDMA.

Para executar mpipingpong no cluster:

1. No nó principal ou num computador cliente configurado corretamente, abra um Prompt de comando.
2. Para calcular a latência entre pares de nós numa implementação do Azure de rajada de quatro nós, escreva o seguinte comando para submeter uma tarefa para executar mpipingpong com um tamanho de pacote pequeno e diversas iterações:
   
    ```Command
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```
   
    O comando devolve o ID da tarefa que é submetido.
   
    Se tiver implementado o cluster HPC Pack implementado em VMs do Azure, especifique um grupo do nó que contém as VMs de nó implementadas num serviço cloud único de computação e modificar os **mpiexec** comando da seguinte forma:
   
    ```Command
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```
3. Quando a tarefa estiver concluída, para ver a saída (no caso, a saída de tarefa 1 da tarefa), escreva o seguinte
   
    ```Command
    task view <JobID>.1
    ```
   
    em que &lt; *JobID* &gt; é o ID da tarefa que foi submetido.
   
    A saída inclui resultados de latência semelhantes ao seguinte.
   
    ![Latência de pongue de ping][pingpong1]
4. Para calcular a taxa de transferência entre pares de nós de rajada do Azure, escreva o seguinte comando para submeter uma tarefa para executar **mpipingpong** com um tamanho de pacotes grandes e algumas iterações:
   
    ```Command
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```
   
    O comando devolve o ID da tarefa que é submetido.
   
    Num cluster HPC Pack implementado em VMs do Azure, modifique o comando conforme indicado no passo 2.
5. Quando a tarefa estiver concluída, para ver o resultado (no caso, a saída de tarefa 1 da tarefa), escreva o seguinte:
   
    ```Command
    task view <JobID>.1
    ```
   
   A saída inclui resultados de débito semelhantes ao seguinte.
   
   ![Débito do pongue de ping][pingpong2]

### <a name="mpi-application-considerations"></a>Considerações de aplicações MPI
Seguem-se considerações para executar aplicações MPI com o HPC Pack no Azure. Alguns aplicam-se apenas às implementações de nós do Azure (instâncias de função de trabalho adicionadas numa configuração de "explosão para o Azure").

* Instâncias de função de trabalho num serviço em nuvem são periodicamente reaprovisionadas sem aviso prévio pelo Azure (por exemplo, para manutenção do sistema ou no caso de que falha de uma instância). Se uma instância é reaprovisionada enquanto está em execução uma tarefa MPI, a instância perde os seus dados e volta para o estado quando foi primeiro implementada, que pode causar a falha da tarefa MPI. Os mais nós que utilizar para uma única tarefa MPI e mais tempo a tarefa é executada, quanto mais provável que uma das instâncias é reaprovisionada enquanto uma tarefa está em execução. Também considere o seguinte se designar um único nó na implementação como um servidor de ficheiros.
* Para executar trabalhos de MPI no Azure, não precisa usar as instâncias com capacidade RDMA. Pode usar qualquer tamanho de instância que seja suportado pelo HPC Pack. No entanto, as instâncias com capacidade RDMA são recomendadas para a execução de trabalhos de MPI relativamente em grande escala que diferenciam-se de que a latência e a largura de banda da rede que liga os nós. Se utilizar outros tamanhos para executar trabalhos de MPI e largura de banda-sensíveis à latência, recomendamos a execução de trabalhos pequenos, em que uma única tarefa é executada em apenas alguns nós.
* Aplicações implementadas para instâncias do Azure estão sujeitas a termos de licenciamento associados à aplicação. Contacte o fornecedor de qualquer aplicação comercial para restrições de licenciamento, ou outras para execução na cloud. Nem todos os fornecedores oferecem licenciamento pay as you go.
* As instâncias do Azure tem uma configuração adicional para nós no local de acesso, partilhas e servidores de licença. Por exemplo, para permitir que os nós do Azure aceder a um servidor de licenças no local, pode configurar uma rede virtual do Azure do site a site.
* Para executar aplicações MPI em instâncias do Azure, registar cada aplicativo de MPI no Firewall do Windows nas instâncias de ao executar o **hpcfwutil** comando. Isso permite comunicações de MPI seja realizada numa porta que é atribuída dinamicamente pelo firewall.
  
  > [!NOTE]
  > Para rajada para implementações do Azure, também pode configurar um comando de exceção de firewall para ser executado automaticamente em todos os nós do Azure novo que são adicionados ao seu cluster. Depois de executar o **hpcfwutil** de comando e certifique-se de que seu aplicativo funciona, adicione o comando ao script de inicialização para os nós do Azure. Para obter mais informações, consulte [utilizar um Script de inicialização de nós de Azure](https://technet.microsoft.com/library/jj899632.aspx).
  > 
  > 
* HPC Pack utiliza a variável de ambiente de cluster CCP_MPI_NETMASK para especificar um intervalo de endereços aceitáveis para a comunicação de MPI. A partir do HPC Pack 2012 R2, a variável de ambiente de cluster CCP_MPI_NETMASK afeta apenas a comunicação de MPI entre nós de computação de cluster associado a um domínio (no local ou em VMs do Azure). A variável é ignorada por nós adicionados de rajada para configuração do Azure.
* Trabalhos de MPI não é possível executar em várias instâncias do Azure que são implementadas nos serviços de cloud diferentes (por exemplo, em rajada para implementações do Azure com modelos de nó diferente, ou nós de computação de VM do Azure implementadas em vários serviços cloud). Se tiver várias implementações de nó do Azure que são iniciadas com modelos de nó diferente, tem de executar a tarefa MPI em apenas um conjunto de nós do Azure.
* Quando adiciona nós do Azure ao seu cluster e colocá-los online, o serviço de agendador de tarefas HPC tenta imediatamente iniciar tarefas em nós. Se apenas uma parte da sua carga de trabalho pode executar no Azure, certifique-se de que atualizar ou criar modelos de trabalho para definir o que os tipos de tarefas podem executar no Azure. Por exemplo, para garantir que as tarefas submetidas com um modelo de tarefa só são executados em nós do Azure, adicione a propriedade de grupos de nó para o modelo de tarefa e selecione AzureNodes como o valor necessário. Para criar grupos personalizados para os nós do Azure, utilize o cmdlet Add-HpcGroup HPC PowerShell.

## <a name="next-steps"></a>Passos Seguintes
* Como alternativa à utilização do HPC Pack, desenvolva com o serviço Azure Batch para executar aplicações MPI em agrupamentos geridos de nós de computação no Azure. Ver [utilizar tarefas de várias instâncias para executar aplicações Message Passing Interface (MPI) no Azure Batch](../../../batch/batch-mpi.md).

<!--Image references-->
[burst]:media/hpcpack-rdma-cluster/burst.png
[iaas]:media/hpcpack-rdma-cluster/iaas.png
[pingpong1]:media/hpcpack-rdma-cluster/pingpong1.png
[pingpong2]:media/hpcpack-rdma-cluster/pingpong2.png

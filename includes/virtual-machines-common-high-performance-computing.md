---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-linux, virtual-machines-windows
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 07/02/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 88aeaf1246583345d1d28218e8e91b3ecbcd166b
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53430020"
---
As organizações têm necessidades de computação em grande escala. Estas cargas de trabalho de Macrocomputação incluem o design de engenharia e análise, cálculos de riscos financeiros, composição de imagem, modelação complexa, simulações Monte Carlo e muito mais. 

Utilize a cloud do Azure para executar com eficiência de computação intensiva Linux e Windows cargas de trabalho, de tarefas de lote paralela para simulações de HPC tradicionais. Execute o HPC e cargas de trabalho do batch na infraestrutura do Azure, com a sua escolha de computação serviços, gerentes de grade, soluções do Marketplace e as aplicações alojadas para fornecedores de (SaaS). O Azure fornece soluções flexíveis para distribuir trabalho e Dimensionar para milhares de VMs ou núcleos e, em seguida, reduzir verticalmente quando necessitar de menos recursos. 



## <a name="solution-options"></a>Opções de solução



* **Soluções do tipo faça mesmo**
    * Configurar o seu próprio ambiente de cluster em máquinas virtuais do Azure ou [conjuntos de dimensionamento de máquinas virtuais](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). 
    * Comparação de precisão e mudar de um cluster no local ou implementar um novo cluster no Azure para uma maior capacidade. 
    * Utilizar modelos Azure Resource Manager para implementar líderes [gestores de carga de trabalho](#workload-managers), infraestrutura, e [aplicativos](#hpc-applications). 
    * Escolher [tamanhos de VM de GPU e de HPC](#hpc-and-gpu-sizes) que incluem ligações de hardware e de rede especializadas para cargas de trabalho MPI ou GPU. 
    * Adicione [armazenamento de elevado desempenho](#hpc-storage) para cargas de trabalho de e/S intensivas.
* **Soluções híbridas**
    * Expandir a sua solução no local para a descarga de cargas de trabalho de pico de "("explosão) para a infraestrutura do Azure
    * Utilizar o cloud computação a pedido com já existentes [Gestor da carga de trabalho](#workload-manager).
    * Aproveitar [tamanhos de VM de GPU e de HPC](#hpc-and-gpu-sizes) para cargas de trabalho MPI ou GPU.
* **Soluções de computação grandes como um serviço**
    * Desenvolver soluções de Macrocomputação personalizadas e fluxos de trabalho usando [do Azure Batch](#azure-batch) e relacionados [serviços do Azure](#related-azure-services).
    * Executar soluções de engenharia e simulação habilitados no Azure de fornecedores, incluindo [Altair](http://www.altair.com/), [Rescale](https://www.rescale.com/azure/), e [Cycle Computing](https://cyclecomputing.com/) (agora [associado Microsoft](https://blogs.microsoft.com/blog/2017/08/15/microsoft-acquires-cycle-computing-accelerate-big-computing-cloud/)).
    * Utilize um [supercomputador Cray](https://www.cray.com/solutions/supercomputing-as-a-service/cray-in-azure) como um serviço alojado no Azure.
* **Soluções do Marketplace**
    * Utilize a escala do [aplicações HPC](#hpc-applications) e [soluções](#marketplace-solutions) oferecidos no [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/). 
    


As secções seguintes fornecem mais informações sobre as tecnologias de suporte e ligações para documentação de orientação.



## <a name="marketplace-solutions"></a>Soluções do Marketplace

Visite o [do Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/) para imagens de VM do Windows e do Linux e soluções projetadas para HPC. Os exemplos incluem:

* [HPC de baseada em CentOS RogueWave](https://azuremarketplace.microsoft.com/marketplace/apps/RogueWave.CentOSbased73HPC?tab=Overview)
* [SUSE Linux Enterprise Server para HPC](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)
*  [Motor de servidor de grade TIBCO](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/tibco-software.gridserverlinuxengine?tab=Overview)
* [VM para Windows e Linux de ciência de dados do Azure](../articles/machine-learning/machine-learning-data-science-virtual-machine-overview.md)
* [d3View](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/xfinityinc.d3view-v5?tab=Overview)
* [UberCloud](https://azure.microsoft.com/search/marketplace/?q=ubercloud)
* [Intel Cloud Edition para Lustre](https://azuremarketplace.microsoft.com/marketplace/apps/intel.intel-cloud-edition-gs)


 
## <a name="hpc-applications"></a>Aplicações de HPC

Execute aplicações de HPC comerciais ou personalizadas no Azure. Vários exemplos nesta secção são benchmarked para dimensionar de forma eficiente com VMs adicionais ou núcleos de computação. Visite o [do Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace) para preparado para implementar soluções.

> [!NOTE]
> Contacte o fornecedor de qualquer aplicação comercial para restrições de licenciamento, ou outras para execução na cloud. Nem todos os fornecedores oferecem licenciamento pay as you go. Poderá precisar de um servidor de licenciamento na cloud para a sua solução ou ligar a um servidor de licenças no local.

### <a name="engineering-applications"></a>Aplicativos de engenharia


* [Altair RADIOSS](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/)
* [ANSYS CFD](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)
* [MATLAB distribuídas de computação de servidor](../articles/virtual-machines/windows/matlab-mdcs-cluster.md)
* [StarCCM +](https://blogs.msdn.microsoft.com/azurecat/2017/07/07/run-star-ccm-in-an-azure-hpc-cluster/)
* [OpenFOAM](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)



### <a name="graphics-and-rendering"></a>Gráficos e composição

* [Autodesk Maya, 3ds Max e Arnold](../articles/batch/batch-rendering-service.md) sobre o Azure Batch 

### <a name="ai-and-deep-learning"></a>IA e aprendizagem aprofundada

* [Microsoft Cognitive Toolkit](https://docs.microsoft.com/cognitive-toolkit/cntk-on-azure)
* [VM de aprendizagem profunda](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning)
* [Receitas de Shipyard de batch para aprendizagem profunda](https://github.com/Azure/batch-shipyard/tree/master/recipes#deeplearning)






## <a name="hpc-and-gpu-vm-sizes"></a>Tamanhos de VM de GPU e de HPC
O Azure oferece uma variedade de tamanhos para [Linux](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) e [Windows](../articles/virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) VMs, incluindo tamanhos concebidos para cargas de trabalho de computação intensiva. Por exemplo, H16r e H16mr VMs podem ligar a uma rede RDMA de back-end de alto débito. Esta rede de nuvem pode melhorar o desempenho dos aplicativos em paralelo rigidamente acoplados a ser executado [Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) ou do Intel MPI. 

VMs de série N apresentam GPUs NVIDIA concebido para aplicações intensivas de computação ou grande intensidade de gráficos, incluindo a aprendizagem de inteligência artificial (IA) e visualização. 

Saiba mais:

* Tamanhos de computação de alto desempenho [Linux](../articles/virtual-machines/linux/sizes-hpc.md) e [Windows](../articles/virtual-machines/windows/sizes-hpc.md) VMs 
* Tamanhos ativadas para GPU [Linux](../articles/virtual-machines/linux/sizes-gpu.md) e [Windows](../articles/virtual-machines/windows/sizes-gpu.md) VMs 

Aprenda a:

* [Configurar um cluster RDMA do Linux para executar aplicações MPI](../articles/virtual-machines/linux/classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Configurar um cluster RDMA do Windows com o Microsoft HPC Pack para executar aplicações MPI](../articles/virtual-machines/windows/classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Utilizar VMs de computação intensiva em conjuntos do Batch](../articles/batch/batch-pool-compute-intensive-sizes.md)



## <a name="azure-batch"></a>Azure Batch
[Batch](../articles/batch/batch-technical-overview.md) é uma plataforma de serviço para a execução de paralelas em grande escala e de alto desempenho aplicações de computação (HPC) com eficiência na cloud. O Azure Batch agenda computação intensiva de trabalho para serem executadas num conjunto gerido de máquinas virtuais e pode automaticamente Dimensionar recursos de computação para satisfazer as necessidades das suas tarefas. 

Fornecedores SaaS ou os programadores podem utilizar os SDKs do Batch e as ferramentas para integrar aplicações HPC ou cargas de trabalho de contentor com o Azure, estagiar dados para o Azure e criar pipelines de execução de tarefa. 

Aprenda a:

* [Comece a desenvolver com o Batch](../articles/batch/quick-run-dotnet.md)
* [Utilize exemplos de código do Azure Batch](https://github.com/Azure/azure-batch-samples)
* [Utilizar VMs de baixa prioridade com o Batch](../articles/batch/batch-low-pri-vms.md)
* [Executar cargas de trabalho HPC em contentores com o Batch Shipyard](https://github.com/Azure/batch-shipyard)
* [Executar cargas de trabalho paralelas do R no Batch](https://github.com/Azure/doAzureParallel)
* [Execute tarefas de Spark a pedido em Batch](https://github.com/Azure/aztk)

## <a name="workload-managers"></a>Gestores de carga de trabalho

Seguem-se exemplos de gestores de cluster e a carga de trabalho que podem ser executadas na infraestrutura do Azure. Crie clusters autónomos em VMs do Azure ou expandidas para VMs do Azure a partir de um cluster no local. 
* [Computação de voo alces](https://azuremarketplace.microsoft.com/marketplace/apps/alces-flight-limited.alces-flight-compute-solo?tab=Overview)
* [TIBCO DataSynapse GridServer](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/) 
* [Gestor de clusters de brilhante](http://www.brightcomputing.com/technology-partners/microsoft)
* [IBM espectro Symphony and Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/)
* [PBS Pro](http://pbspro.org)
* [Microsoft HPC Pack](https://technet.microsoft.com/library/mt744885.aspx) -veja as opções para executar no [Windows](../articles/virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) e [Linux](../articles/virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) VMs 



## <a name="hpc-storage"></a>Armazenamento HPC

Cargas de trabalho do Batch em grande escala e HPC têm à demanda por acesso e armazenamento de dados que exceda as capacidades dos sistemas de ficheiros de cloud tradicional. Implementar as soluções de sistema de ficheiros paralelos no Azure, tal como [Lustre](http://lustre.org/) e [BeeGFS](http://www.beegfs.com/content/).

Saiba mais:

* [Sistemas de arquivos virtual no Azure em paralelo](https://azure.microsoft.com/resources/parallel-virtual-file-systems-on-microsoft-azure/)
* Soluções de armazenamento de cloud de elevado desempenho partir [Avere](http://www.averesystems.com/about-us/about-avere) (agora [associado com a Microsoft](https://blogs.microsoft.com/blog/2018/01/03/microsoft-to-acquire-avere-systems-accelerating-high-performance-computing-innovation-for-media-and-entertainment-industry-and-beyond/))


## <a name="related-azure-services"></a>Serviços relacionados do Azure

Máquinas virtuais do Azure, conjuntos de dimensionamento de máquinas virtuais, Batch e serviços de computação relacionados são a base da maioria das soluções de HPC do Azure. No entanto, sua solução pode tirar partido de vários serviços do Azure relacionados. Eis uma lista parcial:

### <a name="storage"></a>Armazenamento

* [BLOB, tabela e armazenamento de filas](../articles/storage/storage-introduction.md)
* [Armazenamento de ficheiros](../articles/storage/storage-files-introduction.md)

### <a name="data-and-analytics"></a>Dados e análises
* [HDInsight](../articles/hdinsight/hadoop/apache-hadoop-introduction.md)
* [Data Factory](../articles/data-factory/introduction.md)
* [Geração 1 do Data Lake Storage](../articles/data-lake-store/data-lake-store-overview.md)
* [Databricks](../articles/azure-databricks/what-is-azure-databricks.md)
* [Base de Dados SQL](../articles/sql-database/sql-database-technical-overview.md)

### <a name="ai-and-machine-learning"></a>IA e aprendizagem automática
* [Serviços de Machine Learning](../articles/machine-learning/service/overview-what-is-azure-ml.md)
* [Genomics](../articles/genomics/overview-what-is-genomics.md)

### <a name="networking"></a>Redes
* [Rede Virtual](../articles/virtual-network/virtual-networks-overview.md)
* [ExpressRoute](../articles/expressroute/expressroute-introduction.md)

### <a name="containers"></a>Contentores
* [Serviço de Contentor](../articles/container-service/dcos-swarm/container-service-intro.md)
* [Serviço Kubernetes do Azure (AKS)](../articles/aks/intro-kubernetes.md)
* [Container Registry](../articles/container-registry/container-registry-intro.md)



## <a name="customer-stories"></a>Histórias dos clientes

Exemplos de clientes que tem resolvido os problemas de negócios com soluções HPC do Azure:

* [ANEO](https://customers.microsoft.com/story/it-provider-finds-highly-scalable-cloud-based-hpc-redu) 
* [AXA Global P&C](https://customers.microsoft.com/story/axa-global-p-and-c)
* [Axioma](https://customers.microsoft.com/story/axioma-delivers-fintechs-first-born-in-the-cloud-multi-asset-class-enterprise-risk-solution)
* [d3View](https://customers.microsoft.com/story/big-data-solution-provider-adopts-new-cloud-gains-thou)
* [EFS](https://customers.microsoft.com/story/efs-professionalservices-azure)
* [Hymans Robertson](https://customers.microsoft.com/story/hymans-robertson)
* [MetLife](https://enterprise.microsoft.com/en-us/customer-story/industries/insurance/metlife/)
* [Pesquisa da Microsoft](https://customers.microsoft.com/doclink/fast-lmm-and-windows-azure-put-genetics-research-on-fa)
* [Milliman](https://customers.microsoft.com/story/actuarial-firm-works-to-transform-insurance-industry-w)
* [Mitsubishi UFJ internacional de valores mobiliários](https://customers.microsoft.com/story/powering-risk-compute-grids-in-the-cloud)
* [NeuroInitiative](https://customers.microsoft.com/en-us/story/neuroinitiative-health-provider-azure)
* [Schlumberger](https://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
* [Towers Watson](https://customers.microsoft.com/story/insurance-tech-provider-delivers-disruptive-solutions)


## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre as soluções de Macrocomputação para [simulação de engenharia](https://simulation.azure.com/), [composição](https://azure.microsoft.com/solutions/big-compute/rendering/), [banca e mercados de capitais](https://finance.azure.com/), e [genomics](https://enterprise.microsoft.com/en-us/industries/health/genomics/) .
* Para os anúncios mais recentes, veja o [blogue da equipa do Microsoft HPC e Batch](http://blogs.technet.com/b/windowshpc/) e o [blogue do Azure](https://azure.microsoft.com/blog/tag/hpc/).

* Utilizar o Azure gerido e dimensionável [Batch](https://azure.microsoft.com/services/batch/) serviço para executar cargas de trabalho de computação intensiva, sem ter de gerir a infraestrutura subjacente [Saiba mais](https://azure.microsoft.com/solutions/architecture/hpc-big-compute-saas/)




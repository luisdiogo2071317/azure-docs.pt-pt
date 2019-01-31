---
title: Planear o seu sistema de vFXT Avere - Azure
description: Explica o planeamento para o fazer antes de implementar Avere vFXT para o Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: e60c92c22382112558307062afdeb87e08075765
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55298930"
---
# <a name="plan-your-avere-vfxt-system"></a>Planear o seu sistema Avere vFXT

Este artigo explica como planejar um vFXT Avere novo para o cluster do Azure para se certificar-se de que o cluster que cria é posicionado e o tamanho adequado para as suas necessidades. 

Antes de passar no Azure Marketplace ou criar todas as VMs, considere a forma do cluster irá interagir com outros elementos no Azure. Planear onde estarão localizados na sua rede privada e sub-redes recursos do cluster e decidir onde será seu armazenamento de back-end. Certifique-se de que os nós do cluster que cria poderosos o suficiente para suportar o fluxo de trabalho. 

Continue a ler para obter mais informações.

## <a name="resource-group-and-network-infrastructure"></a>Infraestrutura de rede e de grupo de recursos

Considere onde será os elementos de seu vFXT Avere para implementação do Azure. O diagrama abaixo mostra uma esquema possível para o vFXT Avere para componentes do Azure:

![Diagrama que mostra o cluster de VMs de controlador e cluster dentro de uma sub-rede. Em torno da sub-rede o limite é um limite de vnet. Dentro da vnet é um Hexágono que representa o ponto de extremidade do serviço de armazenamento; ele é conectado com uma seta tracejada para um armazenamento de BLOBs fora da vnet.](media/avere-vfxt-components-option.png)

Siga estas diretrizes quando planear a infraestrutura de rede Avere vFXT do seu sistema:

* Todos os elementos devem ser geridos com uma nova subscrição criada para a implementação de vFXT Avere. As vantagens incluem: 
  * Controlo custo mais simples - modo de exibição e ciclos de todos os custos de recursos, a infraestrutura e a computação numa subscrição de auditoria.
  * Limpeza mais fácil - pode remover a subscrição completa quando terminar com o projeto.
  * Criação de partições conveniente de recurso quotas - proteger outras cargas de trabalho críticas de limitação de recursos possível quando o grande número de clientes utilizado para seu fluxo de trabalho de computação de alto desempenho, isolando os clientes de vFXT Avere a visualização e de cluster numa subscrição individual.

* Localize os sistemas de computação de clientes próximo vFXT cluster. Armazenamento de back-end pode ser mais remoto.  

* Para simplificar, localize o cluster de vFXT e o controlador de cluster VM na mesma rede virtual (vnet) e no mesmo grupo de recursos. Também devem utilizar a mesma conta de armazenamento. (O controlador de cluster cria o cluster e também pode ser utilizado para gestão de linha de comandos cluster.)  

* O cluster tem de estar localizado na sua própria sub-rede para evitar conflitos de endereços IP com clientes ou os recursos de computação. 

## <a name="ip-address-requirements"></a>Requisitos de endereços IP 

Certifique-se de que a sub-rede do seu cluster tem um intervalo de endereços IP grande o suficiente para suportar o cluster. 

O cluster de vFXT Avere utiliza os seguintes endereços IP:

* Endereço IP de gestão de um cluster. Este endereço pode mover a partir do nó para nó do cluster, mas está sempre disponível para que possa ligar para a ferramenta de configuração do painel de controlo de Avere.
* Para cada nó de cluster:
  * Pelo menos um endereço IP com clientes. (Todos os endereços com clientes são geridos do cluster *vserver*, que pode movê-los entre nós, conforme necessário.)
  * Um endereço IP para comunicação de cluster
  * Endereço IP de uma instância (atribuído à VM)

Se utilizar o armazenamento de Blobs do Azure, também poderá precisar de endereços IP a partir de vnet do seu cluster:  

* Uma conta de armazenamento de Blobs do Azure requer, pelo menos, cinco endereços IP. Este requisito não se esqueça se localizar o armazenamento de BLOBs na mesma vnet como o seu cluster.
* Se utilizar o armazenamento de Blobs do Azure que está fora da rede virtual para o seu cluster, deve criar um ponto de extremidade de serviço de armazenamento dentro da vnet. Este ponto final não utiliza um endereço IP.

Tem a opção para localizar recursos de rede e armazenamento de BLOBs (se utilizado) em diferentes grupos de recursos do cluster.

## <a name="vfxt-node-sizes"></a>tamanhos de nó vFXT 

As VMs que servem como nós de cluster determinam a capacidade de débito e o armazenamento de pedido da cache. Pode escolher entre dois tipos de instância, com memória diferente, o processador e características de armazenamento local. 

Cada nó de vFXT será idêntico. Ou seja, se criar um cluster de três nós terá três VMs do mesmo tipo e tamanho. 

| Tipo de instância | vCPUs | Memória  | Armazenamento SSD local  | Discos de dados máximos | Débito de disco eliminadas do cache | NIC (contagem) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D16s_v3 | 16  | 64 GiB  | 128 GiB  | 32 | 25,600 IOPS <br/> 384 MBps | MBps de 8.000 (8) |
| Standard_E32s_v3 | 32  | 256 GiB | 512 GiB  | 32 | 51,200 IOPS <br/> 768 MBps | MBps de 16 000 (8)  |

Cache de disco por nó é configurável e pode rage de 1000 GB a 8000 GB. 1 TB por nó é o tamanho da cache recomendada para nós de Standard_D16s_v3 e 4 TB por nó é recomendada para Standard_E32s_v3 nós.

Para obter mais informações sobre estas VMs, leia os seguintes documentos do Microsoft Azure:

* [Tamanhos de máquinas virtuais de fins gerais](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general)
* [Tamanhos de máquinas virtuais com otimização de memória](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)

## <a name="account-quota"></a>Quota de conta

Certifique-se de que a sua subscrição tem a capacidade para executar o cluster de vFXT Avere, bem como todos os sistemas de computação ou o cliente a ser utilizados. Leia [Quota para o cluster vFXT](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster) para obter detalhes.

## <a name="back-end-data-storage"></a>Armazenamento de dados back-end

Onde deve o cluster de vFXT Avere armazenar os dados quando não está no cache? Decida se o conjunto de trabalho será armazenado os longo prazo num novo contentor de BLOBs ou num sistema de armazenamento de hardware ou numa cloud existente. 

Se pretender utilizar o armazenamento de Blobs do Azure para o back-end, deve criar um novo contentor como parte da criação do cluster vFXT. Esta opção cria e configura o novo contentor, para que fique pronta a utilizar assim que o cluster estiver pronto. 

Leia [criar o vFXT Avere para o Azure](avere-vfxt-deploy.md#create-the-avere-vfxt-for-azure) para obter detalhes.

> [!NOTE]
> Apenas contentores de armazenamento de BLOBs vazias podem ser utilizados como se filtram de núcleo para o sistema de vFXT Avere. O vFXT tem de ser capaz de gerir o seu arquivo de objetos sem a necessidade de preservar dados existentes. 
>
> Leia [mover dados para o cluster vFXT](avere-vfxt-data-ingest.md) para saber como copiar dados para o contentor do cluster de novo com eficiência com computadores cliente e a cache de vFXT Avere.

Se pretender utilizar um sistema de armazenamento no local existente, deve adicioná-lo para o cluster vFXT depois de criado. Leia [configurar o armazenamento](avere-vfxt-add-storage.md) para obter instruções detalhadas sobre como adicionar um sistema de armazenamento existente para o cluster de vFXT Avere.

## <a name="cluster-access"></a>Acesso de cluster 

O vFXT Avere para cluster do Azure está localizado numa sub-rede privada e o cluster não tem um endereço IP público. Tem de ter algum método de acessar a sub-rede privada para a administração de cluster e ligações de cliente. 

As opções de acesso incluem:

* Saltar anfitrião - atribuir um endereço IP público a uma VM separada dentro da rede privada e utilizá-lo para criar um túnel SSL para os nós do cluster. 

  > [!TIP]
  > Se definir um endereço IP público no controlador de cluster, pode utilizá-lo como o host de atalhos. Leia [controlador de Cluster como ir anfitrião](#cluster-controller-as-jump-host) para obter mais informações.

* Rede privada virtual (VPN) – configurar uma VPN ponto a site ou site a site para a sua rede privada.

* O Azure ExpressRoute - configurar uma ligação privada através de e o parceiro do ExpressRoute. 

Para obter detalhes sobre estas opções, leia os [documentação de rede Virtual do Azure sobre a comunicação de internet](../virtual-network/virtual-networks-overview.md#communicate-with-the-internet).

### <a name="cluster-controller-as-jump-host"></a>Controlador de cluster como rápida ao anfitrião

Se definir um endereço IP público no controlador de cluster, pode utilizá-lo como um anfitrião de atalhos para contactar o cluster de vFXT Avere de fora da sub-rede privada. No entanto, uma vez que o controlador tem privilégios de acesso para modificar nós de cluster, esta ação cria um risco de segurança pequeno.  

Para obter mais segurança com um endereço IP público, utilize um grupo de segurança de rede para permitir o acesso de entrada apenas através da porta 22.

Ao criar o cluster, pode escolher se deve ou não criar um endereço IP público no controlador de cluster. 

* Se criar uma nova vnet ou de uma nova sub-rede, o controlador de cluster será atribuído um endereço IP público.
* Se selecionar uma vnet existente e uma sub-rede, o controlador de cluster terão apenas endereços IP privados. 

## <a name="next-step-understand-the-deployment-process"></a>Passo seguinte: Compreender o processo de implantação

[Descrição geral da implementação](avere-vfxt-deploy-overview.md) dá uma visão geral de todos os passos necessários para criar um vFXT Avere para o sistema do Azure e prepará-lo servir dados.  
---
title: Criar clusters do Service Fabric no Windows Server e Linux | Documentos da Microsoft
description: Clusters do Service Fabric executados no Windows Server e Linux, que significa que será capaz de implantar e alojar as aplicações do Service Fabric em qualquer lugar, pode executar Windows Server ou Linux.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/28/2018
ms.author: dekapur
ms.openlocfilehash: 80b331d32fe1e7bb4eb331bd981106968bc73bed
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44163214"
---
# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Criar clusters do Service Fabric no Windows Server ou Linux
Um cluster do Service Fabric é um conjunto ligado à rede de máquinas virtuais ou físicas, no qual os microsserviços são implementados e geridos. Uma máquina ou VM que faça parte de um cluster é chamado de um nó de cluster. Clusters podem ser dimensionados para milhares de nós. Se adicionar novos nós ao cluster, o Service Fabric rebalances as réplicas de partição de serviço e as instâncias em aumento do número de nós. Em geral melhora o desempenho do aplicativo e reduz a contenção de acesso à memória. Se os nós do cluster não estão a ser utilizados com eficiência, pode diminuir o número de nós do cluster. Service Fabric rebalances novamente as instâncias e réplicas de partição entre o menor número de nós para garantir uma melhor utilização do hardware em cada nó.

Service Fabric permite a criação de clusters do Service Fabric em quaisquer VMs ou computadores que executam o Windows Server ou Linux. Isso significa que pode implementar e executar aplicações do Service Fabric em qualquer ambiente em que tem um conjunto de computadores Windows Server ou Linux que estão interligados, seja no local, Microsoft Azure ou com qualquer fornecedor de cloud.

## <a name="create-service-fabric-clusters-on-azure"></a>Criar clusters do Service Fabric no Azure
A criação de um cluster no Azure é feita através de um modelo de modelo de recursos ou o [portal do Azure](https://portal.azure.com). Leia [criar um cluster do Service Fabric com um modelo do Resource Manager](service-fabric-cluster-creation-via-arm.md) ou [criar um cluster do Service Fabric a partir do portal do Azure](service-fabric-cluster-creation-via-portal.md) para obter mais informações.

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Sistemas operativos suportados para clusters no Azure
É possível criar clusters de máquinas virtuais que executam esses sistemas operacionais:

* Windows Server 2012 R2
* Windows Server 2016 
* 1709 do Windows Server
* Linux Ubuntu 16.04

> [!NOTE]
> Se optar por implementar o Service Fabric no Windows Server 1709, tenha em atenção que (1) não é do long term servicing branch, pelo que poderá ter de mover as versões no futuro, e (2) se implementa contentores, contentores criados no Windows Server 2016 não funcionam no Windows Server  1709 e vice-versa (terá de recriá-los para implementá-las).
>

## <a name="create-service-fabric-standalone-clusters-on-premises-or-with-any-cloud-provider"></a>Criar autónoma do Service Fabric clusters no local ou com qualquer fornecedor de cloud
O Service Fabric fornece um pacote de instalação para a criação de autónomo Service Fabric clusters no local ou em qualquer fornecedor de cloud.

Para obter mais informações sobre como configurar autónomos do Service Fabric clusters no Windows Server, leia [criação do cluster do Service Fabric para o Windows Server](service-fabric-cluster-creation-for-windows-server.md)

  > [!NOTE]
  > Atualmente não suporta clusters autónomos para Linux. Linux é suportado no one box para desenvolvimento e clusters de várias máquinas do Linux do Azure.
  >

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a>Quaisquer implementações de cloud vs. implementações no local
O processo para criar um cluster do Service Fabric no local é semelhante ao processo de criação de um cluster em qualquer cloud à sua escolha, com um conjunto de VMs. As etapas iniciais para aprovisionar as VMs são regidas pelo fornecedor de cloud ou no ambiente no local que está a utilizar. Depois de ter um conjunto de VMs com a conectividade de rede ativada entre eles, em seguida, os passos para configurar o pacote de recursos de infraestrutura do serviço, edite as definições do cluster e execute a criação de cluster e scripts de gestão são idênticas. Isto garante que seu conhecimento e experiência de operando e gerenciando os clusters do Service Fabric é transferível quando opta por novos ambientes de alojamento de destino.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>Benefícios da criação de clusters do Service Fabric autónomo
* É livre para escolher qualquer fornecedor de cloud para alojar o seu cluster.
* Aplicações do Service Fabric, uma vez escritas, podem ser executadas em vários ambientes de alojamento com o mínimo para sem alterações.
* Dados de conhecimento da criação de aplicações do Service Fabric transporta ao longo de um ambiente de alojamento para outro.
* Experiência operacional de execução e gestão do Service Fabric clusters executa diversas através de um ambiente para outro.
* Base de clientes ampla é não vinculado por restrições de ambiente de alojamento.
* Uma camada extra de confiabilidade e a proteção contra falhas ampla existe porque pode mover os serviços ao longo para outro ambiente de implantação se um fornecedor de center ou na cloud de dados tem uma indisponibilidade.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Sistemas operativos suportados para clusters autónomos
É possível criar clusters em VMs ou computadores que executam esses sistemas operacionais (Linux ainda não é suportado):

* Windows Server 2012 R2
* Windows Server 2016 

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Vantagens de clusters do Service Fabric no Azure através de autónomos, que clusters do Service Fabric criadas no local
Executar clusters do Service Fabric no Azure fornece a opção vantagens em relação no local, por isso, se não tiver necessidades específicas de onde executar seus clusters, em seguida, sugerimos que executá-los no Azure. No Azure, podemos fornecer integração com outros serviços, o que torna as operações e gerenciamento de cluster mais fácil e mais confiável e funcionalidades do Azure.

* **Portal do Azure:** portal do Azure torna mais fácil criar e gerir clusters.
* **O Azure Resource Manager:** utilização do Gestor de recursos do Azure permite a gestão fácil de todos os recursos utilizados pelo cluster, como uma unidade e simplifica o controlo de custos e a faturação.
* **Cluster do Service Fabric como um recurso do Azure** cluster A Service Fabric é um recurso do Azure, para que pode modelar tal como faz a outros recursos no Azure.
* **Integração com a infraestrutura do Azure** Service Fabric coordena com a infraestrutura do Azure subjacente para o sistema operacional, rede e outras atualizações melhorar a disponibilidade e fiabilidade das suas aplicações.  
* **Diagnósticos:** no Azure, podemos fornecer a integração com o diagnóstico do Azure e o Log Analytics.
* **Dimensionamento automático:** para os clusters no Azure, podemos fornecer a funcionalidade de dimensionamento automático incorporada devido a conjuntos de dimensionamento de Máquina Virtual. Em outros ambientes de cloud e no local, terá de criar sua própria funcionalidade ou escala manualmente usando as APIs que expõe o Service Fabric para o dimensionamento de clusters de dimensionamento automático.

## <a name="next-steps"></a>Passos Seguintes

* Criar um cluster no VMs ou computadores que executam o Windows Server: [criação do cluster do Service Fabric para o Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Criar um cluster no VMs ou computadores que executam o Linux: [criar um cluster do Linux](service-fabric-cluster-creation-via-portal.md)
* Saiba mais sobre as [opções de suporte do Service Fabric](service-fabric-support.md)


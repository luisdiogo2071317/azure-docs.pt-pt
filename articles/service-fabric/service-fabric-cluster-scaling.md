---
title: Dimensionamento de clusters Service Fabric do Azure | Microsoft Docs
description: Saiba mais sobre o dimensionamento de clusters de Service Fabric no ou enviados e ou reduzir verticalmente.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/09/2018
ms.author: ryanwi;aljo
ms.openlocfilehash: 897370e48b4eb0db5c99c1102910c89f6f56cc23
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="scaling-service-fabric-clusters"></a>Clusters de dimensionamento do Service Fabric
Um cluster do Service Fabric é um conjunto de ligados à rede de máquinas virtuais ou físicos para o qual os micro-serviços são implementados e geridos. Um computador ou a VM que faz parte de um cluster é designado por um nó. Clusters podem conter potencialmente milhares de nós. Depois de criar um cluster do Service Fabric, pode dimensionar o cluster horizontalmente (alterar o número de nós) ou verticalmente (alterar os recursos de nós).  Pode dimensionar o cluster em qualquer altura, mesmo quando as cargas de trabalho em execução no cluster.  Como o cluster dimensiona, as suas aplicações, dimensionar bem.

Dimensionar por que razão o cluster? Aplicações transaccionais alteram ao longo do tempo.  Poderá ter de aumentar os recursos de cluster para cumprir o tráfego de rede ou de carga de trabalho de aplicação maior ou diminuir os recursos do cluster quando ignora a pedido.

### <a name="scaling-in-and-out-or-horizontal-scaling"></a>Dimensionamento e terminar ou dimensionamento horizontal
Altera o número de nós no cluster.  Assim que os novos nós aderirem ao cluster, o [Gestor de recursos do Cluster](service-fabric-cluster-resource-manager-introduction.md) move serviços-lhes que reduz a carga de nós existentes.  Também pode reduzir o número de nós de recursos do cluster não estão a ser utilizado de forma eficiente.  Como nós deixam o cluster, os serviços mover desativar os nós e carga aumenta nos restantes nós.  Reduzir o número de nós num cluster em execução no Azure pode poupar dinheiro, uma vez que lhe pagar para o número de VMs, utilização e não a carga de trabalho nessas VMs.  

- Vantagens: Infinita escala, em teoria.  Se a aplicação foi concebida para escalabilidade, pode ativar o crescimento ilimitado ao adicionar mais nós.  A ferramentas em ambientes de nuvem torna mais fácil adicionar ou remover nós, para que seja fácil ajustar a capacidade e paga apenas os recursos que utilizar.  
- Desvantagens: Aplicações tem de ser [concebida para escalabilidade](service-fabric-concepts-scalability.md).  Bases de dados de aplicação e persistência podem necessitar de trabalho da arquitetura adicional, bem como dimensionar.  [Coleções fiáveis](service-fabric-reliable-services-reliable-collections.md) nos serviços de monitorização de estado de Service Fabric, no entanto, torna mais fácil e dimensionar os dados da aplicação.

### <a name="scaling-up-and-down-or-vertical-scaling"></a>Aumentar e reduzir verticalmente ou dimensionamento vertical 
Altera os recursos (CPU, memória ou armazenamento) de nós no cluster.
- Vantagens: Software e arquitetura da aplicação permanece igual.
- Desvantagens: Finita escala, uma vez que existe um limite de quanto pode aumentar os recursos em nós individuais. Período de inatividade, dado que precisará colocar máquinas físicas ou virtuais offline para adicionar ou remover recursos.

## <a name="scaling-an-azure-cluster-in-or-out"></a>Dimensionamento de um cluster do Azure ou reduzir
Conjuntos de dimensionamento de máquina virtual são um recurso de computação do Azure que pode utilizar para implementar e gerir uma coleção de máquinas virtuais como um conjunto. Cada tipo de nó que está definido num cluster do Azure é [configurado como um conjunto de dimensionamento separado](service-fabric-cluster-nodetypes.md). Cada tipo de nó, em seguida, pode ser ampliado na ou saída de forma independente, têm conjuntos diferentes de portas abertas e pode ter as métricas de capacidade diferentes. 

Quando o dimensionamento de um cluster do Azure, mantenha as seguintes diretrizes em mente:
- tipos de nó principal executar cargas de trabalho de produção sempre devem ter cinco ou mais nós.
- executar cargas de trabalho de produção com monitorização de estado de tipos de nó principal não devem ter sempre cinco ou mais nós.
- executar cargas de trabalho de produção sem monitorização de estado de tipos de nó principal não devem ter sempre dois ou mais nós.
- Qualquer tipo de nó de [nível de durabilidade](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) de ouro ou Silver deve ter sempre cinco ou mais nós.
- Não remover aleatórios VM instâncias/nós de um tipo de nó, utilize sempre o conjunto de dimensionamento da máquina virtual escala para baixo de funcionalidade. A eliminação de instâncias de VM aleatórias negativamente pode afetar a capacidade de sistemas de balanceamento de carga corretamente.
- Se utilizar regras de dimensionamento automático, defina as regras de modo a que o dimensionamento da (remoção de instâncias de VM) é feito um nó de cada vez. Não é seguro dimensionamento pendente mais de uma instância de cada vez.

Uma vez que os tipos de nó de Service Fabric no seu cluster são constituídos por conjuntos de dimensionamento de máquina virtual no back-end, pode [configurar regras de dimensionamento automático ou dimensionar manualmente](service-fabric-cluster-scale-up-down.md) cada conjunto de dimensionamento de máquina virtual/tipo do nó.

### <a name="programmatic-scaling"></a>Dimensionamento programático
Em vários cenários, [dimensionamento de um cluster manualmente ou com regras de dimensionamento automático](service-fabric-cluster-scale-up-down.md) são boas soluções. Para cenários mais avançados, no entanto, estes não podem ser a opção adequada. Desvantagens potenciais para estas abordagens incluem:

- Dimensionar Manualmente requer a iniciar sessão e explicitamente pedir operações de dimensionamento. Se operações de dimensionamento forem necessárias com frequência, ou em alturas imprevisíveis, esta abordagem não pode ser uma boa solução.
- Quando as regras de dimensionamento automático remover uma instância de um conjunto de dimensionamento de máquina virtual, estes não remove automaticamente dados de conhecimento desse nó do cluster de Service Fabric associado, a menos que o tipo de nó tem um nível de durabilidade de Silver ou Gold. Porque as regras de dimensionamento automático funcionam em escala definido ao nível (e não ao nível do Service Fabric), as regras de dimensionamento automático podem remover nós de Service Fabric sem encerrado-las sem problemas. Esta remoção de nó grosseiro sair "fantasma" Estado do nó de Service Fabric após operações de escala. Uma pessoa (ou um serviço) teria de limpar periodicamente o estado do nó removido no cluster de Service Fabric.
- Um tipo de nó com um nível de durabilidade de ouro ou Silver limpa automaticamente removidas nós, pelo que não é necessária nenhuma limpar adicional.
- Apesar de existirem [métricas muitos](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) suportado pelas regras de dimensionamento automático, é ainda um conjunto limitado. Se o seu cenário chama-se em algum métrica não abrangida desse conjunto de dimensionamento, em seguida, as regras de dimensionamento automático não podem ser uma boa opção.

Como deve abordar o dimensionamento do Service Fabric depende do seu cenário. Se o dimensionamento é invulgar, a capacidade de adicionar ou remover nós manualmente é provavelmente suficiente. Para cenários mais complexos, regras de dimensionamento automático e SDKs exposição a capacidade de dimensionar programaticamente oferecem alternativas poderosas.

As APIs do Azure existem que permitem conjuntos de dimensionamento de aplicações para trabalhar programaticamente com a máquina virtual e clusters do Service Fabric. Se as opções de dimensionamento automático existente não funcionam para o seu cenário, estas APIs possibilitam a implementação da lógica de dimensionamento personalizada. 

Uma abordagem para implementar esta funcionalidade de dimensionamento automático efetuadas home consiste em Adicionar um novo serviço sem monitorização de estado para a aplicação de Service Fabric para gerir as operações de dimensionamento. A criar os seus próprios de dimensionamento serviço fornece que o mais elevado grau de controlo e customizability através da aplicação do dimensionamento comportamento. Isto pode ser útil para cenários que requerem controlo preciso sobre quando ou como uma aplicação dimensiona de entrada ou saída. No entanto, este controlo é fornecido com um compromisso de complexidade de código. Através desta abordagem significa que terá de próprio código dimensionamento, que é não trivial. No âmbito do serviço `RunAsync` método, um conjunto de acionadores pode determinar se o dimensionamento é necessário (incluindo a verificação de parâmetros, tais como o tamanho máximo do cluster e dimensionamento cooldowns).   

A API utilizada para o conjunto de interações de dimensionamento de máquina virtual (ambos para verificar o número atual de instâncias de máquina virtual e modificá-lo) está a [biblioteca de computação de gestão do Azure fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/). A biblioteca de computação fluent fornece uma API de fácil utilização para interagir com conjuntos de dimensionamento de máquina virtual.  Para interagir com o próprio cluster do Service Fabric, utilize [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).

O código de dimensionamento não tem de ser executado como um serviço no cluster para ser ampliada, embora. Ambos `IAzure` e `FabricClient` pode ligar-se aos respetivos recursos do Azure associados remotamente, pelo que o serviço de dimensionamento pode facilmente uma aplicação de consola ou o serviço Windows, executando a partir de fora da aplicação de Service Fabric.

Com base nestas limitações, pode pretender [implementar mais personalizada modelos de dimensionamento automáticos](service-fabric-cluster-programmatic-scaling.md).


## <a name="scaling-a-standalone-cluster-in-or-out"></a>Um cluster autónomo entrada ou saída de dimensionamento
Clusters autónomos permitem implementar recursos de infraestrutura do serviço cluster no local ou no fornecedor de nuvem da sua preferência.  Tipos de nó são compostos por computadores físicos ou máquinas virtuais, dependendo da sua implementação. Em comparação com clusters em execução no Azure, o processo de dimensionamento de um cluster autónomo é ligeiramente mais envolvido.  Tem de alterar manualmente o número de nós no cluster e, em seguida, executar uma atualização da configuração de cluster.

Remoção de nós, pode iniciar várias atualizações. Alguns nós são marcados com `IsSeedNode=”true”` Etiquetar e podem ser identificados consultando o cluster de manifesto utilizando [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest). Remoção de nós pode demorar mais do que outras pessoas, uma vez que os nós seed tem de ser movidos em torno no tais cenários. O cluster tem de manter um mínimo de três nós de tipo de nó principal.

Quando o dimensionamento de um cluster autónomo, mantenha as seguintes diretrizes em mente:
- A substituição de nós principais deve ser executado um nó após a outra, em vez de remover e, em seguida, adicionar em lotes.
- Antes de remover um tipo de nó, verifique se existem quaisquer nós a referenciar o tipo de nó. Remova estes nós antes de remover o tipo de nó correspondente. Depois de todos os nós correspondentes são removidos, pode remover o NodeType da configuração do cluster e iniciar uma configuração de fazer a actualização utilizando [início ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade).

Para obter mais informações, consulte [dimensionar um cluster autónomo](service-fabric-cluster-windows-server-add-remove-nodes.md).

## <a name="scaling-an-azure-cluster-up-or-down"></a>Aumentar um cluster do Azure ou para baixo
Conjuntos de dimensionamento de máquina virtual são um recurso de computação do Azure que pode utilizar para implementar e gerir uma coleção de máquinas virtuais como um conjunto. Cada tipo de nó que está definido num cluster do Azure é [configurado como um conjunto de dimensionamento separado](service-fabric-cluster-nodetypes.md). Em seguida, pode ser gerido a cada tipo de nó em separado.  Dimensionamento de um tipo de nó ou reduzir verticalmente envolve a alterar o SKU de instâncias da máquina virtual no conjunto de dimensionamento. 

> [!WARNING]
> Recomendamos que altere o SKU de VM de um tipo de nó/conjunto de dimensionamento, a menos que está a ser executado em [Silver durabilidade ou superior](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster). Alterar tamanho da VM SKU é uma operação de infraestrutura do destrutivas de dados no local. Sem capacidade algum atraso ou monitorizar esta alteração, é possível que a operação pode causar a perda de dados para os serviços com monitorização de estado ou fazer com que outros problemas operacionais imprevistos, mesmo para cargas de trabalho sem monitorização de estado. 
>

Quando o dimensionamento de um cluster do Azure, mantenha a orientação seguinte em mente:
- Se o dimensionamento para baixo de um tipo de nó principal, deve nunca reduzir-verticalmente mais de que o [escalão de fiabilidade](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) permite.

O processo de dimensionamento de um tipo de nó ou reduzir verticalmente é diferente consoante se é um tipo de nó não principal ou primária.

### <a name="scaling-non-primary-node-types"></a>Tipos de nó não primário dimensionamento
Crie um novo tipo de nó com os recursos que necessita.  Atualize as restrições de posicionamento de executar os serviços para incluir o novo tipo de nó.  Gradualmente (um de cada vez), reduzir a contagem de instâncias de contagem de instâncias de tipo de nó antigo para zero, para que a fiabilidade do cluster não é afetada.  Serviços gradualmente irão migrar para o novo tipo de nó, como o tipo de nó antigo é decommisioned.

### <a name="scaling-the-primary-node-type"></a>O tipo de nó principal de dimensionamento
Recomendamos que altere o SKU de VM do tipo de nó principal. Se precisar de mais capacidade de cluster, recomendamos adicionar mais instâncias. 

Se que não é possível, pode criar um novo cluster e [restaurar o estado da aplicação](service-fabric-reliable-services-backup-restore.md) (se aplicável) do cluster antigo. Não é necessário restaurar o estado do serviço qualquer sistema, são recriadas quando implementa as aplicações para o novo cluster. Se foram apenas aplicações em execução sem monitorização de estado no seu cluster, em seguida, tudo o que fazer é implementar aplicações para o novo cluster, não tem nada para restaurar. Se optar por voltar a rota não suportada e pretender alterar o SKU de VM, em seguida, marca modificações para o dimensionamento da máquina virtual definição do conjunto de modelo para refletir o SKU de novo. Se o cluster tem apenas um tipo de nó, em seguida, certifique-se de que todas as suas aplicações com monitorização de estado respondem a todos os [Service eventos de ciclo de vida de réplica](service-fabric-reliable-services-lifecycle.md) (como a réplica de compilação está encravada) em tempo útil e se a réplica de serviço reconstruir duração é inferior a cinco minutos (nível de durabilidade prata). 

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [escalabilidade de aplicação](service-fabric-concepts-scalability.md).
* [Dimensionar um cluster do Azure ou reduzir](service-fabric-tutorial-scale-cluster.md).
* [Dimensionar um cluster do Azure através de programação](service-fabric-cluster-programmatic-scaling.md) através do Azure fluent computação SDK.
* [Um cluster de standaone entrada ou saída de escala](service-fabric-cluster-windows-server-add-remove-nodes.md).


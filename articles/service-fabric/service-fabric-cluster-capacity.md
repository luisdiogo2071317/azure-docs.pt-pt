---
title: Planear a capacidade de cluster do Service Fabric | Microsoft Docs
description: Capacidade de cluster do Service Fabric considerações de planeamento. Nodetypes, operações, características de durabilidade e fiabilidade camadas
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: ''
ms.assetid: 4c584f4a-cb1f-400c-b61f-1f797f11c982
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2018
ms.author: chackdan
ms.openlocfilehash: a5046a5e3771e95d76bb6edc7987a1e3176abeb0
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309420"
---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Considerações de planeamento de capacidade do cluster de Service Fabric
Para qualquer implementação de produção, o planeamento de capacidade é um passo importante. Seguem-se alguns dos itens que terá de considerar como parte do processo.

* O número de tipos de nó que do cluster tem de começar a utilizar
* As propriedades de cada tipo de nó (tamanho, o servidor primário, a internet com número de VMs, etc.)
* As características de fiabilidade e durabilidade do cluster

Informe-por breves instantes Reveja cada um destes itens.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>O número de tipos de nó que do cluster tem de começar a utilizar
Em primeiro lugar, tem de descobrir que o cluster que estiver a criar vai ser utilizado para.  Os tipos de aplicações estiver a planear implementar para este cluster? Se não estiver encriptado no objetivo do cluster, não são mais provável ainda está preparado para introduzir a processo de planeamento da capacidade.

Estabelece o número de tipos de nó que do cluster tem de começar a.  Cada tipo de nó está mapeado para um conjunto de dimensionamento de máquina virtual. Cada tipo de nó pode então ser aumentado ou reduzido verticalmente de forma independente, pode ter conjuntos diferentes de portas abertas e ter métricas de capacidade diferente. Por isso, a decisão do número de tipos de nó essencialmente vem para baixo para as seguintes considerações:

* A sua aplicação tem vários serviços e qualquer um deles tem de ser public ou para a internet? Aplicações típicas contém um serviço de front-end do gateway que recebe a entrada de um cliente e um ou mais back-end serviços que comunicam com os serviços front-end. Por isso, neste caso, pode ficar ter, pelo menos, dois tipos de nó.
* Os serviços (que compõem a sua aplicação) dispõem de necessidades de infraestrutura diferentes, como maior RAM ou superiores ciclos da CPU? Por exemplo, vamos assuma que contém a aplicação que pretende implementar um serviço front-end e um serviço de back-end. Pode executar o serviço de front-end em VMs mais pequenas (tamanhos de VM, como D2) que tem portas abertas à internet.  O serviço de back-end, no entanto, é intensiva de cálculo e tem de executar em VMs maior (com tamanhos de VM, como D4, D6, D15) que não são internet enfrentam.
  
  Neste exemplo, embora pode optar por colocar em pausa todos os serviços de um tipo de nó, recomendamos que coloque-los num cluster com dois tipos de nó.  Isto permite que cada tipo de nó tem propriedades diferentes, como a conectividade à internet ou o tamanho da VM. Separadamente, também pode ser escalado para o número de VMs.  
* Porque não é possível prever o futuro, aceda com factos que sabe e escolha o número de tipos de nó que as suas aplicações precisam para começar. Pode sempre adicionar ou remover tipos de nó mais tarde. Um cluster do Service Fabric tem de ter, pelo menos, um tipo de nó.

## <a name="the-properties-of-each-node-type"></a>As propriedades de cada tipo de nó
O **tipo de nó** podem ser vistos como equivalentes às funções nos serviços em nuvem. Tipos de nó definem os tamanhos VM, o número de VMs e as respetivas propriedades. Cada tipo de nó que está definido num cluster de Service Fabric mapeia para um [conjunto de dimensionamento da máquina virtual](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview).  
Cada tipo de nó é uma escala distinta definida e pode ser escalada para cima ou para baixo de forma independente, têm conjuntos diferentes de portas abertas e têm as métricas de capacidade diferentes. Para obter mais informações sobre as relações entre os tipos de nós e os conjuntos de dimensionamento de máquina virtual, como RDP para uma das instâncias, como abrir novas portas e assim sucessivamente, consulte [tipos de nó de cluster do Service Fabric](service-fabric-cluster-nodetypes.md).

Um cluster do Service Fabric pode consistir em mais do que um tipo de nó. Que eventos, o cluster consiste num tipo de nó principal e um ou mais tipos de nó não principal.

Um tipo de nó único simplesmente não pode exceder os 100 nós por conjunto de dimensionamento da máquina virtual. Poderá ter de adicionar conjuntos de dimensionamento de máquina virtual para alcançar a escala de destino e o dimensionamento automático não é possível automagically adicionar conjuntos de dimensionamento de máquina virtual. Adicionar conjuntos de dimensionamento de máquina virtual no local para um cluster em direto é uma tarefa difícil e frequentemente como resultado, os utilizadores aprovisionamento novos clusters com os tipos de nó adequado aprovisionados no momento da criação. 

### <a name="primary-node-type"></a>Tipo de nó principal

Os serviços de sistema do Service Fabric (por exemplo, o serviço do Gestor de clusters ou o serviço de arquivo de imagens) são colocados no tipo de nó principal. 

![Captura de ecrã de um cluster que tem dois tipos de nó][SystemServices]

* O **tamanho mínimo de VMs** para o nó principal tipo é determinado pelo **o escalão de durabilidade** que escolher. O escalão de durabilidade de predefinição está Bronze. Consulte [as características de durabilidade do cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) para obter mais detalhes.  
* O **número mínimo de VMs** para o nó principal tipo é determinado pelo **escalão de fiabilidade** que escolher. O escalão de fiabilidade de predefinição está Silver. Consulte [as características de fiabilidade do cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-reliability-characteristics-of-the-cluster) para obter mais detalhes.  

A partir do modelo Azure Resource Manager, o tipo de nó principal está configurado com o `isPrimary` atributo sob o [definição de tipo de nó](https://docs.microsoft.com/en-us/azure/templates/microsoft.servicefabric/clusters#nodetypedescription-object).

### <a name="non-primary-node-type"></a>Tipo de nó principal não

Num cluster com vários tipos de nó, existe um tipo de nó principal e o resto não principal.

* O **tamanho mínimo de VMs** para o nó não primário tipos é determinado pelo **o escalão de durabilidade** que escolher. O escalão de durabilidade de predefinição está Bronze. Consulte [as características de durabilidade do cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) para obter mais detalhes.  
* O **número mínimo de VMs** para tipos de nó principal não é um. No entanto, deve escolher este número com base no número de réplicas de aplicações/serviços que pretende executar neste tipo de nó. O número de VMs num tipo de nó pode ser aumentado depois de implementar o cluster.

## <a name="the-durability-characteristics-of-the-cluster"></a>As características de durabilidade do cluster
O escalão de durabilidade é utilizado para indicar ao sistema os privilégios que tenham as VMs com a infraestrutura do Azure subjacente. O tipo de nó principal, este privilégio permite que o Service Fabric colocar em pausa qualquer pedido de infraestrutura de nível de VM (por exemplo, um reinício VM, recriação de imagem de VM ou migração de VM) que afetam os requisitos de quórum para os serviços do sistema e os serviços com monitorização de estado. Os tipos de nó não principal, este privilégio permite-Service Fabric colocar em pausa quaisquer pedidos de infraestrutura de nível de VM (por exemplo, o reinício da VM, recriação de imagem de VM e de migração de VM) que afetam os requisitos de quórum para os serviços com monitorização de estado.

| Escalão de durabilidade  | Número mínimo necessário de VMs | SKUs VM suportados                                                                  | Atualizações de que tornar a sua VMSS                               | Atualizações e manutenção iniciada pelo Azure                                                              | 
| ---------------- |  ----------------------------  | ---------------------------------------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Dourado             | 5                              | SKUs de nó completo dedicados a um único cliente (por exemplo, L32s GS5, G5, DS15_v2, D15_v2) | Pode sofrer um atraso de até aprovados pelo cluster do Service Fabric | Pode ser colocado em pausa durante 2 horas por UD para permitir tempo adicional para réplicas recuperar de falhas anteriores |
| Prateado           | 5                              | VMs de único núcleo ou superior                                                        | Pode sofrer um atraso de até aprovados pelo cluster do Service Fabric | Não pode ser atrasada durante qualquer período de tempo significativo                                                    |
| Bronze           | 1                              | Todos                                                                                | Não é atrasada pelo cluster do Service Fabric           | Não pode ser atrasada durante qualquer período de tempo significativo                                                    |

> [!WARNING]
> Obter os tipos de nó em execução com Bronze durability _sem privilégios_. Isto significa que as tarefas de infraestrutura que afetam as cargas de trabalho sem monitorização de estado serão não ser paradas ou atrasadas, que pode afetar as cargas de trabalho. Utilize apenas Bronze para tipos de nó que executam apenas as cargas de trabalho sem monitorização de estado. Para cargas de trabalho de produção, em execução Silver ou acima é recomendada. 
>

**Vantagens da utilização de níveis de durabilidade Silver ou Gold**
 
- Reduz o número de passos necessários numa operação de escala no (ou seja, a desativação de nós e Remove-ServiceFabricNodeState é chamado automaticamente).
- Reduz o risco de perda de dados devido a operações de infraestrutura do Azure ou uma operação alteração iniciada pelo cliente de SKU de VM de no local.

**Desvantagens da utilização de níveis de durabilidade Silver ou Gold**
 
- Relacionadas com as implementações para o dimensionamento da máquina virtual definido e outro recursos do Azure podem sofrer um atraso, podem tempo ou podem ser impedidos inteiramente por problemas no seu cluster ou ao nível da infraestrutura. 
- Aumenta o número de [eventos de ciclo de vida de réplica](service-fabric-reliable-services-lifecycle.md) (por exemplo, primárias trocas) devido a automatizada deactivations de nó durante as operações de infraestrutura do Azure.
- Demora nós fora de serviço para períodos de tempo durante as atualizações de software da plataforma do Azure ou a manutenção de hardware atividades estão a ocorrer. Poderá ver nós com o estado desativar/desativado durante estas atividades. Esta reduz temporariamente a capacidade do seu cluster, mas não deve afetar a disponibilidade do seu cluster ou aplicações.

### <a name="recommendations-for-when-to-use-silver-or-gold-durability-levels"></a>Recomendações para quando utilizar os níveis de durabilidade Silver ou Gold

Utilizar Silver ou Gold durabilidade para todos os tipos de nó serviços com monitorização de estado de espera na escala de anfitrião (Reduza a contagem de instâncias VM) com frequência, e preferir que operações de implementação é atrasada e a capacidade para ser reduzido favor simplificando estes escala-in operações. Os cenários de escalamento horizontal (adicionar instâncias de VMs) desempenha à sua escolha do escalão de durabilidade, apenas escala-in.

### <a name="changing-durability-levels"></a>Alterar os níveis de durabilidade
- Não podem ser alterada uma versão anterior tipos de nó com níveis de durabilidade de Silver ou Gold em para Bronze.
- Atualizar a partir do Bronze Silver ou Gold pode demorar algumas horas.
- Quando alterar o nível de durabilidade, lembre-se de que a atualização da configuração Service Fabric de extensão no seu recurso de conjunto de dimensionamento de máquina virtual e, na definição do tipo de nó no seu recurso de cluster do Service Fabric. Estes valores têm de corresponder.

### <a name="operational-recommendations-for-the-node-type-that-you-have-set-to-silver-or-gold-durability-level"></a>Recomendações operacionais para o nó de tipo que definiu para durabilidade prata ou gold nível.

- Manter o cluster e aplicações em bom estado de todas as vezes e certifique-se de que as aplicações respondem a todos os [Service eventos de ciclo de vida de réplica](service-fabric-reliable-services-lifecycle.md) (como a réplica de compilação está encravada) atempadamente.
- Adotar formas mais segura para efetuar uma alteração de VM SKU (dimensionamento cima/para baixo): a alterar o SKU de VM de um conjunto de dimensionamento de máquina virtual é, inerentemente, uma operação não segura e, por isso, devem ser evitado se possível. Eis o processo que pode seguir para evitar problemas comuns.
    - **Para tipos de nó não principal:** é recomendado que crie o novo conjunto de dimensionamento de máquina virtual, modifique a restrição de posicionamento de serviço a incluir o tipo de nó/conjunto de dimensionamento de máquina virtual novo e, em seguida, reduzir a instância de conjunto de dimensionamento de máquina virtual antigo Contagem como 0, um nó numa altura (trata-se para se certificar de que a remoção de nós não afeta a fiabilidade do cluster).
    - **Para o tipo de nó principal:** a nossa recomendação é que não altere o SKU de VM do tipo de nó principal. A alteração do tipo de nó principal que SKU não é suportada. Se o motivo para o SKU nova capacidade, recomendamos adicionar mais instâncias. Se esse não é possível, crie um novo cluster e [restaurar o estado da aplicação](service-fabric-reliable-services-backup-restore.md) (se aplicável) do cluster antigo. Não é necessário restaurar o estado do serviço qualquer sistema, são recriadas quando implementa as aplicações para o novo cluster. Se foram apenas aplicações em execução sem monitorização de estado no seu cluster, em seguida, tudo o que fazer é implementar aplicações para o novo cluster, não tem nada para restaurar. Se optar por voltar a rota não suportada e pretender alterar o SKU de VM, em seguida, marca modificações para o dimensionamento da máquina virtual definição do conjunto de modelo para refletir o SKU de novo. Se o cluster tem apenas um tipo de nó, em seguida, certifique-se de que todas as suas aplicações com monitorização de estado respondem a todos os [Service eventos de ciclo de vida de réplica](service-fabric-reliable-services-lifecycle.md) (como a réplica de compilação está encravada) em tempo útil e se a réplica de serviço reconstruir duração é inferior a cinco minutos (nível de durabilidade prata). 

    > [!WARNING]
    > Alteração do tamanho do SKU de VM para conjuntos de dimensionamento de máquina virtual não está em execução, pelo menos, prata durabilidade não é recomendada. Alterar tamanho da VM SKU é uma operação de infraestrutura do destrutivas de dados no local. Sem, pelo menos, alguma capacidade de atraso ou monitorizar esta alteração, é possível que a operação pode causar a perda de dados para os serviços com monitorização de estado ou fazer com que outros problemas operacionais imprevistos, mesmo para cargas de trabalho sem monitorização de estado. 
    > 
    
- Manter um número mínimo de cinco nós para qualquer conjunto de dimensionamento de máquina virtual que tem um nível de durabilidade de ouro ou Silver ativado.
- Cada conjunto com o nível de durabilidade Silver ou Gold de dimensionamento VM tem de mapear para o seu próprio tipo de nó no cluster de Service Fabric. Mapeamento de VM de vários conjuntos de dimensionamento para um tipo de nó único irão impedir coordenação entre o cluster do Service Fabric e a infraestrutura do Azure funcionar corretamente.
- Não eliminar aleatórias instâncias VM, utilize sempre o conjunto de dimensionamento da máquina virtual escala para baixo de funcionalidade. A eliminação de instâncias de VM aleatórias tem um potencial de criação de imbalances na instância VM distribuídos por UD e df. Este desequilíbrio negativamente pode afetar a capacidade de sistemas corretamente para equilibrar a carga entre as réplicas de serviço/instâncias de serviço.
- Se utilizar o dimensionamento automático, defina as regras que escala em (remoção de instâncias VM) são efetuadas apenas um nó de cada vez. Não é seguro dimensionamento pendente mais de uma instância de cada vez.
- Se eliminar ou Desalocação VMs no tipo de nó principal, nunca deve reduzir o número de VMs alocados abaixo que requer o escalão de fiabilidade. Estas operações serão bloqueadas indefinidamente um conjunto com um nível de durabilidade de Silver ou Gold de dimensionamento.

## <a name="the-reliability-characteristics-of-the-cluster"></a>As características de fiabilidade do cluster
O escalão de fiabilidade é utilizado para definir o número de réplicas os sistema de serviços de que pretende executar neste cluster no tipo de nó principal. Mais o número de réplicas, mais fiável os serviços de sistema estão no seu cluster.  

O escalão de fiabilidade pode efetuar os seguintes valores:

* Platinum - executar os serviços do sistema com um destino de réplica definir a contagem de nove
* Ouro - executar os serviços do sistema com um destino de réplica definir a contagem de sete
* Prata - executar os serviços do sistema com um destino de réplica definir a contagem de cinco 
* Bronze – executar os serviços do sistema com um destino de réplica definir a contagem de três

> [!NOTE]
> O escalão de fiabilidade, escolha determina o número mínimo de nós que tem de ter o tipo de nó principal. 
> 
> 

### <a name="recommendations-for-the-reliability-tier"></a>Recomendações para o escalão de fiabilidade

Ao aumentar ou diminuir o tamanho do seu cluster (a soma de instâncias VM em todos os tipos de nó), tem de atualizar a fiabilidade do cluster de um escalão para outro. Fazê-lo aciona as atualizações de cluster necessário alterar a réplica de serviços de sistema definido contagem. Aguarde a atualização em curso para concluir antes de efetuar outras alterações para o cluster, como adicionar nós.  Pode monitorizar o progresso da atualização no Service Fabric Explorer ou executando [Get ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

Eis a recomendação sobre como escolher o escalão de fiabilidade.

| **Tamanho do cluster** | **Escalão de fiabilidade** |
| --- | --- |
| 1 |Especifique o parâmetro de escalão de fiabilidade, o sistema calcula-la |
| 3 |Bronze |
| 5 ou 6|Prateado |
| 7 ou 8 |Dourado |
| 9 e até |Platinum |

## <a name="primary-node-type---capacity-guidance"></a>Tipo de nó primário - orientações de capacidade

Esta é a orientação para planear a capacidade de tipo de nó principal:

- **Número de instâncias VM para executar qualquer carga de trabalho de produção no Azure:** tem de especificar um tamanho de tipo de nó principal mínimo de 5. 
- **Número de instâncias VM para executar cargas de trabalho de teste no Azure** pode especificar um tamanho de tipo de nó principal mínimo de 1 ou 3. O cluster de um nó, é executado com uma configuração especial e por isso, escala fora desse cluster não é suportada. O cluster de um nó não possui nenhuma fiabilidade e, por isso, no seu modelo do Resource Manager, é necessário remover/não especificar que a configuração (não definir o valor de configuração não é suficiente). Se configurar o cluster de um nó configurar através do portal, em seguida, a configuração está automaticamente tratada. Um e três clusters de nó não são suportados para executar cargas de trabalho de produção. 
- **SKU de VM:** tipo de nó principal é onde executar os serviços do sistema, para o SKU de VM que escolher, deve tomar em consideração o horário de pico geral carregar pretende colocar em cluster. Eis uma analogia ilustrar o que devo dizer aqui-considerar o tipo de nó principal como as "Lungs", é o que fornece oxygen para o "-brain" e, por isso, se o "-brain" não consegue suficiente oxygen, o corpo sofrerá. 

Uma vez que as necessidades de capacidade de um cluster é determinado pela carga de trabalho que pretende executar no cluster, não é possível fornecemos-lhe qualitativa orientações para a carga de trabalho específica, no entanto esta é a orientação abrangente para o ajudar a começar a utilizar

Para cargas de trabalho de produção: 

- O SKU de VM recomendada é D3 padrão ou D3_V2 padrão ou equivalente com um mínimo de 14 GB de local SSD.
- A utilização de suportado mínimo SKU de VM é D1 padrão ou D1_V2 padrão ou equivalente com um mínimo de 14 GB de local SSD. 
- Core parcial SKUs de VM, como A0 padrão não são suportadas para cargas de trabalho de produção.
- Standard A1 SKU não é suportado para cargas de trabalho de produção por motivos de desempenho.

> [!WARNING]
> Atualmente, a alteração do tamanho de VM SKU num cluster em execução de nó principal não é suportada. Por isso, escolha o tipo de nó principal VM SKU cuidadosamente, tendo em conta as necessidades futuras de capacidade. Neste momento, a única forma de suportados para mover o tipo de nó principal para um novo SKU de VM (inferior ou superior) está a criar um novo cluster com a capacidade adequada, implementar as suas aplicações e, em seguida, restaurar o estado da aplicação (se aplicável) a partir de [ mais recente do serviço cópias de segurança](service-fabric-reliable-services-backup-restore.md) efetuou do cluster antigo. Não é necessário restaurar o estado do serviço qualquer sistema, são recriados ao implementar aplicações para o novo cluster. Se foram apenas aplicações em execução sem monitorização de estado no seu cluster, em seguida, tudo o que fazer é implementar aplicações para o novo cluster, não tem nada para restaurar.
> 

## <a name="non-primary-node-type---capacity-guidance-for-stateful-workloads"></a>Tipo de nó de não-primary - orientações de capacidade para cargas de trabalho com monitorização de estado

Esta orientação é para cargas de trabalho com monitorização de estado utilizando o Service fabric [coleções fiáveis ou reliable Actors](service-fabric-choose-framework.md) que estejam a executar o tipo de nó não principal.

**Número de instâncias VM:** para produção as cargas de trabalho com monitorização de estado, é recomendado executar com uma contagem de réplica e o destino da mínimo de 5. Isto significa que no estado de repouso acaba por ficar com uma réplica (a partir de um conjunto de réplicas) em cada domínio de falhas e o domínio de atualização. O conceito de escalão de fiabilidade todo para o tipo de nó principal é uma forma de especificar esta definição para serviços do sistema. Por isso, a mesma consideração aplica-se os serviços de monitorização de estado.

Por isso, para cargas de trabalho de produção, o tamanho de tipo mínimo recomendado não - nó principal é 5, se estiver a executar cargas de trabalho com monitorização de estado no mesmo.

**SKU de VM:** este é o tipo de nó em que os serviços de aplicação estiver a executar, pelo que o SKU de VM que escolher, deve ter em consideração o pico de carga que pretende colocar em cada nó. As necessidades de capacidade da nodetype, é determinado pela carga de trabalho que pretende executar no cluster, pelo que não é possível fornecemos-lhe qualitativa orientações para a carga de trabalho específica, no entanto esta é a orientação abrangente para o ajudar a começar a utilizar

Para cargas de trabalho de produção 

- O SKU de VM recomendada é D3 padrão ou D3_V2 padrão ou equivalente com um mínimo de 14 GB de local SSD.
- A utilização de suportado mínimo SKU de VM é D1 padrão ou D1_V2 padrão ou equivalente com um mínimo de 14 GB de local SSD. 
- Core parcial SKUs de VM, como A0 padrão não são suportadas para cargas de trabalho de produção.
- SKU standard de A1 especificamente não é suportado para cargas de trabalho de produção por motivos de desempenho.

## <a name="non-primary-node-type---capacity-guidance-for-stateless-workloads"></a>Tipo de nó de não-primary - orientações de capacidade para cargas de trabalho sem monitorização de estado

Esta orientação das cargas de trabalho sem monitorização de estado que estejam a executar o nodetype não principal.

**Número de instâncias VM:** para produção as cargas de trabalho sem monitorização de estado, o tamanho de tipo de mínimo suportado não - nó principal é 2. Isto permite-lhe executar, duas instâncias sem monitorização de estado da sua aplicação e permitindo que o serviço sobreviver a perda de uma instância VM. 

**SKU de VM:** este é o tipo de nó em que os serviços de aplicação estiver a executar, pelo que o SKU de VM que escolher, deve ter em consideração o pico de carga que pretende colocar em cada nó. As necessidades de capacidade do tipo de nó é determinado pela carga de trabalho que pretende executar no cluster, pelo que não é possível fornecemos-lhe qualitativa orientações para a carga de trabalho específica, no entanto esta é a orientação abrangente para o ajudar a começar a utilizar

Para cargas de trabalho de produção 

- O SKU de VM recomendada é D3 padrão ou D3_V2 padrão ou equivalente. 
- A utilização de suportado mínimo SKU de VM é D1 padrão ou D1_V2 padrão ou equivalente. 
- Core parcial SKUs de VM, como A0 padrão não são suportadas para cargas de trabalho de produção.
- Standard A1 SKU não é suportado para cargas de trabalho de produção por motivos de desempenho.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Passos Seguintes
Depois de concluir o seu planeamento de capacidade e configurar um cluster, leia o seguinte:

* [Segurança de cluster do Service Fabric](service-fabric-cluster-security.md)
* [O dimensionamento de clusters de Service Fabric](service-fabric-cluster-scaling.md)
* [Planear a recuperação após desastre](service-fabric-disaster-recovery.md)
* [Definir a relação de Nodetypes dimensionamento de máquina virtual](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png

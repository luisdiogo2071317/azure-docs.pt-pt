---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-windows, azure-resource-manager
author: genlin
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 04/14/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 24d89b617c347bc9443b437c92cb034acb3e05cb
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
Quando criar uma máquina virtual (VM), reinicie VMs (desalocadas) paradas ou redimensionar uma VM, o Microsoft Azure aloca recursos de computação para a sua subscrição. Iremos são continuamente investir na infraestrutura adicional e funcionalidades para se certificar de que temos sempre todos os tipos VM disponíveis para suportar a pedido do cliente. No entanto, ocasionalmente, podem ocorrer falhas de atribuição de recursos devido ao crescimento inédito em termos de serviços do Azure em regiões específicas. Este problema pode ocorrer ao tentar criar ou iniciar VMs numa região enquanto as VMs apresentam o seguinte código de erro e a mensagem:

**Código de erro**: AllocationFailed ou ZonalAllocationFailed

**Mensagem de erro**: "Falha na alocação. Não existe capacidade suficiente para o tamanho da VM pedida nesta região. Saiba mais sobre a melhorar a probabilidade de sucesso de alocação em http://aka.ms/allocation-guidance"

Este artigo explica as causas de algumas das falhas de alocação comuns e sugere possíveis responsabilidade.

Se o problema do Azure não esteja endereçado neste artigo, visite o [fóruns do Azure no MSDN e Stack Overflow](https://azure.microsoft.com/support/forums/). Pode publicar o seu problema estes fóruns ou a @AzureSupport no Twitter. Além disso, pode ficheiro um pedido de suporte do Azure, selecionando o suporte de Get na [suporte do Azure](https://azure.microsoft.com/support/options/) site.

Até que o tipo VM preferencial esteja disponível na sua região preferencial, Aconselhamos os clientes que se ocorrerem problemas de implementação a ter em consideração a documentação de orientação na tabela seguinte como solução temporária. 

Identificar cenário que melhor de adeque ao seu incidente e, em seguida, repita o pedido de alocação ao utilizar a solução sugerida correspondente para aumentar a probabilidade de sucesso de alocação. Em alternativa, pode sempre voltar a tentar mais tarde. Isto acontece porque existem recursos suficientes podem ter sido libertados no cluster, a região ou a zona para acomodar o seu pedido. 


## <a name="resize-a-vm-or-add-vms-to-an-existing-availability-set"></a>Redimensionar uma VM ou adicionar VMs a um conjunto de disponibilidade existente

### <a name="cause"></a>Causa

Definir um pedido para redimensionar uma VM ou adicionar que uma VM a um conjunto de disponibilidade existente tem de ser feita no cluster original que aloja a disponibilidade existente. O tamanho da VM solicitado é suportado pelo cluster, mas o cluster pode atualmente tem capacidade suficiente. 

### <a name="workaround"></a>Solução

Se a VM pode fazer parte de um conjunto de disponibilidade diferente, crie uma VM numa conjunto (na mesma região) de disponibilidade diferente. Esta nova VM, em seguida, pode ser adicionada à mesma rede virtual.

Parar (desalocar) todas as VMs no mesmo disponibilidade definido, em seguida, reinicie cada um deles.
Para parar: clique em grupos de recursos > [o grupo de recursos] > recursos > [o conjunto de disponibilidade] > máquinas > [sua máquina virtual] > parar.
Depois de parar todas as VMs, selecione a primeira VM e, em seguida, clique em Iniciar.
Este passo certifica-se de que é executada uma nova tentativa de atribuição e que um novo cluster pode ser selecionado que tem capacidade suficiente.

## <a name="restart-partially-stopped-deallocated-vms"></a>Reinício parcialmente parada (desalocadas) VMs

### <a name="cause"></a>Causa

Desalocação parcial significa que parada (desalocada) uma ou mais, mas não todos, VMs na disponibilidade de um conjunto. Quando anular a atribuição de uma VM, são lançados os recursos associados. Reinício de VMs de um conjunto de disponibilidade parcialmente desalocada é igual a adição de VMs para um conjunto de disponibilidade existente. Por conseguinte, o pedido de alocação tem de ser tentou no cluster original anfitriões que o existente conjunto de disponibilidade que podem não ter capacidade suficiente.

### <a name="workaround"></a>Solução

Parar (desalocar) todas as VMs no mesmo disponibilidade definido, em seguida, reinicie cada um deles.
Para parar: clique em grupos de recursos > [o grupo de recursos] > recursos > [o conjunto de disponibilidade] > máquinas > [sua máquina virtual] > parar.
Depois de parar todas as VMs, selecione a primeira VM e, em seguida, clique em Iniciar.
Esta ação irá garantir que é executada uma nova tentativa de atribuição e que um novo cluster pode ser selecionado que tem capacidade suficiente.

## <a name="restart-fully-stopped-deallocated-vms"></a>Reinício totalmente parada (desalocadas) VMs

### <a name="cause"></a>Causa

Meios de Desalocação completo que deixaram (desalocado) todas as VMs num conjunto de disponibilidade. O pedido de alocação para reiniciar estes VMs destina-se a todos os clusters que suportam o tamanho pretendido dentro da região ou a zona. Alterar o seu pedido de alocação pelas sugestões neste artigo e repita o pedido para melhorar a hipótese de sucesso de alocação. 

### <a name="workaround"></a>Solução

Se utilizar mais antigos série VM ou tamanhos, tais como Dv1, DSv1, Av1, D15v2 ou DS15v2, considere mover para versões mais recentes. Ver estas recomendações para tamanhos de VM específicas.
Se não tiver a opção para utilizar um tamanho VM diferente, experimente implementar noutra região na mesma geografia. Para obter mais informações sobre os tamanhos VM disponíveis em cada região na https://aka.ms/azure-regions

Se estiver a utilizar zonas disponibilidade, tente outra zona na região que poderão ter a capacidade disponível para o tamanho da VM pedida.

Se o pedido de atribuição é grande (mais do que 500 núcleos), consulte a documentação de orientação nas secções seguintes para dividir o pedido para implementações mais pequenas.

## <a name="allocation-failures-for-older-vm-sizes-av1-dv1-dsv1-d15v2-ds15v2-etc"></a>Falhas de atribuição para tamanhos de VM mais antigas (Av1 Dv1, DSv1, D15v2, DS15v2, etc.)

Como podemos expandir a infraestrutura do Azure, iremos implementar hardware de geração de mais recente que foi concebido para suportar os tipos de máquina virtual mais recentes. Algumas das VMs de série anteriores não são executados na nossa infraestrutura de geração mais recente. Por este motivo, os clientes podem ter ocasionalmente falhas de atribuição para estes SKUs de legado. Para evitar este problema, incentivamos os clientes que estão a utilizar máquinas virtuais de série legado considere mover para as VMs mais recentes equivalentes pelas seguintes recomendações: estas VMs estão otimizadas para o hardware mais recente e irá permitir-lhe tirar partido das melhores preço e desempenho. 

|Legado-série/tamanho da VM|Recomendado mais recente-série/tamanho da VM|Mais informações|
|----------------------|----------------------------|--------------------|
|Série Av1|[Série Av2](../articles/virtual-machines/windows/sizes-general.md#av2-series)|https://azure.microsoft.com/blog/new-av2-series-vm-sizes/
|Dv1 ou série DSv1 (D1 para D5)|[Dv3 ou DSv3 série](../articles/virtual-machines/windows/sizes-general.md#dsv3-series-sup1sup)|https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/
|Dv1 ou série DSv1 (D11 para D14)|[Ev3 ou ESv3 série](../articles/virtual-machines/windows/sizes-memory.md#ev3-series)|
|D15v2 ou DS15v2|Se estiver a utilizar o modelo de implementação do Gestor de theResource para tirar o máximo partido dos tamanhos de VM maiores, considere mover D16v3/DS16v3 ou D32v3/DS32v3. Estes são concebidos para serem executados no hardware de geração de mais recente. Se estiver a utilizar o modelo de implementação Resource Manager para se certificar de que a instância VM está isolada para hardware dedicado a um único cliente, considere mover para os novo isolados tamanhos de VM, E64i_v3 ou E64is_v3, que são concebidos para serem executados no hardware de geração de mais recente. |https://azure.microsoft.com/blog/new-isolated-vm-sizes-now-available/

## <a name="allocation-failures-for-large-deployments-more-than-500-cores"></a>Falhas de atribuição para grandes implementações (mais do que 500 núcleos)

Reduza o número de instâncias do tamanho da VM pedida e, em seguida, repita a operação de implementação. Além disso, para implementações maiores, pode querer avaliar [conjuntos de dimensionamento de máquina virtual do Azure](https://docs.microsoft.com/azure/virtual-machine-scale-sets/). O número de instâncias VM automaticamente pode aumentar ou diminuir em resposta a pedido ou um agendamento definido, e tiver uma maior hipótese de sucesso de alocação porque as implementações de serem distribuídas por vários clusters. 

## <a name="background-information"></a>Informações gerais
### <a name="how-allocation-works"></a>Como funciona a alocação
Os servidores nos datacenters do Azure são divididos em partições em clusters. Normalmente, é tentado um pedido de alocação em vários clusters, mas é possível que determinadas restrições do pedido de alocação forcem a plataforma do Azure para tentar o pedido em apenas um cluster. Neste artigo, iremos irá consultar este como "afixado para um cluster." Diagrama 1 abaixo ilustra o caso de uma alocação normal, que é tentada em vários clusters. Diagrama 2 ilustra o caso de uma alocação que tenha afixado ao Cluster 2 uma vez que é onde está alojado o conjunto de CS_1 do serviço de nuvem ou de disponibilidade existente.
![Diagrama de atribuição](./media/virtual-machines-common-allocation-failure/Allocation1.png)

### <a name="why-allocation-failures-happen"></a>Por que motivo ocorrem falhas de atribuição
Quando um pedido de alocação está afixado para um cluster, não há uma maior possibilidade de conseguir encontrar recursos livres, uma vez que o agrupamento de recursos disponíveis é menor. Além disso, se o pedido de alocação está afixado para um cluster, mas o tipo de recurso pedido não é suportado nesse cluster, o pedido irá falhar mesmo se o cluster tem de libertar recursos. O 3 diagrama seguinte ilustra o caso em que uma alocação afixada falha porque o cluster de candidatos só tem de libertar recursos. Diagrama 4 ilustra as maiúsculas e minúsculas onde uma alocação afixada falha porque o cluster de candidatos apenas não suporta o tamanho da VM pedido, apesar do cluster tem de libertar recursos.

![Falha na alocação afixado](./media/virtual-machines-common-allocation-failure/Allocation2.png)



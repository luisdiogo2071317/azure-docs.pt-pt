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
ms.openlocfilehash: f403e060859df6d1de96a3c0d478d57df2677eee
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "31531060"
---
Seguem-se cenários comuns de alocação que fazer com que um pedido de alocação para ser afixado. Iremos irá aprofundar cada cenário neste artigo.

- Redimensionar uma VM ou adicionar VMs ou instâncias de função a um serviço em nuvem existente
- Reinício parcialmente parada (desalocadas) VMs
- Reinício totalmente parada (desalocadas) VMs
- Implementações de teste e produção (plataforma como serviço apenas)
- Grupo de afinidade (VM ou proximidade do serviço)
- Rede virtual com base em afinidade – grupo

Quando receber um erro de alocação, verifique se qualquer um dos cenários listados aplicam-se o erro. Utilize o erro de alocação que é devolvido pela plataforma do Azure para identificar o cenário correspondente. Se o pedido está afixado, remova algumas das restrições afixação para abrir o seu pedido para clusters mais, deste modo, aumentar a probabilidade de sucesso de alocação.
Em geral, se o erro de estado de que "o tamanho da VM pedido não é suportado", pode sempre tentar novamente mais tarde. Isto acontece porque existem recursos suficientes podem ter sido libertados do cluster para acomodar o seu pedido. Se o problema é que o tamanho da VM pedido não é suportado, experimente um tamanho VM diferente. Caso contrário, a única opção é remover a restrição de afixação.

Dois cenários comuns de falha estão relacionados com grupos de afinidades. No passado, um grupo de afinidades foi utilizado para fornecer próximos VMs e instâncias de serviço ou foi utilizada para permitir a criação de uma rede virtual. Com a introdução das redes virtuais regionais, grupos de afinidades já não são necessários para criar uma rede virtual. Com a redução de latência de rede numa infraestrutura do Azure, a recomendação para utilizar grupos de afinidade para VMs ou proximidade do serviço foi alterada.

O diagrama a seguir apresenta taxonomia dos cenários de atribuição (afixado). 

![Alocação afixado taxonomia](./media/virtual-machines-common-allocation-failure/Allocation3.png)

## <a name="resize-a-vm-or-add-vms-or-role-instances-to-an-existing-cloud-service"></a>Redimensionar uma VM ou adicionar VMs ou instâncias de função a um serviço em nuvem existente
**Erro**

Upgrade_VMSizeNotSupported ou GeneralError

**Causa desta afixação de cluster**

Um pedido para redimensionar uma VM ou adicionar uma VM ou uma instância de função a um serviço em nuvem existente tem de ser tentada no cluster original que aloja o serviço em nuvem existente. Criar um novo serviço em nuvem permite que a plataforma do Azure localizar outro cluster que tem de libertar recursos ou suporta o tamanho da VM que pediu.

**Solução**

Se o erro é Upgrade_VMSizeNotSupported *, experimente um tamanho VM diferente. Se utilizar um tamanho VM diferente não é uma opção, mas é aceitável para utilizar um endereço IP virtual (VIP) diferentes, crie um novo serviço em nuvem para alojar a nova VM e adicionar o novo serviço em nuvem para a rede virtual regional onde estiver a executar as VMs existentes. Se o serviço em nuvem existente não utilizar uma rede virtual regional, pode ainda criar uma nova rede virtual para o novo serviço de nuvem e, em seguida, ligar os [rede virtual existente para a nova rede virtual](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Ver mais informações sobre [redes virtuais regionais](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Se o erro GeneralError *, é provável que o tipo de recurso (por exemplo, um determinado tamanho da VM) é suportado pelo cluster, mas o cluster não tem recursos gratuitos neste momento. É semelhante ao cenário acima, adicione o recurso de computação pretendido através da criação de um novo serviço de nuvem (tenha em atenção que o novo serviço de nuvem tem de utilizar um VIP diferente) e utilizar uma rede virtual regional para ligar os seus serviços em nuvem.

## <a name="restart-partially-stopped-deallocated-vms"></a>Reinício parcialmente parada (desalocadas) VMs
**Erro**

GeneralError *

**Causa desta afixação de cluster**

Desalocação parcial significa que parada (desalocadas) uma ou mais, mas não todos, VMs num serviço em nuvem. Quando parar (desalocar) uma VM, os recursos associados são lançados. Reiniciar essa VM parada (desalocada), por conseguinte, é um novo pedido de alocação. Reinício de VMs num serviço em nuvem parcialmente desalocada é equivalente a adição de VMs para um serviço em nuvem existente. O pedido de atribuição tem de ser tentada no cluster original que aloja o serviço em nuvem existente. Criação de um serviço de nuvem diferente, permite que a plataforma do Azure localizar outro cluster que possui o recurso gratuito ou suporta o tamanho da VM que pediu.

**Solução**

Se é aceitável para utilizar um VIP diferente, eliminar as VMs paradas (desalocadas) (mas manter os discos associados) e adicione as VMs através de um serviço em nuvem diferente. Utilize uma rede virtual regional para ligar os serviços de cloud:

* Se o serviço em nuvem existente utiliza uma rede virtual regional, adicione simplesmente o novo serviço em nuvem para a mesma rede virtual.
* Se o serviço em nuvem existente não utiliza uma rede virtual regional, crie uma nova rede virtual para o novo serviço de nuvem e, em seguida, [ligar a sua rede virtual existente para a nova rede virtual](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Ver mais informações sobre [redes virtuais regionais](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

## <a name="restart-fully-stopped-deallocated-vms"></a>Reinício totalmente parada (desalocadas) VMs
**Erro**

GeneralError *

**Causa desta afixação de cluster**

Meios de Desalocação completo que deixaram (desalocado) todas as VMs de um serviço em nuvem. Os pedidos de alocação para reiniciar estes VMs tem de ser tentada no cluster original que aloja o serviço em nuvem. Criar um novo serviço em nuvem permite que a plataforma do Azure localizar outro cluster que tem de libertar recursos ou suporta o tamanho da VM que pediu.

**Solução**

Se é aceitável para utilizar um VIP diferente, elimine as originais paradas (desalocadas) as VMs (, mas manter os discos associados) e eliminar o serviço de nuvem correspondente (os recursos de computação associado já foram lançados quando é parada (desalocada) as VMs). Crie um novo serviço em nuvem para adicionar as VMs novamente.

## <a name="stagingproduction-deployments-platform-as-a-service-only"></a>Implementações de teste/produção (plataforma como serviço apenas)
**Erro**

New_General * ou New_VMSizeNotSupported *

**Causa desta afixação de cluster**

A implementação de teste e a implementação de produção de um serviço em nuvem estão alojados no mesmo cluster. Ao adicionar a segunda implementação, o pedido de atribuição correspondente será tentado no mesmo cluster que aloja a primeira implementação.

**Solução**

Elimine a primeira implementação e o serviço em nuvem original e reimplemente o serviço de nuvem. Esta ação pode encaminhado para a primeira implementação de um cluster tem recursos livres suficientes para se ajustar ambas as implementações ou num cluster que suporta os tamanhos VM que pediu.

## <a name="affinity-group-vmservice-proximity"></a>Grupo de afinidade (proximidade VM/serviço)
**Erro**

New_General * ou New_VMSizeNotSupported *

**Causa desta afixação de cluster**

Nenhuma computação recursos atribuídos a um grupo de afinidade está associado a um cluster. Os pedidos de novos recursos de computação em que o grupo de afinidade estão tentadas no mesmo cluster onde estão alojados os recursos existentes. Isto acontece se os novos recursos são criados através de um novo serviço em nuvem ou através de um serviço em nuvem existente.

**Solução**

Se um grupo de afinidade não é necessário, não utilize um grupo de afinidade ou agrupar os recursos de computação em vários grupos de afinidade.

## <a name="affinity-group-based-virtual-network"></a>Baseado em grupo de afinidade de rede virtual
**Erro**

New_General * ou New_VMSizeNotSupported *

**Causa desta afixação de cluster**

Antes de redes virtuais regionais foram introduzidas, era necessário para associar uma rede virtual um grupo de afinidade. Como resultado, computação recursos colocados num grupo de afinidades estão vinculados pelas mesmas restrições conforme descrito no "cenário de alocação: grupo de afinidade (proximidade VM/serviço)" secção acima. Os recursos de computação estão associados a um cluster.

**Solução**

Se não precisar de um grupo de afinidade, crie uma nova rede virtual regional para os novos recursos que está a adicionar, e, em seguida, [ligar a sua rede virtual existente para a nova rede virtual](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Ver mais informações sobre [redes virtuais regionais](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Em alternativa, pode [migrar a rede virtual baseado em grupo de afinidade para uma rede virtual regional](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/)e, em seguida, adicione novamente os recursos pretendidos.

---
title: "Ligações de saída no Azure | Microsoft Docs"
description: "Este artigo explica como o Azure permite VMs a comunicar com serviços de internet pública."
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
ms.assetid: 5f666f2a-3a63-405a-abcd-b2e34d40e001
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/05/2018
ms.author: kumud
ms.openlocfilehash: 1c776d94d217622186d880352c518ad5a34b0949
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="outbound-connections-in-azure"></a>Ligações de saída no Azure

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]


O Azure oferece a conectividade de saída para implementações de cliente através de vários mecanismos diferentes. Este artigo descreve quais são os cenários, quando estas são aplicadas, como funcionam e como geri-los.

Uma implementação no Azure pode comunicar com pontos finais fora do Azure no espaço de endereço IP público. Quando inicia a uma instância de um fluxo de saída para um destino no espaço de endereço IP público, o Azure mapeia dinamicamente o endereço IP privado para um endereço IP público. Após a criação desta mapeamento, tráfego de retorno deste fluxo originated saída também pode aceder ao endereço IP privado onde o fluxo foi originado.

Azure utiliza a tradução de endereços de rede de origem (realizar o SNAT) para executar esta função. Quando vários endereços IP privados são masquerading atrás de um único endereço IP público, o Azure utiliza [tradução de endereços (TERESA) de porta](#pat) para representação endereços IP privados. Portas efémeras são utilizadas para TERESA e são [preallocated](#preallocatedports) com base no tamanho do conjunto.

Existem vários [cenários saídos](#scenarios). Pode combinar estes cenários, conforme necessário. Revê-los cuidadosamente de forma a compreender as capacidades, restrições e padrões de acordo com o modelo de implementação e o cenário de aplicação. Reveja as orientações para [gerir estes cenários](#snatexhaust).

## <a name="scenarios"></a>Descrição geral do cenário

O Azure tem dois modelos de implementação principais: o Azure Resource Manager e clássico. Balanceador de carga do Azure e os recursos relacionados são explicitamente definidos quando estiver a utilizar [do Azure Resource Manager](#arm). Implementações clássicas separar o conceito de um balanceador de carga e rápida uma função semelhante através da definição de pontos finais de um [serviço em nuvem](#classic). O aplicável [cenários](#scenarios) para a implementação dependem o modelo de implementação que utiliza.

### <a name="arm"></a>O Azure Resource Manager

O Azure oferece atualmente três métodos diferentes para atingir a conectividade de saída para os recursos do Azure Resource Manager. [Clássico](#classic) as implementações tiverem um subconjunto destes cenários.

| Cenário | Método | Descrição |
| --- | --- | --- |
| [1. VM com um endereço IP público de nível de instância (com ou sem Balanceador de carga)](#ilpip) | Realizar o SNAT, porta masquerading não utilizado |Azure utiliza o IP público atribuído à configuração de IP da NIC de. a instância A instância tem todas as portas efémeras disponíveis. |
| [2. Balanceador de carga público associado uma VM (nenhum endereço IP público de nível de instância na instância)](#lb) | Realizar o SNAT com porta masquerading (PAT) utilizando o frontends de Balanceador de carga |Azure partilha o endereço IP público de frontends de Balanceador de carga público com vários endereços IP privados. Azure utiliza portas efémeras dos frontends a TERESA. |
| [3. VM autónoma (nenhum Balanceador de carga, nenhum endereço IP público de nível de instância)](#defaultsnat) | Realizar o SNAT com porta masquerading (PAT) | Azure automaticamente designa um endereço IP público para realizar o SNAT, partilhe este endereço IP público com vários endereços IP privados do conjunto de disponibilidade e utiliza portas efémeras deste endereço IP público. Este é um cenário de contingência para os cenários anteriores. Não recomendamos-lo se precisar de visibilidade e controlo. |

Se não pretender que uma VM para comunicar com pontos finais fora do Azure no espaço de endereços IP públicos, pode utilizar grupos de segurança de rede (NSGs) para bloquear o acesso conforme necessário. A secção [impedir a conectividade de saída](#preventoutbound) aborda os NSGs em mais detalhe. Orientações sobre a conceber, implementar e gerir uma rede virtual sem acesso de saída estão fora do âmbito deste artigo.

### <a name="classic"></a>Clássica (Serviços de nuvem)

Os cenários disponíveis para implementações clássicas são um subconjunto dos cenários disponíveis para [do Azure Resource Manager](#arm) implementações e básico de Balanceador de carga.

Uma máquina virtual clássica tem os mesmos três cenários fundamentais, conforme descrito para recursos do Azure Resource Manager ([1](#ilpip), [2](#lb), [3](#defaultsnat)). Uma função de trabalho web clássico tem apenas dois cenários ([2](#lb), [3](#defaultsnat)). [Estratégias de mitigação](#snatexhaust) também têm as mesmas diferenças.

O algoritmo utilizado para [preallocating portas efémeras](#ephemeralprots) para TERESA para implementações clássicas é igual para implementações de recursos do Azure Resource Manager.  

### <a name="ilpip"></a>Cenário 1: VM com um endereço IP público de nível de instância

Neste cenário, a VM possui uma instância do nível pública IP (ILPIP) atribuída. Como são preocupações ligações de saída, é irrelevante se a VM está ou não de balanceamento de carga. Este cenário tem precedência sobre as outras. Quando é utilizado um ILPIP, a VM utiliza o ILPIP para todos os fluxos de saída.  

Não é utilizada a porta masquerading (TERESA) e a VM tem todas as portas efémeras disponíveis para utilização.

Se muitos fluxos de saída inicia a sua aplicação e experiência de esgotamento de porta de realizar o SNAT, considere atribuir um [ILPIP para mitigar restrições de realizar o SNAT](#assignilpip). Reveja [esgotamento de realizar o SNAT gerir](#snatexhaust) na íntegra.

### <a name="lb"></a>Cenário 2: VM de balanceamento de carga sem um endereço IP público de nível de instância

Neste cenário, a VM faz parte de um conjunto de back-end de Balanceador de carga público. A VM não tem um endereço IP público atribuído. O recurso de Balanceador de carga tem de ser configurado com uma regra de Balanceador de carga para criar uma ligação entre o público front-end IP com o conjunto de back-end. 

Se não concluir esta configuração de regra, o comportamento é conforme descrito no cenário para [autónoma de VM com nenhuma IP público de nível de instância](#defaultsnat). Não é necessário para a regra ter um serviço de escuta de trabalho no conjunto de back-end para a sonda de estado de funcionamento com êxito.

Quando a VM de balanceamento de carga cria um fluxo de saída, Azure traduz o endereço IP de origem privada do fluxo de saída para o endereço IP público do front-end de Balanceador de carga público. Azure utiliza realizar o SNAT para executar esta função. Também utiliza o Azure [PAT](#pat) para masquerade vários endereços IP privados atrás de um endereço IP público. 

Portas efémeras de frontend de endereço IP público do Balanceador de carga são utilizadas para distinguir fluxos individuais teve originados pela VM. Realizar o SNAT dinamicamente utiliza [preallocated portas efémeras](#preallocatedports) quando são criados os fluxos de saída. Neste contexto, as portas efémeras utilizadas para realizar o SNAT são designadas por portas de realizar o SNAT.

Portas de realizar o SNAT são preallocated conforme descrito no [compreender realizar o SNAT e TERESA](#snat) secção. Se estiver a um recurso finito que pode ser esgotado. É importante compreender a forma como são [consumido](#pat). Para compreender como estruturar para este consumo e mitigar conforme necessário, reveja [esgotamento de realizar o SNAT gerir](#snatexhaust).

Quando [vários endereços IP (públicos) associados básico de Balanceador de carga](load-balancer-multivip-overview.md), qualquer estes IP público endereços são um [candidato para fluxos de saída](#multivipsnat), e uma está selecionada.  

Para monitorizar o estado de funcionamento de ligações de saída com o Balanceador de carga básico, pode utilizar [análise de registos para o Balanceador de carga](load-balancer-monitor-log.md) e [registos de eventos de alertas](load-balancer-monitor-log.md#alert-event-log) para monitorizar as mensagens de esgotamento de porta de realizar o SNAT.

### <a name="defaultsnat"></a>Cenário 3: Autónomo VM sem um endereço IP público de nível de instância

Neste cenário, a VM não faz parte de um conjunto de Balanceador de carga do Azure e não tem um endereço ILPIP atribuído ao mesmo. Quando a VM cria um fluxo de saída, Azure traduz o endereço IP de origem privada do fluxo de saída para um endereço IP público de origem. O endereço IP público utilizado para este fluxo de saída não é configurável e não count contra o limite de recurso IP público da subscrição. 

Azure utiliza realizar o SNAT com porta masquerading ([PAT](#pat)) para executar esta função. Este cenário é semelhante à [cenário 2](#lb), mas não há nenhum controlo sobre o endereço IP utilizado. Este é um cenário de contingência para quando não existem cenários 1 e 2. Não recomendamos este cenário se pretender que o controlo sobre o endereço de saída.

Portas de realizar o SNAT são preallocated conforme descrito no [compreender realizar o SNAT e TERESA](#snat) secção. Se estiver a um recurso finito que pode ser esgotado. É importante compreender a forma como são [consumido](#pat). Para compreender como estruturar para este consumo e mitigar conforme necessário, reveja [esgotamento de realizar o SNAT gerir](#snatexhaust).

### <a name="combinations"></a>Vários cenários combinados

Pode combinar os cenários descritos nas secções anteriores para obter um resultado específico. Quando existem vários cenários, uma ordem de precedência de aplica-se: [cenário 1](#ilpip) tem precedência sobre [cenário 2](#lb) e [3](#defaultsnat) (apenas no Azure Resource Manager). [Cenário 2](#lb) substitui [cenário 3](#defaultsnat) (Azure Resource Manager e clássico).

Um exemplo é uma implementação do Gestor de recursos do Azure onde a aplicação depende descontos elevados ligações de saída para um número limitado de destinos, mas também recebe fluxos entrados através de um front-end de Balanceador de carga. Neste caso, pode combinar cenários 1 e 2 para relief. Para padrões adicionais, consulte [esgotamento de realizar o SNAT gerir](#snatexhaust).

### <a name="multivipsnat"></a> Vários frontends para fluxos de saída

Balanceador de carga básico escolhe um front-end único a ser utilizada para fluxos de saída quando [vários frontends IP (público)](load-balancer-multivip-overview.md) são candidatos para fluxos de saída. Esta seleção não é configurável e deverá considerar o algoritmo de seleção para ser aleatório. Pode designar um endereço IP específico para fluxos de saída, conforme descrito em [combinadas de vários cenários](#combinations).

## <a name="snat"></a>Compreender a realizar o SNAT e TERESA

### <a name="pat"></a>Porta masquerading realizar o SNAT (TERESA)

Quando um recurso de Balanceador de carga público está associado com instâncias VM, cada origem de ligação de saída é foi reescrita. A origem é rescrita do espaço de endereços IP privado da rede virtual para o endereço IP público do load balancer de front-end. No espaço de endereço IP público, 5-cadeias de identificação do fluxo (endereço IP de origem, porta de origem, o protocolo de transporte IP, endereço IP de destino, porta de destino) tem de ser exclusivo.  

Portas efémeras (portas de realizar o SNAT) são utilizadas para atingir esse objetivo após a conversão de endereço IP de origem privada, porque vários fluxos provenientes de um único endereço IP público. 

Uma porta de realizar o SNAT é consumida por fluxo para um endereço IP de destino único, a porta e protocolo. Para vários fluxos para o mesmo endereço IP de destino, a porta e protocolo, cada fluxo consome uma única porta de realizar o SNAT. Isto garante que os fluxos são exclusivos quando provenientes do mesmo endereço IP público e vá para o mesmo endereço IP de destino, porta e protocolo. 

Vários fluxos, cada um para um endereço IP de destino diferente, a porta e protocolo, partilham uma única porta de realizar o SNAT. O endereço IP de destino, porta e protocolo tornar fluxos exclusivo sem a necessidade de origem adicionais portas para distinguir fluxos no espaço de endereço IP público.

Quando os recursos de porta de realizar o SNAT ficam esgotados, fluxos de saída falharem até que as portas de realizar o SNAT fluxos existentes da versão. Balanceador de carga reclama portas realizar o SNAT quando o fluxo fecha e utiliza um [tempo limite de inatividade de 4 minutos](#idletimeout) para realizar o SNAT as portas de fluxos de inatividade de reclamação.

Padrões mitigar condições que normalmente levar ao esgotamento de porta de realizar o SNAT, reveja o [gerir realizar o SNAT](#snatexhaust) secção.

### <a name="preallocatedports"></a>Preallocation de portas efémeras para a porta masquerading realizar o SNAT (TERESA)

Realizar o SNAT masquerading Azure utiliza um algoritmo para determinar o número de preallocated realizar o SNAT portas disponíveis com base no tamanho do conjunto back-end ao utilizar a porta ([PAT](#pat)). Portas de realizar o SNAT são portas efémeras disponíveis para um determinado endereço de origem IP público.

Azure preallocates realizar o SNAT portas para a configuração de IP da NIC de cada VM. Quando uma configuração de IP é adicionada ao conjunto, as portas de realizar o SNAT são preallocated para esta configuração de IP com base no tamanho de conjunto back-end. Para as funções de trabalho web clássico, a alocação é por instância de função. Quando são criados os fluxos de saída, [PAT](#pat) dinamicamente consome (até ao limite preallocated) e liberta estas portas quando fecha o fluxo ou [tempos limite de inatividade](#ideltimeout) acontecer.

A tabela seguinte mostra os preallocations de porta de realizar o SNAT para as camadas de tamanhos de conjunto de back-end:

| Tamanho do conjunto (instâncias de VM) | Portas de realizar o SNAT preallocated por configuração de IP|
| --- | --- |
| 1-50 | 1,024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1,000 | 32 |

Lembre-se de que o número de portas de realizar o SNAT disponíveis traduz diretamente para o número de fluxos. Uma única porta de realizar o SNAT pode ser reutilizada vários destinos exclusivo. As portas são consumidas apenas se for necessário para disponibilizar fluxos exclusivo. Para obter orientações de conceção e a respetiva mitigação, consulte a secção sobre [como gerir este recurso exhaustible](#snatexhaust) e a secção que descreve [PAT](#pat).

Alteração do tamanho do seu conjunto de back-end pode afetar alguns dos seus fluxos estabelecidos. Se o tamanho do conjunto back-end aumenta e passa para a próxima camada, metade das suas portas de realizar o SNAT preallocated são recuperados durante a transição para a próxima camada superior do conjunto back-end. Fluxos que estão associados uma porta de realizar o SNAT reclaimed serão o tempo limite e tem de ser restabelecer. Se um novo fluxo for tentado, o fluxo será bem sucedida imediatamente, desde que existem portas preallocated disponíveis.

Se o tamanho do conjunto back-end diminui e passa para um escalão inferior, aumenta o número de portas de realizar o SNAT disponíveis. Neste caso, existentes alocado realizar o SNAT portas e os seus respetivos fluxos não são afetados.

## <a name="snatexhaust"></a>Gestão de esgotamento de porta de realizar o SNAT (TERESA)

[Portas efémeras](#preallocatedports) utilizado para [PAT](#pat) são um recurso exhaustible, conforme descrito em [VM autónoma, sem um endereço IP público de nível de instância](#defaultsnat) e [VM de balanceamento de carga sem um Endereço IP público de nível de instância](#lb).

Se souber que está a iniciar muitas TCP ou UDP ligações de saída para o mesmo endereço IP de destino e a porta e observe a efetuar ligações de saída ou aconselhado pelo suporte que está a esgotar os portas de realizar o SNAT (preallocated [efémeras portas](#preallocatedports) utilizado pelo [PAT](#pat)), tem várias opções de mitigação geral. Reveja estas opções e decidir o que está disponível e melhor para o seu cenário. É possível que um ou mais podem ajudar a gerir este cenário.

Se estiver a ter problemas em compreender o comportamento de ligação de saída, pode utilizar as estatísticas de pilha IP (netstat). Ou pode ser útil observar comportamentos de ligação através da utilização de capturas de pacotes. Pode efetuar estas capturas de pacotes no SO convidado da sua instância ou utilizar [observador de rede de captura de pacotes](../network-watcher/network-watcher-packet-capture-manage-portal.md).

### <a name="connectionreuse"></a>Modificar a aplicação reutilizar ligações 
Pode reduzir a pedido para portas efémeras que são utilizados para realizar o SNAT por reutilizar as ligações na sua aplicação. Isto é particularmente verdadeiro protocolos, como HTTP/1.1, onde a reutilização de ligação é a predefinição. E outros protocolos que utilizam HTTP como transporte (por exemplo, REST) podem beneficiar por sua vez. 

Reutilização é sempre melhor do que individuais, atómicas ligações de TCP para cada pedido. Reutilize resulta em mais performant, transações de TCP muito eficientes.

### <a name="connection pooling"></a>Modificar a aplicação para utilizar o agrupamento de ligação
Pode utilizar uma ligação de agrupamento de esquema na sua aplicação, onde os pedidos internamente estão distribuídos por um conjunto de ligações (cada reutilizar sempre que possível) fixo. Este esquema restringe o número de portas efémeras em utilização e cria um ambiente mais previsível. Este esquema também pode aumentar o débito de pedidos ao permitir várias operações simultâneas, quando uma ligação única está a bloquear a resposta de uma operação.  

Agrupamento de ligação poderá já existir dentro do framework que está a utilizar para desenvolver a sua aplicação ou as definições de configuração para a sua aplicação. Pode combinar o agrupamento de ligações com reutilização de ligação. Os pedidos de vários consumam, em seguida, um número fixo e previsível de portas para o mesmo endereço IP de destino e a porta. Os pedidos também beneficiam da utilização eficiente de transações de TCP, reduzindo a utilização de recursos e latência. Transações de UDP também podem beneficiar, porque a gerir o número de fluxos UDP pode evitar a esgotar os condições e gerir a utilização de porta de realizar o SNAT por sua vez.

### <a name="retry logic"></a>Modificar a aplicação para utilizar menos agressiva lógica de repetição
Quando [preallocated portas efémeras](#preallocatedports) utilizado para [PAT](#pat) estão esgotadas, ou aplicação ocorram falhas, agressiva ou força bruta repete sem decay e término lógica causar esgotamento a ocorrer ou manter. Pode reduzir a pedido para portas efémeras utilizando uma menor agressiva lógica de repetição. 

Portas efémeras tem um minuto 4 tempo limite de inatividade (não ajustável). Se as repetições são demasiado agressiva, o esgotamento não tem nenhuma oportunidade de limpar no seu próprio. Por conseguinte, considerar como – e com que frequência – a aplicação repete transações é uma parte fundamental da estrutura.

### <a name="assignilpip"></a>Atribuir um IP público ao nível de instância para cada VM
Atribuir um ILPIP altera o seu cenário para [IP público de nível de instância para uma VM](#ilpip). Todas as portas efémeras do IP público que são utilizadas para cada VM estão disponíveis para a VM. (Por oposição cenários em que as portas efémeras de um IP público são partilhadas com todas as VMs associado o conjunto de back-end respetivos.) Existem trade-offs a ter em consideração, como o custo adicional de endereços IP públicos e o impacto potencial da branca um grande número de endereços IP individuais.

>[!NOTE] 
>Esta opção não está disponível para as funções de trabalho web.

### <a name="idletimeout"></a>Utilizar keepalives para repor o tempo limite inativo da saída

Ligações de saída têm um tempo de limite de inatividade de 4 minutos. Não é ajustável este tempo limite. No entanto, pode utilizar o transporte (por exemplo, o TCP keepalives) ou keepalives de camada de aplicação para atualizar um fluxo de inatividade e repor este tempo limite de inatividade, se necessário.

## <a name="discoveroutbound"></a>Detetar o IP público que utiliza uma VM
Existem várias formas para determinar o endereço IP de origem público de uma ligação de saída. OpenDNS fornece um serviço que pode apresentar-lhe o endereço IP público da sua VM. 

Ao utilizar o comando nslookup, pode enviar uma consulta DNS para o myip.opendns.com de nome para a resolução de OpenDNS. O serviço devolve o endereço IP de origem que foi utilizado para enviar a consulta. Quando executar a consulta seguinte da sua VM, a resposta é o IP público utilizado relativamente a essa VM:

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventoutbound"></a>Impedir a conectividade de saída
Por vezes, não é desejável para uma VM para a permissão para criar um fluxo de saída. Pode existir um requisito para gerir os destinos que podem ser acedidos com fluxos de saída ou destinos de que podem começar a fluxos de entrada. Neste caso, pode utilizar [grupos de segurança de rede](../virtual-network/virtual-networks-nsg.md) para gerir os destinos em que a VM pode aceder. Também pode utilizar os NSGs para gerir que destino público pode iniciar os fluxos de entrada. 

Ao aplicar um NSG a uma VM com balanceamento de carga, preste atenção para o [predefinido etiquetas](../virtual-network/virtual-networks-nsg.md#default-tags) e [predefinido regras](../virtual-network/virtual-networks-nsg.md#default-rules). Tem de se certificar de que a VM pode receber pedidos de sonda de estado de funcionamento do Balanceador de carga do Azure. 

Se um NSG bloquear pedidos de sonda de estado de funcionamento da etiqueta predefinida AZURE_LOADBALANCER, a pesquisa de estado de funcionamento da VM falha e a VM está marcado como. Balanceador de carga para a enviar de novos fluxos para essa VM.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [básico de Balanceador de carga](load-balancer-overview.md).
- Saiba mais sobre [grupos de segurança de rede](../virtual-network/virtual-networks-nsg.md).
- Saiba mais sobre algumas da outra chave [capacidades de rede](../networking/networking-overview.md) no Azure.

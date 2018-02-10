---
title: "Noções sobre ligações de saída no Azure | Microsoft Docs"
description: "Este artigo explica como o Azure permite VMs a comunicar com serviços de Internet pública."
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
ms.openlocfilehash: e1a2b4c281194ec4c70e4d9fe1bc97c5aa61436e
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="understanding-outbound-connections-in-azure"></a>Compreender as ligações de saída no Azure

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]


O Azure oferece a conectividade de saída para implementações de cliente através de vários mecanismos diferentes.  Este artigo descreve quais são os cenários, quando estas são aplicadas, como funcionam e como geri-los.

Uma implementação no Azure pode comunicar com pontos finais fora do Azure no espaço de endereços IP públicos. Quando inicia a uma instância de um fluxo de saída para um destino no espaço de endereço IP público, o Azure mapeia dinamicamente o endereço IP privado para um endereço IP público.  Quando tiver sido criado desta mapeamento, tráfego de retorno deste fluxo originated saída também pode aceder ao endereço IP privado onde o fluxo foi originado.

Azure utiliza a tradução de endereços de rede de origem (realizar o SNAT) para executar esta função.  Quando vários endereços IP privados são masquerading atrás de um único endereço IP público, o Azure utiliza [tradução de endereços (TERESA) de porta](#pat) para representação endereços IP privados.  Portas efémeras são utilizadas para TERESA e são [preallocated](#preallocatedports) com base no tamanho do conjunto.

Existem vários [cenários saídos](#scenarios). Estes cenários podem ser combinados, conforme necessário. Revê-los cuidadosamente de forma a compreender as capacidades, restrições e padrões de acordo com o modelo de implementação e o cenário de aplicação.  Reveja as orientações para [gerir estes cenários](#snatexhaust).

## <a name="scenarios"></a>Descrição geral do cenário

O Azure tem dois modelos de implementação principais: o Azure Resource Manager e clássico. Balanceador de carga e de recursos relacionados explicitamente definidos quando utilizar [recursos do Azure Resource Manager](#arm).  Implementações clássicas separar o conceito de um balanceador de carga e rápida uma função semelhante através da definição de pontos finais de um [serviço em nuvem](#classic). O aplicável [cenários](#scenarios) para a sua implementação dependente na implementação de que o modelo é utilizado.

### <a name="arm"></a>O Azure Resource Manager

O Azure oferece atualmente três métodos diferentes para atingir a conectividade de saída para os recursos do Azure Resource Manager.  [Clássico](#classic) as implementações tiverem um subconjunto destes cenários.

| Cenário | Método | Descrição |
| --- | --- | --- |
| [1. VM com o endereço IP público de nível de instância (com ou sem Balanceador de carga)](#ilpip) | Realizar o SNAT, porta masquerading não utilizado |Azure utiliza o IP público atribuído à configuração de IP da NIC de. a instância  A instância tem todas as portas efémeras disponíveis. |
| [2. Balanceador de carga público associado à VM (nenhum endereço IP público de nível de instância numa instância)](#lb) | Realizar o SNAT com porta masquerading (PAT) utilizando o frontend(s) de Balanceador de carga |O endereço IP público do Balanceador de carga público, frontend(s) com vários IP privado endereços e utiliza as portas efémeras dos frontend(s) a TERESA partilha o Azure. |
| [3. VM autónoma (nenhum Balanceador de carga, nenhum endereço IP público de nível de instância)](#defaultsnat) | Realizar o SNAT com porta masquerading (PAT) | Azure designa um endereço IP público para realizar o SNAT, partilhe este endereço IP público com vários endereços IP privados do conjunto de disponibilidade e utiliza automaticamente as portas efémeras deste endereço IP público.  Este cenário é um cenário de contingência para os cenários que precede 1 e 2 e não recomendado se precisa de visibilidade e controlo. |

Se não pretender que uma VM para comunicar com pontos finais fora do Azure no espaço de endereços IP públicos, pode utilizar grupos de segurança de rede (NSG) para bloquear o acesso conforme necessário.  Utilizar NSGs é abordada mais detalhadamente na [impedir a conectividade de saída](#preventoutbound).  Detalhes de implementação e a documentação de orientação de design para saber como conceber e gerir uma rede Virtual sem acesso de saída estão fora do âmbito deste artigo.

### <a name="classic"></a>Clássica (Serviços de nuvem)

Os cenários disponíveis para implementações clássicas são um subconjunto dos cenários disponíveis para [do Azure Resource Manager](#arm) implementações e básico de Balanceador de carga.

Uma Máquina Virtual clássico tem os mesmos três cenários fundamentais, conforme descrito para recursos do Azure Resource Manager ([1](#ilpip), [2](#lb), [3](#defaultsnat)). Uma função de trabalho Web clássico tem apenas dois cenários ([2](#lb), [3](#defaultsnat)).  [Estratégias de mitigação](#snatexhaust) também têm as mesmas diferenças.

O algoritmo utilizado para [preallocating portas efémeras](#ephemeralprots) para TERESA para implementações clássicas é igual para implementações de recursos do Azure Resource Manager.  

### <a name="ilpip"></a>Cenário 1: VM com um endereço IP público de nível de instância

Neste cenário, a VM possui uma instância do nível pública IP (ILPIP) atribuída. Como são preocupações ligações de saída, é irrelevante se a VM está ou não de balanceamento de carga.  Este cenário tem precedência sobre as outras. Quando é utilizado um ILPIP, a VM utiliza o ILPIP para todos os fluxos de saída.  

Não é utilizada a porta masquerading (TERESA) e a VM tem todas as portas efémeras disponíveis para utilização.

Se muitos fluxos de saída inicia a sua aplicação e experiência de esgotamento de porta de realizar o SNAT, pode considerar a atribuir um [ILPIP para mitigar restrições de realizar o SNAT](#assignilpip). Reveja [esgotamento de realizar o SNAT gerir](#snatexhaust) -íntegra.

### <a name="lb"></a>Cenário 2: VM de balanceamento de carga sem o endereço IP público de nível de instância

Neste cenário, a VM faz parte de um conjunto de back-end de Balanceador de carga público.  A VM não tem um endereço IP público atribuído. O recurso de Balanceador de carga tem de ser configurado com uma regra de Balanceador de carga para criar uma ligação entre o público front-end IP com o conjunto de back-end. Se não concluir esta configuração de regra, o comportamento é conforme descrito no cenário para [autónoma de VM com nenhuma IP público de nível de instância](#defaultsnat).  Não é necessário para a regra a ter um serviço de escuta trabalho no conjunto de back-end ou a pesquisa de estado de funcionamento com êxito.

Quando a VM de balanceamento de carga cria um fluxo de saída, Azure traduz o endereço IP de origem privada do fluxo de saída para o endereço IP público do front-end de Balanceador de carga público. Azure utiliza origem rede endereço tradução (realizar o SNAT) para executar esta função, bem como [tradução de endereços (TERESA) de porta](#pat) para masquerade vários endereços IP privados atrás de um endereço IP público. Portas efémeras de frontend de endereço IP público do Balanceador de carga são utilizadas para distinguir fluxos individuais teve originados pela VM. Realizar o SNAT dinamicamente utiliza [preallocated portas efémeras](#preallocatedports) quando são criados os fluxos de saída. Neste contexto, as portas efémeras utilizadas para realizar o SNAT são referidas como portas de realizar o SNAT.

Portas de realizar o SNAT são preallocated conforme descrito no [compreender realizar o SNAT e TERESA](#snat) secção e são um recurso finito que pode ser esgotado. É importante compreender a forma como são [consumido](#pat). Reveja [esgotamento de realizar o SNAT gerir](#snatexhaust) para compreender como estruturar para e mitigar conforme necessário.

Quando [vários endereços IP (públicos) associados a um balanceador de carga básico](load-balancer-multivip-overview.md), qualquer estes IP público endereços são um [candidato para fluxos de saída e o outro está selecionado](#multivipsnat).  

Pode utilizar [análise de registos para o Balanceador de carga](load-balancer-monitor-log.md) e [mensagens de esgotamento de porta de registos de eventos de alerta para monitor para realizar o SNAT](load-balancer-monitor-log.md#alert-event-log) para monitorizar o estado de funcionamento de ligações de saída com básico de Balanceador de carga.

### <a name="defaultsnat"></a>Cenário 3: Autónomo VM sem o endereço IP público de nível de instância

A VM não faz parte de um conjunto de Balanceador de carga do Azure e não tem um endereço de IP de público ao nível do instância (ILPIP) atribuído ao mesmo. Quando a VM cria um fluxo de saída, Azure traduz o endereço IP de origem privada do fluxo de saída para um endereço IP público de origem. O endereço IP público utilizado para este fluxo de saída não é configurável e não count contra o limite de recurso IP público da subscrição. Azure utiliza origem rede endereço tradução (realizar o SNAT) com a porta masquerading ([PAT](#pat)) para executar esta função. Este cenário é semelhante à [cenário 2](#lb), mas não há nenhum controlo sobre o endereço IP utilizado.  Este é um cenário de contingência para quando cenários 1 e o cenário 2 não existe.  Este cenário não é recomendado se o controlo sobre o endereço de saída for pretendido.

Portas de realizar o SNAT são preallocated conforme descrito no [compreender realizar o SNAT e TERESA](#snat) secção e são um recurso finito que pode ser esgotado. É importante compreender a forma como são [consumido](#pat). Reveja [esgotamento de realizar o SNAT gerir](#snatexhaust) para compreender como estruturar para e mitigar conforme necessário.

### <a name="combinations"></a>Vários cenários combinados

Os cenários descritos nas secções que precede podem ser combinados para alcançar um resultado específico.  Quando existem vários cenários, uma ordem de precedência de aplica-se: [cenário 1](#ilpip) tem precedência sobre [cenário 2](#lb) e [3](#defaultsnat) (Azure Resource Manager apenas) e [cenário 2](#lb) substitui [cenário 3](#defaultsnat) (Azure Resource Manager e clássico).

Um exemplo é uma implementação do Gestor de recursos do Azure onde a aplicação depende descontos elevados ligações de saída para um número limitado de destinos, mas também recebe fluxos entrados através de um front-end de Balanceador de carga. Neste caso, pode combinar cenários 1 e 2 para relief.  Reveja [esgotamento de realizar o SNAT gerir](#snatexhaust) padrões adicionais.

### <a name="multivipsnat"></a>Vários frontends para fluxos de saída

Básico de Balanceador de carga irá escolher um front-end única para ser utilizados fluxos de saída quando [vários frontends IP (público)](load-balancer-multivip-overview.md) são candidatos para fluxos de saída.  Esta seleção não é configurável e o algoritmo de seleção deve ser considerado como aleatório.  Pode designar um endereço IP específico para a saída conforme descrito em [combinado cenários](#combinations).

## <a name="snat"></a>Compreender a realizar o SNAT e TERESA

### <a name="pat"></a>Porta masquerading realizar o SNAT (TERESA)

Quando um recurso de Balanceador de carga público está associado com instâncias VM, cada origem de ligação de saída é foi reescrita. A origem é rescrita do espaço de endereços IP privado da rede virtual para o endereço IP público front-end de Balanceador de carga. No espaço de endereço IP público, 5-cadeias de identificação do fluxo (endereço IP de origem, porta de origem, o protocolo de transporte IP, endereço IP de destino, porta de destino) tem de ser exclusivo.  Portas efémeras são utilizadas para atingir esse objetivo após a conversão de endereço IP de origem privada, uma vez que vários fluxos provenientes de um único endereço IP público.  Estas portas efémeras são designadas por portas de realizar o SNAT. 

Uma porta de realizar o SNAT é consumida por fluxo para um endereço IP de destino único, a porta e protocolo. Para vários fluxos para o mesmo endereço IP de destino, a porta e protocolo, cada fluxo consome uma única porta de realizar o SNAT. Isto garante que os fluxos são exclusivos quando teve o mesmo endereço IP público para o mesmo endereço IP de destino, a porta e protocolo. 

Vários fluxos, cada um para um endereço IP de destino diferente, a porta e protocolo, irão partilhar uma única porta de realizar o SNAT. O endereço IP de destino, porta e protocolo torna fluxos exclusivo sem a necessidade de origem adicionais portas para ser utilizada para distinguir fluxos no espaço de endereços IP públicos.

Quando os recursos de porta de realizar o SNAT ficam esgotados, fluxos de saída falharem até que as portas de realizar o SNAT lançadas pela fluxos existentes. Balanceador de carga reclama portas realizar o SNAT quando o fluxo fecha e utiliza um [tempo limite de inatividade de 4 minutos](#idletimeout) para realizar o SNAT as portas de fluxos de inatividade de reclamação.

Reveja o [gerir realizar o SNAT](#snatexhaust) secção padrões de correção atenuar condições que normalmente levar ao esgotamento de porta de realizar o SNAT.

### <a name="preallocatedports"></a>Preallocation de portas efémeras para a porta masquerading realizar o SNAT (TERESA)

Realizar o SNAT masquerading Azure utiliza um algoritmo para determinar o número de preallocated realizar o SNAT portas disponíveis com base no tamanho do conjunto back-end ao utilizar a porta ([PAT](#pat)).  Portas de realizar o SNAT são portas efémeras disponíveis para um determinado endereço de origem IP público.

Azure preallocates realizar o SNAT portas para a configuração de IP da NIC de cada VM. Quando uma configuração de IP é adicionada ao conjunto, as portas de realizar o SNAT são preallocated para esta configuração de IP com base no tamanho de conjunto back-end. Para funções de trabalho Web clássico, a alocação é por instância de função.  Quando são criados os fluxos de saída, [PAT](#pat) dinamicamente consome (até ao limite preallocated) e liberta estas portas quando fecha o fluxo ou [tempos limite de inatividade](#ideltimeout).

A tabela seguinte mostra os preallocations de porta de realizar o SNAT para as camadas de tamanhos de conjunto back-end:

| Tamanho do conjunto (instâncias de VM) | Portas de realizar o SNAT preallocated por configuração de IP|
| --- | --- |
| 1 - 50 | 1024 |
| 51 - 100 | 512 |
| 101 - 200 | 256 |
| 201 - 400 | 128 |
| 401 - 800 | 64 |
| 801 - 1,000 | 32 |

É importante lembrar-se de que o número de portas de realizar o SNAT disponíveis traduz diretamente para o número de ligações. Uma única porta de realizar o SNAT pode ser reutilizada vários destinos exclusivo. As portas são apenas utilizadas se for necessário para disponibilizar fluxos exclusivo. Consulte [como gerir este recurso exhaustible](#snatexhaust) para obter orientações sobre a estrutura e a respetiva mitigação, bem como a secção que descreve [PAT](#pat).

Alteração do tamanho do seu conjunto de back-end pode afetar alguns dos seus fluxos estabelecidos. Se o tamanho do conjunto de back-end aumenta e passa para a próxima camada, metade das suas portas de realizar o SNAT preallocated são recuperados durante a transição para a próxima camada superior do conjunto back-end. Fluxos que estão associados uma porta de realizar o SNAT reclaimed serão o tempo limite e tem de ser restabelecer. Novas tentativas de ligação com êxito imediatamente, desde que existem portas preallocated disponíveis.

Se o tamanho do conjunto de back-end diminui e passa para um escalão inferior, aumenta o número de portas de realizar o SNAT disponíveis. Neste caso, existentes alocado realizar o SNAT portas e os seus respetivos fluxos não são afetados.

## <a name="snatexhaust"></a>Gestão de esgotamento de porta de realizar o SNAT (TERESA)

[Portas efémeras](#preallocatedports) utilizado para [PAT](#pat) são um recurso exhaustible conforme descrito em [VM autónoma, sem o endereço IP público de nível de instância](#defaultsnat) e [VM de balanceamento de carga sem Endereço IP público de nível de instância](#lb).

Se souber são Iniciar muitas TCP ou UDP ligações de saída para o mesmo endereço IP de destino e a porta e observe a efetuar ligações de saída ou aconselhado pelo suporte são esgotar os portas de realizar o SNAT (preallocated [portas efémeras](#preallocatedports)utilizado pelo [PAT](#pat)), tem várias opções de mitigação geral.  Reveja estas opções e decidir o que está disponível e melhor para o seu cenário.  É possível um ou mais podem ajudar a gerir este cenário.

Se estiver a ter problemas em compreender o comportamento de ligação de saída, pode utilizar estatísticas de pilha IP (netstat) ou pode ser útil observar os comportamentos de ligação com capturas de pacotes.  Pode efetuar estas capturas de pacotes no SO convidado da sua instância ou utilizar [observador de rede de captura de pacotes](../network-watcher/network-watcher-packet-capture-manage-portal.md).

### <a name="connectionreuse"></a>Modificar a aplicação reutilizar ligações 
Pode reduzir a pedido para portas efémeras utilizado para realizar o SNAT por reutilizar as ligações na sua aplicação.  Isto é particularmente verdadeiro protocolos, tal como HTTP/1.1 onde reutilizar a ligação é a predefinição.  E outros protocolos utilizando HTTP como transporte (ou seja, REST) podem beneficiar por sua vez.  É sempre melhor do que individuais, atómicas ligações de TCP para cada pedido e reutilização resulta em mais performant, transações de TCP muito eficientes.

### <a name="connection pooling"></a>Modificar a aplicação para utilizar o agrupamento de ligações
Pode utilizar uma ligação de agrupamento de esquema na sua aplicação, onde os pedidos internamente estão distribuídos por um conjunto de ligações (cada reutilizar sempre que possível) fixo.  Este restrições o número de efémeras portas em utilização e cria um ambiente mais previsível.  Isto também pode aumentar o débito de pedidos ao permitir várias operações simultâneas, quando uma ligação única está a bloquear a resposta de uma operação.  Agrupamento de ligações pode já existir dentro do framework que está a utilizar para desenvolver a sua aplicação ou as definições de configurações para a sua aplicação.  Pode combinar isto com reutilização de ligação e os pedidos de vários consumam um número fixo e previsível de portas para o mesmo endereço IP de destino e a porta ao também benefiting contra a utilização muito eficiente de transações de TCP, reduzindo a latência e de recursos utilização.  Transações de UDP também podem beneficiar como gerir o número de UDP fluxos podem por sua vez evitar a esgotar os condições e gerir a utilização de porta de realizar o SNAT.

### <a name="retry logic"></a>Modificar a aplicação para utilizar menos agressiva lógica de repetição
Quando [preallocated portas efémeras](#preallocatedports) utilizado para [PAT](#pat) estão esgotadas, ou aplicação ocorram falhas, agressiva ou força bruta repete sem decay e término lógica causar esgotamento a ocorrer ou manter. Pode reduzir a pedido para portas efémeras utilizando uma menor agressiva lógica de repetição.   Portas efémeras tem um minuto 4 tempo limite de inatividade (não ajustável) e se as repetições são demasiado agressiva, o esgotamento não tem nenhuma oportunidade de limpar no seu próprio.  Por conseguinte, considerar como e frequência a sua aplicação repete transações é uma consideração crítica do design.

### <a name="assignilpip"></a>Atribuir um IP público de nível de instância para cada VM
Isto altera o seu cenário para [IP público de nível de instância para uma VM](#ilpip).  Todas as portas efémeras do IP público utilizado para cada VM estão disponíveis para a VM (por oposição a cenários em que a portas efémeras de um IP público são partilhadas com todas as VMS associado ao conjunto de back-end correspondentes).  Existem trade-offs a ter em consideração, como o custo adicional de endereços IP públicos e o impacto potencial da branca um grande número de endereços IP individuais.

>[!NOTE] 
>Esta opção não está disponível para as funções de trabalho Web.

### <a name="idletimeout"></a>Utilizar keepalives para repor o tempo limite inativo da saída

Ligações de saída têm um tempo de limite de inatividade de 4 minutos. Não é ajustável. No entanto, pode utilizar o transporte (ou seja, TCP keep-alives) ou keepalives de camada de aplicação para atualizar um fluxo de inatividade e repor este tempo limite de inatividade, se necessário.

## <a name="discoveroutbound"></a>Detetar o IP público utilizado por uma determinada VM
Existem várias formas para determinar o endereço IP de origem público de uma ligação de saída. OpenDNS fornece um serviço que pode apresentar-lhe o endereço IP público da sua VM. Utilizando o comando nslookup, pode enviar uma consulta DNS para o myip.opendns.com de nome para a resolução de OpenDNS. O serviço devolve o endereço IP de origem que foi utilizado para enviar a consulta. Quando executar a consulta seguinte da sua VM, a resposta é o IP público utilizado relativamente a essa VM.

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventoutbound"></a>Impedir a conectividade de saída
Por vezes, não é desejável para uma VM para a permissão para criar um fluxo de saída ou poderá haver um requisito para gerir os destinos que podem ser acedidos com fluxos de saída ou que destinos podem começar a fluxos de entrada. Neste caso, utilize [grupos de segurança de rede (NSG)](../virtual-network/virtual-networks-nsg.md) para gerir os destinos em que a VM pode aceder, bem como o destino público, pode iniciar os fluxos de entrada. Quando aplica um NSG a uma VM com balanceamento de carga, terá de prestar atenção ao [predefinido etiquetas](../virtual-network/virtual-networks-nsg.md#default-tags) e [predefinido regras](../virtual-network/virtual-networks-nsg.md#default-rules).

Tem de se certificar de que a VM pode receber pedidos de sonda de estado de funcionamento do Balanceador de carga do Azure. Se um NSG bloquear pedidos de sonda de estado de funcionamento da etiqueta predefinida AZURE_LOADBALANCER, a pesquisa de estado de funcionamento da VM falha e a VM está marcado como. Balanceador de carga para a enviar de novos fluxos para essa VM.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [básico de Balanceador de carga](load-balancer-overview.md).
- Saiba mais sobre [grupos de segurança de rede](../virtual-network/virtual-networks-nsg.md).
- Saiba mais sobre algumas da outra chave [capacidades de rede](../networking/networking-overview.md) no Azure.

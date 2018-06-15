---
title: Ligações de saída no Azure (clássica) | Microsoft Docs
description: Este artigo explica como o Azure permite serviços para comunicar com serviços de internet pública em nuvem.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2018
ms.author: kumud
ms.openlocfilehash: f6452d8f88b91fe0cbf144ce951b84ba4cec0047
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33939826"
---
# <a name="outbound-connections-classic"></a>Ligações de saída (clássica)

O Azure oferece a conectividade de saída para implementações de cliente através de vários mecanismos diferentes. Este artigo descreve quais são os cenários, quando estas são aplicadas, como funcionam e como geri-los.

>[!NOTE]
>Este artigo abrange apenas a implementações clássicas.  Reveja [ligações de saída](load-balancer-outbound-connections.md) para todos os cenários de implementação do Gestor de recursos no Azure.

Uma implementação no Azure pode comunicar com pontos finais fora do Azure no espaço de endereço IP público. Quando inicia a uma instância de um fluxo de saída para um destino no espaço de endereço IP público, o Azure mapeia dinamicamente o endereço IP privado para um endereço IP público. Após a criação desta mapeamento, tráfego de retorno deste fluxo originated saída também pode aceder ao endereço IP privado onde o fluxo foi originado.

Azure utiliza a tradução de endereços de rede de origem (realizar o SNAT) para executar esta função. Quando vários endereços IP privados são masquerading atrás de um único endereço IP público, o Azure utiliza [tradução de endereços (TERESA) de porta](#pat) para representação endereços IP privados. Portas efémeras são utilizadas para TERESA e são [preallocated](#preallocatedports) com base no tamanho do conjunto.

Existem vários [cenários saídos](#scenarios). Pode combinar estes cenários, conforme necessário. Revê-los cuidadosamente de forma a compreender as capacidades, restrições e padrões de acordo com o modelo de implementação e o cenário de aplicação. Reveja as orientações para [gerir estes cenários](#snatexhaust).

## <a name="scenarios"></a>Descrição geral do cenário

O Azure oferece três métodos diferentes para atingir implementações clássicas de conectividade de saída.  Nem todas as implementações clássicas têm todos os três cenários disponíveis:

| Cenário | Método | Protocolos IP | Descrição | Função de trabalho Web | IaaS | 
| --- | --- | --- | --- | --- | --- |
| [1. VM com um endereço IP público de nível de instância](#ilpip) | Realizar o SNAT, porta masquerading não utilizado | TCP, UDP, ICMP, ESP | Azure utiliza o IP público atribuído a Máquina Virtual. A instância tem todas as portas efémeras disponíveis. | Não | Sim |
| [2. ponto final com balanceamento de carga público](#publiclbendpoint) | Realizar o SNAT com porta masquerading (PAT) para o ponto final público | TCP, UDP | Azure partilha o IP endereço público ponto final público com vários pontos finais privados. Azure utiliza portas efémeras do ponto final público para TERESA. | Sim | Sim |
| [3. VM autónoma ](#defaultsnat) | Realizar o SNAT com porta masquerading (PAT) | TCP, UDP | Azure automaticamente designa um endereço IP público para realizar o SNAT, partilhe este endereço IP público com a implementação de toda e utiliza portas efémeras do endereço IP do ponto final público para TERESA. Este é um cenário de contingência para os cenários anteriores. Não recomendamos-lo se precisar de visibilidade e controlo. | Sim | Sim |

Este é um subconjunto da funcionalidade de ligação de saída disponível para implementações do Resource Manager no Azure.  

Diferentes implementações no clássica têm funcionalidades diferentes:

| Implementação clássica | Funcionalidade disponível | 
| --- | --- |
| Máquina Virtual | cenário [1](#ilpip), [2](#publiclbendpoint), ou [3](#defaultsnat) |
| Função de trabalho Web | apenas de cenário [2](#publiclbendpoint), [3](#defaultsnat) | 

[Estratégias de mitigação](#snatexhaust) também têm as mesmas diferenças.

O [algoritmo utilizado para preallocating portas efémeras](#ephemeralports) para TERESA para implementações clássicas é igual para implementações de recursos do Azure Resource Manager.

### <a name="ilpip"></a>Cenário 1: VM com um endereço IP público de nível de instância

Neste cenário, a VM possui uma instância do nível pública IP (ILPIP) atribuída. Como são preocupações ligações de saída, é irrelevante se a VM possui um ponto final de balanceamento de carga ou não. Este cenário tem precedência sobre as outras. Quando é utilizado um ILPIP, a VM utiliza o ILPIP para todos os fluxos de saída.  

Um público IP atribuído a uma VM é um 1:1 relação (em vez de 1:many) e implementado como um NAT de 1:1 sem monitorização de estado.  Não é utilizada a porta masquerading (TERESA) e a VM tem todas as portas efémeras disponíveis para utilização.

Se muitos fluxos de saída inicia a sua aplicação e experiência de esgotamento de porta de realizar o SNAT, considere atribuir um [ILPIP para mitigar restrições de realizar o SNAT](#assignilpip). Reveja [esgotamento de realizar o SNAT gerir](#snatexhaust) na íntegra.

### <a name="publiclbendpoint"></a>Cenário 2: Com balanceamento de carga ponto final público

Neste cenário, a função de trabalho Web ou VM está associada um endereço IP público através do ponto final com balanceamento de carga. A VM não tem um endereço IP público atribuído. 

Quando a VM de balanceamento de carga cria um fluxo de saída, Azure traduz o endereço IP de origem privada do fluxo de saída para o endereço IP público do ponto final público de balanceamento de carga. Azure utiliza realizar o SNAT para executar esta função. Também utiliza o Azure [PAT](#pat) para masquerade vários endereços IP privados atrás de um endereço IP público. 

Portas efémeras de frontend de endereço IP público do Balanceador de carga são utilizadas para distinguir fluxos individuais teve originados pela VM. Realizar o SNAT dinamicamente utiliza [preallocated portas efémeras](#preallocatedports) quando são criados os fluxos de saída. Neste contexto, as portas efémeras utilizadas para realizar o SNAT são designadas por portas de realizar o SNAT.

Portas de realizar o SNAT são preallocated conforme descrito no [compreender realizar o SNAT e TERESA](#snat) secção. Se estiver a um recurso finito que pode ser esgotado. É importante compreender a forma como são [consumido](#pat). Para compreender como estruturar para este consumo e mitigar conforme necessário, reveja [esgotamento de realizar o SNAT gerir](#snatexhaust).

Quando [vários pontos finais públicos com balanceamento de carga](load-balancer-multivip.md) existe, se qualquer um destes endereços IP públicos um [candidato para fluxos de saída](#multivipsnat), e é selecionada uma aleatoriamente.  

### <a name="defaultsnat"></a>Cenário 3: Nenhum endereço IP público associado

Neste cenário, a função de trabalho Web ou VM não faz parte de um ponto de final público com balanceamento de carga.  E, no caso de VM, não tem um endereço ILPIP atribuído ao mesmo. Quando a VM cria um fluxo de saída, Azure traduz o endereço IP de origem privada do fluxo de saída para um endereço IP público de origem. O endereço IP público utilizado para este fluxo de saída não é configurável e não count contra o limite de recurso IP público da subscrição.  Azure atribui automaticamente este endereço.

Azure utiliza realizar o SNAT com porta masquerading ([PAT](#pat)) para executar esta função. Este cenário é semelhante à [cenário 2](#lb), mas não há nenhum controlo sobre o endereço IP utilizado. Este é um cenário de contingência para quando não existem cenários 1 e 2. Não recomendamos este cenário se pretender que o controlo sobre o endereço de saída. Se as ligações de saída são uma parte fundamental da sua aplicação, deve escolher outro cenário.

Portas de realizar o SNAT são preallocated conforme descrito no [compreender realizar o SNAT e TERESA](#snat) secção.  O número de VMs ou funções de trabalho Web partilha o endereço IP público determina o número de portas efémeras preallocated.   É importante compreender a forma como são [consumido](#pat). Para compreender como estruturar para este consumo e mitigar conforme necessário, reveja [esgotamento de realizar o SNAT gerir](#snatexhaust).

## <a name="snat"></a>Compreender a realizar o SNAT e TERESA

### <a name="pat"></a>Porta masquerading realizar o SNAT (TERESA)

Quando uma implementação estabelece uma ligação de saída, cada origem de ligação de saída é foi reescrita. A origem é rescrita do espaço de endereço IP privado para o IP público associado à implementação (baseada em cenários descritos acima). No espaço de endereço IP público, 5-cadeias de identificação do fluxo (endereço IP de origem, porta de origem, o protocolo de transporte IP, endereço IP de destino, porta de destino) tem de ser exclusivo.  

Portas efémeras (portas de realizar o SNAT) são utilizadas para atingir esse objetivo após a conversão de endereço IP de origem privada, porque vários fluxos provenientes de um único endereço IP público. 

Uma porta de realizar o SNAT é consumida por fluxo para um endereço IP de destino único, a porta e protocolo. Para vários fluxos para o mesmo endereço IP de destino, a porta e protocolo, cada fluxo consome uma única porta de realizar o SNAT. Isto garante que os fluxos são exclusivos quando provenientes do mesmo endereço IP público e vá para o mesmo endereço IP de destino, porta e protocolo. 

Vários fluxos, cada um para um endereço IP de destino diferente, a porta e protocolo, partilham uma única porta de realizar o SNAT. O endereço IP de destino, porta e protocolo tornar fluxos exclusivo sem a necessidade de origem adicionais portas para distinguir fluxos no espaço de endereço IP público.

Quando os recursos de porta de realizar o SNAT ficam esgotados, fluxos de saída falharem até que as portas de realizar o SNAT fluxos existentes da versão. Balanceador de carga reclama portas realizar o SNAT quando o fluxo fecha e utiliza um [tempo limite de inatividade de 4 minutos](#idletimeout) para realizar o SNAT as portas de fluxos de inatividade de reclamação.

Padrões mitigar condições que normalmente levar ao esgotamento de porta de realizar o SNAT, reveja o [gerir realizar o SNAT](#snatexhaust) secção.

### <a name="preallocatedports"></a>Preallocation de portas efémeras para a porta masquerading realizar o SNAT (TERESA)

Realizar o SNAT masquerading Azure utiliza um algoritmo para determinar o número de preallocated realizar o SNAT portas disponíveis com base no tamanho do conjunto back-end ao utilizar a porta ([PAT](#pat)). Portas de realizar o SNAT são portas efémeras disponíveis para um determinado endereço de origem IP público.

Azure preallocates realizar o SNAT portas quando é implementada uma instância com base no quantas instâncias VM ou função de trabalho Web partilham um determinado endereço IP público.  Quando são criados os fluxos de saída, [PAT](#pat) dinamicamente consome (até ao limite preallocated) e liberta estas portas quando fecha o fluxo ou [tempos limite de inatividade](#ideltimeout) acontecer.

A tabela seguinte mostra os preallocations de porta de realizar o SNAT para as camadas de tamanhos de conjunto de back-end:

| Instâncias | Portas de realizar o SNAT preallocated por instância |
| --- | --- |
| 1 a 50 | 1,024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |

Lembre-se de que o número de portas de realizar o SNAT disponíveis traduz diretamente para o número de fluxos. Uma única porta de realizar o SNAT pode ser reutilizada vários destinos exclusivo. As portas são consumidas apenas se for necessário para disponibilizar fluxos exclusivo. Para obter orientações de conceção e a respetiva mitigação, consulte a secção sobre [como gerir este recurso exhaustible](#snatexhaust) e a secção que descreve [PAT](#pat).

Alterar o tamanho da implementação pode afetar alguns dos seus fluxos estabelecidos. Se o tamanho do conjunto back-end aumenta e passa para a próxima camada, metade das suas portas de realizar o SNAT preallocated são recuperados durante a transição para a próxima camada superior do conjunto back-end. Fluxos que estão associados uma porta de realizar o SNAT reclaimed serão o tempo limite e tem de ser restabelecer. Se um novo fluxo for tentado, o fluxo será bem sucedida imediatamente, desde que existem portas preallocated disponíveis.

Se o tamanho da implementação diminui e passa para um escalão inferior, aumenta o número de portas de realizar o SNAT disponíveis. Neste caso, existentes alocado realizar o SNAT portas e os seus respetivos fluxos não são afetados.

As alocações de portas de realizar o SNAT são específico de protocolo de transporte IP (TCP e UDP que e mantidó em separado) e são lançadas sob as seguintes condições:

### <a name="tcp-snat-port-release"></a>Versão de porta de TCP realizar o SNAT

- Se o servidor/cliente envia FIN/ACK, porta de realizar o SNAT será lançada após 240 segundos.
- Se um RST é utilizado, porta de realizar o SNAT será lançada após 15 segundos.
- foi atingido o tempo limite de inatividade

### <a name="udp-snat-port-release"></a>Versão de porta UDP realizar o SNAT

- foi atingido o tempo limite de inatividade

## <a name="problemsolving"></a> Resolução do problema 

Esta secção destina-se para ajudar a mitigar o esgotamento de realizar o SNAT e outros cenários que podem ocorrer com ligações de saída no Azure.

### <a name="snatexhaust"></a> Gestão de esgotamento de porta de realizar o SNAT (TERESA)
[Portas efémeras](#preallocatedports) utilizado para [PAT](#pat) são um recurso exhaustible, conforme descrito em [não associado a um IP público](#defaultsnat) e [ponto final com balanceamento de carga público](#publiclbendpoint).

Se souber que está a iniciar muitas TCP ou UDP ligações de saída para o mesmo endereço IP de destino e a porta e observe a efetuar ligações de saída ou aconselhado pelo suporte que está a esgotar os portas de realizar o SNAT (preallocated [efémeras portas](#preallocatedports) utilizado pelo [PAT](#pat)), tem várias opções de mitigação geral. Reveja estas opções e decidir o que está disponível e melhor para o seu cenário. É possível que um ou mais podem ajudar a gerir este cenário.

Se estiver a ter problemas em compreender o comportamento de ligação de saída, pode utilizar as estatísticas de pilha IP (netstat). Ou pode ser útil observar comportamentos de ligação através da utilização de capturas de pacotes.

#### <a name="connectionreuse"></a>Modificar a aplicação reutilizar ligações 
Pode reduzir a pedido para portas efémeras que são utilizados para realizar o SNAT por reutilizar as ligações na sua aplicação. Isto é particularmente verdadeiro protocolos, como HTTP/1.1, onde a reutilização de ligação é a predefinição. E outros protocolos que utilizam HTTP como transporte (por exemplo, REST) podem beneficiar por sua vez. 

Reutilização é sempre melhor do que individuais, atómicas ligações de TCP para cada pedido. Reutilize resulta em mais performant, transações de TCP muito eficientes.

#### <a name="connection pooling"></a>Modificar a aplicação para utilizar o agrupamento de ligação
Pode utilizar uma ligação de agrupamento de esquema na sua aplicação, onde os pedidos internamente estão distribuídos por um conjunto de ligações (cada reutilizar sempre que possível) fixo. Este esquema restringe o número de portas efémeras em utilização e cria um ambiente mais previsível. Este esquema também pode aumentar o débito de pedidos ao permitir várias operações simultâneas, quando uma ligação única está a bloquear a resposta de uma operação.  

Agrupamento de ligação poderá já existir dentro do framework que está a utilizar para desenvolver a sua aplicação ou as definições de configuração para a sua aplicação. Pode combinar o agrupamento de ligações com reutilização de ligação. Os pedidos de vários consumam, em seguida, um número fixo e previsível de portas para o mesmo endereço IP de destino e a porta. Os pedidos também beneficiam da utilização eficiente de transações de TCP, reduzindo a utilização de recursos e latência. Transações de UDP também podem beneficiar, porque a gerir o número de fluxos UDP pode evitar a esgotar os condições e gerir a utilização de porta de realizar o SNAT por sua vez.

#### <a name="retry logic"></a>Modificar a aplicação para utilizar menos agressiva lógica de repetição
Quando [preallocated portas efémeras](#preallocatedports) utilizado para [PAT](#pat) estão esgotadas, ou aplicação ocorram falhas, agressiva ou força bruta repete sem decay e término lógica causar esgotamento a ocorrer ou manter. Pode reduzir a pedido para portas efémeras utilizando uma menor agressiva lógica de repetição. 

Portas efémeras tem um minuto 4 tempo limite de inatividade (não ajustável). Se as repetições são demasiado agressiva, o esgotamento não tem nenhuma oportunidade de limpar no seu próprio. Por conseguinte, considerar como – e com que frequência – a aplicação repete transações é uma parte fundamental da estrutura.

#### <a name="assignilpip"></a>Atribuir um IP público ao nível de instância para cada VM
Atribuir um ILPIP altera o seu cenário para [IP público de nível de instância para uma VM](#ilpip). Todas as portas efémeras do IP público que são utilizadas para cada VM estão disponíveis para a VM. (Por oposição cenários em que as portas efémeras de um IP público são partilhadas com todas as VMs associados à respetiva implementação.) Existem trade-offs a ter em consideração, como o impacto potencial da branca um grande número de endereços IP individuais.

>[!NOTE] 
>Esta opção não está disponível para as funções de trabalho web.

### <a name="idletimeout"></a>Utilizar keepalives para repor o tempo limite inativo da saída

Ligações de saída têm um tempo de limite de inatividade de 4 minutos. Não é ajustável este tempo limite. No entanto, pode utilizar o transporte (por exemplo, o TCP keepalives) ou keepalives de camada de aplicação para atualizar um fluxo de inatividade e repor este tempo limite de inatividade, se necessário.  Contacte o fornecedor de qualquer software instaladores se esta opção é suportada ou como ativá-la.  Geralmente, apenas um lado tem de gerar keepalives para repor o tempo limite de inatividade. 

## <a name="discoveroutbound"></a>Detetar o IP público que utiliza uma VM
Existem várias formas para determinar o endereço IP de origem público de uma ligação de saída. OpenDNS fornece um serviço que pode apresentar-lhe o endereço IP público da sua VM. 

Ao utilizar o comando nslookup, pode enviar uma consulta DNS para o myip.opendns.com de nome para a resolução de OpenDNS. O serviço devolve o endereço IP de origem que foi utilizado para enviar a consulta. Quando executar a consulta seguinte da sua VM, a resposta é o IP público utilizado relativamente a essa VM:

    nslookup myip.opendns.com resolver1.opendns.com


## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [Balanceador de carga](load-balancer-overview.md) utilizados em implementações do Resource Manager.
- Conheça o modo [ligação saída](load-balancer-outbound-connections.md) cenários disponíveis nas implementações do Resource Manager.

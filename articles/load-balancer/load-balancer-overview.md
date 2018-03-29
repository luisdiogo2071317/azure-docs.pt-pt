---
title: Descrição geral do Balanceador de carga do Azure | Microsoft Docs
description: Descrição geral das funcionalidades, a arquitetura e a implementação de Balanceador de carga do Azure. Saiba como funciona o Balanceador de carga e aproveitam na nuvem.
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
ms.date: 03/21/2018
ms.author: kumud
ms.openlocfilehash: 3a5d1e897d8ffe063ecf9277bef346c8b7c5092b
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="azure-load-balancer-overview"></a>Azure Load Balancer overview (Descrição Geral do Balanceador de Carga do Azure)

Balanceador de carga do Azure permite-lhe dimensionar as suas aplicações e criar a elevada disponibilidade para os serviços. Balanceador de carga suporta cenários de entrada como saídos e fornece a latência baixa, débito elevado e reduz horizontalmente até milhões de fluxos de todas as aplicações de TCP e UDP.   

Balanceador de carga será distribuir fluxos de entrada novo que chegam no front-end de Balanceador de carga para as instâncias do conjunto de back-end, de acordo com as regras e as sondas de estado de funcionamento.  

Além disso, um balanceador de carga públicos também podem fornecer ligações de saída para as máquinas virtuais no interior da rede virtual por traduzir os respetivos endereços IP privados para endereços IP públicos.

Balanceador de carga do Azure está disponível em duas SKUs diferentes: básico e padrão.  Existem diferenças na escala, funcionalidades e preços.  Qualquer cenário possível com Balanceador de carga básico também pode ser criado com Balanceador de carga padrão, embora a abordagem poderá ser ligeiramente diferente.  Como saber sobre o Balanceador de carga, é importante para se familiarizar com as noções básicas e diferenças de específico de SKU.

## <a name="why-use-load-balancer"></a>Porquê utilizar o Balanceador de carga? 

Balanceador de carga do Azure pode ser utilizado para:

* Carregar saldo tráfego de entrada à Internet para máquinas virtuais. Esta configuração é conhecida como um [Balanceador de carga público](#publicloadbalancer).
* Tráfego de balanceamento de carga entre máquinas virtuais dentro de uma rede virtual. Também pode contactar um front-end de Balanceador de carga de uma rede no local, num cenário híbrido. Ambos os cenários utilizam uma configuração que é conhecida como um [Balanceador de carga interno](#internalloadbalancer).
* Porta reencaminhar tráfego para uma porta específica em máquinas virtuais específicas com regras NAT de entrada.
* Fornecer [conectividade de saída](load-balancer-outbound-connections.md) para máquinas virtuais no interior da rede virtual, utilizando um balanceador de carga público.


>[!NOTE]
> O Azure oferece um conjunto de soluções para os seus cenários de balanceamento de carga completamente gerido.  Se estiver à procura para a terminação de TLS ("descarga de SSL") ou pelo processamento de camada de aplicação de pedidos HTTP/HTTPS, reveja [Gateway de aplicação](../application-gateway/application-gateway-introduction.md).  Se estiver à procura de global DNS balanceamento de carga, reveja [Gestor de tráfego](../traffic-manager/traffic-manager-overview.md).  Os cenários de ponto a ponto podem tirar partido do combinar estas soluções conforme necessário.

## <a name="what-is-load-balancer"></a>O que é o Balanceador de Carga?

Pode existir um recurso de Balanceador de carga como um balanceador de carga público ou um balanceador de carga interno. Funções do recurso de Balanceador de carga são expresso como um front-end, uma regra, uma pesquisa de estado de funcionamento e uma definição de conjunto de back-end.  Máquinas virtuais são colocadas no conjunto de back-end, especificando o conjunto de back-end da máquina virtual.

Recursos de Balanceador de carga são objetos dentro do qual pode express como o Azure deve programa respetiva infraestrutura de multi-inquilino para alcançar o cenário de que pretende criar.  Não há nenhuma relação direta entre os recursos de Balanceador de carga e a infraestrutura real; criar um balanceador de carga não criar uma instância e capacidade está sempre disponível. 

## <a name="fundamental-load-balancer-features"></a>Funcionalidades fundamentais do Balanceador de carga

Balanceador de carga fornece as seguintes capacidades fundamentais para as aplicações TCP e UDP:

* **Balanceamento de carga**

    Balanceador de carga do Azure permite-lhe criar uma regra para distribuir a chegar ao front-end para instâncias do conjunto de back-end o tráfego de balanceamento de carga.  Utiliza um algoritmo de hash para a distribuição dos fluxos de entrada e reescreve os cabeçalhos de fluxos para instâncias do conjunto de back-end em conformidade. Um servidor está disponível para receber novos fluxos quando a sonda de estado de funcionamento indica um ponto final de back-end em bom estado.
    
    Por predefinição, utiliza um hash de 5 cadeias de identificação é composto por endereço IP de origem, porta de origem, endereço IP de destino, porta de destino e número de protocolo IP para mapear fluxos para servidores disponíveis.  Pode optar por criar a afinidade com um endereço IP de origem específico ao aceitar para um hash de 2 ou 3 identificação para uma determinada regra.  Todos os pacotes do mesmo fluxo de pacotes chegam na mesma instância atrás de front-end com balanceamento de carga.  Quando o cliente inicia um novo fluxo do mesmo IP de origem, as alterações de porta de origem. O 5-cadeias de identificação resultante podem fazer com que o tráfego Ir para um ponto final de back-end diferentes, como resultado.

    Para obter mais informações, consulte [modo de distribuição do Balanceador de carga](load-balancer-distribution-mode.md). O gráfico seguinte mostra a distribuição com base em hash:

    ![Distribuição com base em hash](./media/load-balancer-overview/load-balancer-distribution.png)

    *Figura - com base em Hash de distribuição*

* **Reencaminhamento de porta**

    Balanceador de carga do Azure permite-lhe criar uma regra NAT de entrada para reencaminhar tráfego de porta de uma porta específica de um endereço IP de front-end específico para uma porta específica de uma instância de back-end específicos dentro da rede Virtual. Isto também é efetuado através de distribuição com base em hash mesma como balanceamento de carga.  Cenários comuns para esta capacidade são as sessões de protocolo RDP (Remote Desktop Protocol) ou de Secure Shell (SSH) para instâncias de máquina virtual individual dentro da rede Virtual.  Pode mapear vários pontos finais internos para as portas diferentes do mesmo endereço de IP de front-end. Pode utilizá-las para administrar remotamente as máquinas virtuais através da Internet sem a necessidade de uma caixa de ir adicionais.

* **Aplicação agnóstico relativamente e transparente**

    Balanceador de carga não interage diretamente com o TCP ou UDP ou camada da aplicação e quaisquer TCP ou UDP baseada em cenário pode ser suportado.  Por exemplo, enquanto o Balanceador de carga não terminar TLS em si, pode criar e aumentar horizontalmente com o Balanceador de carga de aplicações de TLS e termine a ligação de TLS na máquina virtual propriamente dito. Balanceador de carga não terminar um fluxo e handshakes protocolo são sempre diretamente entre o cliente e a instância do conjunto de back-end selecionado de hash. Por exemplo, um handshake TCP é sempre entre o cliente e a máquina virtual de back-end selecionado.  E uma resposta a um pedido para um front-end for uma resposta gerada a partir da máquina virtual de back-end.  Desempenho de rede de saída do Balanceador de carga está limitado apenas pela máquina virtual SKU escolher e fluxos permanecerá ativo durante longos períodos de tempo se o tempo limite de inatividade nunca foi atingido.

* **Reconfiguração automática**

    Balanceador de carga do Azure de forma instantânea próprio reconfigura quando dimensionar as instâncias ou reduzir verticalmente. Adicionar ou remover máquinas virtuais do conjunto de back-end reconfigura o Balanceador de carga sem operações adicionais no recurso de Balanceador de carga.

* **Sondas de estado de funcionamento**

    Balanceador de carga do Azure utiliza sondas de estado de funcionamento que define para determinar o estado de funcionamento de instâncias no conjunto de back-end. Quando uma sonda não responder, o Balanceador de carga para o envio de novas ligações para as instâncias de mau estado de funcionamento. As ligações existentes não são afetadas e irão continuar até que a aplicação concluir o fluxo, ocorre um limite de tempo inativo, ou a máquina virtual está encerrada.

    Três tipos de sondas que são suportados:

    - **Sonda personalizada HTTP:** pode utilizá-la para criar a sua própria lógica personalizada para determinar o estado de funcionamento de uma instância de conjunto de back-end. O Balanceador de carga regularmente vai sondar o ponto final (cada 15 segundos, por predefinição). A instância é considerada bom estado de funcionamento se responder com um HTTP 200 dentro do período de tempo limite (predefinição de segundos 31). Qualquer Estado diferente de HTTP 200 faz com que esta pesquisa falhar.  Isto também é útil para implementar a sua própria lógica para remover as instâncias de rotação do Balanceador de carga. Por exemplo, pode configurar a instância para devolver um Estado não 200 se a instância está acima da CPU de 90%.   Esta pesquisa substitui a sonda de agente de convidados predefinida.

    - **Sonda personalizada TCP:** esta pesquisa depende do estabelecimento de sessão TCP com êxito a uma porta de pesquisa definidos.  O serviço de escuta especificado na máquina virtual existe, desde que esta pesquisa será bem sucedida. Se a ligação é recusada, a pesquisa irá falhar. Esta pesquisa substitui a sonda de agente de convidados predefinida.

    - **Pesquisa de agente do convidado (na plataforma como um serviço de máquinas virtuais apenas):** o Balanceador de carga, também pode utilizar o agente convidado dentro da máquina virtual. O agente convidado escuta e responde com uma resposta de HTTP 200 OK apenas quando a instância está no estado pronto. Se o agente não conseguir responder com um HTTP 200 OK, o Balanceador de carga marca a instância como responder e deixa de envio de tráfego para essa instância. O Balanceador de carga continua a tentar aceder a instância. Se o agente convidado responde com uma HTTP 200, o Balanceador de carga irá enviar o tráfego para essa instância novamente.  As pesquisas de agente do convidado são último recurso e não devem ser utilizadas quando o HTTP ou TCP configurações de sonda personalizada são possíveis. 
    
* **Ligações de saída (origem NAT)**

    Todos os fluxos de saída de endereços IP privados dentro da sua rede Virtual para endereços IP públicos na Internet podem ser convertidos para um endereço IP de front-end de Balanceador de carga. Quando um front-end público está associado a uma máquina virtual de back-end através de uma regra de balanceamento de carga, o Azure programas ligações de saída para ser automaticamente convertido em endereço IP do público front-end. Isto também é denominado origem NAT (realizar o SNAT). Realizar o SNAT proporciona vantagens importantes:

    + Permite a fácil recuperação após desastre e atualização dos serviços, uma vez que o front-end pode ser atribuído dinamicamente a outra instância do serviço.
    + -Facilita a gestão de lista (ACL) de controlo de acesso. As ACLs expressas em termos de front-end IPs não alterar como os serviços de aumentar verticalmente, pendente ou obter implementada novamente.

    Consulte [ligações de saída](load-balancer-outbound-connections.md) artigo para ver um debate detalhado desta capacidade.

Padrão de Balanceador de carga tem capacidades de específico de SKU adicionais para além destes Noções básicas.  Reveja o resto deste artigo para obter mais detalhes.

## <a name="skus"></a> Comparação de SKU de Balanceador de carga

Balanceador de carga do Azure suporta dois SKUs diferentes: básico e padrão.  Existem diferenças no cenário aumentar, funcionalidades e preços.  Qualquer cenário possível com básico Balanceador de carga pode ser criado com o padrão de Balanceador de carga, bem como.  Na verdade, as APIs para ambos os SKUs são semelhantes e invocado através da especificação de um SKU.  A API para suportar SKUs para o Balanceador de carga e o IP público está disponível a partir da API de 2017-08-01.  Ambos os SKUs de ter o mesmo API geral e a estrutura.

No entanto, dependendo do que é escolhido SKU, os detalhes da configuração do cenário completo poderão ser ligeiramente diferentes. A documentação do Balanceador de carga chama a atenção quando um artigo é aplicável a apenas um SKU específico. Reveja a tabela seguinte abaixo para comparar e compreender as diferenças.  Reveja [descrição geral do Balanceador de carga padrão](load-balancer-standard-overview.md) para obter mais detalhes.

>[!NOTE]
> Novos designs de devem considerar a utilização do padrão de Balanceador de carga. 

Máquinas virtuais autónomas, conjuntos de disponibilidade e conjuntos de dimensionamento de máquina virtual só podem ser ligados a um SKU nunca ambos. Quando utilizado com endereços IP públicos, o Balanceador de carga e o endereço IP público SKU tem de corresponder. Balanceador de carga e SKUs de IP público não são mutável.

_É uma melhor prática para especificar os SKUs explicitamente, apesar de ainda não é obrigatório._  Neste momento, as alterações necessárias estão a ser mantidas num mínimo. Se não for especificado um SKU, ela é interpretada como a sua intenção utilizar SKU básico na versão da API de 2017-08-01.

>[!IMPORTANT]
>Padrão de Balanceador de carga é um novo produto de Balanceador de carga e amplamente um superconjunto básico de Balanceador de carga.  Existem importantes e deliberate diferenças entre ambos os produtos.  Qualquer cenário de ponto a ponto possível com básico Balanceador de carga pode ser criado com o padrão de Balanceador de carga.  Se já são utilizados para básico Balanceador de carga, devem familiarizá-lo com padrão Balanceador de carga para compreender o comportamento entre Standard e Basic e o respetivo impacto nas alterações. Reveja esta secção cuidadosamente.

| | [Standard SKU](load-balancer-standard-overview.md) | SKU Básico |
| --- | --- | --- |
| Tamanho do conjunto de back-end | até 1000 instâncias | até 100 instâncias |
| Pontos finais de conjunto de back-end | Qualquer máquina virtual numa única rede virtual, incluindo blend de máquinas virtuais, conjuntos de disponibilidade, conjuntos de dimensionamento de máquina virtual. | máquinas virtuais pertencentes uma escala de máquina virtual ou conjunto de disponibilidade único conjunto |
| Zonas de Disponibilidade | Com redundância de zona e zonal frontends de entrada e de mapeamentos de fluxos de saída, saída continuam a vigorar após falha de zona, cross-zona balanceamento de carga | / |
| Diagnóstico | Monitor do Azure, métricas multidimensionais, incluindo byte e contadores de pacotes, estado de funcionamento da sonda de estado, as tentativas de ligação (SIN de TCP), estado de funcionamento da ligação de saída (realizar o SNAT com êxito ou falhados fluxos), medidas plane de dados do Active Directory | Análise de registos do Azure, contagem de estado de funcionamento do conjunto de back-end de Balanceador de carga público só, alerta de esgotamento de realizar o SNAT |
| HA portas | Balanceador de carga interno | / |
| Seguro por predefinição | predefinição fechada para pontos finais públicos de Balanceador de carga e de IP e um grupo de segurança de rede deve ser utilizado para explicitamente lista branca para o tráfego flua | predefinição aberto, grupo de segurança opcional |
| Ligações de saída | Vários frontends com por regra ativamente. Um cenário de saída _tem_ explicitamente criado para a máquina virtual para conseguir utilizar a conectividade de saída.  [Pontos finais do serviço de VNet](../virtual-network/virtual-network-service-endpoints-overview.md) podem ser acedidos sem conectividade de saída e não contam para processamento de dados.  Todos os endereços IP públicos, incluindo serviços do Azure PaaS não está disponíveis como pontos finais do serviço de VNet, devem ser atingidos através de conectividade de saída e a contagem para processamento de dados. Quando uma máquina virtual está a funcionar apenas um balanceador de carga interno, ligações de saída através de predefinição realizar o SNAT não estão disponíveis. Programação de realizar o SNAT saída é o protocolo de transporte específicos com base no protocolo de entrada de balanceamento de carga regra. | Único front-end, selecionado aleatória, quando várias frontends estão presentes.  Quando apenas Balanceador de carga interno está a servir uma máquina virtual, é utilizada a predefinição realizar o SNAT. |
| Vários frontends | Entrada e saída | Apenas de entrada |
| Operações de gestão | A maioria dos segundos de < 30 de operações | 60-90 segundos típica |
| SLA | 99,99% de caminho de dados com duas máquinas virtuais em bom estado | Implícita no SLA de VM | 
| Preços | -Lhe cobrados com base no número de regras, processados de dados de entrada ou saída associados a recursos  | Sem qualquer encargo |

Reveja [os limites de serviço para o Balanceador de carga](https://aka.ms/lblimits).  Para o Balanceador de carga padrão Reveja também mais detalhadas [descrição geral](load-balancer-standard-overview.md), [preços](https://aka.ms/lbpricing), e [SLA](https://aka.ms/lbsla).

## <a name="concepts"></a>Conceitos

### <a name = "publicloadbalancer"></a>Balanceador de carga públicos

Balanceador de carga público mapeia o endereço IP público e o número de porta de tráfego de entrada para o endereço IP privado e o número de porta da máquina virtual e vice versa para o tráfego de resposta da máquina virtual. Regras de balanceamento de carga permitem-lhe distribuir tipos específicos de tráfego entre várias máquinas virtuais ou serviços. Por exemplo, pode propagar-se a carga do tráfego de pedido web em vários servidores web.

A figura seguinte mostra um ponto final com balanceamento de carga para o tráfego da web que é partilhado entre três máquinas virtuais para a porta TCP pública e privada de 80. Estas três máquinas virtuais estão num conjunto com balanceamento de carga.

![exemplo de Balanceador de carga públicos](./media/load-balancer-overview/IC727496.png)

**Figura 1: Tráfego de web utilizando um balanceador de carga público de balanceamento de carga**

Quando os clientes de Internet enviam pedidos de página web para o endereço IP público de uma aplicação web na porta TCP 80, o Azure Load Balancer distribui os pedidos entre as três máquinas virtuais no conjunto com balanceamento de carga. Para obter mais informações sobre os algoritmos de Balanceador de carga, consulte o [página de descrição geral do Balanceador de carga](load-balancer-overview.md#load-balancer-features).

Por predefinição, o Balanceador de carga do Azure distribui o tráfego de rede equitativamente entre várias instâncias de máquina virtual. Também pode configurar a afinidade de sessão. Para obter mais informações, consulte [modo de distribuição do Balanceador de carga](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> Balanceador de carga interno

Balanceador de carga interno apenas direciona o tráfego para recursos que estejam dentro de uma rede virtual ou uma VPN que utilizam para aceder à infraestrutura do Azure. No que esta respeita o Balanceador de carga interno é diferente de um balanceador de carga público. Infraestrutura do Azure restringe o acesso para os endereços IP de front-end com balanceamento de carga de uma rede virtual. Endereços IP de front-end e redes virtuais são expostos nunca diretamente para um ponto final de internet. Aplicações de linha de negócio internas executam no Azure e são acedidas a partir do Azure ou a partir dos recursos no local.

Balanceador de carga interno permite que os seguintes tipos de balanceamento de carga:

* Dentro de uma rede virtual: balanceamento de carga de VMs na rede virtual para um conjunto de VMs que residem na mesma rede virtual.
* Para uma rede virtual em vários locais: carregar o balanceamento de computadores no local para um conjunto de VMs que residem na mesma rede virtual. 
* Para aplicações de várias camadas: para aplicações de várias camadas de acesso à internet onde as camadas de back-end não são através da Internet de balanceamento de carga. As camadas de back-end requerem a partir da internet orientada de balanceamento de carga do tráfego camada (veja a figura 2).
* Para aplicações de linha de negócio: para aplicações de linha de negócio que estão alojadas no Azure sem hardware de Balanceador de carga adicional ou software de balanceamento de carga. Este cenário inclui a servidores no local que estão no conjunto de computadores cujo tráfego está com balanceamento de carga.

![exemplo de Balanceador de carga interno](./media/load-balancer-overview/IC744147.png)

**Figura 2 - aplicações de várias camadas utilizando ambos os balanceadores de carga interno e público de balanceamento de carga**

## <a name="pricing"></a>Preços
Padrão de Balanceador de carga é um produto cobrado com base no número de regras configuradas e todos os dados de entrada e saídos processados de balanceamento de carga. Para obter informações sobre preços padrão Balanceador de carga, visite o [preços do Balanceador de carga](https://azure.microsoft.com/pricing/details/load-balancer/) página.

Balanceador de carga básico é oferecido, sem encargos.

## <a name="sla"></a>SLA

Para obter informações sobre o SLA de Balanceador de carga padrão, visite o [SLA do Balanceador de carga](https://aka.ms/lbsla) página. 

## <a name="next-steps"></a>Passos Seguintes

- Reveja [padrão Balanceador de carga em mais detalhe](load-balancer-standard-overview.md)
- Saiba como utilizar [padrão Balanceador de carga e zonas de disponibilidade](load-balancer-standard-availability-zones.md)
- Saiba como utilizar [Balanceador de carga para ligações de saída](load-balancer-outbound-connections.md)
- Saiba mais sobre [portas HA de Balanceador de carga](load-balancer-ha-ports-overview.md)
- Saiba como utilizar [Balanceador de carga com várias Frontends](load-balancer-multivip-overview.md)
- Saiba mais sobre [pontos finais do serviço de VNet](../virtual-network/virtual-network-service-endpoints-overview.md)
- Saiba como criar um [Balanceador de carga público básico](load-balancer-get-started-internet-portal.md)


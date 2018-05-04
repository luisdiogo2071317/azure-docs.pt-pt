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
Customer intent: As an IT administrator, I want to learn more about the Azure Load Balancer service and what I can use it for.
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 2d9e0fc50bed4e8301a24a062407b490d688803d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="what-is-azure-load-balancer"></a>O que é o Balanceador de carga do Azure?

Com o Balanceador de carga do Azure pode dimensionar as suas aplicações e criar a elevada disponibilidade para os serviços. Balanceador de carga suporta cenários de entrada e saídos, proporciona baixa latência e débito alto e dimensiona até milhões de fluxos de todas as aplicações de TCP e UDP.  

Balanceador de carga distribui fluxos de entrada novo que chegam no balanceador de carga front-end para instâncias de conjunto back-end, de acordo com as regras e as sondas de estado de funcionamento. 

Além disso, um balanceador de carga públicos pode fornecer ligações de saída para as máquinas virtuais (VMs) no interior da rede virtual por traduzir os respetivos endereços IP privados para endereços IP públicos.

Balanceador de carga do Azure está disponível em duas SKUs: básico e padrão. Existem diferenças na escala, funcionalidades e preços. Qualquer cenário que é possível com Balanceador de carga básico também pode ser criado com o Balanceador de carga padrão, embora as abordagens poderão diferir ligeiramente. Como saber sobre o Balanceador de carga, é importante para se familiarizar com as noções básicas e diferenças de específico de SKU.

## <a name="why-use-load-balancer"></a>Porquê utilizar o Balanceador de carga? 

Pode utilizar o Balanceador de carga do Azure para:

* Balanceamento de carga receber tráfego de internet para as suas VMs. Esta configuração é conhecida como um [Balanceador de carga públicos](#publicloadbalancer).
* Tráfego de balanceamento de carga entre VMs no interior de uma rede virtual. Também pode contactar um end de front-de Balanceador de carga a partir de uma rede no local, num cenário híbrido. Ambos os cenários utilizam uma configuração que é conhecida como um [Balanceador de carga interno](#internalloadbalancer).
* Porta reencaminhar tráfego para uma porta específica em VMs específicas com regras de tradução (NAT) de endereços de rede de entrada.
* Fornecer [conectividade de saída](load-balancer-outbound-connections.md) para VMs dentro da sua rede virtual, utilizando um balanceador de carga público.


>[!NOTE]
> O Azure oferece um conjunto de soluções de balanceamento de carga completamente geridos para os seus cenários. Se estiver à procura de terminação de protocolo de segurança de camada de transporte (TLS) ("descarga de SSL") ou o pedido por-HTTP/HTTPS, o processamento de camada de aplicação, reveja [Gateway de aplicação](../application-gateway/application-gateway-introduction.md). Se estiver à procura de global DNS balanceamento de carga, reveja [Gestor de tráfego](../traffic-manager/traffic-manager-overview.md). Os cenários de ponto a ponto poderão beneficiar de combinar estas soluções conforme necessário.

## <a name="what-are-load-balancer-resources"></a>Quais são os recursos de Balanceador de carga?

Pode existir um recurso de Balanceador de carga como um balanceador de carga públicos ou um balanceador de carga interno. Funções do recurso de Balanceador de carga são expresso como um front-end, uma regra, uma pesquisa de estado de funcionamento e uma definição de conjunto back-end. Coloque o VMs no conjunto de back-end, especificando o conjunto de back-end da VM.

Recursos de Balanceador de carga são objetos dentro do qual pode express como o Azure deve programa respetiva infraestrutura de multi-inquilino para alcançar o cenário de que pretende criar. Não há nenhuma relação direta entre recursos de Balanceador de carga e a infraestrutura real. Criar um balanceador de carga não criar uma instância e capacidade está sempre disponível. 

## <a name="fundamental-load-balancer-features"></a>Funcionalidades fundamentais do Balanceador de carga

Balanceador de carga fornece as seguintes capacidades fundamentais para as aplicações TCP e UDP:

* **Balanceamento de carga**

    Com o Balanceador de carga do Azure, pode criar uma regra de balanceamento de carga para distribuir o tráfego chega front-end para instâncias do conjunto back-end. Balanceador de carga utiliza um algoritmo de hash para a distribuição dos fluxos de entrada e reescreve os cabeçalhos de fluxos para instâncias do conjunto back-end em conformidade. Um servidor está disponível para receber novos fluxos quando uma sonda do Estado de funcionamento indica um bom ponto final de back-end.
    
    Por predefinição, o Balanceador de carga utiliza um hash de 5 cadeias de identificação é composto por endereço IP de origem, porta de origem, endereço IP de destino, porta de destino e número de protocolo IP para mapear fluxos para servidores disponíveis. Pode optar por criar a afinidade com um endereço IP de origem específico ao aceitar para um hash de 2 ou 3 identificação para uma determinada regra. Todos os pacotes do mesmo fluxo de pacotes chegam na mesma instância atrás o front-end com balanceamento de carga. Quando o cliente inicia um novo fluxo do mesmo IP de origem, as alterações de porta de origem. Como resultado, 5-cadeias de identificação podem fazer com que o tráfego Ir para um endpoint diferente do back-end.

    Para obter mais informações, consulte [modo de distribuição do Balanceador de carga](load-balancer-distribution-mode.md). A imagem seguinte mostra a distribuição com base em hash:

    ![Distribuição com base em hash](./media/load-balancer-overview/load-balancer-distribution.png)

    *Figura: Com base em Hash de distribuição*

* **Reencaminhamento de porta**

    Com o Balanceador de carga, pode criar uma regra NAT de entrada para reencaminhar tráfego de porta de uma porta específica de um endereço IP Front-end específico para uma porta específica de uma instância específica do back-end dentro da rede virtual. Isto também é efetuado através de distribuição com base em hash mesma como balanceamento de carga. Cenários comuns para esta capacidade são as sessões de protocolo RDP (Remote Desktop Protocol) ou de Secure Shell (SSH) para instâncias de VM individuais dentro da rede Virtual do Azure. Pode mapear vários pontos finais internos para várias portas no mesmo endereço IP Front-end. Pode utilizá-los para administrar remotamente as suas VMs através da internet sem a necessidade de uma caixa de ir adicionais.

* **Aplicação agnóstico relativamente e transparente**

    Balanceador de carga não interage diretamente com o TCP ou UDP ou camada da aplicação e quaisquer TCP ou UDP baseada em cenário pode ser suportado. Por exemplo, embora o Balanceador de carga não terminar TLS próprio, pode criar e aumentar horizontalmente aplicações TLS através da utilização de Balanceador de carga e, em seguida, termine a ligação de TLS na própria VM. Balanceador de carga não fecha um fluxo e handshakes protocolo ocorrerem sempre diretamente entre o cliente e a instância de conjunto de back-end selecionado de hash. Por exemplo, um handshake TCP sempre ocorre entre o cliente e a VM de back-end selecionada. Uma resposta a um pedido para um front-end é uma resposta que é gerada a partir da VM de back-end. Desempenho de rede de saída do Balanceador de carga só está limitado pelo SKU de VM que escolher e fluxos permanecem alive por períodos demorados se o tempo limite de inatividade nunca foi atingido.

* **Reconfiguração automática**

    Balanceador de carga instantaneamente próprio reconfigura quando dimensionar as instâncias ou reduzir verticalmente. Adicionar ou remover as VMs de conjunto de back-end reconfigura o Balanceador de carga sem operações adicionais no recurso de Balanceador de carga.

* **Sondas de estado de funcionamento**

     Para determinar o estado de funcionamento de instâncias no conjunto de back-end, o Balanceador de carga utiliza sondas de estado de funcionamento que definir. Quando uma sonda não responder, o Balanceador de carga para o envio de novas ligações para as instâncias de mau estado de funcionamento. As ligações existentes não são afetadas e podem continuar até que a aplicação concluir o fluxo, ocorre um limite de tempo inativo, ou a VM está a ser encerrada.

    Três tipos de sondas que são suportados:

    - **Pesquisa HTTP personalizada**: pode utilizar esta pesquisa para criar a sua própria lógica personalizada para determinar o estado de funcionamento de uma instância de conjunto back-end. O Balanceador de carga sondas regularmente o ponto final (cada 15 segundos, por predefinição). A instância é considerada bom estado de funcionamento se responder com um HTTP 200 dentro do período de tempo limite (predefinição de segundos 31). Qualquer Estado diferente de HTTP 200 faz com que esta pesquisa falhar. Esta pesquisa também é útil para implementar a sua própria lógica para remover as instâncias de rotação do Balanceador de carga. Por exemplo, pode configurar a instância para devolver um Estado não 200 se a instância for superior a 90 por cento de CPU.  Esta pesquisa substitui a sonda de agente de convidados predefinida.

    - **Sonda personalizada TCP**: esta pesquisa depende de estabelecer uma sessão TCP com êxito para uma porta de pesquisa definidos. Desde que o serviço de escuta especificado na VM existe, esta pesquisa é concluída com êxito. Se a ligação é recusada, falha de pesquisa. Esta pesquisa substitui a sonda de agente de convidados predefinida.

    - **Pesquisa de agente do convidado (na plataforma como um VMs de serviço [PaaS] apenas)**: O balanceador de carga, também pode utilizar o agente convidado dentro da VM. O agente convidado escuta e responde com uma resposta de HTTP 200 OK apenas quando a instância está no estado pronto. Se o agente não conseguir responder com um HTTP 200 OK, o Balanceador de carga marca a instância como responder e deixa de envio de tráfego para essa instância. O Balanceador de carga continua a tentar aceder a instância. Se o agente convidado responde com uma HTTP 200, o Balanceador de carga envia tráfego a essa instância novamente. As pesquisas de agente do convidado são último recurso e não devem ser utilizadas quando o HTTP ou TCP configurações de sonda personalizada são possíveis. 
    
* **Ligações de saída (origem NAT)**

    Todos os fluxos de saída de endereços IP privados dentro da sua rede virtual para endereços IP públicos na internet podem ser convertidos para um endereço IP Front-end de Balanceador de carga. Quando um front-end público está associado a uma VM de back-end através de uma regra de balanceamento de carga, o Azure programas ligações de saída para ser automaticamente convertido no endereço IP público front-end. Isto também é denominado origem NAT (realizar o SNAT). Realizar o SNAT proporciona vantagens importantes:

    * Permite que uma atualização simples e recuperação após desastre de serviços, porque o front-end pode ser atribuído dinamicamente a outra instância do serviço.
    * -Facilita a gestão de lista (ACL) de controlo de acesso. As ACLs expressadas em termos de IPs front-end não alteram como escala de serviços ou reduzir verticalmente ou obterem implementada novamente.

    Para obter mais informações, consulte [ligações de saída](load-balancer-outbound-connections.md).

Padrão de Balanceador de carga tem capacidades específico de SKU adicionais para além destes Noções básicas. Reveja o resto deste artigo para obter mais detalhes.

## <a name="skus"></a> Comparação de SKU de Balanceador de carga

Balanceador de carga suporte básico e padrão SKUs, cada diferente na escala de cenário, funcionalidades e preços. Qualquer cenário que é possível com básico Balanceador de carga pode ser criado com o padrão de Balanceador de carga, bem como. Na verdade, as APIs para ambos os SKUs são semelhantes e invocado através da especificação de um SKU. A API para suportar SKUs para o Balanceador de carga e o IP público está disponível a partir da API de 2017-08-01. Ambos os SKUs de ter o mesmo API geral e a estrutura.

No entanto, consoante o SKU que escolher, a configuração do cenário completo poderão diferir ligeiramente. Documentação do Balanceador de carga chama a atenção quando um artigo só se aplica a um SKU específico. Comparar e compreender as diferenças, consulte a tabela seguinte. Para obter mais informações, consulte [descrição geral do Balanceador de carga padrão](load-balancer-standard-overview.md).

>[!NOTE]
> Se estiver a utilizar um cenário de conceção mais recente, considere utilizar o padrão de Balanceador de carga. 

VMs de autónomo, conjuntos de disponibilidade e conjuntos de dimensionamento VM podem ser ligados a apenas um SKU, nunca ambos. Quando utilizá-los com os endereços IP públicos, Balanceador de carga e o endereço IP público SKU tem de corresponder. Balanceador de carga e SKUs de IP público não são mutável.

_É uma melhor prática para especificar os SKUs explicitamente, apesar de ainda não é obrigatório._  Neste momento, as alterações necessárias estão a ser mantidas num mínimo. Se não for especificado um SKU, ela é interpretada como uma intenção para utilizar a versão de API de 2017-08-01 do SKU básico.

>[!IMPORTANT]
>Padrão de Balanceador de carga é um novo produto de Balanceador de carga e amplamente um superconjunto básico de Balanceador de carga. Não existem diferenças importantes e deliberate entre os dois produtos. Qualquer cenário de ponto-a-ponto que é possível com Balanceador de carga básico também pode ser criado com o padrão de Balanceador de carga. Se já estiver a utilizado ao balanceador de carga básico, deverá familiarizá-lo com padrão Balanceador de carga para compreender as alterações mais recentes no comportamento entre padrão e Basic e o impacto. Reveja esta secção cuidadosamente.

| | [Standard SKU](load-balancer-standard-overview.md) | SKU Básico |
| --- | --- | --- |
| Tamanho do conjunto de back-end | Mais de 1000 instâncias. | Até 100 instâncias. |
| Pontos finais de conjunto back-end | Qualquer VM com uma única rede virtual, incluindo um blend de VMs, conjuntos de disponibilidade e conjuntos de dimensionamento VM. | VMs num conjunto de disponibilidade único ou conjunto de dimensionamento VM. |
| Zonas de Disponibilidade do Azure | Com redundância de zona e zonal extremidades front-para a entrada e mapeamentos de fluxo de saída, saída continuam a vigorar após falha de zona, cross-zona balanceamento de carga. | / |
| Diagnóstico | Monitor do Azure, métricas multidimensionais, incluindo byte e contadores de pacotes, estado de funcionamento da sonda de estado, as tentativas de ligação (SIN de TCP), estado de funcionamento da ligação de saída (realizar o SNAT com êxito ou falhados fluxos), medidas plane de dados do Active Directory. | Análise de registos do Azure para público carregar balanceador apenas, o alerta de esgotamento de realizar o SNAT, contagem de estado de funcionamento do conjunto back-end. |
| HA portas | Balanceador de carga interno. | / |
| Seguro por predefinição | Por predefinição, fechado para IP e carga balanceador pontos finais públicos. Para o tráfego flua, um grupo de segurança de rede deve ser utilizado para explicitamente a lista branca entidades. | Abra o predefinido, grupo de segurança de rede opcional. |
| Ligações de saída | Vários frente termina com ativamente em por regras. Um cenário de saída _tem_ explicitamente criado para a VM ser capazes de utilizar a conectividade de saída. [Pontos finais do serviço de rede virtual](../virtual-network/virtual-network-service-endpoints-overview.md) podem ser acedidos sem conectividade de saída e não é considerado de processamento de dados. Todos os endereços IP públicos, incluindo os serviços do Azure PaaS que não estão disponíveis como pontos finais do serviço de rede virtual, devem ser atingidos através de conectividade de saída e a contagem na direção de processamento de dados. Quando um balanceador de carga interno está a servir uma VM, ligações de saída através de predefinição realizar o SNAT não estão disponíveis. Programação de realizar o SNAT saída é o protocolo de transporte específicos, com base no protocolo de regra de balanceamento de carga de entrada. | Único front-end, selecionado aleatória, quando existem vários extremidades front. Quando um balanceador de carga interno está a servir uma VM, é utilizada a predefinição realizar o SNAT. |
| Vários extremidades frontal | Entrada e saída. | Só de entrada. |
| Operações de gestão | Segundos de < 30 a maioria das operações. | 60-90 segundos típica. |
| SLA | 99,99 percentagem de para um caminho de dados com duas VMs de bom estado de funcionamento. | Implícita no SLA de VM. | 
| Preços | Os encargos são baseados no número de regras e processados de dados de entrada ou saída que estão associados o recursos.  | Sem qualquer encargo. |

Para obter mais informações, consulte [os limites de serviço para o Balanceador de carga](https://aka.ms/lblimits). Para detalhes de Balanceador de carga padrão, consulte [descrição geral](load-balancer-standard-overview.md), [preços](https://aka.ms/lbpricing), e [SLA](https://aka.ms/lbsla).

## <a name="concepts"></a>Conceitos

### <a name = "publicloadbalancer"></a>Balanceador de carga públicos

Um balanceador de carga público mapeia o público IP endereço e número de porta de tráfego de entrada para o privada IP endereço e número de porta da VM e vice versa para o tráfego de resposta da VM. Ao aplicar regras de balanceamento de carga, pode distribuir tipos específicos de tráfego entre várias VMs ou serviços. Por exemplo, pode propagar-se a carga do tráfego de pedido web em vários servidores web.

A figura seguinte mostra um ponto final com balanceamento de carga para o tráfego da web que é partilhado entre três VMs para a porta TCP pública e privada 80. Estas três VMs estão num conjunto com balanceamento de carga.

![exemplo de Balanceador de carga públicos](./media/load-balancer-overview/IC727496.png)

*Figura: Carregar balanceamento tráfego web, utilizando um balanceador de carga público*

Quando os clientes de internet enviam pedidos de página Web para o endereço IP público de uma aplicação web na porta TCP 80, o Azure Load Balancer distribui os pedidos de três VMs no conjunto com balanceamento de carga. Para obter mais informações sobre os algoritmos de Balanceador de carga, consulte o [página de descrição geral do Balanceador de carga](load-balancer-overview.md#load-balancer-features).

Por predefinição, o Balanceador de carga do Azure distribui o tráfego de rede equitativamente entre várias instâncias VM. Também pode configurar a afinidade de sessão. Para obter mais informações, consulte [modo de distribuição do Balanceador de carga](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> Balanceador de carga interno

Um balanceador de carga interno direciona o tráfego apenas para recursos que estejam dentro de uma rede virtual ou uma VPN que utilizam para aceder à infraestrutura do Azure. No que esta respeita, um balanceador de carga interno difere de um balanceador de carga público. Infraestrutura do Azure restringe o acesso ao balanceamento de carga front-end endereços IP de uma rede virtual. Endereços IP Front-end e redes virtuais são expostos nunca diretamente para um ponto final de internet. Aplicações de linha de negócio internas executam no Azure e são acedidas a partir do Azure ou a partir dos recursos no local.

Um balanceador de carga interno permite que os seguintes tipos de balanceamento de carga:

* **Dentro de uma rede virtual**: balanceamento de carga de VMs na rede virtual para um conjunto de VMs que residem na mesma rede virtual.
* **Para uma rede virtual em vários locais**: carregar o balanceamento de computadores no local para um conjunto de VMs que residem na mesma rede virtual. 
* **Para aplicações de várias camadas**: para aplicações de várias camadas de acesso à internet onde as camadas de back-end não são através da Internet de balanceamento de carga. Os escalões de back-end requerem balanceamento de carga do tráfego da internet orientada camada (consulte a figura seguinte).
* **Para aplicações de linha de negócio**: para aplicações de linha de negócio que estão alojadas no Azure sem hardware de Balanceador de carga adicional ou software de balanceamento de carga. Este cenário inclui a servidores no local que estão no conjunto de computadores cujo tráfego está com balanceamento de carga.

![exemplo de Balanceador de carga interno](./media/load-balancer-overview/IC744147.png)

*Figura: O balanceamento de carga aplicações de várias camadas utilizando ambos os balanceadores de carga interno e pública*

## <a name="pricing"></a>Preços
Utilização de Balanceador de carga padrão é cobrada com base no número de regras de balanceamento de carga configurados e a quantidade de dados de entrada e saídas processados. Standard Balanceador de carga preços informação, vá para o [preços do Balanceador de carga](https://azure.microsoft.com/pricing/details/load-balancer/) página.

Balanceador de carga básico é oferecido, sem encargos.

## <a name="sla"></a>SLA

Para obter informações sobre o SLA de Balanceador de carga padrão, vá para o [SLA do Balanceador de carga](https://aka.ms/lbsla) página. 

## <a name="next-steps"></a>Passos Seguintes

Tem agora uma descrição geral do Balanceador de carga do Azure. Para começar com um balanceador de carga a utilizar, criar um, crie as VMs com uma extensão personalizada do IIS instalada e balanceamento de carga da aplicação web entre as VMs. Para saber como, consulte o [criar um balanceador de carga básico](quickstart-create-basic-load-balancer-portal.md) início rápido.

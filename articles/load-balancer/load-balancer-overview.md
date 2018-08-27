---
title: Descrição geral do Balanceador de carga do Azure | Documentos da Microsoft
description: Descrição geral das funcionalidades, a arquitetura e a implementação de Balanceador de carga do Azure. Saiba como funciona o Balanceador de carga e tirar partido dos mesmos na cloud.
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
ms.date: 08/20/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 618b00906a799e1b8cfcfac5ee6bcc3a714c2f87
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42918747"
---
# <a name="what-is-azure-load-balancer"></a>O que é o Balanceador de carga do Azure?

Com o Balanceador de carga do Azure pode dimensionar as suas aplicações e criar disponibilidade elevada para os seus serviços. Balanceador de carga dá suporte a cenários de entrada e saídos, oferece baixa latência e alto débito e aumenta até milhões de fluxos para todas as aplicações TCP e UDP.  

Balanceador de carga distribui novos fluxos de entrada que chegam no front-end de Balanceador de carga para instâncias de conjunto de back-end, de acordo com as regras e sondas de estado de funcionamento. 

Além disso, um balanceador de carga público pode fornecer as ligações de saída para máquinas virtuais (VMs) no interior da rede virtual ao traduzir os respetivos endereços IP privados para endereços IP públicos.

O Balanceador de carga do Azure está disponível em dois SKUs: básico e Standard. Existem diferenças em escala, funcionalidades e preços. Também é possível criar qualquer cenário que pode fazer com o Balanceador de carga básico com o Balanceador de carga Standard, embora as abordagens podem ser ligeiramente diferentes. Como saber sobre o Balanceador de carga, é importante para se familiarizar com os conceitos básicos e diferenças de específico de SKU.

## <a name="why-use-load-balancer"></a>Porquê utilizar o Balanceador de carga? 

Pode utilizar o Azure Load Balancer para:

* Tráfego da internet de entrada balanceamento de carga para as suas VMs. Esta configuração é conhecida como uma [Balanceador de carga público](#publicloadbalancer).
* Tráfego de balanceamento de carga entre VMs numa rede virtual. Também pode aceder um balanceador de carga front-end da partir de uma rede no local, num cenário híbrido. Ambos os cenários usam uma configuração que é conhecida como um [Balanceador de carga interno](#internalloadbalancer).
* Tráfego de encaminhamento de porta para uma porta específica no VMs específicas com regras de tradução (NAT) de endereço de rede de entrada.
* Fornecer [conectividade de saída](load-balancer-outbound-connections.md) para as VMs no interior da rede virtual, utilizando um balanceador de carga público.


>[!NOTE]
> O Azure oferece um conjunto de soluções de balanceamento de carga totalmente geridas para os seus cenários. Se estiver à procura de terminação de protocolo TLS (Transport Layer Security) (“descarga de SSL”) ou de processamento de camada de aplicação por pedido HTTP/HTTPS, reveja [Gateway de Aplicação](../application-gateway/application-gateway-introduction.md). Se estiver à procura de global DNS balanceamento de carga, reveja [Gestor de tráfego](../traffic-manager/traffic-manager-overview.md). Combinar estas soluções conforme necessário poderá trazer benefícios aos seus cenários completos.

## <a name="what-are-load-balancer-resources"></a>Quais são os recursos do Balanceador de carga?

Um recurso de Balanceador de carga pode existir como um balanceador de carga público ou um balanceador de carga interno. Funções do recurso de Balanceador de carga são expressos como um front-end, uma regra, uma sonda de estado de funcionamento e uma definição de conjunto de back-end. Coloque as VMs para o pool de back-end, especificando o conjunto de back-end da VM.

Recursos do Balanceador de carga são objetos nos quais pode expressar como o Azure deve programar a sua infraestrutura de multi-inquilino para alcançar o cenário que deseja criar. Não existe nenhuma relação direta entre recursos do Balanceador de carga e a infraestrutura real. Criar um balanceador de carga não cria uma instância, e a capacidade está sempre disponível. 

## <a name="fundamental-load-balancer-features"></a>Recursos fundamentais do Balanceador de carga

Balanceador de carga fornece as seguintes capacidades fundamentais para as aplicações TCP e UDP:

* **Balanceamento de carga**

    Com o Balanceador de carga do Azure, pode criar uma regra de balanceamento de carga para distribuir o tráfego que chega ao front-end para instâncias de conjunto de back-end. Balanceador de carga utiliza um algoritmo com base em hash para a distribuição de fluxos de entrada e reescreve os cabeçalhos de fluxos para instâncias de conjunto de back-end em conformidade. Um servidor está disponível para receber novos fluxos quando uma sonda de estado de funcionamento indica que um ponto de final de back-end em bom estado.
    
    Por predefinição, o Balanceador de carga utiliza um hash de 5 cadeias de identificação composto por endereço IP de origem, porta de origem, endereço IP de destino, porta de destino e número de protocolo IP para mapear fluxos para servidores disponíveis. Pode optar por criar afinidade com um endereço IP de origem específico ao optar por um hash de 2 ou 3-cadeias de identificação para uma determinada regra. Todos os pacotes do mesmo fluxo de pacote chegarem na mesma instância por trás do front-end com balanceamento de carga. Quando o cliente inicia um novo fluxo do mesmo IP de origem, as alterações de porta de origem. Como resultado, a 5 cadeias de identificação poderão fazer com que o tráfego Ir para um ponto de final de back-end diferente.

    Para obter mais informações, consulte [modo de distribuição de Balanceador de carga](load-balancer-distribution-mode.md). A imagem seguinte mostra a distribuição baseada em hash:

    ![Distribuição baseada em hash](./media/load-balancer-overview/load-balancer-distribution.png)

    *Figura: A distribuição de Hash com base*

* **Encaminhamento de porta**

    Com o Balanceador de carga, pode criar uma regra NAT de entrada para reencaminhar tráfego de porta de uma porta específica de um endereço IP de front-end específicas para uma porta específica de uma instância específica de back-end dentro da rede virtual. Isso também é realizado através da distribuição baseada em hash mesmo como balanceamento de carga. Cenários comuns para esta capacidade são sessões do protocolo RDP (Remote Desktop) ou Secure Shell (SSH) para instâncias de VM individuais dentro da rede Virtual do Azure. Pode mapear vários pontos de extremidade internos para as várias portas no mesmo endereço IP de front-end. Pode usá-los para administrar remotamente as suas VMs através da internet sem a necessidade de uma jumpbox adicionais.

* **Aplicação agnóstica e transparente**

    Balanceador de carga não interage diretamente com o TCP ou UDP ou a camada de aplicativo e qualquer TCP ou cenário de aplicativo de UDP pode ser suportado.  Balanceador de carga não terminar ou se originam fluxos, interagir com o payload do fluxo, não fornece nenhuma função de gateway de camada de aplicativo e sempre ocorrem handshakes de protocolo diretamente entre o cliente e a instância de conjunto de back-end.  Uma resposta a um fluxo de entrada é sempre uma resposta de uma máquina virtual.  Quando chega o fluxo na máquina virtual, o endereço IP de origem original também é preservado.  Alguns exemplos ilustram melhor transparência:
    - Cada ponto de extremidade é apenas respondido por uma VM.  Por exemplo, um handshake TCP sempre ocorre entre o cliente e o VM de back-end selecionado.  Uma resposta a um pedido para um front-end é uma resposta gerada pela VM de back-end. Quando a conectividade com um front-end validar com êxito, que está a validar a conectividade de ponto a ponto para, pelo menos, uma máquina de virtual de back-end.
    - Payloads de aplicativo são transparentes para o Balanceador de carga e qualquer UDP ou TCP de aplicações pode ser suportada. Para cargas de trabalho que requerem por processamento de pedidos HTTP ou manipulação de payloads de camada de aplicativo (por exemplo, análise de HTTP URLs), deve utilizar um balanceador de carga de camada 7, como [Gateway de aplicação](https://azure.microsoft.com/services/application-gateway).
    - Uma vez que o Balanceador de carga é agnóstico para o payload TCP e descarga de TLS ("SSL") não for fornecido, pode criar cenários criptografados de ponto a ponto com o Balanceador de carga e obter grandes aumentar horizontalmente para aplicativos de TLS ao terminar a ligação de TLS a própria VM.  Por exemplo, sua capacidade de chaves de sessão TLS é limitada apenas pelo tipo e número de VMs que adiciona ao agrupamento de back-end.  Se necessitar de "Descarga de SSL", tratamento de camada de aplicativo ou pretende delegar a gestão de certificados para o Azure, deve utilizar o Balanceador de carga de camada 7 do Azure [Gateway de aplicação](https://azure.microsoft.com/services/application-gateway) em vez disso.
        

* **Reconfiguração automática**

    Balanceador de carga instantaneamente reconfigura-se de quando instâncias de aumentar ou reduzir verticalmente. Adicionar ou remover VMs de conjunto de back-end reconfigura o Balanceador de carga sem operações adicionais no recurso do Balanceador de carga.

* **Sondas de estado de funcionamento**

    Para determinar o estado de funcionamento de instâncias no conjunto de back-end, o Balanceador de carga utiliza sondas de estado de funcionamento que definir. Quando uma sonda não consegue responder, o Balanceador de carga para enviar ligações novas para as instâncias em mau estado de funcionamento. Ligações existentes não são afetadas e podem continuar até que a aplicação concluir o fluxo, ocorre um tempo limite de inatividade, ou a VM é encerrada.
     
    Balanceador de carga fornece [tipos de sonda de estado de funcionamento de diferentes](load-balancer-custom-probe-overview.md#types) para pontos de extremidade TCP, HTTP e HTTPS.

    Além disso, ao utilizar os serviços de nuvem de clássico, um tipo adicional é permitido: [agente convidado](load-balancer-custom-probe-overview.md#guestagent).  Isso deve ser a considerar para ser uma sonda de estado de funcionamento de último recurso e não é recomendado quando outras opções são viáveis.
    
* **Ligações de saída (SNAT)**

    Todos os fluxos de saída de endereços IP privados no interior da rede virtual para endereços IP públicos na internet podem ser traduzidos para um endereço IP de front-end de Balanceador de carga. Quando um front-end público está ligado a um VM de back-end por meio de uma regra de balanceamento de carga, o Azure programas ligações de saída para ser convertido automaticamente para o endereço IP público de front-end.

    * Ative uma atualização simples e recuperação após desastre de serviços, porque o front-end pode ser mapeado dinamicamente para outra instância do serviço.
    * Mais fácil acesso (ACL) lista gestão de controlo para. ACLs expressas em termos de front-end IPs, não altere como o dimensionamento de serviços ou reduzir verticalmente ou obter reimplantados.  A tradução de ligações de saída para um número menor de endereços IP que máquinas podem reduzir a carga de listas de permissões.

    Para obter mais informações, consulte [ligações de saída](load-balancer-outbound-connections.md).

Balanceador de carga standard tem capacidades específico de SKU adicionais além desses conceitos básicos. Reveja o resto deste artigo para obter detalhes.

## <a name="skus"></a> Comparação de SKU do Balanceador de carga

Load Balancer suporta os SKUs básico e padrão, cada um diferentes na escala de cenário, recursos e preços. Qualquer cenário que pode fazer com o Balanceador de carga básico pode ser criado com o Balanceador de carga Standard também. Na verdade, as APIs para ambos os SKUs são semelhantes e invocado por meio da especificação de um SKU. A API para SKUs de suporte para o Balanceador de carga e o IP público está disponível a partir da API de 2017-08-01. Ambos os SKUs tenham a mesma API geral e a estrutura.

No entanto, dependendo de qual SKU que escolher, a configuração do cenário completo poderá ser ligeiramente diferentes. Documentação do Load Balancer chama quando um artigo só se aplica a um SKU específico. Para comparar e compreender as diferenças, veja a seguinte tabela. Para obter mais informações, consulte [descrição geral do Balanceador de carga Standard](load-balancer-standard-overview.md).

>[!NOTE]
> Novos designs devem adotar o Balanceador de carga Standard. 

VMs autónomas e conjuntos de disponibilidade e conjuntos de dimensionamento de máquina virtual podem ser conectados a apenas um SKU, não ambos. Quando usá-los com endereços IP públicos, Balanceador de carga e o endereço IP público SKU tem de corresponder. SKUs de IP público do Balanceador de carga e não são mutáveis.

_É melhor prática para especificar os SKUs explicitamente, mesmo que ainda não é obrigatório._  Neste momento, as alterações necessárias são mantidas num mínimo. Se não for especificado um SKU, ela é interpretada como uma intenção de utilizar a versão de API de 2017-08-01 de SKU básico.

>[!IMPORTANT]
>Balanceador de carga Standard é um novo produto de Balanceador de carga e amplamente um superconjunto do Balanceador de carga básico. Existem diferenças importantes ponderada e deliberadas entre os dois produtos. Também é possível criar qualquer cenário de ponto-a-ponto que é possível com o Balanceador de carga básico com o Balanceador de carga Standard. Se já está acostumado a Balanceador de carga básico, deve se familiarizar com o Balanceador de carga Standard para compreender as alterações mais recentes no comportamento entre o Standard e Basic e seu impacto. Rever esta secção com cuidado.

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

Para obter mais informações, consulte [limites de serviço para o Balanceador de carga](https://aka.ms/lblimits). Para obter detalhes de Balanceador de carga Standard, veja [descrição geral](load-balancer-standard-overview.md), [preços](https://aka.ms/lbpricing), e [SLA](https://aka.ms/lbsla).

## <a name="concepts"></a>Conceitos

### <a name = "publicloadbalancer"></a>Balanceador de carga público

Um balanceador de carga público mapeia o IP público endereço e porta número do tráfego de entrada para o IP endereço e a porta número privado da VM e vice-versa para o tráfego de resposta da VM. Ao aplicar regras de balanceamento de carga, pode distribuir os tipos específicos de tráfego em várias VMs ou serviços. Por exemplo, pode distribuir a carga de tráfego de solicitação da web em vários servidores web.

A figura seguinte mostra um ponto final com balanceamento de carga para tráfego da web que é partilhado entre três VMs para o público e da porta TCP 80. São esses três VMs num conjunto com balanceamento de carga.

![Exemplo de Balanceador de carga público](./media/load-balancer-overview/IC727496.png)

*Figura: Carregar o balanceamento de tráfego web com um balanceador de carga público*

Quando os clientes de internet enviam solicitações de página Web para o endereço IP público de uma aplicação web na porta TCP 80, o Azure Load Balancer distribui os pedidos entre três VMs no conjunto com balanceamento de carga. Para obter mais informações sobre os algoritmos de Balanceador de carga, consulte a [funcionalidades do Balanceador de carga](load-balancer-overview.md##fundamental-load-balancer-features) seção deste artigo.

Por predefinição, o Balanceador de carga do Azure distribui o tráfego de rede igualmente entre várias instâncias de VM. Também pode configurar a afinidade de sessão. Para obter mais informações, consulte [modo de distribuição de Balanceador de carga](load-balancer-distribution-mode.md).

### <a name = "internalloadbalancer"></a> Balanceador de carga interno

Um balanceador de carga interno direciona o tráfego apenas a recursos que estão dentro de uma rede virtual ou utilizem uma VPN para aceder à infraestrutura do Azure. Nesse sentido, um balanceador de carga interno é diferente de um balanceador de carga público. Infraestrutura do Azure restringe o acesso para os endereços IP com balanceamento de carga front-end de uma rede virtual. endereços IP de front-end e redes virtuais são expostas nunca diretamente para um ponto de extremidade de internet. Aplicações de linha de negócio internas são executadas no Azure e são acessadas a partir do Azure ou a partir de recursos no local.

Um balanceador de carga interno que permite que os seguintes tipos de balanceamento de carga:

* **Dentro de uma rede virtual**: balanceamento de carga de VMs na rede virtual para um conjunto de VMs que residam na mesma rede virtual.
* **Para uma rede virtual em vários locais**: balanceamento de carga de computadores no local para um conjunto de VMs que residam na mesma rede virtual. 
* **Para aplicações de várias camadas**: balanceamento de carga para aplicações de várias camadas de acesso à internet em que os escalões de back-end não são destinados à internet. Os escalões de back-end requerem balanceamento de carga tráfego da internet orientada para camada (consulte a figura seguinte).
* **Para aplicações de linha de negócio**: balanceamento de carga para aplicativos de linha de negócio que estão alojados no Azure sem hardware de Balanceador de carga adicional ou software. Este cenário inclui servidores no local que estão no conjunto de computadores cujo tráfego está com balanceamento de carga.

![Exemplo de Balanceador de carga interno](./media/load-balancer-overview/IC744147.png)

*Figura: Balanceamento de carga aplicações de várias camadas, com Balanceador de carga públicos e internos*

## <a name="pricing"></a>Preços
Utilização de Balanceador de carga Standard é cobrada com base no número de regras de balanceamento de carga configurados e a quantidade de dados de entrada e de saída processados. Para o Balanceador de carga Standard preços informações, vá para o [preços do Balanceador de carga](https://azure.microsoft.com/pricing/details/load-balancer/) página.

Balanceador de carga básico é disponibilizado sem custos.

## <a name="sla"></a>SLA

Para obter informações sobre o SLA de Balanceador de carga Standard, vá para o [SLA de Balanceador de carga](https://aka.ms/lbsla) página. 

## <a name="limitations"></a>Limitações

- Balanceador de carga é um produto TCP ou UDP para balanceamento de carga e encaminhamento de porta para estes protocolos IP específicos.  Regras de balanceamento de carga e regras NAT de entrada são suportadas para TCP e UDP e não são suportadas para outros protocolos IP, incluindo o ICMP. Balanceador de carga não encerra, responder ou, caso contrário, interagir com a carga de um fluxo UDP ou TCP. Não é um proxy. Validação com êxito de conectividade para um front-end tem de efetuar lugar em banda com o mesmo protocolo usado numa entrada ou balanceamento regra NAT de carga (TCP ou UDP) _e_ , pelo menos, uma das suas máquinas virtuais tem de gerar uma resposta para um cliente ver uma resposta de um front-end.  Não receber uma resposta em banda a partir do front-end de Balanceador de carga indica que nenhuma máquina virtual foram capaz de responder.  Não é possível interagir com um front-end de Balanceador de carga sem uma máquina virtual capaz de responder.  Isso também se aplica a ligações de saída onde [masquerade de porta SNAT](load-balancer-outbound-connections.md#snat) é apenas suportado para TCP e UDP; outros protocolos IP incluindo ICMP também irão falhar.  Atribua um endereço de IP público de nível de instância para atenuar.
- Ao contrário dos balanceadores de carga público que fornecem [ligações de saída](load-balancer-outbound-connections.md) durante a transição de endereços IP privados dentro da rede virtual para endereços IP públicos, balanceadores de carga interno não traduzir saída teve origem ligações para o front-end de um balanceador de carga interno como ambos estão no espaço de endereços IP privados.  Isso evita a possibilidade de exaustão de porta SNAT dentro exclusivo espaço de endereços IP interno onde a tradução não é necessária.  O efeito é que, se um fluxo de saída de uma VM no conjunto de back-end tenta um fluxo para front-end de Balanceador de carga interno no qual conjunto reside _e_ é mapeado para ele mesmo, as duas primeiras partes do fluxo não coincidem e o fluxo irá falhar.  Se o fluxo não mapeado voltar para a mesma VM no agrupamento de back-end que criou o fluxo para o front-end, o fluxo será concluída com êxito.   Quando o fluxo é mapeado para ele mesmo o fluxo de saída é apresentada se originar de VM para o front-end e o fluxo de entrada correspondente é exibido originar da VM para si próprio. Do ponto de vista para o sistema operacional convidado, as partes de entrada e saídas do mesmo fluxo não correspondem dentro da máquina virtual. A pilha TCP não irá reconhecer estas metades do mesmo fluxo como parte do mesmo fluxo de origem e de destino não coincidir com.  Quando o fluxo é mapeado para qualquer outra VM no conjunto de back-end, corresponderão ao metades do fluxo e a VM pode responder com êxito para o fluxo.  O sintoma para este cenário é o tempo limite das conexões intermitentes quando o fluxo retorna para o mesmo back-end o fluxo de origem. Existem várias soluções de alternativas comuns para atingir com fiabilidade a este cenário (com origem fluxos a partir de um conjunto de back-end para back-end conjuntos respectivo interno Balanceador de carga front-end) que incluem a inserção de uma camada de proxy por detrás do Balanceador de carga interno ou [usando regras de estilo DSR](load-balancer-multivip-overview.md).  Os clientes podem combinar um balanceador de carga interno com o proxy de terceiros ou substituto interno 3º [Gateway de aplicação](../application-gateway/application-gateway-introduction.md) para cenários de proxy limitados a HTTP/HTTPS. Embora pudesse usar um balanceador de carga público para atenuar, o cenário de resultante é propenso a [esgotamento de SNAT](load-balancer-outbound-connections.md#snat) e deve ser evitada, a menos que gerenciada com cautela.

## <a name="next-steps"></a>Passos Seguintes

Tem agora uma descrição geral do Balanceador de carga do Azure. Para começar a utilizar com o uso de um balanceador de carga, crie uma, crie VMs com uma extensão personalizada do IIS instalado e o balanceamento de carga da aplicação web entre as VMs. Para saber como, veja a [criar um balanceador de carga básico](quickstart-create-basic-load-balancer-portal.md) início rápido.

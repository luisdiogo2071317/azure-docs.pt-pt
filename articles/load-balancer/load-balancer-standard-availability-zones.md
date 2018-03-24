---
title: Zonas de disponibilidade e de Balanceador de carga Standard do Azure | Microsoft Docs
description: Balanceador de carga padrão e zonas de disponibilidade
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
ms.openlocfilehash: 70b39b854a3b7cb28716d3cb290998690dbeb549
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="standard-load-balancer-and-availability-zones"></a>Balanceador de carga padrão e zonas de disponibilidade

SKU Standard do Azure do Balanceador de carga suporta [disponibilidade zonas](../availability-zones/az-overview.md) cenários. Vários novos conceitos estão disponíveis com o Balanceador de carga padrão, que lhe permitem otimizar disponibilidade no seu cenário de ponto a ponto ao alinhar recursos com zonas, bem como distribuí-los através de zonas.  Reveja [disponibilidade zonas](../availability-zones/az-overview.md) para obter orientações sobre o que são zonas de disponibilidade, as regiões que suportam atualmente zonas de disponibilidade e de outras relacionadas com produtos e conceitos. Zonas de disponibilidade em combinação com o padrão de Balanceador de carga é um conjunto de funcionalidades amplo e flexível que pode criar vários cenários diferentes.  Reveja este documento não compreende-las [conceitos](#concepts) e cenário fundamental [conceber de orientações](#design).

>[!NOTE]
> O SKU de padrão de Balanceador de carga está atualmente em pré-visualização. Durante a pré-visualização, a funcionalidade não pode ter o mesmo nível de disponibilidade e fiabilidade como versão de funcionalidades que estão em geral disponibilidade. Para obter mais informações, consulte [Termos de Utilização Suplementares do Microsoft Azure para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Utilizar geralmente disponível [SKU básico de Balanceador de carga](load-balancer-overview.md) para os serviços de produção. Para utilizar [pré-visualização de zonas de disponibilidade](https://aka.ms/availabilityzones) com esta pré-visualização requer um [separado inscrição](https://aka.ms/availabilityzones), além de inscrever-se para o Balanceador de carga [pré-visualização padrão](#preview-sign-up).

## <a name="concepts"></a> Conceitos de zonas de disponibilidade aplicados ao balanceador de carga

Não há nenhuma relação direta entre os recursos de Balanceador de carga e a infraestrutura real; criar um balanceador de carga não criar uma instância. Recursos de Balanceador de carga são objetos dentro do qual pode express como o Azure deve programa respetiva infraestrutura de multi-inquilino prebuilt para alcançar o cenário de que pretende criar.  Isto é significativo no contexto das zonas de disponibilidade como um único recurso de Balanceador de carga pode controlar a programação da infraestrutura em várias zonas de disponibilidade enquanto um serviço com redundância de zona é apresentado como um recurso de um ponto de vista do cliente.

Funções de um recurso de Balanceador de carga são expresso como um front-end, uma regra, uma pesquisa de estado de funcionamento e uma definição de conjunto de back-end.

No contexto das zonas de disponibilidade, o comportamento e as propriedades de um recurso de Balanceador de carga descritas como zonal ou com redundância de zona.  Zona redundante e zonal descrevem zonality de uma propriedade.  No contexto de Balanceador de carga, com redundância de zona sempre significa *todas as zonas* e significa zonal guaranteeing o serviço para um *única zona*.

Público e interno Load Balancer suporta cenários com redundância de zona e zonal e ambos podem direcionar o tráfego através de zonas conforme necessário (*balanceamento de carga entre-zona*).

Um recurso de Balanceador de carga em si é regional e nunca zonal.  E uma VNet e sub-rede estão sempre regional e nunca zonal.

### <a name="frontend"></a>Front-end

Um front-end de Balanceador de carga é uma configuração de IP de front-end que façam referência um recurso de endereço IP público ou um endereço IP privado dentro da sub-rede de um recurso de rede virtual.  Compõe o ponto final com balanceamento de carga em que o seu serviço é exposto.

Um recurso de Balanceador de carga pode conter frontends zonal e com redundância de zona em simultâneo.

Quando um recurso IP público tem sido garantido para uma zona, o zonality (ou a sua falta) não é mutável.  Se pretender alterar ou omita o zonality de front-end IP público, tem de recriar o IP público da zona apropriada.  

Pode alterar o zonality de front-end de um balanceador de carga interno, remover e recriar o front-end, alterar ou omitindo a zonality.

Ao utilizar vários frontends, reveja [frontends vários Balanceador de carga](load-balancer-multivip-overview.md) para considerações importantes.

#### <a name="zone-redundant-by-default"></a>Zona redundante por predefinição

Numa região com zonas de disponibilidade, um front-end de Balanceador de carga padrão é com redundância de zona por predefinição.  Um endereço IP de front-end único pode sobreviver a falha de zona e pode ser utilizado para aceder a todos os membros do conjunto de back-end independentemente da zona. Isto não significa que o caminho de dados hitless, mas qualquer tentativas ou reestablishment será concluída com êxito. Os esquemas de redundância DNS não são necessários. Único endereço IP de front-end é fornecido em simultâneo por implementações de infraestrutura independentes em cada zona de disponibilidade.  Zona redundante significa que todos os fluxos de entrada ou saídos são servidos pelo todas as zonas de disponibilidade numa região em simultâneo com um único endereço IP.

Um ou mais zonas de disponibilidade pode falhar e o caminho de dados desde uma zona nos permanecem região survives bom estado de funcionamento. Configuração com redundância de zona é a predefinição e não requer nenhuma ação adicional.  Quando uma região obtiver a possibilidade de suportar zonas de disponibilidade, um front-end existente torna-se com redundância de zona automaticamente.

Utilize o seguinte script para criar um endereço IP público com redundância de zona para o Balanceador de carga interno padrão. Se estiver a utilizar modelos de Gestor de recursos existentes na sua configuração, adicione o **sku** secção para estes modelos.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "public_ip_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
```

Utilize o seguinte script para criar um endereço IP de front-end com redundância de zona para o Balanceador de carga interno padrão. Se estiver a utilizar modelos de Gestor de recursos existentes na sua configuração, adicione o **sku** secção para estes modelos.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "zone_redundant_frontend",
                        "properties": {
                            "subnet": {
                                "Id": "[variables('subnetRef')]"
                            },
                            "privateIPAddress": "10.0.0.6",
                            "privateIPAllocationMethod": "Static"
                        }
                    },
                ],
```

#### <a name="optional-zone-guarantee"></a>Garantia de zona opcional

Pode optar por ter um front-end garantida para uma zona único, que é conhecido como um *front-end zonal*.  Isto significa que qualquer fluxo de entrada ou saído é fornecido por uma único zona numa região.  O fate de partilhas de front-end com o estado de funcionamento da zona.  O caminho de dados não é afetado por falhas em zonas que não sejam de onde foi garantida. Pode utilizar zonal frontends para expor um endereço IP por zona de disponibilidade.  Além disso, pode consumir zonal frontends diretamente ou, quando o front-end é composta por endereços IP públicos, integrá-los com uma produto, como de balanceamento de carga DNS [Gestor de tráfego](../traffic-manager/traffic-manager-overview.md) e utilizar um único nome DNS, que um cliente irá resolver para vários endereços IP zonal.  Também pode utilizar este expor por zona com balanceamento de carga pontos finais para monitorizar individualmente cada zona.  Se pretender que o se misturem estes conceitos (com redundância de zona e zonal para mesmo back-end), reveja [frontends vários Azure Balanceador de carga](/load-balancer-multivip-overview.md).

Para um público Balanceador de carga front-end, adicione um *zonas* parâmetro para o IP público referenciado pela configuração de IP de front-end.  

Para um interno Balanceador de carga front-end, adicione um *zonas* parâmetro para a configuração de IP de front-end de Balanceador de carga interno. O front-end zonal faz com que o Balanceador de carga garantir um endereço IP numa sub-rede para uma zona específica.

Utilize o seguinte script para criar um endereço IP público padrão zonal na disponibilidade zona 1. Se estiver a utilizar modelos de Gestor de recursos existentes na sua configuração, adicione o **sku** secção para estes modelos.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "public_ip_standard",
            "location": "region",
            "zones": [ "1" ],
            "sku":
            {
                "name": "Standard"
            },
```

Utilize o seguinte script para criar um interno padrão Balanceador de carga front-end na disponibilidade zona 1.

Se estiver a utilizar modelos de Gestor de recursos existentes na sua configuração, adicione o **sku** secção para estes modelos. Além disso, defina o **zonas** propriedade da configuração de IP Front-end para o recurso subordinado.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "zonal_frontend_in_az1",
                        "zones": [ "1" ],
                        "properties": {
                            "subnet": {
                                "Id": "[variables('subnetRef')]"
                            },
                            "privateIPAddress": "10.0.0.6",
                            "privateIPAllocationMethod": "Static"
                        }
                    },
                ],
```

### <a name="cross-zone-load-balancing"></a>Balanceamento de carga entre zona

Balanceamento de carga entre zona é a capacidade de Balanceador de carga para alcançar um ponto final de back-end em qualquer zona e é independente de front-end e os respetivos zonality.

Se pretende alinhar e garantir a sua implementação numa única zona, alinhe zonal front-end e back-end zonal recursos para a mesma zona. É necessária nenhuma ação adicional.

### <a name="backend"></a>Back-end

Balanceador de carga funciona com máquinas virtuais.  Qualquer VM numa VNet único pode fazer parte do conjunto back-end independentemente se pretende ou não foi garantida para uma zona ou que zona foi garantida.

Se pretende alinhar e garantir o front-end e back-end com uma única zona, apenas colocar a VMs dentro da mesma zona para o conjunto de back-end respetivo.

Se pretender endereço VMs em várias zonas, simplesmente ao colocar as VMs de várias zonas ao mesmo conjunto de back-end.  Quando define a utilização de dimensionamento da máquina virtual, é possível colocar um ou mais conjuntos de dimensionamento de máquina virtual ao mesmo conjunto de back-end.  E cada um destes conjuntos de dimensionamento de máquina virtual pode ser uma única ou várias zonas.

### <a name="outbound-connections"></a>Ligações de saída

[Ligações de saída](load-balancer-outbound-connections.md) são servidos pelo todas as zonas e são automaticamente com redundância de zona numa região com zonas de disponibilidade quando uma máquina virtual está associada a um balanceador de carga público e um front-end com redundância de zona.  As alocações de porta de realizar o SNAT de saída de ligação continuam a vigorar após falhas de zona.  

Por sua vez, se a VM se encontra associada a um balanceador de carga público e zonal front-end, ligações de saída garantidas para ser servidos pelo única zona.  Ligações de saída partilham fate com estado de funcionamento da zona correspondentes.

O preallocation de porta de realizar o SNAT e o algoritmo é o mesmo com ou sem zonas.

### <a name="health-probes"></a>Sondas do estado de funcionamento

As definições de pesquisa de estado de funcionamento existentes permanecem conforme forem sem zonas de disponibilidade.  Mas iremos tiver expandido o modelo de estado de funcionamento de um nível de infraestrutura. 

Quando utilizar com redundância de zona frontends, o Balanceador de carga expande o modelo de estado de funcionamento interno para independentemente sonda a acessibilidade de uma VM a partir de cada zona de disponibilidade e encerrar caminhos em horários que podem não ter conseguido sem a intervenção do cliente.  Se um caminho fornecido não está disponível da infraestrutura de Balanceador de carga de uma zona a uma VM noutra zona, o Balanceador de carga pode detetar e evitar esta falha. Outras zonas quem podem aceder esta VM podem continuar a servir de VM a partir do respetivo frontends respetivo.  Como resultado, é possível que durante eventos de falha, cada zona pode ter as distribuições de fluxo ligeiramente diferente ao proteger o estado de funcionamento global do seu serviço de ponto a ponto.

## <a name="design"></a> Considerações de design

Balanceador de carga é propositadamente flexível no contexto das zonas de disponibilidade. Pode optar por são alinhados com zonas ou pode optar por ser com redundância de zona.  Pode ter uma maior disponibilidade o preço de uma maior complexidade e tem de design para disponibilidade para um desempenho ideal.  Vamos Observe algumas considerações de design importantes.

### <a name="automatic-zone-redundancy"></a>Redundância de zona automática

Balanceador de carga faz com que simples com um único IP, como um front-end com redundância de zona. Um endereço IP com redundância de zona com segurança pode servir um recurso zonal qualquer zona e pode continuam a vigorar após uma ou mais falhas de zona, desde que uma zona permanece bom na região. Por outro lado, um front-end zonal é uma redução do serviço para um único fate de zona e partilhas com o respetivo horário.

Redundância de zona não implica hitless datapath ou plane controlo;  é expressamente plane de dados. Fluxos com redundância de zona podem utilizar zonas e fluxos de um cliente irão utilizar todas as zonas de bom estado de funcionamento numa região. Em caso de falha de zona, os fluxos de tráfego através de zonas de bom estado de funcionamento neste ponto no tempo não são afetados.  Fluxos de tráfego através de uma zona no momento da falha de zona podem ser afetados, mas podem recuperar as aplicações e estes fluxos podem continuar às zonas de bom estado de funcionamento restantes na região após a retransmissão ou reestablishment assim que o Azure tem convergido em torno da falha de zona.

### <a name="xzonedesign"></a> Entre limites de zona

É importante compreender que sempre que um serviço de ponto a ponto atravesse zonas, partilhar fate com não uma zona mas potencialmente várias zonas.  Como resultado, o serviço de ponto a ponto poderá não ter adquirido qualquer disponibilidade através de implementações não zonal.

Evite a introdução de indesejados dependências entre zona que serão anula ganhos de disponibilidade quando através de zonas de disponibilidade.  Quando a sua aplicação consiste de vários componentes e pretende que sejam resilientes a falhas de zona, que deve demorar são mais importantes para se certificar de sobrevivência dos componentes mais importantes suficientes na eventualidade de ocorrer uma falha de zona.  Por exemplo, um único componente de crítico para a sua aplicação pode afetar a aplicação toda se só existir numa zona que não seja o zone(s) operacional.  Além disso, considere também o restauro de zona e a forma como a aplicação irá convergir. Vamos rever alguns pontos chaves e utilizá-los a como inspiração para questões que acha que através do seu cenário específico.

- Se a aplicação tem dois componentes, como um endereço IP e uma VM com o disco gerido e está a ser garantidos na zona 1 e não será sobrevive a zona 2, quando a zona 1 falha o serviço de ponto a ponto quando zona 1 irá falhar.  Não cruzada zonas, exceto se compreender a que está a criar um modo de potencialmente hazardous falha.

- Se a aplicação tem dois componentes, como um endereço IP e uma VM com geridos disco e garantidos com redundância de zona e zona 1, respetivamente, o serviço de ponto a ponto será continuam a vigorar após falha de zona de zona 2, de zona 3, ou ambos, a menos que zona 1 falhou.  No entanto, perder algumas capacidade à razão sobre o estado de funcionamento do seu serviço se tudo está a observar a acessibilidade do front-end.  Pondere desenvolver um modelo de estado de funcionamento e a capacidade mais extenso.  Poderá utilizar os conceitos com redundância de zona e zonal em conjunto para expandir conhecimentos aprofundados e a capacidade de gestão.

- Se a aplicação tem dois componentes, como um front-end de Balanceador de carga com redundância de zona e um conjunto de dimensionamento de máquina virtual de zona cruzada em três zonas, os recursos em zonas não são afetados pela falha estarão disponíveis, mas o serviço de ponto a ponto poderá ser afetado em termos de capacidade durante a falha de zona. A partir de uma perspetiva de infraestrutura, a implementação pode sobrevive a uma ou mais falhas de zona e isto gera as perguntas seguintes:
  - Compreender como a sua aplicação motivos pelos quais sobre esses falhas e a capacidade de degradada?
  - Precisa de ter as proteções no seu serviço para forçar a ativação pós-falha para um par de região, se necessário?
  - Como irá monitorizar, detetar e mitigar cenário? Poderá utilizar o diagnóstico padrão Balanceador de carga para melhorar a monitorização de desempenho do serviço de ponto a ponto. Considere o que está disponível e o que poderá ter augmentation para uma visão geral.

- Zonas podem tornar a falhas mais facilmente compreendi e contidos.  No entanto, falha de zona é não diferem outras falhas quando se trata conceitos como tempos limite, as repetições e algoritmos de término. Apesar de Balanceador de carga do Azure fornece caminhos com redundância de zona e tenta recuperar rapidamente, um nível de pacotes em tempo real,. de retransmissões ou reestablishments poderão ocorrer durante o onset de uma falha e é importante compreender a forma como a sua aplicação copes com falhas. Será sobreviver seu esquema de balanceamento de carga, mas terá de planear para o seguinte:
  - Quando uma zona falha, o serviço de ponto a ponto compreende isto e se o estado for perdido, como, recuperará?
  - Quando uma zona devolve, a sua aplicação compreende como convergir em segurança?

### <a name="zonalityguidance"></a> Zona redundante versus zonal

Zona redundante pode fornecer um zona agnóstico e na mesma opção resiliente tempo com um único IP endereço para o serviço.  Pode reduzir complexidade por sua vez.  Zona redundante também tem mobilidade através de zonas e pode ser utilizado em segurança em recursos em qualquer zona.  Além disso, é uma prova futura em regiões sem zonas de disponibilidade, o que pode limitar as alterações necessárias depois de uma região obter zonas de disponibilidade.  A sintaxe de configuração para um endereço IP com redundância de zona ou front-end é bem sucedida em qualquer região, incluindo as pessoas sem zonas de disponibilidade.

Zonal pode fornecer uma garantia para uma zona explícita partilha fate com o estado de funcionamento da zona. Associar um IP zonal endereço ou zonal front-end de Balanceador de carga pode ser um atributo desejável ou razoável especialmente se o seu recurso ligado é uma VM zonal na mesma zona.  Ou, talvez, a aplicação requer o conhecimento explícito sobre que zona um recurso estiver localizado no e pretende pelo motivo sobre a disponibilidade em zonas separadas explicitamente.  Pode optar por expõe vários frontends zonal para um serviço de ponto a ponto distribuído zonas (ou seja, por zona frontends zonal para vários dimensionamento da máquina de virtual zonal define).  E se o seu frontends zonal são endereços IP públicos, pode utilizar estes frontends zonal vários para expor o serviço com [Gestor de tráfego](../traffic-manager/traffic-manager-overview.md).  Em alternativa, pode utilizar vários frontends zonal obter das informações de estado de funcionamento e desempenho zona através de monitorização de soluções de terceiros e expor o serviço com um front-end com redundância de zona global. Só deve serve recursos zonal com frontends zonal alinhada à mesma zona e evitar potencialmente prejudiciais zona em vários cenários para zonal recursos.  Recursos zonal apenas existem em regiões onde existem zonas de disponibilidade.

Não há nenhum orientações gerais que um é uma melhor opção que outros sem saberem a arquitetura de serviço.

## <a name="limitations"></a>Limitações

- Enquanto os dados plane é totalmente com redundância de zona (a menos que foi especificada a garantia zonal), controlo plane operações não são totalmente com redundância de zona.

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [zonas de disponibilidade](../availability-zones/az-overview.md)
- Saiba mais sobre [padrão Balanceador de carga](load-balancer-standard-overview.md)

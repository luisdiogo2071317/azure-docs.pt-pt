---
title: Descrição geral do Balanceador de carga Standard do Azure | Microsoft Docs
description: Descrição geral das funcionalidades de Balanceador de carga do Azure Standard
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2018
ms.author: kumud
ms.openlocfilehash: 20897137c617ddf9a33a8f4966bcd7e30ac7c60c
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261938"
---
# <a name="azure-load-balancer-standard-overview"></a>Descrição geral do padrão de Balanceador de carga do Azure

Balanceador de carga do Azure permite-lhe dimensionar as suas aplicações e criar a elevada disponibilidade para os serviços. Balanceador de carga e pode ser utilizado para cenários de entrada como saídos proporciona baixa latência, débito elevado e dimensiona até milhões de fluxos de todas as aplicações de TCP e UDP. 

Este artigo concentra-se no balanceador de carga padrão.  Para obter uma descrição mais geral para o Balanceador de carga do Azure, reveja [descrição geral do Balanceador de carga](load-balancer-overview.md) bem.

## <a name="what-is-standard-load-balancer"></a>O que é padrão Balanceador de carga?

Padrão de Balanceador de carga é um produto de Balanceador de carga novo para todos os TCP e UDP aplicações com uma funcionalidade expandida e mais granular definido através de Balanceador de carga básico.  Apesar de existirem diversas semelhanças, é importante para se familiarizar com as diferenças conforme descrito neste artigo.

Pode utilizar o padrão de Balanceador de carga como um balanceador de carga público ou interno. E pode ser ligada uma máquina virtual para um público e um recurso de Balanceador de carga interno.

Funções do recurso de Balanceador de carga são sempre expresso como um front-end, uma regra, uma pesquisa de estado de funcionamento e uma definição de conjunto de back-end.  Um recurso pode conter várias regras. Pode colocar as máquinas virtuais para o conjunto de back-end, especificando o conjunto de back-end do recurso de NIC da máquina virtual.  Este parâmetro é transmitido através do perfil de rede e expandido quando utilizar conjuntos de dimensionamento de máquina virtual.

Um aspeto chave é o âmbito da rede virtual para o recurso.  Enquanto básico Balanceador de carga existe no âmbito do conjunto de disponibilidade, um balanceador de carga Standard está totalmente integrado com o âmbito de uma rede virtual e aplicam a todos os conceitos de rede virtual.

Recursos de Balanceador de carga são objetos dentro do qual pode express como o Azure deve programa respetiva infraestrutura de multi-inquilino para alcançar o cenário de que pretende criar.  Não há nenhuma relação direta entre os recursos de Balanceador de carga e a infraestrutura real; criar um balanceador de carga não criar uma instância, capacidade está sempre disponível e existem não arranque ou dimensionamento atrasos a ter em consideração. 

>[!NOTE]
> O Azure oferece um conjunto de soluções para os seus cenários de balanceamento de carga completamente gerido.  Se estiver à procura para a terminação de TLS ("descarga de SSL") ou pelo processamento de camada de aplicação de pedidos HTTP/HTTPS, reveja [Gateway de aplicação](../application-gateway/application-gateway-introduction.md).  Se estiver à procura de global DNS balanceamento de carga, reveja [Gestor de tráfego](../traffic-manager/traffic-manager-overview.md).  Os cenários de ponto a ponto podem tirar partido do combinar estas soluções conforme necessário.

## <a name="why-use-standard-load-balancer"></a>Porquê utilizar o padrão de Balanceador de carga?

O Balanceador de Carga Standard permite-lhe dimensionar as suas aplicações e criar uma elevada disponibilidade para implementações de pequena escala em arquiteturas com várias zonas grandes e complexas.

Reveja a tabela abaixo para obter uma descrição geral das diferenças entre padrão Balanceador de carga e básico Balanceador de carga:

>[!NOTE]
> Novos designs de devem considerar a utilização do padrão de Balanceador de carga. 

| | SKU Standard | SKU Básico |
| --- | --- | --- |
| Tamanho do conjunto de back-end | até 1000 instâncias | até 100 instâncias |
| Pontos finais de conjunto de back-end | Qualquer máquina virtual numa única rede virtual, incluindo blend de máquinas virtuais, conjuntos de disponibilidade, conjuntos de dimensionamento de máquina virtual. | máquinas virtuais pertencentes uma escala de máquina virtual ou conjunto de disponibilidade único conjunto |
| Zonas de Disponibilidade | Com redundância de zona e zonal frontends de entrada e de mapeamentos de fluxos de saída, saída continuam a vigorar após falha de zona, cross-zona balanceamento de carga | / |
| Diagnóstico | Monitor do Azure, métricas multidimensionais, incluindo byte e contadores de pacotes, estado de funcionamento da sonda de estado, as tentativas de ligação (SIN de TCP), estado de funcionamento da ligação de saída (realizar o SNAT com êxito ou falhados fluxos), medidas plane de dados do Active Directory | Análise de registos do Azure, contagem de estado de funcionamento do conjunto de back-end de Balanceador de carga público só, alerta de esgotamento de realizar o SNAT |
| HA portas | Balanceador de carga interno | / |
| Seguro por predefinição | predefinição fechada para pontos finais públicos de Balanceador de carga e de IP e um grupo de segurança de rede deve ser utilizado para explicitamente lista branca para o tráfego flua | predefinição aberto, grupo de segurança opcional |
| Ligações de saída | Vários frontends com por regra ativamente. Um cenário de saída _tem_ explicitamente criado para a máquina virtual para conseguir utilizar a conectividade de saída.  [Pontos finais do serviço de VNet](../virtual-network/virtual-network-service-endpoints-overview.md) podem ser acedidos sem conectividade de saída e não contam para processamento de dados.  Todos os endereços IP públicos, incluindo serviços do Azure PaaS não está disponíveis como pontos finais do serviço de VNet, devem ser atingidos através de conectividade de saída e a contagem para processamento de dados. Quando uma máquina virtual está a funcionar apenas um balanceador de carga interno, ligações de saída através de predefinição realizar o SNAT não estão disponíveis. Programação de realizar o SNAT saída é o protocolo de transporte específicos com base no protocolo de entrada de balanceamento de carga regra. | Único front-end, selecionado aleatória, quando várias frontends estão presentes.  Quando apenas Balanceador de carga interno está a servir uma máquina virtual, é utilizada a predefinição realizar o SNAT. |
| Vários front-ends | Entrada e saída | Apenas de entrada |
| Operações de gestão | A maioria dos segundos de < 30 de operações | 60-90 segundos típica |
| SLA | 99,99% de caminho de dados com duas máquinas virtuais em bom estado | Implícita no SLA de VM | 
| Preços | -Lhe cobrados com base no número de regras, processados de dados de entrada ou saída associados a recursos  | Sem qualquer encargo |

Reveja [os limites de serviço para o Balanceador de carga](https://aka.ms/lblimits), bem como [preços](https://aka.ms/lbpricing), e [SLA](https://aka.ms/lbsla).


### <a name="backend"></a>Conjunto back-end

Conjuntos de back-end de Balanceador de carga padrão expande a qualquer recurso de máquina virtual numa rede virtual.  Pode conter até 1000 instâncias de back-end.  Uma instância de back-end é uma configuração de IP, o que é uma propriedade de um recurso NIC.

O conjunto de back-end pode conter máquinas virtuais autónomas, conjuntos de disponibilidade ou conjuntos de dimensionamento de máquina virtual.  Também pode misturar recursos no conjunto de back-end. Pode combinar até 150 recursos no conjunto back-end por recurso de Balanceador de carga.

Ao considerar como estruturar o conjunto de back-end, pode conceber para o menor número de recursos de conjunto de back-end individuais para otimizar ainda mais a duração das operações de gestão.  Não há qualquer diferença no desempenho de plane de dados ou a escala.

## <a name="az"></a>Zonas de disponibilidade

Balanceador de carga Standard suporta capacidades adicionais em regiões onde zonas de disponibilidade estão disponíveis.  Estas funcionalidades são incrementais para todos os Balanceador de carga padrão fornece.  Configurações de zonas de disponibilidade estão disponíveis para o Balanceador de carga de padrão público e interno.

Não zonal frontends tornar-se com redundância de zona por predefinição, quando implementado numa região com zonas de disponibilidade.   Um front-end com redundância de zona survives falha de zona e é fornecido pela infraestrutura dedicada em todas as zonas em simultâneo. 

Além disso, pode garantir o front-end para uma zona específica. Um front-end zonal partilha fate com o respetivo horário e é fornecido apenas por infraestrutura dedicada numa única zona.

Balanceamento de carga entre zona está disponível para o conjunto de back-end e qualquer recurso de máquina virtual numa vnet pode fazer parte de um conjunto de back-end.

Reveja [debate detalhado das zonas de disponibilidade relacionadas com capacidades](load-balancer-standard-availability-zones.md).

### <a name="diagnostics"></a> Diagnóstico

Balanceador de carga padrão fornece métricas multidimensionais por meio do Monitor do Azure.  Estas métricas podem ser filtradas, agrupadas e divididas para uma dimensão especificada.  Fornecem históricos e atuais informações sobre desempenho e estado de funcionamento do seu serviço.  Estado de funcionamento de recursos também é suportado.  Segue-se uma breve descrição geral de diagnóstico suportado:

| Métrica | Descrição |
| --- | --- |
| Disponibilidade de VIP | Padrão de Balanceador de carga continuamente exercises o caminho de dados de dentro de uma região para o Balanceador de carga front-end para a pilha SDN que suporte o VM. Bom estado de funcionamento instâncias permanecerem, desde que a medição segue o mesmo caminho que o tráfego com balanceamento de carga da sua aplicação. O caminho de dados que é utilizado pelos seus clientes também está a ser validado. A medida é invisível à sua aplicação e não interfere com outras operações.|
| Disponibilidade do DIP | Padrão de Balanceador de carga utiliza um serviço que monitoriza o estado de funcionamento do ponto final da aplicação, de acordo com as definições de configuração de pesquisa de estado de funcionamento distribuído. Esta métrica fornece um agregado ou por ponto final-vista filtrada de cada ponto final de instância individual no balanceador de carga do agrupamento.  Pode ver como o Balanceador de carga visualiza o estado de funcionamento da sua aplicação, como indicado pela sua configuração de pesquisa do Estado de funcionamento.
| Pacotes de SIN | Padrão de Balanceador de carga não terminar ligações TCP ou interagir com fluxos de pacotes TCP ou UDP. Fluxos e os respetivos handshakes são sempre entre a origem e a instância VM. Para resolver melhor os cenários de protocolo TCP, pode efetuar a utilização de SIN contadores de pacotes para compreender quantos ligação de TCP são realizadas tentativas. A métrica reporta o número de pacotes de TCP SIN que foram recebidos.|
| Ligações de realizar o SNAT | Padrão de Balanceador de carga reporta o número de fluxos de saída que são masqueraded para o endereço IP público front-end. Portas de realizar o SNAT são um recurso exhaustible. Esta métrica pode dar uma indicação de como descontos elevados a aplicação é depender de realizar o SNAT para saída fluxos teve origem.  Contadores para fluxos de realizar o SNAT de saída com êxito e são reportados e podem ser utilizados para resolver problemas e compreender o estado de funcionamento dos seus fluxos de saída.|
| Contadores de bytes | Padrão de Balanceador de carga relatórios os dados processados por front-end.|
| Contadores de pacote | Padrão de Balanceador de carga relatórios pacotes processados por front-end.|

Reveja [debate do diagnóstico de Balanceador de carga padrão de detalhado](load-balancer-standard-diagnostics.md).

### <a name="haports"></a>HA portas

Balanceador de carga Standard suporta um novo tipo de regra.  

Pode configurar regras para efetuar o dimensionamento de aplicações e altamente fiáveis de balanceamento de carga. Ao utilizar uma regra de Balanceador de carga padrão de balanceamento de carga HA portas irá fornecer por balanceamento em todas as portas efémeras do endereço IP de front-end de um interno padrão de carga do Balanceador de carga de fluxo.  A funcionalidade é útil para outros cenários onde é impractical ou indesejáveis para especificar portas individuais.

Uma regra de balanceamento de carga de portas HA permite-lhe criar ativa-passiva ou ativo-ativo n + 1 cenários para dispositivos de rede Virtual e de qualquer aplicação que necessita de grandes intervalos de portas de entrada.  Uma sonda do Estado de funcionamento pode ser utilizada para determinar quais back-ends devem receber novos fluxos.  Pode utilizar um grupo de segurança de rede para emular um cenário de intervalo de portas.

>[!IMPORTANT]
> Se estiver a planear utilizar uma aplicação Virtual de rede, consulte o fornecedor para obter orientações sobre se os seus produtos foi testado com portas HA e siga as orientações específicas para implementação. 

Reveja [detalhadas debate do HA portas](load-balancer-ha-ports-overview.md).

### <a name="securebydefault"></a>Seguro por predefinição

Balanceador de carga padrão é totalmente integrado à rede virtual.  A rede virtual está numa rede privada, fechada.  Porque padrão balanceadores de carga e os endereços IP públicos padrão foram concebidos para permitir esta rede virtual ser acedido a partir de fora da rede virtual, estes recursos agora a predefinição fechada, a menos que abre-los. Isto significa que os grupos de segurança de rede (NSGs) agora são utilizados para permitir explicitamente e lista branca de tráfego permitido.  Pode criar o seu centro de dados virtual completo e decidir através do NSG que e quando devem estar disponível.  Se não tiver um NSG numa sub-rede ou NIC de recurso de máquina virtual, o tráfego não é permitido para aceder a este recurso.

Para saber mais sobre NSGs e como aplicá-las para o seu cenário, consulte [grupos de segurança de rede](../virtual-network/security-overview.md).

### <a name="outbound"></a> Ligações de saída

Balanceador de carga suporta cenários de entrada e saídos.  Balanceador de carga padrão é significativamente diferente básico Balanceador de carga no que respeita à ligações de saída.

Origem de rede endereço tradução (realizar o SNAT) é utilizado para mapear internos, privados endereços IP na sua rede virtual para endereços IP públicos no frontends de Balanceador de carga.

Balanceador de carga padrão introduz um novo algoritmo para um [mais robusto, escalável e previsível algoritmo de realizar o SNAT](load-balancer-outbound-connections.md#snat) e capacidades de novo ativa, remove ambiguidade e configurações de explícita força em vez de lado efeitos. Estas alterações são necessárias para permitir novas funcionalidades surgir. 

Estas são as chaves tenets lembrar-se ao trabalhar com o padrão de Balanceador de carga:

- após a conclusão de uma regra de unidades o recurso de Balanceador de carga.  todos os programação do Azure deriva de configuração.
- Quando estiverem disponíveis vários frontends, todos os frontends são utilizados e cada front-end multiplica o número de portas de realizar o SNAT disponíveis
- Pode escolher e controlar se não desejar para um determinado front-end a utilizar para ligações de saída.
- cenários de saída são explícitos e conectividade de saída não existir até que foi especificado.
- regras de balanceamento de carga inferir como realizar o SNAT está programado. Regras de balanceamento de carga são específico do protocolo. Realizar o SNAT é específico do protocolo e a configuração deve refletir isto em vez de criar um efeito.

#### <a name="multiple-frontends"></a>Vários front-ends
Se pretender mais portas de realizar o SNAT porque está à espera ou são já estão a ocorrer um excesso para ligações de saída, também pode adicionar inventário de porta de realizar o SNAT incremental configurando frontends adicionais, regras e conjuntos de back-end para a mesma máquina virtual recursos.

#### <a name="control-which-frontend-is-used-for-outbound"></a>Controlo que front-end é utilizada para a saída
Se pretender restringir as ligações de saída para apenas provenientes de um endereço IP de front-end específico, opcionalmente, pode desativar a saída realizar o SNAT na regra que expresse precisa o mapeamento de saída.

#### <a name="control-outbound-connectivity"></a>Conectividade de saída de controlo
Padrão de Balanceador de carga existe no contexto da rede virtual.  Uma rede virtual é uma rede privada isolada.  A menos que existe uma associação com um endereço IP público, conectividade pública não é permitida.  Pode aceder [pontos finais do serviço de VNet](../virtual-network/virtual-network-service-endpoints-overview.md) porque estão dentro do e local à sua rede virtual.  Se pretender estabelecer conectividade de saída para um destino fora da rede virtual, tem duas opções:
- atribuir um endereço IP público do Standard SKU como um endereço IP público de nível de instância para o recurso de máquina virtual ou
- Coloque o recurso de máquina virtual no conjunto back-end de Balanceador de carga público padrão.

Ambos permitirá a conectividade de saída da rede virtual para fora da rede virtual. 

Se lhe _apenas_ tem um balanceador de carga de padrão interno, associados ao conjunto de back-end em que o recurso de máquina virtual se encontra, a máquina virtual só pode aceder a recursos de rede virtual e [serviço VNet Pontos finais](../virtual-network/virtual-network-service-endpoints-overview.md).  Pode seguir os passos descritos no parágrafo anterior para criar a conectividade de saída.

Conectividade de saída de um recurso de máquina virtual não associado a permanece SKUs padrão como antes.

Reveja [detalhadas debate de ligações de saída](load-balancer-outbound-connections.md).

### <a name="multife"></a>Vários frontends
Balanceador de carga suporta várias regras com vários frontends.  Balanceador de carga padrão expande este para cenários de saída.  Cenários de saída são essencialmente o inverso de uma regra de balanceamento de carga na entrada.  O entrada também a regra de balanceamento de carga cria um associar para ligações de saída. Standard Load Balancer utiliza todos os frontends associados a um recurso de máquina virtual através de uma regra de balanceamento de carga.  Além disso, um parâmetro no balanceamento de carga da regra e permite-lhe suprimir uma balanceamento de carga regra para fins de conectividade de saída, que permite a seleção de frontends específico, incluindo none.

Para comparação, básico Balanceador de carga seleciona um front-end único aleatória, e não há nenhuma capacidade para controlar o que foi selecionado.

Reveja [detalhadas debate de ligações de saída](load-balancer-outbound-connections.md).

### <a name="operations"></a> Operações de gestão

Recursos padrão do Balanceador de carga existem numa plataforma completamente nova infraestrutura.  Isto permite operações de gestão significativamente mais rápidas para SKUs padrão e os tempos de conclusão são, normalmente, menos de 30 segundos por recurso de Standard SKU.  Tenha em atenção que como conjuntos back-end aumentam de tamanho, a duração necessária para o back-end conjunto alterado também aumento.

Pode modificar o Balanceador de carga padrão de recursos e mover um endereço IP público padrão de uma máquina virtual para outro muito mais rápido.

## <a name="migration-between-skus"></a>Migração entre os SKUs

SKUs não são mutável. Siga os passos nesta secção para mover de um recurso SKU para outro.

>[!IMPORTANT]
>Reveja este documento na íntegra para compreender as diferenças entre os SKUs e ter examinadas cuidadosamente o seu cenário.  Terá de fazer alterações adicionais para alinhar com o seu cenário.

### <a name="migrate-from-basic-to-standard-sku"></a>Migrar de básica para o Standard SKU

1. Crie um novo padrão recurso (Balanceador de carga e IPs públicos, conforme necessário). Recriar as regras e definições de pesquisa.

2. Criar um novo ou atualizar o NSG existente no NIC ou uma sub-rede para o tráfego com balanceamento de carga de lista branca, pesquisa, bem como qualquer outro tráfego de que pretende permitir.

3. Remova os recursos de SKU básico (Balanceador de carga e IPs públicos, conforme aplicável) todas as instâncias VM. Lembre-se de que também remover todas as instâncias VM de um conjunto de disponibilidade.

4. Anexe todas as instâncias VM para os novos recursos de Standard SKU.

### <a name="migrate-from-standard-to-basic-sku"></a>Migrar de padrão para o SKU básico

1. Crie um novo recurso básico (Balanceador de carga e IPs públicos, conforme necessário). Recriar as regras e definições de pesquisa. 

2. Remova os recursos de Standard SKU (Balanceador de carga e IPs públicos, conforme aplicável) todas as instâncias VM. Lembre-se de que também remover todas as instâncias VM de um conjunto de disponibilidade.

3. Anexe todas as instâncias VM para os novos recursos de SKU básico.

>[!IMPORTANT]
>
>Existem limitações à utilização do básico e padrão SKUs.
>
>HA portas e ao diagnóstico do Standard SKU só está disponível no Standard SKU. Não é possível migrar a partir do SKU Standard para o SKU básico e também retêm estas funcionalidades.
>
>Básico e padrão SKU têm um número de diferenças, conforme descrito neste artigo.  Certifique-se compreender e preparar para os mesmos.
>
>SKUs de correspondência tem de ser utilizado para recursos de Balanceador de carga e um IP público. Não é possível ter uma mistura de recursos de SKU básico e recursos de Standard SKU. Não é possível anexar máquinas virtuais autónomas, máquinas virtuais num recurso de conjunto de disponibilidade ou uma máquina virtual dos recursos do conjunto de dimensionamento para ambos os SKUs em simultâneo.

## <a name="region-availability"></a>Disponibilidade de região

Padrão de Balanceador de carga está atualmente disponível em todas as regiões de nuvem pública.

## <a name="sla"></a>SLA

Estão disponíveis com um SLA de 99,99% padrão balanceadores de carga.  Reveja o [padrão SLA de Balanceador de carga](https://aka.ms/lbsla) para obter mais detalhes.

## <a name="pricing"></a>Preços

Padrão de Balanceador de carga é um produto cobrado com base no número de regras configuradas e todos os dados de entrada e saídos processados de balanceamento de carga. Para obter informações sobre preços padrão Balanceador de carga, visite o [preços do Balanceador de carga](https://aka.ms/lbpricing) página.

## <a name="limitations"></a>Limitações

- SKUs não são mutável. Não pode alterar o SKU de um recurso existente.
- Conjunto de disponibilidade de um recurso de máquina virtual autónoma, recurso ou recurso de conjunto de dimensionamento de máquina virtual pode referenciar um SKU nunca ambos.
- Uma regra de Balanceador de carga não pode abranger duas redes virtuais.  Frontends e as respetivas instâncias de back-end relacionados tem de estar localizadas na mesma rede virtual.  
- Frontends de Balanceador de carga não estão acessíveis através de peering de rede virtual global.
- [Move operações de subscrição](../azure-resource-manager/resource-group-move-resources.md) não são suportadas para recursos padrão LB de SKU e PIP.
- As funções de trabalho Web sem uma VNet e outros serviços da plataforma Microsoft podem ser acessíveis quando é utilizado apenas um padrão Balanceador de carga interno devido a um efeito de como os serviços de pre-VNet e outra plataforma dos serviços de função. Não precisará da tal como o respetivo serviço próprio ou subjacentes plataforma pode alterações sem aviso prévio. Têm de assumir sempre tem de criar [conectividade de saída](load-balancer-outbound-connections.md) explicitamente se assim o desejar quando utiliza um padrão Balanceador de carga interno apenas.
- Balanceador de carga é um produto TCP ou UDP para balanceamento de carga e reencaminhamento de porta para estes protocolos IP específicos.  Regras de balanceamento de carga e regras NAT de entrada são suportadas para TCP e UDP e não suportadas para outros protocolos IP, incluindo ICMP. Balanceador de carga não terminar, responder ou, caso contrário, interagir com a carga de um fluxo UDP ou TCP. Não se trata de um proxy. Validação com êxito de conectividade para um front-end tem de colocar em banda do local com o mesmo protocolo utilizado uma balanceamento ou entrada regra NAT de carga (TCP ou UDP) _e_ pelo menos uma das suas máquinas virtuais tem de gerar uma resposta para um cliente Para ver uma resposta de um front-end.  Não receber uma resposta em banda do front-end de Balanceador de carga indica que nenhuma máquina virtual foram podem responder.  Não é possível interagir com um balanceador de carga front-end sem uma máquina virtual podem responder.  Isto também se aplica a ligações de saída onde [representação porta realizar o SNAT](load-balancer-outbound-connections.md#snat) é apenas suportado para TCP e UDP; quaisquer outros protocolos IP, incluindo ICMP também irão falhar.  Atribua um endereço de IP público de nível de instância de correção para atenuar.
- Ao contrário dos balanceadores de carga público, que fornecem [ligações de saída](load-balancer-outbound-connections.md) quando transição de endereços IP privados dentro da rede virtual para endereços IP públicos, balanceadores de carga interno não traduzir saída teve origem as ligações para o front-end de um balanceador de carga interno como ambos estão no espaço de endereços IP privados.  Isto evita a possibilidade de esgotamento de realizar o SNAT dentro único espaço de endereços IP interno onde a tradução não é necessária.  O efeito é que, se um fluxo de saída de uma VM no conjunto de back-end tenta um fluxo de front-end de Balanceador de carga interno no qual agrupamento onde reside, está a _e_ está mapeada para si próprio, ambos os legs do fluxo não correspondem e o fluxo irá falhar .  Se o fluxo não mapeado para a mesma VM no conjunto de back-end que tiver criado o fluxo para o front-end, o fluxo será bem sucedida.   Quando o fluxo mapeia para si próprio o fluxo de saída parece provenientes da VM para o front-end e o fluxo de entrada correspondente parece provenientes da VM para si próprio. Ponto de vista o SO convidado das partes de entrada e saídas do mesmo fluxo não correspondem dentro da máquina virtual. A pilha TCP não irá reconhece estes meios do mesmo fluxo como parte do mesmo fluxo como origem e de destino não correspondem.  Quando o fluxo está mapeado para qualquer outra VM no conjunto de back-end, corresponderá meios do fluxo e a VM pode responder com êxito para o fluxo.  Sintoma para este cenário é tempos limite de ligação intermitente. Existem várias soluções comuns para alcançar fiável neste cenário (originadas fluxos a partir de um conjunto de back-end para o back-end conjuntos de Balanceador de carga interno respetivos front-end) que incluem a inserção de um proxy de terceiros por trás de carga interno Balanceador ou [utilizando regras de estilo DSR](load-balancer-multivip-overview.md).  Enquanto pode utilizar um balanceador de carga público para mitigar, o cenário resultante é suscetível a [esgotamento de realizar o SNAT](load-balancer-outbound-connections.md#snat) e deve ser evitada, a menos que cuidadosamente geridos.

## <a name="next-steps"></a>Passos Seguintes

- Saiba como utilizar [padrão Balanceador de carga e zonas de disponibilidade](load-balancer-standard-availability-zones.md)
- Saiba mais sobre [disponibilidade zonas](../availability-zones/az-overview.md).
- Saiba mais sobre [diagnósticos de Balanceador de carga padrão](load-balancer-standard-diagnostics.md).
- Saiba mais sobre [suportado métricas multidimensionais](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftnetworkloadbalancers) para obter um diagnóstico no [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md).
- Saiba como utilizar [Balanceador de carga para ligações de saída](load-balancer-outbound-connections.md)
- Saiba mais sobre [padrão Balanceador de carga com regras de balanceamento de carga de portas HA](load-balancer-ha-ports-overview.md)
- Saiba como utilizar [Balanceador de carga com várias Frontends](load-balancer-multivip-overview.md)
- Saiba mais sobre [redes virtuais](../virtual-network/virtual-networks-overview.md).
- Saiba mais sobre [grupos de segurança de rede](../virtual-network/security-overview.md).
- Saiba mais sobre [pontos finais do serviço de VNet](../virtual-network/virtual-network-service-endpoints-overview.md)
- Saiba mais sobre algumas da outra chave [capacidades de rede](../networking/networking-overview.md) no Azure.
- Saiba mais sobre [Balanceador de carga](load-balancer-overview.md).

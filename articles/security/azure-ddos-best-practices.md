---
title: Os procedimentos de proteção do Azure DDoS & arquitetura de referência | Microsoft Docs
description: Saiba mais sobre como pode utilizar dados de registo para obter conhecimentos aprofundados sobre a sua aplicação.
services: security
author: barclayn
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/20/2018
ms.author: barclayn
ms.openlocfilehash: 4b2d785f5b9095a2decfc65ec46808ff6f65c38e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="azure-ddos-protection-best-practices-and-reference-architecture"></a>Proteção DDoS do Azure: Melhores práticas e arquitetura de referência

Este documento é direcionado para os decisores de IT e técnicos de segurança. Familiaridade com o Azure, rede e segurança esperado.

Conceber o denial of service (DDoS) distribuído resiliência requer planeamento e estruturação de vários modos de falha. Este documento fornece as melhores práticas para criar aplicações no Azure para resiliência contra ataques de DDoS.

## <a name="types-of-attacks"></a>Tipos de ataques

DDoS é um tipo de ataque utilizado em esforço a esgotar os recursos da aplicação. O objetivo é a afetar a disponibilidade e a capacidade de processar pedidos legítimos da aplicação. Ataques são cada vez mais sofisticadas e superior em tamanho e no impacto. Ataques de DDoS podem ser segmentados em qualquer ponto final que está publicamente acessível através da Internet.

O Azure oferece proteção contínua contra ataques de DDoS. Esta proteção é integrada para a plataforma do Azure, por predefinição e sem custos adicionais. Para além das principais proteção DDoS fornecida na plataforma, além disso, temos uma nova oferta com o nome "[Azure DDoS proteção padrão](https://azure.microsoft.com/services/ddos-protection/)", que fornece capacidades avançadas de mitigação de DDoS contra ataques à rede e é automaticamente otimizado para proteger os seus recursos do Azure específicos. Proteção é simple ativar durante a criação de novas redes virtuais. Também pode ser feito após a criação inicial e não necessita de aplicação ou recurso alterações.

![](media/azure-ddos-best-practices/image1.png)

Ataques de DDoS podem ser amplamente classificados em três (3) diferentes categorias

### <a name="volumetric-attacks"></a>Ataques volumetric

Ataques volumetric são os tipos de ataques de DDoS mais comuns. Ataques volumetric são assaults de força bruta que as camadas de transporte de rede e de destino. Tentam a esgotar os recursos essas ligações de rede. Estes ataques utilizam frequentemente vários sistemas infetados para inundar as camadas de rede com uma quantidade substancial de tráfego seemingly legítima. Protocolos de camada de rede diferentes, tais como controlo mensagem ICMP (Internet Protocol), protocolo UDP (User Datagram) e protocolo de controlo de transmissão (TCP) são utilizados neste tipo de ataque.

Utilizadas mais frequentemente a camada de rede DDoS ataques são SIN TCP sobrecarregar, eco ICMP, excessivos de UDP, DNS e NTP ataque amplificação. Este tipo de ataque pode ser utilizado não só para interromper o serviço, mas também como um smokescreen de intrusão de rede mais possui conteúdo nefasto e destino. Um exemplo de um ataque volumetric recente é o [Memcached exploração](https://www.wired.com/story/github-ddos-memcached/) que o impacto no GitHub. Este ataque visados porta UDP 11211 e gerados 1.35 Tb/s do volume de ataque.

### <a name="protocol-attacks"></a>Ataques de protocolo

Protocolo de ataques de protocolos de aplicação de destino. Tente utilizar todos os recursos disponíveis nos dispositivos de infraestrutura como firewalls, servidores de aplicações e Balanceadores de carga. Ataques de protocolo utilizam pacotes que são com formato incorreto ou contenham anomalias de protocolo. Estes ataques operam enviando o maior número de pedidos abertos, servidores e outros resposta de dispositivos de comunicação e a aguardar uma resposta de pacote. O destino tenta responder aos pedidos de abrir, eventualmente, fazendo com que o sistema de destino para falhas.

O exemplo mais comuns de um ataque de DDoS baseado no protocolo é grande quantidade de SIN TCP. Este ataque, um sucessivamente de pedidos de TCP SIN é direcionado para um destino e pode ser utilizada para sobrecarregá-lo. O objetivo consiste em tornar o destino não responde. A interrupção de Dyn de 2016, para além dos que está a ser um ataque de camada de aplicação também consistiu em inundações TCP SIN direcionada para a porta 53 dos servidores DNS do Dyn.

### <a name="resource-attacks"></a>Ataques de recursos

Camada da aplicação de destino de ataques de recursos. Se acionam processos de back-end em esforço de sobrecarregar o sistema de destino. Recurso de ataques de tráfego de abuso parecida normal, mas que consulta o intensivas em CPU ativada para o servidor. O volume de tráfego necessário a esgotar os recursos é uma menor que outro tipo de ataques. O tráfego de um ataque de recurso é indistinguishable de tráfego legítimo, tornando difíceis de detetar. Os ataques de recursos mais comuns são nos serviços DNS e HTTP/HTTPS.

## <a name="shared-responsibility-in-the-cloud"></a>Responsabilidade partilhada na nuvem

Uma estratégia de aprofundada defesa é necessário para combat os tipos e sofisticação de ataques de aumento. Segurança é uma responsabilidade partilhada entre o cliente e a Microsoft. Microsoft refere-se a isto como um [modelo responsabilidade partilhado](https://azure.microsoft.com/blog/microsoft-incident-response-and-shared-responsibility-for-cloud-computing/). A figura abaixo mostra esta divisão de responsabilidade.

![](media/azure-ddos-best-practices/image2.png)

Clientes do Azure beneficiam rever a nossa melhores práticas e criar globalmente aplicações concebidas e testadas para falha distribuídas.

## <a name="fundamental-best-practices"></a>Fundamentais melhores práticas

Ataques de DDoS são e o aumento súbito. A seguinte secção fornece orientação prescritiva para criar serviços resilientes DDoS no Azure.

### <a name="design-for-security"></a>Conceber para segurança

Os clientes devem certificar-se de segurança é uma prioridade em todo o ciclo de vida inteiro de uma aplicação, de design e implementação para a implementação e operações. As aplicações podem ter erros que permitem que um volume de pedidos crafted para utilizar uma quantidade de recursos, causando uma interrupção do serviço inordinate relativamente baixo. Para proteger um serviço em execução no Microsoft Azure, os clientes devem ter uma boa compreensão da respetiva arquitetura de aplicação e deve focar-se no [5 pillars de qualidade de software](https://docs.microsoft.com/azure/architecture/guide/pillars).
Os clientes devem conhecer volumes de tráfego típica, o modelo de conectividade entre a aplicação e outras aplicações e os pontos finais de serviço expostos à Internet pública.

Além disso, garantir que uma aplicação é resiliente para processar um denial of service direcionada para a aplicação em si é essencial da importância. Segurança e privacidade estão integradas diretamente no plataforma do Azure, começando com o [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx). O SDL endereços segurança em cada fase de desenvolvimento e assegura que o Azure é continuamente atualizado para torná-lo ainda mais seguros.

### <a name="design-for-scalability"></a>Estrutura de escalabilidade

A escalabilidade é a capacidade de um sistema de processar um aumento de carga. Os clientes têm de design as aplicações para [aumentar horizontalmente](https://docs.microsoft.com/azure/architecture/guide/design-principles/scale-out) para satisfazer o pedido de carga amplified, especificamente num evento de ataque DDoS. Se a aplicação depende de uma única instância de um serviço, cria um ponto único de falha. Aprovisionamento de várias instâncias melhora a resiliência e a escalabilidade.

Para [App Service do Azure](../app-service/app-service-value-prop-what-is.md), selecione um [plano do App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) que oferece várias instâncias. Para os serviços de nuvem do Azure, configure cada uma das suas funções utilizar [várias instâncias](../cloud-services/cloud-services-choose-me.md). Para [máquinas de virtuais (VMs) do Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-about/?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), certifique-se de que a arquitetura VM inclui mais de uma VM e que cada VM está incluído num [conjunto de disponibilidade](../virtual-machines/virtual-machines-windows-manage-availability.md). Recomendamos que utilize [conjuntos de dimensionamento de Máquina Virtual](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) para capacidades de dimensionamento automático.

### <a name="defense-in-depth"></a>Defesa em profundidade

É a ideia atrás de defesa em profundidade para gerir o risco com estratégias defesas diversos. Layering defesas de segurança de uma aplicação reduz a possibilidade de um ataque com êxito. Recomendamos que os clientes implementam estruturas seguras para as aplicações através das capacidades incorporadas da plataforma do Azure.

Por exemplo, o risco de ataques aumenta o tamanho ou a área de superfície da aplicação. Redução de área de superfície recomenda-se através de listas brancas para fechar pendente IP exposto espaço & as portas de escuta que não são necessários nos balanceadores de carga de endereços ([ALB](../load-balancer/load-balancer-get-started-internet-portal.md)/[Gateway de aplicação](../application-gateway/application-gateway-create-probe-portal.md)) ou através de [de rede (NSG) de grupos de segurança.](../virtual-network/virtual-networks-nsg.md)
Pode utilizar [etiquetas de serviço](/virtual-network/security-overview.md) e [grupos de segurança de aplicações](/virtual-network/security-overview.md) para minimizar a complexidade para a criação de regra de segurança e configurar a segurança de rede como uma extensão natural da estrutura de uma aplicação.

Os clientes devem implementar serviços do Azure num [rede virtual](../virtual-network/virtual-networks-overview.md) sempre que possível. Isto permite que os recursos do serviço comunicar através de endereços IP privados. Tráfego do serviço do Azure de uma rede virtual utiliza endereços IP públicos como endereços IP de origem, por predefinição. Utilizar [pontos finais de serviço](../virtual-network/virtual-network-service-endpoints-overview.md) passará tráfego do serviço para utilizar endereços de rede virtual privada como os endereços IP de origem ao aceder o serviço do Azure a partir de uma rede virtual.

Muitas vezes, vemos cliente recursos no local ao obter atacados juntamente com os respetivos recursos no Azure. Se estiver a ligar um ambiente no local para o Azure, recomendamos que estará a minimizar a exposição de recursos no local para a Internet pública. Pode utilizar a escala e as capacidades avançadas de proteção DDoS do Azure ao implementar as entidades públicas conhecidas no Azure. Uma vez que estas entidades acessíveis publicamente são, muitas vezes, um destino de ataques de DDoS, colocar-las no Azure reduz o impacto nos seus recursos no local.

## <a name="azure-ddos-protection"></a>Proteção DDoS do Azure

O Azure tem dois DDoS as ofertas de serviços que fornecem proteção a partir da rede ataques (Layer 3 e 4) - DDoS básica e proteção DDoS padrão. 

### <a name="azure-ddos-basic-protection"></a>Proteção básica do Azure DDoS

Proteção básica está integrada do Azure por predefinição, sem custos adicionais. A escala completa e a capacidade da rede implementada global do Azure fornece defesa contra comuns camada ataques à rede através de Always On nos mitigação de monitorização e em tempo real de tráfego. Proteção DDoS Basic requer sem alterações de configuração ou a aplicação do utilizador. Todos os serviços do Azure, incluindo PaaS serviços como o DNS do Azure estão protegidos por DDoS de proteção básico.

![](media/azure-ddos-best-practices/image3.png)

Proteção de DDoS básica do Azure é composta por componentes de hardware e software. Um plane de controlo de software decide quando, where, e o tipo de tráfego deve ser steered através de dispositivos de hardware que analisem e remova o tráfego de ataque. O plane controlo faz com que esta decisão com base em proteção de DDoS um toda a infraestrutura *política*, que está estaticamente definido e aplicada universalmente para todos os clientes do Azure.

Por exemplo, a política de proteção DDoS Especifica em que volume de tráfego deve ser a proteção *acionada* (ou seja, o tráfego de inquilino deve ser encaminhado através de limpeza aparelhos) e, posteriormente, como a limpeza devem aparelhos *mitigar* ataque.

Serviço de proteção de DDoS básico do Azure é direcionado para proteção de infraestrutura e a proteção da plataforma do Azure. -Atenua tráfego quando tem mais de uma velocidade que é por isso, significativa que podem afetar o vários clientes num ambiente multi-inquilino. Não fornece a alertas ou por cliente personalizada políticas.

### <a name="azure-ddos-standard-protection"></a>Proteção de padrão DDoS do Azure

Proteção padrão fornece funcionalidades de mitigação DDoS avançadas e automaticamente está otimizada para proteger os seus recursos do Azure específicos numa rede Virtual. A proteção é simple ativar a qualquer rede Virtual novo ou existente e não necessita de nenhuma aplicação ou alterações de recurso. Tem várias vantagens relativamente ao serviço básico, incluindo registo, alertas e telemetria. Descritos abaixo são as chaves differentiators do serviço Azure DDoS proteção padrão.

#### <a name="adaptive-realtime-tuning"></a>Otimização de adaptável em tempo real

Serviço de proteção de DDoS básico do Azure ajuda a proteger os clientes mas apenas os protege para impedir a afetar outros clientes. Por exemplo, se um serviço é aprovisionado para um volume normal de tráfego de entrada legítimo que é menor do que o *taxa de Acionador* da política de proteção DDoS de toda a infraestrutura, pode aceder um ataque DDoS recursos desse cliente despercebidas. Em geral, mais a natureza complexa de ataques recentes (por exemplo, vetores multi DDoS distribuídos), bem como os comportamentos de específicas da aplicação de inquilinos chamar por cliente, as políticas de proteção personalizada.
Isto é conseguido utilizando dois insights:

1. Camada de aprendizagem automática de (por IP) por cliente padrões de tráfego de 3/4, e
2. Dado que a escala do Azure poder absorver quantidade significativa de tráfego, minimizando falsos positivos.

![](media/azure-ddos-best-practices/image5.png)

#### <a name="ddos-protection-telemetry-monitoring--alerting"></a>Telemetria de proteção DDoS, monitorização e alertas

Com DDoS proteção padrão, expomos telemetria avançada através de [Azure Monitor](/monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) durante a duração de um ataque DDoS. Alertas podem ser configuradas para qualquer uma das métricas de Monitor de Azure expostas pela proteção DDoS. O registo pode ser integrado Splunk (Event Hubs do Azure), Log Analytics do Azure e Storage do Azure para análises avançadas através da interface de diagnósticos de Monitor do Azure.

##### <a name="ddos-mitigation-policies"></a>Políticas de mitigação DDoS

No portal do Azure, clique em **Monitor** \> **métricas**. No **métricas** ecrã, selecione o grupo de recursos, o tipo de recurso do endereço IP público e o seu endereço IP público do Azure. Métricas de DDoS vai estar visíveis no painel de métricas disponíveis.

Aplica-se DDoS proteção padrão **três políticas de mitigação otimizados automaticamente (TCP SIN, TCP e UDP)** para cada um IP público do recurso protegido, na VNET que tenha DDoS ativada. Pode ver os limiares de política, selecionando a métrica **'Pacotes para acionar DDoS mitigação de entrada'**.

![](media/azure-ddos-best-practices/image7.png)

Os limiares de política são configurado automaticamente através do nosso do machine learning a criação de perfis de tráfego de rede com base. Mitigação DDoS ocorre para um endereço IP sob ataque apenas quando for excedido o limiar de política.

##### <a name="under-ddos-attack"></a>Sob ataque DDoS

Se o endereço IP público sob ataque, o valor 'em DDoS ataques ou não' Métrica activa/desactiva a 1 como podemos efetuar mitigação sobre o tráfego de ataque.

![](media/azure-ddos-best-practices/image8.png)

É recomendável configurar um alerta nesta métrica, para ser notificado quando existe uma mitigação de DDoS Active Directory efetuada no seu endereço IP público.

Para obter mais informações, consulte [gerir Azure DDoS proteção padrão no portal do Azure](../virtual-network/ddos-protection-manage-portal.md).

#### <a name="resource-attacks"></a>Ataques de recursos

Ataques de recursos na camada de aplicação específicos, os clientes devem configurar a Firewall de aplicação Web (WAF) para ajudar a aplicações web seguro. WAF inspeciona o tráfego de entrada web para bloquear o SQL Server injections, processamento de scripts entre sites, DDoS e outros ataques de camada 7. O Azure oferece [WAF como uma funcionalidade de Gateway de aplicação](../application-gateway/application-gateway-web-application-firewall-overview.md) para proteção centralizada das suas aplicações web de vulnerabilidades e explorações comuns. Existem varieties das ofertas WAF de parceiros do Azure que podem ser mais adequados às suas necessidades através de [do Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?search=WAF&page=1).

Firewalls de aplicação Web, mesmo que são suscetíveis a ataques de esgotamento de volumetric & Estado. Recomendamos vivamente a ativação da proteção DDoS padrão na rede Virtual para proteger contra volumetric WAF & ataques de protocolo. Para obter mais informações, consulte a secção de arquitetura de referência.

### <a name="protection-planning"></a>Planear a proteção

Planeamento e preparação são fundamentais para compreender a forma como um sistema executará durante um ataque DDoS. Este planeamento e preparação também ajudará a conceber um plano de resposta de gestão de incidentes.

Os clientes devem certificar-se que DDoS proteção padrão está ativado na rede virtual da internet com pontos finais. Configurar alertas de DDoS ajuda a manter um constante olho watchful em quaisquer potenciais ataques na sua infraestrutura. Os clientes devem monitorizar independentemente das suas aplicações. Têm de compreender o comportamento normal da aplicação. Passos tem de ser efetuados se a aplicação não está a comportar conforme esperado durante um ataque DDoS.

#### <a name="ddos-attacks-orchestration"></a>Orquestração de ataques de DDoS

É uma boa prática para testar a sua pressupostos sobre a forma como os serviços de respondem a um ataque, mesmo antes de ocorre ao realizar simulações periódicas. Durante os testes, confirme que os serviços ou aplicações continuam a funcionar conforme esperado e que não existe nenhum interrupção à experiência de utilizador final. Identificar lacunas de uma tecnologia de & o processo de ponto de vista e incorporar na estratégia de resposta DDoS. Uma recomendação geral consiste em efetuar esses testes em ambientes de testes ou durante as horas de pico para minimizar o impacto para ambiente de produção.

Podemos ter parcerias com [BreakingPoint nuvem](https://www.ixiacom.com/products/breakingpoint-cloud) para criar uma interface onde os clientes do Azure podem gerar tráfego contra proteção DDoS ativada pontos finais públicos para simulações. [Nuvem de ponto de interrupção](https://www.ixiacom.com/products/breakingpoint-cloud) simulação permite-lhe:

- Validar como proteção do Microsoft Azure DDoS protege os recursos do Azure contra ataques de DDoS

- Otimizar o seu processo de resposta a incidentes enquanto sob ataque DDoS

- Compatibilidade de DDoS do documento

- Preparar as equipas de segurança de rede

Atuais é uma luta relentless exigir inovação constante na defesa. Proteção DDoS padrão do Azure é um Estado de avançados oferta de estado de avançados oferta para clientes com uma solução eficaz mitigar ataques de DDoS cada vez mais complexas.

## <a name="components-of-a-ddos-response-strategy"></a>Componentes de uma estratégia de resposta DDoS

Na maioria dos cenários quando um ataque DDoS é direcionado para os recursos do Azure, não há intervenção mínimo necessária de um ponto de vista do utilizador final. Ainda assim, incorporando DDoS mitigação como parte da estratégia de resposta a incidentes da organização irá garantir um impacto mínimo para a continuidade do negócio.

### <a name="microsoft-threat-intelligence"></a>Ameaças da Microsoft

Microsoft tem uma rede de intelligence de um vasto conjunto de ameaças que utiliza o conhecimento coletivo uma Comunidade de segurança expandida que suporte serviços online da Microsoft, parceiros da Microsoft e relações dentro da Comunidade de segurança da Internet. Como fornecedor de infraestrutura críticos, Microsoft recebe avisos antecipados sobre ameaças, demora ameaças adquiridas a partir de outros Microsoft online services e cliente global da Microsoft base. Todas as ameaças da Microsoft está incorporada nos produtos de proteção do Azure DDoS.

Além disto, unidade de Crimes digitais (DCU da Microsoft) efetua estratégias ofensivas contra botnets, uma origem comuns de comando e controlo de ataques de DDoS.

### <a name="perform-a-risk-evaluation-of-your-azure-resources"></a>Efetuar uma avaliação de risco dos seus recursos do Azure

é imperativo para compreender o âmbito da sua conta de risco de um ataque DDoS numa base contínua. Os clientes devem periodicamente solicitar próprios: que novos recursos do Azure publicamente disponíveis precisam de proteção? Existe um ponto único de falhas no serviço? Como podem serviços ser isolados para limitar o impacto de um ataque ao ainda disponibilizar serviços aos clientes válidos? Existem redes virtuais onde DDoS proteção padrão deve ser ativada, mas não é? São os meus serviços ativo/ativo com ativação pós-falha em várias regiões?

### <a name="customer-ddos-response-team"></a>Equipa de resposta do cliente DDoS

Criar uma equipa de resposta DDoS é um passo chave garantir rápida e eficaz resposta a um ataque. Os clientes tem de identificar vários contactos na sua organização que irá supervisionam de planeamento e em execução. A equipa de resposta DDoS deve ter uma compreensão profunda sobre do serviço Azure DDoS proteção padrão e devem ser apt no identificar e mitigar um ataque para em coordenação com vários clientes internos e externos, incluindo a equipa de suporte da Microsoft conforme necessário.

A Microsoft recomenda incorporando a equipa de resposta de DDoS planeamento e a simulação exercícios, incluindo a escala de teste, como parte da sua disponibilidade do serviço e o planeamento de continuidade normal. 

### <a name="during-an-attack"></a>Durante um ataque

Padrão de proteção de DDoS do Azure irá identificar e mitigar ataques de DDoS sem qualquer intervenção do utilizador. Para ser notificado quando existe uma mitigação de Active Directory para um IP público protegido, pode [configurar um alerta](../virtual-network/ddos-protection-manage-portal.md) na métrica "em DDoS ataques ou não". Pode continuar a revisão e criar alertas como pretendido para outras métricas DDoS compreender a escala do ataque, o tráfego a ser ignorados, etc.

#### <a name="when-to-contact-microsoft-support"></a>Quando contactar o suporte da Microsoft

- Se durante um ataque DDoS achar que o desempenho do recurso protegido é significativamente degradado ou o recurso não está disponível.

- Se pensa que o serviço de proteção DDoS não está a comportar conforme esperado. Serviço de proteção DDoS só irá iniciar a mitigação se o abaixo critérios forem satisfeitos:

    - O valor métrico ' política ao iniciar a mitigação DDoS (SIN de TCP/TCP/UDP) é menor o tráfego recebido no recurso de IP público de proteção.

- Se souber, vai ter um evento planeado viral que direciona para aumento significativo tráfego de rede.

- Se um ator tem threatened lançar um ataque DDoS relativamente aos seus recursos.

Para o negócio crítico afetar problemas, crie uma gravidade [suporta permissão](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

### <a name="post-attack-steps"></a>Passos de ataque de POST

É sempre um boa estratégia para fazer uma postmortem depois de um ataque e readjust a estratégia de resposta DDoS conforme necessário. Aspetos a considerar:

- Foi não existe qualquer interrupção do serviço ou o utilizador final experiência devido a falta de arquitetura escalável?

- As aplicações ou serviços sofreu mais?

- Como efetiva foi a estratégia de resposta de DDoS e como foi-ser melhorada ainda mais?

Se suspeitar que está sob um ataque DDoS remeta através do seu normais canais de suporte do Azure.

## <a name="ddos-protection-reference-architectures"></a>Arquiteturas de referência de proteção DDoS

Padrão de proteção DDoS foi concebido [para serviços que são implementados numa rede Virtual.](../virtual-network/virtual-network-for-azure-services.md) Para outros serviços, a predefinição proteção básico DDoS será aplicada. As arquiteturas de referências específicas indicadas abaixo são dispostas por cenários, com os padrões da arquitetura agrupados.

### <a name="virtual-machine-windowslinux-workloads"></a>Cargas de trabalho de máquina virtual (Windows/Linux)

#### <a name="application-running-on-load-balanced-vms"></a>Aplicação em execução de carga com balanceamento de VMs

Esta arquitetura de referência mostra um conjunto de práticas comprovadas para executar várias máquinas virtuais (VMs) do Windows num conjunto de dimensionamento atrás de um balanceador de carga, para melhorar a disponibilidade e escalabilidade. Esta arquitetura pode ser utilizada para qualquer carga de trabalho sem monitorização de estado, como um servidor web.

![](media/azure-ddos-best-practices/image9.png)

Nesta arquitetura, uma carga de trabalho é distribuída por várias instâncias de VM. Existe um único endereço IP público e o tráfego de Internet é distribuído para as VMs com um balanceador de carga. Padrão de proteção DDoS está ativado na rede Virtual do Azure (internet) do Balanceador de carga que tem o IP público com associados.

O Balanceador de carga distribui pedidos recebidos de Internet para as instâncias de VM. Conjuntos de dimensionamento VM permitem o número de VMs para ser ampliada entrada ou saída manual ou automaticamente com base nas regras predefinidas. Isto é importante se o recurso estiver sob ataque DDoS. Consulte este [artigo](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm), para obter mais informações sobre esta arquitetura de referência.

#### <a name="applications-running-on-windows-n-tier"></a>Aplicações em execução no Windows N camadas

Existem várias formas de implementar uma arquitetura de N camadas. O diagrama mostra uma aplicação Web de 3 camadas típica. Esta arquitetura baseia-se [executar as VMs com balanceamento de carga para escalabilidade e disponibilidade](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm). As camadas Web e Business utilizam VMs com balanceamento de carga.

![](media/azure-ddos-best-practices/image10.png)

A arquitetura acima, DDoS proteção padrão está ativada na rede Virtual. Todos os IPs públicos na rede Virtual irá obter Layer3/Layer4 DDoS proteção. Para a proteção de 7 camadas, Gateway de aplicação no WAF SKU devem ser implementado. Consulte [isto](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/n-tier) artigo, para obter mais informações sobre esta arquitetura de referência.

#### <a name="paas-web-application"></a>Aplicação web de PaaS

Esta arquitetura de referência mostra a executar uma aplicação do App Service do Azure numa única região. Esta arquitetura mostra definida das práticas comprovadas para uma aplicação web que utiliza [App Service do Azure](https://azure.microsoft.com/documentation/services/app-service/) e [SQL Database do Azure](https://azure.microsoft.com/documentation/services/sql-database/).
Modo de espera região é configurado para cenários de ativação pós-falha.

![](media/azure-ddos-best-practices/image11.png)

Gestor de tráfego encaminha os pedidos recebidos para Gateway de aplicação de uma das regiões. Durante as operações normais, encaminha os pedidos para o Gateway de aplicação na região de Active Directory. Se nessa região ficar indisponível, Gestor de tráfego falhar o Gateway de aplicação na região de modo de espera.

Todo o tráfego destinado à aplicação web de Internet é encaminhado para o [endereço IP público do Gateway de aplicação](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview) através do Gestor de tráfego. Neste cenário, o serviço de aplicações (aplicações web) propriamente dito não é diretamente externamente enfrentam e é protegido ao Gateway de aplicação. Recomenda-se para configurar o Application Gateway WAF SKU (modo impedir) para proteger contra ataques de 7 camadas (HTTP/HTTPS/Web Socket). Além disso, as aplicações Web estão configuradas para [aceitar o tráfego apenas a partir do Gateway de aplicação](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/) endereço IP.

Consulte [isto](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region) artigo, para obter mais informações sobre esta arquitetura de referência.

### <a name="mitigation-for-non-web-paas-services"></a>Mitigação para serviços de PaaS não web

#### <a name="hdinsight-on-azure"></a>HDInsight no Azure

Esta arquitetura de referência mostra configurar DDoS proteção Std. para [cluster do HDInsight](https://docs.microsoft.com/azure/hdinsight/) no Azure. Tem de certificar-se de que o cluster do HDInsight está ligado a uma rede virtual e proteção DDoS está ativada na rede Virtual.

![](media/azure-ddos-best-practices/image12.png)

![](media/azure-ddos-best-practices/image13.png)

Nesta arquitetura, o tráfego destinado ao cluster do HDInsight a partir da internet é encaminhado para o IP público associado com o Balanceador de carga de gateway do HDInsight. O Balanceador de carga do gateway, em seguida, envia o tráfego para os nós principais ou nós de trabalho diretamente. Dada DDoS proteção padrão está ativada na rede Virtual do HDInsight, todos os IPs públicos na rede Virtual irá obter Layer3/Layer4 DDoS proteção. Arquitetura de referência acima pode ser combinada com N-camada/várias-Region referência arquiteturas.

Para obter mais detalhes sobre esta arquitetura de referência, consulte [expandir utilizando uma Azure Virtual Network do Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network?toc=%2fazure%2fvirtual-network%2ftoc.json) documentação.

### <a name="azure-api-management"></a>Gestão de API do Azure

Esta arquitetura de referência protege o ponto final público do [gestão de API](../api-management/api-management-key-concepts.md) publicar APIs para clientes externos para a organização de recursos. APIM deve ser implementado numa rede Virtual externa para ativar a proteção DDoS.

![](media/azure-ddos-best-practices/image15.png)

Ao configurar a rede Virtual externa, portal de gestão de API do gateway & Programador estejam acessíveis a partir da internet pública através de um balanceador de carga público. Nesta arquitetura, DDoS proteção padrão está ativado na rede Virtual externa APIM de rede Virtual. O tráfego é encaminhado através da internet para o endereço IP público de APIM, que se encontra protegido contra ataques à rede Layer3/Layer4. Para proteger contra ataques de camada 7 HTTP/HTTPs, pode configurar um Gateway de aplicação no modo de WAF.

Lista de serviços adicionais que são implementadas numa rede Virtual e pode ser configurada para DDoS proteção padrão é mantida [aqui](../virtual-network/virtual-network-for-azure-services.md). Padrão de proteção DDoS só suporta a recursos do Azure Resource Manager. *Implementação de ambiente de serviço de aplicações (ASE) injetado numa VNET com um IP público não é suportada nativamente.* Para obter detalhes sobre a proteção de ambiente de ASE, consulte nesta secção.

## <a name="next-steps"></a>Passos Seguintes

* [Página de produto de proteção DDoS do Azure](https://azure.microsoft.com/services/ddos-protection/)

* [Blogue de proteção DDoS do Azure](http://aka.ms/ddosblog)

* [Documentação de proteção DDoS do Azure ](../virtual-network/ddos-protection-overview.md)

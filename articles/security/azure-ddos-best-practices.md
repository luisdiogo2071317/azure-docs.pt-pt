---
title: Proteção DDoS do Azure melhores práticas e de referência arquiteturas | Microsoft Docs
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
ms.date: 06/06/2018
ms.author: barclayn
ms.openlocfilehash: b802c7b96bd8d0cfa56347d45542495caf69d7e4
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34824715"
---
# <a name="azure-ddos-protection-best-practices-and-reference-architectures"></a>Proteção DDoS do Azure: Melhores práticas e arquiteturas de referência

Este artigo destina-se a decisores IT e técnicos de segurança. Se espera que está familiarizado com o Azure, rede e segurança.

Conceber o denial of service (DDoS) distribuído resiliência requer planeamento e estruturação de vários modos de falha. Este artigo fornece as melhores práticas para criar aplicações no Azure para resiliência contra ataques de DDoS.

## <a name="types-of-attacks"></a>Tipos de ataques

DDoS é um tipo de ataque que tenta a esgotar os recursos da aplicação. O objetivo é a afetar a disponibilidade da aplicação e a respetiva capacidade de processar pedidos legítimos. Ataques são cada vez mais sofisticadas e superior em tamanho e no impacto. Ataques de DDoS podem ser segmentados em qualquer ponto final que está publicamente acessível através da internet.

O Azure oferece proteção contínua contra ataques de DDoS. Esta proteção, está integrada-se a plataforma do Azure por predefinição e, em não custo extra. 

Para além das principais proteção DDoS na plataforma, [Azure DDoS proteção padrão](https://azure.microsoft.com/services/ddos-protection/) fornece capacidades avançadas de mitigação de DDoS contra ataques à rede. -La automaticamente está otimizado para proteger os seus recursos do Azure específicos. Proteção é simple ativar durante a criação de novas redes virtuais. Também pode ser feito após a criação e não necessita de aplicação ou recurso alterações.

![A função de proteção do Azure DDoS proteger clientes e uma rede virtual a partir de um atacante](media/azure-ddos-best-practices/image1.png)

Ataques de DDoS podem ser classificados em três categorias: volumetric, o protocolo e recursos.

### <a name="volumetric-attacks"></a>Ataques volumetric

Ataques volumetric têm o tipo mais comuns do ataque DDoS. Ataques volumetric são assaults de força bruta que as camadas de transporte de rede e de destino. Repita a esgotar os recursos, como ligações de rede. 

Estes ataques utilizam frequentemente vários sistemas infetados para inundar as camadas de rede com tráfego seemingly legítima. Utilizam os protocolos de camada de rede, como o controlo de mensagem de ICMP (Internet Protocol), protocolo UDP (User Datagram) e protocolo de controlo de transmissão (TCP).

Ataques de DDoS frequentemente utilizadas de camada de rede são SIN TCP excessivos de eco ICMP, UDP excessivos de DNS, e amplificação NTP ataques. Este tipo de ataque pode ser utilizado não só para interromper o serviço, mas também como um smokescreen de intrusão de rede mais possui conteúdo nefasto e destino. Um exemplo de um ataque volumetric recente é o [Memcached exploração](https://www.wired.com/story/github-ddos-memcached/) que afetados GitHub. Este ataque visados porta UDP 11211 e gerados 1.35 Tb/s do volume de ataque.

### <a name="protocol-attacks"></a>Ataques de protocolo

Protocolo de ataques de protocolos de aplicação de destino. Tentam utilizar cópias de segurança todos os recursos disponíveis dispositivos de infraestrutura como firewalls, servidores de aplicações, e Balanceadores de carga. Ataques de protocolo utilizam pacotes que são com formato incorreto ou contenham anomalias de protocolo. Estes ataques operam enviando grande número de pedidos abertos que servidores e outros dispositivos de comunicação responder e a aguardar uma resposta de pacote. O destino tenta responder aos pedidos de abrir, provocando, eventualmente, o sistema para falhas.

O exemplo mais comuns de um ataque de DDoS baseado no protocolo é grande quantidade de SIN de TCP. Este ataque, um sucessivamente de pedidos de TCP SIN tenta sobrecarregar um destino. O objetivo consiste em tornar o destino não responde. A interrupção de Dyn de 2016, para além dos que está a ser um ataque de camada de aplicação, consistiu em TCP SIN floods essa porta destino 53 dos servidores DNS do Dyn.

### <a name="resource-attacks"></a>Ataques de recursos

Camada da aplicação de destino de ataques de recursos. Se acionam processos de back-end em esforço de sobrecarregar um sistema. Recurso de ataques de tráfego de abuso parecida normal, mas que consulta o intensivas em CPU ativada para o servidor. O volume de tráfego necessário a esgotar os recursos é inferior de outro tipo de ataques. O tráfego de um ataque de recurso é indistinguishable de tráfego legítimo, tornando difícil de detetar. Os ataques de recursos mais comuns são nos serviços DNS e HTTP/HTTPS.

## <a name="shared-responsibility-in-the-cloud"></a>Responsabilidade partilhada na nuvem

Uma estratégia de defesa em profundidade ajuda combat a variedade de aumento e sofisticação de ataques. Segurança é uma responsabilidade partilhada entre o cliente e a Microsoft. Microsoft chama a isto uma [modelo responsabilidade partilhado](https://azure.microsoft.com/blog/microsoft-incident-response-and-shared-responsibility-for-cloud-computing/). A figura seguinte mostra esta divisão de responsabilidade:

![Responsabilidades do cliente e do Azure](media/azure-ddos-best-practices/image2.png)

Clientes do Azure beneficiam rever as melhores práticas da Microsoft e globalmente criar aplicações que são concebidas e testadas para falha distribuídas.

## <a name="fundamental-best-practices"></a>Fundamentais melhores práticas

As secções seguintes fornecem uma orientação prescritiva para criar serviços DDoS resiliente no Azure.

### <a name="design-for-security"></a>Conceber para segurança

Certifique-se de que a segurança é uma prioridade em todo o ciclo de vida inteiro de uma aplicação, de design e implementação para a implementação e operações. As aplicações podem ter erros que permitem que um volume de pedidos para utilizar uma quantidade de recursos, causando uma interrupção do serviço inordinate relativamente baixo. 

Para ajudar a proteger um serviço em execução no Microsoft Azure, deve ter uma boa compreensão da sua arquitetura de aplicação e concentrar-se no [cinco pillars de qualidade de software](https://docs.microsoft.com/azure/architecture/guide/pillars).
Que deve conhecer volumes de tráfego típica, o modelo de conectividade entre a aplicação e outras aplicações e os pontos finais de serviço que estão expostos à internet pública.

Garantir que uma aplicação é resiliente para processar um denial of service que é direcionada para a aplicação em si é mais importante. Segurança e privacidade que são criadas para a plataforma do Azure, a partir de [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx). O SDL endereços segurança em cada fase de desenvolvimento e assegura que o Azure é continuamente atualizado para torná-lo ainda mais seguros.

### <a name="design-for-scalability"></a>Estrutura de escalabilidade

A escalabilidade é quão bem um sistema consegue processar o aumento de carga. Deve conceber as suas aplicações para [aumentar horizontalmente](https://docs.microsoft.com/azure/architecture/guide/design-principles/scale-out) para satisfazer a procura de uma carga amplified, especificamente no caso de um ataque DDoS. Se a aplicação depende de uma única instância de um serviço, cria um ponto único de falha. Aprovisionamento de várias instâncias faz com que o seu sistema mais resiliente e dimensionável mais.

Para [App Service do Azure](../app-service/app-service-value-prop-what-is.md), selecione um [plano do App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) que oferece várias instâncias. Para os serviços de nuvem do Azure, configure cada uma das suas funções utilizar [várias instâncias](../cloud-services/cloud-services-choose-me.md). Para [Virtual Machines do Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-about/?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), certifique-se de que a arquitetura de máquina virtual (VM) inclui mais de uma VM e que cada VM está incluído num [conjunto de disponibilidade](../virtual-machines/virtual-machines-windows-manage-availability.md). Recomendamos que utilize [conjuntos de dimensionamento de máquina virtual](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) para capacidades de dimensionamento automático.

### <a name="defense-in-depth"></a>Defesa em profundidade

É a ideia atrás de defesa em profundidade para gerir os riscos através da utilização de diversas estratégias defesas. Layering defesas de segurança de uma aplicação reduz a possibilidade de um ataque com êxito. Recomendamos que implementar estruturas seguras para as suas aplicações, utilizando as capacidades incorporadas da plataforma do Azure.

Por exemplo, o risco de ataques aumenta com o tamanho (*área de superfície*) da aplicação. Pode reduzir a área de superfície utilizando a listas brancas fechar-se para baixo o espaço de endereços IP exposto e está a escutar as portas que não são necessários nos balanceadores de carga ([Azure Load Balancer](../load-balancer/load-balancer-get-started-internet-portal.md) e [Gateway de aplicação do Azure](../application-gateway/application-gateway-create-probe-portal.md)). [Grupos de segurança (NSGs) de rede](../virtual-network/security-overview.md) são outra forma para reduzir a superfície de ataque.
Pode utilizar [etiquetas de serviço](/virtual-network/security-overview.md#service-tags) e [grupos de segurança de aplicações](/virtual-network/security-overview.md#application-security-groups) para minimizar a complexidade para criar regras de segurança e configuração de segurança de rede, como uma extensão natural da estrutura de uma aplicação.

Deve implementar serviços do Azure num [rede virtual](../virtual-network/virtual-networks-overview.md) sempre que possível. Esta prática permite que os recursos do serviço comunicar através de endereços IP privados. Tráfego do serviço do Azure de uma rede virtual utiliza endereços IP públicos como endereços IP de origem, por predefinição. Utilizar [pontos finais de serviço](../virtual-network/virtual-network-service-endpoints-overview.md) passará tráfego do serviço para utilizar endereços de rede virtual privada como os endereços IP de origem quando estiver a aceder no serviço do Azure a partir de uma rede virtual.

Muitas vezes, vemos dos clientes recursos no local ao obter atacados juntamente com os respetivos recursos no Azure. Se estiver a estabelecer a ligação de um ambiente no local para o Azure, recomendamos que estará a minimizar a exposição de recursos no local para a internet pública. Pode utilizar a escala e as capacidades avançadas de proteção DDoS do Azure ao implementar as entidades públicas conhecidas no Azure. Uma vez que estas entidades acessíveis publicamente são, muitas vezes, um destino de ataques de DDoS, colocar-las no Azure reduz o impacto nos seus recursos no local.

## <a name="azure-offerings-for-ddos-protection"></a>Ofertas do Azure para proteção DDoS

O Azure tem dois DDoS as ofertas de serviços que fornecem proteção contra ataques à rede (Layer 3 e 4): DDoS proteção básico e padrão de proteção DDoS. 

### <a name="ddos-protection-basic"></a>DDoS proteção básico

Proteção básica está integrada do Azure por predefinição, sem custos adicionais. A escala e a capacidade da rede implementada global do Azure fornece defesa contra ataques de camada de rede comuns através de mitigação sempre no tráfego de monitorização e em tempo real. DDoS proteção básico requer sem alterações de configuração ou a aplicação do utilizador. DDoS proteção básica ajuda a proteger todos os serviços do Azure, incluindo PaaS serviços como o DNS do Azure.

![Mapear representação da rede do Azure, com o texto "Global DDoS mitigação presença" e "Esquerda a capacidade de mitigação DDoS"](media/azure-ddos-best-practices/image3.png)

Proteção DDoS básica no Azure é constituído por componentes de hardware e software. Um plane de controlo de software decide quando, where, e o tipo de tráfego deve ser steered através de dispositivos de hardware que analisem e remova o tráfego de ataque. O plane controlo faz com que esta decisão com base em proteção de DDoS um toda a infraestrutura *política*. Esta política é definir estaticamente e aplicada universalmente para todos os clientes do Azure.

Por exemplo, a política de proteção DDoS Especifica em que volume de tráfego deve ser a proteção *acionado.* (Ou seja, o tráfego de inquilino deve ser encaminhado através de aplicações de limpeza.) A política especifica, em seguida, como os dispositivos de limpeza devem *mitigar* ataque.

O serviço de proteção básico do Azure DDoS visa da infraestrutura de proteção e da plataforma do Azure. -Atenua tráfego quando tem mais de uma velocidade que é significativa, por isso, de que isto poderá afetar vários clientes num ambiente multi-inquilino. Não fornece a alertas ou por cliente personalizada políticas.

### <a name="ddos-protection-standard"></a>Padrão de proteção DDoS

Proteção padrão fornece funcionalidades de mitigação de DDoS avançadas. -La automaticamente está otimizado para ajudar a proteger os recursos do Azure específicos numa rede virtual. Proteção é simple ativar a qualquer rede virtual novo ou existente e requer sem alterações de aplicação ou recurso. Tem várias vantagens relativamente ao serviço básico, incluindo registo, alertas e telemetria. As secções seguintes descrevem as funcionalidades principais do serviço Azure DDoS proteção padrão.

#### <a name="adaptive-real-time-tuning"></a>Adaptável de otimização em tempo real

O serviço de proteção básico do Azure DDoS ajuda a proteger os clientes e evitar impactos para outros clientes. Por exemplo, se um serviço é aprovisionado para um volume normal de tráfego de entrada legítimo que é menor do que o *taxa de Acionador* da política de proteção DDoS toda a infraestrutura, poderá ir um ataque DDoS recursos desse cliente despercebidas. Mais geralmente, a complexidade de ataques recentes (por exemplo, vetores multi DDoS distribuídos) e comportamentos específicas da aplicação de inquilinos chamar por cliente, as políticas de proteção personalizada. O serviço concretiza esta personalização utilizando dois insights:

- Configuração automática de padrões de tráfego per cliente (por IP) de camada 3 e 4.

- Minimizar falsos positivos, considerar que a escala do Azure poder absorver uma quantidade significativa de tráfego.

![Diagrama de como DDoS proteção padrão funciona, "A criação de política" dentro de um círculo](media/azure-ddos-best-practices/image5.png)

#### <a name="ddos-protection-telemetry-monitoring-and-alerting"></a>Telemetria de proteção DDoS, monitorização e alertas

Padrão de proteção DDoS expõe telemetria avançada através de [Azure Monitor](/monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) durante a duração de um ataque DDoS. Pode configurar alertas para qualquer uma das métricas de Monitor do Azure que utiliza proteção DDoS. Pode integrar o registo com Splunk (Event Hubs do Azure), Log Analytics do Azure e Storage do Azure para análises avançadas através da interface de diagnósticos de Monitor do Azure.

##### <a name="ddos-mitigation-policies"></a>Políticas de mitigação DDoS

No portal do Azure, selecione **Monitor** > **métricas**. No **métricas** painel, selecione o grupo de recursos, selecione um tipo de recurso dos **endereço IP público**e selecione o seu endereço IP público do Azure. Métricas de DDoS são visíveis no **as métricas disponíveis** painel.

Padrão de proteção DDoS aplica-se três políticas de mitigação otimizados automaticamente (TCP SIN, TCP e UDP) para cada um IP público do recurso protegido, na rede virtual que tenha DDoS ativada. Pode ver os limiares de política, selecionando a métrica **pacotes para acionar DDoS mitigação de entrada**.

![Gráfico de métricas e as métricas disponíveis](media/azure-ddos-best-practices/image7.png)

Os limiares de política são configurado automaticamente através de machine learning com base na rede tráfego a criação de perfis. Mitigação DDoS ocorre para um endereço IP sob ataque apenas quando for excedido o limiar de política.

##### <a name="metric-for-an-ip-address-under-ddos-attack"></a>Métrica para um endereço IP sob ataque DDoS

Se o endereço IP público em ataques, o valor para a métrica **ataques de DDoS em ou não** é alterado para 1 como proteção DDoS efetua mitigação sobre o tráfego de ataque.

![Métrica "Em DDoS ataques ou não" e o gráfico](media/azure-ddos-best-practices/image8.png)

É recomendável configurar um alerta nesta métrica. Em seguida, será notificado quando existe uma mitigação DDoS Active Directory efetuada no seu endereço IP público.

Para obter mais informações, consulte [gerir Azure DDoS proteção padrão no portal do Azure](../virtual-network/ddos-protection-manage-portal.md).

#### <a name="web-application-firewall-for-resource-attacks"></a>Firewall de aplicações Web para ataques de recursos

Ataques de recursos na camada de aplicação específicos, deve configurar uma firewall de aplicação web (WAF) para ajudar a aplicações web seguro. Uma WAF inspeciona o tráfego de entrada web para bloquear SQL injections, scripts, DDoS e outros ataques de 7 camadas entre sites. O Azure oferece [WAF como uma funcionalidade de Gateway de aplicação](../application-gateway/application-gateway-web-application-firewall-overview.md) para proteção centralizada das suas aplicações web de vulnerabilidades e explorações comuns. Existem outras ofertas WAF de parceiros do Azure que podem ser mais adequados às suas necessidades através de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?search=WAF&page=1).

Mesmo firewalls de aplicação de web são suscetíveis a ataques de esgotamento de volumetric e estado. Recomendamos vivamente que ativar DDoS proteção padrão na rede virtual para ajudar a proteger contra volumetric WAF e ataques de protocolo. Para obter mais informações, consulte o [arquiteturas de referência de proteção DDoS](#ddos-protection-reference-architectures) secção.

### <a name="protection-planning"></a>Planear a proteção

Planeamento e preparação são fundamentais para compreender a forma como um sistema executará durante um ataque DDoS. Conceber um plano de resposta de gestão de incidentes é parte deste esforço.

Se tiver DDoS proteção padrão, certifique-se de que está ativada na rede virtual de pontos finais de acesso à internet. Configurar alertas de DDoS ajuda-o a constantemente ver quaisquer potenciais ataques na sua infraestrutura. 

Deve monitorizar as aplicações de forma independente. Compreenda o comportamento normal de uma aplicação. Prepare para funcionar se a aplicação não está a comportar conforme esperado durante um ataque DDoS.

#### <a name="testing-through-simulations"></a>Através de simulações de teste

É uma boa prática para testar a sua pressupostos sobre como os serviços irão responder a um ataque ao realizar simulações periódicas. Durante os testes, confirme que os serviços ou aplicações continuam a funcionar conforme esperado e que não existe nenhum interrupção a experiência de utilizador. Identificar lacunas de uma tecnologia e o processo de ponto de vista e incorpore-los na estratégia de resposta DDoS. Recomendamos que efetue esses testes em ambientes de teste ou durante as horas de pico para minimizar o impacto para o ambiente de produção.

Podemos ter parcerias com [BreakingPoint nuvem](https://www.ixiacom.com/products/breakingpoint-cloud) para criar uma interface onde os clientes do Azure podem gerar tráfego contra ativada para proteção DDoS pontos finais públicos para simulações. Pode utilizar o [BreakingPoint nuvem](https://www.ixiacom.com/products/breakingpoint-cloud) simulação para:

- Valide como o Azure DDoS Protection ajuda a proteger os seus recursos do Azure contra ataques de DDoS.

- Otimize o processo de resposta a incidentes enquanto sob ataque DDoS.

- Documente DDoS compatibilidade.

- Preparar as equipas de segurança de rede.

Atuais requer inovação constante na defesa. Proteção DDoS Standard do Azure é um Estado de-última oferta com uma solução eficaz para mitigar ataques de DDoS cada vez mais complexas.

## <a name="components-of-a-ddos-response-strategy"></a>Componentes de uma estratégia de resposta DDoS

Um ataque DDoS que tenha como destino de recursos do Azure, normalmente, requer intervenções mínimas por parte de um ponto de vista do utilizador. Ainda assim, incorporando DDoS mitigação como parte de uma estratégia de resposta a incidentes ajuda a minimizar o impacto para a continuidade do negócio.

### <a name="microsoft-threat-intelligence"></a>Ameaças da Microsoft

A Microsoft tem uma rede de intelligence de um vasto conjunto de ameaça. Esta rede utiliza o conhecimento coletivo uma Comunidade de segurança expandida que suporte serviços online da Microsoft, parceiros da Microsoft e relações dentro da Comunidade de segurança da internet. 

Como fornecedor de infraestrutura críticos, Microsoft recebe avisos antecipados sobre ameaças. Microsoft reúne informações sobre ameaças de que os respetivos serviços online e de base ao cliente global. Microsoft incorpora todas este ameaças novamente os produtos de proteção do Azure DDoS.

Além disso, a unidade Microsoft Crimes digitais (DCU) efetua estratégias ofensivas contra botnets. Botnets são uma origem de comando e controlo de ataques de DDoS comuns.

### <a name="risk-evaluation-of-your-azure-resources"></a>Avaliação de risco dos seus recursos do Azure

é imperativo para compreender o âmbito da sua conta de risco de um ataque DDoS numa base contínua. Periodicamente solicitar manualmente: 
- Os novos recursos do Azure publicamente disponíveis precisam de proteção?

- Existe um ponto único de falha no serviço? 

- Como podem serviços ser isolados para limitar o impacto de um ataque ao ainda disponibilizar serviços aos clientes válidos?

- Existem redes virtuais onde DDoS proteção padrão deve ser ativada, mas não tem? 

- São os meus serviços ativo/ativo com ativação pós-falha em várias regiões?

### <a name="customer-ddos-response-team"></a>Equipa de resposta do cliente DDoS

A criação de uma equipa de resposta de DDoS é um passo chave responder a um ataque rapidamente e eficaz. Identifica contactos na sua organização que irá supervisionam de planeamento e em execução. Esta equipa de resposta DDoS exaustivamente deve compreender o serviço Azure DDoS proteção padrão. Certifique-se de que a equipa pode identificar e mitigar um ataque através da coordenação com clientes internos e externos, incluindo a equipa de suporte da Microsoft.

Para a sua equipa de resposta de DDoS, recomendamos que utilize exercícios simulação como parte da sua disponibilidade do serviço e o planeamento de continuidade normal. Estes exercícios devem incluir testes de escala.

### <a name="alerts-during-an-attack"></a>Alertas durante um ataque

Padrão de proteção de DDoS Azure identifica e mitiga ataques de DDoS sem qualquer intervenção do utilizador. Para ser notificado quando existe uma mitigação de Active Directory para um IP público protegido, pode [configurar um alerta](../virtual-network/ddos-protection-manage-portal.md) na métrica **ataques de DDoS em ou não**. Pode optar por criar alertas para as outras métricas de DDoS compreender a escala de ataque, o tráfego a ser ignorado e outros detalhes.

#### <a name="when-to-contact-microsoft-support"></a>Quando contactar o suporte da Microsoft

- Durante um ataque DDoS, determinar se o desempenho do recurso protegido é gravemente degradado ou o recurso não está disponível.

- Pensa que o serviço de proteção DDoS não está a comportar conforme esperado. 

  O serviço de proteção DDoS inicia mitigação apenas se o valor métrico **política ao iniciar a mitigação DDoS (SIN de TCP/TCP/UDP)** é menor do que o tráfego recebido no recurso de IP público protegido.

- Está a planear um evento viral que irá aumentar significativamente o tráfego de rede.

- Um ator tem threatened lançar um ataque DDoS relativamente aos seus recursos.

Para uma gravidade de criação de ataques que têm um impacto de negócio crítico, [suporta permissão](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

### <a name="post-attack-steps"></a>Passos de pós-ataques de

É sempre um boa estratégia para fazer uma postmortem depois de um ataque e ajustar a estratégia de resposta DDoS conforme necessário. Aspetos a considerar:

- Foi não existe qualquer interrupção do serviço ou utilizador ocorrer devido a falta de arquitetura escalável?

- As aplicações ou serviços sofreu mais?

- Como efeita era a estratégia de resposta DDoS e como pode-ser melhorada?

Se suspeitar que está sob um ataque DDoS, escalar através do seu normais canais de suporte do Azure.

## <a name="ddos-protection-reference-architectures"></a>Arquiteturas de referência de proteção DDoS

Padrão de proteção DDoS foi concebido [para serviços que são implementados numa rede virtual](../virtual-network/virtual-network-for-azure-services.md). Para outros serviços, aplica-se o serviço de proteção de DDoS básico predefinido. Arquiteturas de referência seguinte são dispostas por cenários, com os padrões da arquitetura agrupados.

### <a name="virtual-machine-windowslinux-workloads"></a>Cargas de trabalho de máquina virtual (Windows/Linux)

#### <a name="application-running-on-load-balanced-vms"></a>Aplicação em execução em VMs com balanceamento de carga

Esta arquitetura de referência mostra um conjunto de práticas comprovados para executar várias VMs do Windows na escala definido por trás de um balanceador de carga, para melhorar a disponibilidade e escalabilidade. Esta arquitetura pode ser utilizada para qualquer carga de trabalho sem monitorização de estado, como um servidor web.

![Diagrama de arquitetura de referência para uma aplicação em execução em VMs com balanceamento de carga](media/azure-ddos-best-practices/image9.png)

Nesta arquitetura, uma carga de trabalho é distribuída por várias instâncias de VM. Existe um único endereço IP público, e o tráfego de internet é distribuído às VMs através de um balanceador de carga. Padrão de proteção DDoS está ativado na rede virtual do Azure (internet) do Balanceador de carga que tem o IP público com associados.

O Balanceador de carga distribui pedidos recebidos de internet para as instâncias de VM. Conjuntos de dimensionamento de máquina virtual permitem o número de VMs para ser ampliada entrada ou saída manual ou automaticamente com base nas regras predefinidas. Isto é importante se o recurso estiver sob ataque DDoS. Para obter mais informações sobre esta arquitetura de referência, consulte [neste artigo](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm).

#### <a name="application-running-on-windows-n-tier"></a>Aplicação em execução no Windows N camadas

Existem várias formas de implementar uma arquitetura de N camadas. O diagrama seguinte mostra uma aplicação web de três camadas típica. Esta arquitetura baseia-se o artigo [executar as VMs com balanceamento de carga para escalabilidade e disponibilidade](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm). As camadas Web e Business utilizam VMs com balanceamento de carga.

![Diagrama de arquitetura de referência para uma aplicação em execução no Windows N camadas](media/azure-ddos-best-practices/image10.png)

Nesta arquitetura, DDoS proteção padrão está ativado na rede virtual. Todos os IPs públicos na rede virtual obter proteção DDoS de camada 3 e 4. Para a proteção de 7 camadas, implemente o Gateway de aplicação no WAF SKU. Para obter mais informações sobre esta arquitetura de referência, consulte [neste artigo](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/n-tier).

#### <a name="paas-web-application"></a>Aplicação web PaaS

Esta arquitetura de referência mostra a executar uma aplicação do App Service do Azure numa única região. Esta arquitetura mostra um conjunto de práticas comprovados para uma aplicação web que utiliza [App Service do Azure](https://azure.microsoft.com/documentation/services/app-service/) e [SQL Database do Azure](https://azure.microsoft.com/documentation/services/sql-database/).
Uma região de reserva dinâmica é configurada para cenários de ativação pós-falha.

![Diagrama de arquitetura de referência para uma aplicação web de PaaS](media/azure-ddos-best-practices/image11.png)

Traffic Manager do Azure encaminha os pedidos recebidos para Gateway de aplicação de uma das regiões. Durante as operações normais, encaminha os pedidos no Gateway de aplicação na região Active Directory. Se nessa região ficar indisponível, Gestor de tráfego falhar Gateway de aplicação na região em modo de espera.

Todo o tráfego da internet destinados à aplicação web é encaminhado para o [endereço IP público do Gateway de aplicação](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview) através do Gestor de tráfego. Neste cenário, o serviço de aplicações (aplicação web) propriamente dito não é diretamente externamente enfrentam e é protegido ao Gateway de aplicação. 

Recomendamos que configure o SKU de WAF do Gateway de aplicação (modo impedir) para ajudar a proteger contra ataques de 7 camadas (HTTP/HTTPS/WebSocket). Além disso, as web apps estão configuradas para [aceitar apenas o tráfego do Gateway de aplicação](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/) endereço IP.

Para mais informações sobre esta arquitetura de referência, consulte [neste artigo](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region).

### <a name="mitigation-for-non-web-paas-services"></a>Mitigação para serviços de PaaS não web

#### <a name="hdinsight-on-azure"></a>HDInsight no Azure

Esta arquitetura de referência mostra configurar DDoS proteção padrão para um [cluster do Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/). Certifique-se de que o cluster do HDInsight está ligado a uma rede virtual e que a proteção DDoS está ativada na rede virtual.

!["HDInsight" e "Definições avançadas" painéis, com definições de rede virtual](media/azure-ddos-best-practices/image12.png)

![Seleção para ativar a proteção DDoS](media/azure-ddos-best-practices/image13.png)

Nesta arquitetura, o tráfego destinado ao cluster do HDInsight a partir da internet é encaminhado para o IP público associado com o Balanceador de carga de gateway do HDInsight. O Balanceador de carga do gateway, em seguida, envia o tráfego para os nós principais ou nós de trabalho diretamente. Porque o padrão de proteção DDoS está ativado na rede virtual do HDInsight, todos os IPs públicos na rede virtual obter proteção DDoS de camada 3 e 4. Esta arquitetura de referência pode ser combinada com o N camadas e arquiteturas de multirregião referência.

Para obter mais informações sobre esta arquitetura de referência, consulte o [expandir utilizando uma Azure Virtual Network do Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network?toc=%2fazure%2fvirtual-network%2ftoc.json) documentação.


> [!NOTE]
> Azure ambiente de serviço de aplicações do PowerApps ou a API management numa rede virtual com um IP público são ambos não suportado nativamente.

## <a name="next-steps"></a>Passos Seguintes

* [Página de produto de proteção DDoS do Azure](https://azure.microsoft.com/services/ddos-protection/)

* [Blogue de proteção DDoS do Azure](http://aka.ms/ddosblog)

* [Documentação de proteção DDoS do Azure](../virtual-network/ddos-protection-overview.md)

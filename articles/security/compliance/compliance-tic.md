---
title: Orientação de ligação de Internet fidedigna para o Azure
description: Orientações de ligação de Internet fidedigna para serviços do Azure e SaaS
services: security
author: dlapiduz
ms.assetid: 09511e03-a862-4443-81ac-ede815bdaf25
ms.service: security
ms.topic: article
ms.date: 06/20/2018
ms.author: dlap
ms.openlocfilehash: 9d71efa35713500911c67d1df15612b64c8e97da
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990834"
---
# <a name="trusted-internet-connection-guidance"></a>Documentação de orientação de ligação de Internet fidedigna

## <a name="background"></a>Segundo plano

O objetivo da iniciativa de ligações de Internet fidedigna (TIC), é otimizar e padronizar a segurança de conexões de rede externa individuais atualmente em utilização por agências federais. A política é descrita na OMB (Office de gestão e orçamento) [memorandum M-08-05](https://georgewbush-whitehouse.archives.gov/omb/memoranda/fy2008/m08-05.pdf).

Em Novembro de 2007, o OMB estabelecido o programa de classificação TIC para melhorar a segurança de perímetro de rede federal e funções de resposta a incidentes. Classificação TIC foi projetado para executar a análise de rede de todo o tráfego de entrada e saída gov para identificar assinaturas específicas e com base no padrão de dados e descubra anomalias comportamentais, como a atividade do botnet. Agências foram impostas pela consolidar suas conexões de rede externo e fazer todo o tráfego encaminhado através de intrusão deteção e prevenção de dispositivos (conhecidos como EINSTEIN) que eram hospedados num número limitado de pontos finais de rede (conhecido como Internet fidedigna Ligações).

Em outras palavras, o objetivo da classificação TIC é para agências saber:
- Quem é na minha rede (autorizado ou não autorizado)?
- Quando é minha rede que está sendo acessado e por quê?
- Que recursos estão a ser acedidos?

Hoje em dia que todas as ligações externas de agência devem ser encaminhadas através de uma classificação TIC OMB aprovado. Agências federais são necessários para participar no programa de classificação TIC como um provedor de acesso de classificação TIC (TICAP) ou por contratar os serviços com um dos principais fornecedores de serviços de Internet escalão da 1 referido como fornecedores de geridos fidedigno Internet Protocol Service (MTIPS).  Classificação TIC inclui capacidades fundamentais obrigatórias, que são executadas atualmente pela agências e fornecedor MTIPS. Na versão atual do TIC, a detecção de intrusão EINSTEIN versão 2 e a versão de EINSTEIN 3 acelerada (3A), dispositivos de prevenção de intrusões são implementados em cada TICAP e MTIPS e a Agência estabelece uma Memorandum de compreensão com o departamento de Homeland Security (DHS) para implementar recursos EINSTEIN em sistemas federais.

Como parte das suas responsabilidades na proteção da rede. gov, DHS requer feeds de dados não processados de dados de Netflow Agência para correlacionar incidentes em toda a empresa federal e realizar análise utilizando ferramentas especializadas. Routers DHS fornecem a capacidade de recolher o tráfego de rede IP como ele entra ou sai uma interface. Ao analisar os dados de fluxo de rede numa rede administrador pode determinar coisas como a origem e destino do tráfego, a classe de serviço, etc. Os dados de fluxo de rede são considerados "dados não-content" (por exemplo, cabeçalho, da origem de IP, IP de destino, etc.) e permite DHS conhecer informações em todo o conteúdo; ou seja, que estava fazendo o que e como há muito tempo.

A iniciativa também inclui as políticas de segurança, diretrizes e estruturas que partem do princípio de infraestrutura no local. Como mover organismos públicos para a cloud para alcançar poupanças de custos, a eficiência operacional e a inovação, os requisitos de implementação de classificação TIC são em alguns casos, diminuindo o tráfego de rede e a limitação da velocidade e agilidade com o qual government os utilizadores podem aceder aos respetivos dados com base na cloud.

Este artigo aborda como agências governamentais podem utilizar o Microsoft Azure Government para ajudar a alcançar a conformidade com a política de classificação TIC em todos os serviços de IaaS e PaaS.

## <a name="summary-of-azure-networking-options"></a>Resumo das opções de funcionamento em rede do Azure

Existem três opções principais ao ligar aos serviços do Azure:

1. Ligação de Internet direta: ligar aos serviços do Azure diretamente através de uma conexão de Internet aberta. A média é pública, bem como a ligação. Aplicação e a encriptação de nível de transporte são confiadas para garantir a privacidade. Largura de banda é limitada devido a conectividade de um site com a Internet e de vários fornecedores de Active Directory podem ser utilizados para garantir a resiliência
1. Rede privada virtual: Ligar à sua rede Virtual do Azure em privado com um Gateway de VPN.
A média é pública, como ele percorre a ligação de Internet padrão de um site, mas a conexão seja criptografada num túnel para garantir a privacidade. Largura de banda é limitada consoante os dispositivos VPN e da configuração escolhida. As ligações de Azure ponto a Site são normalmente limitadas a 100 Mbps, enquanto as ligações Site a Site estão limitadas a 1.25 Gbps.
1. Microsoft ExpressRoute: O ExpressRoute é uma ligação direta aos serviços da Microsoft. Uma vez que a conectividade é por meio de um canal de fibra isolado, a ligação pode ser público ou privado, dependendo da configuração utilizada. A largura de banda é normalmente limitada a um máximo de 10 Gbps.

Existem várias formas para satisfazer os requisitos de Trusted Internet ligação apêndice H (considerações sobre a Cloud), foi encontrado no departamento de segurança interna do, "Fidedigna Internet ligações (TIC) arquitetura documento de referência, versão 2.0". Em todo este documento, orientações de DHS TIC serão referida como classificação TIC 2.0.

Para ativar a ligação a partir do departamento ou agência (D/A) para o Azure ou Office 365, sem encaminhamento de tráfego através de D/A TIC, D/R tem de utilizar um túnel encriptado e/ou uma ligação dedicada para o fornecedor de serviços Cloud (CSP). Os serviços CSP podem certificar-se de que conectividade com os ativos de D/A cloud não são oferecidas para a internet pública para acesso de pessoal de agência direto.

Office 365 está em conformidade com a classificação TIC 2.0 apêndice H usando qualquer uma das Express Route com [Peering da Microsoft](https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings#expressroute-routing-domains) ativada ou uma ligação à internet, que criptografa todo o tráfego utilizando TLS 1.2.  Os utilizadores finais de D/R na rede D/A, pode ligar-se por meio de sua rede de agência e a infraestrutura de classificação TIC através da internet. Todos os acessos de internet remoto ao O365 é bloqueado e encaminhado através da Agência. D/A também pode ligar ao Office 365 através de uma ligação do Expressroute com peering da Microsoft, que é um tipo de Peering público, ativada.  

Para o Azure, as opções de 2 (VPN) e 3 (ExpressRoute) podem satisfazer estes requisitos, quando são utilizadas em conjunto com os serviços que limitam o acesso à Internet.

![Microsoft fidedignos diagrama de ligação (TIC) da Internet](media/tic-diagram-a.png)

## <a name="how-azure-infrastructure-as-a-service-offerings-can-help-with-tic-compliance"></a>Como infraestrutura como um ofertas de serviço do Azure pode ajudar a conformidade de classificação TIC

Cumprimento de política de classificação TIC com o IaaS é relativamente simples, uma vez que os clientes do Azure gerir o seu próprio tipo de encaminhamento de rede virtual.

O principal requisito para o ajudar a garantir a conformidade com a arquitetura de referência do PEDI é garantir que a rede Virtual (VNet) torna-se uma extensão privada da rede do departamento ou da Agência. Para se tornar um _privada_ extensão, a política requer tráfego não sai da sua rede, exceto através da ligação de rede no local PEDI. Este processo é conhecido como "Imposição de túnel", que, quando utilizado para a conformidade de classificação TIC, é o processo de encaminhamento de todo o tráfego de qualquer sistema no ambiente de CSP para aceder através de um gateway no local na rede de uma organização saída à internet através da classificação TIC.

Conformidade de classificação TIC de IaaS do Azure pode ser dividida em dois passos principais:

1. Configuração
1. Auditoria

### <a name="azure-iaas-tic-compliance-configuration"></a>Configuração de conformidade de classificação TIC de IaaS do Azure

Para configurar uma arquitetura TIC compatíveis com o Azure, terá de primeiro impedir o acesso direto à internet à sua rede virtual e, em seguida, forçar o tráfego de internet através da rede no local.

#### <a name="prevent-direct-internet-access"></a>Impedir o acesso à Internet direta

Redes de IaaS do Azure é realizada por meio de redes virtuais compostos por sub-redes, para que os controladores de Interface de rede (NICs) de máquinas virtuais estão associados.

O cenário mais simples para suportar a conformidade de classificação TIC é garantir que uma Máquina Virtual ou uma coleção deles, não é possível ligar a quaisquer recursos externos. A desativação de redes externas pode ser garantida com grupos de segurança de rede (NSGs), que podem ser utilizadas para controlar o tráfego para um ou mais NICs ou sub-redes na rede virtual. Um NSG contém as regras de controlo de acesso que permitem ou negam tráfego com base na direção, protocolo, endereço e porta de origem e endereço e porta de destino do tráfego. As regras de um NSG podem ser alteradas em qualquer altura e as alterações são aplicadas a todas as instâncias associadas.  Para saber mais sobre como criar um NSG, consulte este artigo [criar um NSG](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

#### <a name="force-internet-traffic-through-on-premises-network"></a>Forçar o tráfego de Internet através de rede no local

O Azure cria automaticamente as rotas do sistema e atribui-as a cada sub-rede numa rede virtual. Não é possível criar rotas do sistema nem pode removê-las, mas pode substituir algumas por rotas personalizadas. Azure cria rotas do sistema para cada sub-rede predefinidas e adiciona as rotas predefinidas opcionais adicionais para sub-redes específicas ou todas as sub-redes, quando utiliza capacidades do Azure específicas. Esse roteamento garante que o tráfego destinado dentro da rede Virtual permanece dentro da rede Virtual, IANA designado espaços de endereço privado como 10.0.0.0/8 são ignoradas (a não ser incluído no espaço de endereços da rede Virtual) e o encaminhamento de "último recurso" de 0.0.0.0/0 para o ponto final de Internet da rede Virtual.

![Classificação TIC imposição de túnel](media/tic-diagram-c.png)

Para garantir que todo o tráfego atravessa o TIC D/A, todo o tráfego de deixar a rede Virtual tem de ser encaminhado através da ligação no local.  Criar rotas personalizadas por qualquer uma das rotas definidas pelo utilizador criação ou por meio da troca as de rotas do border gateway protocol (BGP) entre o gateway de rede no local e um gateway de rede virtual do Azure. Obter mais informações sobre as rotas definidas pelo utilizador podem ser encontradas em https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#user-defined. Obter mais informações sobre o protocolo BGP também podem ser encontradas em https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#border-gateway-protocol.

#### <a name="user-defined-routes"></a>Rotas definidas pelo utilizador

Se estiver a utilizar um Gateway de rede Virtual baseado na rota, imposição de túnel pode ser feita no Azure, adicionando um tráfego de 0.0.0.0/0 de definição de rota definida pelo utilizador (UDR) para ser encaminhado para um "próximo salto" do Gateway da rede Virtual. Azure dá prioridade rotas de utilizador definidas através de rotas definidas do sistema, portanto, isso resultaria em todo o tráfego não-VNet que está a ser enviado para o Gateway de rede Virtual, o que pode, em seguida, encaminhar para no local. Depois de definida, esta rota definidas pelo utilizador tem de ser associada a todas as sub-redes existentes ou recentemente criada dentro de todas as redes virtuais no seu ambiente do Azure.

![rotas definidas pelo utilizador e classificação TIC](media/tic-diagram-d.png)

#### <a name="border-gateway-protocol"></a>Protocolo BGP

Se estiver a utilizar o ExpressRoute ou uma Border Gateway Protocol (BGP) ativadas Gateway de rede Virtual, o BGP é o mecanismo preferencial para o anúncio de rotas. Com o BGP anunciada rota de 0.0.0.0/0 e Gateways de rede de Virtual com reconhecimento de BGP de ExpressRoute serão Certifique-se de que essa rota padrão é aplicada a todas as sub-redes em suas redes virtuais.

### <a name="azure-iaas-tic-compliance-auditing"></a>Auditoria de conformidade de TIC de IaaS do Azure

O Azure oferece várias formas para auditar a conformidade de classificação TIC.

#### <a name="effective-routes"></a>Rotas Efetivas

Para confirmar que a rota predefinida tiver sido propagada, pode observar que "Rotas efetivas" de uma VM específica, uma NIC específica ou uma tabela de rotas definidas pelo utilizador. Isso pode ser feito através do portal do Azure, tal como descrito em https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-portal, ou através do PowerShell, conforme descrito em https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-powershell. O painel de rotas efetivas permitirá que veja que as rotas de definidas pelo utilizador relevantes, BGP anunciados rotas e rotas de sistema que se aplicam à entidade relevante, como mostrado abaixo.

![captura de ecrã de rotas](media/tic-screen-1.png)

**Tenha em atenção**: não é possível ver as rotas efetivas para uma NIC, a menos que ela associada a uma VM em execução.

#### <a name="network-watcher"></a>Observador de Rede

O observador de rede do Azure oferece várias ferramentas que podem ser utilizadas para auditar a conformidade de classificação TIC.  Saiba mais sobre o observador de rede em https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview.

##### <a name="network-security-groups-flow-logs"></a>Registos de fluxo de grupos de segurança de rede 

O observador de rede do Azure fornece a capacidade de capturar os registos de fluxo de rede que indica os metadados que envolvem o tráfego IP. Além de outros dados, os registos de fluxo de rede contêm os endereços de origem e os destinos de destinos. Isto, combinado com os registos do Gateway de rede Virtual, dispositivos de ponta no local e/ou o TIC, irá permitir a monitorização que todo o tráfego, de fato, está a ser roteado no local. 

## <a name="how-azure-platform-as-a-service-offerings-can-help-with-tic-compliance"></a>Como plataforma do Azure como ofertas de serviço pode ajudar a conformidade de classificação TIC

Serviços de PaaS do Azure como armazenamento do Azure são acessíveis através de um URL acessível pela interne. Qualquer pessoa com credenciais aprovadas pode aceder ao recurso, como uma conta de armazenamento, de qualquer local sem atravessa um TIC. Por esse motivo, muitos clientes do Governo concluir incorretamente que os serviços PaaS do Azure não estão em conformidade com políticas de classificação TIC. Na verdade, muitos serviços de PaaS do Azure podem estar em conformidade com a política de classificação TIC usando a mesma arquitetura como um ambiente de IaaS TIC conforme descrito acima, se eles podem ser anexados a uma rede Virtual (VNet). Integração de serviços de PaaS do Azure com uma VNet do Azure permite que o serviço deve estar em privado acessível a partir dessa VNet e permite personalizado de encaminhamento para 0.0.0.0/0 a ser aplicado através de rotas definidas pelo utilizador ou do BGP, garantindo que todo o tráfego vinculado à Internet é encaminhado no local para atravesse o TIC.  Alguns serviços do Azure podem ser integrados em Vnets utilizando os seguintes padrões:

- **Implemente uma instância dedicada do serviço**: um número crescente de serviços podem ser implementados como instâncias dedicadas com VNet de PaaS anexado pontos de extremidade. Por exemplo, um ambiente de serviço de aplicações (ASE) pode ser implementado no modo "Em ambiente isolado", permitindo que o seu ponto de extremidade de rede restrito a uma VNet. Neste ASE, em seguida, pode alojar vários serviços de PaaS do Azure, como aplicações Web, APIs e funções.
- **Pontos finais de serviço de VNet**: um número crescente de serviços de PaaS permitem que a opção de mover o ponto de extremidade para um IP privado da VNet em vez de um endereço público.

Serviços que suportam a implementação de instâncias dedicadas numa VNet ou pontos finais de serviço a partir de Maio de 2018 são listados abaixo: * (disponibilidade representa comercial do Azure, Azure Government disponibilidade pendentes).

### <a name="service-endpoints"></a>Pontos Finais de Serviço

|Serviço                   |Estado            |
|--------------------------|------------------|
|Cofre de chaves do Azure            | Pré-visualização privada  |
|BD do Cosmos                 | Pré-visualização privada  |
|Identidade                  | Pré-visualização privada  |
|Azure Data Lake           | Pré-visualização privada  |
|SQL Postgress/Mysql       | Pré-visualização privada  |
|Azure SQL Data Warehouse  | Pré-visualização pública   |
|SQL do Azure                 | GA               |
|Armazenamento                   | GA               |

### <a name="vnet-injection"></a>Injeção de VNet

|Serviço                            |Estado            |
|-----------------------------------|------------------|
|Instância gerida de SQL               | Pré-visualização pública   |
|Service(AKS) de contentor do Azure       | Pré-visualização pública   |
|Service Fabric                     | GA               |
|Gestão de API                     | GA               |
|Azure Active Directory             | GA               |
|Azure Batch                        | GA               |
|ASE                                | GA               |
|Redis                              | GA               |
|HDI                                | GA               |
|Conjunto de dimensionamento de máquinas virtuais de computação  | GA               |
|Serviço em nuvem de computação              | GA               |

### <a name="vnet-integration-details"></a>Detalhes de integração de VNet

O diagrama abaixo descreve gerais fluxo de rede para o acesso aos serviços de PaaS com a Injeção de VNet e túnel do serviço de VNet.  Obter mais informações sobre Gateways do serviço de rede, da VNet e as etiquetas de serviço podem ser encontradas aqui https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags.

![Opções de conectividade de PaaS para classificação TIC](media/tic-diagram-e.png)

1. Ligação privada com o Azure através do ExpressRoute. Peering privado do ExpressRoute com túnel forçado é usado para forçar todo o tráfego de VNet de cliente através do ExpressRoute para o local. Peering da Microsoft não é necessário.
2. O Gateway de VPN do Azure utilizados em conjunto com peering da Microsoft do ExpressRoute pode ser utilizado para encriptação de IPSec de ponto a ponto entre a VNet de cliente e o limite de locais de sobreposição. 
3. Conectividade de rede para a VNet de clientes é controlada com grupos da segurança de rede (NSG) permitindo que os clientes permitir/recusar baseado em IP, porta e protocolo.
4. A VNet de cliente é expandida para o serviço de PaaS através da criação de um ponto final de serviço para o serviço do cliente.
5. O ponto de final de serviço de PaaS é protegido para predefinição recusar tudo e apenas permitir o acesso de sub-redes especificadas dentro da VNet do cliente.  A predefinição de negação também inclui ligações provenientes da Internet.
6. Outros serviços do Azure que precisam de aceder a recursos dentro da VNet de cliente devem ser:  
  a. Diretamente implementadas na VNet  
  b. Permitido seletivamente com base na documentação de orientação do serviço do Azure correspondente.

#### <a name="option-1-dedicated-instance-vnet-injection"></a>Opção 1: Instância dedicada "Injeção de VNet"

Com a Injeção de VNet, os clientes seletivamente podem implementar instâncias dedicadas de um determinado serviço do Azure, como o HDInsight, em sua própria VNet. Instâncias de serviço são implementadas numa sub-rede dedicada na VNet de um cliente. Injeção de VNet permite que os recursos de serviço estar acessíveis através de endereços de encaminháveis não Internet.  Instâncias no local podem aceder a estas instâncias de serviço por meio do espaço de endereços da VNet diretamente através do ExpressRoute ou VPN de Site a Site, em vez de abrir firewalls ao espaço de endereços de Internet público. Com uma instância dedicada anexada a um ponto de extremidade, as mesmas estratégias utilizadas para conformidade IaaS TIC podem ser utilizadas, com o padrão garantir que o tráfego vinculado à Internet de encaminhamento é redirecionado para um Gateway de rede Virtual vinculado no local. Entrada e acesso de saída pode ser ainda mais controlado por meio dos grupos de segurança de rede (NSGs) para a sub-rede especificada.

![Diagrama de descrição geral de Injeção de VNet](media/tic-diagram-f.png)

#### <a name="option-2-vnet-service-endpoints"></a>Opção 2: Pontos finais de serviço de VNet 

Um número crescente de serviços de multi-inquilinos do Azure oferece a capacidade de "Ponto final de serviço", um método alternativo para integrar a VNets do Azure. Pontos finais de serviço de VNet expandem o seu espaço de endereços IP de VNet e a identidade da sua VNet para o serviço através de uma ligação direta.  Tráfego da VNet ao serviço do Azure permanece sempre na rede backbone do Azure. Depois de um ponto de extremidade de serviço está ativado para um serviço, ligações ao serviço podem ser restringidas a essa VNet através de políticas expostos pelo serviço. Verificações de acesso são impostas na plataforma pelo serviço do Azure e o acesso ao recurso bloqueado é concedido apenas se o pedido tem origem da VNet/subrede permitido, e/ou os dois IPs utilizado para identificar o tráfego de OnPremises, se utilizar o ExpressRoute. Isso pode ser usado para de fato impedem que o tráfego de entrada/saída de sair diretamente o serviço de PaaS.

![Diagrama de descrição geral de pontos finais de serviço](media/tic-diagram-g.png)

## <a name="using-cloud-native-tools-for-network-situational-awareness"></a>Com as ferramentas nativas de cloud para situacional de rede

O Azure disponibiliza cloud ferramentas nativas para ajudar a garantir que tem a percepção situacional necessária para compreender os fluxos de tráfego da sua rede. Eles não são necessários para estar em conformidade com a política de classificação TIC mas podem melhorar bastante as capacidades de segurança.

### <a name="azure-policy"></a>Azure Policy

O Azure Policy (https://azure.microsoft.com/services/azure-policy/) é um serviço do Azure que fornece a sua organização com melhor capacidade de auditoria e impor as iniciativas de conformidade.  Atualmente disponível em pré-visualização pública nas Clouds comerciais do Azure, mas ainda não no Microsoft Azure para a administração pública, clientes que valorizam o TIC pode começar a planear e testar suas regras de política de garantia de compatibilidade futura. Política do Azure destina-se ao nível da subscrição e fornece uma interface centralizada para gerenciar iniciativas, definições de política, os resultados de imposição e auditoria e gestão de exceções. Além de muitas incorporadas do Azure definições de política, os administradores podem definir suas próprias definições personalizadas através de modelos json simples. Para muitos clientes, a Microsoft recomenda a priorização de auditoria sobre imposição sempre que possível.

As políticas de exemplo seguintes podem ser úteis para cenários de conformidade de classificação TIC:

|Política  |Cenário de exemplo  |A partir de modelo  |
|---------|---------|---------|
|Impor a tabela de rotas definidas pelo utilizador |     Certifique-se de que a rota padrão em todos os pontos de redes virtuais para um Gateway de rede Virtual aprovados para o roteamento no local | https://docs.microsoft.com/azure/azure-policy/scripts/no-user-def-route-table |
|Se o observador de rede não está ativado para a região de auditoria  | Certifique-se de que o observador de rede está ativado para todas as regiões de utilizada  | https://docs.microsoft.com/azure/azure-policy/scripts/net-watch-not-enabled |
|NSG X em cada sub-rede  | Certifique-se de que um NSG (ou conjunto de NSGs aprovados) com o tráfego de Internet bloqueado é aplicado a todas as sub-redes em cada VNet | https://docs.microsoft.com/azure/azure-policy/scripts/nsg-on-subnet |
|NSG X em cada NIC | Certifique-se de que um NSG com tráfego de Internet bloqueado é aplicado a todos os NICs em todas as VMs. | https://docs.microsoft.com/azure/azure-policy/scripts/nsg-on-nic |
|Uso aprovado VNet para interfaces de rede VM  | Certifique-se de que todas as NICs estão numa VNet aprovada | https://docs.microsoft.com/azure/azure-policy/scripts/use-approved-vnet-vm-nics |
|Localizações permitidas | Certifique-se de que todos os recursos são implementados em regiões com a configuração de VNets e o observador de rede em conformidade  | https://docs.microsoft.com/azure/azure-policy/scripts/allowed-locs |
|Tipos de recursos, tais como PublicIPs não permitidos  | Proibir a implementação de tipos de recursos, o que não têm um plano de conformidade. Por exemplo, esta política pode ser utilizada para proibir a implementação de recursos de endereço IP público. Enquanto as regras do NSG podem ser utilizadas para bloquear o tráfego de Internet de entrada com eficiência, evitar o uso de IPs públicos ainda mais reduz a superfície de ataque.    | https://docs.microsoft.com/azure/azure-policy/scripts/not-allowed-res-type  |

### <a name="azure-traffic-analyticshttpsazuremicrosoftcomen-inblogtraffic-analytics-in-preview"></a>Azure [análise de tráfego](https://azure.microsoft.com/en-in/blog/traffic-analytics-in-preview/)

Análise de tráfego do Azure observador de rede consome dados de registo de fluxo e outros registos para fornecer a descrição geral de alto nível de tráfego de rede. Estes dados podem ser útil para auditoria de conformidade de classificação TIC e identificar pontos problemáticos. Um dashboard de alto nível pode servir-se rapidamente ecrã de que as VMs estão a comunicar com a internet, o que, em seguida, forneceria uma lista direcionada para o encaminhamento do PEDI.

![Captura de ecrã de análise de tráfego](media/tic-traffic-analytics-1.png)

"Mapa geográfico" pode ser utilizado para identificar rapidamente os destinos provavelmente físicos do tráfego de Internet para as suas VMs, que lhe permite identificar e fazer a triagem localizações suspeitas ou alteração de padrões.

![Captura de ecrã de análise de tráfego](media/tic-traffic-analytics-2.png)

Um mapa de topologia de rede pode ser utilizado para redes virtuais existentes de pesquisa rapidamente:

![Captura de ecrã de análise de tráfego](media/tic-traffic-analytics-3.png)

### <a name="azure-network-watcher-next-hop"></a>Salto seguinte de observador de rede do Azure

Redes em regiões monitorizados pelo observador de rede podem realizar testes de "Próximo salto", permitindo acesso fácil com base no Portal, digite uma origem e destino para um fluxo de rede de exemplo, para que o observador de rede irá resolver o destino de "Próximo salto". Isso pode ser utilizado em relação a VMs e endereços de Internet de exemplo para se certificar de que o "próximo salto" é realmente o Gateway de rede Virtual.

![Salto seguinte de observador de rede](media/tic-network-watcher.png)

## <a name="conclusions"></a>Conclusões

Acesso do Microsoft Azure, Office 365 e Dynamics 365 pode ser configurado facilmente para ajudar a estar em conformidade com diretrizes de classificação TIC 2.0 apêndice H como escrito e definidos em Maio de 2018.  A Microsoft está ciente de que esta orientação está sujeitas a alterações e será esforçar ajudar os clientes a cumprir as diretrizes de forma atempada conforme novas diretrizes são lançadas.

## <a name="appendix-tic-patterns-for-common-workloads"></a>Apêndice: TIC padrões para cargas de trabalho comuns

| Categoria | Carga de trabalho | IaaS | Dedicado PaaS / Injeção de VNet  | Pontos Finais de Serviço  |
|---------|---------|---------|---------|--------|
| Computação | Máquinas Virtuais do Linux | Sim | | |
| Computação | Máquinas Virtuais do Windows | Sim | | |
| Computação | Conjuntos de Dimensionamento de Máquinas Virtuais | Sim | | |
| Computação | Funções do Azure | | através do ambiente de serviço de aplicações (ASE) | |
| Web e Móvel | Aplicação Web interna | | através do ambiente de serviço de aplicações (ASE) | |
| Web e Móvel | Aplicação móvel interna | | através do ambiente de serviço de aplicações (ASE) | |
| Web e Móvel | Aplicações API | | através do ambiente de serviço de aplicações (ASE) | |
| Contentores | Azure Container Service (ACS) | | | Sim |
| Contentores | Azure Container Service (AKS) * | | | Sim |
| Base de Dados | SQL Database | | SQL do Azure da base de dados geridos instância * | SQL do Azure |
| Base de Dados | Base de Dados do Azure para MySQL | | | Sim |
| Base de Dados | Base de Dados do Azure para PostgreSQL | | | Sim |
| Base de Dados | SQL Data Warehouse | | | Sim |
| Base de Dados | Azure Cosmos DB | | | Sim |
| Base de Dados | Cache de Redis | | Sim | |
| Armazenamento | Blobs | Sim | | |
| Armazenamento | Ficheiros | Sim | | |
| Armazenamento | Filas | Sim | | |
| Armazenamento | Tabelas | Sim | | |
| Armazenamento | Discos | Sim | | |

*: Pré-visualização pública do Azure Government, a partir de Maio de 2018  
* *: Pré-visualização privada no Azure Government, a partir de Maio de 2018

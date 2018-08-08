---
title: Datacenter Virtual do Azure – uma perspectiva de rede
description: Aprenda a criar o seu centro de dados virtual no Azure
services: networking
author: tracsman
manager: rossort
tags: azure-resource-manager
ms.service: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/3/2018
ms.author: jonor
ms.openlocfilehash: fc3f334771c11d6917e15628557adfb59051f0f6
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39621026"
---
# <a name="azure-virtual-datacenter-a-network-perspective"></a>Datacenter Virtual do Azure: Uma perspetiva de rede
**Microsoft Azure**: mover-se rapidamente, poupar dinheiro, integrar dados e aplicações no local

## <a name="overview"></a>Descrição geral
Migrar aplicações no local para o Azure, mesmo sem qualquer alteração significativa (uma abordagem conhecida como "lift- and -shift"), fornece às organizações os benefícios de uma infraestrutura segura e económica. No entanto, para aproveitar ao máximo a agilidade possíveis com a informática na cloud, as empresas devem evoluir suas arquiteturas para tirar partido das capacidades do Azure. Microsoft Azure fornece serviços de hiper escala e infraestrutura, capacidades de nível empresarial e confiabilidade e muitas opções de conectividade híbrida. Os clientes podem optar por aceder a estes serviços na cloud através da Internet ou com o Azure ExpressRoute, que fornece conectividade de rede privada. A plataforma Microsoft Azure permite aos clientes expandir sua infra-estrutura para a cloud e crie arquiteturas de várias camadas de forma totalmente integrada. Além disso, os parceiros da Microsoft fornecem recursos aprimorados, oferecendo serviços de segurança e de aplicações virtuais que estão otimizadas para execução no Azure.

Este artigo fornece uma visão geral dos padrões e estruturas que podem ser utilizadas para resolver as preocupações arquiteturais de dimensionamento, desempenho e segurança muitas que os clientes enfrentam ao pensar em massa mover para a cloud. Uma descrição geral de como de acordo com as diferentes funções de TI organizacionais para a gestão e governação do sistema também são discutidos, com ênfase aos requisitos de segurança e a otimização de custos.

## <a name="what-is-a-virtual-data-center"></a>O que é um centro de dados do Virtual?
Nos primórdios, soluções na cloud foram concebidas para aplicativos de única e relativamente isolada do host, no espectro público. Essa abordagem funcionou bem para alguns anos. No entanto, como os benefícios da nuvem soluções se tornou aparentes e várias cargas de trabalho em grande escala foram alojadas na cloud, segurança, confiabilidade, desempenho, de endereçamento e custos preocupações de implementações de uma ou mais regiões se tornou essenciais ao longo do ciclo de vida do serviço em nuvem.

O diagrama de implementação de nuvem seguinte ilustra alguns exemplos de falhas de segurança (caixa vermelha) e espaço para aplicações virtuais de rede de otimização em cargas de trabalho (caixa amarela).

[![0]][0]

O Centro de dados Virtual (vDC) nasceu dessa necessidade de dimensionamento para suportar cargas de trabalho empresariais e a necessidade de lidar com os problemas apresentados ao fornecer suporte a aplicativos em larga escala na cloud pública.

Uma vDC não é apenas as cargas de trabalho aplicações na cloud, mas também a rede, segurança, gestão e infraestrutura (por exemplo, DNS e serviços de diretório). Normalmente, também fornece uma ligação privada para um centro de dados ou de rede no local. Como mover cargas de trabalho cada vez mais para o Azure, é importante pensar sobre a infraestrutura de suporte e os objetos que estas cargas de trabalho são colocadas em. Pensar com cuidado sobre como os recursos são estruturados pode evitar a proliferação de centenas de "Ilhas de carga de trabalho" que tem de ser geridas separadamente com o fluxo de dados independente, modelos de segurança e desafios de compatibilidade.

Centro de dados Virtual é essencialmente uma coleção de entidades diferentes mas relacionadas com a infraestrutura, funcionalidades e funções de suporte comuns. Ao visualizar as suas cargas de trabalho como uma vDC integrada, pode obter a custo reduzido devido a economia de escala, segurança otimizada por meio de centralização de fluxo de componentes e dados, juntamente com as operações mais fácil, gestão e as auditorias de conformidade.

> [!NOTE]
> É importante compreender que é o vDC **não** um produto do Azure discreto, mas a combinação de várias funcionalidades e capacidades para satisfazer as suas necessidades exatas. vDC é uma maneira de pensar sobre suas cargas de trabalho e a utilização do Azure para maximizar os recursos e capacidades na cloud. O controlador de domínio virtual, por conseguinte, é uma abordagem modular sobre como criar serviços de TI no Azure, sem deixar de respeitar organizacionais funções e responsabilidades.

O vDC pode ajudar as empresas a obter as cargas de trabalho e aplicações para o Azure para os seguintes cenários:

-   Alojar várias cargas de trabalho relacionadas
-   Migrar cargas de trabalho de um ambiente no local para o Azure
-   Implementando segurança partilhada ou centralizada e requisitos de acesso em cargas de trabalho
-   Misturar o DevOps e de TI centralizada adequadamente, de uma grande empresa

A chave para desbloquear as vantagens de vDC, é uma topologia centralizada (hub- and -spoke) com uma combinação de funcionalidades do Azure: [VNet do Azure][VNet], [NSGs] [ NSG], [Peering de VNet][VNetPeering], [rotas definidas pelo utilizador (UDR)][UDR]e a identidade do Azure com [Base de função (RBAC) do controlo de acesso][RBAC].

## <a name="who-needs-a-virtual-data-center"></a>Quem precisa de um Datacenter Virtual?
Todos os clientes do Azure que tem de mover mais do que algumas das cargas de trabalho para o Azure podem beneficiar de pensar sobre a utilização de recursos comuns. Consoante a magnitude, até mesmo aplicativos podem se beneficiar usando os padrões e os componentes utilizados para criar uma vDC.

Se a organização tiver uma segurança de TI, rede, centralizada, e/ou a equipe de conformidade/departamento, uma vDC pode ajudar a impor a política pontos, segregação de imposto e certifique-se uniformidade dos componentes comuns subjacentes, dando-as equipas das aplicações muito mais liberdade e o controle, conforme apropriado para os seus requisitos.

As organizações que procuram DevOps podem utilizar os conceitos de vDC para fornecer autorizados alguns dos recursos do Azure e certifique-se de que eles tenham controle total dentro desse grupo (a subscrição ou grupo de recursos numa subscrição comum), mas a rede e limites de segurança mantenha-se em conformidade, conforme definido por uma diretiva centralizada numa VNet do hub e o grupo de recursos.

## <a name="considerations-on-implementing-a-virtual-data-center"></a>Considerações sobre como implementar um Datacenter Virtual
Ao conceber uma vDC, há vários problemas pivotal a considerar:

-   Identidade e de serviços de diretório
-   Infraestrutura de segurança
-   Conectividade para a cloud
-   Conectividade na cloud

##### <a name="identity-and-directory-service"></a>*Serviço de diretório e identidade*
Serviços de identidade e de diretório são um aspecto fundamental dos dados de todos os datacenters no local e na cloud. Identidade está relacionado com todos os aspetos de acesso e autorização para serviços dentro de vDC. Para ajudar a garantir que apenas os utilizadores autorizados e processos de aceder à sua conta do Azure e recursos, o Azure utiliza vários tipos de credenciais para autenticação. Estes incluem palavras-passe (para aceder à conta do Azure), chaves de criptografia, assinaturas digitais e certificados. [*O Azure multi-factor Authentication* (MFA)] [ MFA] é uma camada adicional de segurança para aceder aos serviços do Azure. O Azure MFA fornece uma autenticação segura com uma variedade de opções de verificação simples — telefonema, mensagem de texto ou notificação de aplicação móvel — e permita aos clientes escolherem o método que preferem.

Qualquer grande empresa tem de definir um processo de gestão de identidade que descreve a gestão de identidades individuais, seus autenticação, autorização, funções e privilégios dentro ou entre o vDC. Os objetivos deste processo devem ser aumentar a segurança e a produtividade e diminuir o custo, tempo de inatividade e tarefas manuais repetitivas.

/ Organizações empresariais pode exigir uma mistura exigente de serviços para a linha-de-empresas diferentes (LOBs) e os empregados têm, muitas vezes, diferentes funções quando envolvidos com projetos diferentes. Uma vDC requer boa cooperação entre equipes diferentes, cada um com definições de funções específicas, para sistemas em execução com boa governação. A matriz de responsabilidades, acesso e os direitos pode ser extremamente complexa. Gestão de identidades nas vDC é implementada através de [ *do Azure Active Directory* (AAD)] [ AAD] e controlo de acesso baseado em funções (RBAC).

Um serviço de diretório é uma infraestrutura de informações compartilhadas para localizar, gerir, administrar e organizar os itens de todos os dias e recursos de rede. Estes recursos podem incluir volumes, pastas, arquivos, impressoras, utilizadores, grupos, dispositivos e outros objetos. Cada recurso na rede é considerado um objeto o servidor de diretório. Informações sobre um recurso são armazenadas como uma coleção de atributos associados esse recurso ou objeto.

Todos os serviços de negócios online da Microsoft se baseiam no Azure Active Directory (AAD) para início de sessão e precisa de outra identidade. O Azure Active Directory é uma solução em cloud de gestão de acessos e identidades altamente disponível e completa que combina serviços de diretório centrais, governação de identidade avançada e gestão de acesso da aplicação. AAD pode ser integrado no Active Directory no local para ativar o início de sessão único para todos os com base na cloud e alojado localmente (no local) aplicativos. Os atributos de utilizador do Active Directory no local podem ser sincronizados automaticamente para o AAD.

Um administrador global único não é necessária para atribuir a todas as permissões numa vDC. Em vez disso, cada departamento específico (ou grupo de utilizadores ou serviços no serviço de diretório) pode ter as permissões necessárias para gerir os seus próprios recursos dentro de uma vDC. Estruturar permissões requer balanceamento. Demasiadas permissões podem impedir a eficiência de desempenho e muito poucos ou permissões flexíveis podem aumentar os riscos de segurança. Ajuda do Azure com base em função de controlo de acesso (RBAC) para resolver esse problema, oferecendo a gestão de acessos detalhada para os recursos de vDC.

##### <a name="security-infrastructure"></a>*Infraestrutura de segurança*
Infraestrutura de segurança, no contexto de uma vDC, principalmente está relacionado com a segregação de tráfego no segmento de rede virtual específica de vDC, e como controlar a entrada e de saída flui em todo o vDC. Azure baseia-se na arquitetura de multi-inquilino, que impede que o tráfego não autorizado e não intencional entre implementações, usando o isolamento de rede Virtual (VNet), acessar listas de controle (ACLs), balanceadores de carga e filtros IP, juntamente com as políticas de fluxo de tráfego. Tradução de endereços de rede (NAT) separa o tráfego de rede interno do tráfego externo.

Recursos de infraestrutura do Azure aloca recursos de infraestrutura para cargas de trabalho de inquilino e gere comunicações de e para máquinas virtuais (VMs). O hipervisor do Azure impõe a separação de memória e o processo entre VMs e com segurança rotas de tráfego de rede para inquilinos do SO convidado.

##### <a name="connectivity-to-the-cloud"></a>*Conectividade para a cloud*
O vDC necessita de conectividade com redes externas para oferecer serviços a clientes, parceiros e/ou os usuários internos. Isso normalmente significa a conectividade não apenas à Internet, mas também a centros de dados e redes no local.

Os clientes podem criar as políticas de segurança para controlar o que e como alojados de vDC específico serviços são acessíveis de/para a Internet através de aplicações virtuais de rede (com a inspeção de tráfego e de filtragem) e personalizados de encaminhamento de políticas e de rede (de filtragem Encaminhamento definido pelo utilizador e grupos de segurança de rede).

As empresas muitas vezes precisam se conectar vDCs a centros de dados no local ou outros recursos. A conectividade entre redes do Azure e no local, por conseguinte, é um aspecto crucial quando o projeto de uma arquitetura eficiente. As empresas têm duas formas diferentes de criar uma interligação entre vDC e no local no Azure: trânsito através da Internet e/ou privadas ligações diretas.

Uma [ **VPN de Site a Site do Azure** ] [ VPN] é um serviço de interligação através da Internet entre redes no local e encriptado de vDC, estabelecida através de seguros ligações (túneis IPsec/IKE). Ligação de Site a Site do Azure é flexível e rápido criar e não requer qualquer aprovisionamento adicional, como todas as ligações ligam através da internet.

[**ExpressRoute** ] [ ExR] é um serviço de conectividade do Azure que lhe permite criar ligações privadas entre vDC e as redes no local. As ligações ExpressRoute não entram na Internet pública e oferecem maior segurança, fiabilidade e maiores velocidades (até 10 Gbps), juntamente com a latência consistente. O ExpressRoute é muito útil para vDCs, como os clientes podem obter os benefícios de regras de conformidade associados a ligações privadas de ExpressRoute.

Implementar as ligações ExpressRoute envolve a começar a usar um provedor de serviços do ExpressRoute. Para clientes que precisam para começar rapidamente, é comum usar inicialmente VPN Site a Site para estabelecer a conectividade entre o vDC e recursos no local e, em seguida, migrar para a ligação do ExpressRoute.

##### <a name="connectivity-within-the-cloud"></a>*Conectividade na cloud*
[VNets] [ VNet] e [VNet Peering] [ VNetPeering] são os serviços de conectividade de rede básicos dentro de uma vDC. Uma VNet garante um limite natural de isolamento para os recursos de vDC e VNet peering permite intercommunication entre VNets de diferentes na mesma região do Azure ou até mesmo em várias regiões. Controlo de tráfego dentro de uma VNet e entre VNets têm de corresponder um conjunto de regras de segurança especificado por meio de listas de controlo de acesso ([grupo de segurança de rede][NSG]), [aplicações virtuais de rede ] [ NVA]e as tabelas de encaminhamento personalizadas ([UDR][UDR]).

## <a name="virtual-data-center-overview"></a>Descrição geral do Centro de dados virtual

### <a name="topology"></a>Topologia
Modelo de hub e spokes estendido o Datacenter Virtual numa única região do Azure

[![1]][1]

O hub é a zona central que controla e inspeciona o tráfego de entrada e/ou saída entre zonas diferentes: Internet, no local e os spokes. A topologia hub- and -spoke dá uma maneira eficiente de impor as políticas de segurança numa localização central, enquanto reduz o potencial para uma configuração incorreta e exposição do departamento de TI.

O hub contém os componentes comuns do serviço consumidos pelos spokes. Aqui estão alguns exemplos típicos de serviços centrais comuns:

-   A infra-estrutura do Active Directory do Windows (com o serviço ADFS relacionado) necessária para a autenticação de utilizador de terceiros por aceder a partir de redes não fidedignas antes de poder aceder às cargas de trabalho no spoke
-   Um serviço DNS para resolver a nomenclatura para a carga de trabalho os spokes, para acesso recursos no local e na Internet
-   Uma infraestrutura PKI, para implementar o início de sessão único nas cargas de trabalho
-   Controlo de fluxo (TCP/UDP) entre os spokes e a Internet
-   Controlo de fluxo entre o spoke e no local
-   Se assim o desejar, fluxo de controle entre um spoke e outro

O vDC reduz o custo geral ao utilizar a infraestrutura de hub partilhado entre vários spokes.

A função de cada spoke pode ser tipos diferentes de anfitriões de cargas de trabalho. Os spokes também podem fornecer uma abordagem modular para implementações repetíveis (por exemplo, desenvolvimento e teste, teste de aceitação do usuário, pré-produção e produção) das cargas de trabalho mesmo. Os spokes também podem ser utilizados para separar e ativar a diferentes grupos na sua organização (por exemplo, grupos de DevOps). Dentro de um spoke, é possível implementar uma carga de trabalho básica ou cargas de trabalho de vários escalões complexas com controlo de tráfego entre as camadas.

##### <a name="subscription-limits-and-multiple-hubs"></a>Limites de subscrição e vários hubs
No Azure, cada componente, seja qual for o tipo, é implementado numa subscrição do Azure. O isolamento de componentes do Azure em diferentes subscrições do Azure pode atender os requisitos de LOBs diferentes, como a definição de cópia de segurança diferenciadas níveis de acesso e autorização.

Um único vDC pode aumentar verticalmente ao grande número de spokes, embora, assim como acontece com todos os sistemas IT, existem limites de plataformas. A implementação de hub está vinculada a uma subscrição do Azure específica, que tem restrições e limites (por exemplo, um número máximo de VNet peerings - consulte [subscrição do Azure e limites do serviço, quotas e restrições] [ Limits] para obter detalhes). Em casos em que limites podem ser um problema, a arquitetura pode aumentar ainda mais ao expandir o modelo de um único hub-spokes para um cluster de hub- and -spoke. Vários hubs num ou mais regiões do Azure podem ser interligados com o VNet Peering, ExpressRoute ou VPN de site a site.

[![2]][2]

A introdução de vários hubs aumenta o esforço de custo e a gestão do sistema e só teria de ser justificada por escalabilidade (exemplos: limites de sistema ou redundância) e replicação regional (exemplos: recuperação pelo utilizador final de desempenho ou um desastre). Em cenários que requerem vários hubs, todos os hubs deve se esforçar para oferecer o mesmo conjunto de serviços para facilitar a operacional.

##### <a name="interconnection-between-spokes"></a>Interligação entre spokes
Dentro de um spoke único, é possível implementar cargas de trabalho de vários escalões complexas. Configurações de várias camadas podem ser implementadas usando sub-redes (um para cada camada) na mesma VNet e filtrar fluxos com NSGs.

Por outro lado, um arquiteto pode desejar implantar uma carga de trabalho de várias camada em várias VNets. Spokes a utilizar o VNet peering, podem ligar a outros spokes no mesmo hub ou hubs diferentes. Um exemplo típico desse cenário é o caso em que servidores de processamento de aplicação estão num spoke (VNet), enquanto a base de dados é implementado num spoke diferente (VNet). Neste caso, é fácil interligar os spokes com o peering de VNet e, deste modo, evitar trânsito através do hub. Um exame cuidadoso de arquitetura e a segurança deve ser efetuado para se certificar de que a ignorar o hub não ignorar ou pontos que talvez só existam no hub de auditoria de segurança importante.

[![3]][3]

Também podem ser interligados spokes para um spoke que age como um concentrador. Essa abordagem cria uma hierarquia de dois níveis: spoke no nível mais elevado (nível 0) se tornar o hub de spokes inferior (nível 1) da hierarquia. Tem dos spokes de vDC reencaminhar o tráfego para o hub central para entrar em contacto com a rede no local ou a internet. Uma arquitetura com dois níveis de hub introduz o encaminhamento complexo que remove os benefícios de uma relação simples hub-and-spoke.

Embora o Azure permite topologias complexas, um dos princípios fundamentais do conceito de vDC é capacidade de repetição e simplicidade. Para minimizar o esforço de gestão, o design simples hub-and-spoke é a arquitetura de referência de vDC recomendada.

### <a name="components"></a>Componentes
Centro de dados Virtual é constituído por quatro tipos de componentes básicos: **infraestrutura**, **redes de perímetro**, **cargas de trabalho**, e **monitorização**.

Cada tipo de componente é constituído por vários recursos e funcionalidades do Azure. Sua vDC é constituído por instâncias de vários tipos de componentes e às muitas variações do mesmo tipo de componente. Por exemplo, pode ter várias instâncias de carga de trabalho diferentes, logicamente separados, que representam diferentes aplicações. Utilize estes tipos de diferentes componentes e as instâncias para, por fim, crie o vDC.

[![4]][4]

A arquitetura de alto nível anterior de uma vDC mostra os tipos de componentes diferentes usados em diferentes horários da topologia hub-spokes. O diagrama mostra os componentes de infraestrutura em várias partes da arquitetura.

Como um acesso de uma prática recomendada (para um controlador de domínio no local ou vDC) direitos e privilégios devem ser baseada em grupo. Lidando com os grupos, em vez de utilizadores individuais ajuda a manter as políticas de acesso consistente entre as equipes e ajuda a minimizar erros de configuração. Atribuir e remover utilizadores de grupos adequados e ajuda a manter os privilégios de um utilizador específico atualizadas.

Cada grupo de função deve ter um prefixo exclusivo nos respetivos nomes que facilita a identificar que grupo está associado com a carga de trabalho. Por exemplo, uma carga de trabalho que aloja um serviço de autenticação pode ter grupos com o nome *AuthServiceNetOps, AuthServiceSecOps, AuthServiceDevOps e AuthServiceInfraOps.* Da mesma forma para centralizada de funções, ou funções não relacionado com um serviço específico, poderia ser precedido pela "Corp", *CorpNetOps* por exemplo.

Muitas organizações usam uma variação dos seguintes grupos para fornecer uma divisão de principais de funções:

-   O *grupo de TI central (Corp)* tem os direitos de propriedade para controlar os componentes de infraestrutura (por exemplo, rede e segurança) e, por conseguinte, tem de ter a função de Contribuidor na subscrição (e têm controle do hub) e direitos de contribuinte de rede em que os spokes. Organização de grande porte freqüentemente divididas essas responsabilidades de gestão entre várias equipes, tais como; um grupo de operações de rede (CorpNetOps) (com o foco exclusivo no sistema de rede) e um grupo de operações de segurança (CorpSecOps) (responsável pela política de segurança e de firewall). Nesse caso específico, dois grupos diferentes tem de ser criado para a atribuição de uma destas funções personalizadas.
-   O *dev & grupo de teste (AppDevOps)* tem a responsabilidade de implementar cargas de trabalho (aplicações ou serviços). Este grupo tem a função de contribuinte de Máquina Virtual para implementações de IaaS e/ou um ou funções mais contribuinte de PaaS (consulte [funções incorporadas para controlo de acesso][Roles]). Opcionalmente, a equipe de desenvolvimento e teste poderá ter de ter visibilidade sobre as políticas de segurança (NSGs) e políticas de encaminhamento (UDR) dentro de hub ou de um spoke específico. Por conseguinte, além das funções de Contribuidor para cargas de trabalho, este grupo também terá da função do leitor de rede.
-   O *grupo de funcionamento e manutenção (CorpInfraOps ou AppInfraOps)* tem a responsabilidade de gerenciar as cargas de trabalho em produção. Este grupo tem de ser um colaborador de subscrição nas cargas de trabalho em quaisquer subscrições de produção. Algumas organizações também podem avaliar se precisarem de um grupo de equipe da suporte adicional de escalonamento, com a função de contribuinte da subscrição em produção e na subscrição central hub, para corrigir potenciais problemas de configuração de produção ambiente.

Uma vDC é estruturado de modo a que grupos criados para os grupos de TI centrais, gerir o hub tem grupos correspondentes ao nível da carga de trabalho. Além da gestão de recursos do hub, apenas os grupos de TI centrais seria capazes de controlar o acesso externo e permissões de nível superior na subscrição. No entanto, os grupos de carga de trabalho seria capazes de controlar os recursos e as permissões da sua VNet independentemente na Central de TI.

O vDC tem de ser particionados de forma segura alojar vários projetos em diferentes linha-de-empresas (LOBs). Todos os projetos requerem diferentes ambientes isolados (desenvolvimento, UAT, produção). As subscrições do Azure separadas para cada um desses ambientes fornecem isolamento natural.

[![5]][5]

O diagrama anterior mostra a relação entre projetos de uma organização, os utilizadores, grupos e os ambientes em que os componentes do Azure são implementados.

Normalmente no IT, um ambiente (ou camada) é um sistema em que vários aplicativos são implantados e executados. As grandes empresas utilizam um ambiente de desenvolvimento (em que as alterações originalmente efetuadas e testadas) e um ambiente de produção (o que os utilizadores finais utilizam). Esses ambientes estão separados, muitas vezes, com vários ambientes entre-lhe para permitir a implementação faseada (implementação) de teste, teste e reversão em caso de problemas. Arquiteturas de implementação variam significativamente, mas, normalmente, o processo básico de começando no desenvolvimento (DEV) e terminando em produção (PROD) ainda é seguido.

Consiste numa arquitetura comum para estes tipos de ambientes de várias camadas de DevOps (desenvolvimento e teste), UAT (teste) e ambientes de produção. As organizações podem tirar partido de um ou vários inquilinos do Azure AD para definir direitos a estes ambientes e o acesso. O diagrama anterior mostra um caso em que dois diferentes inquilinos do Azure AD são utilizados: um para DevOps e UAT e outro exclusivamente para produção.

A presença do Azure AD diferentes inquilinos impõe a separação entre ambientes. O mesmo grupo de utilizadores (como um exemplo, TI Central) tem de autenticar com um URI para aceder a um inquilino diferente do AD modificar as funções ou permissões de ambientes de DevOps ou de produção de um projeto. A presença de autenticação de utilizador diferente para aceder a diferentes ambientes reduz as possíveis falhas e outros problemas causados por erros humanos.

#### <a name="component-type-infrastructure"></a>Tipo de componente: infraestrutura
Este tipo de componente é onde reside a maior parte da infraestrutura de suporte. Também é onde sua centralizado IT, segurança, e/ou das equipas de conformidade passam a maior parte do tempo.

[![6]][6]

Componentes de infra-estrutura fornecem uma interligação entre os diferentes componentes de uma vDC e estão presentes no hub e os spokes. A responsabilidade de gerir e manter os componentes de infraestrutura, normalmente, é atribuída ao central IT e/ou da equipe de segurança.

Uma das principais tarefas que a equipe de infraestrutura de TI é garantir a consistência dos esquemas de endereço IP em toda a empresa. Espaço atribuído para as necessidades de vDC para ser consistente e não sobrepostas com endereços IP privados atribuídos nas suas redes no local de endereços de IP privado.

Embora NAT os routers de limite no local ou em ambientes do Azure pode evitar conflitos de endereços IP, ele adiciona complicações para os componentes da infraestrutura. A simplicidade de gestão é um dos principais objetivos de vDC, para que usar o NAT para tratar preocupações IP não é uma solução recomendada.

Componentes da infraestrutura contenham a seguinte funcionalidade:

-   [**Serviços de identidade e diretório**][AAD]. Acesso a todos os tipos de recursos no Azure é controlado por uma identidade armazenada num serviço de diretório. O serviço de diretório armazena não apenas a lista de utilizadores, mas também os direitos de acesso aos recursos numa subscrição do Azure específica. Estes serviços podem existir apenas na cloud ou podem ser sincronizadas com a identidade no local armazenada no Active Directory.
-   [**Rede virtual**][VPN]. Redes virtuais são um dos principais componentes de uma vDC e permitem-lhe criar um limite de isolamento do tráfego na plataforma do Azure. Uma rede Virtual é composta por um único ou vários segmentos de rede virtual, cada um com um prefixo de rede IP específico (uma sub-rede). A rede Virtual define uma área de perímetro interna em que máquinas de virtuais de IaaS e serviços de PaaS podem estabelecer comunicações privadas. VMs (e serviços de PaaS) em uma rede virtual não pode comunicar diretamente com VMs (e serviços de PaaS) numa rede virtual diferente, mesmo se ambas as redes virtuais são criadas pelo mesmo cliente, na mesma subscrição. Isolamento é uma propriedade crítica que garante a VMs de cliente e comunicação são mantidos privada numa rede virtual.
-   [**UDR**][UDR]. O tráfego numa rede Virtual é encaminhado por predefinição, com base na tabela de encaminhamento do sistema. Uma rota de usuário definir é uma tabela de encaminhamento personalizada que os administradores de rede pode associar a um ou mais sub-redes para substituir o comportamento da tabela de encaminhamento do sistema e definir um caminho de comunicação dentro de uma rede virtual. A presença de UDRs garante que o tráfego de saída do trânsito do spoke através de VMs personalizadas específicas e/ou dispositivos de rede Virtual e Balanceadores de carga presente no hub e, em que os spokes.
-   [**NSG**][NSG]. Um grupo de segurança de rede é uma lista de regras de segurança que atuam como tráfego de filtragem nas portas de origens de IP, IP de destino, protocolos, portas de origem de IP e IP de destino. O NSG pode ser aplicado a uma sub-rede, um cartão de Virtual NIC associado uma VM do Azure, ou ambos. Os NSGs são essenciais para implementar um controle de fluxo correto no hub e, em que os spokes. O nível de segurança que aproveita o NSG é uma função de que portas abrir e com que finalidade. Os clientes devem aplicar filtros adicionais por VM com firewalls baseados em host como IPtables ou a Firewall do Windows.
-   [**DNS**][DNS]. A resolução de nomes de recursos em VNets de uma vDC é fornecida através de DNS. O Azure fornece serviços DNS para os dois [pública][DNS] e [privada] [ PrivateDNS] resolução de nomes. Zonas privadas fornecem resolução de nomes dentro de uma rede virtual e entre redes virtuais. Pode ter zonas privadas do span não apenas entre, redes virtuais na mesma região, mas também em regiões e subscrições. Para a resolução de pública, o DNS do Azure fornece um serviço de alojamento dos domínios DNS que fornece resolução de nomes usando a infra-estrutura do Microsoft Azure. Ao alojar os seus domínios no Azure, pode gerir os recursos DNS com as mesmas credenciais, APIs, ferramentas e faturação dos seus outros serviços do Azure.
-   [**Subscrição** ] [ SubMgmt] e [ **gestão de grupo de recursos**][RGMgmt]. Uma subscrição define um limite de natural para criar vários grupos de recursos no Azure. Recursos numa subscrição são montados juntas em contentores lógicos com o nome grupos de recursos. O grupo de recursos representa um grupo lógico para organizar os recursos de uma vDC.
-   [**RBAC**][RBAC]. Através do RBAC, é possível função organizacional do mapa, juntamente com direitos de acesso a recursos específicos do Azure, permitindo-lhe restringir os utilizadores para apenas um determinado subconjunto de ações. Com o RBAC, pode conceder acesso ao atribuir a função adequada para os utilizadores, grupos e aplicações dentro do escopo relevante. O âmbito de uma atribuição de função pode ser uma subscrição do Azure, um grupo de recursos ou um único recurso. RBAC permite que a herança de permissões. As funções atribuídas a um âmbito principal também concede acesso para os filhos que ele contém. Utilizando o RBAC, pode segregar funções e conceder apenas a quantidade de acesso a utilizadores que precisam para desempenhar as suas funções. Por exemplo, utilize o RBAC para permitir que um funcionário gerir máquinas virtuais numa subscrição, enquanto outro pode gerir bds SQL dentro da mesma subscrição.
-   [**O VNet Peering**][VNetPeering]. A funcionalidade fundamental utilizada para criar a infraestrutura de uma vDC é o VNet Peering, um mecanismo que liga duas redes virtuais (VNets) na mesma região através da rede de centro de dados do Azure ou através do backbone do Azure em todo o mundo em várias regiões.

#### <a name="component-type-perimeter-networks"></a>Tipo de componente: Redes de perímetro
[Rede de perímetro] [ DMZ] componentes (também conhecido como uma rede de rede de Perímetro) permitem fornecer conectividade de rede com a sua no local ou redes de centros de dados físicos, juntamente com qualquer conectividade e para a Internet. Também é onde suas equipes de segurança de rede e provavelmente passam a maior parte do tempo.

Pacotes de entrada devem passar pelos dispositivos de segurança no hub, como o firewall, IDS e IPS, antes de atingir os servidores de back-end nos spokes. Pacotes de vinculado à Internet das cargas de trabalho também devem passar pelos dispositivos de segurança na rede de perímetro para imposição de políticas, inspeção e fins de auditoria, antes de deixarem a rede.

Componentes de rede de perímetro fornecem as seguintes funcionalidades:

-   [Redes virtuais][VNet], [UDR][UDR], [NSG][NSG]
-   [Aplicação de rede Virtual][NVA]
-   [Balanceador de carga][ALB]
-   [Gateway de aplicação][AppGW] / [WAF][WAF]
-   [IPs públicos][PIP]

Normalmente, a central IT e equipes de segurança tem a responsabilidade de definição de requisitos e as operações das redes de perímetro.

[![7]][7]

O diagrama anterior mostra a imposição dos dois perímetros com acesso à internet e uma rede no local, ambos residente no hub. Num único hub, rede de perímetro para internet pode aumentar verticalmente para suportar um grande número de LOBs, usando vários farms de Firewalls de aplicações Web (WAFs) e/ou firewalls.

[**Redes virtuais** ] [ VNet] o hub é normalmente criado numa VNet com várias sub-redes para alojar o tipo de diferente dos serviços de filtragem e inspecionar o tráfego de ou à internet através de NVAs, WAFs e o Azure Gateways de aplicação.

[**UDR** ] [ UDR] usando UDR, os clientes podem implementar firewalls, IDS/IPS e outras aplicações virtuais e encaminhar o tráfego de rede através destas aplicações de segurança para a imposição de política de limite de segurança, auditoria e inspeção. Podem ser criadas as UDRs do hub e spokes para garantir que o tráfego transits pelas VMs personalizadas específicas, dispositivos de rede Virtual e Balanceadores de carga utilizados pelo vDC. Para garantir que o tráfego gerado a partir de VMs residentes em trânsito do spoke para as aplicações virtuais corretas, um UDR tem de ser definido nas sub-redes de spoke ao definir o endereço IP Front-end do Balanceador de carga interno como o next-hop. O Balanceador de carga interno distribui o tráfego interno para as aplicações virtuais (conjunto de back-end de Balanceador de carga).

[![8]][8]

[**Aplicações virtuais de rede** ] [ NVA] no hub, a rede de perímetro com acesso à internet normalmente é gerenciada através de um farm de firewalls e/ou Firewalls de aplicações Web (WAFs).

LOBs diferentes normalmente usam muitos aplicativos web, e esses aplicativos tendem a ser prejudicadas várias vulnerabilidades e explorações potenciais. Firewalls de aplicações Web são um tipo especial de produto utilizado para detetar ataques contra aplicações web (HTTP/HTTPS) em mais detalhes que um firewall genérico. Em comparação com a tecnologia de firewall tradição, WAFs tem um conjunto de funcionalidades específicas a proteger os servidores web interno de ameaças.

Um farm de firewall é o grupo de trabalhar em tandem sob a mesma administração comuns, com um conjunto de regras de segurança para proteger as cargas de trabalho alojadas nos spokes, de firewalls e controlar o acesso a redes no local. Um farm de firewall tem menos especializada em comparação com uma WAF de software, mas tem um âmbito de abrangente de aplicações para filtrar e inspecionar qualquer tipo de tráfego de saída e entrada. Farms de servidores de firewall são normalmente implementados no Azure através de aplicações virtuais de rede (NVAs), que estão disponíveis no Azure marketplace.

É recomendado utilizar um conjunto de NVAs para o tráfego com origem na Internet, e outro para o tráfego com origem no local. Utilizar apenas um conjunto de NVAs para ambos é um risco de segurança, pois fornece não perímetro de segurança entre os dois conjuntos de tráfego de rede. Utilização de NVAs separadas reduz a complexidade das regras de segurança de verificação e faz com que as regras que correspondem à qual solicitação de rede de entrada.

As empresas a maior parte das grandes gerir vários domínios. O DNS do Azure pode ser utilizado para alojar os registos DNS para um determinado domínio. Como exemplo, o endereço de IP Virtual (VIP) do Balanceador de carga externo do Azure (ou os WAFs) pode ser registrado o registo de um registo de DNS do Azure.

[**O Azure Load Balancer** ] [ ALB] Balanceador de carga do Azure oferece um serviço de camada 4 (TCP, UDP), que pode distribuir o tráfego de entrada entre instâncias de serviço definido num conjunto com balanceamento de carga de elevada disponibilidade. O tráfego enviado para o Balanceador de carga, a partir de pontos finais de front-end (pontos de extremidade IP públicos ou pontos finais IP privados) pode ser redistribuído com ou sem tradução de endereços para um conjunto de conjunto de endereços IP de back-end (exemplos que está a ser; Aplicações virtuais de rede ou VMs).

O estado de funcionamento as várias instâncias de servidor também podem testar o Balanceador de carga do Azure e, quando uma sonda não consegue responder o Balanceador de carga para a enviar tráfego para a instância de mau estado de funcionamento. Uma vDC, temos a presença de um balanceador de carga externo no hub (por exemplo, a balancear o tráfego para as NVAs) e em que os spokes (para realizar tarefas como balanceamento de tráfego entre VMs diferentes de um aplicativo de várias camada).

[**Gateway de aplicação** ] [ AppGW] Gateway de aplicação do Microsoft Azure é uma aplicação virtual dedicada, fornecendo o controlador de entrega de aplicações (ADC) como um serviço, oferecendo vários de camada 7 balanceamento de carga recursos para a sua aplicação. Permite-lhe otimizem a produtividade do web farm ao descarregar a terminação SSL com utilização intensiva da CPU para o gateway de aplicação. Proporciona também outras capacidades de encaminhamento de camada 7, incluindo a distribuição round robin de tráfego de entrada, a afinidade de sessão com base em cookies, o encaminhamento baseado no caminho do URL e a capacidade de alojar vários Web sites atrás de um Gateway de Aplicação individual. Também é fornecida uma firewall de aplicações Web (WAF) como parte do SKU da WAF do gateway de aplicação. Este SKU proporciona proteção às aplicações web do web vulnerabilidades e exploits comuns. O Gateway de Aplicação pode ser configurado como um gateway com acesso à Internet, gateway só interno ou uma combinação de ambos. 

[**IPs públicos** ] [ PIP] funcionalidades alguns Azure permitem-lhe associar pontos finais de serviço para um endereço IP público que permite ao seu recurso para ser acedido a partir da internet. Este ponto final utiliza a tradução de endereços de rede (NAT) para encaminhar o tráfego para o endereço interno e a porta na rede virtual do Azure. Este caminho é o modo principal para tráfego externo para passar para a rede virtual. Os endereços de IP público podem ser configurados para determinar qual o tráfego é passado e como e onde é traduzido para a rede virtual.

#### <a name="component-type-monitoring"></a>Tipo de componente: monitorização
Componentes de monitorização fornecem visibilidade e os alertas de todos os outros tipos de componentes. Todas as equipes devem ter acesso a monitorização para os componentes e serviços aos quais têm acesso. Se tiver equipas de operações ou suporte técnico de ajuda centralizado, eles precisariam de tiver integrado o acesso aos dados fornecidos por esses componentes.

Recursos alojados do Azure oferece diferentes tipos de registo e monitorização de serviços para controlar o comportamento do Azure. Governação e controle de cargas de trabalho no Azure é com base não apenas nos recolher dados de registo, mas também a capacidade de realizar ações com base em eventos específicos de comunicadas.

[**O Azure Monitor** ] [ Monitor] -Azure inclui vários serviços que executar individualmente uma tarefa ou função específica no espaço de monitorização. Em conjunto, estes serviços fornecem uma solução abrangente para recolher, analisar e atuar na telemetria da aplicação e dos recursos do Azure que a suportam. Podem também funcionar para monitorizar recursos críticos no local e fornecer um ambiente de monitorização híbrido. Compreender as ferramentas e os dados que estão disponíveis é o primeiro passo para o desenvolvimento de uma estratégia completa de monitorização para a sua aplicação.

Existem dois tipos principais de registos no Azure:

-   [**Registos de atividades** ] [ ActLog] (conhecido também como "Registo operacional") fornecem informações sobre as operações executadas nos recursos na subscrição do Azure. Estes registos comunicam os eventos de plano de controlo para as suas subscrições. Todos os recursos do Azure produz os registos de auditoria.

-   [**Registos de diagnóstico do Azure** ] [ DiagLog] são registos gerados por um recurso que fornecem dados avançados e frequentes sobre o funcionamento desse recurso. O conteúdo estes registos varia consoante o tipo de recurso.

[![9]][9]

Numa vDC, é extremamente importante controlar os registos de NSGs, particularmente estas informações:

-   [**Registos de eventos**][NSGLog]: fornece informações sobre quais regras do NSG são aplicadas a VMs e funções de instância com base no endereço MAC.
-   [**Registos do contador**][NSGLog]: controla quantas vezes cada regra NSG foi executada para negar ou permitir o tráfego.

Todos os registos podem ser armazenados em contas de armazenamento do Azure para fins de cópia de segurança, de análise estática ou de auditoria. Quando os registos são armazenados numa conta de armazenamento do Azure, os clientes podem utilizar diferentes tipos de estruturas para recuperar, preparação, analisar e visualizar estes dados para comunicar o estado de funcionamento dos recursos na nuvem.

As grandes empresas já devem ter adquirido uma estrutura padrão para a monitorização de sistemas no local e podem expandir essa estrutura para integrar registos gerados pelo implementações na cloud. Para organizações que pretendem manter todo o registo na cloud, [Log Analytics] [... / log-analytics/log-analytics-overview. md] é uma ótima opção. Uma vez que o Log Analytics é implementado como um serviço baseado na nuvem, pode tê-lo em funcionamento rapidamente com um investimento mínimo em serviços de infraestrutura. O log Analytics também pode integrar com componentes do System Center, como o System Center Operations Manager para expandir os investimentos de gestão existentes para a cloud.

Log Analytics é um serviço do Azure que ajuda a recolher, correlacionar, pesquisa e atuar sobre os dados de registo e desempenho gerados por sistemas operacionais, aplicativos e componentes da infraestrutura na cloud. Ele oferece aos clientes informações operacionais em tempo real ao utilizar pesquisa integrada e dashboards personalizados para analisar todos os registros de todas as suas cargas de trabalho numa vDC.

O [Monitor de desempenho de rede (NPM)] [ NPM] solução dentro do OMS pode fornecer a rede detalhadas informações ponto-a-ponto, incluindo uma vista única de suas redes do Azure e a redes no local. Com monitores específicos para o ExpressRoute e de serviços públicos.

#### <a name="component-type-workloads"></a>Tipo de componente: cargas de trabalho
Componentes de carga de trabalho são onde residem os seus serviços e aplicações reais. Também é onde as equipes de desenvolvimento de aplicativos passam a maior parte do tempo.

As possibilidades de carga de trabalho são verdadeiramente infinitas. Seguem-se apenas alguns dos tipos de carga de trabalho possíveis:

**Aplicações LOB internas**

Os aplicativos de linha de negócio são aplicações para computadores críticas para a operação em curso de uma empresa. Aplicativos de LOB têm algumas características comuns:

-   **Interativo**. Os aplicativos de LOB são interativos por natureza: dados são introduzidos e resultado/relatórios são devolvidos.
-   **Dados orientados pela**. Os aplicativos de LOB são intensivos com acesso freqüente para as bases de dados ou outro armazenamento de dados.
-   **Integrada**. LOB do departamento de integração de oferta de aplicações com outros sistemas dentro ou fora da organização.

**Sites da web (para a Internet ou interna) do cliente** a maioria dos aplicativos que interagem com a Internet são web sites. O Azure oferece a capacidade de executar um web site numa VM de IaaS ou a partir de um [aplicações Web do Azure] [ WebApps] site (PaaS). Aplicações Web do Azure suporta a integração com VNets que permitem a implementação das aplicações Web no spoke de uma vDC. Ao examinar com acesso à web sites internos, com a integração de VNET, não precisa de expor um ponto de extremidade de Internet para as suas aplicações, mas pode utilizar os recursos através de não-internet encaminháveis endereços privados da sua VNet privada em vez disso.

**Dados/análise de macrodados** quando dados precisam de aumentar verticalmente para um volume muito grande, bases de dados podem não aumentar verticalmente corretamente. Tecnologia do Hadoop oferece um sistema para executar consultas distribuídas em paralelo em grande número de nós. Os clientes têm a opção para executar cargas de trabalho de dados em VMs de IaaS ou PaaS ([HDInsight][HDI]). HDInsight oferece suporte à implantação numa VNet com base na localização, pode ser implementado num cluster num spoke do vDC.

**Eventos e Messaging**
[Event Hubs do Azure] [ EventHubs] é um serviço de ingestão de telemetria de hiperescala que recolhe, transforma e armazena milhões de eventos. Enquanto plataforma de transmissão em fluxo distribuída, oferece baixa latência e tempo de retenção configurável, permitindo-lhe ingerir grandes quantidades de telemetria para o Azure e ler esses dados de várias aplicações. Com os Hubs de eventos, um único fluxo pode suportar pipelines em tempo real e com base no batch.

Uma cloud altamente fiável de mensagens serviço entre aplicativos e serviços, pode ser implementada através de [do Azure Service Bus] [ ServiceBus] que ofertas assíncronas mensagens mediadas entre cliente e servidor, juntamente com estruturado de mensagens first-in-first-out (FIFO) e capacidades de publicação/subscrição.

[![10]][10]

### <a name="multiple-vdc"></a>Vários vDC
Até agora, este artigo tem se concentrado num único vDC, que descreve os componentes básicos e a arquitetura que contribuem para uma vDC resiliente. Funcionalidades do Azure, como os conjuntos de disponibilidade de NVAs, Balanceador de carga do Azure, conjuntos de dimensionamento, juntamente com outros mecanismos de contribuem para um sistema que permitem que crie sólidos dos níveis do SLA para os seus serviços de produção.

No entanto, um único vDC está alojado numa única região e é vulnerável a indisponibilidade principais que pode afetar essa região inteira. Clientes que pretendem conseguir SLAs elevadas necessário proteger os serviços através de implementações do mesmo projeto no vDCs dois (ou mais), colocados em regiões diferentes.

Além das preocupações de SLA, existem vários cenários comuns onde implementar vários vDCs faz sentido:

-   Presença de regionais/Global
-   Recuperação Após Desastre
-   Mecanismo para desviar o tráfego entre o controlador de domínio

#### <a name="regionalglobal-presence"></a>Presença de regionais/Global
Centros de dados do Azure estão presentes em várias regiões em todo o mundo. Ao selecionar vários centros de dados do Azure, os clientes têm de considerar dois fatores relacionados com: distâncias geográficas e a latência. Os clientes têm de avaliar a distância geográfica entre o vDCs e a distância entre o vDC e os usuários finais, para oferecer a melhor experiência de utilizador.

A região do Azure onde estão alojadas vDCs também tem de estar em conformidade com requisitos regulamentares estabelecidos por qualquer jurisdição fiscal legal sob a qual opera sua organização.

#### <a name="disaster-recovery"></a>Recuperação Após Desastre
A implementação de um plano de recuperação após desastre altamente está relacionado com o tipo de carga de trabalho preocupado e a capacidade de sincronizar o estado de carga de trabalho entre vDCs diferentes. O ideal é que a maioria dos clientes pretende sincronizar os dados de aplicativo entre implementações em execução em dois vDCs diferentes para implementar um mecanismo de rápida ativação pós-falha. A maioria dos aplicativos são sensíveis a latência e quais podem provocar potenciais tempo limite e o atraso de sincronização de dados.

Sincronização ou monitorização heartbeat de aplicativos em diferentes vDCs requer comunicação entre eles. Dois vDCs em regiões diferentes podem ser ligadas através de:

-   VNet Peering - Peering de VNet pode ligar hubs em várias regiões
-   Quando os hubs de vDC estão ligados ao mesmo circuito do ExpressRoute de peering privado do ExpressRoute
-   vários circuitos do ExpressRoute ligados através de seu backbones corporativos e a malha de vDC ligado os circuitos do ExpressRoute
-   Ligações de VPN de site a Site entre os hubs de vDC em cada região do Azure

Normalmente, as ligações de Peering de VNet ou ExpressRoute são o mecanismo preferencial para conclusão maior largura de banda e latência consistente quando trânsito através da estrutura principal do Microsoft.

Não há nenhuma mágica receita para validar uma aplicação distribuída entre dois (ou mais) vDCs diferentes localizados em regiões diferentes. Os clientes devem executar testes de qualificação de rede para verificar a latência e largura de banda de conexões e de destino, se a replicação de dados síncrona ou assíncrona é apropriada e o que pode ser o objetivo de tempo ideal de recuperação (RTO) das cargas de trabalho.

#### <a name="mechanism-to-divert-traffic-between-dc"></a>Mecanismo para desviar o tráfego entre o controlador de domínio
Uma técnica eficaz para desviar de entrada de tráfego num DC para outro baseia-se no DNS. [O Gestor de tráfego do Azure] [ TM] usa o mecanismo de sistema de nomes de domínio (DNS) para direcionar o tráfego de utilizador final para o ponto de final público mais adequado numa vDC específico. Através de sondas, Gestor de tráfego verifica periodicamente o estado de funcionamento do serviço de pontos finais públicos no vDCs diferentes e, em caso de falha desses pontos de extremidade, encaminha automaticamente para o secundário vDC.

O Gestor de tráfego funciona em pontos finais públicos do Azure e pode ser usado, por exemplo, para controlo/desviar tráfego para VMs do Azure e as aplicações Web no vDC apropriado. Gestor de tráfego é resiliente mesmo diante de uma falha de região do Azure inteira e pode controlar a distribuição de tráfego de utilizador para pontos finais de serviço no vDCs diferentes com base em vários critérios (por exemplo, falha de um serviço numa vDC específico, ou ao selecionar o vDC com a menor latência de rede para o cliente).

### <a name="conclusion"></a>Conclusão
O Centro de dados do Virtual é uma abordagem à migração de centro de dados para a cloud que utiliza uma combinação de funcionalidades e capacidades para criar uma arquitetura dimensionável no Azure que maximiza a utilização de recursos na cloud, reduzindo os custos e simplifica a governação de sistema. O conceito de vDC se baseia numa topologia hub-spokes, fornecendo comuns serviços partilhados no hub e permitindo aplicações/cargas de trabalho específicas em que os spokes. Uma vDC corresponde a estrutura das funções da empresa, onde diferentes departamentos (Central de TI, DevOps, operação e manutenção) funcionam em conjunto, cada um com uma lista específica de funções e tarefas. Uma vDC satisfaz os requisitos para uma migração "Lift and Shift", mas também oferece muitas vantagens em implementações de cloud nativa.

## <a name="references"></a>Referências
As seguintes funcionalidades foram abordadas neste documento. Clique nas hiperligações para saber mais.

| | | |
|-|-|-|
|Recursos de rede|Balanceamento de Carga|Conectividade|
|[Redes virtuais do Azure][VNet]</br>[Grupos de segurança de rede][NSG]</br>[Registos de NSG][NSGLog]</br>[Encaminhamento definido pelo utilizador][UDR]</br>[Aplicações virtuais de rede][NVA]</br>[Endereços IP públicos][PIP]</br>[DNS]|[Balanceador de carga do Azure (L3) ][ALB]</br>[Gateway de aplicação (L7) ][AppGW]</br>[Firewall de aplicações Web][WAF]</br>[Gestor de tráfego do Azure][TM] |[O VNet Peering][VNetPeering]</br>[Rede privada virtual][VPN]</br>[ExpressRoute][ExR]
|Identidade</br>|Monitorização</br>|Melhores práticas</br>|
|[O Azure Active Directory][AAD]</br>[Multi-factor Authentication][MFA]</br>[Controlos de acesso de Base de função][RBAC]</br>[Funções do AAD predefinido][Roles] |[O Azure Monitor][Monitor]</br>[Registos de atividades][ActLog]</br>[Registos de diagnóstico][DiagLog]</br>[Microsoft Operations Management Suite][OMS]</br>[Monitor de desempenho de rede][NPM]|[Melhores práticas de redes de perímetro][DMZ]</br>[Gestão de subscrições][SubMgmt]</br>[Gestão de grupo de recursos][RGMgmt]</br>[Limites de subscrição do Azure][Limits] |
|Outros serviços do Azure|
|[Aplicações Web do Azure][WebApps]</br>[HDInsights (Hadoop) ][HDI]</br>[Hubs de Eventos][EventHubs]</br>[Service Bus][ServiceBus]|



## <a name="next-steps"></a>Próximos Passos
 - Explore [VNet Peering][VNetPeering], a tecnologia de base para designs de vDC hub- and -spoke
 - Implemente [AAD] [ AAD] para obter uma introdução [RBAC] [ RBAC] exploração
 - Desenvolver um modelo de gestão de recursos e subscrição e o modelo de acordo com a estrutura, requisitos, de RBAC e as políticas da sua organização. A atividade mais importante está planejando. Tanto quanto prático, planeie reorganizations, fusões, novas linhas de produto, etc.

<!--Image References-->
[0]: ./media/networking-virtual-datacenter/redundant-equipment.png "exemplos de sobreposição de componente" 
[1]: ./media/networking-virtual-datacenter/vdc-high-level.png "exemplo de nível superior de vDC hub- and -spoke"
[2]: ./media/networking-virtual-datacenter/hub-spokes-cluster.png "cluster de hubs- and -spoke"
[3]: ./media/networking-virtual-datacenter/spoke-to-spoke.png "spoke-para-and-spoke"
[4]: ./media/networking-virtual-datacenter/vdc-block-level-diagram.png "diagrama de nível de bloco do vDC"
[5]: ./media/networking-virtual-datacenter/users-groups-subsciptions.png "utilizadores, grupos, subscrições e projetos"
[6]: ./media/networking-virtual-datacenter/infrastructure-high-level.png "diagrama de alto nível de infraestrutura"
[7]: ./media/networking-virtual-datacenter/highlevel-perimeter-networks.png "diagrama de alto nível de infraestrutura"
[8]: ./media/networking-virtual-datacenter/vnet-peering-perimeter-neworks.png "redes de perímetro e de VNet Peering"
[9]: ./media/networking-virtual-datacenter/high-level-diagram-monitoring.png "diagrama de alto nível para monitorização"
[10]: ./media/networking-virtual-datacenter/high-level-workloads.png "diagrama de alto nível para a carga de trabalho"

<!--Link References-->
[Limits]: https://docs.microsoft.com/azure/azure-subscription-service-limits
[Roles]: https://docs.microsoft.com/azure/role-based-access-control/built-in-roles
[VNet]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview
[NSG]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg
[DNS]: https://docs.microsoft.com/azure/dns/dns-overview
[PrivateDNS]: https://docs.microsoft.com/azure/dns/private-dns-overview
[VNetPeering]: https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview 
[UDR]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview 
[RBAC]: https://docs.microsoft.com/azure/role-based-access-control/overview
[MFA]: https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication
[AAD]: https://docs.microsoft.com/azure/active-directory/active-directory-whatis
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways 
[ExR]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction 
[NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[SubMgmt]: https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-subscription-governance 
[RGMgmt]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview
[DMZ]: https://docs.microsoft.com/azure/best-practices-network-security
[ALB]: https://docs.microsoft.com/azure/load-balancer/load-balancer-overview
[PIP]: https://docs.microsoft.com/azure/virtual-network/resource-groups-networking#public-ip-address
[AppGW]: https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[WAF]: https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview
[Monitor]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/
[ActLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs 
[DiagLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs
[NSGLog]: https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log
[OMS]: https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview
[NPM]: https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor
[WebApps]: https://docs.microsoft.com/azure/app-service/
[HDI]: https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-introduction
[EventHubs]: https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs 
[ServiceBus]: https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview
[TM]: https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview

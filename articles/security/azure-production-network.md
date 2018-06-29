---
title: Rede de produção do Azure
description: Este artigo fornece uma descrição geral da rede de produção do Microsoft Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 5c0bfae35464e73278a1efd9c04a03123bb9018a
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102487"
---
# <a name="azure-production-network"></a>Rede de produção do Azure
Os utilizadores de rede de produção do Azure incluem clientes externos ao aceder às suas próprias aplicações do Microsoft Azure, bem como os técnicos de suporte de Microsoft Azure internos que gerir a rede de produção. Os métodos de acesso de segurança e mecanismos de proteção para estabelecer ligações à rede de produção do Azure são abordados neste artigo.

## <a name="internet-routing-and-fault-tolerance"></a>Encaminhamento de Internet e tolerância a falhas
Fornecer uma infraestrutura de serviço de nome (WADNS) do Microsoft Azure domínio de internos e externos globalmente redundante juntamente com vários clusters de servidores de serviço de nomes de domínio (DNS) de principal e secundário para tolerância a falhas ao rede adicional do Microsoft Azure os controlos da segurança, tais como NetScaler são utilizados para impedir distribuídas ataques Denial of Service (DDoS distribuídos) e proteger a integridade dos serviços de DNS do Microsoft Azure.

Os servidores WADNS estão localizados em várias instalações do Centro de dados. A implementação de WADNS incorpora uma hierarquia de servidores DNS secundário/primária publicamente resolver nomes de domínio do cliente do Azure. Os nomes de domínio, normalmente, resolver para um endereço de CloudApp.net que encapsula num wrapper o endereço IP Virtual (VIP) para o serviço do cliente. Exclusivo para o Azure, o VIP correspondente ao endereço IP dedicado (DIP) interno da tradução inquilino é feito por responsável por essa VIP de balanceadores de carga Microsoft.

Azure está alojado nos centros de dados do Azure distribuídos geograficamente nos EUA e baseia-se em plataformas de encaminhamento de estado da última implementação robustas e dimensionáveis normas da arquitetura. Algumas das funcionalidades importantes são:

- Mudar de etiqueta multiprotocolo (MPLS) com base em fornecer uma utilização eficiente de ligação e a degradação correto do serviço se houver uma falha de engenharia de tráfego
- Redes são implementadas com "necessário e um" (N + 1) arquiteturas de redundância ou superior.
- Externamente, centros de dados são servidos pelo circuitos de rede dedicado, largura de banda alta redundantly ligar propriedades com mais 1,200 fornecedores de serviços de Internet global em vários pontos de peering fornecer que excedam gigabytes 2.000 por segundo (Gbps) do capacidade de limite.

Como Microsoft detém os seus próprios circuitos de rede entre centros de dados, estes atributos ajudam a oferta Azure alcançar 99.9 + % de disponibilidade de rede sem a necessidade de fornecedores de serviços de Internet de terceiros tradicionais.

## <a name="connection-to-production-network-and-associated-firewalls"></a>Ligação à rede de produção e Firewalls associadas
A política de fluxo de tráfego de Internet de rede do Azure direciona o tráfego para a rede de produção do Azure localizado no Centro de dados regional mais próximo no interior dos Estados Unidos. Uma vez que os centros de dados de produção do Azure mantém a arquitetura de rede consistente e hardware, o abaixo fluxo de tráfego descrição aplica-se consistente para todos os centros de dados.

Depois do tráfego de Internet para o Azure é encaminhado para o Centro de dados mais próximo, é estabelecida uma ligação para os routers de acesso. Estes routers acesso servem para isolar o tráfego entre nós do Azure e VMs com instâncias de cliente. Dispositivos de infraestrutura de rede nas localizações de acesso e limite são os pontos de limites em que são aplicados filtros de entrada e/ou de saída. Estes routers são configurados através de uma ACL em camadas para filtrar o tráfego de rede indesejado e aplicar limites de velocidade de tráfego, se necessário. O tráfego permitido por ACL é encaminhado para os balanceadores de carga. Routers de distribuição foram concebidos para permitir que apenas os endereços IP aprovado Microsoft, forneça o spoofing de antimalware e estabelecer ligações de TCP utilizar ACLs.

Dispositivos de balanceamento de carga externos atrás os routers de acesso para efetuar a tradução de endereços de rede (NAT) IPs encaminháveis para a Internet para IPs interno do Azure. Estes também encaminhar pacotes para válido IPs interno de produção e de portas e atuam como um mecanismo de proteção para limitar a exposição de espaço de endereços de rede de produção interno.

Por predefinição, o Microsoft impõe Hypertext Transfer Protocol Secure (HTTPS) para todo o tráfego que está a ser transmitido para o web do cliente browsers, incluindo a iniciar sessão e todo o tráfego após essa data. A utilização de TLS v1.2 permite que um túnel seguro para o tráfego através de. ACLs nos routers de núcleo de acesso e certifique-se que a origem do tráfego é consistente com o que é esperado.

Uma distinção importante nesta arquitetura em comparação comparada a arquitetura de segurança tradicionais é que existem não existem firewalls de hardware dedicado, intrusões especializadas deteção/prevenção dispositivos ou outras aplicações de segurança normalmente esperadas antes são feitas ligações ao ambiente de produção do Azure. Os clientes esperam, normalmente, estes dispositivos de firewall de hardware na rede do Azure; No entanto, existem nenhum utilizada no Azure. Quase exclusivamente, essas funcionalidades de segurança incorporadas no software com o ambiente do Azure para fornecer os mecanismos de segurança de vários em camadas robusta, incluindo as capacidades de firewall. Além disso, o âmbito do limite e sprawl associado de dispositivos de segurança críticas é mais fácil de gerir e inventário conforme mostrado na ilustração acima, porque é gerida pelo software do Azure a executar.

## <a name="core-security-and-firewall-features"></a>Funcionalidades de segurança e de firewall de núcleo
Azure implementa segurança software robusta e funcionalidades de firewall em vários níveis para impor as funcionalidades de segurança esperadas, normalmente, num ambiente tradicional para proteger o limite de autorização de segurança do núcleo.

### <a name="azure-security-features"></a>Funcionalidades de segurança do Azure
Azure implementa firewalls de software baseado em anfitrião dentro da rede de produção. Vários principais de segurança e funcionalidades de firewall residem dentro do núcleo do ambiente do Azure. Estas funcionalidades de segurança refletem uma estratégia de defesa em profundidade no ambiente do Azure. Dados de cliente no Microsoft Azure estão protegidos por firewalls seguintes:

**Firewall de hipervisor (pacote filtro)**: este firewall está implementado no hipervisor e configurado pelo agente de FC. Este firewall protege o inquilino no interior da VM contra acesso não autorizado. Por predefinição, quando é criada uma VM todo o tráfego é bloqueado e, em seguida, o agente FC adiciona exceções de regras/no filtro para permitir tráfego autorizado.

Existem duas categorias de regras que são programadas aqui:

- Regras de infraestrutura ou de configuração do computador: por predefinição, todas as comunicações está bloqueada. Existem exceções para permitir que uma VM enviar e receber informações de protocolo de configuração dinâmica de anfitrião (DHCP), as informações de DNS, enviar o tráfego à Internet "pública", saída para as outras VMs no cluster FC e servidor de ativação de SO. Uma vez que as máquinas virtuais permitidas a lista de saída destinos não inclui as sub-redes de router do Microsoft Azure e outras propriedades da Microsoft, as regras agirem como uma camada de defesa para os mesmos.
- Ficheiro de configuração de função: Define as ACLs de entrada com base no modelo de serviço dos inquilinos. Por exemplo, se um inquilino tem um front-end da web na porta 80 numa determinada VM, a porta 80 está aberta todos os endereços IP. Se a VM possui uma função de trabalho em execução, é aberta a função de trabalho apenas para a VM no mesmo inquilino.

**Firewall de anfitrião nativo**: armazenamento e de recursos de infraestrutura do Microsoft Azure executam num nativo, que tem não hipervisor e, por conseguinte, a Firewall do Windows está configurada com dois conjuntos de regras que se acima.

**Firewall de anfitrião**: A firewall do anfitrião protege partição do anfitrião, o que é executado o hipervisor. As regras são programadas para permitir apenas o FC e ir caixas para falar à partição do anfitrião numa porta específica. As outras exceções são permitir a resposta DHCP e Respostas DNS. Azure utiliza um ficheiro de configuração da máquina, que tem o modelo de regras de firewall para a partição do anfitrião. Também é uma exceção de firewall de anfitrião que permite que as VMs comunicar com anfitriões componentes, durante a transmissão & servidor servidor de metadados, através do protocolo/portas específicas.

**Firewall de convidado**: peça da Firewall do Windows do SO convidado, que é configurável pelo cliente em VMs do cliente e de armazenamento.

Funcionalidades de segurança adicionais incorporado nas capacidades do Azure:

- Componentes de infraestrutura são atribuídos a endereços IP que sejam IPs dedicado (DIPs). Um atacante na Internet não é possível resolver tráfego para esses endereços porque iria atingirem não Microsoft. Routers de Gateway para a Internet filtra pacotes endereçadas unicamente para endereços internos, pelo que não introduziria a rede de produção. Os componentes de apenas aceitam o tráfego direcionado para VIPs são balanceadores de carga.
- As firewalls implementadas em todos os nós internos tem três considerações de arquitetura de segurança primário para qualquer cenário determinado:

   - Estes são colocadas atrás do Balanceador de carga (LB) e aceitam pacotes a partir de qualquer lugar. Estes são se destina a ser expostos externamente e iriam correspondem para as portas abrir uma firewall de perímetro tradicional.
   - Aceite apenas pacotes de um conjunto limitado de endereços. Esta é a parte da estratégia de aprofundada de defesa contra ataques denial of service. Essas ligações são autenticadas criptograficamente.
   - Firewalls só podem ser acedidas a partir de nós internos selecionadas, nesse caso aceitem pacotes apenas a partir de uma lista de endereços IP de origem, as quais são DIPs dentro da rede do Azure enumerada. Por exemplo, um ataque na rede empresarial direcionar pedidos para estes endereços, mas seriam bloqueados, exceto se o endereço de origem do pacote foi um na lista enumerada dentro da rede do Azure.
   - O router de acesso no perímetro bloqueia pacotes de saída endereçadas a um endereço dentro da rede do Azure devido ao respetivos encaminhamentos estáticos configurados.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre o que faz Microsoft para proteger a infraestrutura do Azure, consulte:

- [Instalações do Azure, no local e a segurança física](azure-physical-security.md)
- [Disponibilidade da infraestrutura do Azure](azure-infrastructure-availability.md)
- [Componentes de sistema de informações do Azure e limites](azure-infrastructure-components.md)
- [Arquitetura de rede do Azure](azure-infrastructure-network.md)
- [Funcionalidades de segurança da base de dados de SQL do Microsoft Azure](azure-infrastructure-sql.md)
- [Gestão e operações de produção do Azure](azure-infrastructure-operations.md)
- [Monitorização da infraestrutura do Azure](azure-infrastructure-monitoring.md)
- [Integridade da infraestrutura do Azure](azure-infrastructure-integrity.md)
- [Proteção de dados de cliente no Azure](azure-protection-of-customer-data.md)

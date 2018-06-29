---
title: Funcionalidades de segurança da base de dados SQL do Azure
description: Este artigo fornece que uma descrição geral da SQL Database do Azure protege os dados de cliente no Azure.
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
ms.openlocfilehash: cca8febb004029b13b0df09a047da701c4528e8e
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102581"
---
# <a name="microsoft-azure-sql-database-security-features"></a>Funcionalidades de segurança da base de dados de SQL do Microsoft Azure    
Base de dados de SQL do Microsoft Azure fornece um serviço de base de dados relacional no Azure. Para proteger os dados de cliente e fornecer funcionalidades de segurança forte que os clientes se espera de um serviço de base de dados relacional, base de dados SQL tem os suas próprias conjuntos de funcionalidades de segurança. Estas capacidades baseiam-se os controlos que são herdados do Azure.

## <a name="security-capabilities"></a>Capacidades de segurança

### <a name="usage-of-tabular-data-stream-tds-protocol"></a>Utilização do protocolo de fluxo de dados em tabela (recebido)
Base de dados do Microsoft Azure SQL Server só suporta o protocolo TDS, que requer a base de dados esteja acessível apenas através de predefinição porta de TCP/1433.

### <a name="microsoft-azure-sql-database-firewall"></a>Firewall de base de dados SQL do Microsoft Azure
Para ajudar a proteger os dados dos clientes, a base de dados do Microsoft Azure SQL inclui uma funcionalidade de firewall, que, por predefinição, impede que todo o acesso ao servidor de base de dados SQL, conforme mostrado abaixo.

![Firewall de base de dados SQL do Azure][1]

A firewall do gateway fornece a capacidade de limitar endereços permitindo para um controlo granular aos clientes para especificar os intervalos de endereços IP aceitáveis. A firewall concede acesso com base no endereço IP de origem de cada pedido.

Configuração da firewall pode ser conseguida utilizando um Portal de gestão ou através de programação utilizando a API de REST de gestão de base de dados do Microsoft Azure SQL. A firewall do Gateway de base de dados SQL do Microsoft Azure por predefinição impede que todos os acessos TDS do cliente para bases de dados do Microsoft Azure SQL. Acesso tem de ser configurado utilizando ACLs para permitir ligações de base de dados do Microsoft Azure SQL por origem e os endereços de Internet de destino, protocolos e números de porta.

### <a name="dosguard"></a>DoSGuard
Ataques do denial of service (DoS) são reduzidas por um serviço de Gateway de base de dados do SQL Server denominado DoSGuard. DoSGuard controla ativamente inícios de sessão falhados a partir de endereços IP. Se existirem vários inícios de sessão falhados de um endereço IP específico durante um período de tempo, o endereço IP é impedido de aceder a quaisquer recursos do serviço para um período de tempo predefinido.

Para além dos acima, o gateway de base de dados do Microsoft Azure SQL também efetua a:

- Negociações de capacidade de canal seguro para implementar a certificação FIPS 140-2 TDS validada ligações encriptadas ao estabelecer ligação com os servidores de base de dados.
- Inspeção de pacotes TDS com monitorização de estado ao aceitar ligações de clientes. O gateway valida as informações de ligação e passa nos pacotes recebido para o servidor físico adequado com base no nome da base de dados especificado na cadeia de ligação.

O princípio abrangente para segurança de rede de oferta de base de dados do Microsoft Azure SQL é apenas permitir a ligação e a comunicação que é necessária permitir que o serviço de funcionar. Todas as outras portas, protocolos e as ligações estão bloqueadas por predefinição. VLANs e as ACLs são utilizadas para restringir comunicações de rede por origem e redes de destino, protocolos e números de porta.

Mecanismos aprovados para implementar as ACLs baseadas na rede incluem: ACLs de routers e Balanceadores de carga. Estes são geridos pelo redes do Azure, a firewall da VM do convidado e regras de firewall gateway de base de dados do Microsoft Azure SQL, que são configuradas pelo cliente.

## <a name="data-segregation-and-customer-isolation"></a>Isolamento de segregação e o cliente de dados
A rede de produção do Azure está estruturada, de modo a que os componentes de sistema acessível publicamente são separados de recursos internos. Existem limites físicos e lógicos entre servidores web fornecer acesso para o portal do Azure destinados ao público e do Azure virtual infraestrutura subjacente, onde residem os instâncias da aplicação de cliente e dados de cliente.

Todas as informações de acessíveis publicamente são geridas dentro da rede de produção do Azure. A rede de produção sujeitos a autenticação de dois fatores e mecanismos de proteção de limite, utiliza a funcionalidade de segurança e de firewall definir descrita na secção anterior e utiliza as funções de isolamento de dados conforme indicado abaixo.

### <a name="unauthorized-systems-and-isolation-of-fc"></a>Sistemas não autorizados e isolamento de FC
Uma vez que o FC o orchestrator central dos recursos de infraestrutura do Microsoft Azure, são significativos controlos implementados para mitigar ameaças, especialmente FAs potencialmente comprometidos dentro de aplicações de cliente. FC não reconhece qualquer hardware cujas informações de dispositivo (por exemplo, endereço MAC) não foi previamente carregadas dentro de FC. Os servidores DHCP na FC configurou apresenta uma lista de endereços MAC de nós que estão dispostos a efetuar o arranque. Mesmo se estiverem ligados a sistemas não autorizados, eles não são incorporados no inventário de recursos de infraestrutura e, por conseguinte, não ligados ou autorizados a comunicar com qualquer sistema dentro do inventário de recursos de infraestrutura. Isto reduz o risco de sistemas não autorizados a comunicar com o FC e obter acesso a VLAN e o Azure.

### <a name="vlan-isolation"></a>Isolamento de VLAN
A rede de produção do Azure logicamente é segregada para três VLANs primárias:

- A VLAN principal – interconnects nós não fidedignos do cliente
- A VLAN de FC – contém fidedigna FCs e sistemas de suporte
- O dispositivo VLAN – contém rede fidedigna e outros dispositivos de infraestrutura

### <a name="packet-filtering"></a>Filtragem de pacotes
O IPFilter e as firewalls do software implementadas no SO de raiz e SO convidado de nós de impor restrições de conectividade e impedir que o tráfego entre VMs não autorizado.

### <a name="hypervisor-root-os-and-guest-vms"></a>Hipervisor, SO de raiz e as VMs de convidado
O isolamento do SO raiz das VMs de convidado e as VMs de convidado umas é gerido pelo hipervisor e o SO de raiz.

### <a name="types-of-rules-on-firewalls"></a>Tipos de regras de Firewalls
Uma regra está definida como:

{Security Response Center (Src), porta Src, destino IP, porta de destino, destino protocolo de endereço IP, entrada/saída, tempo limite de fluxo com monitorização de estado/sem monitorização de estado, com monitorização de estado}.

Pacotes de SIN sejam permitidos entrada ou saída apenas se permite que qualquer uma das regras. Para TCP, o Microsoft Azure utiliza regras sem monitorização de Estado onde o princípio é que só permite que todos os pacotes não-SIN ou a sair da VM. A premissa de segurança está pilha qualquer anfitrião resiliente de ignorar um não-SIN se não vistos um pacote de SIN anteriormente. O próprio protocolo TCP é com monitorização de estado e, em combinação com o SYNbased sem monitorização de estado regra distribui um comportamento geral de uma implementação com monitorização de estado.

Para o protocolo UDP (User Datagram), o Microsoft Azure utiliza uma regra com monitorização de estado. Sempre que um pacote UDP corresponde a uma regra, um fluxo inverso é criado na outra direção. Este fluxo tem um limite de tempo incorporada.

Os clientes são responsáveis por configurar as seus próprios firewalls por cima do Microsoft Azure oferece. Aqui os clientes conseguem definem as regras para o tráfego de entrada e saída.

### <a name="production-configuration-management"></a>Gestão de configuração de produção
Configurações de segurança padrão são mantidas por equipas respetivas operações no Azure e a base de dados do Microsoft Azure SQL. Todas as alterações de configuração para sistemas de produção estão documentadas e acompanhadas através de um sistema de controlo central. As alterações de software e hardware são registadas através do sistema de controlo central. As alterações de rede relacionado com a ACL são registadas utilizando o serviço de gestão de ACL (AMS).

Todas as alterações de configuração para o Microsoft Azure são desenvolvidas e testadas no ambiente de teste; e após implementado no ambiente de produção. Compilações de software são revistas como parte do teste. Verificações de segurança e privacidade são revistas como parte dos critérios de lista de verificação de entrada. As alterações são implementadas em intervalos agendados pela equipa de implementação correspondentes. Versões são revistas e assinadas pela equipa da equipa a respetiva implementação antes de que são implementados em produção.

As alterações são monitorizadas para êxito. No cenário de falha, a alteração é back revertida para o estado anterior ou for implementada uma correção para resolver a falha com aprovação dos empregados designados. Depósito de origem, o Git, do TFS, MDS, Runners, monitorização de segurança do Azure (ASM), o FC e a plataforma de WinFabric são utilizadas para gerir centralmente, aplicam-se e verifique as definições de configuração do ambiente virtual do Azure.

Da mesma forma, as alterações de hardware e de rede estabelecer passos de validação para avaliar a respetiva conformidade com os requisitos de compilação. As versões são revistas e autorizadas através de uma coordenada alteração Advisory quadro (CAB) dos respetivos grupos em toda a pilha.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre o que faz Microsoft para proteger a infraestrutura do Azure, consulte:

- [Instalações do Azure, no local e a segurança física](azure-physical-security.md)
- [Disponibilidade da infraestrutura do Azure](azure-infrastructure-availability.md)
- [Componentes de sistema de informações do Azure e limites](azure-infrastructure-components.md)
- [Arquitetura de rede do Azure](azure-infrastructure-network.md)
- [Rede de produção do Azure](azure-production-network.md)
- [Gestão e operações de produção do Azure](azure-infrastructure-operations.md)
- [Monitorização da infraestrutura do Azure](azure-infrastructure-monitoring.md)
- [Integridade da infraestrutura do Azure](azure-infrastructure-integrity.md)
- [Proteção de dados de cliente no Azure](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-infrastructure-sql/sql-database-firewall.png

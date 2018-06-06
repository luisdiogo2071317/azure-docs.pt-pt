---
title: Segurança do Azure e conformidade Blueprint - PaaS Web de aplicação para FedRAMP
description: Segurança do Azure e conformidade Blueprint - PaaS Web de aplicação para FedRAMP
services: security
author: jomolesk
ms.assetid: 41570fc1-4d74-48ed-afb0-ef1be857029e
ms.service: security
ms.topic: article
ms.date: 06/01/2018
ms.author: jomolesk
ms.openlocfilehash: 20aa842fb8168bc28a388c817f4e4eedbdd63ebd
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34728028"
---
# <a name="azure-security-and-compliance-blueprint-paas-web-application-for-fedramp"></a>Segurança do Azure e conformidade Blueprint: aplicação Web de PaaS para FedRAMP

## <a name="overview"></a>Descrição geral

O [risco Federal e o programa de gestão de autorização (FedRAMP)](https://www.fedramp.gov/) é um programa de toda a government dos Estados Unidos que fornece uma abordagem padronizada para avaliação de segurança, autorização e monitorização contínua para a nuvem os produtos e serviços. Esta segurança do Azure e a conformidade Blueprint fornece orientações para como fornecer uma plataforma Microsoft Azure como uma arquitetura de serviço (PaaS), que ajuda a implementar um subconjunto dos controlos FedRAMP elevado. Esta solução fornece orientações para a implementação e configuração de recursos do Azure para uma arquitetura de referência comum, que demonstra formas em que os clientes podem cumpre os requisitos específicos de segurança e conformidade e funciona como uma base para os clientes criar e configurar as suas próprias soluções no Azure.

Esta arquitetura de referência, guias de implementação do controlo associado e modelos de ameaça destinam-se para servir como base para os clientes ajustar os seus requisitos específicos e não devem ser utilizados como-é num ambiente de produção. Implementar uma aplicação para este ambiente sem modificação é insuficiente para completamente cumprir os requisitos de linha de base FedRAMP elevado. Tenha em atenção o seguinte:
- A arquitetura fornece uma linha de base para ajudar os clientes implementar cargas de trabalho no Azure de forma FedRAMP compatíveis.
- Os clientes são responsáveis para realizar a segurança adequada e avaliações de conformidade de qualquer solução incorporada com esta arquitetura, como os requisitos podem variar com base nas especificações de implementação de cada cliente.

## <a name="architecture-diagram-and-components"></a>Diagrama da arquitetura e componentes
Esta solução fornece uma arquitetura de referência para uma aplicação web de PaaS com um back-end da SQL Database do Azure. A aplicação web está alojada num isolado Azure aplicação ambiente de serviço, que é um ambiente de privado, dedicado num centro de dados do Azure. A carga de ambiente equilibra o tráfego para a aplicação web através de VMs geridas pelo Azure. Esta arquitetura também inclui os grupos de segurança de rede, um Gateway de aplicação, o DNS do Azure e o Balanceador de carga. Além disso, o Operations Management Suite fornece análises em tempo real de estado de funcionamento do sistema e segurança. **Azure recomenda configurar uma ligação VPN ou ExpressRoute para importação de dados e de gestão para a sub-rede de arquitetura de referência.**

![Aplicação Web de PaaS para FedRAMP diagrama da arquitetura de referência](images/fedramp-paaswa-architecture.png?raw=true) "PaaS a aplicação Web para o diagrama de arquitetura de referência FedRAMP"

Esta solução utiliza os seguintes serviços do Azure. Detalhes da arquitetura de implementação estão localizados no [arquitetura de implementação](#deployment-architecture) secção.

- Azure Active Directory
- Azure Key Vault
- Base de Dados SQL do Azure
- Gateway de Aplicação
    - (1) Firewall de aplicações web
        - modo de firewall: prevenção
        - Conjunto de regras: OWASP 3.0
        - serviço de escuta: porta 443
- Rede virtual do Azure
- Grupos de segurança de rede
- DNS do Azure
- Storage do Azure
- Operations Management Suite
- Azure Monitor
- V2 de ambiente de serviço de aplicações
- Azure Load Balancer
- Aplicação Web do Azure
- Azure Resource Manager

## <a name="deployment-architecture"></a>Arquitetura de implementação
A secção seguinte descreve em detalhe os elementos de implementação e a implementação.

**O Azure Resource Manager**: [do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) permite que os clientes trabalhar com os recursos na solução como um grupo. Os clientes podem implementar, atualizar ou eliminar todos os recursos para a solução numa operação única e coordenada. Os clientes utilizam um modelo para a implementação e esse modelo pode funcionar para ambientes diferentes, como de teste, teste e produção. O Resource Manager fornece segurança, auditoria e etiquetagem funcionalidades para ajudar a gerir os respetivos recursos após a implementação de clientes.

**Aplicação ambiente de serviço v2**: O [ambiente de serviço de aplicações do Azure (ASE)](https://docs.microsoft.com/azure/app-service/environment/intro) é uma funcionalidade do serviço de aplicações que fornece um ambiente completamente isolado e dedicado para execução segura de aplicações de serviço de aplicações numa escala elevada.

ASEs isoladas execute apenas aplicações de um único cliente e sempre são implementadas numa rede virtual. Os clientes não têm controlo detalhado sobre o tráfego de rede de aplicação de entrada e saída e as aplicações podem estabelecer ligações seguras de alta velocidade através de redes virtuais aos recursos da empresa no local.

Utilização de ASEs para esta arquitetura são permitidas para controlos/configurações seguintes:

- Alojar dentro de uma rede Virtual do Azure seguros e regras de segurança de rede
- ASE configurado com o certificado autoassinado do ILB para comunicação HTTPS
- [Modo de balanceamento de carga interno](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer)
- Desativar [TLS 1.0](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Alteração [cifras TLS](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Controlo [portas de N/W de tráfego de entrada](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [Firewall de aplicação Web – restrinja dados](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- Permitir [tráfego SQL Database do Azure](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)

O [orientações e recomendações](#guidance-and-recommendations) secção contém informações adicionais sobre ASEs.

**Aplicação Web do Azure**: [Web Apps do Azure](https://docs.microsoft.com/azure/app-service/) permite aos clientes criar e alojar aplicações web na linguagem de programação da sua eleição sem gerir a infraestrutura. Este serviço garante dimensionamento automático e elevada disponibilidade, suporta Windows e Linux e permite implementações automáticas a partir do GitHub, Visual Studio Team Services ou de qualquer repositório de Git.

### <a name="virtual-network"></a>Rede Virtual
A arquitetura define uma rede privada virtual com um espaço de endereços de 10.200.0.0/16.

**Grupos de segurança de rede**: [(NSGs) de grupos de segurança de rede](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) contêm listas de controlo de acesso que permitem ou negam o tráfego dentro de uma rede virtual. Os NSGs podem ser utilizados para proteger o tráfego de uma sub-rede ou nível VM individuais. Os seguintes NSGs de existir:
- 1 NSG para Gateway de aplicação
- 1 NSG para ambiente de serviço de aplicações
- 1 NSG para base de dados SQL do Azure

Cada um dos NSGs ter específicas portas e protocolos abrir para que a solução possam funcionar corretamente e em segurança. Além disso, as configurações seguintes estão ativadas para cada NSG:
  - [Os registos de diagnóstico e de eventos](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) estão ativadas e armazenados numa conta do storage
  - Análise de registos do OMS está ligado a [diagnóstico do NSG](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Sub-redes**: cada sub-rede está associada a respetiva NSG correspondente.

**O DNS do Azure**: O sistema de nomes de domínio ou DNS, é responsável por traduzir (ou resolver) um nome de Web site ou serviço para o endereço IP. [O DNS do Azure](https://docs.microsoft.com/azure/dns/dns-overview) é um serviço de alojamento para domínios DNS que fornece a resolução de nomes através da infraestrutura do Azure. Ao alojar domínios no Azure, os utilizadores podem gerir registos DNS utilizando as ferramentas de credenciais, APIs, mesmo e, de faturação como outros serviços do Azure. O DNS do Azure também suporta privados domínios DNS.

**Balanceador de carga do Azure**: [Balanceador de carga do Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) permite aos clientes dimensionar as aplicações e criar a elevada disponibilidade para serviços. Balanceador de carga suporta cenários de entrada como saídos e fornece a latência baixa, débito elevado e reduz horizontalmente até milhões de fluxos de todas as aplicações de TCP e UDP.

### <a name="data-in-transit"></a>Dados em trânsito
Azure encripta todas as comunicações de e para os datacenters do Azure, por predefinição. Todas as transações ao Storage do Azure através do portal do Azure ocorrem através de HTTPS.

### <a name="data-at-rest"></a>Dados inativos
A arquitetura protege os dados Inativos através de encriptação, base de dados de auditoria e de outras medidas.

**Armazenamento do Azure**: para satisfazer os dados encriptados em requisitos de rest, todos os [Storage do Azure](https://azure.microsoft.com/services/storage/) utiliza [encriptação do serviço de armazenamento](https://docs.microsoft.com/azure/storage/storage-service-encryption).

**Azure Disk Encryption**
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) aproveita a funcionalidade de BitLocker do Windows para fornecer a encriptação de volume para discos de dados. A solução integra-se com o Cofre de chaves do Azure para ajudar a controlar e gerir as chaves de encriptação de disco.

**Base de dados SQL do Azure**: instância de base de dados de SQL do Azure utiliza as seguintes medidas de segurança da base de dados:
-   [Autorização e autenticação AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) permite a gestão de identidades de utilizadores de base de dados e outros serviços Microsoft numa localização central.
-   [Auditoria de base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) controla da base de dados eventos e escreve-los para uma auditoria iniciem sessão uma conta de armazenamento do Azure.
-   Base de dados SQL do Azure está configurado para utilizar [encriptação de dados transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), que efetua a encriptação em tempo real e a desencriptação da base de dados, cópias de segurança associadas e os ficheiros de registo de transações para proteger as informações em rest.
-   [Regras de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impedir o acesso de todos os servidores de base de dados até que são concedidas permissões adequadas. A firewall concede acesso a bases de dados com base no endereço IP de origem de cada pedido.
-   [A deteção de ameaças do SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) permite a deteção e a resposta a potenciais ameaças à medida que ocorrem fornecendo alertas de segurança para acesso de base de dados anómalas, ataques de injeção de SQL, potenciais vulnerabilidades e atividades suspeitas da base de dados padrões.
-   [Encriptados colunas sempre](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) Certifique-se de que os dados confidenciais nunca é apresentada como texto não encriptado no interior do sistema de base de dados. Depois de ativar a encriptação de dados, apenas as aplicações de cliente ou servidores de aplicações com o acesso às chaves podem aceder aos dados de texto simples.
- [Segurança ao nível da linha](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) permite aos utilizadores definir políticas para restringir o acesso aos dados para descontinuar o processamento.
- [Máscara de dados dinâmicos da base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) podem ser efetuados após implementa a arquitetura de referência. Os clientes terão de ajustar as definições de aderir ao respetiva esquema de base de dados de máscara de dados dinâmicos.

### <a name="identity-management"></a>Gestão de identidades
As seguintes tecnologias de fornecem a identidade de capacidades de gestão no ambiente do Azure:
-   [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) é multi-inquilino baseado na nuvem diretório e identidade do serviço de gestão da Microsoft. Todos os utilizadores para esta solução são criados no AAD, incluindo os utilizadores que acedem a base de dados do SQL do Azure.
-   Autenticação para a aplicação é executada com o AAD. Para obter mais informações, consulte [integrar aplicações com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Além disso, a encriptação de coluna de base de dados utiliza o Azure Active Directory para autenticar a aplicação a SQL Database do Azure. Para obter mais informações, consulte como [proteger dados sensíveis na base de dados do Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Controlo de acesso baseado em funções do Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) permite precisamente direcionadas para gestão de acesso ao Azure. Acesso de subscrição está limitado ao administrador da subscrição e acesso a recursos pode ser limitado com base na função de utilizador.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) permite que os clientes minimizar o número de utilizadores que tenham acesso aos determinadas informações.  Os administradores podem utilizar o AAD Privileged Identity Management para detetar, restringir e monitorizar as identidades privilegiadas e o respetivo acesso aos recursos. Esta funcionalidade também pode ser utilizada para impor o acesso administrativo a pedido, just-in-time quando necessário.
- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) Deteta potenciais vulnerabilidades que afetam as identidades de uma organização, configura as respostas automatizadas detetadas ações suspeitas relacionada com identidades da organização e investiga incidentes suspeitas tome medidas adequadas resolvê-los.

### <a name="security"></a>Segurança
**Gestão de segredos** a solução utiliza [Cofre de chaves do Azure](https://azure.microsoft.com/services/key-vault/) para a gestão de chaves e segredos. O Cofre de Chaves do Azure ajuda a salvaguardar as chaves criptográficas e os segredos utilizados pelas aplicações em cloud e pelos serviços. As seguintes capacidades de Cofre de chaves do Azure ajudam a proteger dados e acesso a esses dados de clientes:
- Políticas de acesso avançadas são configuradas de forma necessidade.
- As políticas de acesso do Cofre de chaves são definidas com permissões mínimas para chaves e segredos.
- Todas as chaves e segredos no Cofre de chaves tem datas de expiração.
- Todas as chaves no Cofre de chaves estão protegidas por módulos de hardware especializado de segurança (HSMs). O tipo de chave é uma chave RSA de 2048 protegidas de HSM bits.
- Todos os utilizadores e identidades são concedidas as permissões necessárias mínimo utilizando o controlo de acesso baseado em funções.
- Registos de diagnóstico para o Cofre de chaves estão ativados com um período de retenção de, pelo menos, a 365 dias.
- Operações de criptografia permitidas para as chaves estão limitadas às necessárias.

**Gateway de aplicação** a arquitetura reduz o risco de vulnerabilidades de segurança através de um Gateway de aplicação com Firewall de aplicações Web e o ruleset OWASP ativada. Capacidades adicionais incluem:
- [End-to-End-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Ativar [descarga de SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Desativar [TLS v 1.0 e v 1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Firewall de aplicações Web](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview)
- [Modo de prevenção](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) com ruleset OWASP 3.0
- Ativar [registo de diagnóstico](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Sondas de estado de funcionamento personalizado](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Centro de segurança do Azure](https://azure.microsoft.com/services/security-center) e [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) fornecer proteção adicional e notificações. Centro de segurança do Azure também fornece um sistema de reputação de URLs.

### <a name="logging-and-auditing"></a>Registo e auditoria
Conjunto de gestão de operações fornece extenso registo de atividade de utilizador e de sistema, bem como o estado de funcionamento do sistema. O conjunto de gestão do Operations [Log Analytics](https://azure.microsoft.com/services/log-analytics/) solução recolhe e analisa os dados gerados pelos recursos no Azure e ambientes no local.
- **Registos de atividade**: [registos de atividade](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) fornecem informações aprofundadas operações executadas nos recursos numa subscrição. Registos de atividade podem ajudar a determinar o iniciador de uma operação, hora de ocorrência e o estado.
- **Os registos de diagnóstico**: [registos de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) incluem todos os registos emitidos por cada recurso. Estes registos incluem registos de sistema de eventos do Windows, os registos de armazenamento do Azure, os registos de auditoria do Cofre de chaves e os registos de acesso e de firewall de Gateway de aplicação.
- **Registo arquivar**: todos os registos de diagnóstico de escrita para uma conta de armazenamento do Azure centralizada e encriptados para arquivo. O período de retenção é configurável pelo utilizador, cópia de segurança para dias 730, para satisfazer os requisitos de retenção específico da organização. Estes registos ligar ao Log Analytics do Azure para o processamento, armazenamento e relatórios do dashboard.

Além disso, as soluções do seguintes Operations Management suite são incluídas como parte desta arquitetura:
-   [Do Active Directory avaliação](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): O Active Directory estado de funcionamento de verificação solução avalia o risco e o estado de funcionamento de ambientes de servidor num intervalo regular e fornece uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
-   [Avaliação de antimalware](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): solução Antimalware a relatórios de estado de proteção, software maligno e ameaças.
-   [A automatização do Azure](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): solução a automatização do Azure armazena, é executado e gere runbooks. Nesta solução, os runbooks ajudam a recolher registos de Application Insights e a SQL Database do Azure.
-   [Auditoria e segurança](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): auditoria e segurança dashboard fornece uma alto nível aprofundadas sobre o estado de segurança de recursos, fornecendo as métricas em domínios de segurança, problemas relevantes, deteções, ameaças e consultas de segurança comuns.
-   [Avaliação do SQL Server](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): O estado de funcionamento de SQL Consulte solução avalia o risco e o estado de funcionamento de ambientes de servidor num intervalo regular e fornece aos clientes com uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
-   [Gestão de atualizações](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): solução de gestão de atualizações a permite a gestão de cliente de atualizações de segurança do sistema operativo, incluindo o processo de instalação de atualizações necessárias e o estado de atualizações disponíveis.
-   [Estado de funcionamento do agente](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): solução do estado de funcionamento do agente comunica quantos agentes implementados e respetiva distribuição geográfica, bem como quantos agentes que estão a responder e o número de agentes que submeter dados operacionais.
-   [Os registos de atividade do Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): solução de análise de registos de atividade o auxilia com a análise de registos a atividade do Azure em todas as subscrições do Azure para um cliente.
-   [Registo de alterações](https://docs.microsoft.com/azure/automation/automation-change-tracking): controlo de alterações a solução permite aos clientes identificar facilmente as alterações no ambiente.

**Monitor do Azure**
[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) ajuda os utilizadores a monitorizar o desempenho, manter a segurança e identificar tendências, permitindo que as organizações de auditoria, criar alertas e arquivar dados, incluindo chamadas da API de controlo nos recursos do Azure dos clientes.

## <a name="threat-model"></a>Modelo de ameaça

O diagrama de fluxo de dados para esta arquitetura de referência está disponível para [transferir](https://aka.ms/fedrampPaaSWebAppDFD) ou podem ser encontrados abaixo. Este modelo pode ajudar os clientes os pontos de risco potencial na infraestrutura do sistema quando efetuar alterações.

![Aplicação Web de PaaS para o modelo de ameaça FedRAMP](images/fedramp-paaswa-threat-model.png?raw=true "PaaS Web Applicaiton para o modelo de ameaça FedRAMP")

## <a name="compliance-documentation"></a>Documentação de conformidade
O [segurança do Azure e conformidade Blueprint - FedRAMP elevada cliente responsabilidade matriz](https://aka.ms/blueprinthighcrm) apresenta uma lista de todos os controlos de segurança necessários para a linha de base FedRAMP elevado. A matriz indica se a implementação de cada controlo é da responsabilidade do Microsoft, o cliente, ou partilhados entre os dois.

O [segurança do Azure e conformidade Blueprint - FedRAMP PaaS WebApp elevada controlo implementação matriz](https://aka.ms/fedrampPaaSWebAppCIM) apresenta uma lista de todos os controlos de segurança necessários para a linha de base FedRAMP elevado. A matriz fornece informações no qual os controlos estão abrangidos pela arquitetura de aplicação da web PaaS, incluindo como a implementação cumpre os requisitos de cada controlo coberto obter descrições detalhadas.

## <a name="guidance-and-recommendations"></a>Orientações e recomendações
### <a name="vpn-and-expressroute"></a>ExpressRoute e de VPN
Um túnel VPN seguro ou [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) tem de ser configurado com segurança estabelecer uma ligação para os recursos implementados como parte da arquitetura de referência da aplicação de web PaaS. Adequadamente a configurar uma VPN ou ExpressRoute, os clientes podem adicionar uma camada de proteção de dados em trânsito.

Ao implementar um túnel VPN seguro com o Azure, é possível criar uma ligação VPN entre uma rede no local e uma rede Virtual do Azure. Esta ligação ocorre através da Internet e permite aos clientes de forma segura as informações de "túnel" dentro de uma ligação encriptada entre a rede do cliente e o Azure. VPN de Site a site é uma tecnologia de segura, madura que tenha sido implementada por empresas de todos os tamanhos de decades. O [modo de túnel IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) é utilizada nesta opção como um mecanismo de encriptação.

Porque o tráfego de túnel VPN atravessam a Internet e uma VPN de site a site, a Microsoft oferece a opção de ligação de outro, ainda mais seguros. O ExpressRoute do Azure é uma WAN dedicada ligação entre o Azure e uma localização no local ou um fornecedor de alojamento do Exchange. Como as ligações do ExpressRoute não passam para a Internet, estas ligações oferecem mais fiabilidade, velocidades mais rápidas, latências inferiores e uma maior segurança ligações típicas através da Internet. Além disso, porque se trata de uma ligação direta do fornecedor de telecommunication do cliente, os dados não viajam através da Internet e, por conseguinte, não estão expostos ao mesmo.

Melhores práticas para implementar uma rede híbrida segura que expande uma rede no local para o Azure são [disponíveis](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Exclusão de Responsabilidade

 - Este documento destina-se apenas a fins informativos. MICROSOFT FAZ COM QUE NÃO OFERECE NENHUMA GARANTIA, EXPRESSAS, IMPLÍCITA OU ESTATUTARIAMENTE AS INFORMAÇÕES NESTE DOCUMENTO. Este documento é fornecido "como-está." As informações e opiniões expressados neste documento, incluindo URLs e outras referências de Web site da Internet, podem ser alteradas sem aviso prévio. Os clientes ler este documento da sua responsabilidade utilizá-la.
 - Este documento não fornece aos clientes com quaisquer direitos legais a nenhuma propriedade intelectual de qualquer produto da Microsoft ou de soluções.
 - Os clientes podem copiar e utilizar este documento para efeitos de referência interno.
 - Algumas recomendações neste documento poderão resultar numa maior dados, a rede ou a utilização de recursos de computação no Azure e podem aumentar os custos de licenciamento ou de subscrição do Azure do cliente.
 - Esta arquitetura destina-se para servir como base para os clientes ajustar os seus requisitos específicos e não devem ser utilizada como-é num ambiente de produção.
 - Este documento é desenvolvido como uma referência e não deve ser utilizado para definir todas as formas através do qual um cliente pode cumpre os requisitos de conformidade específico e regulamentos. Os clientes devem procurar suporte legal da respetiva organização em implementações de cliente aprovados.

---
title: Segurança do Azure e o esquema de conformidade - aplicação Web de PaaS para NHS do Reino Unido
description: Segurança do Azure e o esquema de conformidade - aplicação Web de PaaS para NHS do Reino Unido
services: security
author: jomolesk
ms.assetid: fe409add-6d09-4062-b3c8-d74574747739
ms.service: security
ms.topic: article
ms.date: 06/15/2018
ms.author: jomolesk
ms.openlocfilehash: 60f0c7db8e97c9fb90f18e41e9c8ae872d348331
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53600692"
---
# <a name="azure-security-and-compliance-blueprint-paas-web-application-for-uk-nhs"></a>Azure Security and Compliance Blueprint: Aplicação Web de PaaS para NHS do Reino Unido

## <a name="overview"></a>Descrição geral

Este Azure no Security and Compliance Blueprint fornece uma arquitetura de referência e orientação para uma plataforma como uma solução de serviço (PaaS) adequada para a coleção, armazenamento e obtenção de dados de cuidados de saúde. Esta solução demonstra maneiras em que, os clientes podem estar em conformidade com a orientação fornecida na [guia de práticas de bom em segurança de Cloud](https://digital.nhs.uk/data-and-information/looking-after-information/data-security-and-information-governance/nhs-and-social-care-data-off-shoring-and-the-use-of-public-cloud-services/health-and-social-care-cloud-security-good-practice-guide) publicada pela [NHS Digital](https://digital.nhs.uk/), um parceiro do Reino Unido (Reino Unido) Departamento de estado de funcionamento e sociais cuidados de saúde (DHSC). O guia de práticas de boa de segurança do Cloud baseia-se a 14 [princípios de segurança de Cloud](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) publicado pelo do Reino Unido National Cyber Security Centre (NCSC).

Esta arquitetura de referência, o guia de implementação e o modelo de risco são deve ser usado como uma base para os clientes que se ajustar às suas necessidades específicas e não devem ser utilizados como-está num ambiente de produção sem configuração adicional. Os clientes são responsáveis pela realização de segurança adequados e avaliações de conformidade de qualquer solução criada com esta arquitetura, como os requisitos podem variar com base nas especificidades da implementação de cada cliente.

## <a name="architecture-diagram-and-components"></a>Diagrama de arquitetura e componentes

Esta solução fornece uma arquitetura de referência para um aplicativo da web de PaaS com um back-end de base de dados do Azure SQL. O aplicativo web é hospedado num Azure App Service ambiente isolado, que é um ambiente privado e dedicado num datacenter do Azure. A carga de ambiente equilibra o tráfego para a aplicação web nas máquinas virtuais geridas pelo Azure. Todas as ligações externas requerem TLSv1.2. Esta arquitetura também inclui os grupos de segurança de rede, um Gateway de aplicação, o DNS do Azure e o Balanceador de carga.

A solução utiliza contas de armazenamento do Azure, o que os clientes podem configurar para utilizar a encriptação do serviço de armazenamento para manter a confidencialidade dos dados em repouso. Azure armazena três cópias dos dados num datacenter selecionados de um cliente para resiliência. Armazenamento com redundância geográfico garante que os dados vão ser replicados para um datacenter secundário centenas de quilómetros de distância e armazenados novamente como três cópias dentro desse datacenter, impedir que um evento adverso no Centro de dados principal do cliente resulta numa perda de dados.

Para maior segurança, todos os recursos nesta solução são geridos como um grupo de recursos através do Gestor de recursos do Azure. Controlo de acesso baseado em funções do Azure Active Directory é utilizado para controlar o acesso para implementar recursos e as chaves no Azure Key Vault. Estado de funcionamento do sistema é monitorizado através do Centro de segurança do Azure e o Azure Monitor. Os clientes configurar ambos os serviços de monitorização para capturar os registos e exibir o estado de funcionamento do sistema num dashboard único, navegável facilmente. O Gateway de aplicação do Azure está configurado como uma firewall no modo de prevenção e não permite o tráfego que não seja TLSv1.2. A solução utiliza o ambiente de serviço de aplicações do Azure v2 para isolar a camada da web num ambiente de não multi-inquilino.

**A Microsoft recomenda configurar uma ligação VPN ou ExpressRoute para importação de dados e de gestão para a sub-rede de arquitetura de referência.**

![Aplicação Web de PaaS para o diagrama de arquitetura de referência do Reino Unido NHS](images/uknhs-paaswa-architecture.png?raw=true "aplicativo da Web de PaaS para o diagrama de arquitetura de referência de NHS do Reino Unido")

Esta solução utiliza os seguintes serviços do Azure. Os detalhes a arquitetura de implementação estão no [arquitetura de implantação](#deployment-architecture) secção.

- Gateway de Aplicação
    - Firewall de aplicação Web
        - Modo de firewall: prevenção
        - Conjunto de regras: OWASP
        - Porta do serviço de escuta: 443
- Azure Active Directory
- Ambiente de serviço de aplicações do Azure v2
- Automatização do Azure
- DNS do Azure
- Azure Key Vault
- Azure Load Balancer
- Azure Monitor
- Azure Resource Manager
- Centro de Segurança do Azure
- Base de Dados SQL do Azure
- Storage do Azure
- Rede Virtual do Azure
    - Grupos de segurança de rede
- Aplicação Web do Azure

## <a name="deployment-architecture"></a>Arquitetura de implantação

A secção seguinte fornece detalhes sobre os elementos de implantação e a implementação.

**O Azure Resource Manager**: [O Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) permite que os clientes trabalhar com os recursos da solução como um grupo. Os clientes podem implementar, atualizar ou eliminar todos os recursos para a solução numa operação única e coordenada. Os clientes a utilizar um modelo para a implementação e esse modelo pode funcionar para ambientes diferentes, tais como teste, transição e produção. O Resource Manager proporciona segurança, auditoria e etiquetagem recursos para ajudar os clientes a gerir os seus recursos após a implementação.

**Ambiente de serviço de aplicações v2**: O ambiente de serviço de aplicações do Azure é uma funcionalidade de serviço de aplicações que fornece um ambiente totalmente isolado e dedicado para uma execução segura de aplicativos de serviço de aplicações a uma escala elevada.

O ambiente de serviço de aplicações está isolado para executar apenas um único aplicativo e sempre é implementado numa rede virtual. Esta funcionalidade de isolamento permite que a arquitetura de referência para o isolamento de inquilino completo, removê-lo a partir do ambiente de multi-inquilino do Azure. Esta funcionalidade de isolamento é necessário para satisfazer os requisitos do princípio do Reino Unido NHS 3. Os clientes têm um controle refinado sobre o tráfego de rede aplicação de entrada e saída e as aplicações podem estabelecer conexões seguras de alta velocidade nas redes virtuais para recursos da empresa no local. Os clientes podem "dimensionamento automático" com o ambiente de serviço de aplicações baseadas em métricas de carga, o orçamento disponível ou uma agenda definida.


Utilização do ambiente de serviço de aplicações para esta arquitetura permite as seguintes configurações/controles:

- Alojar dentro de uma rede virtual do Azure protegida e regras de segurança de rede
- Certificado de Balanceador de carga interno com assinatura automática para comunicação HTTPS. Como melhor prática, a Microsoft recomenda a utilização de uma autoridade de certificação fidedigna para uma maior segurança.
- [Modo de balanceamento de carga interno](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer)
- Desativar [TLS 1.0](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Alteração [cifras TLS](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Controle [portas de N/W de tráfego de entrada](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [Firewall de aplicações Web – restringir dados](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- Permitir [tráfego de base de dados do Azure SQL](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)

**Aplicação Web do Azure**: [Serviço de aplicações do Azure](https://docs.microsoft.com/azure/app-service/) permite aos clientes criar e alojar aplicações web na linguagem de programação de sua preferência sem ter de gerir infraestrutura. Este serviço garante dimensionamento automático e elevada disponibilidade, suporta Windows e Linux e permite implementações automáticas a partir do GitHub, DevOps do Azure ou de qualquer repositório de Git.

### <a name="virtual-network"></a>Rede Virtual

A arquitetura define uma rede privada virtual com um espaço de endereços de 10.200.0.0/16.

**Grupos de segurança de rede**: [Grupos de segurança de rede](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) contêm listas de controlo de acesso que permitem ou negam o tráfego dentro de uma rede virtual. Grupos de segurança de rede podem ser utilizados para proteger o tráfego numa sub-rede ou o nível de máquina virtual individual. Existem os seguintes grupos de segurança de rede:

- grupo de segurança de rede de 1 para o Gateway de aplicação
- grupo de segurança de rede de 1 para o ambiente de serviço de aplicações
- grupo de segurança de rede de 1 para a base de dados do Azure SQL

Cada um dos grupos de segurança de rede têm portas específicas e protocolos abrir para que a solução possam funcionar corretamente e de forma segura. Além disso, as seguintes configurações são habilitadas para cada grupo de segurança de rede:

- [Eventos e registos de diagnóstico](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) forem ativadas e armazenados numa conta de armazenamento
- O log Analytics está ligado à [grupo de segurança de rede&#39;s os registos de diagnóstico](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Sub-redes**: Cada sub-rede está associado a seu grupo de segurança de rede correspondente.

**O DNS do Azure**: O sistema de nomes de domínio ou DNS, é responsável pela tradução (ou resolver) um nome de Web site ou serviço para o endereço IP. [O DNS do Azure](https://docs.microsoft.com/azure/dns/dns-overview) é um serviço de alojamento dos domínios DNS que fornece resolução de nome, através da infraestrutura do Azure. Ao alojar os domínios no Azure, os utilizadores podem gerir registos DNS com as mesmas credenciais, APIs, ferramentas e faturação dos outros serviços do Azure. O DNS do Azure também suporta domínios DNS privados.

**Balanceador de carga do Azure**: [O Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) permite aos clientes Dimensionar seus aplicativos e criar disponibilidade elevada para serviços. Balanceador de carga dá suporte a cenários de entrada e de saída e fornece um débito elevado, de baixa latência e aumenta até milhões de fluxos para todas as aplicações TCP e UDP.

### <a name="data-in-transit"></a>Dados em trânsito

Azure encripta todas as comunicações de e para os datacenters do Azure por predefinição. A ocorrência de todas as transações para o armazenamento do Azure através do portal do Azure através de HTTPS.

### <a name="data-at-rest"></a>Dados inativos

A arquitetura protege os dados em descanso através da encriptação, a auditoria de base de dados e outras medidas.

**O armazenamento do Azure**: Para atender aos dados criptografados em requisitos de rest, todos os [armazenamento do Azure](https://azure.microsoft.com/services/storage/) utiliza [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Isto ajuda a proteger e salvaguardar os dados para oferecer suporte a compromissos de segurança organizacional e requisitos de conformidade definidos pelo NHS Digital.

**Azure Disk Encryption**: [O Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) aproveita a funcionalidade BitLocker do Windows para fornecer encriptação de volume para discos de dados. A solução se integra com o Azure Key Vault para ajudar a controlar e gerir as chaves de encriptação de disco.

**Base de dados SQL do Azure**: A instância de base de dados do Azure SQL usa as seguintes medidas de segurança da base de dados:

- [Autenticação do Active Directory e a autorização](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) permite a gestão de identidades de utilizadores de base de dados e outros serviços da Microsoft num local central.
- [A auditoria de base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) faixas de base de dados eventos e escreve-os para uma auditoria registo numa conta de armazenamento do Azure.
- Base de dados SQL do Azure está configurado para utilizar [encriptação de dados transparente](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), que realiza a encriptação em tempo real e a desencriptação da base de dados, cópias de segurança associadas e ficheiros de registo de transação para proteger as informações em rest. Encriptação de dados transparente fornece garantia de que são armazenados os dados não tiver sido sujeita a acesso não autorizado.
- [Regras de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impedir o acesso de todos os servidores de base de dados até que sejam concedidas as permissões apropriadas. A firewall concede acesso a bases de dados com base no endereço IP de origem de cada pedido.
- [Deteção de ameaças SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) permite a deteção e resposta a ameaças potenciais à medida que ocorrem ao fornecer alertas de segurança para atividades suspeitas da base de dados, potenciais vulnerabilidades, ataques de injeção de SQL e acesso de base de dados anómalas padrões.
- [Encriptados colunas](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) Certifique-se de que os dados confidenciais nunca é apresentada como texto não criptografado dentro do sistema de base de dados. Depois de ativar a encriptação de dados, apenas as aplicações de cliente ou servidores de aplicações com acesso às chaves podem aceder aos dados de texto sem formatação.
- [Máscara de dados dinâmicos da base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) limita a exposição de dados confidenciais ao mascará os dados para aplicações ou utilizadores sem privilégios. Máscara de dados dinâmica pode detetar dados potencialmente confidenciais e sugerir as máscaras de apropriado a ser aplicada automaticamente. Isto ajuda a identificar e reduzir o acesso aos dados, de modo que ele não for fechada a base de dados por meio do acesso não autorizado. Os clientes são responsáveis por ajustar as definições de cumprir para seu esquema de base de dados de máscara de dados dinâmicos.

### <a name="identity-management"></a>Gestão de identidades

As seguintes tecnologias fornecem capacidades para gerir o acesso aos dados no ambiente do Azure:

- [O Azure Active Directory](https://azure.microsoft.com/services/active-directory/) é o Microsoft&#39;s multi-inquilino com base na cloud diretório e identidade do serviço de gestão. Todos os utilizadores para esta solução são criados no Azure Active Directory, incluindo os utilizadores que acedem a base de dados do SQL do Azure.
- Autenticação para a aplicação é executada com o Azure Active Directory. Para obter mais informações, consulte [integrar aplicações com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Além disso, a encriptação de coluna de base de dados utiliza o Azure Active Directory para autenticar a aplicação à base de dados do Azure SQL. Para obter mais informações, consulte como [proteger dados confidenciais na base de dados do Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- [Controlo de acesso baseado em função do Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) permite aos administradores definir permissões de acesso detalhado para conceder apenas a quantidade de acesso que os utilizadores precisam realizar seus trabalhos. Em vez de dar a cada permissão de utilizador sem restrições para recursos do Azure, os administradores podem permitir apenas certas ações para aceder aos dados. Acesso à subscrição está limitado ao administrador da subscrição.
- [O Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) permite que os clientes minimizar o número de utilizadores que têm acesso a determinadas informações. Os administradores podem usar o Azure Active Directory Privileged Identity Management para detetar, restringir e monitorizar identidades privilegiadas e respetivo acesso a recursos. Esta funcionalidade também pode ser utilizada para impor acesso administrativo a pedido e just-in-time quando necessário.
- [O Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) Deteta potenciais vulnerabilidades que afetam uma organização&#39;identidades s, configura as respostas automáticas para detetado ações suspeitas relacionadas com uma organização&#39;identidades s e investiga os incidentes suspeitos para tomar as medidas adequadas para resolvê-los.

### <a name="security"></a>Segurança

**Gestão de segredos**: A solução utiliza [do Azure Key Vault](https://azure.microsoft.com/services/key-vault/) para o gerenciamento de chaves e segredos. O Cofre de Chaves do Azure ajuda a salvaguardar as chaves criptográficas e os segredos utilizados pelas aplicações em cloud e pelos serviços. As seguintes capacidades do Azure Key Vault ajudam os clientes a proteger e aceder a esses dados:

- Políticas de acesso avançadas são configuradas de forma necessidade.
- Políticas de acesso do Cofre de chaves são definidas com o mínimo permissões necessárias para chaves e segredos.
- Todas as chaves e segredos no Key Vault tem datas de expiração.
- Todas as chaves no Cofre de chaves estão protegidas por módulos de segurança de hardware especializado. O tipo de chave estiver protegida por um módulo de hardware de segurança chave RSA de 2048 bits.
- Todos os utilizadores e identidades são concedidas as permissões mínimas necessárias usando o controle de acesso baseado em funções.
- Registos de diagnóstico para o Cofre de chaves estão ativados com um período de retenção de, pelo menos, 365 dias.
- Operações de criptografia permitidas para as chaves estão limitadas aos obrigatórios.

**Centro de segurança do Azure**: Com o [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro), esta solução pode centralmente aplicam-se e gerir políticas de segurança em cargas de trabalho, limitar a exposição a ameaças e detetar e responder a ataques. Além disso, o Centro de segurança do Azure acessa as configurações existentes dos serviços do Azure para fornecer a configuração e recomendações de serviço para o ajudar a melhorar a postura de segurança e proteger os dados.

Centro de segurança do Azure utiliza uma variedade de capacidades de deteção para alertar os clientes de possíveis ataques direcionados aos respetivos ambientes. Estes alertas contêm informações valiosas sobre o que acionou o alerta, os recursos afetados e a origem do ataque. Centro de segurança do Azure tem um conjunto de [predefinidos alertas de segurança](https://docs.microsoft.com/azure/security-center/security-center-alerts-type), que são acionados quando uma ameaça ou atividade suspeita tem lugar. [Regras de alerta personalizadas](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) no Centro de segurança do Azure permitem aos clientes definir novos alertas de segurança com base nos dados que já são recolhidos do seu ambiente.

Centro de segurança do Azure fornece alertas de segurança priorizados e incidentes, tornando mais simples detetar e resolver potenciais problemas de segurança. R [relatório de inteligência de ameaças](https://docs.microsoft.com/azure/security-center/security-center-threat-report) é gerado para cada detetado contra ameaças para ajudar as equipes de resposta a incidentes a investigar e resolver ameaças.

**Gateway de aplicação do Azure**: A arquitetura reduz o risco de vulnerabilidades de segurança com um Gateway de aplicação do Azure com uma firewall de aplicações web configurado e o conjunto de regras do OWASP ativada. Capacidades adicionais incluem:

- [SSL de ponto final das](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Ativar [descarga de SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Desativar [TLS versões 1.0 e 1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Firewall de aplicações Web](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (modo de prevenção)
- [Modo de prevenção](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) com o conjunto de regras do OWASP 3.0
- Ativar [registo de diagnósticos](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Sondas de estado de funcionamento personalizados](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Centro de segurança do Azure](https://azure.microsoft.com/services/security-center) e [Assistente do Azure](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) fornecer proteção adicional e notificações. Centro de segurança do Azure também fornece um sistema de reputação.

### <a name="logging-and-auditing"></a>Registro e auditoria

Serviços do Azure extensivamente de registo do sistema e a atividade do utilizador, bem como o estado de funcionamento do sistema:
- **Registos de atividades**: [Registos de atividades](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) fornecem informações sobre as operações executadas em recursos numa subscrição. Registos de atividades podem ajudar a determinar o iniciador de uma operação, hora da ocorrência e o estado.
- **Os registos de diagnóstico**: [Os registos de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) incluem todos os registos emitidos por cada recurso. Estes registos incluem registos de sistema de eventos do Windows, registos de armazenamento do Azure, registos de auditoria do Cofre de chaves e os registos de acesso e de firewall do Gateway de aplicação. Todos os registos de diagnóstico escrevem para uma conta de armazenamento do Azure centralizado e criptografado para arquivamento. O período de retenção é configurável pelo utilizador, cópia de segurança e 730 dias, para atender aos requisitos de retenção de específicas da organização.

**Log Analytics**: Estes registos são consolidados no [do Log Analytics](https://azure.microsoft.com/services/log-analytics/) para processamento, armazenamento e relatórios do dashboard. Depois de recolhidos, os dados são organizados em tabelas separadas para cada tipo de dados, que permite que todos os dados sejam analisados em conjunto, independentemente da respetiva origem. Além disso, o Centro de segurança do Azure integra-se com o Log Analytics que permite aos clientes utilizar consultas do Log Analytics para aceder aos respetivos dados de eventos de segurança e combiná-los com dados de outros serviços.

A seguinte do Log Analytics [soluções de gestão](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) são incluídos como parte desta arquitetura:
-   [Avaliação do Active Directory](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): A solução de verificação de estado de funcionamento do Active Directory avalia o risco e estado de funcionamento dos ambientes de servidor num intervalo regular e fornece uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
- [Avaliação do SQL](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): A solução de verificação de estado de funcionamento do SQL avalia o risco e estado de funcionamento dos ambientes de servidor num intervalo regular e fornece aos clientes uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
- [Estado de funcionamento do agente](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): A solução de estado de funcionamento do agente relata o número de agentes é implementado e a distribuição geográfica, como número de agentes que não respondem e o número de agentes que está a enviar dados operacionais.
-   [Log Analytics da atividade](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): A solução Log Analytics da atividade auxilia com uma análise de registos de atividade do Azure em todas as subscrições do Azure para um cliente.

**A automatização do Azure**: [A automatização do Azure](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) armazena, executa e gere runbooks. Nesta solução, runbooks ajudam a recolher registos de base de dados do Azure SQL. A automação [controlo de alterações](https://docs.microsoft.com/azure/automation/automation-change-tracking) solução permite aos clientes identificar facilmente as alterações no ambiente.

**O Azure Monitor**: [O Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) ajuda os usuários a acompanhar o desempenho, manter a segurança e identificar tendências, permitindo que as organizações de auditoria, criar alertas e arquivar dados, incluindo o controlo de chamadas à API em seus recursos do Azure.

## <a name="threat-model"></a>Modelo de risco

O diagrama de fluxo de dados para esta arquitetura de referência está disponível para [transferir](https://aka.ms/uknhs-paaswa-tm) ou podem ser encontradas abaixo. Esse modelo pode ajudar os clientes a compreender os pontos do potencial risco de na infraestrutura de sistema ao fazer modificações.

![Aplicação Web de PaaS para o modelo de risco do Reino Unido NHS](images/uknhs-paaswa-threat-model.png?raw=true "aplicativo da Web de PaaS para o modelo de risco NHS do Reino Unido")

## <a name="compliance-documentation"></a>Documentação de conformidade

O [Azure no Security and Compliance Blueprint – matriz de responsabilidade do Reino Unido NHS cliente](https://aka.ms/uknhs-crm) apresenta uma lista de todos os requisitos de NHS do Reino Unido. Essa matriz se a implementação de cada princípio é da responsabilidade da Microsoft, o cliente, os detalhes ou partilhados entre os dois.

O [Azure no Security and Compliance Blueprint – matriz implementação de aplicação Web do Reino Unido NHS PaaS](https://aka.ms/uknhs-paaswa-cim) fornece informações sobre quais NHS do Reino Unido requisitos são resolvidos à arquitetura do aplicativo web PaaS, incluindo detalhada descrições de como a implementação cumpre os requisitos de cada princípio coberto.

## <a name="guidance-and-recommendations"></a>Orientações e recomendações

### <a name="vpn-and-expressroute"></a>ExpressRoute e VPN

Um túnel VPN seguro ou [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) tem de ser configurado com segurança estabelecer uma ligação para os recursos implementados como parte desta arquitetura de referência de aplicação de web de PaaS. Configurando adequadamente uma VPN ou ExpressRoute, os clientes podem adicionar uma camada de proteção para dados em trânsito.

Ao implementar um túnel VPN seguro com o Azure, é possível criar uma ligação privada virtual entre uma rede no local e uma rede virtual do Azure. Esta ligação ocorre através da Internet e permite que os clientes de forma segura &quot;túnel&quot; informações dentro de uma ligação encriptada entre o cliente&#39;rede s e o Azure. VPN site a site é uma tecnologia madura e segura que tenha sido implementada por empresas de todos os tamanhos há décadas. O [modo de encapsulamento IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) é utilizada nesta opção como um mecanismo de criptografia.

Porque o tráfego de túnel VPN atravessa a Internet com uma VPN de site a site, a Microsoft oferece a opção de ligação de outra, ainda mais seguro. O Azure ExpressRoute é uma WAN dedicada ligação entre o Azure e uma localização no local ou um fornecedor de alojamento do Exchange. Como as ligações ExpressRoute não entram na Internet, estas ligações oferecem mais fiabilidade, velocidades mais rápidas, latências mais baixas e maior segurança do que as ligações normais pela Internet. Além disso, como se trata de uma conexão direta de cliente&#39;fornecedor de telecomunicação s, os dados não percorre a Internet e, portanto, não são expostos ao mesmo.

Melhores práticas para implementar uma rede híbrida segura que expande uma rede no local para o Azure são [disponível](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Exclusão de Responsabilidade

- Este documento é apenas para fins informativos. A MICROSOFT FAZ NÃO OFERECE QUAISQUER GARANTIAS, EXPRESSAS, IMPLÍCITAS OU ESTATUTÁRIAS QUANTO ÀS INFORMAÇÕES CONTIDAS NESTE DOCUMENTO. Este documento é fornecido &quot;como-é.&quot; Informações e opiniões expressados neste documento, incluindo URLs e outras referências de Web site da Internet, podem ser alteradas sem aviso prévio. Os clientes a ler este documento assume o risco de usá-lo.
- Este documento não fornece aos clientes quaisquer direitos de propriedade intelectual em qualquer produto da Microsoft ou soluções.
- Os clientes podem copiar e utilizar este documento para efeitos de referência interna.
- Algumas recomendações neste documento pode resultar em maior de dados, a rede ou a utilização de recursos de computação no Azure e pode aumentar um cliente&#39;s licença do Azure ou os custos de subscrição.
- Esta arquitetura deve ser usado como uma base para os clientes que se ajustar às suas necessidades específicas e não devem ser utilizada como-está num ambiente de produção.
- Este documento é desenvolvido como uma referência e não deve ser usado para definir todos os meios pelos quais um cliente pode atender aos requisitos de conformidade e normas. Os clientes procuram legal suporte da sua organização em implementações do cliente aprovadas.

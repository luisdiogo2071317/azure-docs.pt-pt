---
title: Segurança do Azure e conformidade Blueprint - análise para FedRAMP
description: Segurança do Azure e conformidade Blueprint - análise para FedRAMP
services: security
author: jomolesk
ms.assetid: b4675715-668e-4557-9c1c-698aabf62ceb
ms.service: security
ms.topic: article
ms.date: 05/02/2018
ms.author: jomolesk
ms.openlocfilehash: db9e49cc4dc02b6864bee2dc4b73ff3c085f5380
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="azure-security-and-compliance-blueprint-analytics-for-fedramp"></a>Blueprint de conformidade e segurança do Azure: análise para FedRAMP

## <a name="overview"></a>Descrição geral

O [risco Federal e o programa de gestão de autorização (FedRAMP)](https://www.fedramp.gov/) é um programa de toda a government dos Estados Unidos que fornece uma abordagem padronizada para avaliação de segurança, autorização e monitorização contínua para a nuvem os produtos e serviços. Esta segurança do Azure e a conformidade Blueprint fornece orientações para como fornecer uma arquitetura de análise do Microsoft Azure que ajuda a implementar um subconjunto dos controlos FedRAMP elevado. Esta solução fornece orientações para a implementação e configuração de recursos do Azure para uma arquitetura de referência comum, que demonstra formas em que os clientes podem cumpre os requisitos específicos de segurança e conformidade e funciona como uma base para os clientes criar e configurar as suas próprias soluções de análise do Azure.

Esta arquitetura de referência, guias de implementação do controlo associado e modelos de ameaça destinam-se para servir como base para os clientes ajustar os seus requisitos específicos e não devem ser utilizados como-é num ambiente de produção. Implementar uma aplicação para este ambiente sem modificação é insuficiente para completamente cumprir os requisitos de linha de base FedRAMP elevado. Tenha em atenção o seguinte:
- A arquitetura fornece uma linha de base para ajudar os clientes implementar cargas de trabalho no Azure de forma FedRAMP compatíveis.
- Os clientes são responsáveis para realizar a segurança adequada e avaliações de conformidade de qualquer solução incorporada com esta arquitetura, como os requisitos podem variar com base nas especificações de implementação de cada cliente.

## <a name="architecture-diagram-and-components"></a>Diagrama da arquitetura e componentes

Esta solução fornece uma plataforma de análise no qual os clientes podem criar as suas próprias ferramentas de análise. Arquitetura de referência descreve um caso de utilização genérico onde os clientes introduzir dados através da importação de dados em massa, o administrador de dados do SQL ou através de atualizações de dados operacionais através de um utilizador operacional. Ambos incorporate fluxos de trabalho [das funções do Azure](https://docs.microsoft.com/azure/azure-functions/functions-overview) para importar dados para a base de dados do SQL Server. As funções do Azure tem de ser configuradas pelo cliente através do Portal do Azure para processar as tarefas de importação exclusivas para do cada cliente seus próprio requisitos de análise.

Microsoft Azure oferece uma variedade de serviços de análises e relatórios para o cliente; No entanto, esta solução incorpora Azure Analysis Services em conjunto com a base de dados do Azure SQL rapidamente procurar através de dados e fornecer resultados mais rápidos através de mais inteligente modelação de dados de cliente. Serviços de análise do Azure é um formulário de aprendizagem que se destina a aumentar a velocidades de consulta ao detetar novas relações entre conjuntos de dados. Depois dos dados tem sido preparados através de várias funções estatísticas, até 7 consulta adicionais agrupamentos (8 total, incluindo o servidor de cliente) podem ser sincronizados com os mesmos modelos em tabela para propagar-se a carga de trabalho de consulta e reduzir os tempos de resposta.

Para análise avançada e relatórios, bases de dados SQL pode ser configurado com índices columnstore. Serviços de análise do Azure e bases de dados SQL podem ser escalado para cima ou para baixo ou encerradas completamente em resposta a utilização de cliente. Todo o tráfego SQL é encriptado com SSL através da inclusão de certificados autoassinados. Como melhor prática, o Azure recomenda a utilização de uma autoridade de certificação fidedigna para a segurança avançada.

Depois de dados são carregados para a base de dados do SQL do Azure e preparados pelo Azure Analysis Services, é digested pelo utilizador operacional e a administração de dados do SQL com o Power BI. Power BI apresenta dados intuitivo e obtém em conjunto informações entre vários conjuntos de dados para desenhar conhecimentos aprofundados maior. O nível elevado de adaptability e integração fácil com a SQL Database do Azure garante que os clientes podem configurá-lo para processar um grande número de cenários conforme exigido pelas suas necessidades de negócio.

A solução completa é criada após um Storage do Azure que configurar os clientes de conta do Portal do Azure. Armazenamento do Azure encripta todos os dados com a encriptação do serviço de armazenamento para manter a confidencialidade dos dados inativos.  Armazenamento redundante geográfica (GRS) assegura que um evento adverso no Centro de dados principal do cliente não resultará na perda de dados como uma segunda cópia será armazenada num local separado centenas de quilómetros ausente.

Para maior segurança, esta arquitetura gere os recursos com o Azure Active Directory e o Cofre de chaves do Azure. Estado de funcionamento do sistema é monitorizado através do Operations Management Suite (OMS) e Monitor do Azure. Os clientes configurar ambos os serviços de monitorização para capturar os registos e apresentar o estado de funcionamento do sistema num dashboard único, facilmente navegáveis dos.

Base de dados SQL do Azure normalmente é gerido através do SQL Server Management Studio (SSMS), que é executado a partir de uma máquina local configurada para aceder à base de dados do SQL do Azure através de uma ligação VPN ou ExpressRoute segura. **Azure recomenda configurar uma ligação VPN ou Azure ExpressRoute para importação de dados e de gestão para o grupo de recursos de arquitetura de referência.**

![Análise de diagrama de arquitetura de referência FedRAMP](images/fedramp-analytics-reference-architecture.png?raw=true "análise para FedRAMP diagrama da arquitetura de referência")

### <a name="roles"></a>Funções
A análise blueprint descreve um cenário com três tipos de utilizador geral: o utilizador operacional, o administrador de dados do SQL e o engenheiro de sistemas. Azure baseada em funções controlo de acesso (RBAC) permite a implementação de gestão de acesso preciso através de funções personalizadas incorporadas. Estão disponíveis para configurar recursos [controlo de acesso baseado em funções](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) definido que estipule e implementar [funções predefinidas](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles).

#### <a name="systems-engineer"></a>Engenheiro de sistemas
O engenheiro de sistemas proprietária da subscrição do cliente para o Azure e configura a implementação da solução através do Portal do Azure.

#### <a name="sqldata-administrator"></a>Administrador de dados do SQL
O administrador de dados do SQL estabelece a função de importação de dados em massa e a função de atualização de dados operacionais para carregar para a base de dados SQL do Azure. O administrador de dados do SQL não é responsável por quaisquer atualizações de dados operacional na base de dados, mas poderão ver os dados através do Power BI.

#### <a name="operational-user"></a>Utilizador operacional
O utilizador operacional atualiza regularmente os dados e possui a geração de dados diárias. O utilizador operacional também será interpretar os resultados através do Power BI.

### <a name="azure-services"></a>Serviços do Azure

Esta solução utiliza os seguintes serviços do Azure. Os detalhes da arquitetura de implementação estão no [arquitetura de implementação](#deployment-architecture) secção.
- Funções do Azure
- Base de Dados SQL do Azure
- Serviço de análise do Azure
- Azure Active Directory
- Azure Key Vault
- OMS
- Azure Monitor
- Storage do Azure
- Gateway do ExpressRoute/VPN
- Dashboard do Power BI

## <a name="deployment-architecture"></a>Arquitetura de implementação
A secção seguinte descreve em detalhe os elementos de desenvolvimento e implementação.

![texto alternativo](images/fedramp-analytics-components.png?raw=true "análises do diagrama de componentes FedRAMP")

**As funções do Azure**: [das funções do Azure](https://docs.microsoft.com/azure/azure-functions/functions-overview) são as soluções para execução de pequenos blocos de código na nuvem através de linguagens de programação mais. As funções nesta solução integram com o Storage do Azure para automaticamente extrair dados de cliente para a nuvem, o facilitar o início integração com outros serviços do Azure. As funções que são facilmente dimensionáveis e apenas cobrado um custo quando estiverem a executar.

**Serviço de análise do Azure**: [Azure Analysis Service](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview) fornece modelação de dados da empresa e a integração com serviços da plataforma de dados do Azure. Serviço de análise do Azure acelera navegar pelas quantidades enormes de dados através da combinação de dados de várias origens para um modelo de dados único.

**Power BI**: [Power BI](https://docs.microsoft.com/power-bi/service-azure-and-power-bi) fornece análises e funcionalidade de relatórios para os clientes a tentar obter conhecimentos aprofundados maior fora os seus esforços de processamento de dados.

### <a name="networking"></a>Redes
**Grupos de segurança de rede**: [NSGs](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) estão configurados para gerir o tráfego direcionado em serviços e recursos implementados. Grupos de segurança de rede estão definidos como um esquema de negar por predefinição e permitir apenas o tráfego que está incluído na lista previamente configurada de controlo de acesso (ACL).

Cada um dos NSGs ter específicas portas e protocolos abrir para que a solução possam funcionar corretamente e em segurança. Além disso, as configurações seguintes estão ativadas para cada NSG:
  - [Os registos de diagnóstico e de eventos](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) estão ativadas e armazenados numa conta do storage
  - OMS [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics) está ligado a registos de diagnóstico o NSG.

### <a name="data-at-rest"></a>Dados inativos
A arquitetura protege os dados Inativos através de encriptação, base de dados de auditoria e de outras medidas.

**Replicação de dados** Azure Government tem duas opções para [replicação de dados](https://docs.microsoft.com/azure/storage/common/storage-redundancy):
 - A predefinição para replicação de dados é **armazenamento Georredundante (GRS)**, no modo assíncrono que armazena dados de cliente num centro de dados separadas fora da região primária. Isto garante uma recuperação de dados de um evento de perda total para o Centro de dados principal.
 - **Localmente redundante armazenamento (LRS)** em alternativa pode ser configurada através da conta de armazenamento do Azure. LRS replica os dados dentro de uma unidade de escala de armazenamento, que está alojado na mesma região na qual o cliente cria a respetiva conta. Todos os dados são replicados em simultâneo, garantindo que não existem dados de cópia de segurança há perdidos de uma falha de unidade de escala de armazenamento primário.

**Armazenamento do Azure** para satisfazer os dados encriptados em requisitos de rest, todos os serviços implementados neste partido da arquitetura de referência [Storage do Azure](https://azure.microsoft.com/services/storage/), que armazena os dados com [encriptação do serviço de armazenamento](https://docs.microsoft.com/azure/storage/storage-service-encryption).

**Azure Disk Encryption**
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) aproveita a funcionalidade de BitLocker do Windows para fornecer a encriptação de volume para o SO e discos de dados. A solução integra-se com o Cofre de chaves do Azure para ajudar a controlar e gerir as chaves de encriptação de disco.

**Base de dados SQL do Azure** instância de base de dados de SQL do Azure utiliza as seguintes medidas de segurança da base de dados:
-   [Autorização e autenticação AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) permite a gestão de identidades de utilizadores de base de dados e outros serviços Microsoft numa localização central.
-   [Auditoria de base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) controla da base de dados eventos e escreve-los para uma auditoria iniciem sessão uma conta de armazenamento do Azure.
-   Base de dados do SQL Server está configurado para utilizar [encriptação de dados transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), que efetua a encriptação em tempo real e a desencriptação de dados e registo de ficheiros para proteger informações inativos. TDE fornece assurance que armazenados os dados não foi sujeito acesso não autorizado.
-   [Regras de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impedir o acesso de todos os servidores de base de dados até que são concedidas permissões adequadas. A firewall concede acesso a bases de dados com base no endereço IP de origem de cada pedido.
-   [A deteção de ameaças do SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) permite a deteção e a resposta a potenciais ameaças à medida que ocorrem fornecendo alertas de segurança para acesso de base de dados anómalas, ataques de injeção de SQL, potenciais vulnerabilidades e atividades suspeitas da base de dados padrões.
-   [Sempre encriptado colunas](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) Certifique-se de que os dados confidenciais nunca é apresentada como texto não encriptado no interior do sistema de base de dados. Depois de ativar a encriptação de dados, apenas as aplicações de cliente ou servidores de aplicações com o acesso às chaves podem aceder aos dados de texto simples.
-   [Máscara de dados dinâmicos da base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) podem ser efetuados após implementa a arquitetura de referência. Os clientes terão de ajustar as definições de aderir ao respetiva esquema de base de dados de máscara de dados dinâmicos.

### <a name="logging-and-audit"></a>Auditoria e registo
[Monitor do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) gera uma apresentação de segurança de todos os dados, incluindo atividade registos, métricas e dados de diagnóstico, ativar os clientes criem uma visão geral do Estado de funcionamento do sistema de monitorização.  
[Operations Management Suite (OMS)](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) fornece extenso registo de atividade de utilizador e de sistema, bem como o estado de funcionamento do sistema. O OMS [Log Analytics](https://azure.microsoft.com/services/log-analytics/) solução recolhe e analisa os dados gerados pelos recursos no Azure e ambientes no local.
- **Registos de atividade**: [registos de atividade](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) fornecem informações aprofundadas operações executadas nos recursos numa subscrição.
- **Os registos de diagnóstico**: [registos de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) incluem todos os registos emitidos por cada recurso. Estes registos incluem registos de sistema de eventos do Windows e armazenamento de Blobs do Azure, tabelas e registos de fila.
- **Os registos de firewall**: O Gateway de aplicação fornece completa diagnóstico e aceder a registos. Estão disponíveis para os recursos de Gateway de aplicação com capacidade WAF registos de firewall.
- **Inicie sessão arquivo**: todos os registos de diagnóstico de escrita para uma conta de armazenamento do Azure centralizada e encriptados para arquivo com um período de retenção definido de 2 dias. Estes registos ligar ao Log Analytics do Azure para o processamento, armazenamento e relatórios do dashboard.

Além disso, as seguintes soluções OMS são incluídas como parte desta arquitetura:
-   [A automatização do Azure](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): solução a automatização do Azure armazena, é executado e gere runbooks.
-   [Auditoria e segurança](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): auditoria e segurança dashboard fornece uma alto nível aprofundadas sobre o estado de segurança de recursos, fornecendo as métricas em domínios de segurança, problemas relevantes, deteções, ameaças e consultas de segurança comuns.
-   [Avaliação do SQL Server](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): O estado de funcionamento de SQL Consulte solução avalia o risco e o estado de funcionamento de ambientes de servidor num intervalo regular e fornece aos clientes com uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
-   [Os registos de atividade do Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): solução de análise de registos de atividade o auxilia com a análise de registos a atividade do Azure em todas as subscrições do Azure para um cliente.

### <a name="identity-management"></a>Gestão de identidades
-   Autenticação para a aplicação é executada a utilizar o Azure AD. Para obter mais informações, consulte [integrar aplicações com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Além disso, a encriptação de coluna de base de dados utiliza o Azure AD para autenticar a aplicação a SQL Database do Azure. Para obter mais informações, consulte como [proteger dados sensíveis na base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) Deteta potenciais vulnerabilidades que afetam as identidades de uma organização, configura as respostas automatizadas detetadas ações suspeitas relacionada com identidades da organização e investiga incidentes suspeitas tome medidas adequadas resolvê-los.
-   [Azure baseada em funções controlo de acesso (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) permite concentra-se a gestão de acesso para o Azure. Acesso de subscrição está limitado ao administrador da subscrição.

Para saber mais sobre como utilizar as funcionalidades de segurança da base de dados do Azure SQL, consulte o [aplicação de demonstração do Contoso Clinic](https://github.com/Microsoft/azure-sql-security-sample) exemplo.

### <a name="security"></a>Segurança
**Gestão de segredos**: A solução utiliza [Cofre de chaves do Azure](https://azure.microsoft.com/services/key-vault/) para a gestão de chaves e segredos. O Cofre de Chaves do Azure ajuda a salvaguardar as chaves criptográficas e os segredos utilizados pelas aplicações em cloud e pelos serviços.

## <a name="guidance-and-recommendations"></a>Orientações e recomendações

### <a name="expressroute-and-vpn"></a>ExpressRoute e de VPN
[ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou de um túnel seguro de VPN tem de ser configurado com segurança estabelecer uma ligação aos recursos implementada como parte desta arquitetura de referência de análise de dados. Como as ligações do ExpressRoute não passam para a Internet, estas ligações oferecem mais fiabilidade, velocidades mais rápidas, latências inferiores e uma maior segurança ligações típicas através da Internet. Configurando adequadamente ExpressRoute ou uma VPN, os clientes podem adicionar uma camada de proteção de dados em trânsito.

### <a name="azure-active-directory-setup"></a>Configuração do Active Directory do Azure
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) é essencial para gerir a implementação e aprovisionamento de acesso ao pessoal interagir com o ambiente. Um Windows Server Active Directory existente, pode ser integrado com o AAD no [quatro cliques](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Os clientes também podem juntar a infraestrutura do Active Directory implementada (controladores de domínio) para um AAD existente ao fazer a infraestrutura do Active Directory implementada um subdomínio de uma floresta do AAD.

### <a name="additional-services"></a>Serviços adicionais
#### <a name="iaas---vm-vonsiderations"></a>IaaS - vonsiderations VM
Esta solução PaaS incorporadas quaisquer VMs de IaaS do Azure. Um cliente pode criar uma VM do Azure para executar muitos destes serviços PaaS. Neste caso, funcionalidades específicas e serviços para a continuidade do negócio e OMS podem ser aproveitados:

##### <a name="business-continuity"></a>Continuidade do negócio
- **Elevada disponibilidade**: cargas de trabalho do servidor são agrupadas num [do conjunto de disponibilidade](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) para ajudar a garantir a elevada disponibilidade das máquinas virtuais no Azure. Pelo menos uma máquina virtual está disponível durante um evento de manutenção planeada ou, reunião de 99,95% SLA do Azure.

- **O Cofre de serviços de recuperação**: O [cofre dos serviços de recuperação](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) aloja dados de cópia de segurança e protege todas as configurações de máquinas virtuais do Azure nesta arquitetura. Com um cofre de serviços de recuperação, os clientes restaurar ficheiros e pastas a partir de uma VM do IaaS sem restaurar toda a VM, permitindo tempos de restauro mais rápidos.

##### <a name="oms"></a>OMS
-   [Avaliação do AD](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): O Active Directory estado de funcionamento de verificação solução avalia o risco e o estado de funcionamento de ambientes de servidor num intervalo regular e fornece uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
-   [Avaliação de antimalware](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): solução Antimalware a relatórios de estado de proteção, software maligno e ameaças.
-   [Gestão de atualizações](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): solução de gestão de atualizações a permite a gestão de cliente de atualizações de segurança do sistema operativo, incluindo o processo de instalação de atualizações necessárias e o estado de atualizações disponíveis.
-   [Estado de funcionamento do agente](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): solução do estado de funcionamento do agente comunica quantos agentes implementados e respetiva distribuição geográfica, bem como quantos agentes que estão a responder e o número de agentes que submeter dados operacionais.
-   [Registo de alterações](https://docs.microsoft.com/azure/automation/automation-change-tracking): controlo de alterações a solução permite aos clientes identificar facilmente as alterações no ambiente.

##### <a name="security"></a>Segurança
- **Proteção contra software maligno**: [Antimalware da Microsoft](https://docs.microsoft.com/azure/security/azure-security-antimalware) para máquinas virtuais fornece a capacidade de proteção em tempo real que ajuda a identifica e remove vírus, spyware e outro software malicioso, com alertas configuráveis ao conhecido software malicioso ou indesejável, tentar instalar ou executar em máquinas virtuais protegidas.
- **Aplicar o patch gestão**: implementadas como parte desta arquitetura de referência de máquinas virtuais do Windows estão configuradas por predefinição para receber as atualizações automáticas do serviço de atualização do Windows. Esta solução também inclui o OMS [da automatização do Azure](https://docs.microsoft.com/azure/automation/automation-intro) serviço através do qual implementações atualizadas podem ser criadas para máquinas virtuais de patch quando necessário.

#### <a name="azure-commercial"></a>Comercial do Azure
Apesar desta arquitetura de análise de dados, não se destina a ser implementação para o [Azure comerciais](https://azure.microsoft.com/overview/what-is-azure/) ambiente, objetivos semelhantes podem ser conseguidos através dos serviços descritos nesta arquitetura de referência, bem como serviços adicionais disponíveis apenas no ambiente do Azure comercial. Tenha em atenção que a Azure comerciais mantém um FedRAMP JAB P-/ATO ao nível de impacto moderado, permitindo agências governamentais e parceiros para implementar a informações confidenciais moderately para a nuvem tirar partido do ambiente do Azure comercial.

Comercial Azure oferece uma ampla variedade de serviços de análise para extrair informações fora de grandes quantidades de dados:
-   [Azure Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio/what-is-ml-studio), um componente do [Cortana Intelligence Suite](https://azure.microsoft.com/overview/ai-platform/), desenvolvidas pela organização um modelo de Análise Preditiva a partir de um ou mais origens de dados. Funções estatísticas são utilizadas ao longo de vários iterações para gerar um modelo eficiente que aplicações como o Power BI, em seguida, podem consumir.
-   [O Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) permite que a captura de dados de qualquer tamanho, tipo e velocidade de ingestão num único local para análises exploratórias e operacionais. O Azure Data Lake Store é compatível com a maioria dos componentes de origem do ecossistema do Hadoop e é integrado corretamente com outros serviços do Azure.

## <a name="threat-model"></a>Modelo de ameaça

O diagrama de fluxo de dados (DFD) para esta arquitetura de referência está disponível para [transferir](https://aka.ms/blueprintanalyticsthreatmodel) ou podem ser encontrados abaixo:

## <a name="compliance-documentation"></a>Documentação de conformidade
O [segurança do Azure e conformidade Blueprint – FedRAMP elevada cliente responsabilidade matriz](https://aka.ms/blueprinthighcrm) apresenta uma lista de todos os controlos de segurança necessários para a linha de base FedRAMP elevado. Da mesma forma, o [segurança do Azure e conformidade Blueprint – FedRAMP moderada cliente responsabilidade matriz](https://aka.ms/blueprintanalyticscimmod) apresenta uma lista de todos os controlos de segurança necessários para a linha de base FedRAMP moderada. Os documentos de detalhe se a implementação de cada controlo é da responsabilidade do Microsoft, o cliente, ou partilhados entre os dois.

O [segurança do Azure e conformidade Blueprint - FedRAMP elevada controlo implementação matriz](https://aka.ms/blueprintanalyticscimhigh) e o [segurança do Azure e conformidade Blueprint - FedRAMP moderada controlo implementação matriz](https://aka.ms/blueprintanalyticscimmod) fornecer informações no qual os controlos estão abrangidos pela arquitetura de análise para cada linha de base do FedRAMP, incluindo como a implementação cumpre os requisitos de cada controlo coberto obter descrições detalhadas.

## <a name="disclaimer"></a>Exclusão de Responsabilidade

 - Este documento destina-se apenas a fins informativos. MICROSOFT FAZ COM QUE NÃO OFERECE NENHUMA GARANTIA, EXPRESSAS, IMPLÍCITA OU ESTATUTARIAMENTE AS INFORMAÇÕES NESTE DOCUMENTO. Este documento é fornecido "como-está." As informações e opiniões expressados neste documento, incluindo URLs e outras referências de Web site da Internet, podem ser alteradas sem aviso prévio. Os clientes ler este documento da sua responsabilidade utilizá-la.
 - Este documento não fornece aos clientes com quaisquer direitos legais a nenhuma propriedade intelectual de qualquer produto da Microsoft ou de soluções.
 - Os clientes podem copiar e utilizar este documento para efeitos de referência interno.
 - Algumas recomendações neste documento poderão resultar numa maior dados, a rede ou a utilização de recursos de computação no Azure e podem aumentar os custos de licenciamento ou de subscrição do Azure do cliente.
 - Esta arquitetura destina-se para servir como base para os clientes ajustar os seus requisitos específicos e não devem ser utilizada como-é num ambiente de produção.
 - Este documento é desenvolvido como uma referência e não deve ser utilizado para definir todas as formas através do qual um cliente pode cumpre os requisitos de conformidade específico e regulamentos. Os clientes devem procurar suporte legal da respetiva organização em implementações de cliente aprovados.

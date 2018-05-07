---
title: Segurança do Azure e conformidade Blueprint - do armazém de dados para a automatização de FedRAMP
description: Segurança do Azure e conformidade Blueprint - do armazém de dados para a automatização de FedRAMP
services: security
author: jomolesk
ms.assetid: 834d1ff6-8369-455f-b052-1ef301e3d7e6
ms.service: security
ms.topic: article
ms.date: 05/02/2018
ms.author: jomolesk
ms.openlocfilehash: 110ce131286f437e051dd859eb4d2baa29f106f6
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="azure-security-and-compliance-blueprint-data-warehouse-for-fedramp-automation"></a>Segurança do Azure e conformidade Blueprint: armazém de dados para a automatização de FedRAMP

## <a name="overview"></a>Descrição geral

O [risco Federal e o programa de gestão de autorização (FedRAMP)](https://www.fedramp.gov/) é um programa de toda a government dos Estados Unidos que fornece uma abordagem padronizada para avaliação de segurança, autorização e monitorização contínua para a nuvem os produtos e serviços. Esta segurança do Azure e a conformidade Blueprint fornece orientações para como fornecer uma arquitetura do Microsoft Azure data warehouse que ajuda a implementar um subconjunto dos controlos FedRAMP elevado. Esta solução fornece orientações para a implementação e configuração de recursos do Azure para uma arquitetura de referência comum, que demonstra formas em que os clientes podem cumpre os requisitos específicos de segurança e conformidade e funciona como uma base para os clientes criar e configurar as suas próprias soluções de armazém de dados no Azure.

Esta arquitetura de referência, guias de implementação do controlo associado e modelos de ameaça destinam-se para servir como base para os clientes ajustar os seus requisitos específicos e não devem ser utilizados como-é num ambiente de produção. Implementar uma aplicação para este ambiente sem modificação é insuficiente para completamente cumprir os requisitos de linha de base FedRAMP elevado. Tenha em atenção o seguinte:
- A arquitetura fornece uma linha de base para ajudar os clientes implementar cargas de trabalho no Azure de forma FedRAMP compatíveis.
- Os clientes são responsáveis para realizar a segurança adequada e avaliações de conformidade de qualquer solução incorporada com esta arquitetura, como os requisitos podem variar com base nas especificações de implementação de cada cliente.

## <a name="architecture-diagram-and-components"></a>Diagrama da arquitetura e componentes

Esta solução fornece uma arquitetura de referência do armazém de dados que implementa um armazém de dados de nuvem de elevado desempenho e seguro. Existem duas camadas de dados separada nesta arquitetura: um onde os dados são importados, armazenados e testado dentro de um ambiente em cluster do SQL Server e outra para o armazém de dados SQL do Azure onde os dados são carregados utilizando uma ferramenta ETL (por exemplo, [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase)Consultas de T-SQL) para processamento. Depois dos dados são armazenados no Azure SQL Data Warehouse, análise pode executar uma grande escala.

Microsoft Azure oferece uma variedade de serviços de análises e relatórios para o cliente. Esta solução inclui o SQL Server Reporting Services (SSRS) para a criação rápida de relatórios do armazém de dados SQL do Azure. Todo o tráfego SQL é encriptado com SSL através da inclusão de certificados autoassinados. Como melhor prática, o Azure recomenda a utilização de uma autoridade de certificação fidedigna para a segurança avançada.

Dados no armazém de dados SQL do Azure são armazenados nas tabelas relacionais com armazenamento columnar, um formato que reduz significativamente os custos de armazenamento de dados ao melhorar o desempenho da consulta.  Dependendo dos requisitos de utilização, os recursos de computação do Azure SQL Data Warehouse podem ser escalados para cima ou para baixo ou encerrados completamente se existem não existem processos Active Directory necessidade de recursos de computação.

Um balanceador de carga do SQL Server gere tráfego SQL, assegurando um desempenho elevado. Todas as máquinas virtuais nesta arquitetura de referência implementar um conjunto de disponibilidade com instâncias do SQL Server configuradas num grupo de Disponibilidade AlwaysOn para capacidades de elevada disponibilidade e recuperação após desastre.

Esta arquitetura de referência do armazém de dados também inclui uma camada do Active Directory (AD) para a gestão de recursos dentro da arquitetura. A sub-rede do Active Directory permite a fácil adoção sob uma estrutura de floresta de AD maior, permitindo operação contínua do ambiente, mesmo quando o acesso à floresta superior não está disponível. Todas as máquinas virtuais estão associados a um domínio para o escalão de Active Directory e utilizar políticas de grupo do Active Directory para impor a segurança e conformidade configurações ao nível do sistema operativo.

Uma máquina virtual funciona como um anfitrião de bastion de gestão, fornecendo uma ligação segura para os administradores de recursos de acesso implementado. Carrega os dados para a área de transição através deste anfitrião de bastion de gestão. **Azure recomenda configurar uma ligação VPN ou Azure ExpressRoute para importação de dados e de gestão para a sub-rede de arquitetura de referência.**

![Armazém de dados para o diagrama de arquitetura de referência FedRAMP](images/fedramp-datawarehouse-architecture.png?raw=true "do armazém de dados para o diagrama de arquitetura de referência FedRAMP")

Esta solução utiliza os seguintes serviços do Azure. Os detalhes da arquitetura de implementação estão no [arquitetura de implementação](#deployment-architecture) secção.

Máquinas Virtuais do Azure
-   (1) anfitrião de bastion
-   (2) controlador de domínio o active Directory
-   (2) nó de Cluster de servidor do SQL Server
-   (1) SQL Server servidor testemunha

Conjuntos de Disponibilidade
-   (1) controladores de domínio o active Directory
-   (1) nós de cluster do SQL Server e da testemunha

Rede Virtual
-   (4) sub-redes
-   (4) grupos de segurança de rede

SQL Data Warehouse

SQL Server Reporting Services

Balanceador de carga SQL do Azure

Azure Active Directory

Cofre de serviços de recuperação

Azure Key Vault

Operations Management Suite (OMS)

## <a name="deployment-architecture"></a>Arquitetura de implementação

A secção seguinte descreve em detalhe os elementos de desenvolvimento e implementação.

**O SQL Data Warehouse**: [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) é um armazém de dados de empresa (EDW) que tira partido em massa paralelas processamento (MPP) rapidamente executar consultas complexas em petabytes de dados. Importar dados de grandes para o SQL Data Warehouse com consultas de PolyBase T-SQL simples e utilize potência de MPP para executar a análise de elevado desempenho.

**SQL Server Reporting Services**: [SQL Server Reporting Services](https://docs.microsoft.com/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) permite criação rápida de relatórios com tabelas, gráficos, mapas, medidores, matrizes e mais para o Azure SQL Data Warehouse.

**Anfitrião de bastion**: O anfitrião de bastion for o único ponto de entrada que permite aos utilizadores aceder os recursos implementados neste ambiente. O anfitrião de bastion fornece uma ligação segura a recursos implementados, permitindo apenas tráfego remoto de endereços IP públicos numa lista segura. Para permitir o tráfego de (RDP) de ambiente de trabalho remoto, a origem do tráfego tem de ser definido no grupo de segurança de rede (NSG).

Uma máquina virtual foi criada como um anfitrião de bastion associados a um domínio com as seguintes configurações:
-   [Extensão de antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Extensão do OMS](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Extensão de diagnóstico do Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) com o Cofre de chaves do Azure (respeita Azure Government, PCI DSS, HIPAA e outros requisitos)
-   Um [política de encerramento automático](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) reduzir o consumo de recursos de máquina virtual quando não está em utilização
-   [Proteção de credenciais do Windows Defender](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) ativado para que as credenciais e outros segredos executadas num ambiente protegido que está isolado do sistema operativo em execução

### <a name="virtual-network"></a>Rede virtual
Esta arquitetura de referência define uma rede privada virtual com um espaço de endereços de 10.0.0.0/16.

**Grupos de segurança de rede**: [NSGs](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) conter acesso as listas de controlo (ACLs) que permitem ou negam o tráfego numa VNet. Os NSGs podem ser utilizados para proteger o tráfego de uma sub-rede ou nível VM individuais. Os seguintes NSGs de existir:
  - Um NSG para a camada de dados (Clusters do SQL Server, SQL Server testemunho e Balanceador de carga do SQL Server)
  - Um NSG para o anfitrião de bastion de gestão
  - Um NSG para o Active Directory
  - Um NSG para o SQL Server Reporting Services

Cada um dos NSGs ter específicas portas e protocolos abrir para que a solução possam funcionar corretamente e em segurança. Além disso, as configurações seguintes estão ativadas para cada NSG:
  - [Os registos de diagnóstico e de eventos](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) estão ativadas e armazenados numa conta do storage
  - Análise de registos do OMS está ligado a [diagnóstico do NSG](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Sub-redes**: cada sub-rede está associada a respetiva NSG correspondente.

### <a name="data-at-rest"></a>Dados inativos
A arquitetura protege os dados Inativos através de encriptação, base de dados de auditoria e de outras medidas.

**Armazenamento do Azure** para satisfazer os dados encriptados em requisitos de rest, todos os [Storage do Azure](https://azure.microsoft.com/services/storage/) utiliza [encriptação do serviço de armazenamento](https://docs.microsoft.com/azure/storage/storage-service-encryption).

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

### <a name="business-continuity"></a>Continuidade do negócio
**Elevada disponibilidade**: cargas de trabalho do servidor são agrupadas num [do conjunto de disponibilidade](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) para ajudar a garantir a elevada disponibilidade das máquinas virtuais no Azure. Pelo menos uma máquina virtual está disponível durante um evento de manutenção planeada ou, reunião de 99,95% SLA do Azure.

**O Cofre de serviços de recuperação**: O [cofre dos serviços de recuperação](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) aloja dados de cópia de segurança e protege todas as configurações de máquinas virtuais do Azure nesta arquitetura. Com um cofre de serviços de recuperação, os clientes restaurar ficheiros e pastas a partir de uma VM do IaaS sem restaurar toda a VM, permitindo tempos de restauro mais rápidos.

### <a name="logging-and-audit"></a>Auditoria e registo
[Operations Management Suite (OMS)](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) fornece extenso registo de atividade de utilizador e de sistema, bem como o estado de funcionamento do sistema. O OMS [Log Analytics](https://azure.microsoft.com/services/log-analytics/) solução recolhe e analisa os dados gerados pelos recursos no Azure e ambientes no local.
- **Registos de atividade**: [registos de atividade](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) fornecem informações aprofundadas operações executadas nos recursos numa subscrição.
- **Os registos de diagnóstico**: [registos de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) incluem todos os registos emitidos por cada recurso. Estes registos incluem registos de sistema de eventos do Windows e armazenamento de Blobs do Azure, tabelas e registos de fila.
- **Os registos de firewall**: O Gateway de aplicação fornece completa diagnóstico e aceder a registos. Estão disponíveis para os recursos de Gateway de aplicação com capacidade WAF registos de firewall.
- **Inicie sessão arquivo**: todos os registos de diagnóstico de escrita para uma conta de armazenamento do Azure centralizada e encriptados para arquivo com um período de retenção definido de 2 dias. Estes registos ligar ao Log Analytics do Azure para o processamento, armazenamento e relatórios do dashboard.

Além disso, as seguintes soluções OMS são incluídas como parte desta arquitetura:
-   [Avaliação do AD](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): O Active Directory estado de funcionamento de verificação solução avalia o risco e o estado de funcionamento de ambientes de servidor num intervalo regular e fornece uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
-   [Avaliação de antimalware](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): solução Antimalware a relatórios de estado de proteção, software maligno e ameaças.
-   [A automatização do Azure](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): solução a automatização do Azure armazena, é executado e gere runbooks.
-   [Auditoria e segurança](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): auditoria e segurança dashboard fornece uma alto nível aprofundadas sobre o estado de segurança de recursos, fornecendo as métricas em domínios de segurança, problemas relevantes, deteções, ameaças e consultas de segurança comuns.
-   [Avaliação do SQL Server](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): O estado de funcionamento de SQL Consulte solução avalia o risco e o estado de funcionamento de ambientes de servidor num intervalo regular e fornece aos clientes com uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
-   [Gestão de atualizações](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): solução de gestão de atualizações a permite a gestão de cliente de atualizações de segurança do sistema operativo, incluindo o processo de instalação de atualizações necessárias e o estado de atualizações disponíveis.
-   [Estado de funcionamento do agente](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): solução do estado de funcionamento do agente comunica quantos agentes implementados e respetiva distribuição geográfica, bem como quantos agentes que estão a responder e o número de agentes que submeter dados operacionais.
-   [Os registos de atividade do Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): solução de análise de registos de atividade o auxilia com a análise de registos a atividade do Azure em todas as subscrições do Azure para um cliente.
-   [Registo de alterações](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): controlo de alterações a solução permite aos clientes identificar facilmente as alterações no ambiente.

### <a name="identity-management"></a>Gestão de identidades
As seguintes tecnologias de fornecem a identidade de capacidades de gestão no ambiente do Azure:
-   [Do Active Directory (AD)](https://azure.microsoft.com/services/active-directory/) pode ser multi-inquilino baseado na nuvem diretório e identidade do serviço de gestão da Microsoft. Todos os utilizadores para a solução foram criados no Azure Active Directory, incluindo os utilizadores que acedem a base de dados do SQL Server.
-   Autenticação para a aplicação é executada a utilizar o Azure AD. Para obter mais informações, consulte [integrar aplicações com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Além disso, a encriptação de coluna de base de dados utiliza o Azure AD para autenticar a aplicação a SQL Database do Azure. Para obter mais informações, consulte como [proteger dados sensíveis na base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) Deteta potenciais vulnerabilidades que afetam as identidades de uma organização, configura as respostas automatizadas detetadas ações suspeitas relacionada com identidades da organização e investiga incidentes suspeitas tome medidas adequadas resolvê-los.
-   [Azure baseada em funções controlo de acesso (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) permite concentra-se a gestão de acesso para o Azure. Acesso de subscrição está limitado ao administrador da subscrição.

Para saber mais sobre como utilizar as funcionalidades de segurança da base de dados do Azure SQL, consulte o [aplicação de demonstração do Contoso Clinic](https://github.com/Microsoft/azure-sql-security-sample) exemplo.

### <a name="security"></a>Segurança
**Gestão de segredos**: A solução utiliza [Cofre de chaves do Azure](https://azure.microsoft.com/services/key-vault/) para a gestão de chaves e segredos. O Cofre de Chaves do Azure ajuda a salvaguardar as chaves criptográficas e os segredos utilizados pelas aplicações em cloud e pelos serviços.

**Proteção contra software maligno**: [Antimalware da Microsoft](https://docs.microsoft.com/azure/security/azure-security-antimalware) para máquinas virtuais fornece a capacidade de proteção em tempo real que ajuda a identifica e remove vírus, spyware e outro software malicioso, com alertas configuráveis ao conhecido software malicioso ou indesejável, tentar instalar ou executar em máquinas virtuais protegidas.

**Aplicar o patch gestão**: implementadas como parte desta arquitetura de referência de máquinas virtuais do Windows estão configuradas por predefinição para receber as atualizações automáticas do serviço de atualização do Windows. Esta solução também inclui o OMS [da automatização do Azure](https://docs.microsoft.com/azure/automation/automation-intro) serviço através do qual implementações atualizadas podem ser criadas para máquinas virtuais de patch quando necessário.


## <a name="guidance-and-recommendations"></a>Orientações e recomendações
### <a name="expressroute-and-vpn"></a>ExpressRoute e de VPN
[ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou de um túnel seguro de VPN tem de ser configurado com segurança estabelecer uma ligação para os recursos implementados como parte desta arquitetura de referência do armazém de dados. Como as ligações do ExpressRoute não passam para a Internet, estas ligações oferecem mais fiabilidade, velocidades mais rápidas, latências inferiores e uma maior segurança ligações típicas através da Internet. Configurando adequadamente ExpressRoute ou uma VPN, os clientes podem adicionar uma camada de proteção de dados em trânsito.

### <a name="extract-transform-load-etl-process"></a>Processo de extrair-transformação-carregamento (ETL)
[O PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) pode carregar dados para o Azure SQL Data Warehouse sem a necessidade de um ETL separada ou importar a ferramenta. O PolyBase permite o acesso aos dados através de consultas de T-SQL. Da Microsoft intelligence de negócio e pilha de análise, bem como as ferramentas de terceiros compatíveis com o SQL Server, podem ser utilizadas com o PolyBase.

### <a name="azure-active-directory-setup"></a>Configuração do Active Directory do Azure
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) é essencial para gerir a implementação e aprovisionamento de acesso ao pessoal interagir com o ambiente. Um Windows Server Active Directory existente, pode ser integrado com o AAD no [quatro cliques](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Os clientes também podem juntar a infraestrutura do Active Directory implementada (controladores de domínio) para um AAD existente ao fazer a infraestrutura do Active Directory implementada um subdomínio de uma floresta do AAD.

### <a name="additional-services"></a>Serviços adicionais
Apesar desta arquitetura do armazém de dados, não se destina a ser implementação para o [Azure comerciais](https://azure.microsoft.com/overview/what-is-azure/) ambiente, objetivos semelhantes podem ser conseguidos através dos serviços descritos nesta arquitetura de referência, bem como serviços adicionais disponíveis apenas no ambiente do Azure comercial. Tenha em atenção que a Azure comerciais mantém um FedRAMP JAB P-/ATO ao nível de impacto moderado, permitindo agências governamentais e parceiros para implementar a informações confidenciais moderately para a nuvem tirar partido do ambiente do Azure comercial.

Ofertas do Azure comerciais que uma ampla variedade de serviços esse identificador formatado e o armazenamento de dados não formatados e a transição para ser utilizado no armazenamento de dados, incluindo:
-   [O Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) é um serviço em nuvem gerido que é criado para híbrida complexas extrair-transformação-carregamento (ETL), extrair-carga-transformação (ELT) e os dados dos projetos de integração. Utilizar o Azure Data Factory, os clientes, podem criar e agendar fluxos de trabalho condicionada por dados chamados pipelines ingestão de dados de arquivos de dados diferentes. Os clientes podem processar e transformar os dados de saída para os arquivos de dados, tais como o Azure SQL Data Warehouse.
-   [O Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) permite que a captura de dados de qualquer tamanho, tipo e velocidade de ingestão num único local para análises exploratórias e operacionais. O Azure Data Lake Store é compatível com a maioria dos componentes de origem do ecossistema do Hadoop e é integrado corretamente com outros serviços do Azure, tais como o Azure SQL Data Warehouse.

## <a name="threat-model"></a>Modelo de ameaça

O diagrama de fluxo de dados (DFD) para esta arquitetura de referência está disponível para [transferir](https://aka.ms/blueprintdwthreatmodel) ou podem ser encontrados abaixo:

![Armazém de dados para o modelo de ameaça FedRAMP](images/fedramp-datawarehouse-threat-model.png?raw=true "do armazém de dados para o modelo de ameaça FedRAMP")

## <a name="compliance-documentation"></a>Documentação de conformidade
O [segurança do Azure e conformidade Blueprint – FedRAMP elevada cliente responsabilidade matriz](https://aka.ms/blueprinthighcrm) apresenta uma lista de todos os controlos de segurança necessários para a linha de base FedRAMP elevado. Da mesma forma, o [segurança do Azure e conformidade Blueprint – FedRAMP moderada cliente responsabilidade matriz](https://aka.ms/blueprintcrmmod) apresenta uma lista de todos os controlos de segurança necessários para a linha de base FedRAMP moderada. Os documentos de detalhe se a implementação de cada controlo é da responsabilidade do Microsoft, o cliente, ou partilhados entre os dois.

O [segurança do Azure e conformidade Blueprint - FedRAMP elevada controlo implementação matriz](https://aka.ms/blueprintdwcimhigh) e o [segurança do Azure e conformidade Blueprint - FedRAMP moderada controlo implementação matriz](https://aka.ms/blueprintdwcimmod) fornecer informações no qual os controlos estão abrangidos pela arquitetura de armazém de dados para cada linha de base do FedRAMP, incluindo como a implementação cumpre os requisitos de cada controlo coberto obter descrições detalhadas.

## <a name="disclaimer"></a>Exclusão de Responsabilidade

 - Este documento destina-se apenas a fins informativos. MICROSOFT FAZ COM QUE NÃO OFERECE NENHUMA GARANTIA, EXPRESSAS, IMPLÍCITA OU ESTATUTARIAMENTE AS INFORMAÇÕES NESTE DOCUMENTO. Este documento é fornecido "como-está." As informações e opiniões expressados neste documento, incluindo URLs e outras referências de Web site da Internet, podem ser alteradas sem aviso prévio. Os clientes ler este documento da sua responsabilidade utilizá-la.
 - Este documento não fornece aos clientes com quaisquer direitos legais a nenhuma propriedade intelectual de qualquer produto da Microsoft ou de soluções.
 - Os clientes podem copiar e utilizar este documento para efeitos de referência interno.
 - Algumas recomendações neste documento poderão resultar numa maior dados, a rede ou a utilização de recursos de computação no Azure e podem aumentar os custos de licenciamento ou de subscrição do Azure do cliente.
 - Esta arquitetura destina-se para servir como base para os clientes ajustar os seus requisitos específicos e não devem ser utilizada como-é num ambiente de produção.
 - Este documento é desenvolvido como uma referência e não deve ser utilizado para definir todas as formas através do qual um cliente pode cumpre os requisitos de conformidade específico e regulamentos. Os clientes devem procurar suporte legal da respetiva organização em implementações de cliente aprovados.

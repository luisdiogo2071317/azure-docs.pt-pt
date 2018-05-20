---
title: Segurança do Azure e conformidade Blueprint - para GDPR do armazém de dados
description: Segurança do Azure e conformidade Blueprint - para GDPR do armazém de dados
services: security
author: jomolesk
ms.assetid: 7a33fb3b-cfb6-49dd-ab4d-c53cf0d5da41
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: a091f4c941b1f4a338ad23956dbba3a7b89b87e8
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="azure-security-and-compliance-blueprint-data-warehouse-for-gdpr"></a>Segurança do Azure e conformidade Blueprint: para GDPR do armazém de dados

## <a name="overview"></a>Descrição geral
O Regulamento de proteção de dados (GDPR) geral contém muitos requisitos sobre recolher, armazenar e a utilização de informações pessoais, incluindo como as organizações a identificarem e proteger os dados pessoais, acomodar os requisitos de transparência, detetarem e comunicar falhas de dados pessoais e técnicos de privacidade de formação e outros empregados. O GDPR proporciona indivíduos maior controlo sobre os respetivos dados pessoais e impõe muitos obrigações novo sobre as organizações que recolhem, identificador ou analisam os dados pessoais. O GDPR impõe novas regras sobre as organizações que ofereçam bens e serviços para as pessoas da União Europeia (EU) ou que recolher e analisam dados associados ao residentes de EU. O GDPR aplica-se, independentemente de onde está localizada numa organização.

Microsoft concebido do Azure com as políticas de privacidade para salvaguardar os dados na nuvem, incluindo as categorias de dados pessoais identificados pelo GDPR e medidas de segurança líder da indústria. Microsoft [termos contractual](http://aka.ms/Online-Services-Terms) consolidar Microsoft os requisitos de processadores.

Esta segurança do Azure e a conformidade Blueprint fornece orientações para implementar uma arquitetura de armazém de dados no Azure que auxilia com requisitos do GDPR. Esta solução demonstra formas em que os clientes podem cumpre os requisitos específicos de segurança e conformidade e funciona como uma base para os clientes criar e configurar as suas próprias soluções de armazém de dados no Azure. Os clientes podem utilizar esta arquitetura de referência e siga da Microsoft [processo quatro passos](https://aka.ms/gdprebook) no respetivo journey a compatibilidade de GDPR:
1. Detetar: Identifique quais os dados pessoais existem e a respetiva localização.
2. Gerir: Regulam os dados pessoais como é utilizado e acedido.
3. Proteger: Estabelece controlos de segurança para evitar, detetar e responder a vulnerabilidades e falhas de dados.
4. Relatório: Manter a documentação necessária e gerir pedidos de dados e infringir notificações.

Esta arquitetura de referência, o guia de implementação associados e o modelo de ameaça destinam-se para servir como base para os clientes para se adaptar os seus requisitos específicos e não devem ser utilizados como-é num ambiente de produção. Tenha em atenção o seguinte:
- A arquitetura fornece uma linha de base para ajudar os clientes implementar cargas de trabalho no Azure de forma GDPR compatíveis.
- Os clientes são responsáveis para realizar a segurança adequada e avaliações de conformidade de qualquer solução incorporada com esta arquitetura, como os requisitos podem variar com base nas especificações de implementação de cada cliente.

## <a name="architecture-diagram-and-components"></a>Diagrama da arquitetura e componentes
Esta solução fornece uma arquitetura de referência que implementa um armazém de dados de nuvem de elevado desempenho e seguro. Existem duas camadas de dados separada nesta arquitetura: um onde os dados são importados, armazenados e testado dentro de um ambiente em cluster do SQL Server e outra para o armazém de dados SQL do Azure onde os dados são carregados utilizando uma ferramenta ETL (por exemplo, [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase)Consultas de T-SQL) para processamento. Depois dos dados são armazenados no Azure SQL Data Warehouse, análise pode executar uma grande escala.

O Azure oferece uma variedade de serviços de análises e relatórios para o cliente. Esta solução inclui o SQL Server Reporting Services (SSRS) para a criação rápida de relatórios do armazém de dados SQL do Azure. Todo o tráfego SQL é encriptado com SSL através da inclusão de certificados autoassinados. Como melhor prática, o Azure recomenda a utilização de uma autoridade de certificação fidedigna para a segurança avançada.

Dados no armazém de dados SQL do Azure são armazenados nas tabelas relacionais com armazenamento columnar, um formato que reduz significativamente os custos de armazenamento de dados ao melhorar o desempenho da consulta.  Dependendo dos requisitos de utilização, os recursos de computação do Azure SQL Data Warehouse podem ser escalados para cima ou para baixo ou encerrados completamente se existem não existem processos Active Directory necessidade de recursos de computação.

Um balanceador de carga do SQL Server gere tráfego SQL, assegurando um desempenho elevado. Todas as máquinas virtuais nesta arquitetura de referência implementar um conjunto de disponibilidade com instâncias do SQL Server configuradas num grupo de Disponibilidade AlwaysOn para capacidades de elevada disponibilidade e recuperação após desastre.

Esta arquitetura de referência do armazém de dados também inclui uma camada do Active Directory (AD) para a gestão de recursos dentro da arquitetura. A sub-rede do Active Directory permite a fácil adoção sob uma estrutura de floresta de AD maior, permitindo operação contínua do ambiente, mesmo quando o acesso à floresta superior não está disponível. Todas as máquinas virtuais estão associados a um domínio para o escalão de Active Directory e utilizar políticas de grupo do Active Directory para impor a segurança e conformidade configurações ao nível do sistema operativo.

Uma máquina virtual funciona como um anfitrião de bastion de gestão, fornecendo uma ligação segura para os administradores de recursos de acesso implementado. Carrega os dados para a área de transição através deste anfitrião de bastion de gestão. **Azure recomenda configurar uma ligação VPN ou Azure ExpressRoute para importação de dados e de gestão para a sub-rede de arquitetura de referência.**

![Armazém de dados para o diagrama de arquitetura de referência GDPR](images/gdpr-datawarehouse-architecture.png?raw=true "do armazém de dados para o diagrama de arquitetura de referência GDPR")

Esta solução utiliza os seguintes serviços do Azure. Os detalhes da arquitetura de implementação estão no [arquitetura de implementação](#deployment-architecture) secção.

-   Máquinas Virtuais do Azure
    - (1) anfitrião de bastion
    -   (2) controlador de domínio o active Directory
    -   (2) nó de Cluster de servidor do SQL Server
    -   (1) SQL Server servidor testemunha
-   Conjuntos de Disponibilidade
    - Controladores de domínio do Active Directory
    - Nós de Cluster do SQL Server e da testemunha
-   Rede Virtual
    - (4) sub-redes
    -   (4) grupos de segurança de rede
- SQL Data Warehouse
- SQL Server Reporting Services
- Balanceador de carga SQL do Azure
- Azure Active Directory
- Cofre de serviços de recuperação
- Azure Key Vault
- Operations Management Suite (OMS)
- Catálogo de Dados do Azure
- Centro de Segurança do Azure

## <a name="deployment-architecture"></a>Arquitetura de implementação
A secção seguinte descreve em detalhe os elementos de implementação e a implementação.

**O SQL Data Warehouse**: [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) é uma empresa dados armazém (EDW) que tira partido em massa paralelas processamento (MPP) rapidamente executar consultas complexas em petabytes de dados, permitindo que os utilizadores identifica eficazmente os dados pessoais. Os utilizadores podem utilizar consultas de PolyBase T-SQL simples para importar dados de grandes para o armazém de dados do SQL Server e utilizam a potência de MPP para executar a análise de elevado desempenho.

**SQL Server Reporting Services (SSRS)**: [SQL Server Reporting Services](https://docs.microsoft.com/en-us/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) fornece criação rápida de relatórios com tabelas, gráficos, mapas, medidores, matrizes e mais para o Azure SQL Data Warehouse.

**Catálogo de dados**: [catálogo de dados](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) faz com que as origens de dados facilmente Detetáveis e compreensíveis pelos utilizadores que gerem os dados. Origens de dados comuns podem ser registadas, etiquetadas e pesquisou dados pessoais. Os dados permanecem na localização existente, mas uma cópia dos metadados é adicionada ao catálogo de dados, juntamente com uma referência para a localização de origem de dados. Os metadados também são indexados para tornar cada origem de dados facilmente detetável através da pesquisa e compreensível para os utilizadores que a detetarem.

**Anfitrião de bastion**: O anfitrião de bastion for o único ponto de entrada que permite aos utilizadores aceder os recursos implementados neste ambiente. O anfitrião de bastion fornece uma ligação segura a recursos implementados, permitindo apenas tráfego remoto de endereços IP públicos numa lista segura. Para permitir o tráfego de (RDP) de ambiente de trabalho remoto, a origem do tráfego tem de ser definido no grupo de segurança de rede (NSG).

Esta solução cria uma máquina virtual como um anfitrião de bastion associados a um domínio com as seguintes configurações:
-   [Extensão de antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Extensão do OMS](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Extensão de diagnóstico do Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) com o Cofre de chaves do Azure
-   Um [política de encerramento automático](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) reduzir o consumo de recursos de máquina virtual quando não está em utilização
-   [Proteção de credenciais do Windows Defender](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) ativado para que as credenciais e outros segredos executadas num ambiente protegido que está isolado do sistema operativo em execução

### <a name="virtual-network"></a>Rede virtual
Esta arquitetura de referência define uma VNet com um espaço de endereços de 10.0.0.0/16 privada.

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
A arquitetura protege os dados Inativos através de várias medidas, incluindo encriptação e auditoria de base de dados.

**Armazenamento do Azure**: para satisfazer os dados encriptados em requisitos de rest, todos os [Storage do Azure](https://azure.microsoft.com/services/storage/) utiliza [encriptação do serviço de armazenamento](https://docs.microsoft.com/azure/storage/storage-service-encryption). Isto ajuda a proteger e salvaguardar os dados pessoais para suportar os compromissos de segurança organizacional e requisitos de conformidade definidos pelo GDPR.

**Azure Disk Encryption**: [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) aproveita a funcionalidade de BitLocker do Windows para fornecer a encriptação de volume para o SO e discos de dados. A solução integra-se com o Cofre de chaves do Azure para ajudar a controlar e gerir as chaves de encriptação de disco.

**Base de dados SQL do Azure**: instância de base de dados de SQL do Azure utiliza as seguintes medidas de segurança da base de dados:
-   [Autorização e autenticação AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) permite a gestão de identidades de utilizadores de base de dados e outros serviços Microsoft numa localização central.
-   [Auditoria de base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) controla da base de dados eventos e escreve-los para uma auditoria iniciem sessão uma conta de armazenamento do Azure.
-   Base de dados do SQL Server está configurado para utilizar [encriptação de dados transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), que efetua a encriptação em tempo real e a desencriptação da base de dados, cópias de segurança associadas e os ficheiros de registo de transações para proteger as informações em rest. TDE fornece assurance que armazenava os dados pessoais não foi sujeito acesso não autorizado.
-   [Regras de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impedir o acesso de todos os servidores de base de dados até que são concedidas permissões adequadas. A firewall concede acesso a bases de dados com base no endereço IP de origem de cada pedido.
-   [A deteção de ameaças do SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) permite a deteção e a resposta a potenciais ameaças à medida que ocorrem fornecendo alertas de segurança para acesso de base de dados anómalas, ataques de injeção de SQL, potenciais vulnerabilidades e atividades suspeitas da base de dados padrões.
-   [Encriptados colunas sempre](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) Certifique-se de que os dados pessoais confidenciais nunca é apresentada como texto não encriptado no interior do sistema de base de dados. Depois de ativar a encriptação de dados, apenas as aplicações de cliente ou servidores de aplicações com o acesso às chaves podem aceder aos dados de texto simples.
- O [propriedades expandidas](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) funcionalidade pode ser utilizada para descontinuar o processamento de assuntos de dados, permite aos utilizadores adicionar propriedades personalizadas para objetos de base de dados e Etiquetar dados como "Discontinued" para suportar a lógica da aplicação para impedir que o processamento dos dados pessoais associados.
- [Segurança ao nível da linha](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) permite aos utilizadores definir políticas para restringir o acesso aos dados para descontinuar o processamento.
- [SQL Server da base de dados dinâmico dados máscara (ddm de autor)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) limita a exposição de dados pessoais confidenciais por máscara de dados para aplicações ou utilizadores não privilegiados. Ddm de autor pode automaticamente detetar dados potencialmente sensíveis e sugerir as máscaras de adequado sejam aplicadas. Isto ajuda-o com a identificação dos dados pessoais elegíveis para proteção de GDPR e para reduzir o acesso, se não for fechada a base de dados através de acesso não autorizado. **Nota: Os clientes terão de ajustar as definições de ddm de autor aderir ao respetiva esquema de base de dados.**

### <a name="identity-management"></a>Gestão de identidades
As seguintes tecnologias de fornecem capacidades para gerir o acesso a dados pessoais no ambiente do Azure:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) é multi-inquilino baseado na nuvem diretório e identidade do serviço de gestão da Microsoft. Todos os utilizadores para esta solução são criados no AAD, incluindo os utilizadores que acedem a base de dados do SQL Server.
-   Autenticação para a aplicação é executada com o AAD. Para obter mais informações, consulte [integrar aplicações com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Além disso, a encriptação de coluna de base de dados utiliza o AAD para autenticar a aplicação a SQL Database do Azure. Para obter mais informações, consulte como [proteger dados sensíveis na base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure baseada em funções controlo de acesso (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) permite aos administradores definir as permissões de acesso detalhada para conceder apenas a quantidade de acesso que os utilizadores precisam desempenhar as suas funções. Em vez de fornecer todas as permissões de utilizador sem restrições para recursos do Azure, os administradores podem permitir apenas determinadas ações para aceder a dados pessoais. Acesso de subscrição está limitado ao administrador da subscrição.
- [AAD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) permite que os clientes minimizar o número de utilizadores que tenham acesso aos determinadas informações tais como os dados pessoais.  Os administradores podem utilizar o AAD Privileged Identity Management para detetar, restringir e monitorizar as identidades privilegiadas e o respetivo acesso aos recursos. Esta funcionalidade também pode ser utilizada para impor o acesso administrativo a pedido, just-in-time quando necessário.
- [Proteção de identidade AAD](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) Deteta potenciais vulnerabilidades que afetam as identidades de uma organização, configura as respostas automatizadas detetadas ações suspeitas relacionada com identidades da organização e investiga suspeita incidentes tome medidas adequadas resolvê-los.

### <a name="security"></a>Segurança
**Gestão de segredos**: A solução utiliza [Cofre de chaves do Azure](https://azure.microsoft.com/services/key-vault/) para a gestão de chaves e segredos. O Cofre de Chaves do Azure ajuda a salvaguardar as chaves criptográficas e os segredos utilizados pelas aplicações em cloud e pelos serviços. As seguintes capacidades de Cofre de chaves do Azure ajudam a proteger os dados pessoais e de acesso a esses dados de clientes:
- Políticas de acesso avançadas são configuradas de forma necessidade.
- As políticas de acesso do Cofre de chaves são definidas com permissões mínimas para chaves e segredos.
- Todas as chaves e segredos no Cofre de chaves tem datas de expiração.
- Todas as chaves no Cofre de chaves estão protegidas por módulos de hardware especializado de segurança (HSMs). O tipo de chave é uma chave RSA de 2048 protegidas de HSM bits.
- Todos os utilizadores e identidades são concedidas as permissões necessárias mínimo através do RBAC.
- Registos de diagnóstico para o Cofre de chaves estão ativados com um período de retenção de, pelo menos, a 365 dias.
- Operações de criptografia permitidas para as chaves estão limitadas às necessárias.

**Aplicar o patch gestão**: implementadas como parte desta arquitetura de referência de máquinas virtuais do Windows estão configuradas por predefinição para receber as atualizações automáticas do serviço de atualização do Windows. Esta solução também inclui o OMS [da automatização do Azure](https://docs.microsoft.com/azure/automation/automation-intro) serviço através do qual implementações atualizadas podem ser criadas para máquinas virtuais de patch quando necessário.

**Proteção contra software maligno**: [Antimalware da Microsoft](https://docs.microsoft.com/azure/security/azure-security-antimalware) para máquinas virtuais fornece a capacidade de proteção em tempo real que ajuda a identifica e remove vírus, spyware e outro software malicioso, com alertas configuráveis ao conhecido software malicioso ou indesejável, tentar instalar ou executar em máquinas virtuais protegidas.

**Alertas de segurança**: [Centro de segurança do Azure](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro) permite aos clientes monitorizar o tráfego, recolher registos e analisar as origens de dados de ameaças. Além disso, o Centro de segurança do Azure acede a configuração existente dos serviços do Azure para fornecer recomendações de serviço para ajudar a melhorar a postura de segurança e proteger os dados pessoais e de configuração. Centro de segurança do Azure inclui um [ameaça relatório intelligence](https://docs.microsoft.com/en-us/azure/security-center/security-center-threat-report) para cada detetados ameaça para o ajudar a equipas de resposta a incidentes investigar e remedeie ameaças.

### <a name="business-continuity"></a>Continuidade do negócio
**Elevada disponibilidade**: cargas de trabalho do servidor são agrupadas num [do conjunto de disponibilidade](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) para ajudar a garantir a elevada disponibilidade das máquinas virtuais no Azure. Pelo menos uma máquina virtual está disponível durante um evento de manutenção planeada ou, reunião de 99,95% SLA do Azure.

**O Cofre de serviços de recuperação**: O [cofre dos serviços de recuperação](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) aloja dados de cópia de segurança e protege todas as configurações de máquinas virtuais do Azure nesta arquitetura. Com um cofre de serviços de recuperação, os clientes restaurar ficheiros e pastas a partir de uma VM do IaaS sem restaurar toda a VM, permitindo tempos de restauro mais rápidos.

### <a name="logging-and-auditing"></a>Registo e auditoria
[Operations Management Suite (OMS)](https://docs.microsoft.com/en-us/azure/operations-management-suite/operations-management-suite-overview) fornece extenso registo de atividade de utilizador e de sistema, bem como o estado de funcionamento do sistema. O OMS [Log Analytics](https://azure.microsoft.com/services/log-analytics/) solução recolhe e analisa os dados gerados pelos recursos no Azure e ambientes no local.
- **Registos de atividade**: [registos de atividade](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) fornecem informações aprofundadas operações executadas nos recursos numa subscrição. Registos de atividade podem ajudar a determinar o iniciador de uma operação, hora de ocorrência e o estado.
- **Os registos de diagnóstico**: [registos de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) incluem todos os registos emitidos por cada recurso. Estes registos incluem registos de sistema de eventos do Windows e armazenamento de Blobs do Azure, tabelas e registos de fila.
- **Inicie sessão arquivo**: todos os registos de diagnóstico de escrita para uma conta de armazenamento do Azure centralizada e encriptados para arquivo. O período de retenção é configurável pelo utilizador, cópia de segurança para dias 730, para satisfazer os requisitos de retenção específico da organização. Estes registos ligar ao Log Analytics do Azure para o processamento, armazenamento e relatórios do dashboard.

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

## <a name="threat-model"></a>Modelo de ameaça

O diagrama de fluxo de dados para esta arquitetura de referência está disponível para [transferir](https://aka.ms/gdprDWdfd) ou podem ser encontrados abaixo. Este modelo pode ajudar os clientes os pontos de risco potencial na infraestrutura do sistema quando efetuar alterações.

![Armazém de dados para o modelo de ameaça GDPR](images/gdpr-datawarehouse-threat-model.png?raw=true "do armazém de dados para o modelo de ameaça GDPR")

## <a name="compliance-documentation"></a>Documentação de conformidade
O [segurança do Azure e conformidade Blueprint – GDPR cliente responsabilidade matriz](https://aka.ms/gdprCRM) lista responsabilidades de controlador e do processador para todos os artigos GDPR. Tenha em atenção que, para os serviços do Azure, um cliente é, normalmente, o controlador e Microsoft Age como o processador.

O [segurança do Azure e conformidade Blueprint - matriz de implementação do armazém de dados GDPR](https://aka.ms/gdprDW) fornece informações sobre qual GDPR artigos são abordados a arquitetura do armazém de dados, incluindo descrições detalhadas do como o implementação satisfaz os requisitos de cada artigo coberto.

## <a name="guidance-and-recommendations"></a>Orientações e recomendações
### <a name="vpn-and-expressroute"></a>ExpressRoute e de VPN
Um túnel VPN seguro ou [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) tem de ser configurado com segurança estabelecer uma ligação para os recursos implementados como parte desta arquitetura de referência do armazém de dados. Adequadamente a configurar uma VPN ou ExpressRoute, os clientes podem adicionar uma camada de proteção de dados em trânsito.

Ao implementar um túnel VPN seguro com o Azure, é possível criar uma ligação privada virtual entre uma rede no local e uma rede Virtual do Azure. Esta ligação ocorre através da Internet e permite aos clientes de forma segura as informações de "túnel" dentro de uma ligação encriptada entre a rede do cliente e o Azure. VPN de Site a site é uma tecnologia de segura, madura que tenha sido implementada por empresas de todos os tamanhos de decades. O [modo de túnel IPsec](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) é utilizada nesta opção como um mecanismo de encriptação.

Porque o tráfego de túnel VPN atravessam a Internet e uma VPN de site a site, a Microsoft oferece a opção de ligação de outro, ainda mais seguros. O ExpressRoute do Azure é uma WAN dedicada ligação entre o Azure e uma localização no local ou um fornecedor de alojamento do Exchange. Como as ligações do ExpressRoute não passam para a Internet, estas ligações oferecem mais fiabilidade, velocidades mais rápidas, latências inferiores e uma maior segurança ligações típicas através da Internet. Além disso, porque se trata de uma ligação direta do fornecedor de telecommunication do cliente, os dados não viajam através da Internet e, por conseguinte, não estão expostos ao mesmo.

Melhores práticas para implementar uma rede híbrida segura que expande uma rede no local para o Azure são [disponíveis](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-etl-process"></a>Processo de extrair-transformação-carregamento (ETL)
[O PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) pode carregar dados para o Azure SQL Data Warehouse sem a necessidade de um ETL separada ou importar a ferramenta. O PolyBase permite o acesso aos dados através de consultas de T-SQL. Da Microsoft intelligence de negócio e pilha de análise, bem como as ferramentas de terceiros compatíveis com o SQL Server, podem ser utilizadas com o PolyBase.

### <a name="azure-active-directory-setup"></a>Configuração do Active Directory do Azure
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) é essencial para gerir a implementação e aprovisionamento de acesso ao pessoal interagir com o ambiente. Um Windows Server Active Directory existente, pode ser integrado com o AAD no [quatro cliques](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Os clientes também podem juntar a infraestrutura do Active Directory implementada (controladores de domínio) para um AAD existente ao fazer a infraestrutura do Active Directory implementada um subdomínio de uma floresta do AAD.

### <a name="optional-services"></a>Serviços opcionais
O Azure oferece uma variedade de serviços para ajudá-lo com o armazenamento e a transição de dados formatados e não formatados. Os seguintes serviços podem ser adicionados a esta arquitetura de referência dependendo dos requisitos de cliente:
-   [O Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) é um serviço em nuvem gerido que é criado para híbrida complexas extrair-transformação-carregamento (ETL), extrair-carga-transformação (ELT) e os dados dos projetos de integração. O Azure Data Factory tem capacidades para ajudar o rastreio e localizar dados pessoais, incluindo a visualização e ferramentas para identificar quando dados chegaram e onde provém de monitorização. Utilizar o Azure Data Factory, os clientes, podem criar e agendar fluxos de trabalho condicionada por dados chamados pipelines ingestão de dados de arquivos de dados diferentes. Estes pipelines permitem aos clientes para a ingestão de dados a partir de origens internas e externas. Os clientes podem processar e transformar os dados de saída para os arquivos de dados, tais como o Azure SQL Data Warehouse.
- Os clientes podem testar dados não estruturados no [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview), que permite a captura de dados de qualquer tamanho, tipo e velocidade de ingestão num único local para análises exploratórias e operacionais.  Azure Data Lake tem capacidades que permitem a extração e conversão de dados. O Azure Data Lake Store é compatível com a maioria dos componentes de origem do ecossistema do Hadoop e é integrado corretamente com outros serviços do Azure, tais como o Azure SQL Data Warehouse.

## <a name="disclaimer"></a>Exclusão de Responsabilidade

 - Este documento destina-se apenas a fins informativos. MICROSOFT FAZ COM QUE NÃO OFERECE NENHUMA GARANTIA, EXPRESSAS, IMPLÍCITA OU ESTATUTARIAMENTE AS INFORMAÇÕES NESTE DOCUMENTO. Este documento é fornecido "como-está." As informações e opiniões expressados neste documento, incluindo URLs e outras referências de Web site da Internet, podem ser alteradas sem aviso prévio. Os clientes ler este documento da sua responsabilidade utilizá-la.
 - Este documento não fornece aos clientes com quaisquer direitos legais a nenhuma propriedade intelectual de qualquer produto da Microsoft ou de soluções.
 - Os clientes podem copiar e utilizar este documento para efeitos de referência interno.
 - Algumas recomendações neste documento poderão resultar numa maior dados, a rede ou a utilização de recursos de computação no Azure e podem aumentar os custos de licenciamento ou de subscrição do Azure do cliente.
 - Esta arquitetura destina-se para servir como base para os clientes ajustar os seus requisitos específicos e não devem ser utilizada como-é num ambiente de produção.
 - Este documento é desenvolvido como uma referência e não deve ser utilizado para definir todas as formas através do qual um cliente pode cumpre os requisitos de conformidade específico e regulamentos. Os clientes devem procurar suporte legal da respetiva organização em implementações de cliente aprovados.

---
title: Segurança do Azure e o esquema de conformidade – armazém de dados para o GDPR
description: Segurança do Azure e o esquema de conformidade – armazém de dados para o GDPR
services: security
author: jomolesk
ms.assetid: 7a33fb3b-cfb6-49dd-ab4d-c53cf0d5da41
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: 6a777418c5381f1f52bae31ad4e697248587fc6d
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45576549"
---
# <a name="azure-security-and-compliance-blueprint-data-warehouse-for-gdpr"></a>Azure Security and Compliance Blueprint: armazém de dados para o GDPR

## <a name="overview"></a>Descrição geral
O Regulamento de proteção de dados (GDPR) geral contém muitos requisitos sobre a recolher, armazenar e usar informações pessoais, incluindo como as organizações a identificarem e proteger dados pessoais, acomodar os requisitos de transparência, detetar e comunicar violações de dados pessoais e o pessoal de privacidade train e outros funcionários. O GDPR oferece maior controle sobre seus dados pessoais de indivíduos e impõe obrigações de novo vários de organizações que recolhem, processarem ou analisar os dados pessoais. O GDPR impõe as regras de novo em organizações que oferecem bens e serviços a pessoas localizadas na União Europeia (UE), ou que recolha e analisam dados associados a residentes da UE. O GDPR aplica-se, independentemente de onde está localizada a uma organização.

A Microsoft criou o Azure com as políticas de privacidade para salvaguardar os dados na cloud, incluindo as categorias de dados pessoais identificados pelo GDPR e medidas de segurança líderes do setor. Da Microsoft [termos contratuais](http://aka.ms/Online-Services-Terms) consolidar Microsoft para os requisitos de processadores.

Este Azure no Security and Compliance Blueprint fornece orientações para implementar uma arquitetura de armazém de dados no Azure que o ajuda com os requisitos do GDPR. Esta solução demonstra maneiras em que os clientes podem satisfazer os requisitos específicos de segurança e conformidade e serve como uma base para os clientes criar e configurar suas próprias soluções de armazém de dados no Azure. Os clientes podem utilizar esta arquitetura de referência e siga da Microsoft [processo de quatro etapas](https://aka.ms/gdprebook) na sua jornada para conformidade com o GDPR:
1. Detetar: Identifica os dados pessoais que existem e a respetiva localização.
2. Gerir: Regulam os dados pessoais como são utilizadas e acedidos.
3. Proteger: Estabelece controlos de segurança para prevenir, detetar e responder a vulnerabilidades e violações de dados.
4. Relatório: Manter a documentação necessária e gerir pedidos de dados e notificações de violações.

Esta arquitetura de referência, o guia de implementação associados e o modelo de risco são deve ser usado como uma base para os clientes para se adaptar às suas necessidades específicas e não devem ser utilizados como-está num ambiente de produção. Tenha em atenção o seguinte:
- A arquitetura fornece uma linha de base para ajudar os clientes a implementar cargas de trabalho para o Azure de forma compatível com o GDPR.
- Os clientes são responsáveis pela realização de segurança adequados e avaliações de conformidade de qualquer solução criada com esta arquitetura, como os requisitos podem variar com base nas especificidades da implementação de cada cliente.

## <a name="architecture-diagram-and-components"></a>Diagrama de arquitetura e componentes
Esta solução fornece uma arquitetura de referência que implementa um armazém de dados de cloud seguro e de alto desempenho. Existem duas camadas de dados separado nesta arquitetura: uma em que os dados são importados, armazenados e de teste dentro de um ambiente de cluster de SQL e outro para o Azure SQL Data Warehouse em que os dados são carregados com uma ferramenta ETL (por exemplo, [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase)Consultas de T-SQL) para processamento. Assim que os dados são armazenados no armazém de dados SQL do Azure, podem executar análises em grande escala.

O Azure oferece uma variedade de serviços de relatórios e análise do cliente. Esta solução inclui o SQL Server Reporting Services (SSRS) para a criação rápida de relatórios do armazém de dados SQL do Azure. Todo o tráfego SQL é encriptado com SSL através da inclusão de certificados autoassinados. Como melhor prática, o Azure recomenda a utilização de uma autoridade de certificação fidedigna para uma maior segurança.

Dados no armazém de dados SQL do Azure são armazenados em tabelas relacionais com o armazenamento em colunas, um formato que reduz significativamente os custos de armazenamento de dados enquanto melhora o desempenho da consulta.  Dependendo dos requisitos de utilização, os recursos de computação do Azure SQL Data Warehouse podem ser aumentados vertical ou horizontalmente ou desativar completamente se existirem não existem processos ativos que exigem recursos de computação.

Um balanceador de carga de SQL gere o tráfego SQL, garantindo alto desempenho. Implementar todas as máquinas virtuais nesta arquitetura de referência num conjunto com instâncias do SQL Server configuradas num grupo de Disponibilidade AlwaysOn para capacidades de elevada disponibilidade e recuperação após desastre de disponibilidade.

Esta arquitetura de referência do armazém de dados também inclui uma camada do Active Directory (AD) para a gestão de recursos dentro da arquitetura. A sub-rede do Active Directory permite a adoção fácil numa estrutura de floresta de AD maior, permitindo que o ambiente da operação contínua, mesmo quando o acesso para a floresta maior não está disponível. Todas as máquinas virtuais estão associados a um domínio para o escalão do Active Directory e utilize políticas de grupo do Active Directory para impor configurações de segurança e conformidade ao nível do sistema operativo.

Uma máquina virtual funciona como um anfitrião de bastião de gestão, fornecendo uma ligação segura para os administradores de recursos de acesso implementado. O carregamento dos dados para a área de teste por meio deste anfitrião de bastião de gestão. **Azure recomenda configurar uma ligação VPN ou Azure ExpressRoute para importação de dados e de gestão para a sub-rede de arquitetura de referência.**

![Armazém de dados para o diagrama de arquitetura de referência do GDPR](images/gdpr-datawarehouse-architecture.png?raw=true "armazém de dados para o diagrama de arquitetura de referência GDPR")

Esta solução utiliza os seguintes serviços do Azure. Os detalhes a arquitetura de implementação estão no [arquitetura de implantação](#deployment-architecture) secção.

-   Máquinas Virtuais do Azure
    - (1) anfitrião de bastião
    -   (2) controlador de domínio do as Active Directory
    -   (2) nó de Cluster de servidor SQL
    -   (1) o SQL Server testemunha
-   Conjuntos de Disponibilidade
    - Controladores de domínio do Active Directory
    - Nós de Cluster SQL e da testemunha
-   Rede Virtual
    - (4) sub-redes
    -   (4) grupos de segurança de rede
- SQL Data Warehouse
- SQL Server Reporting Services
- Balanceador de carga SQL do Azure
- Azure Active Directory
- Cofre dos serviços de recuperação
- Azure Key Vault
- Operations Management Suite (OMS)
- Catálogo de Dados do Azure
- Centro de Segurança do Azure

## <a name="deployment-architecture"></a>Arquitetura de implantação
A secção seguinte fornece detalhes sobre os elementos de implantação e a implementação.

**O SQL Data Warehouse**: [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) é uma empresa armazém de dados (EDW) que tira partido Massivamente paralela de processamento (MPP) para executar rapidamente consultas complexas em petabytes de dados, permitindo que os usuários identificar com eficiência os dados pessoais. Os utilizadores podem utilizar consultas simples de PolyBase T-SQL para importar grandes quantidades de dados para o SQL Data Warehouse e utilizam o poder do MPP para executar análises de elevado desempenho.

**SQL Server Reporting Services (SSRS)**: [SQL Server Reporting Services](https://docs.microsoft.com/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) fornece a rápida criação de relatórios com tabelas, gráficos, mapas, medidores, matrizes e mais para o Azure SQL Data Warehouse.

**Catálogo de dados**: [catálogo de dados](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) torna os dados facilmente Detetáveis e compreensíveis pelos utilizadores que gerem os dados. Origens de dados comuns podem ser registadas, marcadas e está à procura os dados pessoais. Os dados permanecem na localização existente, mas uma cópia dos respetivos metadados é adicionada ao catálogo de dados, juntamente com uma referência para a localização de origem de dados. Os metadados também são indexados para tornar cada origem de dados facilmente detetável através da pesquisa e compreensível para os utilizadores que a detetarem.

**Anfitrião de bastião**: O anfitrião de bastião é o único ponto de entrada que permite aos utilizadores aceder os recursos implementados neste ambiente. O anfitrião de bastião fornece uma ligação segura a recursos implementados, permitindo apenas tráfego remoto a partir de endereços IP públicos numa lista segura. Para permitir o tráfego de (protocolo RDP) de área de trabalho remoto, a origem do tráfego precisa de ser definidos no grupo de segurança de rede (NSG).

Esta solução cria uma máquina virtual como um anfitrião de bastião associados a um domínio com as seguintes configurações:
-   [Extensão de antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Extensão do OMS](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Extensão de diagnóstico do Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [O Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) com o Azure Key Vault
-   Uma [política de encerramento automático](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) para reduzir o consumo de recursos de máquina virtual quando não está em utilização
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) ativado para que as credenciais e outros segredos executar num ambiente protegido, que está isolado do sistema operativo em execução

### <a name="virtual-network"></a>Rede virtual
Esta arquitetura de referência define uma VNet privada com um espaço de endereços de 10.0.0.0/16.

**Grupos de segurança de rede**: [NSGs](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) contêm o controle de listas de acesso (ACLs) que permitem ou negam o tráfego numa VNet. Podem ser utilizados NSGs para proteger o tráfego numa sub-rede ou o nível VM individual. Existem os NSGs seguintes:
  - Um NSG para a camada de dados (Clusters do SQL Server, SQL Server testemunha e Balanceador de carga SQL)
  - Um NSG para o anfitrião de bastião de gestão
  - Um NSG para o Active Directory
  - Um NSG para o SQL Server Reporting Services

Cada um dos NSGs têm portas específicas e protocolos abrir para que a solução pode trabalhar de forma segura e corretamente. Além disso, as seguintes configurações são habilitadas para cada NSG:
  - [Eventos e registos de diagnóstico](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) forem ativadas e armazenados numa conta de armazenamento
  - Log Analytics do OMS está ligado a [diagnósticos do NSG](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Sub-redes**: cada sub-rede está associada ao seu NSG correspondente.

### <a name="data-at-rest"></a>Dados inativos
A arquitetura protege dados em repouso através de várias medidas, incluindo encriptação e a auditoria de base de dados.

**O armazenamento do Azure**: para atender aos dados criptografados em requisitos de rest, todos os [armazenamento do Azure](https://azure.microsoft.com/services/storage/) utiliza [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Isto ajuda a proteger e salvaguardar os dados pessoais para oferecer suporte a compromissos de segurança organizacional e requisitos de conformidade definidos pelo GDPR.

**O Azure Disk Encryption**: [do Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) aproveita a funcionalidade BitLocker do Windows para fornecer encriptação de volume para o SO e discos de dados. A solução se integra com o Azure Key Vault para ajudar a controlar e gerir as chaves de encriptação de disco.

**Base de dados SQL do Azure**: instância de base de dados de SQL do Azure utiliza as seguintes medidas de segurança da base de dados:
-   [Autenticação e autorização AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) permite a gestão de identidades de utilizadores de base de dados e outros serviços da Microsoft num local central.
-   [A auditoria de base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) faixas de base de dados eventos e escreve-os para uma auditoria registo numa conta de armazenamento do Azure.
-   Base de dados SQL está configurado para utilizar [encriptação de dados transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), que realiza a encriptação em tempo real e a desencriptação da base de dados, cópias de segurança associadas e ficheiros de registo de transação para proteger as informações em rest. TDE fornece assurance que armazenados dados pessoais não tiver sido sujeita a acesso não autorizado.
-   [Regras de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impedir o acesso de todos os servidores de base de dados até que sejam concedidas as permissões apropriadas. A firewall concede acesso a bases de dados com base no endereço IP de origem de cada pedido.
-   [Deteção de ameaças SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) permite a deteção e resposta a ameaças potenciais à medida que ocorrem ao fornecer alertas de segurança para atividades suspeitas da base de dados, potenciais vulnerabilidades, ataques de injeção de SQL e acesso de base de dados anómalas padrões.
-   [Sempre as colunas encriptadas](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) Certifique-se de que os dados pessoais confidenciais nunca é apresentada como texto não criptografado dentro do sistema de base de dados. Depois de ativar a encriptação de dados, apenas as aplicações de cliente ou servidores de aplicações com acesso às chaves podem aceder aos dados de texto sem formatação.
- O [Propriedades estendidas](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) funcionalidade pode ser utilizada para descontinuar o processamento de titulares dos dados, já que permite aos utilizadores adicionar propriedades personalizadas para objetos de base de dados e marcar os dados como "Discontinued" para suportar a lógica de aplicação para impedir que o processamento de dados pessoais associados.
- [Segurança ao nível da linha](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) permite aos utilizadores definir políticas para restringir o acesso aos dados de descontinuar o processamento.
- [SQL da base de dados dinâmica dados máscara (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) limita a exposição de dados pessoais confidenciais ao mascará os dados para aplicações ou utilizadores sem privilégios. Ddm de autor pode detetar dados potencialmente confidenciais e sugerir as máscaras de apropriado a ser aplicada automaticamente. Isto ajuda na identificação dos dados pessoais qualificar para proteção com o GDPR e para reduzir o acesso, de modo que ele não for fechada a base de dados por meio do acesso não autorizado. **Nota: Os clientes terão de ajustar as definições de ddm de autor de cumprir para seu esquema de base de dados.**

### <a name="identity-management"></a>Gestão de identidades
As seguintes tecnologias fornecem capacidades para gerir o acesso aos dados pessoais no ambiente do Azure:
-   [O Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) é multi-inquilino com base na cloud diretório e identidade do serviço de gestão da Microsoft. Todos os utilizadores para esta solução são criados no AAD, incluindo os utilizadores que acedem a base de dados SQL.
-   Autenticação para a aplicação é executada através do AAD. Para obter mais informações, consulte [integrar aplicações com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Além disso, a encriptação de coluna de base de dados utiliza o AAD para autenticar a aplicação à base de dados do Azure SQL. Para obter mais informações, consulte como [proteger dados confidenciais na base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Do Azure com base em função de controlo de acesso (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) permite aos administradores definir permissões de acesso detalhado para conceder apenas a quantidade de acesso que os utilizadores precisam realizar seus trabalhos. Em vez de dar todas as permissões de utilizador sem restrições para recursos do Azure, os administradores podem permitir apenas certas ações para aceder aos dados pessoais. Acesso à subscrição está limitado ao administrador da subscrição.
- [AAD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) permite que os clientes minimizar o número de utilizadores que têm acesso a informações específicas, como os dados pessoais.  Os administradores podem utilizar o AAD Privileged Identity Management para detetar, restringir e monitorizar identidades privilegiadas e respetivo acesso a recursos. Esta funcionalidade também pode ser utilizada para impor acesso administrativo a pedido e just-in-time quando necessário.
- [Proteção de identidade do AAD](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) Deteta potenciais vulnerabilidades que afetam as identidades da organização, configura as respostas automáticas para detetado ações suspeitas relacionadas com identidades de uma organização e investiga suspeita incidentes para tomar as medidas adequadas para resolvê-los.

### <a name="security"></a>Segurança
**Gestão de segredos**: A solução utiliza [do Azure Key Vault](https://azure.microsoft.com/services/key-vault/) para o gerenciamento de chaves e segredos. O Cofre de Chaves do Azure ajuda a salvaguardar as chaves criptográficas e os segredos utilizados pelas aplicações em cloud e pelos serviços. As seguintes capacidades do Azure Key Vault ajudam os clientes a proteger os dados pessoais e de acesso a esses dados:
- Políticas de acesso avançadas são configuradas de forma necessidade.
- Políticas de acesso do Cofre de chaves são definidas com o mínimo permissões necessárias para chaves e segredos.
- Todas as chaves e segredos no Key Vault tem datas de expiração.
- Todas as chaves no Cofre de chaves estão protegidas por módulos de hardware especializado de segurança (HSMs). O tipo de chave é uma chave RSA de 2048 protegidas de HSM bits.
- Todos os utilizadores e identidades são concedidas as permissões mínimas necessárias através do RBAC.
- Registos de diagnóstico para o Cofre de chaves estão ativados com um período de retenção de, pelo menos, 365 dias.
- Operações de criptografia permitidas para as chaves estão limitadas aos obrigatórios.

**Gerenciamento de patches**: máquinas de virtuais do Windows implementadas como parte desta arquitetura de referência estão configuradas por predefinição para receber atualizações automáticas do serviço de atualização do Windows. Esta solução também inclui o OMS [automatização do Azure](https://docs.microsoft.com/azure/automation/automation-intro) serviço por meio do qual as implementações atualizadas podem ser criadas para máquinas de virtuais de patch quando necessário.

**Proteção contra software maligno**: [Antimalware da Microsoft](https://docs.microsoft.com/azure/security/azure-security-antimalware) para máquinas virtuais fornece a capacidade de proteção em tempo real que ajuda a identifica e remove vírus, spyware e outro software malicioso, com alertas configuráveis Quando conhecido software malicioso ou indesejável se tentar instalar ou executar nas máquinas virtuais protegidas.

**Alertas de segurança**: [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro) permite aos clientes monitorizar o tráfego, recolher registos e analisar as origens de dados relativamente a ameaças. Além disso, o Centro de segurança do Azure acede a configuração existente de serviços do Azure para fornecer recomendações de serviço para ajudar a melhorar a postura de segurança e proteger dados pessoais e de configuração. Centro de segurança do Azure inclui um [relatório de inteligência de ameaças](https://docs.microsoft.com/azure/security-center/security-center-threat-report) para cada detetado contra ameaças para ajudar as equipes de resposta a incidentes investigar e remediar ameaças.

### <a name="business-continuity"></a>Continuidade do negócio
**Elevada disponibilidade**: cargas de trabalho do servidor são agrupadas num [conjunto de disponibilidade](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) para ajudar a garantir a elevada disponibilidade de máquinas virtuais no Azure. Pelo menos uma máquina virtual está disponível durante um evento de manutenção não planeada ou não planeada, atendendo a 99,95% do SLA do Azure.

**Cofre dos serviços de recuperação**: A [cofre dos Recovery Services](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) hospeda os dados de cópia de segurança e protege todas as configurações de máquinas virtuais do Azure nesta arquitetura. Com um cofre de serviços de recuperação, os clientes podem restaurar ficheiros e pastas a partir de uma VM de IaaS sem restaurar a VM inteira, permitindo que os tempos de restauro mais rápidos.

### <a name="logging-and-auditing"></a>Registro e auditoria
[Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) fornece o registo extensivo de atividade do sistema e do usuário, bem como o estado de funcionamento do sistema. O OMS [do Log Analytics](https://azure.microsoft.com/services/log-analytics/) solução recolhe e analisa os dados gerados pelos recursos no Azure e ambientes no local.
- **Registos de atividades**: [registos de atividades](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) fornecem informações sobre as operações executadas em recursos numa subscrição. Registos de atividades podem ajudar a determinar o iniciador de uma operação, hora da ocorrência e o estado.
- **Os registos de diagnóstico**: [registos de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) incluem todos os registos emitidos por cada recurso. Estes registos incluem registos de sistema de eventos do Windows e armazenamento de Blobs do Azure, tabelas e registos de fila.
- **Inicie sessão arquivando**: todos os registos de diagnóstico escrever para uma conta de armazenamento do Azure centralizado e criptografado para arquivamento. O período de retenção é configurável pelo utilizador, cópia de segurança e 730 dias, para atender aos requisitos de retenção de específicas da organização. Estes registos ligar ao Azure Log Analytics para processamento, armazenamento e relatórios do dashboard.

Além disso, as seguintes soluções do OMS são incluídas como parte desta arquitetura:
-   [Avaliação do AD](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): a solução da verificação do Active Directory Health avalia o risco e estado de funcionamento dos ambientes de servidor num intervalo regular e fornece uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
-   [Avaliação de antimalware](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): solução Antimalware a relatórios de status de malware, ameaças e a proteção.
-   [A automatização do Azure](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): solução a automatização do Azure armazena, executa e gere runbooks.
-   [Segurança e auditoria](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): A segurança e auditoria do dashboard fornece uma visão alto nível do Estado de segurança de recursos, fornecendo as métricas em domínios de segurança, problemas relevantes, deteções, informações sobre ameaças e consultas de segurança comuns.
-   [Avaliação do SQL](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): a solução da verificação de integridade de SQL avalia o risco e estado de funcionamento dos ambientes de servidor num intervalo regular e fornece aos clientes uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
-   [Gestão de atualizações](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): solução de gestão de atualizações permite a gestão de cliente de atualizações de segurança do sistema operativo, incluindo o processo de instalação de atualizações necessárias e o estado das atualizações disponíveis.
-   [Estado de funcionamento do agente](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): solução funcionamento de agente a relatórios quantos agentes estão implementados e a distribuição geográfica, como número de agentes que não respondem e o número de agentes que está a enviar dados operacionais.
-   [Registos de atividades do Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): ajuda a solução da análise de registos de atividade com uma análise de registos de atividade do Azure em todas as subscrições do Azure para um cliente.
-   [Controlo de alterações](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): controlo de alterações a solução permite aos clientes identificar facilmente as alterações no ambiente.

## <a name="threat-model"></a>Modelo de risco

O diagrama de fluxo de dados para esta arquitetura de referência está disponível para [transferir](https://aka.ms/gdprDWdfd) ou podem ser encontradas abaixo. Esse modelo pode ajudar os clientes a compreender os pontos do potencial risco de na infraestrutura de sistema ao fazer modificações.

![Armazém de dados para o modelo de risco do GDPR](images/gdpr-datawarehouse-threat-model.png?raw=true "armazém de dados para o modelo de risco GDPR")

## <a name="compliance-documentation"></a>Documentação de conformidade
O [Azure no Security and Compliance Blueprint – matriz de responsabilidade do cliente com o GDPR](https://aka.ms/gdprCRM) apresenta uma lista de responsabilidades de controlador e o processador para todos os artigos GDPR. Tenha em atenção que, para serviços do Azure, um cliente é, normalmente, o controlador e Microsoft atua como o processador.

O [Azure no Security and Compliance Blueprint - matriz de implementação de armazém de dados com o GDPR](https://aka.ms/gdprDW) fornece informações sobre qual GDPR artigos são resolvidos à arquitetura do armazém de dados, incluindo descrições detalhadas de como o implementação cumpre os requisitos de cada artigo coberto.

## <a name="guidance-and-recommendations"></a>Orientações e recomendações
### <a name="vpn-and-expressroute"></a>ExpressRoute e VPN
Um túnel VPN seguro ou [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) tem de ser configurado com segurança estabelecer uma ligação para os recursos implementados como parte desta arquitetura de referência do armazém de dados. Configurando adequadamente uma VPN ou ExpressRoute, os clientes podem adicionar uma camada de proteção para dados em trânsito.

Ao implementar um túnel VPN seguro com o Azure, é possível criar uma ligação privada virtual entre uma rede no local e uma rede Virtual do Azure. Essa conexão ocorre através da Internet e permite que os clientes com segurança as informações de "encapsulamento" dentro de uma ligação encriptada entre a rede do cliente e o Azure. VPN site a Site é uma tecnologia madura e segura que tenha sido implementada por empresas de todos os tamanhos há décadas. O [modo de encapsulamento IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) é utilizada nesta opção como um mecanismo de criptografia.

Porque o tráfego de túnel VPN atravessa a Internet com uma VPN de site a site, a Microsoft oferece a opção de ligação de outra, ainda mais seguro. O Azure ExpressRoute é uma WAN dedicada ligação entre o Azure e uma localização no local ou um fornecedor de alojamento do Exchange. Como as ligações ExpressRoute não entram na Internet, estas ligações oferecem mais fiabilidade, velocidades mais rápidas, latências mais baixas e maior segurança do que as ligações normais pela Internet. Além disso, como se trata de uma conexão direta do fornecedor de telecomunicação do cliente, os dados não percorre a Internet e, portanto, não são expostos ao mesmo.

Melhores práticas para implementar uma rede híbrida segura que expande uma rede no local para o Azure são [disponível](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-etl-process"></a>Processo de Extract-Transform-Load (ETL)
[O PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) pode carregar dados para o Azure SQL Data Warehouse sem a necessidade de um ETL separado ou importar a ferramenta. O PolyBase permite o acesso aos dados através de consultas de T-SQL. Da Microsoft inteligência comercial e pilha de análise, bem como ferramentas de terceiros compatíveis com o SQL Server, podem ser utilizadas com o PolyBase.

### <a name="azure-active-directory-setup"></a>Configuração do Active Directory do Azure
[O Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) é essencial para a gestão da implementação e aprovisionamento do acesso ao pessoal de interação com o ambiente. Um Windows Server Active Directory existente pode ser integrado com o AAD no [quatro cliques](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Os clientes podem associar a infraestrutura de Active Directory implementada (controladores de domínio) para um AAD existente, tornando a infraestrutura de Active Directory implementada um subdomínio de uma floresta do AAD.

### <a name="optional-services"></a>Serviços opcionais
O Azure oferece uma variedade de serviços para ajudar com o armazenamento e a preparação de dados formatados e formatados. Os seguintes serviços podem ser adicionados a esta arquitetura de referência, consoante os requisitos dos clientes:
-   [O Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) é um serviço cloud gerido criado para híbridos complexos extract-transform-load (ETL), extract-load-transform (ELT) e dados de projetos de integração. O Azure Data Factory tem capacidades para ajudar a rastreamento e localizar dados pessoais, incluindo a visualização e ferramentas para identificar quando recebe dados e de onde vieram de monitorização. Com o Azure Data Factory, os clientes podem criar e agendar fluxos de trabalho condicionados por dados denominados pipelines ingerir dados de arquivos de dados diferentes. Esses pipelines permitem aos clientes ingerir dados de origens internas e externas. Os clientes podem, em seguida, processar e transformar os dados de saída em arquivos de dados, como o Azure SQL Data Warehouse.
- Os clientes podem testar dados não estruturados na [do Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview), que permite a captura de dados de qualquer tamanho, tipo, e velocidade de ingestão num único local para análises exploratórias e operacionais.  O Azure Data Lake tem capacidades que permitem a extração e a conversão de dados. Azure Data Lake Store é compatível com mais componentes de origem no ecossistema do Hadoop e integra-se perfeitamente com outros serviços do Azure, como o Azure SQL Data Warehouse.

## <a name="disclaimer"></a>Exclusão de Responsabilidade

 - Este documento é apenas para fins informativos. A MICROSOFT FAZ NÃO OFERECE QUAISQUER GARANTIAS, EXPRESSAS, IMPLÍCITAS OU ESTATUTÁRIAS QUANTO ÀS INFORMAÇÕES CONTIDAS NESTE DOCUMENTO. Este documento é fornecido "como-está." Informações e opiniões expressados neste documento, incluindo URLs e outras referências de Web site da Internet, podem ser alteradas sem aviso prévio. Os clientes a ler este documento assume o risco de usá-lo.
 - Este documento não fornece aos clientes quaisquer direitos de propriedade intelectual em qualquer produto da Microsoft ou soluções.
 - Os clientes podem copiar e utilizar este documento para efeitos de referência interna.
 - Algumas recomendações neste documento podem resultar em maior de dados, a rede ou a utilização de recursos de computação no Azure e podem aumentar os custos de licenciamento ou de subscrição do Azure num cliente.
 - Esta arquitetura deve ser usado como uma base para os clientes que se ajustar às suas necessidades específicas e não devem ser utilizada como-está num ambiente de produção.
 - Este documento é desenvolvido como uma referência e não deve ser usado para definir todos os meios pelos quais um cliente pode atender aos requisitos de conformidade e normas. Os clientes procuram legal suporte da sua organização em implementações do cliente aprovadas.

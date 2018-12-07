---
title: Segurança do Azure e o esquema de conformidade - Analytics para o GDPR
description: Segurança do Azure e o esquema de conformidade - Analytics para o GDPR
services: security
author: jomolesk
ms.assetid: fe97b5e5-72d8-4930-bbe9-ead2b6ef3542
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: 8e4245de5d6a025fa87c8644678896596b07c49e
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "53001125"
---
# <a name="azure-security-and-compliance-blueprint-analytics-for-gdpr"></a>Azure Security and Compliance Blueprint: análise para o GDPR

## <a name="overview"></a>Descrição geral
O Regulamento de proteção de dados (GDPR) geral contém muitos requisitos sobre a recolher, armazenar e usar informações pessoais, incluindo como as organizações a identificarem e proteger dados pessoais, acomodar os requisitos de transparência, detetar e comunicar violações de dados pessoais e o pessoal de privacidade train e outros funcionários. O GDPR oferece maior controle sobre seus dados pessoais de indivíduos e impõe obrigações de novo vários de organizações que recolhem, processarem ou analisar os dados pessoais. O GDPR impõe as regras de novo em organizações que oferecem bens e serviços a pessoas localizadas na União Europeia (UE), ou que recolha e analisam dados associados a residentes da UE. O GDPR aplica-se, independentemente de onde está localizada a uma organização.

A Microsoft criou o Azure com as políticas de privacidade para salvaguardar os dados na cloud, incluindo as categorias de dados pessoais identificados pelo GDPR e medidas de segurança líderes do setor. Da Microsoft [termos contratuais](https://aka.ms/Online-Services-Terms) consolidar Microsoft para os requisitos de processadores.

Este Azure no Security and Compliance Blueprint fornece orientações para implementar uma arquitetura de análise de dados no Azure que o ajuda com os requisitos do GDPR. Esta solução demonstra maneiras em que os clientes podem satisfazer requisitos de conformidade e de segurança específicas e serve como uma base para os clientes criar e configurar suas próprias soluções de análise de dados no Azure. Os clientes podem utilizar esta arquitetura de referência e siga da Microsoft [processo de quatro etapas](https://aka.ms/gdprebook) na sua jornada para conformidade com o GDPR:
1. Detetar: Identifica os dados pessoais que existem e a respetiva localização.
2. Gerir: Regulam os dados pessoais como são utilizadas e acedidos.
3. Proteger: Estabelece controlos de segurança para prevenir, detetar e responder a vulnerabilidades e violações de dados.
4. Relatório: Manter a documentação necessária e gerir pedidos de dados e notificações de violações.

Esta arquitetura de referência, o guia de implementação associados e o modelo de risco são deve ser usado como uma base para os clientes para se adaptar às suas necessidades específicas e não devem ser utilizados como-está num ambiente de produção. Tenha em atenção o seguinte:
- A arquitetura fornece uma linha de base para ajudar os clientes a implementar cargas de trabalho para o Azure de forma compatível com o GDPR.
- Os clientes são responsáveis pela realização de segurança adequados e avaliações de conformidade de qualquer solução criada com esta arquitetura, como os requisitos podem variar com base nas especificidades da implementação de cada cliente.

## <a name="architecture-diagram-and-components"></a>Diagrama de arquitetura e componentes
Esta solução oferece uma plataforma de análise sobre a qual os clientes podem criar suas próprias ferramentas de análise. A arquitetura de referência descreve um caso de utilização genérica, onde os dados por meio de importações de dados em massa pelo administrador de dados do SQL ou por meio de atualizações de dados operacionais por meio de um utilizador operacionais de entrada clientes. Ambos os fluxos de trabalho incorporam as funções do Azure para importar dados para a base de dados do Azure SQL. As funções do Azure tem de ser configuradas pelo cliente através do portal do Azure para lidar com as tarefas de importação exclusivas para do cada cliente próprio requisitos de análise.

O Azure oferece uma variedade de serviços de relatórios e análise do cliente; No entanto, esta solução incorpora serviços do Azure Machine Learning em conjunto com a base de dados do Azure SQL para navegar rapidamente por dados e fornecer resultados mais rapidamente por meio de forma mais inteligente modelagem dos dados dos clientes. O Azure Machine Learning é um formulário do machine learning que se destina a aumentar a velocidade de consulta ao detetar relações novas entre conjuntos de dados. Depois dos dados foi treinados por meio de várias funções de estatísticas, até 7 de consulta adicionais conjuntos (8 total, incluindo o servidor de cliente) podem ser sincronizados com os mesmos modelos em tabela para distribuir a carga de trabalho de consulta e reduzir os tempos de resposta.

Para análise avançada e relatórios, bases de dados SQL do Azure podem ser configurados com índices columnstore. Bases de dados do Azure Machine Learning e o SQL do Azure podem ser aumentados vertical ou horizontalmente ou desativar completamente em resposta a utilização do cliente. Todo o tráfego SQL é encriptado com SSL através da inclusão de certificados autoassinados. Como melhor prática, o Azure recomenda a utilização de uma autoridade de certificação fidedigna para uma maior segurança.

Assim que os dados são carregados para a base de dados do SQL do Azure e formação do Azure Machine Learning, é digerida, o utilizador operacional e o administrador de dados do SQL com o Power BI. O Power BI apresenta dados de forma intuitiva e reúne informações em vários conjuntos de dados para organizar melhor maior. O alto grau de capacidade de adaptação e uma integração fácil com a base de dados SQL do Azure garante que os clientes podem configurá-lo para processar uma grande variedade de cenários, conforme exigido pelas suas necessidades de negócios.

A solução inteira é construída de armazenamento do Azure que os clientes configurar no portal do Azure. O armazenamento do Azure encripta todos os dados com o Storage Service Encryption para manter a confidencialidade dos dados em repouso. Armazenamento com redundância geográfica (GRS) garante que um evento adverso no Centro de dados principal do cliente não resultará numa perda de dados como uma segunda cópia será armazenada numa localização separada centenas de quilómetros de distância.

Para maior segurança, esta arquitetura gere os recursos com o Azure Active Directory e o Azure Key Vault. Estado de funcionamento do sistema é monitorizado através do Log Analytics e o Azure Monitor. Os clientes configurar ambos os serviços de monitorização para capturar os registos e exibir o estado de funcionamento do sistema num dashboard único, navegável facilmente.

Base de dados SQL do Azure normalmente é gerido através do SQL Server Management Studio (SSMS), que é executado a partir de uma máquina local configurada para aceder a base de dados do SQL do Azure através de uma ligação segura de VPN ou ExpressRoute. **Azure recomenda configurar uma ligação VPN ou ExpressRoute para dados de gestão e importar para o grupo de recursos de arquitetura de referência**.

![Diagrama da arquitetura de referência de análise para GDPR](images/gdpr-analytics-architecture.png?raw=true "diagrama da arquitetura de referência de análise para GDPR")

Esta solução utiliza os seguintes serviços do Azure. Os detalhes a arquitetura de implementação estão no [arquitetura de implantação](#deployment-architecture) secção.

- Funções do Azure
- Base de Dados SQL do Azure
- Azure Machine Learning
- Azure Active Directory
- Azure Key Vault
- Log Analytics
- Azure Monitor
- Storage do Azure
- Dashboard do Power BI
- Catálogo de Dados do Azure
- Centro de Segurança do Azure
- Application Insights
- Azure Event Grid
- grupos de segurança de rede

## <a name="deployment-architecture"></a>Arquitetura de implantação
A secção seguinte fornece detalhes sobre os elementos de implantação e a implementação.

**O Azure Event Grid**
[Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview) permite que os clientes podem criar facilmente aplicações com arquiteturas baseadas em eventos. Os utilizadores selecionam o recurso do Azure que gostariam de subscrever e dar o manipulador de eventos ou um ponto de extremidade para enviar o evento de webhook. Os clientes possam proteger pontos finais de webhook ao adicionar parâmetros de consulta para o URL do webhook durante a criação de uma subscrição de evento. O Azure Event Grid suporta apenas pontos finais do webhook HTTPS. O Azure Event Grid permite aos clientes controlar o nível de acesso que é atribuído aos usuários diferentes para fazer várias operações de gestão, tais como as subscrições de eventos da lista, criar novos e gerar as chaves. Grelha de eventos utiliza o controlo de acesso de controlo (RBAC).

**As funções do Azure**
[as funções do Azure](https://docs.microsoft.com/azure/azure-functions/functions-overview) é um serviço de computação sem servidor que permite aos utilizadores executar código a pedido sem ter de aprovisionar ou gerir a infraestrutura explicitamente. Utilize as Funções do Azure para executar um script ou fragmento de código em resposta a uma variedade de eventos.

**O serviço do Azure Machine Learning**
[do Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/) é uma técnica de ciência de dados que permite aos computadores utilizar dados existentes para prever futuros comportamentos, resultados e tendências.

**O catálogo de dados do Azure**: [catálogo de dados](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) torna os dados facilmente Detetáveis e compreensíveis pelos utilizadores que gerem os dados. Origens de dados comuns podem ser registadas, marcadas e está à procura os dados pessoais. Os dados permanecem na localização existente, mas uma cópia dos respetivos metadados é adicionada ao catálogo de dados, juntamente com uma referência para a localização de origem de dados. Os metadados também são indexados para tornar cada origem de dados facilmente detetável através da pesquisa e compreensível para os utilizadores que a detetarem.

### <a name="virtual-network"></a>Rede virtual
Esta arquitetura de referência define uma VNet privada com um espaço de endereços de 10.0.0.0/16.

**Grupos de segurança de rede**: [NSGs](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) contêm o controle de listas de acesso (ACLs) que permitem ou negam o tráfego numa VNet. Podem ser utilizados NSGs para proteger o tráfego numa sub-rede ou o nível VM individual. Existem os NSGs seguintes:
  - Um NSG para o Active Directory
  - Um NSG para a carga de trabalho

Cada um dos NSGs têm portas específicas e protocolos abrir para que a solução pode trabalhar de forma segura e corretamente. Além disso, as seguintes configurações são habilitadas para cada NSG:
  - [Eventos e registos de diagnóstico](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) forem ativadas e armazenados numa conta de armazenamento
  - O log Analytics está ligado a [diagnósticos do NSG](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Sub-redes**: cada sub-rede está associada ao seu NSG correspondente.

### <a name="data-in-transit"></a>Dados em trânsito
Azure encripta todas as comunicações de e para os datacenters do Azure por predefinição. A ocorrência de todas as transações para o armazenamento do Azure através do portal do Azure através de HTTPS.

### <a name="data-at-rest"></a>Dados inativos

A arquitetura protege os dados em descanso através da encriptação, a auditoria de base de dados e outras medidas.

**O armazenamento do Azure** de acordo com os dados encriptados em requisitos de rest, todos os [armazenamento do Azure](https://azure.microsoft.com/services/storage/) utiliza [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Isto ajuda a proteger e salvaguardar os dados pessoais para oferecer suporte a compromissos de segurança organizacional e requisitos de conformidade definidos pelo GDPR.

**O Azure Disk Encryption**
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) aproveita a funcionalidade BitLocker do Windows para fornecer encriptação de volume para discos de dados. A solução se integra com o Azure Key Vault para ajudar a controlar e gerir as chaves de encriptação de disco.

**Base de dados SQL do Azure**: instância de base de dados de SQL do Azure utiliza as seguintes medidas de segurança da base de dados:
-   [Autenticação e autorização AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) permite a gestão de identidades de utilizadores de base de dados e outros serviços da Microsoft num local central.
-   [A auditoria de base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) faixas de base de dados eventos e escreve-os para uma auditoria registo numa conta de armazenamento do Azure.
-   Base de dados SQL do Azure está configurado para utilizar [encriptação de dados transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), que realiza a encriptação em tempo real e a desencriptação da base de dados, cópias de segurança associadas e ficheiros de registo de transação para proteger as informações em rest. TDE fornece assurance que armazenados dados pessoais não tiver sido sujeita a acesso não autorizado.
-   [Regras de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impedir o acesso de todos os servidores de base de dados até que sejam concedidas as permissões apropriadas. A firewall concede acesso a bases de dados com base no endereço IP de origem de cada pedido.
-   [Deteção de ameaças SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) permite a deteção e resposta a ameaças potenciais à medida que ocorrem ao fornecer alertas de segurança para atividades suspeitas da base de dados, potenciais vulnerabilidades, ataques de injeção de SQL e acesso de base de dados anómalas padrões.
-   [Sempre as colunas encriptadas](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) Certifique-se de que os dados pessoais confidenciais nunca é apresentada como texto não criptografado dentro do sistema de base de dados. Depois de ativar a encriptação de dados, apenas as aplicações de cliente ou servidores de aplicações com acesso às chaves podem aceder aos dados de texto sem formatação.
- [Propriedades estendidas](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) pode ser utilizado para descontinuar o processamento de titulares dos dados, já que permite aos utilizadores adicionar propriedades personalizadas para objetos de base de dados e marcar os dados como "Discontinued" para suportar a lógica de aplicação para impedir que o processamento de associados dados pessoais.
- [Segurança ao nível da linha](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) permite aos utilizadores definir políticas para restringir o acesso aos dados de descontinuar o processamento.
- [SQL Dynamic Data Masking (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) limita a exposição de dados pessoais confidenciais ao mascará os dados para aplicações ou utilizadores sem privilégios. Ddm de autor pode detetar dados potencialmente confidenciais e sugerir as máscaras de apropriado a ser aplicada automaticamente. Isto ajuda na identificação dos dados pessoais qualificar para proteção com o GDPR e para reduzir o acesso, de modo que ele não for fechada a base de dados por meio do acesso não autorizado. **Nota: Os clientes terão de ajustar as definições de ddm de autor de cumprir para seu esquema de base de dados.**

### <a name="identity-management"></a>Gestão de identidades
As seguintes tecnologias fornecem capacidades para gerir o acesso aos dados pessoais no ambiente do Azure:
-   [O Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) é multi-inquilino com base na cloud diretório e identidade do serviço de gestão da Microsoft. Todos os utilizadores para esta solução são criados no AAD, incluindo os utilizadores que acedem a base de dados do Azure SQL.
-   Autenticação para a aplicação é executada através do AAD. Para obter mais informações, consulte [integrar aplicações com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Além disso, a encriptação de coluna de base de dados utiliza o AAD para autenticar a aplicação à base de dados do Azure SQL. Para obter mais informações, consulte como [proteger dados confidenciais na base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Do Azure com base em função de controlo de acesso (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) permite aos administradores definir permissões de acesso detalhado para conceder apenas a quantidade de acesso que os utilizadores precisam realizar seus trabalhos. Em vez de dar todas as permissões de utilizador sem restrições para recursos do Azure, os administradores podem permitir apenas certas ações para aceder aos dados pessoais. Acesso à subscrição está limitado ao administrador da subscrição.
- [AAD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) permite que os clientes minimizar o número de utilizadores que têm acesso a informações específicas, como os dados pessoais.  Os administradores podem utilizar o AAD Privileged Identity Management para detetar, restringir e monitorizar identidades privilegiadas e respetivo acesso a recursos. Esta funcionalidade também pode ser utilizada para impor acesso administrativo a pedido e just-in-time quando necessário.
-   [Proteção de identidade do AAD](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) Deteta potenciais vulnerabilidades que afetam as identidades da organização, configura as respostas automáticas para detetado ações suspeitas relacionadas com identidades de uma organização e investiga suspeita incidentes para tomar as medidas adequadas para resolvê-los.

### <a name="security"></a>Segurança
**Gestão de segredos** a solução utiliza [do Azure Key Vault](https://azure.microsoft.com/services/key-vault/) para o gerenciamento de chaves e segredos. O Cofre de Chaves do Azure ajuda a salvaguardar as chaves criptográficas e os segredos utilizados pelas aplicações em cloud e pelos serviços. As seguintes capacidades do Azure Key Vault ajudam os clientes a proteger os dados pessoais e de acesso a esses dados:
- Políticas de acesso avançadas são configuradas de forma necessidade.
- Políticas de acesso do Cofre de chaves são definidas com o mínimo permissões necessárias para chaves e segredos.
- Todas as chaves e segredos no Key Vault tem datas de expiração.
- Todas as chaves no Cofre de chaves estão protegidas por módulos de hardware especializado de segurança (HSMs). O tipo de chave é uma chave RSA de 2048 protegidas de HSM bits.
- Todos os utilizadores e identidades são concedidas as permissões mínimas necessárias através do RBAC.
- Registos de diagnóstico para o Cofre de chaves estão ativados com um período de retenção de, pelo menos, 365 dias.
- Operações de criptografia permitidas para as chaves estão limitadas aos obrigatórios.

**Alertas de segurança**: [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro) permite aos clientes monitorizar o tráfego, recolher registos e analisar as origens de dados relativamente a ameaças. Além disso, o Centro de segurança do Azure acede a configuração existente de serviços do Azure para fornecer recomendações de serviço para ajudar a melhorar a postura de segurança e proteger dados pessoais e de configuração. Centro de segurança do Azure inclui um [relatório de inteligência de ameaças](https://docs.microsoft.com/azure/security-center/security-center-threat-report) para cada detetado contra ameaças para ajudar as equipes de resposta a incidentes investigar e remediar ameaças.

### <a name="logging-and-auditing"></a>Registro e auditoria

[Log Analytics](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) fornece o registo extensivo de atividade do sistema e do usuário, bem como o estado de funcionamento do sistema. O [do Log Analytics](https://azure.microsoft.com/services/log-analytics/) solução recolhe e analisa os dados gerados pelos recursos no Azure e ambientes no local.
- **Registos de atividades**: [registos de atividades](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) fornecem informações sobre as operações executadas em recursos numa subscrição. Registos de atividades podem ajudar a determinar o iniciador de uma operação, hora da ocorrência e o estado.
- **Os registos de diagnóstico**: [registos de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) incluem todos os registos emitidos por cada recurso. Estes registos incluem registos de sistema de eventos do Windows e armazenamento de Blobs do Azure, tabelas e registos de fila.
- **Arquivo de log**: todos os registos de diagnóstico escrever para uma conta de armazenamento do Azure centralizado e criptografado para arquivamento com um período de retenção definida de 2 dias. Estes registos ligar ao Azure Log Analytics para processamento, armazenamento e relatórios do dashboard.

Além disso, as seguintes soluções de monitorização são incluídas como parte desta arquitetura:
-   [Avaliação do AD](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): a solução da verificação do Active Directory Health avalia o risco e estado de funcionamento dos ambientes de servidor num intervalo regular e fornece uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
-   [Avaliação de antimalware](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): solução Antimalware a relatórios de status de malware, ameaças e a proteção.
-   [A automatização do Azure](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): solução a automatização do Azure armazena, executa e gere runbooks.
-   [Segurança e auditoria](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): A segurança e auditoria do dashboard fornece uma visão alto nível do Estado de segurança de recursos, fornecendo as métricas em domínios de segurança, problemas relevantes, deteções, informações sobre ameaças e consultas de segurança comuns.
-   [Avaliação do SQL](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): a solução da verificação de integridade de SQL avalia o risco e estado de funcionamento dos ambientes de servidor num intervalo regular e fornece aos clientes uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
-   [Gestão de atualizações](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): solução de gestão de atualizações permite a gestão de cliente de atualizações de segurança do sistema operativo, incluindo o processo de instalação de atualizações necessárias e o estado das atualizações disponíveis.
-   [Estado de funcionamento do agente](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): solução funcionamento de agente a relatórios quantos agentes estão implementados e a distribuição geográfica, como número de agentes que não respondem e o número de agentes que está a enviar dados operacionais.
-   [Registos de atividades do Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): ajuda a solução da análise de registos de atividade com uma análise de registos de atividade do Azure em todas as subscrições do Azure para um cliente.
-   [Controlo de alterações](https://docs.microsoft.com/azure/automation/automation-change-tracking): controlo de alterações a solução permite aos clientes identificar facilmente as alterações no ambiente.

**O Azure Monitor**
[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) ajuda os clientes a acompanhar o desempenho, manter a segurança e identificar tendências, permitindo que as organizações de auditoria, criar alertas e arquivar dados, incluindo a API de controlo chama-se nos recursos dos clientes do Azure.

**O Application Insights**
[Application Insights](https://docs.microsoft.com/azure/application-insights/) é um serviço de gestão de desempenho de aplicações (APM) extensível para desenvolvedores da web em várias plataformas. Usá-lo para monitorizar a aplicação web em direto. Ele Deteta anomalias de desempenho e inclui ferramentas de análise poderosas para ajudar a diagnosticar problemas e para compreender o que os utilizadores fazem realmente com a aplicação. Foi concebido para ajudar os usuários a melhorar continuamente o desempenho e a usabilidade.

## <a name="threat-model"></a>Modelo de risco

O diagrama de fluxo de dados para esta arquitetura de referência está disponível para [transferir](https://aka.ms/gdprAnalyticsdfd) ou podem ser encontradas abaixo. Esse modelo pode ajudar os clientes a compreender os pontos do potencial risco de na infraestrutura de sistema ao fazer modificações.

![Modelo de ameaças de análise para o GDPR](images/gdpr-analytics-threat-model.png?raw=true "modelo de ameaças de análise para o GDPR")

## <a name="compliance-documentation"></a>Documentação de conformidade
O [Azure no Security and Compliance Blueprint – matriz de responsabilidade do cliente com o GDPR](https://aka.ms/gdprCRM) apresenta uma lista de responsabilidades de controlador e o processador para todos os artigos GDPR. Tenha em atenção que, para serviços do Azure, um cliente é, normalmente, o controlador e Microsoft atua como o processador.

O [Azure no Security and Compliance Blueprint - matriz de implementação de análise de dados com o GDPR](https://aka.ms/gdprAnalytics) fornece informações sobre qual GDPR artigos são resolvidos à arquitetura de análise de dados, incluindo descrições detalhadas de como o implementação cumpre os requisitos de cada artigo coberto.


## <a name="guidance-and-recommendations"></a>Orientações e recomendações
### <a name="vpn-and-expressroute"></a>ExpressRoute e VPN
Um túnel VPN seguro ou [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) tem de ser configurado com segurança estabelecer uma ligação para os recursos implementados como parte desses dados análise de arquitetura de referência. Configurando adequadamente uma VPN ou ExpressRoute, os clientes podem adicionar uma camada de proteção para dados em trânsito.

Ao implementar um túnel VPN seguro com o Azure, é possível criar uma ligação privada virtual entre uma rede no local e uma rede Virtual do Azure. Essa conexão ocorre através da Internet e permite que os clientes com segurança as informações de "encapsulamento" dentro de uma ligação encriptada entre a rede do cliente e o Azure. VPN site a Site é uma tecnologia madura e segura que tenha sido implementada por empresas de todos os tamanhos há décadas. O [modo de encapsulamento IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) é utilizada nesta opção como um mecanismo de criptografia.

Porque o tráfego de túnel VPN atravessa a Internet com uma VPN de site a site, a Microsoft oferece a opção de ligação de outra, ainda mais seguro. O Azure ExpressRoute é uma WAN dedicada ligação entre o Azure e uma localização no local ou um fornecedor de alojamento do Exchange. Como as ligações ExpressRoute não entram na Internet, estas ligações oferecem mais fiabilidade, velocidades mais rápidas, latências mais baixas e maior segurança do que as ligações normais pela Internet. Além disso, como se trata de uma conexão direta do fornecedor de telecomunicação do cliente, os dados não percorre a Internet e, portanto, não são expostos ao mesmo.

Melhores práticas para implementar uma rede híbrida segura que expande uma rede no local para o Azure são [disponível](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-etl-process"></a>Processo de Extract-Transform-Load (ETL)
[O PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) pode carregar dados para a base de dados do Azure SQL sem a necessidade de um ETL separado ou importar a ferramenta. O PolyBase permite o acesso aos dados através de consultas de T-SQL. Da Microsoft inteligência comercial e pilha de análise, bem como ferramentas de terceiros compatíveis com o SQL Server, podem ser utilizadas com o PolyBase.

### <a name="azure-active-directory-setup"></a>Configuração do Active Directory do Azure
[O Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) é essencial para a gestão da implementação e aprovisionamento do acesso ao pessoal de interação com o ambiente. Um Windows Server Active Directory existente pode ser integrado com o AAD no [quatro cliques](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Os clientes podem associar a infraestrutura de Active Directory implementada (controladores de domínio) para um AAD existente, tornando a infraestrutura de Active Directory implementada um subdomínio de uma floresta do AAD.

## <a name="disclaimer"></a>Exclusão de Responsabilidade

 - Este documento é apenas para fins informativos. A MICROSOFT FAZ NÃO OFERECE QUAISQUER GARANTIAS, EXPRESSAS, IMPLÍCITAS OU ESTATUTÁRIAS QUANTO ÀS INFORMAÇÕES CONTIDAS NESTE DOCUMENTO. Este documento é fornecido "como-está." Informações e opiniões expressados neste documento, incluindo URLs e outras referências de Web site da Internet, podem ser alteradas sem aviso prévio. Os clientes a ler este documento assume o risco de usá-lo.
 - Este documento não fornece aos clientes quaisquer direitos de propriedade intelectual em qualquer produto da Microsoft ou soluções.
 - Os clientes podem copiar e utilizar este documento para efeitos de referência interna.
 - Algumas recomendações neste documento podem resultar em maior de dados, a rede ou a utilização de recursos de computação no Azure e podem aumentar os custos de licenciamento ou de subscrição do Azure num cliente.
 - Esta arquitetura deve ser usado como uma base para os clientes que se ajustar às suas necessidades específicas e não devem ser utilizada como-está num ambiente de produção.
 - Este documento é desenvolvido como uma referência e não deve ser usado para definir todos os meios pelos quais um cliente pode atender aos requisitos de conformidade e normas. Os clientes procuram legal suporte da sua organização em implementações do cliente aprovadas.

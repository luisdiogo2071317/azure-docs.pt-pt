---
title: Segurança do Azure e conformidade Blueprint - análise para GDPR
description: Segurança do Azure e conformidade Blueprint - análise para GDPR
services: security
author: jomolesk
ms.assetid: fe97b5e5-72d8-4930-bbe9-ead2b6ef3542
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: 9114dea1f95198b5ff8dc1bf759be5d1179885fd
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="azure-security-and-compliance-blueprint-analytics-for-gdpr"></a>Blueprint de conformidade e segurança do Azure: análise para GDPR

## <a name="overview"></a>Descrição geral
O Regulamento de proteção de dados (GDPR) geral contém muitos requisitos sobre recolher, armazenar e a utilização de informações pessoais, incluindo como as organizações a identificarem e proteger os dados pessoais, acomodar os requisitos de transparência, detetarem e comunicar falhas de dados pessoais e técnicos de privacidade de formação e outros empregados. O GDPR proporciona indivíduos maior controlo sobre os respetivos dados pessoais e impõe muitos obrigações novo sobre as organizações que recolhem, identificador ou analisam os dados pessoais. O GDPR impõe novas regras sobre as organizações que ofereçam bens e serviços para as pessoas da União Europeia (EU) ou que recolher e analisam dados associados ao residentes de EU. O GDPR aplica-se, independentemente de onde está localizada numa organização.

Microsoft concebido do Azure com as políticas de privacidade para salvaguardar os dados na nuvem, incluindo as categorias de dados pessoais identificados pelo GDPR e medidas de segurança líder da indústria. Microsoft [termos contractual](http://aka.ms/Online-Services-Terms) consolidar Microsoft os requisitos de processadores.

Esta segurança do Azure e a conformidade Blueprint fornece orientações para implementar uma arquitetura de análise de dados no Azure que auxilia com requisitos do GDPR. Esta solução demonstra formas em que os clientes podem cumpre os requisitos de conformidade e segurança específica e funciona como uma base para os clientes criar e configurar as suas próprias soluções de análise de dados no Azure. Os clientes podem utilizar esta arquitetura de referência e siga da Microsoft [processo quatro passos](https://aka.ms/gdprebook) no respetivo journey a compatibilidade de GDPR:
1. Detetar: Identifique quais os dados pessoais existem e a respetiva localização.
2. Gerir: Regulam os dados pessoais como é utilizado e acedido.
3. Proteger: Estabelece controlos de segurança para evitar, detetar e responder a vulnerabilidades e falhas de dados.
4. Relatório: Manter a documentação necessária e gerir pedidos de dados e infringir notificações.

Esta arquitetura de referência, o guia de implementação associados e o modelo de ameaça destinam-se para servir como base para os clientes para se adaptar os seus requisitos específicos e não devem ser utilizados como-é num ambiente de produção. Tenha em atenção o seguinte:
- A arquitetura fornece uma linha de base para ajudar os clientes implementar cargas de trabalho no Azure de forma GDPR compatíveis.
- Os clientes são responsáveis para realizar a segurança adequada e avaliações de conformidade de qualquer solução incorporada com esta arquitetura, como os requisitos podem variar com base nas especificações de implementação de cada cliente.

## <a name="architecture-diagram-and-components"></a>Diagrama da arquitetura e componentes
Esta solução fornece uma plataforma de análise no qual os clientes podem criar as suas próprias ferramentas de análise. Arquitetura de referência descreve um caso de utilização genérico onde os clientes introduzir dados através da importação de dados em massa, o administrador de dados do SQL ou através de atualizações de dados operacionais através de um utilizador operacional. Ambos os fluxos de trabalho incorporar as funções do Azure para importar dados para a SQL Database do Azure. As funções do Azure tem de ser configuradas pelo cliente através do portal do Azure para processar as tarefas de importação exclusivas para do cada cliente seus próprio requisitos de análise.

O Azure oferece uma variedade de serviços de análises e relatórios para o cliente; No entanto, esta solução incorpora serviços do Azure Machine Learning, juntamente com a base de dados do Azure SQL rapidamente procurar através de dados e fornecer resultados mais rápidos através de mais inteligente modelação de dados de cliente. Azure Machine Learning é um formulário de aprendizagem que se destina a aumentar a velocidades de consulta ao detetar novas relações entre conjuntos de dados. Depois dos dados tem sido preparados através de várias funções estatísticas, até 7 consulta adicionais agrupamentos (8 total, incluindo o servidor de cliente) podem ser sincronizados com os mesmos modelos em tabela para propagar-se a carga de trabalho de consulta e reduzir os tempos de resposta.

Para análise avançada e relatórios, bases de dados do Azure SQL pode ser configurado com índices columnstore. Azure Machine Learning e bases de dados do Azure SQL podem ser escalado para cima ou para baixo ou encerradas completamente em resposta a utilização de cliente. Todo o tráfego SQL é encriptado com SSL através da inclusão de certificados autoassinados. Como melhor prática, o Azure recomenda a utilização de uma autoridade de certificação fidedigna para a segurança avançada.

Depois de dados são carregados para a base de dados do SQL do Azure e preparados pelo Azure Machine Learning, é digested pelo utilizador operacional e a administração de dados do SQL com o Power BI. Power BI apresenta dados intuitivo e obtém em conjunto informações entre vários conjuntos de dados para desenhar conhecimentos aprofundados maior. O nível elevado de adaptability e integração fácil com a SQL Database do Azure garante que os clientes podem configurá-lo para processar um grande número de cenários conforme exigido pelas suas necessidades de negócio.

A solução completa está criados com base no armazenamento do Azure que os clientes configurar do portal do Azure. Armazenamento do Azure encripta todos os dados com a encriptação do serviço de armazenamento para manter a confidencialidade dos dados inativos. Armazenamento redundante geográfica (GRS) assegura que um evento adverso no Centro de dados principal do cliente não resultará na perda de dados como uma segunda cópia será armazenada num local separado centenas de quilómetros ausente.

Para maior segurança, esta arquitetura gere os recursos com o Azure Active Directory e o Cofre de chaves do Azure. Estado de funcionamento do sistema é monitorizado através do Operations Management Suite (OMS) e Monitor do Azure. Os clientes configurar ambos os serviços de monitorização para capturar os registos e apresentar o estado de funcionamento do sistema num dashboard único, facilmente navegáveis dos.

Base de dados SQL do Azure normalmente é gerido através do SQL Server Management Studio (SSMS), que é executado a partir de uma máquina local configurada para aceder à base de dados do SQL do Azure através de uma ligação VPN ou ExpressRoute segura. **Importar de Azure recomenda configurar uma ligação VPN ou ExpressRoute para gestão e os dados para o grupo de recursos de arquitetura de referência**.

![Diagrama da arquitetura de referência de análise para GDPR](images/gdpr-analytics-architecture.png?raw=true "diagrama da arquitetura de referência de análise para GDPR")

Esta solução utiliza os seguintes serviços do Azure. Os detalhes da arquitetura de implementação estão no [arquitetura de implementação](#deployment-architecture) secção.

- Funções do Azure
- Base de Dados SQL do Azure
- Azure Machine Learning
- Azure Active Directory
- Azure Key Vault
- Operations Management Suite (OMS)
- Azure Monitor
- Storage do Azure
- Dashboard do Power BI
- Catálogo de Dados do Azure
- Centro de Segurança do Azure
- Application Insights
- Azure Event Grid
- grupos de segurança de rede

## <a name="deployment-architecture"></a>Arquitetura de implementação
A secção seguinte descreve em detalhe os elementos de implementação e a implementação.

**Grelha de eventos do Azure**
[grelha de eventos do Azure](https://docs.microsoft.com/en-us/azure/event-grid/overview) permite que os clientes criem facilmente aplicações com arquiteturas baseada em eventos. Os utilizadores selecionam o recurso do Azure que pretende subscrever e conceder o processador de eventos ou webhook um ponto final para enviar o evento. Os clientes podem proteger pontos finais de webhook ao adicionar parâmetros de consulta para o URL do webhook quando criar uma subscrição de evento. Grelha de eventos do Azure suporta apenas a pontos finais de webhook HTTPS. Grelha de eventos do Azure permite aos clientes controlar o nível de acesso aos diferentes utilizadores várias operações de gestão, tais como as subscrições de eventos de lista, criar novos, e gerar as chaves. Grelha de evento utiliza o controlo de acesso em funções do Azure (RBAC).

**As funções do Azure**
[das funções do Azure](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview) é um serviço de servidor sem computação, que permite aos utilizadores executar código a pedido sem ter de aprovisionar ou gerir a infraestrutura explicitamente. Utilize as Funções do Azure para executar um script ou fragmento de código em resposta a uma variedade de eventos.

**O Azure Machine Learning**
[Azure Machine Learning](https://docs.microsoft.com/en-us/azure/machine-learning/preview/) é uma técnica de ciência de dados permite que os computadores a utilizar dados existentes para prever futuros comportamentos, resultados e tendências.

**Catálogo de dados do Azure**: [catálogo de dados](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) faz com que as origens de dados facilmente Detetáveis e compreensíveis pelos utilizadores que gerem os dados. Origens de dados comuns podem ser registadas, etiquetadas e pesquisou dados pessoais. Os dados permanecem na localização existente, mas uma cópia dos metadados é adicionada ao catálogo de dados, juntamente com uma referência para a localização de origem de dados. Os metadados também são indexados para tornar cada origem de dados facilmente detetável através da pesquisa e compreensível para os utilizadores que a detetarem.

### <a name="virtual-network"></a>Rede virtual
Esta arquitetura de referência define uma VNet com um espaço de endereços de 10.0.0.0/16 privada.

**Grupos de segurança de rede**: [NSGs](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) conter acesso as listas de controlo (ACLs) que permitem ou negam o tráfego numa VNet. Os NSGs podem ser utilizados para proteger o tráfego de uma sub-rede ou nível VM individuais. Os seguintes NSGs de existir:
  - Um NSG para o Active Directory
  - Um NSG para a carga de trabalho

Cada um dos NSGs ter específicas portas e protocolos abrir para que a solução possam funcionar corretamente e em segurança. Além disso, as configurações seguintes estão ativadas para cada NSG:
  - [Os registos de diagnóstico e de eventos](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-nsg-manage-log) estão ativadas e armazenados numa conta do storage
  - Análise de registos do OMS está ligado a [diagnóstico do NSG](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Sub-redes**: cada sub-rede está associada a respetiva NSG correspondente.

### <a name="data-in-transit"></a>Dados em trânsito
Azure encripta todas as comunicações de e para os datacenters do Azure, por predefinição. Todas as transações ao Storage do Azure através do portal do Azure ocorrem através de HTTPS.

### <a name="data-at-rest"></a>Dados inativos

A arquitetura protege os dados Inativos através de encriptação, base de dados de auditoria e de outras medidas.

**Armazenamento do Azure** para satisfazer os dados encriptados em requisitos de rest, todos os [Storage do Azure](https://azure.microsoft.com/services/storage/) utiliza [encriptação do serviço de armazenamento](https://docs.microsoft.com/azure/storage/storage-service-encryption). Isto ajuda a proteger e salvaguardar os dados pessoais para suportar os compromissos de segurança organizacional e requisitos de conformidade definidos pelo GDPR.

**Azure Disk Encryption**
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) aproveita a funcionalidade de BitLocker do Windows para fornecer a encriptação de volume para discos de dados. A solução integra-se com o Cofre de chaves do Azure para ajudar a controlar e gerir as chaves de encriptação de disco.

**Base de dados SQL do Azure**: instância de base de dados de SQL do Azure utiliza as seguintes medidas de segurança da base de dados:
-   [Autorização e autenticação AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) permite a gestão de identidades de utilizadores de base de dados e outros serviços Microsoft numa localização central.
-   [Auditoria de base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) controla da base de dados eventos e escreve-los para uma auditoria iniciem sessão uma conta de armazenamento do Azure.
-   Base de dados SQL do Azure está configurado para utilizar [encriptação de dados transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), que efetua a encriptação em tempo real e a desencriptação da base de dados, cópias de segurança associadas e os ficheiros de registo de transações para proteger as informações em rest. TDE fornece assurance que armazenava os dados pessoais não foi sujeito acesso não autorizado.
-   [Regras de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impedir o acesso de todos os servidores de base de dados até que são concedidas permissões adequadas. A firewall concede acesso a bases de dados com base no endereço IP de origem de cada pedido.
-   [A deteção de ameaças do SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) permite a deteção e a resposta a potenciais ameaças à medida que ocorrem fornecendo alertas de segurança para acesso de base de dados anómalas, ataques de injeção de SQL, potenciais vulnerabilidades e atividades suspeitas da base de dados padrões.
-   [Encriptados colunas sempre](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) Certifique-se de que os dados pessoais confidenciais nunca é apresentada como texto não encriptado no interior do sistema de base de dados. Depois de ativar a encriptação de dados, apenas as aplicações de cliente ou servidores de aplicações com o acesso às chaves podem aceder aos dados de texto simples.
- [Propriedades expandidas](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) podem ser utilizados para descontinuar o processamento de assuntos de dados, que permite aos utilizadores adicionar propriedades personalizadas para objetos de base de dados e Etiquetar dados como "Discontinued" para suportar a lógica da aplicação para impedir que o processamento de associados dados pessoais.
- [Segurança ao nível da linha](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) permite aos utilizadores definir políticas para restringir o acesso aos dados para descontinuar o processamento.
- [SQL Server dinâmica dados máscara (ddm de autor)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) limita a exposição de dados pessoais confidenciais por máscara de dados para aplicações ou utilizadores não privilegiados. Ddm de autor pode automaticamente detetar dados potencialmente sensíveis e sugerir as máscaras de adequado sejam aplicadas. Isto ajuda-o com a identificação dos dados pessoais elegíveis para proteção de GDPR e para reduzir o acesso, se não for fechada a base de dados através de acesso não autorizado. **Nota: Os clientes terão de ajustar as definições de ddm de autor aderir ao respetiva esquema de base de dados.**

### <a name="identity-management"></a>Gestão de identidades
As seguintes tecnologias de fornecem capacidades para gerir o acesso a dados pessoais no ambiente do Azure:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) é multi-inquilino baseado na nuvem diretório e identidade do serviço de gestão da Microsoft. Todos os utilizadores para esta solução são criados no AAD, incluindo os utilizadores que acedem a SQL Database do Azure.
-   Autenticação para a aplicação é executada com o AAD. Para obter mais informações, consulte [integrar aplicações com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Além disso, a encriptação de coluna de base de dados utiliza o AAD para autenticar a aplicação a SQL Database do Azure. Para obter mais informações, consulte como [proteger dados sensíveis na base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure baseada em funções controlo de acesso (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) permite aos administradores definir as permissões de acesso detalhada para conceder apenas a quantidade de acesso que os utilizadores precisam desempenhar as suas funções. Em vez de fornecer todas as permissões de utilizador sem restrições para recursos do Azure, os administradores podem permitir apenas determinadas ações para aceder a dados pessoais. Acesso de subscrição está limitado ao administrador da subscrição.
- [AAD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) permite que os clientes minimizar o número de utilizadores que tenham acesso aos determinadas informações tais como os dados pessoais.  Os administradores podem utilizar o AAD Privileged Identity Management para detetar, restringir e monitorizar as identidades privilegiadas e o respetivo acesso aos recursos. Esta funcionalidade também pode ser utilizada para impor o acesso administrativo a pedido, just-in-time quando necessário.
-   [Proteção de identidade AAD](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) Deteta potenciais vulnerabilidades que afetam as identidades de uma organização, configura as respostas automatizadas detetadas ações suspeitas relacionada com identidades da organização e investiga suspeita incidentes tome medidas adequadas resolvê-los.

### <a name="security"></a>Segurança
**Gestão de segredos** a solução utiliza [Cofre de chaves do Azure](https://azure.microsoft.com/services/key-vault/) para a gestão de chaves e segredos. O Cofre de Chaves do Azure ajuda a salvaguardar as chaves criptográficas e os segredos utilizados pelas aplicações em cloud e pelos serviços. As seguintes capacidades de Cofre de chaves do Azure ajudam a proteger os dados pessoais e de acesso a esses dados de clientes:
- Políticas de acesso avançadas são configuradas de forma necessidade.
- As políticas de acesso do Cofre de chaves são definidas com permissões mínimas para chaves e segredos.
- Todas as chaves e segredos no Cofre de chaves tem datas de expiração.
- Todas as chaves no Cofre de chaves estão protegidas por módulos de hardware especializado de segurança (HSMs). O tipo de chave é uma chave RSA de 2048 protegidas de HSM bits.
- Todos os utilizadores e identidades são concedidas as permissões necessárias mínimo através do RBAC.
- Registos de diagnóstico para o Cofre de chaves estão ativados com um período de retenção de, pelo menos, a 365 dias.
- Operações de criptografia permitidas para as chaves estão limitadas às necessárias.

**Alertas de segurança**: [Centro de segurança do Azure](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro) permite aos clientes monitorizar o tráfego, recolher registos e analisar as origens de dados de ameaças. Além disso, o Centro de segurança do Azure acede a configuração existente dos serviços do Azure para fornecer recomendações de serviço para ajudar a melhorar a postura de segurança e proteger os dados pessoais e de configuração. Centro de segurança do Azure inclui um [ameaça relatório intelligence](https://docs.microsoft.com/en-us/azure/security-center/security-center-threat-report) para cada detetados ameaça para o ajudar a equipas de resposta a incidentes investigar e remedeie ameaças.

### <a name="logging-and-auditing"></a>Registo e auditoria

[Operations Management Suite (OMS)](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) fornece extenso registo de atividade de utilizador e de sistema, bem como o estado de funcionamento do sistema. O OMS [Log Analytics](https://azure.microsoft.com/services/log-analytics/) solução recolhe e analisa os dados gerados pelos recursos no Azure e ambientes no local.
- **Registos de atividade**: [registos de atividade](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) fornecem informações aprofundadas operações executadas nos recursos numa subscrição. Registos de atividade podem ajudar a determinar o iniciador de uma operação, hora de ocorrência e o estado.
- **Os registos de diagnóstico**: [registos de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) incluem todos os registos emitidos por cada recurso. Estes registos incluem registos de sistema de eventos do Windows e armazenamento de Blobs do Azure, tabelas e registos de fila.
- **Registo arquivar**: todos os registos de diagnóstico de escrita para uma conta de armazenamento do Azure centralizada e encriptados para arquivo com um período de retenção definido de 2 dias. Estes registos ligar ao Log Analytics do Azure para o processamento, armazenamento e relatórios do dashboard.

Além disso, as seguintes soluções OMS são incluídas como parte desta arquitetura:
-   [Avaliação do AD](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): O Active Directory estado de funcionamento de verificação solução avalia o risco e o estado de funcionamento de ambientes de servidor num intervalo regular e fornece uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
-   [Avaliação de antimalware](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): solução Antimalware a relatórios de estado de proteção, software maligno e ameaças.
-   [A automatização do Azure](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): solução a automatização do Azure armazena, é executado e gere runbooks.
-   [Auditoria e segurança](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): auditoria e segurança dashboard fornece uma alto nível aprofundadas sobre o estado de segurança de recursos, fornecendo as métricas em domínios de segurança, problemas relevantes, deteções, ameaças e consultas de segurança comuns.
-   [Avaliação do SQL Server](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): O estado de funcionamento de SQL Consulte solução avalia o risco e o estado de funcionamento de ambientes de servidor num intervalo regular e fornece aos clientes com uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
-   [Gestão de atualizações](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): solução de gestão de atualizações a permite a gestão de cliente de atualizações de segurança do sistema operativo, incluindo o processo de instalação de atualizações necessárias e o estado de atualizações disponíveis.
-   [Estado de funcionamento do agente](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): solução do estado de funcionamento do agente comunica quantos agentes implementados e respetiva distribuição geográfica, bem como quantos agentes que estão a responder e o número de agentes que submeter dados operacionais.
-   [Os registos de atividade do Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): solução de análise de registos de atividade o auxilia com a análise de registos a atividade do Azure em todas as subscrições do Azure para um cliente.
-   [Registo de alterações](https://docs.microsoft.com/en-us/azure/automation/automation-change-tracking): controlo de alterações a solução permite aos clientes identificar facilmente as alterações no ambiente.

**Monitor do Azure**
[Azure Monitor](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/) ajuda os clientes a monitorizar o desempenho, manter a segurança e identificar tendências, permitindo que as organizações de auditoria, criar alertas e arquivar dados, incluindo a API de controlo chamadas de recursos do Azure dos clientes.

**Application Insights**
[Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/) é um serviço de gestão de desempenho de aplicações (APM) extensível para programadores de web em várias plataformas. Utilizá-lo para monitorizar a aplicação web em direto. -Deteta anomalias de desempenho e inclui as ferramentas de análise poderosas para ajudar a diagnosticar problemas e para saber que os utilizadores, na verdade, fazer com a aplicação. Foi concebido para ajudar os utilizadores continuamente melhorar o desempenho e de Facilidade de utilização.

## <a name="threat-model"></a>Modelo de ameaça

O diagrama de fluxo de dados para esta arquitetura de referência está disponível para [transferir](https://aka.ms/gdprAnalyticsdfd) ou podem ser encontrados abaixo. Este modelo pode ajudar os clientes os pontos de risco potencial na infraestrutura do sistema quando efetuar alterações.

![Modelo de ameaça de análise para GDPR](images/gdpr-analytics-threat-model.png?raw=true "análise para GDPR modelo de ameaça")

## <a name="compliance-documentation"></a>Documentação de conformidade
O [segurança do Azure e conformidade Blueprint – GDPR cliente responsabilidade matriz](https://aka.ms/gdprCRM) lista responsabilidades de controlador e do processador para todos os artigos GDPR. Tenha em atenção que, para os serviços do Azure, um cliente é, normalmente, o controlador e Microsoft Age como o processador.

O [segurança do Azure e conformidade Blueprint - matriz de implementação de análise de dados de GDPR](https://aka.ms/gdprAnalytics) fornece informações no qual GDPR artigos são resolvidos pela arquitetura de análise de dados, incluindo descrições detalhadas do como o implementação satisfaz os requisitos de cada artigo coberto.


## <a name="guidance-and-recommendations"></a>Orientações e recomendações
### <a name="vpn-and-expressroute"></a>ExpressRoute e de VPN
Um túnel VPN seguro ou [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) tem de ser configurada em segurança estabelecer uma ligação para os recursos implementados como parte destes dados análise referenciar arquitetura. Adequadamente a configurar uma VPN ou ExpressRoute, os clientes podem adicionar uma camada de proteção de dados em trânsito.

Ao implementar um túnel VPN seguro com o Azure, é possível criar uma ligação privada virtual entre uma rede no local e uma rede Virtual do Azure. Esta ligação ocorre através da Internet e permite aos clientes de forma segura as informações de "túnel" dentro de uma ligação encriptada entre a rede do cliente e o Azure. VPN de Site a site é uma tecnologia de segura, madura que tenha sido implementada por empresas de todos os tamanhos de decades. O [modo de túnel IPsec](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) é utilizada nesta opção como um mecanismo de encriptação.

Porque o tráfego de túnel VPN atravessam a Internet e uma VPN de site a site, a Microsoft oferece a opção de ligação de outro, ainda mais seguros. O ExpressRoute do Azure é uma WAN dedicada ligação entre o Azure e uma localização no local ou um fornecedor de alojamento do Exchange. Como as ligações do ExpressRoute não passam para a Internet, estas ligações oferecem mais fiabilidade, velocidades mais rápidas, latências inferiores e uma maior segurança ligações típicas através da Internet. Além disso, porque se trata de uma ligação direta do fornecedor de telecommunication do cliente, os dados não viajam através da Internet e, por conseguinte, não estão expostos ao mesmo.

Melhores práticas para implementar uma rede híbrida segura que expande uma rede no local para o Azure são [disponíveis](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-etl-process"></a>Processo de extrair-transformação-carregamento (ETL)
[O PolyBase](https://docs.microsoft.com/en-us/sql/relational-databases/polybase/polybase-guide) pode carregar dados para o SQL Database do Azure sem a necessidade de um ETL separada ou importar a ferramenta. O PolyBase permite o acesso aos dados através de consultas de T-SQL. Da Microsoft intelligence de negócio e pilha de análise, bem como as ferramentas de terceiros compatíveis com o SQL Server, podem ser utilizadas com o PolyBase.

### <a name="azure-active-directory-setup"></a>Configuração do Active Directory do Azure
[Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-whatis) é essencial para gerir a implementação e aprovisionamento de acesso ao pessoal interagir com o ambiente. Um Windows Server Active Directory existente, pode ser integrado com o AAD no [quatro cliques](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Os clientes também podem juntar a infraestrutura do Active Directory implementada (controladores de domínio) para um AAD existente ao fazer a infraestrutura do Active Directory implementada um subdomínio de uma floresta do AAD.

## <a name="disclaimer"></a>Exclusão de Responsabilidade

 - Este documento destina-se apenas a fins informativos. MICROSOFT FAZ COM QUE NÃO OFERECE NENHUMA GARANTIA, EXPRESSAS, IMPLÍCITA OU ESTATUTARIAMENTE AS INFORMAÇÕES NESTE DOCUMENTO. Este documento é fornecido "como-está." As informações e opiniões expressados neste documento, incluindo URLs e outras referências de Web site da Internet, podem ser alteradas sem aviso prévio. Os clientes ler este documento da sua responsabilidade utilizá-la.
 - Este documento não fornece aos clientes com quaisquer direitos legais a nenhuma propriedade intelectual de qualquer produto da Microsoft ou de soluções.
 - Os clientes podem copiar e utilizar este documento para efeitos de referência interno.
 - Algumas recomendações neste documento poderão resultar numa maior dados, a rede ou a utilização de recursos de computação no Azure e podem aumentar os custos de licenciamento ou de subscrição do Azure do cliente.
 - Esta arquitetura destina-se para servir como base para os clientes ajustar os seus requisitos específicos e não devem ser utilizada como-é num ambiente de produção.
 - Este documento é desenvolvido como uma referência e não deve ser utilizado para definir todas as formas através do qual um cliente pode cumpre os requisitos de conformidade específico e regulamentos. Os clientes devem procurar suporte legal da respetiva organização em implementações de cliente aprovados.

---
title: Segurança do Azure e conformidade Blueprint - aplicação Web de IaaS para FedRAMP
description: Segurança do Azure e conformidade Blueprint - aplicação Web de IaaS para FedRAMP
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 8fe47cff-9c24-49e0-aa11-06ff9892a468
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2018
ms.author: jomolesk
ms.openlocfilehash: 6a2a72f46c4d5faacb7d5871f4c917a5cd578e96
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809170"
---
# <a name="azure-security-and-compliance-blueprint-iaas-web-application-for-fedramp"></a>Segurança do Azure e conformidade Blueprint: aplicação Web de IaaS para FedRAMP

## <a name="overview"></a>Descrição geral

O [risco Federal e o programa de gestão de autorização (FedRAMP)](https://www.fedramp.gov) é um programa de toda a government E.U.A. que fornece uma abordagem padronizada para avaliação de segurança, autorização e monitorização contínua para produtos de nuvem e serviços. Esta segurança do Azure e a conformidade Blueprint automatização fornece orientações para a implementação de uma infraestrutura em conformidade FedRAMP como um ambiente de serviço (IaaS) adequado para uma simple aplicação de web para a Internet. Esta solução automatiza a implementação e configuração de recursos do Azure para uma arquitetura de referência comum, que demonstra formas em que os clientes podem cumprir os requisitos específicos de segurança e conformidade e funciona como uma foundation para que os clientes criem e Configure as suas próprias soluções no Azure. A solução implementa um subconjunto dos controlos da linha de base FedRAMP elevado, com base no SP do NIST 800-53. Para obter mais informações sobre requisitos de FedRAMP e esta solução, consulte o [documentação de conformidade](#compliance-documentation).
> [!NOTE]
> Esta solução implementa Azure Government.

Esta segurança do Azure e a conformidade Blueprint automatização implementa automaticamente uma arquitetura de referência de aplicação web IaaS com controlos de segurança pré-configurada para ajudar os clientes obter a conformidade com os requisitos de FedRAMP. A solução é constituído por modelos Azure Resource Manager e scripts do PowerShell que guiam de implementação de recursos e a configuração.

Esta arquitetura destina-se para servir como base para os clientes ajustar os seus requisitos específicos e não devem ser utilizada como-é num ambiente de produção. Não é suficiente para completamente cumprir os requisitos de linha de base FedRAMP alta implementar uma aplicação para este ambiente sem modificação. Tenha em atenção o seguinte:
- Esta arquitetura fornece uma linha de base para ajudar os clientes a utilizar o Azure de forma FedRAMP compatíveis.
- Os clientes são responsáveis para realizar a segurança adequada e avaliação de compatibilidade de qualquer solução incorporada com esta arquitetura, como os requisitos podem variar com base nas especificações de implementação de cada cliente.

Para uma descrição geral de como funciona esta solução, veja este [vídeo](https://aka.ms/fedrampblueprintvideo) explicar e demonstrar a implementação.

Clique em [aqui](https://aka.ms/fedrampblueprintrepo) para obter instruções de implementação.

## <a name="architecture-diagram-and-components"></a>Diagrama da arquitetura e componentes
Esta solução implementa uma arquitetura de referência para uma aplicação web de IaaS com um back-end do SQL Server. A arquitetura inclui uma camada web, a camada de dados, infraestrutura do Active Directory, Gateway de aplicação e Balanceador de carga. Máquinas virtuais implementadas para os escalões web e os dados estão configuradas num conjunto de disponibilidade e instâncias do SQL Server estão configuradas num grupo de Disponibilidade AlwaysOn para elevada disponibilidade. Máquinas virtuais estão associados a um domínio e as políticas de grupo do Active Directory são utilizadas para impor a segurança e conformidade configurações ao nível do sistema operativo. Um anfitrião de bastion fornece uma ligação segura para os administradores de recursos de acesso implementado. **Azure recomenda configurar uma ligação VPN ou Azure ExpressRoute para importação de dados e de gestão para a sub-rede de arquitetura de referência.**

![Aplicação Web de IaaS para o diagrama de arquitetura de referência FedRAMP](images/fedramp-iaaswa-architecture.png?raw=true "aplicação Web de IaaS para FedRAMP diagrama da arquitetura de referência")

Esta solução utiliza os seguintes serviços do Azure. Detalhes da arquitetura de implementação estão localizados no [arquitetura de implementação](#deployment-architecture) secção.

- Máquinas Virtuais do Azure
    - (1) o anfitrião de bastion (Windows Server 2016 Datacenter)
    - (2) controlador de domínio o active Directory (Windows Server 2016 Datacenter)
    - (2) nó de cluster do SQL Server (SQL Server 2017 no Windows Server 2016)
    - (2) web/IIS (Centro de dados do Windows Server 2016)
- Conjuntos de Disponibilidade
    - (1) controladores de domínio o active Directory
    - (1) nós de cluster do SQL Server
    - (1) web/IIS
- Rede Virtual do Azure
    - redes virtuais /16 (1)
    - sub-redes /24 (5)
    - As definições de DNS estão definidas em ambos os controladores de domínio
- Azure Load Balancer
- Gateway de Aplicação do Azure
    - (1) Gateway de aplicação WAF ativada
        - modo de firewall: prevenção
        - Conjunto de regras: OWASP 3.0
        - serviço de escuta: porta 443
- Storage do Azure
    - (7) as contas de armazenamento georredundante
- Testemunho de nuvem do Azure
- Cofre dos Serviços de Recuperação
- Azure Key Vault
- Azure Active Directory (Azure AD)
- Azure Resource Manager
- Operations Management Suite (OMS)
- Azure Monitor

## <a name="deployment-architecture"></a>Arquitetura de implementação

A secção seguinte descreve em detalhe os elementos de desenvolvimento e implementação.

**Anfitrião de bastion**: O anfitrião de bastion for o único ponto de entrada que fornece uma ligação segura para os administradores de recursos de acesso implementado. O bastion NSG do anfitrião permite ligações só na porta TCP 3389 para RDP. Os clientes podem configurar ainda mais o anfitrião de bastion para satisfazer os requisitos de proteção do sistema de organização.

### <a name="virtual-network"></a>Rede virtual
A arquitetura define uma rede privada virtual com um espaço de endereços de 10.200.0.0/16.

**Grupos de segurança de rede**: esta solução implementa recursos uma arquitetura com uma sub-rede separada web, uma sub-rede de base de dados, sub-rede do Active Directory e sub-rede de gestão dentro de uma rede virtual. Sub-redes logicamente estão separadas por regras de grupos de segurança de rede aplicadas às sub-redes individuais para restringir o tráfego entre sub-redes de forma a que só é necessário para a funcionalidade de gestão e de sistema.

Consulte a configuração para [grupos de segurança de rede](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) implementado com esta solução. Os clientes podem configurar grupos de segurança de rede editando o ficheiro acima utilizando [esta documentação](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) como guia.

Cada um das sub-redes tem um grupo de segurança de rede dedicado (NSG):
- 1 NSG para Gateway de aplicação (LBNSG)
- 1 NSG para o anfitrião de bastion (MGTNSG)
- 1 NSG principais e cópia de segurança para controladores de domínio (ADNSG)
- 1 NSG para servidores SQL (SQLNSG)
- 1 NSG para a camada Web (WEBNSG)

**Sub-redes**: cada sub-rede está associada a respetiva NSG correspondente.

### <a name="data-at-rest"></a>Dados inativos

A arquitetura protege os dados Inativos utilizando várias medidas de encriptação.

**Armazenamento do Azure**: para satisfazer os requisitos de encriptação de dados em rest, todas as contas de armazenamento utilizam [encriptação do serviço de armazenamento](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

**SQL Server**: SQL Server está configurado para utilizar [encriptação de dados transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption), que efetua a encriptação em tempo real e a desencriptação de dados e registo de ficheiros para proteger informações inativos. TDE fornece assurance que armazenados os dados não foi sujeito acesso não autorizado.

Os clientes também podem configurar as seguintes medidas de segurança do SQL Server:
-   [Autorização e autenticação AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) permite a gestão de identidades de utilizadores de base de dados e outros serviços Microsoft numa localização central.
-   [Auditoria de base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) controla da base de dados eventos e escreve-los para uma auditoria iniciem sessão uma conta de armazenamento do Azure.
-   [Regras de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impedir o acesso de todos os servidores de base de dados até que são concedidas permissões adequadas. A firewall concede acesso a bases de dados com base no endereço IP de origem de cada pedido.
-   [A deteção de ameaças do SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) permite a deteção e a resposta a potenciais ameaças à medida que ocorrem fornecendo alertas de segurança para acesso de base de dados anómalas, ataques de injeção de SQL, potenciais vulnerabilidades e atividades suspeitas da base de dados padrões.
-   [Sempre encriptado colunas](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) Certifique-se de que os dados confidenciais nunca é apresentada como texto não encriptado no interior do sistema de base de dados. Depois de ativar a encriptação de dados, apenas as aplicações de cliente ou servidores de aplicações com o acesso às chaves podem aceder aos dados de texto simples.
-   [Máscara de dados dinâmicos da base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) podem ser efetuados após implementa a arquitetura de referência. Os clientes terão de ajustar as definições de aderir ao respetiva esquema de base de dados de máscara de dados dinâmicos.

**Azure Disk Encryption**: Azure Disk Encryption é utilizado para encriptados discos da máquina virtual IaaS do Windows. [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) aproveita a funcionalidade de BitLocker do Windows para fornecer a encriptação de volume para o SO e discos de dados. A solução é integrada com o Cofre de chaves do Azure para ajudar a controlar e gerir as chaves de encriptação de disco.

### <a name="identity-management"></a>Gestão de identidades

As seguintes tecnologias de fornecem a identidade de capacidades de gestão no ambiente do Azure:
- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) é multi-inquilino baseado na nuvem diretório e identidade do serviço de gestão da Microsoft.
- Autenticação para uma aplicação web implementada de cliente pode ser efetuada utilizando o Azure AD. Para obter mais informações, consulte [integrar aplicações com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  
- [Azure baseada em funções controlo de acesso (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) permite precisamente direcionadas para gestão de acesso ao Azure. Acesso de subscrição está limitado ao administrador da subscrição e acesso a recursos pode ser limitado com base na função de utilizador.
- Uma instância do Active Directory IaaS implementada fornece gestão de identidades ao nível do SO para as máquinas virtuais do IaaS implementadas.

### <a name="security"></a>Segurança
**Gestão de segredos**: A solução utiliza [Cofre de chaves do Azure](https://azure.microsoft.com/services/key-vault/) para a gestão de chaves e segredos. O Cofre de Chaves do Azure ajuda a salvaguardar as chaves criptográficas e os segredos utilizados pelas aplicações em cloud e pelos serviços. O Cofre de chaves do Azure ajuda a gerir as chaves de encriptação de disco de máquina virtual IaaS e segredos para esta arquitetura de referência.

**Aplicar o patch gestão**: implementadas por esta segurança do Azure e a conformidade Blueprint automatização de máquinas virtuais do Windows estão configuradas por predefinição para receber as atualizações automáticas do serviço de atualização do Windows. Esta solução também implementa a solução de automatização do Azure através do qual as implementações de atualização podem ser criadas para implementar patches nos servidores do Windows quando for necessário.

**Proteção contra software maligno**: [Antimalware da Microsoft](https://docs.microsoft.com/azure/security/azure-security-antimalware) para máquinas virtuais fornece a capacidade de proteção em tempo real que ajuda a identifica e remove vírus, spyware e outro software malicioso, com alertas configuráveis ao conhecido software malicioso ou indesejável, tentar instalar ou executar em máquinas virtuais protegidas.

**Gateway de aplicação**: arquitetura reduz o risco de vulnerabilidades de segurança com um Gateway de aplicação de firewall de aplicações web (WAF) e o ruleset OWASP ativado. Capacidades adicionais incluem:

- [End-to-End-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Ativar [descarga de SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Desativar [TLS v 1.0 e v 1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Firewall de aplicações Web](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (modo WAF)
- [Modo de prevenção](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) com ruleset OWASP 3.0

### <a name="business-continuity"></a>Continuidade do negócio

**Elevada disponibilidade**: pelo menos uma máquina virtual está disponível durante um evento de manutenção planeada ou, reunião de 99,95% SLA do Azure. A solução implementa todas as camadas da web e dados de máquinas virtuais da camada um [do conjunto de disponibilidade](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Conjuntos de disponibilidade Certifique-se de que as máquinas virtuais estão distribuídas por vários clusters de hardware isolado para melhorar a disponibilidade. Além disso, esta solução implementa as máquinas virtuais do SQL Server num conjunto de disponibilidade como um [grupo de Disponibilidade AlwaysOn](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview). Fornece a funcionalidade de grupo Always On disponibilidade para as capacidades de elevada disponibilidade e recuperação após desastre.

**O Cofre de serviços de recuperação**: O [cofre dos serviços de recuperação](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) aloja dados de cópia de segurança e protege todas as configurações de máquinas virtuais do Azure nesta arquitetura. Com um cofre de serviços de recuperação, os clientes restaurar ficheiros e pastas a partir de uma VM do IaaS sem restaurar toda a VM, permitindo tempos de restauro mais rápidos.

**Nuvem testemunho**: [nuvem testemunho](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) é um tipo de testemunho de quórum do Cluster de ativação pós-falha no Windows Server 2016, que tira partido do Azure como o ponto de arbitragem. O testemunho de nuvem, tal como outro testemunho de quórum, obtém um voto e pode participar nos cálculos do quórum, mas utiliza o padrão publicamente disponíveis Blob Storage do Azure. Isto elimina os custos de manutenção extra VMs alojada numa nuvem pública.

### <a name="logging-and-auditing"></a>Registo e auditoria

OMS fornece extenso registo de atividade de utilizador e de sistema, bem como o estado de funcionamento do sistema. O OMS [Log Analytics](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) solução recolhe e analisa os dados gerados pelos recursos no Azure e ambientes no local.

- **Registos de atividade:**[registos de atividade](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) fornecem informações aprofundadas operações executadas nos recursos numa subscrição. Registos de atividade podem ajudar a determinar o iniciador de uma operação, hora de ocorrência e o estado.
- **Os registos de diagnóstico:**[registos de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) são emitidos por cada recurso de todos os registos. Estes registos incluem registos de sistema de eventos do Windows, os registos de armazenamento do Azure, os registos de auditoria do Cofre de chaves e os registos de acesso e de firewall de Gateway de aplicação.
- **Arquivo de registo:** todos os registos de diagnóstico de escrita para uma conta de armazenamento do Azure centralizada e encriptados para arquivo. O período de retenção é configurável pelo utilizador, cópia de segurança para dias 730, para satisfazer os requisitos de retenção específico da organização. Estes registos ligar ao Log Analytics do Azure para o processamento, armazenamento e relatórios do dashboard.

Além disso, as seguintes soluções OMS são instaladas como parte desta arquitetura. Tenha em atenção que se trata de responsabilidade do cliente para configurar estas soluções para alinhar com os controlos de segurança de FedRAMP:
-   [Avaliação do AD](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): O Active Directory estado de funcionamento de verificação solução avalia o risco e o estado de funcionamento de ambientes de servidor num intervalo regular e fornece uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
-   [Avaliação de antimalware](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): solução Antimalware a relatórios de estado de proteção, software maligno e ameaças.
-   [A automatização do Azure](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): solução a automatização do Azure armazena, é executado e gere runbooks.
-   [Auditoria e segurança](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): auditoria e segurança dashboard fornece uma alto nível aprofundadas sobre o estado de segurança de recursos, fornecendo as métricas em domínios de segurança, problemas relevantes, deteções, ameaças e consultas de segurança comuns.
-   [Avaliação do SQL Server](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): O estado de funcionamento de SQL Consulte solução avalia o risco e o estado de funcionamento de ambientes de servidor num intervalo regular e fornece aos clientes com uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
-   [Gestão de atualizações](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): solução de gestão de atualizações a permite a gestão de cliente de atualizações de segurança do sistema operativo, incluindo o processo de instalação de atualizações necessárias e o estado de atualizações disponíveis.
-   [Estado de funcionamento do agente](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): solução do estado de funcionamento do agente comunica quantos agentes implementados e respetiva distribuição geográfica, bem como quantos agentes que estão a responder e o número de agentes que submeter dados operacionais.
-   [Os registos de atividade do Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): solução de análise de registos de atividade o auxilia com a análise de registos a atividade do Azure em todas as subscrições do Azure para um cliente.
-   [Registo de alterações](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): controlo de alterações a solução permite aos clientes identificar facilmente as alterações no ambiente.

**Monitor do Azure**
[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) ajuda os utilizadores a monitorizar o desempenho, manter a segurança e identificar tendências, permitindo que as organizações de auditoria, criar alertas e arquivar dados, incluindo chamadas da API de controlo nos recursos do Azure dos clientes.

## <a name="threat-model"></a>Modelo de ameaça
O diagrama de fluxo de dados para esta arquitetura de referência está disponível para [transferir](https://aka.ms/fedrampWAdfd) ou podem ser encontrados abaixo. Este modelo pode ajudar os clientes os pontos de risco potencial na infraestrutura do sistema quando efetuar alterações.

![Aplicação Web de IaaS para o modelo de ameaça FedRAMP](images/fedramp-iaaswa-threat-model.png?raw=true "IaaS Web Applicaiton para o modelo de ameaça FedRAMP")

## <a name="compliance-documentation"></a>Documentação de conformidade

O [segurança do Azure e conformidade Blueprint - FedRAMP elevada cliente responsabilidade matriz](https://aka.ms/blueprinthighcrm) apresenta uma lista de todos os controlos de segurança necessários para a linha de base FedRAMP elevado. A matriz indica se a implementação de cada controlo é da responsabilidade do Microsoft, o cliente, ou partilhados entre os dois.

O [segurança do Azure e conformidade Blueprint - FedRAMP IaaS Web aplicação elevada controlo implementação matriz](https://aka.ms/blueprintwacim) apresenta uma lista de todos os controlos de segurança necessários para a linha de base FedRAMP elevado. A matriz fornece informações no qual os controlos estão abrangidos pela arquitetura de aplicação da web IaaS, incluindo como a implementação cumpre os requisitos de cada controlo coberto obter descrições detalhadas.

## <a name="deploy-the-solution"></a>Implementar a solução

Esta segurança do Azure e a conformidade Blueprint automatização é composta por ficheiros de configuração JSON e scripts do PowerShell que são processados pelo serviço de API do Azure Resource Manager para implementar recursos no Azure. Estão disponíveis instruções de implementação detalhados [aqui](https://aka.ms/fedrampblueprintrepo).
> [!NOTE]
> Esta solução implementa Azure Government.

#### <a name="quickstart"></a>Início Rápido
1. Clonar ou transferir [isto](https://aka.ms/fedrampblueprintrepo) repositório do GitHub para a estação de trabalho local.

2. Execute o script do PowerShell de pré-implementação: azure-blueprint/predeploy/Orchestration_InitialSetup.ps1.

3. Clique no botão abaixo, inicie sessão no portal do Azure, introduza os parâmetros do modelo ARM necessários e clique em **Compra**.

    [![Implementar no Azure](http://azuredeploy.net/AzureGov.png)](https://portal.azure.us/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Ffedramp-iaas-webapp%2Fmaster%2Fazuredeploy.json)

## <a name="guidance-and-recommendations"></a>Orientações e recomendações
### <a name="vpn-and-expressroute"></a>ExpressRoute e de VPN
Um túnel VPN seguro ou [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) tem de ser configurado com segurança estabelecer uma ligação para os recursos implementados como parte da arquitetura de referência aplicação Web de IaaS. Adequadamente a configurar uma VPN ou ExpressRoute, os clientes podem adicionar uma camada de proteção de dados em trânsito.

Ao implementar um túnel VPN seguro com o Azure, é possível criar uma ligação privada virtual entre uma rede no local e uma rede Virtual do Azure. Esta ligação ocorre através da Internet e permite aos clientes de forma segura as informações de "túnel" dentro de uma ligação encriptada entre a rede do cliente e o Azure. VPN de Site a site é uma tecnologia de segura, madura que tenha sido implementada por empresas de todos os tamanhos de decades. O [modo de túnel IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) é utilizada nesta opção como um mecanismo de encriptação.

Porque o tráfego de túnel VPN atravessam a Internet e uma VPN de site a site, a Microsoft oferece a opção de ligação de outro, ainda mais seguros. O ExpressRoute do Azure é uma WAN dedicada ligação entre o Azure e uma localização no local ou um fornecedor de alojamento do Exchange. Como as ligações do ExpressRoute não passam para a Internet, estas ligações oferecem mais fiabilidade, velocidades mais rápidas, latências inferiores e uma maior segurança ligações típicas através da Internet. Além disso, porque se trata de uma ligação direta do fornecedor de telecommunication do cliente, os dados não viajam através da Internet e, por conseguinte, não estão expostos ao mesmo.

Melhores práticas para implementar uma rede híbrida segura que expande uma rede no local para o Azure são [disponíveis](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Exclusão de Responsabilidade

- Este documento destina-se apenas a fins informativos. MICROSOFT FAZ COM QUE NÃO OFERECE NENHUMA GARANTIA, EXPRESSAS, IMPLÍCITA OU ESTATUTARIAMENTE AS INFORMAÇÕES NESTE DOCUMENTO. Este documento é fornecido "como-está." As informações e opiniões expressados neste documento, incluindo URLs e outras referências de Web site da Internet, podem ser alteradas sem aviso prévio. Os clientes ler este documento da sua responsabilidade utilizá-la.  
- Este documento não fornece aos clientes com quaisquer direitos legais a nenhuma propriedade intelectual de qualquer produto da Microsoft ou de soluções.  
- Os clientes podem copiar e utilizar este documento para efeitos de referência interno.  
- Algumas recomendações neste documento poderão resultar numa maior dados, a rede ou a utilização de recursos de computação no Azure e podem aumentar os custos de licenciamento ou de subscrição do Azure do cliente.  
- Esta arquitetura destina-se para servir como base para os clientes ajustar os seus requisitos específicos e não devem ser utilizada como-é num ambiente de produção.
- Este documento é desenvolvido como uma referência e não deve ser utilizado para definir todas as formas através do qual um cliente pode cumpre os requisitos de conformidade específico e regulamentos. Os clientes devem procurar suporte legal da respetiva organização em implementações de cliente aprovados.

---
title: Segurança do Azure e o esquema de conformidade - aplicação Web de IaaS para GDPR
description: Segurança do Azure e o esquema de conformidade - aplicação Web de IaaS para GDPR
services: security
author: jomolesk
ms.assetid: 04d5239c-fff0-4c2d-9379-1fa79ddbec78
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: 08e078841a6fb7f43db6d052ecefca3aabec8687
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/14/2019
ms.locfileid: "54263378"
---
# <a name="azure-security-and-compliance-blueprint---iaas-web-application-for-gdpr"></a>Segurança do Azure e o esquema de conformidade - aplicação Web de IaaS para GDPR

## <a name="overview"></a>Descrição geral
O Regulamento de proteção de dados (GDPR) geral contém muitos requisitos sobre a recolher, armazenar e usar informações pessoais, incluindo como as organizações a identificarem e proteger dados pessoais, acomodar os requisitos de transparência, detetar e comunicar violações de dados pessoais e o pessoal de privacidade train e outros funcionários. O GDPR oferece maior controle sobre seus dados pessoais de indivíduos e impõe obrigações de novo vários de organizações que recolhem, processarem ou analisar os dados pessoais. O GDPR impõe as regras de novo em organizações que oferecem bens e serviços a pessoas localizadas na União Europeia (UE), ou que recolha e analisam dados associados a residentes da UE. O GDPR aplica-se, independentemente de onde está localizada a uma organização.

A Microsoft criou o Azure com as políticas de privacidade para salvaguardar os dados na cloud, incluindo as categorias de dados pessoais identificados pelo GDPR e medidas de segurança líderes do setor. Da Microsoft [termos contratuais](https://aka.ms/Online-Services-Terms) consolidar Microsoft para os requisitos de processadores.

Este Azure no Security and Compliance Blueprint fornece orientações para implementar uma infraestrutura como um ambiente de serviço (IaaS) adequado para um simple aplicativo de web de acesso à Internet. Esta solução demonstra maneiras em que os clientes podem satisfazer os requisitos específicos de segurança e conformidade o gdpr e serve como uma base para os clientes criar e configurar suas próprias soluções de aplicação web IaaS no Azure. Os clientes podem utilizar esta arquitetura de referência e siga da Microsoft [processo de quatro etapas](https://aka.ms/gdprebook) na sua jornada para conformidade com o GDPR:
1. Detete: Identifica os dados pessoais que existem e a respetiva localização.
2. Gestão: Governar os dados pessoais como são utilizadas e acedidos.
3. Proteger: Estabelece os controlos de segurança para prevenir, detetar e responder a vulnerabilidades e violações de dados.
4. Relatório: Manter a documentação necessária e gerir pedidos de dados e notificações de violações.

Esta arquitetura de referência, o guia de implementação associados e o modelo de risco são deve ser usado como uma base para os clientes para se adaptar às suas necessidades específicas e não devem ser utilizados como-está num ambiente de produção. Tenha em atenção o seguinte:
- A arquitetura fornece uma linha de base para ajudar os clientes a implementar cargas de trabalho para o Azure de forma compatível com o GDPR.
- Os clientes são responsáveis pela realização de segurança adequados e avaliações de conformidade de qualquer solução criada com esta arquitetura, como os requisitos podem variar com base nas especificidades da implementação de cada cliente.

## <a name="architecture-diagram-and-components"></a>Diagrama de arquitetura e componentes
Esta solução implementa uma arquitetura de referência para um aplicativo da web de IaaS com um back-end do SQL Server. A arquitetura inclui uma camada web, camada de dados, infra-estrutura do Active Directory, Gateway de aplicação e Balanceador de carga. Máquinas virtuais implementadas para os escalões web e os dados são configuradas num conjunto de disponibilidade e instâncias do SQL Server são configuradas num grupo de Disponibilidade AlwaysOn para elevada disponibilidade. As máquinas virtuais estão associados a um domínio e políticas de grupo do Active Directory são utilizadas para impor configurações de segurança e conformidade ao nível do sistema operativo. Anfitrião de bastião de gestão fornece uma ligação segura para os administradores de recursos de acesso implementado. **Azure recomenda configurar uma ligação VPN ou ExpressRoute para importação de dados e de gestão para a sub-rede de arquitetura de referência.**

![Aplicação Web de IaaS para o diagrama de arquitetura de referência do GDPR](images/gdpr-iaaswa-architecture.png?raw=true "aplicativo da Web de IaaS para o diagrama de arquitetura de referência GDPR")

Esta solução utiliza os seguintes serviços do Azure. Detalhes da arquitetura de implementação estão localizadas no [arquitetura de implantação](#deployment-architecture) secção.

- Máquinas Virtuais do Azure
    - (1) gestão/bastion (Windows Server 2016 Datacenter)
    - (2) controlador de domínio do Active Directory (Windows Server 2016 Datacenter)
    - (2) nó de cluster o SQL Server (SQL Server 2017 no Windows Server 2016)
    - (2) web/IIS (Centro de dados do Windows Server 2016)
- Conjuntos de Disponibilidade
    - (1) controladores de domínio o active Directory
    - (1) nós de cluster do SQL
    - (1) web/IIS
- Rede Virtual do Azure
    - redes virtuais do /16 (1)
    - sub-redes de /24 (5)
    - As definições de DNS estão definidas para os dois controladores de domínio
- Azure Load Balancer
- Gateway de Aplicação do Azure
    - (1) Gateway de aplicação WAF ativado
        - Modo de firewall: prevenção
        - Conjunto de regras: OWASP 3.0
        - serviço de escuta: porta 443
- Storage do Azure
    - (7) contas de armazenamento com redundância geográfica
- Testemunho de nuvem
- Cofre dos Serviços de Recuperação
- Azure Key Vault
- Azure Active Directory (AAD)
- Azure Resource Manager
- Log Analytics
- Centro de Segurança do Azure

## <a name="deployment-architecture"></a>Arquitetura de implantação
A secção seguinte fornece detalhes sobre os elementos de implantação e a implementação.

**Anfitrião de bastião**: O anfitrião de bastião é o único ponto de entrada que permite aos utilizadores aceder os recursos implementados neste ambiente. O anfitrião de bastião fornece uma ligação segura a recursos implementados, permitindo apenas tráfego remoto a partir de endereços IP públicos numa lista segura. Para permitir o tráfego de (protocolo RDP) de área de trabalho remoto, a origem do tráfego deve ser definido no grupo de segurança de rede (NSG).

Esta solução cria uma máquina virtual como um anfitrião de bastião associados a um domínio com as seguintes configurações:
-   [Extensão de antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Extensão do log Analytics](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Extensão de diagnóstico do Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [O Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) com o Azure Key Vault
-   Uma [política de encerramento automático](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) para reduzir o consumo de recursos de máquina virtual quando não está em utilização
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) ativado para que as credenciais e outros segredos executar num ambiente protegido, que está isolado do sistema operativo em execução

### <a name="virtual-network"></a>Rede virtual
A arquitetura define uma rede privada virtual com um espaço de endereços de 10.200.0.0/16.

**Grupos de segurança de rede**: Esta solução implementa recursos numa arquitetura com uma sub-rede de web separado, uma sub-rede de base de dados, sub-rede do Active Directory e a sub-rede de gestão na mesma rede virtual. Sub-redes logicamente são separados por aplicadas às sub-redes individuais para restringir o tráfego entre sub-redes para que apenas necessário para a funcionalidade de gerenciamento do sistema e de regras de grupo de segurança de rede.

Consulte a configuração para [grupos de segurança de rede](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) implementado com esta solução. As organizações podem configurar grupos de segurança de rede ao editar o ficheiro acima usando [esta documentação](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) como guia.

Cada um das sub-redes tem um grupo de segurança de rede dedicada (NSG):
- 1 NSG para o Gateway de aplicação (LBNSG)
- 1 NSG para o anfitrião de bastião (MGTNSG)
- 1 NSG para controladores de domínio primário e de cópia de segurança (ADNSG)
- 1 NSG para servidores SQL e o testemunho de nuvem (SQLNSG)
- 1 NSG para a camada web (WEBNSG)

### <a name="data-in-transit"></a>Dados em trânsito
Azure encripta todas as comunicações de e para os datacenters do Azure por predefinição. Além disso, todas as transações de armazenamento do Azure através do portal do Azure ocorrem através de HTTPS.

### <a name="data-at-rest"></a>Dados inativos
A arquitetura protege dados em repouso através de várias medidas, incluindo encriptação e a auditoria de base de dados.

**O armazenamento do Azure**: Para atender aos dados criptografados em requisitos de rest, todos os [armazenamento do Azure](https://azure.microsoft.com/services/storage/) utiliza [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Isto ajuda a proteger e salvaguardar os dados pessoais para oferecer suporte a compromissos de segurança organizacional e requisitos de conformidade definidos pelo GDPR.

**Azure Disk Encryption**: O Azure Disk Encryption é utilizado para discos de máquinas virtuais de IaaS do Windows criptografados. [O Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) aproveita a funcionalidade BitLocker do Windows para fornecer encriptação de volume para o SO e discos de dados. A solução está integrada no Azure Key Vault para ajudar a controlar e gerir as chaves de encriptação de disco.

**SQL Server**: Instância do SQL Server usa as seguintes medidas de segurança da base de dados:
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
-   [O Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) é multi-inquilino com base na cloud diretório e identidade do serviço de gestão da Microsoft. Todos os utilizadores para esta solução são criados no AAD, incluindo os utilizadores a aceder ao servidor SQL.
-   Autenticação para a aplicação é executada através do AAD. Para obter mais informações, consulte [integrar aplicações com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Além disso, a encriptação de coluna de base de dados utiliza o AAD para autenticar a aplicação para o SQL Server. Para obter mais informações, consulte como [proteger dados confidenciais na base de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Do Azure com base em função de controlo de acesso (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) permite aos administradores definir permissões de acesso detalhado para conceder apenas a quantidade de acesso que os utilizadores precisam realizar seus trabalhos. Em vez de dar todas as permissões de utilizador sem restrições para recursos do Azure, os administradores podem permitir apenas certas ações para aceder aos dados pessoais. Acesso à subscrição está limitado ao administrador da subscrição.
- [AAD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) permite que os clientes minimizar o número de utilizadores que têm acesso a determinados recursos.  Os administradores podem utilizar o AAD Privileged Identity Management para detetar, restringir e monitorizar identidades privilegiadas e respetivo acesso a recursos. Esta funcionalidade também pode ser utilizada para impor acesso administrativo a pedido e just-in-time quando necessário.
- [Proteção de identidade do AAD](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) Deteta potenciais vulnerabilidades que afetam as identidades da organização, configura as respostas automáticas para detetado ações suspeitas relacionadas com identidades de uma organização e investiga suspeita incidentes para tomar as medidas adequadas para resolvê-los.
- Uma instância do Active Directory de IaaS fornece gestão de identidade ao nível do SO para máquinas de virtuais de IaaS implementadas.

### <a name="security"></a>Segurança
**Gestão de segredos**: A solução utiliza [do Azure Key Vault](https://azure.microsoft.com/services/key-vault/) para o gerenciamento de chaves e segredos. O Cofre de Chaves do Azure ajuda a salvaguardar as chaves criptográficas e os segredos utilizados pelas aplicações em cloud e pelos serviços. As seguintes capacidades do Azure Key Vault ajudam os clientes a proteger os dados pessoais e de acesso a esses dados:
- Políticas de acesso avançadas são configuradas de forma necessidade.
- Políticas de acesso do Cofre de chaves são definidas com o mínimo permissões necessárias para chaves e segredos.
- Todas as chaves e segredos no Key Vault tem datas de expiração.
- Todas as chaves no Cofre de chaves estão protegidas por módulos de hardware especializado de segurança (HSMs). O tipo de chave é uma chave RSA de 2048 protegidas de HSM bits.
- Todos os utilizadores e identidades são concedidas as permissões mínimas necessárias através do RBAC.
- Registos de diagnóstico para o Cofre de chaves estão ativados com um período de retenção de, pelo menos, 365 dias.
- Operações de criptografia permitidas para as chaves estão limitadas aos obrigatórios.
- A solução está integrada no Azure Key Vault para gerir chaves de encriptação de disco de máquina virtual IaaS e segredos.

**Gerenciamento de patches**: Máquinas de virtuais do Windows implementadas como parte desta arquitetura de referência estão configuradas por predefinição para receber atualizações automáticas do serviço de atualização do Windows. Esta solução também inclui a [automatização do Azure](https://docs.microsoft.com/azure/automation/automation-intro) serviço por meio do qual as implementações atualizadas podem ser criadas para máquinas de virtuais de patch quando necessário.

**Proteção contra Malware**: [O Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) para máquinas virtuais fornece a capacidade de proteção em tempo real que ajuda a identificar e remover vírus, spyware e outro software malicioso, com alertas configuráveis quando conhecido software malicioso ou indesejável tenta instalar ou executar nas máquinas virtuais protegidas.

**Alertas de segurança**: [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro) permite aos clientes monitorizar o tráfego, recolher registos e analisar as origens de dados relativamente a ameaças. Além disso, o Centro de segurança do Azure acede a configuração existente de serviços do Azure para fornecer recomendações de serviço para ajudar a melhorar a postura de segurança e proteger dados pessoais e de configuração. Centro de segurança do Azure inclui um [relatório de inteligência de ameaças](https://docs.microsoft.com/azure/security-center/security-center-threat-report) para cada detetado contra ameaças para ajudar as equipes de resposta a incidentes investigar e remediar ameaças.

**Gateway de aplicação**: A arquitetura reduz o risco de vulnerabilidades de segurança com um Gateway de aplicação com firewall de aplicações web (WAF) e o conjunto de regras do OWASP ativada. Capacidades adicionais incluem:

- [End-to-End-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Ativar [descarga de SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Desativar [TLS versões 1.0 e 1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Firewall de aplicações Web](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (modo de WAF)
- [Modo de prevenção](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) com o conjunto de regras do OWASP 3.0
- Ativar [registo de diagnósticos](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Sondas de estado de funcionamento personalizados](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Centro de segurança do Azure](https://azure.microsoft.com/services/security-center) e [Assistente do Azure](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) fornecer proteção adicional e notificações. Centro de segurança do Azure também fornece um sistema de reputação.

### <a name="business-continuity"></a>Continuidade do negócio

**Elevada disponibilidade**: A solução implementa todas as máquinas virtuais num [conjunto de disponibilidade](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Conjuntos de disponibilidade garantem que as máquinas virtuais são distribuídas por vários clusters de hardware isolados para melhorar a disponibilidade. Pelo menos uma máquina virtual está disponível durante um evento de manutenção não planeada ou não planeada, atendendo a 99,95% do SLA do Azure.

**Cofre de serviços de recuperação**: O [cofre dos Recovery Services](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) hospeda os dados de cópia de segurança e protege todas as configurações de máquinas virtuais do Azure nesta arquitetura. Com um cofre de serviços de recuperação, os clientes podem restaurar ficheiros e pastas a partir de uma VM de IaaS sem restaurar a VM inteira, permitindo que os tempos de restauro mais rápidos.

**Testemunho de nuvem**: [Testemunho de nuvem](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) é um tipo de testemunho de quórum do Cluster de ativação pós-falha no Windows Server 2016 que tira partido do Azure como o ponto de arbitragem. O testemunho de nuvem, como outro testemunho de quórum, obtém um voto e pode participar nos cálculos do quórum, mas utiliza o armazenamento de Blobs do Azure disponíveis publicamente padrão. Isso elimina a sobrecarga de manutenção extra de VMs alojadas numa nuvem pública.

### <a name="logging-and-auditing"></a>Registro e auditoria

O log Analytics proporciona extenso registo de atividade do sistema e do usuário, bem como o estado de funcionamento do sistema. O [do Log Analytics](https://azure.microsoft.com/services/log-analytics/) solução recolhe e analisa os dados gerados pelos recursos no Azure e ambientes no local.
- **Registos de atividades**: [Registos de atividades](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) fornecem informações sobre as operações executadas em recursos numa subscrição. Registos de atividades podem ajudar a determinar o iniciador de uma operação, hora da ocorrência e o estado.
- **Os registos de diagnóstico**: [Os registos de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) incluem todos os registos emitidos por cada recurso. Estes registos incluem registos de sistema de eventos do Windows, registos de armazenamento do Azure, registos de auditoria do Cofre de chaves e os registos de acesso e de firewall do Gateway de aplicação.
- **Arquivo de log**: Todos os registos de diagnóstico escrevem para uma conta de armazenamento do Azure centralizado e criptografado para arquivamento. O período de retenção é configurável pelo utilizador, cópia de segurança e 730 dias, para atender aos requisitos de retenção de específicas da organização. Estes registos ligar ao Azure Log Analytics para processamento, armazenamento e relatórios do dashboard.

Além disso, as seguintes soluções de monitorização são incluídas como parte desta arquitetura:
-   [Avaliação do AD](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): A solução de verificação de estado de funcionamento do Active Directory avalia o risco e estado de funcionamento dos ambientes de servidor num intervalo regular e fornece uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
-   [Avaliação de antimalware](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): A solução Antimalware relatórios sobre o estado de malware, ameaças e a proteção.
-   [A automatização do Azure](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): A solução de automatização do Azure armazena, executa e gere runbooks.
-   [Segurança e auditoria](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): O dashboard de segurança e auditoria fornece uma alto nível visão do Estado de segurança de recursos, fornecendo as métricas em domínios de segurança, problemas relevantes, deteções, informações sobre ameaças e consultas de segurança comuns.
-   [Avaliação do SQL](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): A solução de verificação de estado de funcionamento do SQL avalia o risco e estado de funcionamento dos ambientes de servidor num intervalo regular e fornece aos clientes uma lista prioritária de recomendações específicas para a infraestrutura de servidor implementado.
-   [Gestão de atualizações](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): A solução de gestão de atualizações permite a gestão de cliente de atualizações de segurança do sistema operativo, incluindo o processo de instalação de atualizações necessárias e o estado das atualizações disponíveis.
-   [Estado de funcionamento do agente](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): A solução de estado de funcionamento do agente relata o número de agentes é implementado e a distribuição geográfica, como número de agentes que não respondem e o número de agentes que está a enviar dados operacionais.
-   [Registos de atividades do Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): A solução Log Analytics da atividade auxilia com uma análise de registos de atividade do Azure em todas as subscrições do Azure para um cliente.
-   [Controlo de alterações](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): A solução de controlo de alterações permite aos clientes identificar facilmente as alterações no ambiente.

## <a name="threat-model"></a>Modelo de risco

O diagrama de fluxo de dados (DFD) para esta arquitetura de referência está disponível para [transferir](https://aka.ms/gdprIaaSdfd) ou podem ser encontradas abaixo. Esse modelo pode ajudar os clientes a compreender os pontos do potencial risco de na infraestrutura de sistema ao fazer modificações.

![Aplicação Web de IaaS para o modelo de risco do GDPR](images/gdpr-iaaswa-threat-model.png?raw=true "aplicativo da Web de IaaS para o modelo de risco GDPR")

## <a name="compliance-documentation"></a>Documentação de conformidade

O [Azure no Security and Compliance Blueprint - matriz de responsabilidade do cliente com o GDPR](https://aka.ms/gdprCRM) apresenta uma lista de responsabilidades de controlador e o processador para todos os artigos GDPR. Tenha em atenção que, para serviços do Azure, um cliente é, normalmente, o controlador e Microsoft atua como o processador.

O [Azure no Security and Compliance Blueprint - matriz de implementação de aplicação de Web de IaaS de GDPR](https://aka.ms/gdprIaaSweb) fornece informações sobre qual GDPR artigos são resolvidos pela arquitetura de aplicação Web IaaS, incluindo descrições detalhadas de como a implementação cumpre os requisitos de cada artigo coberto.

## <a name="guidance-and-recommendations"></a>Orientações e recomendações
### <a name="vpn-and-expressroute"></a>ExpressRoute e VPN
Um túnel VPN seguro ou [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) tem de ser configurado com segurança estabelecer uma ligação para os recursos implementados como parte desta arquitetura de referência de aplicação Web IaaS. Configurando adequadamente uma VPN ou ExpressRoute, os clientes podem adicionar uma camada de proteção para dados em trânsito.

Ao implementar um túnel VPN seguro com o Azure, é possível criar uma ligação privada virtual entre uma rede no local e uma rede Virtual do Azure. Essa conexão ocorre através da Internet e permite que os clientes com segurança as informações de "encapsulamento" dentro de uma ligação encriptada entre a rede do cliente e o Azure. VPN site a Site é uma tecnologia madura e segura que tenha sido implementada por empresas de todos os tamanhos há décadas. O [modo de encapsulamento IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) é utilizada nesta opção como um mecanismo de criptografia.

Porque o tráfego de túnel VPN atravessa a Internet com uma VPN de site a site, a Microsoft oferece a opção de ligação de outra, ainda mais seguro. O Azure ExpressRoute é uma WAN dedicada ligação entre o Azure e uma localização no local ou um fornecedor de alojamento do Exchange. Como as ligações ExpressRoute não entram na Internet, estas ligações oferecem mais fiabilidade, velocidades mais rápidas, latências mais baixas e maior segurança do que as ligações normais pela Internet. Além disso, como se trata de uma conexão direta do fornecedor de telecomunicação do cliente, os dados não percorre a Internet e, portanto, não são expostos ao mesmo.

Melhores práticas para implementar uma rede híbrida segura que expande uma rede no local para o Azure são [disponível](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Exclusão de Responsabilidade

- Este documento é apenas para fins informativos. A MICROSOFT FAZ NÃO OFERECE QUAISQUER GARANTIAS, EXPRESSAS, IMPLÍCITAS OU ESTATUTÁRIAS QUANTO ÀS INFORMAÇÕES CONTIDAS NESTE DOCUMENTO. Este documento é fornecido "como-está." Informações e opiniões expressados neste documento, incluindo URLs e outras referências de Web site da Internet, podem ser alteradas sem aviso prévio. Os clientes a ler este documento assume o risco de usá-lo.  
- Este documento não fornece aos clientes quaisquer direitos de propriedade intelectual em qualquer produto da Microsoft ou soluções.  
- Os clientes podem copiar e utilizar este documento para efeitos de referência interna.  
- Algumas recomendações neste documento podem resultar em maior de dados, a rede ou a utilização de recursos de computação no Azure e podem aumentar os custos de licenciamento ou de subscrição do Azure num cliente.  
- Esta arquitetura deve ser usado como uma base para os clientes que se ajustar às suas necessidades específicas e não devem ser utilizada como-está num ambiente de produção.
- Este documento é desenvolvido como uma referência e não deve ser usado para definir todos os meios pelos quais um cliente pode atender aos requisitos de conformidade e normas. Os clientes procuram legal suporte da sua organização em implementações do cliente aprovadas.

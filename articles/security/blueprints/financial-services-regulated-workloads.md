---
title: "Automatização do Azure Blueprint - serviços financeiros para cargas de trabalho regulados"
description: "Serviços financeiros Blueprint para cargas de trabalho reguladas"
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 17794288-9074-44b5-acc8-1dacceb3f56c
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2017
ms.author: frasim
ms.openlocfilehash: 19e26c16866dada8dcff04a520ce4c208d67c365
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/20/2017
---
# <a name="azure-blueprint-automation-financial-services-blueprint-for-regulated-workloads"></a>Automatização do Azure Blueprint: Blueprint de serviços financeiros para cargas de trabalho reguladas

## <a name="overview"></a>Descrição geral

Blueprint de serviços financeiros para cargas de trabalho reguladas ajuda a implementar uma plataforma segura e compatível como uma aplicação de web de serviço (PaaS) concebida para processar os dados confidenciais na nuvem. O blueprint consiste em scripts automatizados e orientações que demonstramos uma arquitetura de referência simples e uma estrutura que ajuda a simplificar a adoção de soluções do Microsoft Azure. Este blueprint ilustra uma solução ponto a ponto para satisfazer as necessidades das organizações formas de reduzir o custo de implementação na nuvem e o fardo de pesquisa.

Este blueprint foi concebido para satisfazer os requisitos de rigorosos normas conformes definidas pelo American Institute de certificado público Accountants como - SOC 1, SOC 2, DSS 3.2 o council de cartão de pagamento da indústria normas de segurança de dados e FFIEC para o recolha, armazenamento e obtenção de dados financeiras confidenciais. -Demonstra o processamento adequado desses dados ao implementar uma solução que gere dados financeiros num ambiente seguro, em conformidade, de várias camado. A solução é implementada como um ponto-a-ponto solução PaaS baseado no Azure. 

O Blueprint destina-se para servir como base para os clientes criar e compreender os requisitos de gestão de proteger os dados na nuvem. A solução não deve ser utilizada numa implementação de produção como-é, mas para compreender, conceber e implementar serviços do Azure; foi concebido como uma linha de base para ajudar os clientes utilizam o Microsoft Azure de forma segura e compatível.

Um auditor autorizado tem de certificar qualquer solução de cliente de produção que se baseia neste blueprint. Soluções podem variar consoante as especificações de implementação e na geografia cada cliente.

Para uma descrição geral de como funciona esta solução, veja este [vídeo](https://aka.ms/fsiblueprintvideo) que explica e demonstra a sua implementação.

## <a name="solution-components"></a>Componentes da solução

A arquitetura é constituída por dos seguintes componentes e utiliza as capacidades de implementação da solução de conformidade de PCI DSS do Azure.

- **Diagrama da arquitetura**. O diagrama mostra a arquitetura de referência utilizada para a solução de Contoso Webstore.
- **Modelos de implementação**. Nesta implementação, [modelos Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview#template-deployment) são utilizados para implementar automaticamente os componentes da arquitetura no Microsoft Azure, especificando os parâmetros de configuração durante a configuração.
- **Scripts de implementação de automatizada**. Estes scripts ajudam a implementar a solução ponto-a-ponto. Os scripts consistem em:
    - Uma instalação do módulo e [administrador global](/azure/active-directory/active-directory-assign-admin-roles-azure-portal) script de configuração é utilizada para instalar e certifique-se de que funções de administrador global e de módulos do PowerShell necessários estão configuradas corretamente. 
    - Uma instalação de script do PowerShell é utilizada para implementar a solução ponto-a-ponto, fornecida através de um ficheiro. zip e um ficheiro de bacpac que contêm uma aplicação web de demonstração pré-criadas com [exemplo de base de dados do SQL Server](https://github.com/Microsoft/azure-sql-security-sample). conteúdo. O código de origem para esta solução está disponível para revisão [repositório de código Blueprint de processamento do pagamento][code-repo]. 

## <a name="architectural-diagram"></a>Diagrama da arquitetura

![](images/pci-architectural-diagram.png)

## <a name="user-scenario"></a>Cenário de utilizador

O blueprint aborda o seguinte o caso de utilização abaixo.

> Este cenário ilustra como um webstore fictício mover dados confidenciais para uma PaaS na nuvem baseado no Azure solução. A solução de exemplo ilustra o processamento e recolha de informações de utilizador básico e os dados confidenciais selecionados. Este trabalho borrows da automatização do Azure Blueprint: pagamento de processamento para ambientes em conformidade de PCI DSS para processamento de cartão de pagamento. Para obter mais informações sobre este trabalho de expansão ["Rever e orientações para a implementação"](https://aka.ms/pciblueprintprocessingoverview) documento fornece uma revisão de ambientes de conformidade de PCI DSS.

### <a name="use-case"></a>Caso de utilização
Um pequeno webstore chamado *Contoso Webstore* está pronto para mover dados financeiros que inclua as informações de pagamento de cliente para a nuvem. 

O administrador da Contoso Webstore está à procura de uma solução que pode ser rapidamente implementada para atingir os objetivos. Ele utilizará esta prova-do-conceito (POC) para discutir com os intervenientes, como o Azure pode ser utilizado para recolher, armazenar e obter dados financeiros de acordo com os requisitos de conformidade rigorosos.

> Será responsável por realizar a segurança adequada e revisões de conformidade de qualquer solução criadas com a arquitetura utilizada por esta POC, como os requisitos podem variar com base nas especificações da sua implementação e de geografia. 

### <a name="elements-of-the-foundational-architecture"></a>Elementos da arquitetura do básico

A arquitetura dos foi concebida com os seguintes elementos fictícios:

Site de domínio`contosowebstore.com`

Funções de utilizador são utilizadas para ilustrar o caso de utilização e obter informações sobre a interface de utilizador.

#### <a name="role-site-and-subscription-admin"></a>Função: Administração de sites e de subscrição

|Item      |Exemplo|
|----------|------|
|NomeDeUtilizador: |`adminXX@contosowebstore.com`|
| Nome: |`Global Admin Azure PCI Samples`|
|Tipo de utilizador:| `Subscription Administrator and Azure Active Directory Global Administrator`|

- A conta de administrador não é possível ler as informações financeiras desmacaradas. Todas as ações são registadas.
- A conta de administrador não é possível gerir ou iniciar sessão na base de dados SQL.
- A conta de administrador pode gerir subscrições e do Active Directory.

#### <a name="role-sql-administrator"></a>Função: Administrador do SQL Server

|Item      |Exemplo|
|----------|------|
|NomeDeUtilizador: |`sqlAdmin@contosowebstore.com`|
| Nome: |`SQLADAdministrator PCI Samples`|
| Nome próprio: |`SQL AD Administrator`|
|Apelido: |`PCI Samples`|
|Tipo de utilizador:| `Administrator`|

- A conta de sqladmin não é possível ver as informações financeiras e não filtradas. Todas as ações são registadas.
- A conta de sqladmin pode gerir a base de dados SQL.

#### <a name="role-clerk"></a>Função: Clerk

|Item      |Exemplo|
|----------|------|
|NomeDeUtilizador:| `receptionist_EdnaB@contosowebstore.com`|
| Nome: |`Edna Benson`|
| Nome próprio:| `Edna`|
|Apelido:| `Benson`|
| Tipo de utilizador: |`Member`|

Edna Benson é o Gestor de rececionista e empresariais. Ela é responsável por assegurar que as informações de cliente são precisos e faturação está concluída. Edna é o utilizador iniciado sessão para todas as interações com o site de demonstração Contoso Webstore. Edna tem os seguintes direitos de: 

- Edna pode criar e ler as informações de cliente.
- Edna pode modificar informações de cliente.
- Edna podem substituir as informações financeiras.
- Conta de Edna não é possível ver as informações financeiras e não filtradas.

> Contoso Webstore, o utilizador é automaticamente o **Edna** utilizador para testar as capacidades do ambiente implementado.

### <a name="contoso-webstore---estimated-pricing"></a>Contoso Webstore - estimada de preços

Esta arquitetura básico e a aplicação web de exemplo tem uma estrutura de taxa mensal e um custo de utilização por hora que tem de ser considerado ao dimensionar a solução. Estes custos podem ser estimados utilizando o [Calculadora de custos do Azure](https://azure.microsoft.com/pricing/calculator/). A partir de Setembro de 2017, o custo estimado mensalmente para esta solução é ~ $2500 Isto inclui um custo de utilização de $1000/mo para ASE v2. Estes custos irão variar consoante a quantidade de utilização e estão sujeitos a alterações. É incumbent no cliente para calcular os custos estimados mensais no momento da implementação de uma estimativa mais exata. 

Esta solução utilizados os seguintes serviços do Azure. Detalhes da arquitetura de implementação estão localizados no [arquitetura de implementação](#deployment-architecture) secção.

>- Gateway de Aplicação
>- Azure Active Directory
>- V2 de ambiente de serviço de aplicações
>- Análise de registos do OMS
>- Azure Key Vault
>- Grupos de Segurança de Rede
>- BD SQL do Azure
>- Azure Load Balancer
>- Application Insights
>- Centro de Segurança do Azure
>- Aplicação Web do Azure
>- Automatização do Azure
>- Runbooks de automatização do Azure
>- DNS do Azure
>- Rede Virtual do Azure
>- Máquina Virtual do Azure
>- Grupo de recursos do Azure e políticas
>- Armazenamento de Blobs do Azure
>- Controlo de acesso baseado em funções do Azure Active Directory (RBAC)

## <a name="deployment-architecture"></a>Arquitetura de implementação

A secção seguinte descreve em detalhe os elementos de desenvolvimento e implementação.

### <a name="network-segmentation-and-security"></a>Segmentação de rede e segurança

![](images/pci-tiers-diagram.png)

#### <a name="application-gateway"></a>Gateway de Aplicação

A arquitetura dos reduz o risco de vulnerabilidades de segurança através de um Gateway de aplicação com uma firewall de aplicação web (WAF) e o ruleset OWASP ativada. Capacidades adicionais incluem:

- [Fim para final SSL](/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Descarga de SSL](/azure/application-gateway/application-gateway-ssl-portal) ativada
- [TLS v 1.0 e v 1.1](/azure/application-gateway/application-gateway-end-to-end-ssl-powershell) desativada
- [Firewall de aplicações Web](/azure/application-gateway/application-gateway-webapplicationfirewall-overview) (modo WAF)
- [Modo de prevenção](/azure/application-gateway/application-gateway-web-application-firewall-portal) com ruleset OWASP 3.0
- [Registo de diagnóstico](/azure/application-gateway/application-gateway-diagnostics) ativada
- [Sondas de estado de funcionamento personalizado](/azure/application-gateway/application-gateway-create-gateway-portal)
- [Centro de segurança do Azure](https://azure.microsoft.com/services/security-center) e [Azure Advisor](/azure/advisor/advisor-security-recommendations), que proporcionam proteção adicional e notificações. Centro de segurança do Azure também fornece um sistema de reputação de URLs.

#### <a name="virtual-network"></a>Rede virtual

A arquitetura dos define uma rede privada virtual com um espaço de endereços de 10.0.0.0/16.

#### <a name="network-security-groups"></a>Grupos de segurança de rede

Cada uma das camadas de rede tem um grupo de segurança de rede dedicado (NSG):

- Um grupo de segurança de rede DMZ para Gateway de aplicação WAF e firewall
- Um NSG para gestão jumpbox (anfitrião de bastion)
- Um NSG para o ambiente de serviço de aplicações

Cada um dos NSGs ter específicas portas e protocolos abertos para a operação segura e correta da solução. Para obter mais informações, consulte [orientações de PCI - grupos de segurança de rede](#network-security-groups).

Além disso, as configurações seguintes estão ativadas para cada NSG:

- Ativado [eventos e registos de diagnóstico](/azure/virtual-network/virtual-network-nsg-manage-log) são armazenados na conta de armazenamento 
- Ligado a análise de registos do OMS para o [diagnóstico do NSG](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

 
#### <a name="subnets"></a>Sub-redes
 Certifique-se de que cada sub-rede está associada a respetiva NSG correspondente.

#### <a name="custom-domain-ssl-certificates"></a>Certificados SSL de domínio personalizado
 Tráfego HTTPS está ativado para utilizar um certificado SSL de domínio personalizado.

### <a name="data-at-rest"></a>Dados Inativos

A arquitetura protege os dados Inativos através da utilização de encriptação, a auditoria de base de dados e de outras medidas.

#### <a name="azure-storage"></a>Storage do Azure

Para satisfazer os requisitos de dados em rest encriptados, todos os [Storage do Azure](https://azure.microsoft.com/services/storage/) utiliza [encriptação do serviço de armazenamento](/azure/storage/storage-service-encryption).

#### <a name="azure-sql-database"></a>Base de Dados SQL do Azure

A instância de SQL Database do Azure utiliza as seguintes medidas de segurança da base de dados:

- [Autorização e autenticação do AD](/azure/sql-database/sql-database-aad-authentication)
- [Auditoria de base de dados SQL](/azure/sql-database/sql-database-auditing-get-started)
- [Encriptação transparente de dados](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)
- [Regras de firewall](/azure/sql-database/sql-database-firewall-configure), que permite para a gestão de IP de cliente e os conjuntos de trabalho ASE
- [Deteção de ameaças do SQL Server](/azure/sql-database/sql-database-threat-detection-get-started)
- [Sempre encriptadas colunas](/azure/sql-database/sql-database-always-encrypted-azure-key-vault)
- [Máscara de dados dinâmicos da base de dados SQL](/azure/sql-database/sql-database-dynamic-data-masking-get-started), utilizando o script do PowerShell de pós-implementação

### <a name="logging-and-auditing"></a>Registo e auditoria

[Operations Management Suite (OMS)](/azure/operations-management-suite/) pode fornecer o Contoso Webstore com o registo de um vasto conjunto de toda a atividade de utilizador e de sistema, incluem registo de dados financeiros. As alterações podem ser revistas e podem ser verificadas em termos de exatidão. 

- **Registos da atividade.**  [Registos de atividade](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) fornecem informações aprofundadas as operações que foram executadas no recursos na sua subscrição.
- **Registos de diagnóstico.**  [Os registos de diagnóstico](/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) são emitidos por cada recurso de todos os registos. Estes registos incluem registos de sistema de eventos do Windows, os registos de armazenamento de Blobs do Azure, tabelas e os registos de fila.
- **Registos da firewall.**  O Gateway de aplicação fornece completa diagnóstico e aceder a registos. Estão disponíveis recursos de Gateway de aplicação que tenham WAF ativada de registos de firewall.
- **Arquivo de registo.**  Todos os registos de diagnóstico são configurados para escrever uma conta de armazenamento do Azure centralizada e encriptada para arquivo com um período de retenção definido (2 dias). Os registos, em seguida, estão ligados ao Log Analytics do Azure para o processamento, armazenamento e dashboarding. [Análise de registo](https://azure.microsoft.com/services/log-analytics) é um serviço do OMS que ajuda a recolher e analisar dados gerados pelos recursos na sua nuvem e no local ambientes.

### <a name="encryption-and-secrets-management"></a>Gestão de encriptação e segredos

A Contoso Webstore encripta todos os dados confidenciais e utiliza o Cofre de chaves do Azure para gerir as chaves e de impedir a obtenção de CHD.

- [O Cofre de chaves do Azure](https://azure.microsoft.com/services/key-vault/) ajuda a salvaguardar as chaves criptográficas e segredos utilizados pelas aplicações em nuvem e de serviços. 
- [SQL TDE](/sql/relational-databases/security/encryption/transparent-data-encryption) é utilizado para encriptar todos os dados financeiros de cliente.
- Os dados são armazenados no disco utilizando [Azure Disk Encryption](/azure/security/azure-security-disk-encryption) e o BitLocker.

### <a name="identity-management"></a>Gestão de identidades

As seguintes tecnologias de fornecem a identidade de capacidades de gestão no ambiente do Azure.

- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) é o Microsoft multi-inquilino baseado na nuvem diretório e identidade do serviço de gestão. Todos os utilizadores para a solução foram criados no Azure Active Directory, incluindo os utilizadores que acedem a base de dados do SQL Server.
- Autenticação para a aplicação é executada a utilizar o Azure AD. Para obter mais informações, consulte [integrar aplicações com o Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications). Além disso, a encriptação de coluna de base de dados também utiliza o Azure AD para autenticar a aplicação a SQL Database do Azure. Para obter mais informações, consulte [sempre encriptados: proteger os dados sensíveis na base de dados SQL](/azure/sql-database/sql-database-always-encrypted-azure-key-vault). 
- [Azure Active Directory Identity Protection](/azure/active-directory/active-directory-identityprotection) Deteta potenciais vulnerabilidades que podem afetar as identidades da organização, configura as respostas automatizadas detetadas ações suspeitas relacionada com as identidades da organização, e investiga incidentes suspeitas e executa uma ação adequada resolvê-los.
- [Azure baseada em funções controlo de acesso (RBAC)](/azure/active-directory/role-based-access-control-configure) permite precisamente direcionadas para gestão de acesso ao Azure. Acesso de subscrição está limitado ao administrador da subscrição e o acesso do Cofre de chaves do Azure é restringido a todos os utilizadores.

Para saber mais sobre como utilizar as funcionalidades de segurança da base de dados do Azure SQL, consulte o [aplicação de demonstração do Contoso Clinic](https://github.com/Microsoft/azure-sql-security-sample) exemplo.
   
### <a name="web-and-compute-resources"></a>Recursos de computação e de Web

#### <a name="app-service-environment"></a>Ambiente do Serviço de Aplicações

[App Service do Azure](/azure/app-service/) é um serviço gerido para a implementação de aplicações web. A aplicação Contoso Webstore é implementada como um [aplicação Web do App Service](/azure/app-service-web/app-service-web-overview).

[Ambiente de serviço de aplicações do Azure (ASE v2)](/azure/app-service/app-service-environment/intro) é uma funcionalidade do serviço de aplicações que fornece um ambiente completamente isolado e dedicado para execução segura de aplicações do App Service numa escala elevada. é um plano de serviço Premium utilizado por esta arquitetura para ativar a conformidade de PCI DSS.

ASEs isoladas-se de que apenas as aplicações de um único cliente em execução e sempre são implementadas numa rede virtual. Os clientes não têm controlo detalhado sobre o tráfego de rede de aplicação de entrada e saída e as aplicações podem estabelecer ligações seguras de alta velocidade através de redes virtuais aos recursos da empresa no local.

Utilização de ASEs para esta arquitetura permitida para controlos/configurações seguintes:

- Alojar dentro de uma rede virtual protegida e regras de segurança de rede
- ASE configurado com o certificado autoassinado do ILB para comunicação HTTPS
- [Modo de balanceamento de carga interno](/azure/app-service-web/app-service-environment-with-internal-load-balancer) (modo 3)
- [TLS 1.0](/azure/app-service-web/app-service-app-service-environment-custom-settings) desativada
- [Cifras TLS](/azure/app-service-web/app-service-app-service-environment-custom-settings) alterado
- Controlo [portas de N/W de tráfego de entrada](/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic) 
- [WAF - restringir dados](/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- [Tráfego de base de dados SQL](/azure/app-service-web/app-service-app-service-environment-network-architecture-overview) permitido


#### <a name="jumpbox-bastion-host"></a>Jumpbox (anfitrião de bastion)

Uma vez que o ambiente de serviço de aplicações está protegido e bloqueado, tem de haver um mecanismo para permitir qualquer DevOps versões ou as alterações que poderão ser necessárias, tais como a capacidade de monitorizar a aplicação web utilizando o Kudu. Uma máquina virtual está protegida por trás do Balanceador de carga NAT, que fornece a capacidade de ligar à VM numa porta diferente de TCP 3389. 

Uma máquina virtual foi criada como um jumpbox (anfitrião de bastion) com as seguintes configurações:

-   [Extensão de antimalware](/azure/security/azure-security-antimalware)
-   [Extensão do OMS](/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Extensão de diagnóstico do Azure](/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](/azure/security/azure-security-disk-encryption) com o Cofre de chaves do Azure 
-   Um [política de encerramento automático](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) reduzir o consumo de recursos de máquina virtual quando não está em utilização

### <a name="security-and-malware-protection"></a>Proteção de segurança e software maligno

[Centro de segurança do Azure](https://azure.microsoft.com/services/security-center/) fornece uma vista centralizada do Estado de segurança de todos os recursos do Azure. De forma rápida, pode verificar que os controlos de segurança adequados são cumpridos e corretamente configurado e que possa identificar rapidamente quaisquer recursos que necessitam da atenção.  

[Azure Advisor](/azure/advisor/advisor-overview) é um consultor de nuvem personalizado que o ajuda a seguir as melhores práticas para otimizar as implementações do Azure. Este analisa a telemetria de utilização e configuração do recurso e, em seguida, recomenda soluções que podem ajudar a melhorar a eficácia de custo, desempenho, elevada disponibilidade e segurança dos seus recursos Azure.

[Microsoft Antimalware](/azure/security/azure-security-antimalware) para máquinas virtuais e serviços em nuvem do Azure é a capacidade de proteção em tempo real, que ajuda a identificar e remover vírus, spyware e outro software malicioso, com alertas configuráveis ao conhecido malicioso ou indesejável tentativas de software para instalação ou execução em sistemas do Azure.

### <a name="operations-management"></a>Gestão de operações

#### <a name="application-insights"></a>Application Insights

Utilize [Application Insights](https://azure.microsoft.com/services/application-insights/) para obter conhecimentos acionáveis através da gestão de desempenho de aplicações e a análise instantânea.

#### <a name="log-analytics"></a>Log analytics

[Análise de registo](https://azure.microsoft.com/services/log-analytics/) é um serviço no Operations Management Suite (OMS) que o ajuda a recolher e analisar dados gerados pelos recursos na sua nuvem e no local ambientes.

#### <a name="oms-solutions"></a>Soluções do OMS

Estas soluções adicionais do OMS devem ser consideradas e configuradas: 
- [Log Analytics da Atividade](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)
- [Análise de Redes do Azure](/azure/log-analytics/log-analytics-azure-networking-analytics?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Análise SQL do Azure](/azure/log-analytics/log-analytics-azure-sql)
- [Monitorização de Alterações](/azure/log-analytics/log-analytics-change-tracking?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Análise do Cofre de Chaves](/azure/log-analytics/log-analytics-azure-key-vault?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Mapa do Serviço](/azure/operations-management-suite/operations-management-suite-service-map)
- [Auditoria e segurança](https://www.microsoft.com/cloud-platform/security-and-compliance)
- [Antimalware](/azure/log-analytics/log-analytics-malware?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Gestão de Atualizações](/azure/operations-management-suite/oms-solution-update-management)

### <a name="security-center-integration"></a>Integração do Centro de segurança

Destina-se a implementação predefinida para fornecer uma linha de base das recomendações do Centro de segurança que indicam um Estado de bom estado de funcionamento e segura de configuração. Pode ativar a recolha de dados a partir do Centro de segurança do Azure. Para obter mais informações, consulte [Centro de segurança do Azure - introdução](/azure/security-center/security-center-get-started).

## <a name="deploy-the-solution"></a>Implementar a solução

Os componentes para implementar esta solução estão disponíveis no [repositório de código Blueprint de processamento do pagamento][code-repo]. A implementação da arquitetura dos requer vários passos executados através do Microsoft PowerShell v5. Para ligar ao Web site, tem de fornecer um nome de domínio personalizado (tal como contoso.com). Isto for especificado utilizando o `-customHostName` comutador no passo 2. Para obter mais informações, consulte [comprar um nome de domínio personalizado para Web Apps do Azure](/azure/app-service-web/custom-dns-web-site-buydomains-web-app). Um nome de domínio personalizado não é necessário para implementar e executar a solução com êxito, mas não será possível ligar ao Web site para fins de demonstração.

Os scripts de adicionar utilizadores de domínio para o inquilino do Azure AD que especificar. A Microsoft recomenda a criação de um novo do Azure AD de inquilino para utilizar como um teste.

Se ocorrerem problemas durante a implementação, consulte [FAQ e resolução de problemas](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/pci-faq.md).

A Microsoft recomenda vivamente que uma instalação limpa do PowerShell utilizados para implementar a solução. Em alternativa, certifique-se de que está a utilizar os mais recentes módulos necessários para execução adequada dos scripts de instalação. Este exemplo inicia sessão a jumpbox (anfitrião de bastion) e executa os comandos seguintes. Tenha em atenção que isto permite que o comando de domínio personalizado.


1. Instalar módulos necessários e configurar corretamente as funções de administrador.
 
    ```powershell
     .\0-Setup-AdministrativeAccountAndPermission.ps1 
        -azureADDomainName contosowebstore.com
        -tenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
        -subscriptionId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
        -configureGlobalAdmin 
        -installModules
    ```
    Para obter instruções de utilização detalhada, consulte [Script instruções - conta administrativa do programa de configuração e permissão](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/0-Setup-AdministrativeAccountAndPermission.md).
    
2. Instale a gestão de atualizações de solução. 
 
    ```powershell
    .\1-DeployAndConfigureAzureResources.ps1 
        -resourceGroupName contosowebstore
        -globalAdminUserName adminXX@contosowebstore.com 
        -globalAdminPassword **************
        -azureADDomainName contosowebstore.com 
        -subscriptionID XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 
        -suffix PCIcontosowebstore
        -customHostName contosowebstore.com
        -sqlTDAlertEmailAddress edna@contosowebstore.com 
        -enableSSL
        -enableADDomainPasswordPolicy 
    ```
    
    Para obter instruções de utilização detalhada, consulte [instruções de Script – implementar e configurar recursos do Azure](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/1-DeployAndConfigureAzureResources.md).
    
3. OMS registo e monitorização. Quando a solução for implementada uma [Microsoft Operations Management Suite (OMS)](/azure/operations-management-suite/operations-management-suite-overview) pode ser aberta a área de trabalho e os modelos de exemplo fornecidos no repositório de solução podem ser utilizados para ilustrar a forma como pode ser configurado um dashboard de monitorização . Para os modelos do OMS de exemplo, consulte o [omsDashboards pasta](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/1-DeployAndConfigureAzureResources.md). Tenha em atenção que os dados devem ser recolhidos no OMS para modelos para implementar corretamente. Isto pode demorar até uma hora ou mais, dependendo da atividade do site.
 
    Quando configurar o registo do OMS, considere incluindo estes recursos:
 
    - Microsoft.Network/applicationGateways
    - Microsoft.Network/NetworkSecurityGroups
    - Microsoft.Web/serverFarms
    - Microsoft.Sql/servers/databases
    - Microsoft.Compute/virtualMachines
    - Microsoft.Web/sites
    - Microsoft.KeyVault/Vaults
    - Microsoft.Automation/automationAccounts
 

    
## <a name="threat-model"></a>Modelo de ameaça

Um diagrama de fluxo de dados (DFD) e o modelo de ameaça de exemplo para a Contoso Webstore [modelo de ameaça Blueprint de processamento de pagamento](https://aka.ms/pciblueprintthreatmodel).

![](images/pci-threat-model.png)



## <a name="customer-responsibility-matrix"></a>Matriz de responsabilidade do cliente

Os clientes são responsáveis por manter uma cópia do [matriz de resumo de responsabilidade](https://aka.ms/fsiblueprintcrm), que descreve os requisitos de FFIEC são da responsabilidade do cliente e os que são da responsabilidade do Microsoft Azure.



## <a name="disclaimer-and-acknowledgments"></a>Exclusão de responsabilidade e em que as confirmações

*Setembro de 2017*

- Este documento destina-se apenas a fins informativos. MICROSOFT E AVYAN NÃO TORNAR NÃO OFERECE NENHUMA GARANTIA, EXPRESSA, IMPLÍCITA OU ESTATUTARIAMENTE AS INFORMAÇÕES NESTE DOCUMENTO. Este documento é fornecido "como-está." As informações e opiniões expressados neste documento, incluindo URLs e outras referências de Web site da internet, podem ser alteradas sem aviso prévio. Os clientes ler este documento da sua responsabilidade utilizá-la.  
- Este documento não fornece aos clientes com quaisquer direitos legais a nenhuma propriedade intelectual em qualquer produto da Microsoft ou Avyan ou soluções.  
- Os clientes podem copiar e utilizar este documento para efeitos de referência interno.  

  > [!NOTE]
  > Algumas recomendações neste documento poderão resultar numa maior dados, a rede ou a utilização de recursos de computação no Azure e podem aumentar os custos de licenciamento ou de subscrição do Azure do cliente.  

- A solução este documento destina-se como uma arquitetura básico e não podem ser utilizada como-é para fins de produção. Atingirem a total conformidade requer que os clientes Consulte com os respetivos auditor para validar a solução de cliente final.  
- Todos os nomes de cliente, os registos de transações e quaisquer dados relacionados nesta página são fictícios, criado com esta arquitetura fundamentais sobre o objetivo e fornecido para fins de ilustração apenas. Nenhuma associação ou ligação destina-se e deve ser inferido nenhum.  
- Esta solução foi desenvolvida jointly pela Microsoft e consultadoria Avyan e está disponível na [licença MIT](https://opensource.org/licenses/MIT).

### <a name="document-authors"></a>Autores de documentos

* *O Frank Simorjay (Microsoft)*  

[code-repo]: https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms "Repositório de código"

---
title: Segurança operacional melhores práticas do Azure | Documentos da Microsoft
description: Este artigo fornece um conjunto de melhores práticas da segurança operacional do Azure.
services: security
documentationcenter: na
author: unifycloud
manager: mbaldwin
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: d8f6ad48c62ff2021c91e593cee44dd6f5551247
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297913"
---
# <a name="azure-operational-security-best-practices"></a>Segurança operacional melhores práticas do Azure
Segurança operacional do Azure refere-se a serviços, controles e recursos disponíveis aos utilizadores para proteger seus dados, aplicações e outros ativos no Microsoft Azure. Segurança operacional do Azure baseia-se uma estrutura que incorpore os dados recolhidos através de vários recursos que são exclusivos da Microsoft, incluindo o Microsoft Security Development Lifecycle (SDL), o programa Microsoft Security Response Center, e o conhecimento profundo do campo das ameaças de cibersegurança.

Neste artigo, discutimos a uma coleção de práticas recomendadas de segurança da base de dados do Azure. Essas práticas recomendadas são derivadas da nossa experiência com a segurança da base de dados do Azure e as experiências dos clientes, como mesmo.

Para cada melhor prática, Explicamos:
-   O que é a melhor prática
-   Por que pretende ativar essa prática recomendada
-   Se efetuar a ativação para ativar a melhor prática, o que pode ser o resultado
- Como pode aprender permitir a melhor prática

Este artigo Azure operacional melhores práticas de segurança baseia-se numa opinião de consenso e capacidades da plataforma Azure e conjuntos de recursos, tal como existem no momento que este artigo foi escrito. Opiniões e as tecnologias mudam ao longo do tempo e este artigo será atualizado regularmente para refletir essas alterações.

Segurança operacional melhores práticas do Azure discutidas neste artigo incluem:

-   Monitorizar, gerir e proteger a infraestrutura de nuvem
-   Gerir a identidade e na implementação de início de sessão único (SSO)
-   Rastrear pedidos, analisar tendências de utilização e diagnosticar problemas
-   Monitorizar os serviços com uma solução de monitorização centralizada
-   Prevenir, detetar e responder a ameaças
-   Monitorização de rede com base no cenário de ponto a ponto
-   Proteger a implementação com as ferramentas de DevOps comprovadas

## <a name="monitor-manage-and-protect-cloud-infrastructure"></a>Monitorizar, gerir e proteger a infraestrutura de nuvem
Operações de TI é responsável por gerenciar a infraestrutura de datacenters, aplicativos e dados, incluindo a estabilidade e a segurança desses sistemas. No entanto, a ganhar informações de segurança em toda a aumentar, muitas vezes, os ambientes complexos de TI requer que as organizações que improvisar dados de vários sistemas de gerenciamento e segurança.

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) é a solução da Microsoft com base na cloud IT management que ajuda a gerenciar e proteger no local e a infraestrutura na cloud.

[Solução de segurança do OMS e auditoria](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) permite a TI ativamente monitorizar todos os recursos, o que podem ajudar a minimizar o impacto de incidentes de segurança. Domínios de segurança que podem ser utilizados para monitorizar os recursos de ter segurança OMS e auditoria.

Para mais informações sobre o OMS, leia o artigo [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

Para o ajudar a prevenir, detetar e responder a ameaças, [solução de auditoria de segurança do Operations Management Suite (OMS) e](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) recolhe e processa dados sobre os recursos, que inclui:

-   Registo de eventos de segurança
-   Eventos de Rastreio de Eventos para o Windows (ETW)
-   Eventos de auditoria do AppLocker
-   Registo de Firewall do Windows
-   Eventos de Análise de Ameaças Avançada
-   Resultados da avaliação de linha de base
-   Resultados da avaliação de antimalware
-   Resultados da avaliação de atualização/correção
-   Fluxos de syslog explicitamente ativos no agente


## <a name="manage-identity-and-implement-single-sign-on"></a>Gerir a identidade e na implementação de início de sessão único
[Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) é o diretório de multi-inquilino com base na cloud da Microsoft e o serviço de gestão de identidade.

[O Azure AD](https://azure.microsoft.com/services/active-directory/) também inclui um conjunto completo de [gestão de identidades](https://docs.microsoft.com/azure/security/security-identity-management-overview) capacidades, incluindo [autenticação multifator](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication), registo de dispositivos, gestão de palavras-passe self-service, gestão de grupos self-service, a gestão de conta com privilégios, o controlo de acesso baseado em funções, a utilização da aplicação de monitorização, auditoria de avançadas e, monitorização e alertas de segurança.

As seguintes capacidades podem ajudar a proteger aplicativos baseados na nuvem, simplificar processos de TI, cortar custos e ajudar a garantir o cumprimento de objetivos de conformidade empresarial:

-   Gestão de identidades e acessos para a cloud
-   Simplificar o acesso dos utilizadores a qualquer aplicação na cloud
-   Proteger aplicações e dados confidenciais
-   Proporcionar autonomia aos empregados
-   Integrar com o Azure Active Directory

### <a name="identity-and-access-management-for-the-cloud"></a>Gestão de identidades e acessos para a cloud
Azure Active Directory (Azure AD) é uma abrangente [solução de cloud de gestão de identidades e acessos](https://www.microsoft.com/cloud-platform/identity-management), que lhe proporciona um conjunto robusto de capacidades para gerir utilizadores e grupos. Ele ajuda a proteger o acesso no local e na nuvem, incluindo serviços web da Microsoft, como o Office 365 e muito software não Microsoft como um aplicativo de serviço (SaaS).
Para aprender mais ativar a proteção de identidade no Azure AD, consulte [ativar o Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-enable).

### <a name="simplify-user-access-to-any-cloud-app"></a>Simplificar o acesso dos utilizadores a qualquer aplicação na cloud
[Ativar o início de sessão único](https://docs.microsoft.com/azure/active-directory/active-directory-sso-integrate-saas-apps) para simplificar o acesso dos utilizadores a milhares de aplicações na cloud a partir de dispositivos Windows, Mac, Android e iOS. Os utilizadores podem iniciar aplicações a partir de um painel de acesso baseado na web personalizadas ou aplicação móvel através das credenciais da empresa. Utilize o módulo de Proxy de aplicações do Azure AD para ir além das aplicações SaaS e publicar aplicativos da web no local para fornecer acesso remoto altamente seguro e início de sessão único.

### <a name="protect-sensitive-data-and-applications"></a>Proteger aplicações e dados confidenciais
Ativar [multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) para impedir acesso não autorizado para o local e aplicações na cloud ao fornecer um nível adicional de autenticação. Proteja o seu negócio e atenue potenciais ameaças com monitorização de segurança, alertas e relatórios baseados em aprendizagem automática que identificam padrões de acesso inconsistentes.

### <a name="enable-self-service-for-your-employees"></a>Proporcionar autonomia aos empregados
Delegue tarefas importantes aos seus funcionários, como a reposição de palavras-passe e a criação e gestão de grupos. [Ativar a alteração de palavra-passe self-service](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password)repor e self-service grupo de gestão com o Azure AD.

### <a name="integrate-with-azure-active-directory"></a>Integrar com o Azure Active Directory
Estender [do Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate) e qualquer outro local diretórios para o Azure AD para ativar o início de sessão único para todos os aplicativos baseados na nuvem. Os atributos do utilizador podem ser sincronizados automaticamente com o diretório na cloud a partir de variadíssimos tipos de diretórios no local.

Para saber mais sobre a integração do Azure Active Directory e como ativá-lo, leia o artigo [integrar seus diretórios no local com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

## <a name="trace-requests-analyze-usage-trends-and-diagnose-issues"></a>Rastrear pedidos, analisar tendências de utilização e diagnosticar problemas
[Análise de armazenamento do Azure](https://docs.microsoft.com/azure/storage/storage-analytics) efetua o registo e fornece dados de métricas para uma conta de armazenamento. Pode utilizar estes dados para rastrear pedidos, analisar tendências de utilização e diagnosticar problemas relacionados com a sua conta de armazenamento.

Métricas da análise de armazenamento estão ativadas por predefinição para novas contas de armazenamento. Pode ativar o registo e configurar métricas e registo no portal do Azure; Para obter detalhes, consulte [monitorizar uma conta de armazenamento no portal do Azure](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). Também pode ativar a análise de armazenamento através de programação com a API REST ou a biblioteca de cliente. Utilize a operação definir propriedades de serviço para ativar a análise de armazenamento individualmente para cada serviço.

Para obter um guia detalhado sobre como utilizar a análise de armazenamento e outras ferramentas para identificar, diagnosticar e resolver problemas relacionados com o armazenamento do Azure, consulte [monitorizar, diagnosticar e resolver problemas de armazenamento do Microsoft Azure](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting).

Para saber mais sobre a integração do Azure Active Directory e como ativá-lo, leia o artigo [ativar e configurar a análise de armazenamento](https://docs.microsoft.com/rest/api/storageservices/Enabling-and-Configuring-Storage-Analytics?redirectedfrom=MSDN).

## <a name="monitoring-services"></a>Serviços de monitorização
Aplicações na cloud são complexas com muitas partes móveis. A monitorização fornece dados para garantir que seu aplicativo mantém-se e em execução em bom estado. Também ajuda-o a protele potenciais problemas ou resolução de problemas anteriores são. Além disso, pode utilizar dados de monitorização para obter informações aprofundadas sobre a sua aplicação. Esse conhecimento pode ajudá-lo a melhorar o desempenho da aplicação ou a capacidade de manutenção, ou automatize ações que caso contrário, requer intervenção manual.

### <a name="monitor-azure-resources"></a>Monitorizar recursos do Azure
[O Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) é o serviço de plataforma que fornece uma única origem para monitorizar os recursos do Azure. Com o Azure Monitor, pode visualizar, consultar, encaminhar, arquivar e tomar medidas relativamente a métricas e registos provenientes de recursos no Azure. Pode trabalhar com estes dados utilizando o painel do portal do Monitor, [Cmdlets do PowerShell do Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-powershell-samples), a [CLI de Várias Plataformas](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-cli-samples)ou [APIs REST do Azure Monitor](https://msdn.microsoft.com/library/dn931943.aspx).

### <a name="enable-autoscale-with-azure-monitor"></a>Ativar o dimensionamento automático com o Azure monitor
Ativar [dimensionamento automático do Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-autoscale-get-started) aplica-se apenas a conjuntos de dimensionamento de máquinas virtuais (VMSS), serviços cloud, planos de serviço de aplicações e ambientes de serviço de aplicações.

### <a name="manage-roles-permissions-and-security"></a>Gerir funções de segurança e permissões
Muitas equipes precisam estritamente [regular o acesso à monitorização](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-roles-permissions-security) dados e definições. Por exemplo, se tiver os membros da Equipe que trabalham exclusivamente em monitorização (engenheiros de suporte, engenheiros de devops) ou se usar um provedor de serviço gerida, pode querer lhes conceder acesso a dados de monitorização apenas ao restringir a capacidade de criar, modificar, ou Elimine recursos.

Isso mostra como rapidamente aplicam-se de uma função de monitorização incorporada RBAC a um utilizador no Azure ou criar sua própria função personalizada para um utilizador que precisa de permissões limitadas de monitorização. Em seguida, ele aborda considerações de segurança dos seus recursos relacionados com o Azure Monitor e a forma como pode limitar o acesso aos dados que contêm.

## <a name="prevent-detect-and-respond-to-threats"></a>Prevenir, detetar e responder a ameaças
Deteção de ameaças do Centro de segurança funciona através da recolha automática de informações de segurança de recursos do Azure, rede e soluções de parceiros ligadas. O assistente analisa esta informação, muitas vezes correlacionando informações de várias origens, para identificar ameaças. Os alertas de segurança são prioritários no Centro de Segurança, juntamente com recomendações sobre como remediar a ameaça.

-   [Configurar uma política de segurança](https://docs.microsoft.com/azure/security-center/security-center-policies) para a sua subscrição do Azure.
-   Utilize o [recomendações no Centro de segurança](https://docs.microsoft.com/azure/security-center/security-center-recommendations) para o ajudar a proteger os seus recursos do Azure.
-   Reveja e gira os seus atuais [alertas de segurança](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).

O [Centro de Segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro) ajuda-o a evitar, detetar e responder a ameaças com uma maior visibilidade e controlo sobre a segurança dos seus recursos Azure. Fornece gestão de políticas e monitorização de segurança integrada nas suas subscrições do Azure, ajuda a detetar ameaças que caso contrário podem passar despercebidas e funciona com um ecossistema abrangente de soluções de segurança.

O Centro de Segurança proporciona uma prevenção e deteção de ameaças eficiente e de fácil utilização, bem como capacidades de resposta que estão incorporadas no Azure. As principais capacidades são:

-   Compreender o estado de segurança da cloud
-   Controle a segurança da cloud
-   Implemente facilmente soluções de segurança da cloud integradas
-   Detete ameaças e responda com rapidez

### <a name="understand-cloud-security-state"></a>Compreender o estado de segurança da cloud
Utilize o Centro de Segurança do Azure para obter uma visão central do estado de segurança de todos os seus recursos do Azure. Visão geral, certifique-se de que os controlos de segurança adequados estão implementados e corretamente configurado e identifique rapidamente os recursos que requerem atenção.

### <a name="take-control-of-cloud-security"></a>Controle a segurança da cloud
Definir [políticas de segurança](https://docs.microsoft.com/azure/security-center/security-center-policies) para as suas subscrições do Azure, de acordo com as necessidades de segurança de cloud da sua empresa, ajustadas para o tipo de aplicações ou sensibilidade dos dados em cada subscrição. Utilize recomendações orientadas por políticas para guiar os proprietários de recursos ao longo do processo de implementação dos controlos necessários: elimine da segurança da cloud as conjeturas.

### <a name="easily-deploy-integrated-cloud-security-solutions"></a>Implemente facilmente soluções de segurança da cloud integradas
[Ativar soluções de segurança](https://docs.microsoft.com/azure/security-center/security-center-partner-integration) da Microsoft e respetivos parceiros, incluindo firewalls e antimalware líderes da indústria. Utilize um aprovisionamento simplificado para implementar soluções de segurança. Até mesmo as alterações de rede são configuradas para si. Os seus eventos de segurança de soluções de parceiros são automaticamente recolhidos para análise e alerta.

### <a name="detect-threats-and-respond-fast"></a>Detete ameaças e responda com rapidez
Esteja um passo à frente das ameaças atuais e emergentes que surgem na cloud com uma abordagem integrada e voltada para a análise. Ao combinar o Microsoft global [informações sobre ameaças](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities) e conhecimentos, com informações sobre eventos relacionados com a segurança da cloud nas suas implementações do Azure, o Centro de segurança ajuda-o a detetar ameaças reais com antecedência e reduzir os falsos positivos. Alertas de segurança na cloud dão-lhe informações sobre a campanha de ataque, incluindo eventos relacionados e recursos afetados e sugerem formas de resolver os problemas e recuperar rapidamente.

## <a name="end-to-end-scenario-based-network-monitoring"></a>Monitorização de rede com base no cenário de ponto a ponto
Os clientes criar uma rede ponto a ponto no Azure através da orquestração e composição de vários recursos de rede individuais, tais como a VNet, ExpressRoute, o Gateway de aplicação, balanceadores de carga e muito mais. A monitorização está disponível em cada um dos recursos de rede.

[Observador de rede](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) é um serviço regional que lhe permite monitorizar e diagnosticar condições a um cenário de rede nível, para e do Azure. Diagnóstico de rede e ferramentas de visualização disponíveis com o observador de rede ajudam a compreender, diagnosticar e obter informações da sua rede no Azure.

### <a name="automate-remote-network-monitoring-with-packet-capture"></a>Automatize a monitorização remota de redes com a captura de pacotes
Monitorize e diagnostique problemas de rede sem ter de iniciar sessão nas suas máquinas virtuais (VMs) com o Observador de Rede. Acionador [captura de pacotes](https://docs.microsoft.com/azure/network-watcher/network-watcher-alert-triggered-packet-capture) ao definir alertas e obter acesso a informações de desempenho em tempo real ao nível do pacote. Quando se deparar com um problema, pode investigar em pormenor para obter melhores diagnósticos.

### <a name="gain-insight-into-your-network-traffic-using-flow-logs"></a>Obtenha informações sobre o seu tráfego de rede com os registos de fluxo
Criar uma compreensão mais aprofundada do seu tráfego de rede padrão com [registos de fluxo do grupo de segurança de rede](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview). Informações fornecidas pelos registos de fluxo ajudam-o a recolher dados de conformidade, auditoria e monitorização do seu perfil de segurança de rede.

### <a name="diagnose-vpn-connectivity-issues"></a>Diagnostique problemas da conectividade VPN
Observador de rede fornece a capacidade de [diagnosticar os problemas mais comuns de Gateway de VPN e ligações](https://docs.microsoft.com/azure/network-watcher/network-watcher-diagnose-on-premises-connectivity). Permitindo que não apenas para identificar o problema, mas também para utilizar os registos detalhados criados para ajudar a investigar.

Para saber mais sobre como configurar o observador de rede e como ativá-lo, leia o artigo [configurar o observador de rede](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="secure-deployment-using-proven-devops-tools"></a>Proteger a implementação com as ferramentas de DevOps comprovadas
Estes são alguns da lista de Azure DevOps práticas neste espaço de Cloud da Microsoft, o que torna as empresas e equipes, produtiva e eficiente.

-   **Infraestrutura como código (IaC):** infraestrutura como código é um conjunto de técnicas e práticas, que ajudam os profissionais de TI evita o sacrifício associadas com a compilação do dia a dia e a gestão da infraestrutura modular. Ele permite que os profissionais de TI criar e manter seu ambiente de servidor modernas de forma que é semelhante a como os desenvolvedores de software criar e manter o código da aplicação. Para o Azure, temos [do Azure Resource Manager]( https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) permite-lhe aprovisionar as suas aplicações através de um modelo declarativo. Num único modelo, pode implementar vários serviços, bem como as respetivas dependências. Pode utilizar o mesmo modelo para implementar repetidamente a sua aplicação durante cada fase do ciclo de vida da aplicação.
-   **Integração contínua e a implementação:** pode configurar seus projetos de DevOps do Azure para [automaticamente, criar e implementar](https://www.visualstudio.com/docs/build/overview) para aplicações web do Azure ou serviços cloud. DevOps do Azure implementa automaticamente os binários depois de fazer uma compilação para o Azure após cada check-in de código. O processo de compilação do pacote descrito aqui é equivalente ao comando do pacote no Visual Studio e os passos de publicação são equivalentes para o comando Publish no Visual Studio.
-   **Gestão de versões:** Visual Studio [Release Management](https://msdn.microsoft.com/library/vs/alm/release/overview) é uma ótima solução para automatizar a implementação de vários estágio e gerenciar o processo de liberação. Crie pipelines de implementação contínua gerido para libertar rapidez, facilidade e, muitas vezes. Com o Release Management, podemos automatizar muito nosso processo de liberação e pode ter predefinidas fluxos de trabalho de aprovação. Implemente no local e na nuvem, expandir e personalizar conforme necessário.
-   **Monitorização de desempenho da aplicação:** detetar problemas e resolver problemas e melhorar continuamente as suas aplicações. Diagnostique rapidamente quaisquer problemas na sua aplicação em direto. Compreenda o que os utilizadores fazem com isso. A configuração é uma questão fácil de adicionar código JS e uma entrada de webconfig e verá resultados dentro de minutos no portal com todos os detalhes. [Do app insights](https://azure.microsoft.com/documentation/articles/app-insights-start-monitoring-app-health-usage/) ajuda as empresas para a deteção de mais rápida de problemas de & remediação.
-   **Carregar teste & dimensionamento automático:** que podemos encontrar problemas de desempenho na nossa aplicação para melhorar a qualidade de implementação e tem de certificar-se de que a nossa aplicação é sempre se ou disponíveis para dar resposta aos negócios. Certifique-se de que a sua aplicação pode processar o tráfego para a sua campanha de lançamento ou de marketing seguinte. Comece a executar com base na cloud [testes de carga](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) sem maiores atrasos com DevOps do Azure.

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [do Azure de segurança operacional](https://docs.microsoft.com/azure/security/azure-operational-security).
- Para saber mais [Operations Management Suite | Segurança e conformidade](https://www.microsoft.com/cloud-platform/security-and-compliance).
- [Introdução ao Operations Management Suite segurança e solução de auditoria](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started).

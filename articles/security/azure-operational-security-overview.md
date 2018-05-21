---
title: Descrição geral de segurança operacionais do Azure | Microsoft Docs
description: Este artigo fornece uma descrição geral da segurança operacional do Azure.
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
ms.openlocfilehash: c0413678aad16105f732ef23fb60c61fddcdad45
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="azure-operational-security-overview"></a>Descrição geral de segurança operacionais do Azure
[Segurança operacional do Azure](https://docs.microsoft.com/azure/security/azure-operational-security) refere-se os serviços, controlos e funcionalidades disponíveis para os utilizadores para proteger os seus dados, aplicações e outros recursos no Microsoft Azure. É uma estrutura que incorpora o conhecimento adquirido através de uma variedade de capacidades que são exclusivos para a Microsoft. Estas capacidades incluem o Microsoft Security Development Lifecycle (SDL), o programa Microsoft Security Response Center e a deteção profunda de ameaças atuais.

## <a name="operations-management-suite"></a>Operations Management Suite
Uma equipa de operações de TI é responsável por gerir a infraestrutura de centro de dados, aplicações e dados, incluindo a estabilidade e segurança destes sistemas. No entanto, a obter conhecimentos de segurança aprofundados em aumentar complexas ambientes de TI, muitas vezes, requer que as organizações cobble em conjunto de dados a partir de vários sistemas de gestão e segurança.

[Microsoft Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) é uma baseado na nuvem IT solução de gestão que o ajuda a gerir e proteger no local e a infraestrutura de nuvem. A funcionalidade principal é fornecida pelos seguintes serviços que são executados no Azure. Cada serviço fornece uma função de gestão específico. Pode combinar os serviços para alcançar os cenários de gestão diferente. 

### <a name="log-analytics"></a>Log Analytics
[Análise de registos do Azure](http://azure.microsoft.com/documentation/services/log-analytics) fornece serviços de monitorização para o Operations Management Suite através da recolha de dados a partir dos recursos geridos para um repositório central. Estes dados podem incluir eventos, dados de desempenho ou dados personalizados fornecidos através da API. Depois dos dados são recolhidos, está disponível para alertas, análise e exportação. 

Pode consolidar os dados a partir de uma variedade de origens e combinar os dados dos seus serviços do Azure com o seu ambiente no local existente. Análise de registos separa também claramente a recolha dos dados da ação tomada nesses dados, para que todas as ações estão disponíveis para todos os tipos de dados.

### <a name="automation"></a>Automatização
[A automatização do Azure](https://docs.microsoft.com/azure/automation/automation-intro) fornece uma forma de automatizar as tarefas manuais, execução longa, propensas ao erro e frequentemente repetidas que normalmente são executadas num ambiente de nuvem e enterprise. Isto poupa tempo e aumenta a fiabilidade das tarefas administrativas. Mesmo as agendas estas tarefas para serem executadas automaticamente em intervalos regulares. Pode automatizar os processos utilizando runbooks ou automatizar a gestão de configuração utilizando a configuração de estado pretendido.

### <a name="backup"></a>Cópia de segurança
[Cópia de segurança do Azure](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) é o serviço baseado no Azure que pode utilizar para criar cópias de segurança (ou proteger) e restaurar os dados no Microsoft Cloud. Cópia de segurança do Azure substitui a solução de cópia de segurança fora das instalações ou no local existentes por uma solução baseada na nuvem que é fiável, segura e competitiva. 

Cópia de segurança do Azure oferece os componentes onde transferir e implementar no computador adequado ou no servidor ou na nuvem. O componente ou o agente que implementar depende do que pretende proteger. Todos os componentes do Backup do Azure (se estiver a proteger dados no local ou na nuvem) podem ser utilizados para cópias de segurança para um cofre dos serviços de recuperação do Azure no Azure. 

Para obter mais informações, consulte o [tabela de componentes do Backup do Azure](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup#which-azure-backup-components-should-i-use).

### <a name="site-recovery"></a>Site Recovery
[O Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) fornece continuidade do negócio através da orquestração da replicação de máquinas no local virtuais e físicas para o Azure ou para um site secundário. Se o site primário não estiver disponível, ativação pós-falha para a localização secundária para que os utilizadores podem manter a funcionar. Falhar ao sistemas regressar à ordem de trabalho. Centro de segurança do Azure utilizada para efetuar mais a deteção de ameaças inteligente e eficaz.

## <a name="azure-active-directory"></a>Azure Active Directory
[Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-enable-sso-scenario) é uma identidade abrangente de serviço que:

-   Ativa a gestão identidades e acessos (IAM) como um serviço em nuvem.
-   Fornece gestão de acesso central, -início de sessão único (SSO) e relatórios.
-   Suporta a gestão de acesso integrada para [milhares de aplicações](https://azure.microsoft.com/marketplace/active-directory/) no Azure Marketplace, incluindo o Salesforce, Google Apps, caixa e Concur.

Azure AD também inclui um conjunto completo de [capacidades de gestão de identidade](https://docs.microsoft.com/azure/security/security-identity-management-overview#security-monitoring-alerts-and-machine-learning-based-reports), incluindo estes:

- [Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Registo de dispositivo]( https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-overview)
- [Gestão de palavra-passe self-service](https://azure.microsoft.com/resources/videos/self-service-password-reset-azure-ad/)
- [Gestão de grupos self-service](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password)
- [Gestão de contas com privilégios](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure)
- [Controlo de acesso baseado em funções](https://docs.microsoft.com/azure/role-based-access-control/overview)
- [Monitorização da utilização de aplicações](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health)
- [Auditoria de avançada](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Monitorização de segurança e alertas](https://docs.microsoft.com/azure/operations-management-suite/oms-security-responding-alerts)

Azure Active Directory, todas as aplicações que publicar para os seus parceiros e clientes (ou empresariais consumidor) possui a mesma identidade e capacidades de gestão de acesso. Isto permite-lhe reduzir significativamente os custos operacionais.

## <a name="azure-security-center"></a>Centro de Segurança do Azure
[Centro de segurança do Azure](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro) ajuda-o a evitar, detetar e responder a ameaças com uma maior visibilidade (e controlo sobre) a segurança dos seus recursos Azure. Fornece gestão de políticas e monitorização de segurança integrada nas suas subscrições. Ajuda a detetar ameaças que caso contrário podem passar despercebidas e funciona com um ecossistema abrangente de soluções de segurança.

[Proteger dados da máquina virtual (VM)](https://docs.microsoft.com/azure/security-center/security-center-linux-virtual-machine) no Azure mediante a monitorização de ameaças e fornecendo visibilidade para as definições de segurança da sua máquina virtual. O Centro de Segurança pode monitorizar as máquinas virtuais relativamente a:

-   Definições de segurança do sistema operativo com as regras de configuração recomendada.
-   Atualizações críticas que estão em falta e de segurança do sistema.
-   Recomendações de proteção de ponto final.
-   Validação de encriptação de disco.
-   Ataques baseados na rede.

Centro de segurança utiliza [controlo de acesso baseado em funções (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal). RBAC fornece [funções incorporadas](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) que podem ser atribuídos a utilizadores, grupos e serviços no Azure.

Centro de segurança avalia a configuração dos seus recursos para identificar problemas de segurança e vulnerabilidades. No Centro de segurança, verá informações relacionadas com a um recurso apenas quando está atribuída a função de proprietário, Contribuidor ou leitor para a subscrição ou grupo de recursos que um recurso pertence.

>[!Note]
>Para saber mais sobre as funções e permitido ações no Centro de segurança, consulte [permissões no Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-permissions).

Centro de segurança utiliza o Microsoft Monitoring Agent. Este é o mesmo agente que utilizam o Operations Management Suite e o serviço de análise de registos. Dados recolhidos a partir deste agente estão armazenados em qualquer uma análise de registos existente [área de trabalho](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access) associado à sua subscrição do Azure ou uma área de trabalho nova, tendo em conta a geolocalização da VM.

## <a name="azure-monitor"></a>Azure Monitor
Problemas de desempenho na sua aplicação em nuvem podem afetar a sua empresa. Com os vários componentes interligados e versões frequentes, degradations pode acontecer em qualquer altura. E se estiver a desenvolver uma aplicação, os utilizadores, geralmente, detetar problemas que não foi encontrado no teste. Deve conhecer sobre estes problemas imediatamente e deve ter as ferramentas para diagnosticar e corrigir os problemas.

[Monitor do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) ferramenta básico para monitorizar os serviços em execução no Azure. Proporciona ao nível da infraestrutura dados sobre o débito de um serviço e o ambiente surrounding. Se estiver a gerir aplicações tudo no Azure e decidir se pretende aumentar ou reduzir verticalmente de recursos, o Monitor do Azure é o local para iniciar.

Também pode utilizar dados de monitorização para obter conhecimentos aprofundados sobre a sua aplicação. Conhecimentos podem ajudar a melhorar o desempenho da aplicação ou maintainability ou automatizar ações que caso contrário necessitem intervenção manual. 

Monitor do Azure inclui os seguintes componentes.

### <a name="azure-activity-log"></a>Registo de atividade do Azure
O [registo de atividade do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) fornece informações sobre as operações que foram executadas no recursos na sua subscrição. Era anteriormente conhecida como "Registo de auditoria" ou "Registo operacional", porque esta comunica plane de controlo de eventos para as suas subscrições.

### <a name="azure-diagnostic-logs"></a>Registos de diagnóstico do Azure
[Os registos de diagnóstico do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) são emitidos por um recurso e fornecer dados avançados, frequentes sobre o funcionamento desse recurso. O conteúdo estes registos varia consoante o tipo de recurso.

Registos de sistema de eventos do Windows são uma categoria de registos de diagnóstico para as VMs. BLOB, tabela e fila registos são categorias de registos de diagnóstico para contas de armazenamento.

Os registos de diagnóstico é diferente do [registo de atividade](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). O registo de atividade fornece informações sobre as operações que foram executadas no recursos na sua subscrição. Os registos de diagnóstico fornecem informações aprofundadas operations que o seu recurso efetuadas em si.

### <a name="metrics"></a>Métricas
Monitor do Azure fornece a telemetria que dá-lhe visibilidade para o desempenho e estado de funcionamento das suas cargas de trabalho no Azure. O tipo de dados de telemetria do Azure mais importante é o [métricas](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) (também chamado os contadores de desempenho) emitidos pelo Azure mais recursos. Monitor do Azure fornece várias formas de configurar e consumir estas métricas de monitorização e resolução de problemas.

### <a name="azure-diagnostics"></a>Diagnóstico do Azure
Diagnóstico do Azure permite a recolha de dados de diagnóstico sobre uma aplicação implementada. Pode utilizar a extensão de diagnóstico de várias origens. São atualmente suportados [funções do serviço em nuvem do Azure](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [máquinas virtuais do Azure](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) com o Microsoft Windows, e [Azure Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics).


## <a name="azure-network-watcher"></a>Observador de rede do Azure
Os clientes crie uma rede ponto a ponto no Azure através da orquestração e recursos de rede individuais, tais como redes virtuais, Azure ExpressRoute, o Gateway de aplicação do Azure, de composição e Balanceadores de carga. A monitorização está disponível em cada um dos recursos da rede.

Rede ponto a ponto pode ter configurações complexas e as interações entre os recursos. O resultado é cenários complexos que necessitam de baseada em cenários de monitorização através de [observador de rede de Azure](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview).

Observador de rede simplifica a monitorização e diagnóstico da rede do Azure. Pode utilizar as ferramentas de diagnóstico e visualização de observador de rede para:
- Tirar capturas de pacotes remoto numa máquina virtual do Azure.
- Ganhos aprofundadas o tráfego de rede ao utilizar registos de fluxo.
- Diagnosticar ligações e Gateway de VPN do Azure.

Observador de rede atualmente tem as seguintes capacidades:

- [Topologia](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview): fornece uma vista das várias interconnections e as associações entre recursos de rede num grupo de recursos.
-   [Captura de pacotes variável](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview): captura dados de pacote e para uma máquina virtual. Opções de filtragem e controlos otimizados de avançadas, tais como a capacidade de definir a hora e limitações, o tamanho fornecem versatility. Os dados de pacote podem ser armazenados num arquivo de blob ou no disco local no formato .cap.
-   [Certifique-se de fluxo IP](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview): verifica se a um pacote é permitido ou negado com base nos parâmetros de pacotes de 5 cadeias de identificação para obter informações de fluxo (IP de destino, IP de origem, porta de destino, porta de origem e protocolo). Se um grupo de segurança nega o pacote, a regra e o grupo negado pacote são devolvidas.
-   [Próximo salto](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview): determina o salto seguinte para pacotes a ser encaminhados nos recursos de infraestrutura de rede do Azure, pelo que pode diagnosticar qualquer configurada incorretamente rotas definidas pelo utilizador.
-   [Vista do grupo de segurança](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview): obtém as regras de segurança eficaz e aplicados que são aplicadas numa VM.
-   [Os registos de fluxo NSG para grupos de segurança de rede](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview): permitem-lhe capturar registos relacionados com o tráfego que é permitido ou negado pelas regras de segurança no grupo. O fluxo é definido pelas informações de 5 cadeias de identificação: IP, IP de destino, porta de origem, porta de destino e protocolo de origem.
-   [Gateway de rede virtual e a resolução de problemas de ligação](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest): fornece a capacidade de resolução de problemas de ligações e gateways de rede virtual.
-   [Limites de subscrição de rede](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): permite-lhe ver utilização de recursos de rede contra os limites.
-   [Os registos de diagnóstico](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): disponibiliza um painel único para ativar ou desativar os registos de diagnóstico para recursos de rede num grupo de recursos.

Para obter mais informações, consulte [configurar observador de rede](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="devops"></a>DevOps
Antes de [programador operações (DevOps)](https://www.visualstudio.com/learn/what-is-devops/) equipas de desenvolvimento de aplicações, foram responsável pela recolha necessidades comerciais para um programa de software e escrever código. Em seguida, uma equipa separada de pergunta e resposta testado o programa num ambiente de desenvolvimento isolado. Se os requisitos foram satisfeitos, a equipa de pergunta e resposta disponibilizadas o código para operações de implementação. As equipas de implementação mais foram fragmentadas em grupos, como redes e da base de dados. Sempre que um programa de software "é emitido através do padrão de fundo" para uma equipa de independente, adicionada congestionamentos.

DevOps permite equipas fornecer soluções mais seguras, superior qualidade mais rápida e mais cheaply. Os clientes esperam uma experiência fiável e dinâmica quando a consumir os serviços e software. Equipas rapidamente tem iterar sobre atualizações de software e o impacto das atualizações. Estes tem de responder rapidamente com novo desenvolvimento iterações para resolver problemas ou forneça um valor mais.  

Plataformas de cloud, como o Microsoft Azure tem removido congestionamentos tradicionais e ajudou commoditize infraestrutura. Software reigns em todas as empresas como o principal diferenciador e fator os resultados de negócios. Nenhuma organização, programador ou de trabalho IT pode ou deve evitar o movimento de DevOps.

Verão de DevOps madura adotar várias as seguintes práticas. Estas práticas [envolvem pessoas](https://www.visualstudio.com/learn/what-is-devops-culture/) para estratégias de formulário com base nos cenários de negócios. Ferramentas podem ajudá-lo a automatizar as várias práticas.

-   [Gestão de planeamento e projeto seja ágil](https://www.visualstudio.com/learn/what-is-agile/) técnicas são utilizadas para planear e isolar o trabalho em sprints, gerir a capacidade de equipa e ajudar equipas adaptar rapidamente às necessidades empresarias.
-   [Controlo de versão, normalmente, com o Git](https://www.visualstudio.com/learn/what-is-git/), permite que as equipas localizadas em qualquer local no mundo de partilhar a origem e integrar com ferramentas de desenvolvimento de software para automatizar o pipeline de versão.
-   [Integração contínua](https://www.visualstudio.com/learn/what-is-continuous-integration/) unidades a intercalação em curso e o teste de código, o que conduz a localizar defeitos antecipadamente.  Outras vantagens incluem menos tempo desperdiçado no fighting problemas de intercalação e rápido comentários para as equipas de desenvolvimento.
-   [Entrega contínua](https://www.visualstudio.com/learn/what-is-continuous-delivery/) das soluções de software para produção e teste de ambientes de ajuda as organizações rapidamente corrigir os erros e responder a alguma alteração requisitos de negócio.
-   [Monitorização](https://www.visualstudio.com/learn/what-is-monitoring/) das aplicações em execução – ambientes de estado de funcionamento de aplicações, bem como a utilização de cliente - ajuda as organizações formar um hipótese e rapidamente validar ou disprove estratégias incluindo produção.  Dados avançada são capturados e armazenados em vários formatos de registo.
-   [Infraestrutura como código (IaC)](https://www.visualstudio.com/learn/what-is-infrastructure-as-code/) é uma prática que permite a automatização e a validação da criação e teardown de redes e máquinas virtuais para ajudar a entrega de aplicações seguras e estável plataformas de alojamento.
-   [Micro-serviços](https://www.visualstudio.com/learn/what-are-microservices/) arquitetura é utilizada para isolar os casos de utilização empresarial para os serviços de reutilizáveis pequenos.  Esta arquitetura ativa escalabilidade e a eficiência.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre a solução de auditoria e segurança do Operations Management Suite, consulte os artigos seguintes:

- [Segurança e conformidade](https://www.microsoft.com/cloud-platform/security-and-compliance)
- [Monitorizar e responder a alertas de segurança na solução de auditoria e segurança do Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/oms-security-responding-alerts)
- [Monitorizar os recursos na solução de auditoria e segurança do Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources)

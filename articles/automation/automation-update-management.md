---
title: Solução de gestão de atualizações no Azure
description: Este artigo destina-se a ajudá-lo a saber como utilizar esta solução para gerir atualizações aos seus computadores Windows e Linux.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: e426f2b90e3ac3ac6bcb9825c7848c76e52a1021
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="update-management-solution-in-azure"></a>Solução de gestão de atualizações no Azure

A solução de gestão de atualizações na automatização do Azure permite-lhe gerir atualizações de segurança do sistema operativo para os computadores Windows e Linux implementados no Azure, ambientes no local ou outros fornecedores de nuvem. Pode rapidamente avaliar o estado das atualizações disponíveis em todos os computadores agente e gerir o processo de instalação de atualizações necessárias para os servidores.

Pode ativar a Gestão de atualizações de máquinas virtuais diretamente a partir da sua conta de [Automatização do Azure](automation-offering-get-started.md).
Para saber como ativar a gestão de atualizações das máquinas virtuais da sua Conta de automatização, veja [Manage updates for multiple virtual machines (Gerir atualizações de várias máquinas virtuais)](manage-update-multi.md).

## <a name="solution-overview"></a>Descrição geral da solução

Os computadores geridos através da utilização de gestão de atualização, as seguintes configurações para efetuar implementações de avaliação e de atualização:

* Microsoft Monitoring agent para Windows ou Linux
* Configuração de Estado Pretendido do PowerShell (DSC) para Linux
* Função de Trabalho de Runbook Híbrida da Automatização
* Microsoft Update ou Windows Server Update Services para computadores Windows

O diagrama seguinte mostra uma vista concetual do comportamento e ligados de fluxo de dados com a forma como a solução avalia e aplica as atualizações de segurança para todos os Windows Server e os computadores com Linux numa área de trabalho.    

![Atualizar o fluxo de processo de gestão](media/automation-update-management/update-mgmt-updateworkflow.png)

Depois de um computador efetua uma análise de compatibilidade de atualização, o agente reencaminha as informações em massa à análise de registos. Em computadores Windows, a análise de conformidade é realizada de 12 em 12 horas, por predefinição. Para além do agendamento da análise, a análise da compatibilidade da atualização é iniciada em 15 minutos, se o Microsoft Monitoring Agent (MMA) for reiniciado, antes da instalação da atualização e, após a instalação da atualização. Em computadores Linux, a análise de conformidade é realizada de três em três horas por predefinição e são iniciadas ao fim de 15 minutos após o agente MMA ser reiniciado.

A solução reporta até que ponto o computador está atualizado com base na origem com a qual está configurado para sincronizar. Se o computador Windows estiver configurado para reportar para o WSUS, dependendo da última vez que o WSUS se sincronizou com o Microsoft Update, os resultados poderão ser diferentes face àquilo que o Microsoft Update mostra. Este é o mesmo para computadores com Linux que estão configurados para o relatório para um repositório local versus um repositório público.

Pode criar uma implementação agendada para implementar e instalar atualizações de software em computadores que precisam das atualizações. As atualizações classificadas como *Opcionais* não estão incluídas no âmbito da implementação para computadores Windows, apenas as atualizações obrigatórias. A implementação planeada define os computadores de destino recebem as atualizações aplicáveis, explicitamente a especificação de computadores ou selecionar um [grupo de computadores](../log-analytics/log-analytics-computer-groups.md) que baseia-se remotas pesquisas de registo de um conjunto específico de computadores. Também pode especificar uma agenda para aprovar e designar um período de tempo durante o qual as atualizações estão autorizadas a ser instaladas. As atualizações são instaladas por runbooks na Automatização do Azure. Não pode ver estes runbooks, que não requerem nenhuma configuração. Quando é criada uma Implementação de Atualização, é criada uma agenda que inicia um runbook de atualização principal num momento especificado nos computadores incluídos. Este runbook principal inicia um runbook subordinado em cada agente que efetua a instalação das atualizações obrigatórias.

Na data e na hora especificadas na implementação de atualização, os computadores de destino executam a implementação em paralelo. Primeiro, é realizada uma análise para verificar se as atualizações ainda são obrigatórias, que são depois instaladas. Para computadores de cliente do WSUS, se as atualizações são aprovadas não no WSUS, a implementação da atualização falha.

## <a name="clients"></a>Clientes

### <a name="supported-client-types"></a>Tipos de clientes suportados

A tabela seguinte mostra uma lista dos sistemas operativos suportados: 

|Sistema Operativo  |Notas  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Só suporta as avaliações de atualização         |
|Windows Server 2008 R2 SP1 e posterior     |.NET framework 4.5 e o WMF 5.0 ou posterior, são necessários para o Windows Server 2008 R2 SP1        |
|CentOS 6 (x86/x64) e 7 (x64)      | Os agentes do Linux têm de ter acesso a um repositório de atualização.        |
|Red Hat Enterprise 6 (x86/x64) e 7 (x64)     | Os agentes do Linux têm de ter acesso a um repositório de atualização.        |
|SUSE Linux Enterprise Server 11 (x86/x64) e 12 (x64)     | Os agentes do Linux têm de ter acesso a um repositório de atualização.        |
|Ubuntu 12.04 LTS e mais recente x86/x64       |Os agentes do Linux têm de ter acesso a um repositório de atualização.         |

### <a name="unsupported-client-types"></a>Tipos de cliente não suportada

A tabela seguinte lista os sistemas operativos que não são suportados:

|Sistema Operativo  |Notas  |
|---------|---------|
|Cliente Windows     | Sistemas operativos de cliente (Windows 7, Windows 10, etc.) não são suportados.        |
|Windows Server 2016 Nano Server     | Não suportado       |

### <a name="client-requirements"></a>Requisitos do cliente

#### <a name="windows"></a>Windows

Agentes do Windows tem de ser configurados para comunicar com um servidor do Windows Server Update Services (WSUS) ou tem acesso ao Microsoft Update. Também o agente do Windows não pode ser gerido em simultâneo pelo System Center Configuration Manager. O [agente do Windows](../log-analytics/log-analytics-agent-windows.md) é necessária. Este agente é instalado automaticamente, se estiver a integração de VM do Azure.

#### <a name="linux"></a>Linux

Para Linux, a máquina tem de ter acesso a um repositório de atualização, que pode ser privado ou público. Um agente do OMS para Linux configurado para relatar para várias áreas de trabalho de análise de registos não é suportado com esta solução.

Para obter mais informações sobre como instalar o agente do OMS para Linux e transferir a versão mais recente, consulte [agente do Operations Management Suite para Linux](https://github.com/microsoft/oms-agent-for-linux). Para obter informações sobre como instalar o Agente do OMS para Windows, consultar [Operations Management Suite Agent for Windows](../log-analytics/log-analytics-windows-agent.md) (Agente do Operations Management Suite para Windows).  

## <a name="permissions"></a>Permissões
Para criar e gerir implementações de atualização, terá de permissões específicas. Para saber mais sobre estas permissões visitam [acesso funções com base - gestão de atualizações](automation-role-based-access-control.md#update-management) 

## <a name="solution-components"></a>Componentes da solução
Esta solução consiste nos recursos seguintes que são adicionados à sua conta de Automatização e a agentes ligados diretamente ou a grupos de gestão ligados do Operations Manager.

### <a name="hybrid-worker-groups"></a>Grupos de Função de Trabalho Híbrida
Depois de ativar esta solução, qualquer computador com o Windows diretamente ligada à sua área de trabalho de análise de registos é automaticamente configurado como um Runbook Worker híbrido para suportar os runbooks incluídos nesta solução. Para cada computador com o Windows gerido pela solução, estão listadas na página de grupos de trabalho híbrida como um grupo de trabalho híbrida de sistema para a conta de automatização seguir a Convenção de nomenclatura *Hostname FQDN_GUID*. Não pode visar estes grupos com runbooks na sua conta, caso contrário, que estas falhar. Estes grupos destinam-se apenas a suportar a solução de gestão.

No entanto, pode adicionar os computadores Windows a um grupo de Função de Trabalho de Runbook Híbrida na conta de Automatização para suportar runbooks de Automatização, desde que esteja a utilizar a mesma conta para a solução e a subscrição do grupo de Função de Trabalho de Runbook Híbrida. Esta funcionalidade foi adicionada à versão 7.2.12024.0 da Função de Trabalho de Runbook Híbrida.

### <a name="management-packs"></a>Pacotes de gestão

Se o grupo de gestão do System Center Operations Manager está ligado a uma área de trabalho de análise de registos, os seguintes pacotes de gestão estão instalados no Operations Manager. Estes pacotes de gestão também são instalados em computadores Windows ligados diretamente após adicionar esta solução. Estes pacotes de gestão não envolvem qualquer configuração ou gestão.

* Pacote de Informações de Avaliação de Atualização do Microsoft System Center Advisor (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Pacote de Gestão de Implementação de Atualização

Para obter mais informações sobre como são atualizados os pacotes de gestão da solução, veja [Connect Operations Manager to Log Analytics (Ligar o Operations Manager ao Log Analytics)](../log-analytics/log-analytics-om-agents.md).

### <a name="confirm-non-azure-machines-are-onboarded"></a>Confirme a máquinas do Azure não estão integradas

Para confirmar ligadas diretamente as máquinas estão a comunicar com a análise de registos, depois de alguns minutos pode executar a procura de registo seguinte:

#### <a name="linux"></a>Linux

```
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a>Windows

```
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table`
```

Num computador Windows, pode rever o seguinte procedimento para verificar a conectividade do agente de análise do registo:

1.  Abra o Microsoft Monitoring Agent no Painel de Controlo e, no separador **Azure Log Analytics (OMS)**, o agente apresenta uma mensagem que diz: **O Microsoft Monitoring Agent ligou-se com êxito ao serviço Microsoft Operations Management Suite**.   
2.  Abra o Registo de Eventos do Windows, navegue para **Registos de Aplicações e Serviços\Operations Manager** e procure os IDs de Evento 3000 e 5002 no Conector de Serviço de origem. Estes eventos indicarem o computador foi registado com a área de trabalho de análise de registos e está a receber configuração.  

Se o agente não consegue comunicar com a análise de registos e está configurado para comunicar com a internet através de um servidor de firewall ou proxy, certifique-se o servidor de firewall ou proxy está configurado corretamente revendo [configuração de rede para O agente do Windows](../log-analytics/log-analytics-agent-windows.md) ou [configuração de rede para o agente Linux](../log-analytics/log-analytics-agent-linux.md).

> [!NOTE]
> Se os sistemas Linux estão configurados para comunicar com um proxy ou Gateway do OMS e são integração esta solução, a atualização a *proxy.conf* permissões para o grupo de omiuser de conceder permissão de leitura nos ficheiro efetuando o seguinte comandos:  
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`  
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Os agentes do Linux adicionados recentemente mostrarão o estado **Atualizado** depois de ser realizada a avaliação. Este processo pode demorar até seis horas.

Para confirmar um grupo de gestão do Operations Manager está a comunicar com a análise de registos, consulte [validar a integração do Operations Manager com o OMS](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-oms).

## <a name="data-collection"></a>Recolha de dados

### <a name="supported-agents"></a>Agentes suportados
A tabela seguinte descreve as origens ligadas que são suportadas por esta solução.

| Origem Ligada | Suportadas | Descrição |
| --- | --- | --- |
| Agentes do Windows |Sim |A solução recolhe informações sobre atualizações do sistema de agentes do Windows e inicia a instalação das atualizações necessárias. |
| Agentes do Linux |Sim |A solução recolhe informações sobre atualizações de sistema a partir dos agentes do Linux e inicia a instalação das atualizações obrigatórias em distribuições suportadas. |
| Grupo de gestão do Operations Manager |Sim |A solução recolhe informações sobre atualizações do sistema de agentes num grupo de gestão ligado.<br>Não é necessária uma ligação direta a partir do agente do Operations Manager ao Log Analytics. Dados seja reencaminhados do grupo de gestão para a área de trabalho de análise de registos. |

### <a name="collection-frequency"></a>Frequência da recolha
Em cada computador Windows gerido, é feita uma análise duas vezes por dia. A cada 15 minutos, a API do Windows é chamada para consultar a hora da última atualização para determinar se o estado foi alterado e, se for esse o caso, é iniciada uma análise de conformidade. Em cada computador Linux gerido, é feita uma análise de três em três horas.

O dashboard pode demorar entre 30 minutos a seis horas a apresentar os dados atualizados a partir dos computadores geridos.   

## <a name="viewing-update-assessments"></a>Ver avaliações de atualizações
Clique em de **gestão de atualizações** na sua conta de automatização para ver o estado das máquinas.

Esta vista fornece informações sobre as máquinas, em falta atualizações, implementações de atualização e implementações de atualização agendada.

Pode executar uma pesquisa de registo que devolve informações do computador, a atualização ou a implementação, selecionando o item na lista. Esta ação abre o **pesquisa registo** página com uma consulta para o item selecionado.

## <a name="installing-updates"></a>Instalar as atualizações

Depois de terem sido avaliadas as atualizações para todos os computadores Linux e Windows na sua área de trabalho, pode instar as atualizações obrigatórias ao criar uma *Implementação de Atualização*. Uma implementação de atualização é uma instalação agendada de atualizações necessárias para um ou mais computadores. Especifique a data e a hora da implementação, bem como um computador ou grupo de computadores que devem ser incluídos no âmbito da mesma. Para saber mais sobre grupos de computadores, veja [Computer groups in Log Analytics](../log-analytics/log-analytics-computer-groups.md) (Grupos de computadores no Log Analytics). Ao incluir grupos de computadores na sua implementação de atualização, a associação ao grupo é avaliada apenas uma vez no momento da criação da agenda. As alterações subsequentes a um grupo não são refletidas. Para contornar este problema, elimine a implementação da atualização agendada e recrie-a.

> [!NOTE]
> Por predefinição, as VMs do Windows implementadas a partir do Azure Marketplace estão definidas para receber atualizações automáticas do Serviço Windows Update. Este comportamento não se altera depois de adicionar esta solução ou VMs do Windows à sua área de trabalho. Se não ativamente gerir atualizações com esta solução, o comportamento predefinido (automaticamente aplicar atualizações) aplica-se.

Para evitar que as atualizações sejam aplicadas fora da janela de manutenção no Ubuntu, reconfigure o pacote Unattended-Upgrade para desativar as atualizações automáticas. Para obter informações sobre como configurar, veja [Automatic Updates topic in the Ubuntu Server Guide](https://help.ubuntu.com/lts/serverguide/automatic-updates.html) (Tópico de Atualizações Automáticas no Guia do Ubuntu Server).

Para máquinas virtuais criadas a partir de imagens de Red Hat Enterprise Linux (RHEL) a pedido disponíveis no Azure Marketplace, estão registadas para aceder à [Infraestrutura de Atualização do Red Hat (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) implementada no Azure. Qualquer outra distribuição de Linux tem de ser atualizada a partir do repositório de ficheiros online distros, de acordo com os respetivos métodos suportados.  

## <a name="viewing-missing-updates"></a>Atualizações em falta de visualização

Clique em **atualizações em falta** para ver a lista de atualizações que estão em falta das suas máquinas. Cada atualização é apresentado na lista e apresenta informações relativamente ao número de máquinas que requerem a atualização, o sistema operativo e a ligação para obter mais informações. É possível selecionar cada atualização e o **pesquisa registo** página mostra e obter mais detalhes sobre as atualizações.

## <a name="viewing-update-deployments"></a>Ver as implementações de atualizações

Clique em **implementações de atualização** para ver a lista de implementações de atualização existente. Ao clicar em qualquer uma das implementações de atualização na lista abre-se a **atualizar implementação executada** página para essa implementação da atualização.

![Descrição Geral dos Resultados da Implementação de Atualizações](./media/automation-update-management/update-deployment-run.png)

## <a name="creating-an-update-deployment"></a>Criar uma Implementação de Atualizações

Criar uma nova implementação de atualização clicando a **implementação de atualização de agendamento** botão na parte superior do ecrã, para abrir o **nova implementação da atualização** página. Tem de fornecer valores para as propriedades na tabela seguinte:

| Propriedade | Descrição |
| --- | --- |
| Nome |O nome exclusivo para identificar a implementação de atualizações. |
|Sistema Operativo| Linux ou do Windows|
| Máquinas de atualização |Selecionar uma pesquisa guardada ou escolher máquina a partir da lista pendente e selecione máquinas individuais |
|Classificação de atualização|Selecione todas as classificações de atualização que precisa|
|Atualizações para excluir|Introduza todos os KBs para excluir sem o prefixo 'KB'|
|Definições de agendamento|Selecione a hora para iniciar e selecionar qualquer uma vez ou periodicamente para a periodicidade|
| Janela de manutenção |Número de minutos definido para atualizações. O valor pode não ser inferior a 30 minutos e não mais de 6 horas |

## <a name="search-logs"></a>Registos de pesquisa

Além dos detalhes que são fornecidos no portal, as pesquisas podem ser feitas contra os registos. Com o **alterações** clique aberto, da página **Log Analytics**, esta ação abre o **pesquisa de registo** página

### <a name="sample-queries"></a>Consultas de exemplo

A tabela seguinte fornece pesquisas de registo de exemplo para registos de atualização recolhidos por esta solução:

| Consulta | Descrição |
| --- | --- |
|Atualizar<br>&#124;onde UpdateState = = "Necessária" e opcionais = = false<br>&#124;Computador título, KBID, classificação, PublishedDate do projeto |Todos os computadores com atualizações em falta<br>Adicione um dos seguintes para limitar o sistema operativo:<br>OSType = "Windows"<br>OSType == "Linux" |
| Atualizar<br>&#124;onde UpdateState = = "Necessária" e opcionais = = false<br>&#124; where Computer == "ContosoVM1.contoso.com"<br>&#124;Computador título, KBID, produto, PublishedDate do projeto |Atualizações em falta num computador específico (substitua pelo nome do seu computador)|
| Evento<br>&#124;onde EventLevelName = = "error" e o computador no ((atualização &#124; onde (classificação = = "Atualizações de segurança" e classificação = = "Atualizações críticas")<br>&#124;onde UpdateState = = "Necessária" e opcionais = = false <br>&#124; distinct Computer)) |Eventos de erro de computadores que têm atualizações críticas ou de segurança necessárias em falta |
| Atualizar<br>&#124;onde UpdateState = = "Necessária" e opcionais = = false<br>&#124;Título distinto |Atualizações em falta distintas em todos os computadores | 
| UpdateRunProgress<br>&#124; where InstallationStatus == "failed" <br>&#124; summarize AggregatedValue = count() by Computer, Title, UpdateRunName |Computadores com atualizações que falharam na execução de atualização<br>Adicione um dos seguintes para limitar o sistema operativo:<br>OSType = "Windows"<br>OSType == "Linux" | 
| Atualizar<br>&#124; where OSType == "Linux"<br>&#124;onde UpdateState! = "Não necessárias" e (classificação = = "Atualizações críticas" ou classificação = = "Atualizações de segurança")<br>&#124; summarize AggregatedValue = count() by Computer |Lista de todos os computadores Linux, que tem disponível uma atualização do pacote, que aborda vulnerabilidade críticas ou de segurança | 
| UpdateRunProgress<br>&#124; where UpdateRunName == "DeploymentName"<br>&#124; summarize AggregatedValue = count() by Computer|Computadores que foram atualizados nesta execução de atualizações (substitua o valor pelo nome da sua Implementação de Atualizações) | 

## <a name="integrate-with-system-center-configuration-manager"></a>Integrar no System Center Configuration Manager

Os clientes que investiram no System Center Configuration Manager para gerir PCs, servidores e dispositivos móveis também contam com a sua potência e maturidade na gestão de atualizações de software como parte do respetivo ciclo de gestão de atualizações de software (SUM).

Para saber como integrar a solução de gestão de atualizações do OMS com o System Center Configuration Manager, consulte [integrar o System Center Configuration Manager com a gestão de atualização do OMS](oms-solution-updatemgmt-sccmintegration.md).

## <a name="patching-linux-machines"></a>Aplicação de patches máquinas Linux

As secções seguintes explicam potenciais problemas com a aplicação de patches do Linux.

### <a name="package-exclusion"></a>Exclusão de pacote

Em algumas variantes do Linux, tais como Red Hat Enterprise Linux, podem ocorrer atualizações ao nível do SO através de pacotes. Isto pode levar para execuções de atualização de gestão onde o número de versão do SO é alterado. Uma vez que a atualização de gestão utiliza os mesmos métodos para atualizar os pacotes de um administrador teria localmente no computador Linux, este comportamento é intencional.

Para evitar a atualizar a versão do SO através de execuções de gestão de atualizações, pode utilizar o **exclusão** funcionalidade.

Red Hat Enterprise Linux, seria possível o nome do pacote para excluir: server.x86_64 de versão de VM de redhat

![Pacotes para excluir para Linux](./media/automation-update-management/linuxpatches.png)

### <a name="security-patches-not-being-applied"></a>Não é aplicadas de patches de segurança

Ao implementar atualizações para uma máquina com Linux, pode selecionar classificações de atualização. Este procedimento filtra as atualizações que são aplicadas aos que cumprem os critérios especificados. Este filtro ser aplicado localmente no computador quando a atualização é implementada. Porque atualizar gestão efetua sem causa de atualização na nuvem, algumas atualizações podem sinalizadas na gestão de atualização como tendo impacto de segurança, apesar do computador local não ter essas informações. Como resultado, se aplicar atualizações críticas para uma máquina com Linux, poderá ser atualizações, que não estão marcadas como tendo o impacto de segurança em que a máquina e não ser aplicadas. No entanto, atualizar pode ainda relatório de gestão que a máquina como estando não conformidade porque tem informações adicionais sobre a atualização relevante.

Implementar atualizações de classificação de atualização não pode funcionar openSUSE Linux devido ao modelo de aplicação de patches diferentes utilizado.

## <a name="troubleshooting"></a>Resolução de problemas

Esta secção disponibiliza informações para ajudar a resolver problemas com a solução Gestão de Atualizações.

Se ocorrerem problemas ao tentar integrar a solução ou uma máquina virtual, consulte o registo de eventos **Registos de Aplicações e Serviços\Operations Manager** para eventos com o ID 4502 e mensagens de evento com **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**. A tabela seguinte realça as mensagens de erro específicas e uma resolução possível para cada uma.  

| Mensagem | Razão | Solução |   
|----------|----------|----------|  
| Não É Possível Registar a Máquina para Gestão de Patches,<br>O Registo Falhou com a Exceção<br>System.InvalidOperationException: {"Message":"a máquina já está<br>registada numa conta diferente. "} | A máquina já está integrada noutra área de trabalho para Gestão de Atualizações | Efetue uma limpeza dos artefactos antigos ao [eliminar o grupo de runbooks híbridos](automation-hybrid-runbook-worker.md#remove-hybrid-worker-groups)|  
| Não É Possível Registar a Máquina para Gestão de Patches,<br>O Registo Falhou com a Exceção<br>System.Net.Http.HttpRequestException: Ocorreu um erro ao enviar o pedido. ---><br>System.Net.WebException: A ligação subjacente<br>foi fechada: Ocorreu um erro inesperado<br>ao receber. ---> System.ComponentModel.Win32Exception:<br>O cliente e o servidor não conseguem comunicar,<br>porque não possuem um algoritmo comum | Proxy/Gateway/Firewall a bloquear a comunicação | [Rever os requisitos de rede](automation-offering-get-started.md#network-planning)|  
| Não É Possível Registar a Máquina para Gestão de Patches,<br>O Registo Falhou com a Exceção<br>Newtonsoft.Json.JsonReaderException: Erro ao analisar o valor infinito positivo. | Proxy/Gateway/Firewall a bloquear a comunicação | [Rever os requisitos de rede](automation-offering-get-started.md#network-planning)| 
| O certificado apresentado pelo <wsid>.oms.opinsights.azure.com do serviço<br>não foi emitido por uma autoridade de certificação<br>utilizada para os Serviços Microsoft. Contacto<br>o administrador da rede para ver se estão a executar um proxy que intercepte<br>a comunicação TLS/SSL. |Proxy/Gateway/Firewall a bloquear a comunicação | [Rever os requisitos de rede](automation-offering-get-started.md#network-planning)|  
| Não É Possível Registar a Máquina para Gestão de Patches,<br>O Registo Falhou com a Exceção<br>AgentService.HybridRegistration.<br>PowerShell.Certificates.CertificateCreationException:<br>Falha ao criar um certificado autoassinado. ---><br>System. unauthorizedaccessexception: O acesso é negado. | Falha de geração do certificado autoassinado | Verifique se a conta do sistema tem<br>acesso de leitura à pasta:<br>**C:\ProgramData\Microsoft\**<br>**Crypto\RSA**|  

## <a name="next-steps"></a>Passos Seguintes

Continue para o tutorial para saber como gerir as atualizações para as suas VMs do Windows.

> [!div class="nextstepaction"]
> [Gerir as atualizações e correções de erros para as suas VMs do Windows Azure](automation-tutorial-troubleshoot-changes.md)

* Utilizar as Pesquisas de Registos no [Log Analytics](../log-analytics/log-analytics-log-searches.md) para ver dados de atualizações detalhados.
* [Criar alertas](../log-analytics/log-analytics-alerts.md) para quando são detetadas atualizações críticas em falta nos computadores ou quando um computador tiver as atualizações automáticas desativadas.

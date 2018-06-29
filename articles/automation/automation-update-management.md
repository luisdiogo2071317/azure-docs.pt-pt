---
title: Solução de gestão de atualizações no Azure
description: Este artigo destina-se para o ajudar a compreender como utilizar a solução de gestão de atualizações do Azure para gerir atualizações para os computadores Windows e Linux.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 06/28/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 3de93c06285f36353d91a66db975c0a579c1379c
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37097449"
---
# <a name="update-management-solution-in-azure"></a>Solução de gestão de atualizações no Azure

Pode utilizar a solução de gestão de atualizações na automatização do Azure para gerir atualizações do sistema operativo para os computadores Windows e Linux que estão implementados no Azure, nos ambientes no local ou em outros fornecedores de nuvem. Pode rapidamente avaliar o estado das atualizações disponíveis em todos os computadores agente e gerir o processo de instalação de atualizações necessárias para os servidores.

Pode ativar a gestão de atualização para máquinas virtuais diretamente a partir da sua conta de automatização do Azure. Para saber como ativar a gestão de atualizações para as máquinas virtuais da sua conta de automatização, consulte [gerir atualizações para várias máquinas virtuais](manage-update-multi.md). Também pode ativar a gestão de atualização para uma única máquina virtual no painel da máquina virtual no portal do Azure. Este cenário está disponível para [Linux](../virtual-machines/linux/tutorial-monitoring.md#enable-update-management) e [Windows](../virtual-machines/windows/tutorial-monitoring.md#enable-update-management) máquinas virtuais.

## <a name="solution-overview"></a>Descrição geral da solução

Computadores que são geridos pela gestão de atualização de utilizarem as seguintes configurações para efetuar a avaliação e implementações de atualizações:

* Microsoft Monitoring Agent (MMA) para Windows ou Linux
* Configuração de Estado Pretendido do PowerShell (DSC) para Linux
* Função de Trabalho de Runbook Híbrida da Automatização
* O Microsoft Update ou Windows Server Update Services (WSUS) para computadores Windows

O diagrama seguinte mostra uma vista concetual do comportamento e ligados de fluxo de dados com a forma como a solução avalia e aplica as atualizações de segurança para todos os Windows Server e os computadores com Linux numa área de trabalho:

![Atualizar o fluxo de processo de gestão](media/automation-update-management/update-mgmt-updateworkflow.png)

Depois de um computador efetua uma análise de compatibilidade de atualização, o agente reencaminha as informações em massa ao Log Analytics do Azure. Num computador Windows, é efetuada a verificação de compatibilidade cada 12 horas, por predefinição.

Para além do agendamento da análise, a análise da compatibilidade da atualização é iniciada em 15 minutos, se o MMA for reiniciado, antes da instalação da atualização e, após a instalação da atualização.

Para um computador com Linux, a análise de conformidade é executada a cada 3 horas por predefinição. Se o agente MMA for reiniciado, uma análise de compatibilidade é iniciada em 15 minutos.

A solução de relatórios como atualizados, o computador é com base no que de origem estão configurados para sincronizar com. Se o computador do Windows está configurado para comunicar com o WSUS, dependendo se WSUS última sincronização com o Microsoft Update, os resultados podem divergir do que mostra Updates da Microsoft. Este é o mesmo para computadores com Linux que estão configurados para o relatório para um repositório local em vez de para um repositório público.

> [!NOTE]
> Para comunicar corretamente ao serviço, a gestão de atualizações requer determinadas portas e URLs esteja ativado. Para obter mais informações sobre estes requisitos, consulte o artigo [rede planear híbridos](automation-hybrid-runbook-worker.md#network-planning).

Pode criar uma implementação agendada para implementar e instalar atualizações de software em computadores que precisam das atualizações. As atualizações classificadas como *opcional* não estiverem incluídos no âmbito da implementação para computadores Windows. Apenas as atualizações necessárias estão incluídas no âmbito da implementação. 

A implementação planeada define os computadores de destino recebem as atualizações aplicáveis, especificando explicitamente computadores ou selecionando um [grupo de computadores](../log-analytics/log-analytics-computer-groups.md) que baseia-se em procuras de registo de um conjunto específico de computadores. Também especificar uma agenda para aprovar e designar um período de tempo durante os quais podem ser instaladas as atualizações. 

As atualizações são instaladas por runbooks na Automatização do Azure. Não é possível ver estes runbooks e os runbooks não necessita de qualquer configuração. Quando é criada uma implementação de atualização, a implementação da atualização cria uma agenda que inicia um runbook de principal de atualização no momento especificado para os computadores incluídos. O runbook principal inicia um runbook subordinado em cada agente para efetuar a instalação de atualizações necessárias.

A data e hora especificadas na implementação de atualização, os computadores de destino executar a implementação em paralelo. Antes da instalação, é efetuada uma análise para verificar se as atualizações são ainda necessárias. Para computadores de cliente do WSUS, se as atualizações não aprovadas no WSUS, a implementação da atualização falha.

## <a name="clients"></a>Clientes

### <a name="supported-client-types"></a>Tipos de clientes suportados

A tabela seguinte mostra uma lista dos sistemas operativos suportados:

|Sistema operativo  |Notas  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Suporta apenas atualizar avaliações.         |
|Windows Server 2008 R2 SP1 e posterior     |É necessário o .NET framework 4.5 ou posterior. ([Transferir o .NET Framework](/dotnet/framework/install/guide-for-developers))<br/> É necessário o Windows PowerShell 4.0 ou posterior. ([Transferir WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855))<br/> 5.1 do Windows PowerShell é recomendada para uma maior fiabilidade.  ([Transferir WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))        |
|CentOS 6 (x86/x64) e 7 (x64)      | Os agentes do Linux têm de ter acesso a um repositório de atualização. Aplicação de patches baseada na classificação requer 'yum' devolver dados de segurança que CentOS não tem a box.         |
|Red Hat Enterprise 6 (x86/x64) e 7 (x64)     | Os agentes do Linux têm de ter acesso a um repositório de atualização.        |
|SUSE Linux Enterprise Server 11 (x86/x64) e 12 (x64)     | Os agentes do Linux têm de ter acesso a um repositório de atualização.        |
|Ubuntu 14.04 LTS e 16.04 LTS (x86/x64)      |Os agentes do Linux têm de ter acesso a um repositório de atualização.         |

### <a name="unsupported-client-types"></a>Tipos de cliente não suportada

A tabela seguinte lista os sistemas operativos que não são suportados:

|Sistema operativo  |Notas  |
|---------|---------|
|Cliente Windows     | Não são suportados sistemas operativos de cliente (por exemplo, Windows 7 e Windows 10).        |
|Servidor do Windows Server 2016 Nano     | Não suportado.       |

### <a name="client-requirements"></a>Requisitos do cliente

#### <a name="windows"></a>Windows

Agentes do Windows tem de ser configurados para comunicar com um servidor WSUS ou têm de ter acesso ao Microsoft Update. Pode utilizar a gestão de atualizações com o System Center Configuration Manager. Para obter mais informações sobre cenários de integração, consulte o artigo [integrar o System Center Configuration Manager com a gestão de atualizações](oms-solution-updatemgmt-sccmintegration.md#configuration). O [agente do Windows](../log-analytics/log-analytics-agent-windows.md) é necessária. O agente é instalado automaticamente se a integração de uma máquina virtual do Azure.

#### <a name="linux"></a>Linux

Para Linux, a máquina tem de ter acesso a um repositório de atualização. O repositório de atualização pode ser privado ou público. É necessário TLS 1.1 ou TLS 1.2 para interagir com a gestão de atualizações. Um agente do Operations Management Suite (OMS) para o Linux que está configurado para o relatório para várias áreas de trabalho de análise de registos não é suportado com esta solução.

Para obter informações sobre como instalar o agente do OMS para Linux e para transferir a versão mais recente, consulte [agente do Operations Management Suite para Linux](https://github.com/microsoft/oms-agent-for-linux). Para obter informações sobre como instalar o agente do OMS para Windows, consulte [Operations Management Suite agente do Windows](../log-analytics/log-analytics-windows-agent.md).

## <a name="permissions"></a>Permissões

Para criar e gerir implementações de atualização, terá de permissões específicas. Para saber mais sobre estas permissões, consulte [acesso baseado em funções - gestão de atualizações](automation-role-based-access-control.md#update-management).

## <a name="solution-components"></a>Componentes da solução

A solução consiste dos seguintes recursos. Os recursos são adicionados à sua conta de automatização. Se estiver a qualquer um dos agentes ligados diretamente ou num grupo de gestão ligado-Operations Manager.

### <a name="hybrid-worker-groups"></a>Grupos de Função de Trabalho Híbrida

Depois de ativar esta solução, qualquer computador com Windows que está diretamente ligado à sua área de trabalho de análise de registos é automaticamente configurado como um Runbook Worker híbrido para suportar os runbooks que estão incluídos nesta solução.

Cada computador do Windows que é gerida pela solução é listado no **grupos de trabalho híbrida** painel como uma **grupo de trabalho do sistema híbridas** para a conta de automatização. As soluções utilizam a Convenção de nomenclatura *Hostname FQDN_GUID*. Não pode visar estes grupos com runbooks na sua conta. Estas falhar se tentar. Estes grupos destinam-se para suportar a solução de gestão.

Pode adicionar os computadores Windows a um grupo de trabalho de Runbook híbrida na sua conta de automatização para suportar runbooks de automatização, se utilizar a mesma conta para a solução e a associação ao grupo de trabalho de Runbook híbrida. Esta funcionalidade foi adicionada na versão 7.2.12024.0 o Runbook Worker híbrido.

### <a name="management-packs"></a>Pacotes de gestão

Se o grupo de gestão do System Center Operations Manager está ligado a uma área de trabalho de análise de registos, os seguintes pacotes de gestão estão instalados no Operations Manager. Estes pacotes de gestão também são instalados em computadores com Windows diretamente ligados depois de adicionar a solução. Não precisa de configurar ou gerir estes pacotes de gestão.

* Pacote de Informações de Avaliação de Atualização do Microsoft System Center Advisor (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Pacote de Gestão de Implementação de Atualização

Para obter mais informações sobre a forma como os pacotes de gestão de solução são atualizados, consulte [estabelecer a ligação do Operations Manager ao Log Analytics](../log-analytics/log-analytics-om-agents.md).

### <a name="confirm-that-non-azure-machines-are-onboarded"></a>Confirme que não Azure máquinas estão integradas

Para confirmar que ligadas diretamente as máquinas estão a comunicar com a análise de registos, após alguns minutos, pode executar uma procura de registo seguinte.

#### <a name="linux"></a>Linux

```
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a>Windows

```
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

Num computador Windows, pode rever as informações seguintes para verificar a conectividade de agente com a análise de registos:

1. No painel de controlo, abra **Microsoft Monitoring Agent**. No **Log Analytics do Azure** separador, o agente apresenta a seguinte mensagem: **o Microsoft Monitoring Agent foi ligado com êxito ao Log Analytics**.
2. Abra o registo de eventos do Windows. Aceda a **aplicação e o Gestor de serviços de Logs\Operations** e procure o evento ID 3000 e 5002 de ID de evento da origem **conector de serviços**. Estes eventos indicarem que o computador foi registado com a área de trabalho de análise de registos e está a receber configuração.

Se o agente não consegue comunicar com a análise de registos e o agente for configurado para comunicar com a internet através de um servidor de firewall ou proxy, confirme se o servidor de firewall ou proxy está configurado corretamente. Para saber como verificar se o servidor de firewall ou proxy está configurado corretamente, consulte [configuração de rede para o agente do Windows](../log-analytics/log-analytics-agent-windows.md) ou [configuração de rede para o agente Linux](../log-analytics/log-analytics-agent-linux.md).

> [!NOTE]
> Se os sistemas Linux estão configurados para comunicar com um proxy ou Gateway do OMS e está a inclusão desta solução, a atualização a *proxy.conf* permissões para o grupo de omiuser de conceder permissão de leitura no ficheiro utilizando o seguinte comandos:
>
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Agentes de Linux recentemente adicionados mostram um Estado de **atualizados** depois de ter sido efetuada uma avaliação. Este processo pode demorar até seis horas.

Para confirmar que um grupo de gestão do Operations Manager está a comunicar com a análise de registos, consulte [integração de validar o Operations Manager com a análise de registos](../log-analytics/log-analytics-om-agents.md#validate-operations-manager-integration-with-log-analytics).

## <a name="data-collection"></a>Recolha de dados

### <a name="supported-agents"></a>Agentes suportados

A tabela seguinte descreve as origens de ligado que são suportadas por esta solução:

| Origem ligada | Suportado | Descrição |
| --- | --- | --- |
| Agentes do Windows |Sim |A solução recolhe informações sobre atualizações do sistema de agentes do Windows e, em seguida, inicia a instalação de atualizações necessárias. |
| Agentes do Linux |Sim |A solução recolhe informações sobre atualizações do sistema de agentes Linux e, em seguida, inicia a instalação de atualizações necessárias no distribuições suportadas. |
| Grupo de gestão do Operations Manager |Sim |A solução recolhe informações sobre atualizações do sistema de agentes num grupo de gestão ligado.<br/>Uma ligação direta do agente do Operations Manager para análise de registos não é necessária. Dados seja reencaminhados do grupo de gestão para a área de trabalho de análise de registos. |

### <a name="collection-frequency"></a>Frequência da recolha

Uma análise é executada duas vezes por dia para cada computador gerido do Windows. A cada 15 minutos, a API do Windows denomina-se a consulta para a hora da última atualização determinar se o estado mudou. Se tiver alterado o estado, é iniciada uma análise de compatibilidade. 

Uma análise é executada a cada 3 horas para cada computador Linux gerido.

Pode demorar entre 30 minutos e 6 horas para o dashboard apresentar dados atualizados a partir de computadores geridos.

## <a name="viewing-update-assessments"></a>Avaliações de atualização de vista

Na sua conta de automatização, selecione **gestão de atualizações** para ver o estado das máquinas.

Esta vista fornece informações sobre as máquinas, em falta atualizações, implementações de atualização e implementações de atualização agendada. No **conformidade coluna**, pode ver a última vez que a máquina foi avaliada. No **preparação de ATUALIZAÇÃO de agente** coluna, pode ver se o estado de funcionamento do agente de atualização. Se existir um problema, selecione a ligação para ir para resolução de problemas de documentação que pode ajudar a que saber que os passos para corrigir o problema.

Para executar uma pesquisa de registo que devolve informações sobre o computador, atualização ou implementação, selecione o item na lista. O **pesquisa registo** painel abre-se com uma consulta para o item selecionado:

![Atualizar a vista de gestão predefinido](media/automation-update-management/update-management-view.png)

## <a name="install-updates"></a>Instalar atualizações

Depois das atualizações são avaliadas para todos os computadores Linux e Windows na sua área de trabalho, pode instalar as atualizações necessárias através da criação de um *atualizar implementação*. Uma implementação de atualização é uma instalação agendada de atualizações necessárias para um ou mais computadores. Especifique a data e hora para a implementação e um computador ou grupo de computadores para incluir no âmbito de uma implementação. Para saber mais sobre grupos de computadores, veja [Computer groups in Log Analytics](../log-analytics/log-analytics-computer-groups.md) (Grupos de computadores no Log Analytics).

 Ao incluir grupos de computadores na sua implementação de atualização, a associação ao grupo é avaliada apenas uma vez, no momento da criação da agenda. Não são refletidas subsequentes alterações a um grupo. Para contornar este problema, elimine a implementação de atualização agendada e voltar a criar.

> [!NOTE]
> Máquinas virtuais do Windows que são implementadas no Azure Marketplace, por predefinição são definidas para receber as atualizações automáticas do serviço de atualização do Windows. Este comportamento não irá alterar quando adicionar esta solução ou adicionar máquinas virtuais do Windows à sua área de trabalho. Se não ativamente gerir atualizações ao utilizar esta solução, aplica-se o comportamento predefinido (para aplicar automaticamente as atualizações).

Para evitar a ser aplicadas fora da janela de manutenção no Ubuntu de atualizações, reconfigure o pacote de atualização de automático para desativar as atualizações automáticas. Para obter informações sobre como configurar o pacote, consulte [tópico de atualizações automáticas no guia de servidor Ubuntu](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Máquinas virtuais que foram criadas a partir de imagens do Red Hat Enterprise Linux (RHEL) a pedido que estão disponíveis no Azure Marketplace estão registadas para o acesso a [Red Hat atualização infraestrutura (RHUI)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) que é implementada no Azure. Quaisquer outra distribuição Linux têm de ser atualizada do repositório de ficheiros online a distribuição seguindo métodos suportados a distribuição.

## <a name="view-missing-updates"></a>Ver as atualizações em falta

Selecione **atualizações em falta** para ver a lista de atualizações que estão em falta das suas máquinas. Cada atualização é apresentado na lista e pode ser selecionada. São apresentadas a informações sobre o número de máquinas que necessitam de atualização, o sistema operativo e uma hiperligação para obter mais informações. O **pesquisa registo** painel mostra mais detalhes sobre as atualizações.

## <a name="view-update-deployments"></a>Implementações de atualização de vista

Selecione o **implementações de atualização** separador para ver a lista de implementações de atualização existente. Selecione qualquer uma das implementações de atualização da tabela para abrir o **atualizar implementação executada** painel para essa implementação da atualização.

![Descrição geral dos resultados da implementação de atualização](./media/automation-update-management/update-deployment-run.png)

## <a name="create-or-edit-an-update-deployment"></a>Criar ou editar uma implementação de atualização

Para criar uma nova implementação de atualização, selecione **implementação de atualização de agendamento**. O **nova implementação de atualização** abre o painel. Introduza valores para as propriedades descritas na tabela seguinte:

| Propriedade | Descrição |
| --- | --- |
|Nome |O nome exclusivo para identificar a implementação de atualizações. |
|Sistema Operativo| Selecione **Linux** ou **Windows**.|
|Máquinas de atualização |Selecione uma procura guardada ou selecione **máquina** na lista pendente e, em seguida, selecione máquinas individuais. |
|Classificações de atualizações|Selecione todas as classificações de atualização que precisa. CentOS não suporta esta a box.|
|Atualizações para excluir|Introduza as atualizações para excluir. Para o Windows, introduza o artigo KB sem o **KB** prefixo. Para Linux, introduza o nome do pacote ou utilizar um caráter universal.  |
|Definições de agendamento|Selecione a hora para iniciar e, em seguida, selecione **uma vez** ou **periódica** para a periodicidade.|| Janela de manutenção |Número de minutos definido para atualizações. O valor não pode ser inferior a 30 minutos ou mais de 6 horas. |

## <a name="update-classifications"></a>Classificações de atualizações

As tabelas seguintes listam as classificações de atualização na gestão de atualização, com uma definição para cada classificação.

### <a name="windows"></a>Windows

|Classificação  |Descrição  |
|---------|---------|
|Atualizações críticas     | Uma atualização para um problema específico que corrige um crítico não relacionado com segurança.        |
|Atualizações de segurança     | Uma atualização para um problema específico do produto, relacionadas com segurança.        |
|Update rollups     | Um conjunto cumulativo de correções que são agrupadas para facilitar a implementação.        |
|Pacotes de funcionalidades     | Novas funcionalidades do produto que são distribuídas fora de uma versão do produto.        |
|Service packs     | Um conjunto cumulativo de correções que são aplicadas a uma aplicação.        |
|Atualizações de definições     | Uma atualização para vírus ou outros ficheiros de definição.        |
|Ferramentas     | Um utilitário ou funcionalidade que ajuda a concluir as tarefas de um ou mais.        |
|Atualizações     | Uma atualização para uma aplicação ou o ficheiro que está instalado atualmente.        |

### <a name="linux"></a>Linux

|Classificação  |Descrição  |
|---------|---------|
|Atualizações críticas e de segurança     | Atualizações para um problema específico ou um problema específico do produto, relacionadas com segurança.         |
|Outras atualizações     | Todas as outras atualizações que não são críticas natureza ou não são atualizações de segurança.        |

Para Linux, gestão de atualizações pode distinguir entre críticas e atualizações de segurança na nuvem ao apresentar dados da avaliação devido a sem causa de dados na nuvem. Para a aplicação de patches, gestão de atualizações baseia-se em dados de classificação disponíveis no computador. Ao contrário de outras distribuições, CentOS não tenha disponíveis estas informações a box. Se tiver máquinas CentOS configuradas de forma a devolver dados de segurança para o seguinte comando, gestão de atualização irá conseguir patch com base nas classificações.

```bash
sudo yum -q --security check-update
```

Atualmente, não há nenhum método de método suportado para ativar a disponibilidade de dados de classificação nativos no CentOS. Neste momento, apenas melhor esforço suporte é fornecido para os clientes que podem ativar este por si próprios.

## <a name="ports"></a>Portas

Os seguintes endereços são necessários especificamente para a gestão de atualização. Ocorrerá a comunicação para estes endereços através da porta 443.

|Público do Azure  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     |*. ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *. oms.opinsights.azure.us        |
|*.blob.core.windows.net|*. blob.core.usgovcloudapi.net|

Para obter mais informações sobre as portas que requer que o Runbook Worker híbrido, consulte [portas de função de Worker híbrido](automation-hybrid-runbook-worker.md#hybrid-worker-role).

## <a name="search-logs"></a>Registos de pesquisa

Além dos detalhes que são fornecidos no portal do Azure, pode efetuar pesquisas contra os registos. Nas páginas solução, selecione **Log Analytics**. O **pesquisa registo** abre o painel.

Também pode aprender como personalizar as consultas ou utilizá-las de diferentes clientes e mais, visitando: [documentação seach API de análise de registos](
https://dev.loganalytics.io/).

### <a name="sample-queries"></a>Consultas de exemplo

As secções seguintes fornecem exemplos de consultas de registo para os registos de atualização que são recolhidos por esta solução:

#### <a name="single-azure-vm-assessment-queries-windows"></a>Consultas de avaliação do Azure VM únicas (Windows)

Substitua o valor VMUUID com o GUID de VM da máquina virtual que estiver a consultar. Pode encontrar o VMUUID que deve ser utilizada ao executar a consulta seguinte na análise de registos: `Update | where Computer == "<machine name>" | summarize by Computer, VMUUID`

##### <a name="missing-updates-summary"></a>Resumo de atualizações em falta

```
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"b08d5afa-1471-4b52-bd95-a44fea6e4ca8"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security"
```

##### <a name="missing-updates-list"></a>Lista de atualizações em falta

```
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"8bf1ccc6-b6d3-4a0b-a643-23f346dfdf82"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

#### <a name="single-azure-vm-assessment-queries-linux"></a>Consultas de avaliação do únicas VM do Azure (Linux)

Para alguns distros de Linux não existe um [ordenação](https://en.wikipedia.org/wiki/Endianness) incompatibilidade com o valor VMUUID provém do Azure Resource Manager e o que é armazenado na análise de registos. Verifica a seguinte consulta para obter uma correspondência da ordenação. Substitua os valores VMUUID o big endian e little endian o formato do GUID corretamente devolver resultados. Pode encontrar o VMUUID que deve ser utilizada ao executar a consulta seguinte na análise de registos: `Update | where Computer == "<machine name>"
| summarize by Computer, VMUUID`

##### <a name="missing-updates-summary"></a>Resumo de atualizações em falta

```
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

##### <a name="missing-updates-list"></a>Lista de atualizações em falta

```
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl

```

#### <a name="multi-vm-assessment-queries"></a>Consultas de avaliação de várias VMS

##### <a name="computers-summary"></a>Resumos de computadores

```
Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Approved, Optional, Classification) by SourceComputerId, UpdateID
    | distinct SourceComputerId, Classification, UpdateState, Approved, Optional
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed" and (Optional==false or Classification has "Critical" or Classification has "Security") and Approved!=false, iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity
| union (Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by SourceComputerId, Product, ProductArch
    | distinct SourceComputerId, Classification, UpdateState
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed", iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity)
| summarize assessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity>-1), notAssessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==-1), computersNeedCriticalUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==4), computersNeedSecurityUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==2), computersNeeedOtherUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==1), upToDateComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==0)
| summarize assessedComputersCount=sum(assessedComputersCount), computersNeedCriticalUpdatesCount=sum(computersNeedCriticalUpdatesCount),  computersNeedSecurityUpdatesCount=sum(computersNeedSecurityUpdatesCount), computersNeeedOtherUpdatesCount=sum(computersNeeedOtherUpdatesCount), upToDateComputersCount=sum(upToDateComputersCount), notAssessedComputersCount=sum(notAssessedComputersCount)
| extend allComputersCount=assessedComputersCount+notAssessedComputersCount


```

##### <a name="missing-updates-summary"></a>Resumo de atualizações em falta

```
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| union (Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification )
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security"
```

##### <a name="computers-list"></a>Lista de computadores

```
Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Product, Computer, ComputerEnvironment) by SourceComputerId, Product, ProductArch
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed"), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed"), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed"), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2)
| union(Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, Optional, Approved, Computer, ComputerEnvironment) by Computer, SourceComputerId, UpdateID
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed" and Approved!=false), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed" and Approved!=false), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed" and Optional==false and Approved!=false), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2) )
| order by ComplianceOrder asc, missingCriticalUpdatesCount desc, missingSecurityUpdatesCount desc, missingOtherUpdatesCount desc, displayName asc
| project-away ComplianceOrder
```

##### <a name="missing-updates-list"></a>Lista de atualizações em falta

```
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| union(Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2)
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

## <a name="integrate-with-system-center-configuration-manager"></a>Integrar no System Center Configuration Manager

Os clientes que tenham investido no System Center Configuration Manager para gerir PCs, servidores e dispositivos móveis baseiam-se também na força e maturidade do Configuration Manager para ajudar a gerir atualizações de software. O Configuration Manager faz parte do respetivo ciclo de gestão (SUM) de atualização de software.

Para saber como integrar a solução de gestão com o System Center Configuration Manager, consulte [integrar o System Center Configuration Manager com a gestão de atualizações](oms-solution-updatemgmt-sccmintegration.md).

## <a name="patch-linux-machines"></a>Máquinas de patch Linux

As secções seguintes explicam potenciais problemas com a aplicação de patches do Linux.

### <a name="unexpected-os-level-upgrades"></a>Atualizações de nível de SO inesperadas

Em algumas variantes do Linux, tais como Red Hat Enterprise Linux, podem ocorrer atualizações ao nível do SO através de pacotes. Isto poderá originar execuções de atualização de gestão onde o número de versão do SO é alterado. Como atualizar gestão utiliza os mesmos métodos para atualizar os pacotes que um administrador pretende utilizar localmente no computador Linux, este comportamento é intencional.

Para evitar a atualizar a versão do SO através de execuções de gestão de atualizações, utilize o **exclusão** funcionalidade.

Red Hat Enterprise Linux, o nome do pacote para excluir é server.x86_64 de versão de VM de redhat.

![Pacotes para excluir para Linux](./media/automation-update-management/linuxpatches.png)

### <a name="critical--security-patches-arent-applied"></a>Crítico / patches de segurança não são aplicadas

Ao implementar atualizações para uma máquina com Linux, pode selecionar classificações de atualização. Este procedimento filtra as atualizações que são aplicadas aos que cumprem os critérios especificados. Este filtro ser aplicado localmente no computador quando a atualização é implementada.

Porque atualizar gestão efetua sem causa de atualização na nuvem, algumas atualizações podem ser sinalizadas na gestão de atualização como tendo o impacto de segurança, apesar do computador local não ter informações. Como resultado, se aplicar atualizações críticas para uma máquina com Linux, poderá ser atualizações que não estão marcadas como tendo o impacto de segurança que a máquina e as atualizações não são aplicadas.

No entanto, atualizar poderá ainda relatório de gestão que a máquina como estando não conformidade porque tem informações adicionais sobre a atualização relevante.

Implementar atualizações de classificação de atualização não funciona em CentOS a box. Para SUSE, selecionar *apenas* 'Outras atualizações' como a classificação pode resultar na segurança de algumas das atualizações também a ser instalada se as atualizações de segurança relacionadas com zypper (Gestor de pacote) ou as respetivas dependências são necessárias pela primeira vez. Esta é uma limitação do zypper. Em alguns casos, poderá ser necessário para executar novamente a implementação de atualização, certifique-se verifique o registo de atualização.

## <a name="troubleshoot"></a>Resolução de problemas

Para saber como resolver problemas de gestão de atualização, consulte [resolução de problemas de gestão de atualizações](troubleshoot/update-management.md)

## <a name="next-steps"></a>Passos seguintes

Continue para o tutorial para saber como gerir atualizações para as máquinas virtuais do Windows.

> [!div class="nextstepaction"]
> [Gerir as atualizações e correções de erros para as suas VMs do Windows Azure](automation-tutorial-update-management.md)

* Utilizar pesquisas de registo no [Log Analytics](../log-analytics/log-analytics-log-searches.md) para ver os dados de atualização de detalhado.
* [Criar alertas](../log-analytics/log-analytics-alerts.md) quando atualizações críticas são detetadas como estando em falta a partir de computadores ou se um computador tiver desativadas as atualizações automáticas.

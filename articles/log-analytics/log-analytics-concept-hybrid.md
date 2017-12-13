---
title: Recolher dados do seu ambiente com o Log Analytics do Azure | Microsoft Docs
description: "Este tópico ajuda-o a compreender como recolher dados e monitorizar computadores alojados no local ou outro ambiente de nuvem com a análise de registos."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: magoedte
ms.openlocfilehash: 513855084c8b89d97b049f1df2ec24d0f9789afe
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2017
---
# <a name="collect-data-from-computers-in-your-environment-with-log-analytics"></a>Recolher dados de computadores no seu ambiente com a análise de registos

Análise de registos do Azure pode recolher e atuar sobre dados de computadores Windows ou Linux que reside no:

* [Máquinas virtuais do Azure](log-analytics-quick-collect-azurevm.md) através da extensão de VM de análise do registo 
* O Centro de dados como servidores físicos ou máquinas virtuais
* Máquinas virtuais num serviço alojado na nuvem como Amazon Web Services (AWS)

Computadores alojados no seu ambiente podem ser diretamente ligados à análise de registos ou se já estiver a monitorizar estes computadores com o System Center Operations Manager 2012 R2 ou 2016, pode integrar o grupo de gestão Operations gerir com a análise de registos e Continue a manter a sua estratégia e os processos de operações do serviço.  

## <a name="overview"></a>Descrição geral

![log-Analytics-Agent-Direct-Connect-Diagram](media/log-analytics-concept-hybrid/log-analytics-on-prem-comms.png)

Antes de analisar e a funcionar nos dados recolhidos, terá primeiro de instalar e ligar agentes para todos os computadores que pretende enviar dados para o serviço de análise de registos. Pode instalar agentes nos computadores no local utilizando a configuração, a linha de comandos, ou com pretendido Estado Configuration (DSC) na automatização do Azure. 

O agente para Linux e Windows comunica saído com o serviço de análise de registos através da porta TCP 443 e se o computador liga ao servidor de firewall ou proxy para comunicar através da Internet, reveja [configurar o agente para utilização com um servidor proxy ou Gateway do OMS](#configuring-the-agent-for-use-with-a-proxy-server-or-oms-gateway) para compreender que configuração é alterado será tem de ser aplicadas. Se estiver a monitorizar o computador com o System Center 2016 - Operations Manager ou do Operations Manager 2012 R2, pode ser multihomed com o serviço de análise de registos para recolher dados e reencaminhá-los para o serviço e ainda ser monitorizados pelo [do Operations Manager ](log-analytics-om-agents.md). Computadores com Linux monitorizados por um grupo de gestão do Operations Manager integrado com a análise de registos não receber a configuração para origens de dados e os dados recolhidos reencaminhar através do grupo de gestão. O agente do Windows pode reportar até quatro áreas de trabalho, enquanto o agente Linux só suporta relatórios para uma única área de trabalho.  

O agente para Linux e Windows não é apenas para estabelecer a ligação para análise de registos, também suporta a ligação com a automatização do Azure para a função de trabalho de Runbook híbrida do anfitrião e soluções de gestão, tais como controlo de alterações e gestão de atualizações.  Para obter mais informações sobre a função Runbook Worker híbrido, consulte [trabalho de Runbook híbrida de automatização do Azure](../automation/automation-offering-get-started.md#automation-architecture-overview).  

Se as políticas de segurança de TI não permitir que os computadores na sua rede para ligar à Internet, o agente pode ser configurado para ligar ao Gateway para receber as informações de configuração e enviar os dados recolhidos consoante a solução que tiver ativado o OMS. Para obter mais informações e os passos sobre como configurar o agente Linux ou do Windows para comunicar através de um Gateway do OMS para o serviço de análise de registos, consulte [ligar computadores ao OMS utilizando o Gateway do OMS](log-analytics-oms-gateway.md). 

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar, reveja os detalhes seguintes para verificar que cumpre os requisitos mínimos do sistema.

### <a name="windows-operating-system"></a>Sistema operativo Windows
As seguintes versões do sistema operativo Windows oficialmente são suportadas para o agente do Windows:

* Windows Server 2008 Service Pack 1 (SP1) ou posterior
* Windows 7 SP1 e posterior.

#### <a name="network-configuration"></a>Configuração da rede
As informações abaixo lista as informações de configuração de proxy e de firewall necessárias para o agente do Windows comunicar com a análise de registos. O tráfego é de saída da sua rede para o serviço de análise de registos. 

| Recursos do Agente | Portas | Inspeção de HTTPS direto|
|----------------|-------|------------------------|
|*.ods.opinsights.azure.com |443 | Sim |
|*.oms.opinsights.azure.com | 443 | Sim | 
|*.blob.core.windows.net | 443 | Sim | 
|*.azure-automation.net | 443 | Sim | 

### <a name="linux-operating-systems"></a>Sistemas operativos Linux
As distribuições de Linux seguintes são suportadas oficialmente.  No entanto, o agente Linux pode também executar nos outras distribuições não listadas.

* Linux Amazon 2012.09 para 2015.09 (x86/x64)
* CentOS Linux 5, 6 e 7 (x86/x64)
* Oracle Linux 5, 6 e 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 e 7 (x86/x64)
* Debian GNU/Linux 6, 7 e 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 e 12 (x86/x64)

#### <a name="network-configuration"></a>Configuração da rede
As informações abaixo lista as informações de configuração de proxy e de firewall necessárias para o agente do Linux comunicar com a análise de registos. O tráfego é de saída da sua rede para o serviço de análise de registos. 

|Recursos do Agente| Portas |  
|------|---------|  
|*.ods.opinsights.azure.com | Porta 443|   
|*.oms.opinsights.azure.com | Porta 443|   
|*.blob.core.windows.net | Porta 443|   
|*.azure-automation.net | Porta 443|  

O agente Linux suporta comunicar através de um servidor proxy ou Gateway do OMS para o serviço de análise de registos utilizando o protocolo HTTPS.  Autenticação básica e anónima (nome de utilizador/palavra-passe) são suportados.  O servidor proxy pode ser especificado durante a instalação ou ao modificar o ficheiro de configuração proxy.conf após a instalação.  

O valor de configuração de proxy tem a seguinte sintaxe:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Se o servidor proxy necessitar que se autentique, o agente Linux ainda necessita que fornece uma utilizador de pseudo/palavra-passe. Isto pode ser qualquer nome de utilizador ou palavra-passe.

|Propriedade| Descrição |
|--------|-------------|
|Protocolo | https |
|Utilizador | Nome de utilizador opcional para a autenticação de proxy |
|palavra-passe | Palavra-passe opcional para a autenticação de proxy |
|proxyhost | Endereço ou o FQDN do proxy servidor/OMS Gateway |
|porta | Número de porta opcional para o proxy servidor/OMS Gateway |

Por exemplo: `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Se utilizar carateres especiais, tais como "@" na sua palavra-passe, receberá um erro de ligação de proxy porque o valor é analisado incorretamente.  Para contornar este problema, codificar a palavra-passe no URL utilizando uma ferramenta como [URLDecode](https://www.urldecoder.org/).  

## <a name="install-and-configure-agent"></a>Instalar e configurar o agente 
Ligar os computadores no local diretamente com a análise de registos pode ser conseguido através de métodos diferentes consoante as suas necessidades. A tabela seguinte realça cada método para determinar que funciona melhor na sua organização.

|Origem | Método | Descrição|
|-------|-------------|-------------|
| Computador Windows|- [Instalação manual](log-analytics-agent-windows.md)<br>- [DSC de automatização do Azure](log-analytics-agent-windows.md#install-the-agent-using-dsc-in-azure-automation)<br>- [Modelo do Resource Manager com a pilha do Azure](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) |Instale o Microsoft Monitoring agent a partir da linha de comandos ou utilizando um método automatizado, tais como o DSC de automatização do Azure, [System Center Configuration Manager](https://docs.microsoft.com/sccm/apps/deploy-use/deploy-applications), ou com um modelo Azure Resource Manager, se tiver implementado Microsoft Pilha do Azure no seu centro de dados.| 
|Computador com Linux| [Instalação manual](log-analytics-quick-collect-linux-computer.md)|Instale o agente para Linux ao chamar um script de wrapper alojado no GitHub. | 
| System Center Operations Manager|[Integrar o Operations Manager com a análise de registos](log-analytics-om-agents.md) | Configurar a integração entre o Operations Manager e análise de registos para reencaminhar recolhidos dados de computadores Linux e Windows relatam a um grupo de gestão.|  

## <a name="next-steps"></a>Passos seguintes

* Reveja [origens de dados](log-analytics-data-sources.md) para compreender as origens de dados disponíveis para recolher dados do sistema Windows ou Linux. 

* Saiba mais sobre [pesquisas de registo](log-analytics-log-searches.md) para analisar os dados recolhidos a partir de origens de dados e soluções. 

* Saiba mais sobre [soluções](log-analytics-add-solutions.md) que adicionar funcionalidades à análise de registos e também recolher dados para o repositório do OMS.
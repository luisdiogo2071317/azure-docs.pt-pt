---
title: Recolher dados num ambiente híbrido com o agente do Log Analytics do Azure | Documentos da Microsoft
description: Este tópico ajuda-o a compreender como recolher dados e monitorizar computadores alojados no outro ambiente de cloud com o Log Analytics ou no local.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 96feb52bd5702c899faa8d845969ae8ba0995504
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39495361"
---
# <a name="collect-data-in-a-hybrid-environment-with-log-analytics-agent"></a>Recolher dados num ambiente híbrido com o agente do Log Analytics

O Azure Log Analytics pode recolher e agir sobre dados de computadores com o sistema operativo Windows ou Linux em execução no:

* [Máquinas virtuais do Azure](log-analytics-quick-collect-azurevm.md) usando a extensão de VM do Log Analytics 
* Seu datacenter como servidores físicos ou máquinas virtuais
* Máquinas virtuais num serviço alojado na cloud, como Amazon Web Services (AWS)

Computadores alojados no seu ambiente podem ser ligados diretamente ao Log Analytics ou se já estiver a monitorizar estes computadores com o System Center Operations Manager 2012 R2 ou posterior, pode integrar o seu grupo de gestão do Operations Manager com o Log Analytics e continuar a manter seus processos de operações do serviço de TI.  

## <a name="overview"></a>Descrição geral

![log-analytics-agent-direct-connect-diagram](media/log-analytics-concept-hybrid/log-analytics-on-prem-comms.png)

Antes de analisar e atuar sobre os dados recolhidos, tem primeiro de instalar e ligar os agentes para todos os computadores que pretende enviar dados para o serviço Log Analytics. Pode instalar agentes nos computadores no local utilizando a configuração, a linha de comandos, ou com o Desired State Configuration (DSC) na automatização do Azure. 

O agente para Linux e Windows comunica saído com o serviço Log Analytics, através da porta TCP 443 e, se o computador liga-se a um servidor de firewall ou proxy para comunicar através da Internet, reveja os requisitos abaixo para compreender a configuração de rede é necessário.  Se as políticas de segurança de TI não permitir que os computadores na rede para ligar à Internet, pode configurar uma [Gateway de OMS](log-analytics-oms-gateway.md) e, em seguida, configurar o agente para ligar através do gateway para o Log Analytics. O agente pode, em seguida, receber informações de configuração e enviar os dados recolhidos consoante as regras de recolha de dados e soluções ativados. 

Se estiver a monitorizar o computador com o System Center Operations Manager 2012 R2 ou posterior, pode ser multihomed com o serviço do Log Analytics para recolher dados e reencaminhar para o serviço e ainda ser monitorizadas pelo [Operations Manager](log-analytics-om-agents.md). Computadores com Linux monitorizados por um grupo de gestão do Operations Manager integrado com o Log Analytics não recebeu a configuração para origens de dados e os dados recolhidos para a frente através do grupo de gestão. O agente do Windows pode relatar até quatro áreas de trabalho, enquanto o agente Linux só suporta a geração de relatórios para um único espaço de trabalho.  

O agente para Linux e Windows não é apenas para ligar ao Log Analytics, também suporta a automatização do Azure para o host a função de trabalho de Runbook híbrida e soluções de gestão, como o controlo de alterações e gestão de atualizações.  Para obter mais informações sobre a função de trabalho de Runbook híbrida, veja [Runbook Worker híbrido do Azure Automation](../automation/automation-hybrid-runbook-worker.md).  

## <a name="supported-windows-operating-systems"></a>Sistemas de operativos do Windows
As seguintes versões do sistema operativo Windows são suportadas oficialmente para o agente do Windows:

* Windows Server 2008 Service Pack 1 (SP1) ou posterior
* Windows 7 SP1 e posterior.

## <a name="supported-linux-operating-systems"></a>Sistemas operativos Linux suportados
As seguintes distribuições de Linux são suportadas oficialmente.  No entanto, o agente Linux também pode executar em outras distribuições não listadas.  Salvo indicação em contrário, todas as versões secundárias são suportadas para cada versão principal listado.  

* Amazon Linux 2012.09 para 2015.09 (x86/x64)
* Linux centOS 5, 6 e 7 (x86/x64)  
* Oracle Linux 5, 6 e 7 (x86/x64) 
* Red Hat Enterprise Linux Server 5, 6 e 7 (x86/x64)
* Debian GNU/Linux 6, 7 e 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 e 12 (x86/x64)

## <a name="tls-12-protocol"></a>Protocolo TLS 1.2
Para garantir a segurança dos dados em trânsito para o Log Analytics, recomendamos que configure o agente para utilizar, pelo menos, Transport Layer Security (TLS) 1.2. As versões mais antigas do TLS/Secure Sockets Layer (SSL) foram encontradas vulneráveis e enquanto trabalham ainda atualmente para permitir a compatibilidade com versões anteriores, estão **não recomendada**.  Para obter mais informações, consulte [enviar dados de forma segura através de TLS 1.2](log-analytics-data-security.md#sending-data-securely-using-tls-12). 

## <a name="network-firewall-requirements"></a>Requisitos de firewall de rede
As informações abaixo lista as informações de configuração de proxy e de firewall necessárias para o agente Linux e Windows para comunicar com o Log Analytics.  

|Recursos do Agente|Portas |Direção |Inspeção de HTTPS direto|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Porta 443 |Entrada e saída|Sim |  
|*.oms.opinsights.azure.com |Porta 443 |Entrada e saída|Sim |  
|*.blob.core.windows.net |Porta 443 |Entrada e saída|Sim |  
|*.azure-automation.net |Porta 443 |Entrada e saída|Sim |  


Se planeja usar a função de trabalho de Runbook de híbrida de automatização do Azure para ligar e registar com o serviço de automatização para utilizar runbooks no seu ambiente, tem de ter acesso para o número de porta e os URLs descritos em [configurar sua rede para o Runbook Worker híbrido](../automation/automation-hybrid-runbook-worker.md#network-planning). 

O agente do Windows e Linux suporta a comunicação por meio de um servidor proxy ou Gateway do OMS para o serviço do Log Analytics utilizando o protocolo HTTPS.  Autenticação anónima e básica (nome de utilizador/palavra-passe) são suportados.  Para o agente de Windows ligado diretamente ao serviço, a configuração de proxy é especificada durante a instalação ou [após a implementação](log-analytics-agent-manage.md#update-proxy-settings) no painel de controlo ou com o PowerShell.  

Para o agente do Linux, o servidor proxy é especificado durante a instalação ou [após a instalação](log-analytics-agent-manage.md#update-proxy-settings) ao modificar o ficheiro de configuração de proxy. Conf.  O valor de configuração de proxy de agente do Linux tem a seguinte sintaxe:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Se o servidor proxy não requer que se autentique, o agente Linux ainda exige que fornecer um utilizador de pseudo-autenticação/palavra-passe. Isso pode ser qualquer nome de utilizador ou palavra-passe.

|Propriedade| Descrição |
|--------|-------------|
|Protocolo | https |
|Utilizador | Nome de utilizador opcional para a autenticação de proxy |
|palavra-passe | Palavra-passe opcional para a autenticação de proxy |
|proxyhost | Endereço ou FQDN do proxy servidor/OMS Gateway |
|porta | Número de porta opcional para o proxy server/OMS Gateway |

Por exemplo: `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Se utilizar carateres especiais, tais como "\@" a palavra-passe, receberá um erro de ligação de proxy porque o valor é analisado incorretamente.  Para contornar este problema, codificar a palavra-passe no URL usando uma ferramenta como [URLDecode](https://www.urldecoder.org/).  

## <a name="install-and-configure-agent"></a>Instalar e configurar o agente 
Ligar os computadores no local diretamente com o Log Analytics pode ser feito através de métodos diferentes, dependendo das suas necessidades. A tabela seguinte realça a cada método para determinar o que funciona melhor na sua organização.

|Origem | Método | Descrição|
|-------|-------------|-------------|
| Computador Windows|- [Instalação manual](log-analytics-agent-windows.md)<br>- [DSC de automatização do Azure](log-analytics-agent-windows.md#install-the-agent-using-dsc-in-azure-automation)<br>- [Modelo do Resource Manager com o Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) |Instalar o Microsoft Monitoring agent a partir da linha de comando ou usando um método automatizado, como o DSC de automatização do Azure, [System Center Configuration Manager](https://docs.microsoft.com/sccm/apps/deploy-use/deploy-applications), ou com um modelo Azure Resource Manager, se tiver implementado Microsoft O Azure Stack no seu datacenter.| 
|Computador com Linux| [Instalação manual](log-analytics-quick-collect-linux-computer.md)|Instale o agente para Linux chamar um script de wrapper alojado no GitHub. | 
| System Center Operations Manager|[Integrar o Operations Manager com o Log Analytics](log-analytics-om-agents.md) | Configurar a integração entre o Operations Manager e o Log Analytics para reencaminhar dados recolhidos de computadores Linux e Windows que relatam a um grupo de gestão.|  

## <a name="next-steps"></a>Passos Seguintes

* Revisão [origens de dados](log-analytics-data-sources.md) para compreender as origens de dados disponíveis para recolher dados do seu sistema Windows ou Linux. 

* Saiba mais sobre [pesquisas de registos](log-analytics-log-searches.md) para analisar os dados recolhidos a partir de origens de dados e soluções. 

* Saiba mais sobre [soluções](log-analytics-add-solutions.md) que acrescentam funcionalidades ao Log Analytics e também, recolher dados para o repositório do OMS.

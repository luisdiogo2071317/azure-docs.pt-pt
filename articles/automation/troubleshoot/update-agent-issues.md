---
title: Compreender os resultados de verificação de agente do Windows na gestão de atualizações do Azure
description: Saiba como resolver problemas com o agente de gestão de atualizações.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: automation
ms.component: update-management
manager: carmonm
ms.openlocfilehash: 33ae9160cd173ae594407220390c4dab1806fa94
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/16/2018
ms.locfileid: "51824137"
---
# <a name="understand-the-windows-agent-check-results-in-update-management"></a>Compreender os resultados de verificação de agente do Windows na gestão de atualizações

Podem existir muitos motivos pelos quais sua máquina não Azure não está a aparecer **pronto** na gestão de atualizações. Gerenciamento de atualizações, pode verificar o estado de funcionamento de um agente de função de trabalho híbrida para determinar o problema subjacente. Este artigo discute como executar a resolução de problemas do portal do Azure e em cenários offline.

## <a name="start-the-troubleshooter"></a>Iniciar a resolução de problemas

Ao clicar o **resolução de problemas** ligação sob o **preparação do agente de atualização** coluna no portal, inicia o **resolver problemas de agente de atualização** página. Esta página mostra problemas com o agente e uma ligação para este artigo para ajudá-lo a resolver problemas.

![página de lista de VM](../media/update-agent-issues/vm-list.png)

> [!NOTE]
> As verificações de exigem que a VM estar em execução. Se a VM não está em execução, verá um botão para **iniciar a VM**.

Sobre o **resolver problemas de agente de atualização** página, clique em **executar verifica**, para iniciar a resolução de problemas. Utiliza a resolução de problemas [execute o comando](../../virtual-machines/windows/run-command.md) para executar um script no computador para verificar as dependências que tenha o agente. Quando a resolução de problemas estiver concluída, ela retorna o resultado das verificações.

![Resolver problemas de página](../media/update-agent-issues/troubleshoot-page.png)

Quando terminar, os resultados são devolvidos na janela. O [verificar secções](#pre-requisistes-checks) fornecem informações sobre o que cada verificação de que está procurando.

![Página de verificações de atualização de agente](../media/update-agent-issues/update-agent-checks.png)

## <a name="prerequisite-checks"></a>Verificações de pré-requisitos

### <a name="operating-system"></a>Sistema operativo

A verificação do sistema operacional, verifica se a função de trabalho de Runbook híbrida está a executar um dos seguintes sistemas operativos:

|Sistema operativo  |Notas  |
|---------|---------|
|Versão do Windows Server 2008, Windows Server 2008 R2 RTM    | Suporta apenas avaliações de atualização.         |
|Windows Server 2008 R2 SP1 e posterior     |.NET framework 4.5.1 ou posterior é necessária. ([Baixe o .NET Framework](/dotnet/framework/install/guide-for-developers))<br/> É necessário o Windows PowerShell 4.0 ou posterior. ([Transferir WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855))<br/> Windows PowerShell 5.1 é recomendada para maior confiabilidade.  ([Transferir WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))        |

### <a name="net-451"></a>.NET 4.5.1

A verificação de framework .NET, verifica se o sistema tem um mínimo de [.NET Framework 4.5.1](https://www.microsoft.com/download/details.aspx?id=30653) presente.

### <a name="wmf-51"></a>WMF 5.1

A verificação WMF, verifica se o sistema tem a versão necessária do Windows Management Framework. [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) é a versão mais baixa suportada. Recomenda-se que instale [Windows Management Framework 5.1](https://www.microsoft.com/download/details.aspx?id=54616) para maior confiabilidade da função de trabalho de Runbook híbrida.

### <a name="tls-12"></a>TLS 1.2

Esta verificação, determina se estiver a utilizar TLS 1.2 para encriptar as comunicações. TLS 1.0 já não é suportada pela plataforma e recomenda-se que os clientes utilizam o TLS 1.2 para comunicar com a gestão de atualizações.

## <a name="connectivity-checks"></a>Verificações de conectividade

### <a name="registration-endpoint"></a>Ponto final do registo

Esta verificação determina se o agente corretamente pode comunicar com o serviço de agente.

Configurações de proxy e de firewall têm de permitir o agente de trabalho de Runbook híbrida comunicar com o ponto final do registo. Para obter uma lista de endereços e portas para abrir, consulte [planear funções de trabalho híbridas de rede](../automation-hybrid-runbook-worker.md#network-planning)

### <a name="operations-endpoint"></a>Ponto final de operações

Esta verificação determina se o agente corretamente pode comunicar com o serviço de dados de tempo de execução de tarefa.

Configurações de proxy e de firewall têm de permitir o agente de trabalho de Runbook híbrida para comunicar com o serviço de dados de tempo de execução de tarefa. Para obter uma lista de endereços e portas para abrir, consulte [planear funções de trabalho híbridas de rede](../automation-hybrid-runbook-worker.md#network-planning)

## <a name="vm-service-health-checks"></a>Verificações de estado de funcionamento do serviço VM

### <a name="monitoring-agent-service-status"></a>Monitorização do Estado do serviço de agente

Esta verificação determina se o Microsoft Monitoring Agent, `HealthService` está em execução na máquina.

Para saber mais sobre o serviço de resolução de problemas, consulte [o Microsoft Monitoring Agent não está em execução](hybrid-runbook-worker.md#mma-not-running).

Para reinstalar o agente de monitorização da Microsoft, consulte [instalar e configurar o Microsoft Monitoring Agent](/log-analytics/log-analytics-concept-hybrid.md#install-and-configure-agent)

### <a name="monitoring-agent-service-events"></a>Eventos de serviço do agente de monitorização

Esta verificação determina se houve qualquer `4502` eventos no Gestor de operações de registo na máquina nas últimas 24 horas.

Para saber mais sobre este evento, consulte a [guia de resolução de problemas](hybrid-runbook-worker.md#event-4502) para este evento.

## <a name="access-permissions-checks"></a>Verificações de permissões de acesso

### <a name="machinekeys-folder-access"></a>Acesso a pastas MachineKeys

A verificação de acesso a pastas Crypto determina se a conta do sistema Local tem acesso ao `C:\ProgramData\Microsoft\Crypto\RSA`

## <a name="troubleshoot-offline"></a>Resolução de problemas offline

Pode utilizar a resolução de problemas offline numa função de trabalho de Runbook híbridas executando o script localmente. O script [resolução de problemas-WindowsUpdateAgentRegistration](https://www.powershellgallery.com/packages/Troubleshoot-WindowsUpdateAgentRegistration) pode ser encontrado na galeria do PowerShell. Um exemplo da saída deste script é mostrado no exemplo a seguir:

```output
RuleId                      : OperatingSystemCheck
RuleGroupId                 : prerequisites
RuleName                    : Operating System
RuleGroupName               : Prerequisite Checks
RuleDescription             : The Windows Operating system must be version 6.1.7601 (Windows Server 2008 R2 SP1) or higher
CheckResult                 : Passed
CheckResultMessage          : Operating System version is supported
CheckResultMessageId        : OperatingSystemCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : DotNetFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : .Net Framework 4.5+
RuleGroupName               : Prerequisite Checks
RuleDescription             : .NET Framework version 4.5 or higher is required
CheckResult                 : Passed
CheckResultMessage          : .NET Framework version 4.5+ is found
CheckResultMessageId        : DotNetFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : WindowsManagementFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : WMF 5.1
RuleGroupName               : Prerequisite Checks
RuleDescription             : Windows Management Framework version 4.0 or higher is required (version 5.1 or higher is preferable)
CheckResult                 : Passed
CheckResultMessage          : Detected Windows Management Framework version: 5.1.17763.1
CheckResultMessageId        : WindowsManagementFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {5.1.17763.1}

RuleId                      : AutomationAgentServiceConnectivityCheck1
RuleGroupId                 : connectivity
RuleName                    : Registration endpoint
RuleGroupName               : connectivity
RuleDescription             : 
CheckResult                 : Failed
CheckResultMessage          : Unable to find Workspace registration information in registry
CheckResultMessageId        : AutomationAgentServiceConnectivityCheck1.Failed.NoRegistrationFound
CheckResultMessageArguments : {}

RuleId                      : AutomationJobRuntimeDataServiceConnectivityCheck
RuleGroupId                 : connectivity
RuleName                    : Operations endpoint
RuleGroupName               : connectivity
RuleDescription             : Proxy and firewall configuration must allow Automation Hybrid Worker agent to communicate with eus2-jobruntimedata-prod-su1.azure-automation.net
CheckResult                 : Passed
CheckResultMessage          : TCP Test for eus2-jobruntimedata-prod-su1.azure-automation.net (port 443) succeeded
CheckResultMessageId        : AutomationJobRuntimeDataServiceConnectivityCheck.Passed
CheckResultMessageArguments : {eus2-jobruntimedata-prod-su1.azure-automation.net}

RuleId                      : MonitoringAgentServiceRunningCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service status
RuleGroupName               : VM Service Health Checks
RuleDescription             : HealthService must be running on the machine
CheckResult                 : Failed
CheckResultMessage          : Microsoft Monitoring Agent service (HealthService) is not running
CheckResultMessageId        : MonitoringAgentServiceRunningCheck.Failed
CheckResultMessageArguments : {Microsoft Monitoring Agent, HealthService}

RuleId                      : MonitoringAgentServiceEventsCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service events
RuleGroupName               : VM Service Health Checks
RuleDescription             : Event Log must not have event 4502 logged in the past 24 hours
CheckResult                 : Failed
CheckResultMessage          : Microsoft Monitoring Agent service Event Log (Operations Manager) does not exist on the machine
CheckResultMessageId        : MonitoringAgentServiceEventsCheck.Failed.NoLog
CheckResultMessageArguments : {Microsoft Monitoring Agent, Operations Manager, 4502}

RuleId                      : CryptoRsaMachineKeysFolderAccessCheck
RuleGroupId                 : permissions
RuleName                    : Crypto RSA MachineKeys Folder Access
RuleGroupName               : Access Permission Checks
RuleDescription             : SYSTEM account must have WRITE and MODIFY access to 'C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys'
CheckResult                 : Passed
CheckResultMessage          : Have permissions to access C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys
CheckResultMessageId        : CryptoRsaMachineKeysFolderAccessCheck.Passed
CheckResultMessageArguments : {C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys}

RuleId                      : TlsVersionCheck
RuleGroupId                 : prerequisites
RuleName                    : TLS 1.2
RuleGroupName               : Prerequisite Checks
RuleDescription             : Client and Server connections must support TLS 1.2
CheckResult                 : Passed
CheckResultMessage          : TLS 1.2 is enabled by default on the Operating System.
CheckResultMessageId        : TlsVersionCheck.Passed.EnabledByDefault
CheckResultMessageArguments : {}
```

## <a name="next-steps"></a>Passos Seguintes

Para resolver problemas adicionais com suas funções de trabalho de Runbook híbridas, consulte [resolução de problemas - os Runbook Workers híbridos](hybrid-runbook-worker.md)

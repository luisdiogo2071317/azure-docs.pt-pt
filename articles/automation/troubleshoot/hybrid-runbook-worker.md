---
title: Resolução de problemas - trabalhadores de Runbook híbrida de automatização do Azure
description: Este artigo fornece informações de resolução de problemas do Azure Automation Hybrid Runbook Workers
services: automation
ms.service: automation
ms.component: ''
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: a95c9f1edd6983c915316f2900885a8131245860
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2018
ms.locfileid: "53437838"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>Resolver problemas relacionados com os Runbook Workers híbridos

Este artigo fornece informações sobre resolução de problemas com os Runbook Workers híbridos.

## <a name="general"></a>Geral

A função de trabalho de Runbook híbrida depende de um agente para comunicar com a sua conta de automatização para registrar a função de trabalho, receber tarefas de runbook e comunicar o estado. Para Windows, este agente é o Microsoft Monitoring Agent. Para o Linux, é o agente do OMS para Linux.

### <a name="runbook-execution-fails"></a>Cenário: Falha de execução de Runbooks

#### <a name="issue"></a>Problema

Falha de execução de Runbooks e receber o erro seguinte:

```error
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

O runbook está suspenso pouco depois tentar executá-lo três vezes. Existem condições, que podem interromper o runbook a conclusão com êxito e a mensagem de erro relacionados não inclui qualquer informação adicional que indica o porquê.

#### <a name="cause"></a>Causa

Seguem-se possíveis causas possíveis:

* Os runbooks não é possível autenticar com recursos locais

* A função de trabalho híbrida estiver atrás de uma firewall ou proxy

* Os runbooks não é possível autenticar com recursos locais

* O computador designado para executar a funcionalidade do Runbook Worker híbrido cumpre os requisitos mínimos de hardware.

#### <a name="resolution"></a>Resolução

Certifique-se de que o computador tem acesso de *. Azure-Automation.NET saída na porta 443.

Computadores que executam a função de trabalho de Runbook híbrida devem cumprir os requisitos mínimos de hardware antes ao designá-lo para alojar esta funcionalidade. Caso contrário, consoante a utilização de recursos de outros processos em segundo plano e a contenção durante podem de execução do runbook causa o computador ficar sobrecarregado e causar atrasos de tarefa de runbook ou os tempos limite.

Certifique-se o computador designado para executar a funcionalidade do Runbook Worker híbrido cumpre os requisitos mínimos de hardware. Se assim for, monitorize a utilização de CPU e memória para determinar qualquer correlação entre o desempenho de processos de trabalho de Runbook híbrida e Windows. Se existir memória ou pressão de CPU, isto pode indicar a necessidade de atualizar ou adicione processadores adicionais ou aumente a memória para o afunilamento do recurso de endereço e resolva o erro. Em alternativa, selecione um recurso de computação diferentes que pode suportar os requisitos mínimos e dimensionamento quando a carga de trabalho demandas indicarem que um aumento é necessário.

Verifique os **Microsoft SMA** registo de eventos para um evento correspondente com descrição *Win32 processo saiu com o código [4294967295]*. A causa deste erro é que ainda não tiver configurado a autenticação nos runbooks ou especificadas as credenciais Run As para o grupo de trabalho híbrida. Revisão [permissões do Runbook](../automation-hrw-run-runbooks.md#runbook-permissions) para confirmar que configurou corretamente a autenticação para os runbooks.

## <a name="linux"></a>Linux

O Runbook Worker do Linux híbrida depende do agente do OMS para Linux comunicar com a sua conta de automatização para registrar a função de trabalho, receber tarefas de runbook e comunicar o estado. Se o registo de função de trabalho falhar, aqui estão algumas causas possíveis para o erro:

### <a name="oms-agent-not-running"></a>Cenário: O agente do OMS para Linux não está em execução

Se o agente do OMS para Linux não está em execução, isso impede que o Runbook Worker do Linux híbrida comunicar com a automatização do Azure. Certifique-se de que o agente está em execução, introduzindo o seguinte comando: `ps -ef | grep python`. Deverá ver um resultado semelhante ao seguinte, os processos de python com **nxautomation** conta de utilizador. Se as soluções de gestão de atualizações ou a automatização do Azure não estiverem ativadas, nenhum dos seguintes processos está a executar.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

A lista seguinte mostra os processos que estão a utilizar uma função de trabalho do Runbook de híbrida do Linux. Eles estão todos localizados no `/var/opt/microsoft/omsagent/state/automationworker/` diretório.

* **OMS.Conf** -este processo é o processo do Gestor de trabalho, este processo é iniciado diretamente a partir de DSC.

* **Worker.Conf** -este processo é o processo de trabalho híbrida de registado automaticamente, é iniciada pelo Gestor de trabalho. Este processo é utilizado pela gestão de atualizações e é transparente ao usuário. Este processo não estiver presente, se a solução de gestão de atualizações não está ativada na máquina.

* **diy/Worker.Conf** -esse processo é o processo de trabalho híbrida faça mesmo. O processo de trabalho híbrida faça mesmo é utilizado para executar runbooks de utilizador na função de trabalho de Runbook híbrida. Apenas é diferente do autom registado o processo de trabalho híbrida em detalhes a chave que utiliza uma configuração diferente. Este processo não estiver presente se a solução de automatização do Azure não está ativada e a função de trabalho de híbrida do Linux faça mesmo não está registrada.

Se o agente do OMS para Linux não está em execução, execute o seguinte comando para iniciar o serviço: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="class-does-not-exist"></a>Cenário: A classe especificada não existe

Se vir o erro: **A classe especificada não existe....** na `/var/opt/microsoft/omsconfig/omsconfig.log` , em seguida, o agente do OMS para Linux tem de ser atualizado. Execute o seguinte comando para reinstalar o agente do OMS:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a>Windows

A função de trabalho de Runbook do Windows híbrida depende do Microsoft Monitoring Agent para comunicar com a sua conta de automatização para registrar a função de trabalho, receber tarefas de runbook e comunicar o estado. Se o registo de função de trabalho falhar, aqui estão algumas causas possíveis para o erro:

### <a name="mma-not-running"></a>Cenário: O Microsoft Monitoring Agent não está em execução

#### <a name="issue"></a>Problema

O `healthservice` serviço não está em execução na máquina de trabalho de Runbook híbrida.

#### <a name="cause"></a>Causa

Se o serviço Windows do Microsoft Monitoring Agent não está em execução, este cenário impede que a função de trabalho de Runbook híbrida comunicar com a automatização do Azure.

#### <a name="resolution"></a>Resolução

Certifique-se de que o agente está em execução, introduzindo o seguinte comando no PowerShell: `Get-Service healthservice`. Se o serviço estiver parado, introduza o seguinte comando no PowerShell para iniciar o serviço: `Start-Service healthservice`.

### <a name="event-4502"></a> Evento 4502 no registo do Operations Manager

#### <a name="issue"></a>Problema

Na **aplicativo e o Gestor de registos de serviços** registo de eventos, verá o evento 4502 e EventMessage que contém **hybridagent**com a descrição seguinte: *O certificado apresentado pelo serviço \<wsid\>. oms.opinsights.azure.com não foi emitido por uma autoridade de certificação utilizada para os serviços da Microsoft. Contacte o administrador de rede para ver se estão a executar um proxy que intercepte a comunicação TLS/SSL. O artigo KB3126513 tem informações adicionais de resolução de problemas de conectividade.*

#### <a name="cause"></a>Causa

Este problema pode dever-se a firewall, proxy ou de rede, bloquear a comunicação com o Microsoft Azure. Certifique-se de que o computador tem acesso de *. Azure-Automation.NET saída nas portas 443.

#### <a name="resolution"></a>Resolução

Os registos são armazenados localmente em cada função de trabalho híbrida em C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes. Pode verificar se existem quaisquer eventos de aviso ou erro no **aplicativos e serviços Logs\Microsoft-SMA\Operations** e **aplicativo e o Gestor de registos de serviços** registo de eventos que seria indica uma conectividade ou outro problema que afeta a inclusão da função para a automatização do Azure ou o problema estiver sob as operações normais.

[Runbook de saída e mensagens](../automation-runbook-output-and-messages.md) são enviadas para a automatização do Azure de funções de trabalho híbridas tal como tarefas de runbook, que são executados na cloud. Também pode ativar os fluxos de verboso e progresso da mesma forma que faria para outros runbooks.

### <a name="corrupt-cache"></a> Não comunicar de Runbook Worker híbrido

#### <a name="issue"></a>Problema

Está a executar o seu computador de trabalho de Runbook híbrida, mas não vir dados de heartbeat para a máquina na área de trabalho.

A consulta de exemplo seguinte mostra as máquinas numa área de trabalho e o último heartbeat:

```loganalytics
// Last heartbeat of each computer
Heartbeat 
| summarize arg_max(TimeGenerated, *) by Computer
```

#### <a name="cause"></a>Causa

Este problema pode ser causado por um cache danificado na função de trabalho de Runbook híbrida.

#### <a name="resolution"></a>Resolução

Para resolver este problema, inicie sessão para a função de trabalho de Runbook híbrida e execute o seguinte script. Este script interrompe o Microsoft Monitoring Agent, remove o seu cache e reinicia o serviço. Esta ação força a função de trabalho de Runbook híbrida transferir novamente a configuração da automatização do Azure.

```powershell
Stop-Service -Name HealthService

Remove-Item -Path 'C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State' -Recurse

Start-Service -Name HealthService
```

## <a name="next-steps"></a>Passos Seguintes

Se não vir o seu problema ou não é possível resolver o problema, visite um dos seguintes canais de suporte mais:

* Obtenha respostas de especialistas do Azure através dos [fóruns do Azure](https://azure.microsoft.com/support/forums/)
* Ligue-se a [@AzureSupport](https://twitter.com/azuresupport) – a conta oficial do Microsoft Azure para melhorar a experiência do cliente ao ligar a comunidade do Azure aos recursos certos: respostas, suporte e especialistas.
* Se precisar de mais ajuda, pode enviar um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporte**.

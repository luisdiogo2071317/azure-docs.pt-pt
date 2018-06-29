---
title: Resolução de problemas - os Runbook Workers híbridos da automatização do Azure
description: Este artigo fornece informações de resolução de problemas do Azure automatização de Runbook híbridos
services: automation
ms.service: automation
ms.component: ''
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1cae7253a4bfcb4f83baf003a4d9d3c367d8f014
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063918"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>Resolver problemas relacionados com os Runbook Workers híbridos

Este artigo fornece informações sobre como resolver problemas com os Runbook Workers híbridos.

## <a name="general"></a>Geral

O Runbook Worker híbrido depende de um agente para comunicar com a sua conta de automatização para registar o trabalho, receber tarefas de runbook e comunicar estado. Para o Windows, este agente é o Microsoft Monitoring Agent. Para Linux, é o agente do OMS para Linux.

###<a name="runbook-execution-fails"></a>Cenário: Falha de execução do Runbook

#### <a name="issue"></a>Problema

Falha de execução do Runbook e receber o erro seguinte:

```
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

O runbook está suspenso pouco tempo depois tentar executá-lo três vezes. Existem condições, o que poderão interromper o runbook a conclusão com êxito e a mensagem de erro relacionados não inclui qualquer informação adicional que indica o motivo.

#### <a name="cause"></a>Causa

Seguem-se possíveis causas possíveis:

* Os runbooks não é possível autenticar com recursos locais

* A função de trabalho híbrida estiver atrás de um proxy ou firewall

* Os runbooks não é possível autenticar com recursos locais

* O computador designado para executar a funcionalidade do Runbook Worker híbrido cumpre os requisitos mínimos de hardware.

#### <a name="resolution"></a>Resolução

Certifique-se de que o computador tem acesso de saída para *.azure automation.net na porta 443.

Computadores com o Runbook Worker híbrido devem cumprir os requisitos mínimos de hardware antes de indicar que esta funcionalidade do anfitrião. Caso contrário, consoante a utilização de recursos de outros processos em segundo plano e a contenção causado por runbooks durante a execução, o computador fica sobrecarregado e causar atrasos de tarefa de runbook ou tempos limite.

Confirme que o computador designado para executar a funcionalidade do Runbook Worker híbrido cumpre os requisitos mínimos de hardware. Se existir, monitorize a utilização de CPU e memória para determinar a qualquer correlação entre o desempenho dos processos de trabalho de Runbook híbrida e o Windows. Se existir memória ou pressão de CPU, isto pode indicar a necessidade de atualizar ou adicione processadores adicionais ou aumente a memória para resolver o congestionamento do recurso e resolva o erro. Em alternativa, selecione um recurso de computação diferentes que pode suportar os requisitos mínimos e escala, quando a carga de trabalho exigências indicam que um aumento é necessário.

Verifique o **Microsoft SMA** registo de eventos para um evento correspondente com a descrição *Win32 processo abortado com o código [4294967295]*. A causa deste erro é que ainda não configurada a autenticação nos runbooks ou especificar as credenciais Run As para o grupo de trabalho híbridas. Reveja [Runbook permissões](../automation-hrw-run-runbooks.md#runbook-permissions) para confirmar que configurou corretamente a autenticação para os runbooks.

## <a name="linux"></a>Linux

O Runbook Worker híbrido Linux depende o agente do OMS para Linux comunicar com a sua conta de automatização para registar o trabalho, receber tarefas de runbook e comunicar estado. Se o registo do worker falhar, aqui estão algumas causas possíveis para o erro:

###<a name="oms-agent-not-running"></a>Cenário: O agente do OMS para Linux não está em execução

Se o agente do OMS para Linux não está em execução, isto impede o Runbook Worker híbrido Linux ao comunicar com a automatização do Azure. Certifique-se de que o agente está em execução, introduzindo o seguinte comando: `ps -ef | grep python`. Deverá ver um resultado semelhante ao seguinte, os processos de python com **nxautomation** conta de utilizador. Se as soluções de gestão de atualizações ou de automatização do Azure não estiverem ativadas, nenhum dos seguintes processos em execução.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

A lista seguinte mostra os processos que são iniciados para um Runbook Worker híbrido do Linux. Estão todos localizados no `/var/opt/microsoft/omsagent/state/automationworker/` diretório.

* **OMS.Conf** -este é o processo do Gestor de trabalho, isto é diretamente a partir do DSC.

* **Worker.Conf** -este processo é o processo de trabalho automática registado híbrida, é iniciada pelo Gestor de trabalho. Este processo é utilizado pela gestão da atualização e é transparente para o utilizador. Este processo não estará presente se a solução de gestão de atualizações não está ativada na máquina.

* **diy/Worker.Conf** -este processo é o processo de trabalho híbrida DIY. O processo de trabalho híbrida DIY é utilizado para executar runbooks do utilizador sobre o Runbook Worker híbrido. Só é diferente do autom registado o processo de trabalho híbrida nos detalhes chave que utiliza uma configuração diferente. Este processo não está presente, se a solução de automatização do Azure não está ativada e a função de trabalho híbrida DIY Linux não está registada.

Se o agente do OMS para Linux não está em execução, execute o seguinte comando para iniciar o serviço: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

###<a name="class-does-not-exist"></a>Cenário: A classe especificada não existe

Se vir o erro: **a classe especificada não existe...** no `/var/opt/microsoft/omsconfig/omsconfig.log` , em seguida, o agente do OMS para Linux tem de ser atualizado. Execute o seguinte comando para reinstalar o agente do OMS:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a>Windows

O Runbook Worker híbrido Windows depende do Microsoft Monitoring Agent para comunicar com a sua conta de automatização para registar o trabalho, receber tarefas de runbook e comunicar estado. Se o registo do worker falhar, aqui estão algumas causas possíveis para o erro:

###<a name="mma-not-running"></a>Cenário: O Microsoft Monitoring Agent não está em execução

#### <a name="issue"></a>Problema

O `healthservice` serviço não está em execução na máquina de Runbook Worker híbrido.

#### <a name="cause"></a>Causa

Se o serviço Windows do Microsoft Monitoring Agent não está em execução, isto impede o Runbook Worker híbrido comunicar com a automatização do Azure.

#### <a name="resolution"></a>Resolução

Certifique-se de que o agente está em execução, introduzindo o seguinte comando do PowerShell: `Get-Service healthservice`. Se o serviço for parado, introduza o seguinte comando do PowerShell para iniciar o serviço: `Start-Service healthservice`.

###<a name="event-4502"></a> Evento 4502 no registo do Operations Manager

#### <a name="issue"></a>Problema

No **aplicação e o Gestor de serviços de Logs\Operations** registo de eventos, para ver eventos 4502 e EventMessage contendo **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**com a descrição seguinte: *o certificado apresentado pelo serviço \<wsid\>. oms.opinsights.azure.com não foi emitido por uma autoridade de certificação utilizada para os serviços Microsoft. Contacte o administrador de rede para ver se estiverem a executar um proxy que intercepte a comunicação TLS/SSL. O artigo KB3126513 tem informações adicionais de resolução de problemas para problemas de conectividade.*

#### <a name="cause"></a>Causa

Isto pode dever-se a firewall, proxy ou de rede, bloquear a comunicação com o Microsoft Azure. Certifique-se de que o computador tem acesso de saída para *.azure automation.net nas portas 443.

#### <a name="resolution"></a>Resolução

Os registos são armazenados localmente em cada função de trabalho híbrida no C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes. Pode verificar se existem quaisquer avisos ou eventos de erro escritos para o **aplicações e serviços Logs\Microsoft-SMA\Operations** e **aplicação e o Gestor de serviços de Logs\Operations** registo de eventos deverá indicar uma conectividade ou outro problema que afetam a integração de função da automatização do Azure ou o problema ao efetuar operações normais.

[Runbook de saída e mensagens](../automation-runbook-output-and-messages.md) são enviadas para a automatização do Azure do híbrida workers, tal como tarefas de runbook é executado na nuvem. Também pode ativar os fluxos de verboso e progresso da mesma forma que faria para outros runbooks.

## <a name="next-steps"></a>Passos Seguintes

Se não foi possível ver o seu problema ou não conseguir resolver o problema, visite uma das seguintes canais de suporte mais:

* Obtenha respostas de especialistas do Azure através dos [fóruns do Azure](https://azure.microsoft.com/support/forums/)
* Ligue-se a [@AzureSupport](https://twitter.com/azuresupport) – a conta oficial do Microsoft Azure para melhorar a experiência do cliente ao ligar a comunidade do Azure aos recursos certos: respostas, suporte e especialistas.
* Se precisar de mais ajuda, pode ficheiro um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporta**.

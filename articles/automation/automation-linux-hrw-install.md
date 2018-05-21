---
title: Função de Trabalho de Runbook Híbrida do Linux da Automatização do Azure
description: Este artigo fornece informações sobre como instalar um Azure Automation Runbook Worker híbrido que permite a execução de runbooks em computadores baseados em Linux no seu local datacenter ou o ambiente de nuvem.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: aca68b6e8d0e6b80a1504b16b9b3462f20fdc6c4
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="how-to-deploy-a-linux-hybrid-runbook-worker"></a>Como implementar um Runbook Worker híbrido do Linux

A funcionalidade de trabalho de Runbook híbrida da automatização do Azure permite-lhe executar runbooks diretamente no computador que aloja a função e relativamente aos recursos no ambiente para gerir os recursos locais. O Runbook Worker híbrido Linux executa runbooks como um utilizador especial que pode ser elevado para executar comandos que precisam de elevação. Os Runbooks são armazenados e geridos na automatização do Azure e, em seguida, entregar uma ou mais computadores designados. Este artigo decribes como instalar o Runbook Worker híbrido num computador Linux.

## <a name="supported-linux-operating-systems"></a>Sistemas operativos Linux suportados

Segue-se uma lista das distribuições de Linux que são suportados:

* Amazon Linux 2012.09--> 2015.09 (x86/x64)
* CentOS Linux 5, 6 e 7 (x86/x64)
* Oracle Linux 5, 6 e 7 (x86/x64)
* Red Hat Enterprise Linux Server 5,6 e 7 (x86/x64)
* Debian GNU/Linux 6, 7 e 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64)
* Servidor do SUSE Linux Enteprise 11 e 12 (x86/x64)

## <a name="installing-linux-hybrid-runbook-worker"></a>Instalar o Runbook Worker híbrido Linux

Para instalar e configurar um Runbook Worker híbrido no computador Linux, é seguido um processo de reencaminhar simples de instalar e configurar a função manualmente. É necessário ativar o **automatização de trabalho híbrida** solução na sua área de trabalho de análise de registos e, em seguida, executar um conjunto de comandos para registar o computador como uma função de trabalho e adicioná-lo a um grupo novo ou existente.

Seguem-se os requisitos mínimos para um Runbook Worker híbrido Linux:

* Um mínimo de dois núcleos
* Um mínimo de 4 GB de RAM
* Porta 443 (saída)

### <a name="package-requirements"></a>Requisitos do pacote

| **Pacote necessário** | **Descrição** | **Versão mínima**|
|--------------------- | --------------------- | -------------------|
|Glibc |Biblioteca de C GNU| 2.5-12 |
|OpenSSL| Bibliotecas de OpenSSL | 0.9.8E ou 1.0|
|Curl | cURL web cliente | 7.15.5|
|Python ctypes | |
|PAM | Autenticação incorporável|

Antes de continuar, tem de ter em atenção a área de trabalho de análise de registos que está ligada a conta de automatização bem como a chave primária para a sua conta de automatização. Pode encontrar no portal, selecione a sua conta de automatização e selecionar **área de trabalho** para o ID da área de trabalho e selecionar **chaves** para a chave primária. Para obter informações sobre as portas e endereços necessários para o Runbook Worker híbrido, consulte [configuração da sua rede](automation-hybrid-runbook-worker.md#network-planning).

1. Ative a solução "Worker híbrido de automatização" no Azure. Isto pode ser feito ao:

   1. Adicionar o **automatização de trabalho híbrida** solução à sua subscrição utilizando o procedimento em [soluções de gestão de análise de registos de adicionar a sua área de trabalho](../log-analytics/log-analytics-add-solutions.md).
   1. Execute o seguinte cmdlet:

        ```azurepowershell-interactive
         Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

1. Instalar o agente do OMS para Linux executando o seguinte comando, substituindo \<WorkspaceID\> e \<WorkspaceKey\> com os valores apropriados da sua área de trabalho.

   ```bash
   wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
   ```

1. Execute o seguinte comando, alterando os valores para parâmetros *-w*, *-k*, *-g*, e *-i*. Para o *-g* parâmetro, substitua o valor com o nome do grupo de trabalho de Runbook híbrida que o Runbook Worker novo do Linux híbrido deve ser associado. Se o nome não existir já na sua conta de automatização, um novo grupo de trabalho de Runbook híbrida é feito com esse nome.

   ```bash
   sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <LogAnalyticsworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
   ```

1. Após a conclusão do comando, a página de grupos de trabalho híbrida no portal do Azure irá mostrar o novo grupo e o número de membros ou se um grupo existente, o número de membros é incrementado. Pode selecionar o grupo da lista no **grupos de trabalho híbrida** página e selecione o **híbridos** mosaico. No **híbridos** página, verá cada membro do grupo listado.

## <a name="turning-off-signature-validation"></a>Desativar a validação da assinatura

Por predefinição, os Runbook Workers híbridos Linux exigir validação da assinatura. Se executar um runbook não atribuído em relação a uma função de trabalho, verá um erro que contêm "Validação da assinatura falhou". Para desativar a validação da assinatura, execute o seguinte comando, substituindo o segundo parâmetro com o seu ID da área de trabalho de análise de registos:

 ```bash
 sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <LogAnalyticsworkspaceId>
 ```

## <a name="supported-runbook-types"></a>Tipos de runbook suportados

Os Runbook Workers híbridos Linux não suportam o conjunto completo de tipos de runbook que se encontram dentro da automatização do Azure.

Os seguintes tipos de runbook funcionam numa função de trabalho híbrida Linux:

* Python 2
* PowerShell

Os seguintes tipos de runbook não funcionam numa função de trabalho híbrida Linux:

* Fluxo de trabalho do PowerShell
* Gráfico
* Fluxo de trabalho do PowerShell gráfico

## <a name="troubleshooting"></a>Resolução de problemas

O Runbook Worker híbrido Linux depende o agente do OMS para Linux comunicar com a sua conta de automatização para registar o trabalho, receber tarefas de runbook e comunicar estado. Se o registo do worker falhar, aqui estão algumas causas possíveis para o erro:

### <a name="the-oms-agent-for-linux-is-not-running"></a>O agente do OMS para Linux não está em execução

Se o agente do OMS para Linux não está em execução, isto impede o Runbook Worker híbrido Linux ao comunicar com a automatização do Azure. Certifique-se de que o agente está em execução, introduzindo o seguinte comando: `ps -ef | grep python`. Deverá ver um resultado semelhante ao seguinte, os processos de python com **nxautomation** conta de utilizador. Se as soluções de gestão de atualizações ou de automatização do Azure não estiverem ativadas, nenhum dos seguintes processos estarão em execução.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

A lista seguinte mostra os processos que são iniciados para um Runbook Worker híbrido do Linux. Estão todos localizados no `/var/opt/microsoft/omsagent/state/automationworker/` diretório.

* **OMS.Conf** -este é o processo do Gestor de trabalho, isto é diretamente a partir do DSC.

* **Worker.Conf** -este processo é o processo de trabalho automática registado híbrida, é iniciada pelo Gestor de trabalho. Este processo é utilizado pela gestão da atualização e é transparente para o utilizador. Este processo não é existir se a solução de gestão de atualizações não está ativada na máquina.

* **diy/Worker.Conf** -este processo é o processo de trabalho híbrida DIY. O processo de trabalho híbrida DIY é utilizado para executar runbooks do utilizador sobre o Runbook Worker híbrido. Só é diferente do autom registado o processo de trabalho híbrida nos detalhes chave que utiliza uma configuração diferente. Este processo não é estar presente de automatização do Azure solução não está ativada e a função de trabalho híbrida DIY Linux não esteja registado.

Se o agente do OMS para Linux não está em execução, execute o seguinte comando para iniciar o serviço: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="the-specified-class-does-not-exist"></a>A classe especificada não existe

Se vir o erro **a classe especificada não existe...** no `/var/opt/microsoft/omsconfig/omsconfig.log` , em seguida, o agente do OMS para Linux tem de ser atualizado. Execute o seguinte comando para voltar a instalar o agente do OMS:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

Para obter passos adicionais sobre como resolver problemas com a gestão de atualizações, consulte [Update Management - resolução de problemas](automation-update-management.md#troubleshooting)

## <a name="next-steps"></a>Passos Seguintes

* Reveja [executar runbooks num Runbook Worker híbrido](automation-hrw-run-runbooks.md) para saber como configurar os runbooks para automatizar processos no seu centro de dados no local ou outro ambiente de nuvem.
* Para obter instruções sobre como remover os Runbook Workers híbridos, consulte [remover Azure automatização de Runbook híbridos](automation-hybrid-runbook-worker.md#removing-hybrid-runbook-worker)

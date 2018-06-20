---
title: Função de Trabalho de Runbook Híbrida do Linux da Automatização do Azure
description: Este artigo fornece informações sobre como instalar um Runbook Worker híbrido de automatização do Azure, pelo que pode executar os runbooks em computadores baseados em Linux no seu local datacenter ou o ambiente de nuvem.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8507cf99ea22b24aa3026565cb7c4139e4c3742d
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268123"
---
# <a name="deploy-a-linux-hybrid-runbook-worker"></a>Implementar um Runbook Worker híbrido do Linux

Pode utilizar a funcionalidade de trabalho de Runbook híbrida da automatização do Azure para executar runbooks diretamente no computador que aloja a função e relativamente aos recursos no ambiente para gerir os recursos locais. O Runbook Worker híbrido Linux executa runbooks como um utilizador especial que pode ser elevado para executar comandos que precisam de elevação. Os Runbooks são armazenados e geridos na automatização do Azure e, em seguida, entregar uma ou mais computadores designados.

Este artigo descreve como instalar o Runbook Worker híbrido num computador Linux.

## <a name="supported-linux-operating-systems"></a>Sistemas operativos Linux suportados

A funcionalidade do Runbook Worker híbrido suporta as distribuições seguintes:

* Linux Amazon 2012.09 para 2015.09 (x86/x64)
* CentOS Linux 5, 6 e 7 (x86/x64)
* Oracle Linux 5, 6 e 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 e 7 (x86/x64)
* Debian GNU/Linux 6, 7 e 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS e 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 e 12 (x86/x64)

## <a name="installing-a-linux-hybrid-runbook-worker"></a>Instalar um Runbook Worker híbrido do Linux

Para instalar e configurar um Runbook Worker híbrido no computador Linux, pode seguir um processo simples para instalar e configurar a função manualmente. É necessário ativar o **de trabalho híbrida da automatização** solução na sua área de trabalho do Log Analytics do Azure e, em seguida, executar um conjunto de comandos para registar o computador como uma função de trabalho e adicioná-lo a um grupo.

Os requisitos mínimos para um Runbook Worker híbrido Linux são:

* Dois núcleos
* 4 GB de RAM
* Porta 443 (saída)

### <a name="package-requirements"></a>Requisitos do pacote

| **Pacote necessário** | **Descrição** | **Versão mínima**|
|--------------------- | --------------------- | -------------------|
|Glibc |Biblioteca de C GNU| 2.5-12 |
|OpenSSL| Bibliotecas de OpenSSL | 0.9.8E ou 1.0|
|Curl | cURL web cliente | 7.15.5|
|Python ctypes | |
|PAM | Autenticação incorporável|
| **Pacote opcional** | **Descrição** | **Versão mínima**|
| PowerShell Core | Para executar runbooks do PowerShell, do PowerShell é necessário instalar, consulte [instalar o PowerShell Core no Linux](/powershell/scripting/setup/installing-powershell-core-on-linux) para aprender a instalá-lo.  | 6.0.0 |

### <a name="installation"></a>Instalação

Antes de continuar, tenha em atenção a área de trabalho de análise de registos que está ligada a conta de automatização. Tenha também em atenção a chave primária para a sua conta de automatização. Pode encontrar ambas no portal do Azure ao selecionar a automatização da conta, selecionar **área de trabalho** para o ID da área de trabalho e selecionar **chaves** para a chave primária. Para obter informações sobre as portas e endereços que necessita para o Runbook Worker híbrido, consulte [configuração da sua rede](automation-hybrid-runbook-worker.md#network-planning).

1. Ativar o **automatização de trabalho híbrida** solução no Azure utilizando um dos seguintes métodos:

   * Adicionar o **automatização de trabalho híbrida** solução à sua subscrição, utilizando o procedimento em [soluções de gestão de análise de registos de adicionar a sua área de trabalho](../log-analytics/log-analytics-add-solutions.md).
   * Execute o seguinte cmdlet:

        ```azurepowershell-interactive
         Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

1. Instale o agente do OMS para Linux ao executar o comando seguinte. Substitua \<WorkspaceID\> e \<WorkspaceKey\> com os valores apropriados da sua área de trabalho.

   ```bash
   wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
   ```

1. Execute o seguinte comando, alterando os valores para os parâmetros *-w*, *-k*, *-g*, e *-i*. Para o *-g* parâmetro, substitua o valor com o nome do grupo de trabalho de Runbook híbrida que o Runbook Worker novo do Linux híbrido deve ser associado. Se o nome não existir na sua conta de automatização, um novo grupo de trabalho de Runbook híbrida é feito com esse nome.

   ```bash
   sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <LogAnalyticsworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
   ```

1. Depois de concluir o comando, o **grupos de trabalho híbrida** página no portal do Azure que mostra o novo grupo e o número de membros. Se se tratar de um grupo existente, o número de membros é incrementado. Pode selecionar o grupo da lista no **grupos de trabalho híbrida** página e selecione o **híbridos** mosaico. No **híbridos** página, verá cada membro do grupo listado.

## <a name="turning-off-signature-validation"></a>Desativar a validação da assinatura

Por predefinição, os Runbook Workers híbridos Linux exigir validação da assinatura. Se executar um runbook não atribuído em relação a uma função de trabalho, verá um erro que indica "a validação da assinatura falhou." Para desativar a validação da assinatura, execute o seguinte comando. Substitua o segundo parâmetro com o ID de área de trabalho de análise de registos.

 ```bash
 sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <LogAnalyticsworkspaceId>
 ```

## <a name="supported-runbook-types"></a>Tipos de runbook suportados

Os Runbook Workers híbridos Linux não suporta o conjunto completo de tipos de runbook da automatização do Azure.

Os seguintes tipos de runbook funcionam numa função de trabalho híbrida Linux:

* Python 2
* PowerShell

  > [!NOTE]
  > Os runbooks do PowerShell necessitam de núcleo de PowerShell para ser instalado no computador Linux. Consulte [instalar o PowerShell Core no Linux](/powershell/scripting/setup/installing-powershell-core-on-linux) para aprender a instalá-lo.

Os seguintes tipos de runbook não funcionam numa função de trabalho híbrida Linux:

* Fluxo de trabalho do PowerShell
* Gráfico
* Fluxo de trabalho do PowerShell gráfico

## <a name="troubleshooting"></a>Resolução de problemas

O Runbook Worker híbrido Linux depende o agente do OMS para Linux comunicar com a sua conta de automatização para registar o trabalho, receber tarefas de runbook e comunicar estado. Se o registo do worker falhar, aqui estão algumas causas possíveis para o erro.

### <a name="the-oms-agent-for-linux-isnt-running"></a>O agente do OMS para Linux não está em execução

Se o agente do OMS para Linux não está em execução, o Runbook Worker híbrido do Linux não consegue comunicar com a automatização do Azure. Certifique-se de que o agente está em execução, introduzindo o comando `ps -ef | grep python`. 

Deverá ver um resultado semelhante ao seguinte (processa o Python com o **nxautomation** conta de utilizador). Se a solução de gestão de atualizações ou de automatização do Azure não estiver ativada, nenhum dos seguintes processos estarão em execução.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

Os seguintes processos são iniciados para um Runbook Worker híbrido do Linux. Todos os estão localizados no `/var/opt/microsoft/omsagent/state/automationworker/` diretório.

* **OMS.Conf**: Este é o processo do Gestor de trabalho. É iniciada diretamente do pretendido Estado Configuration (DSC).

* **Worker.Conf**: Este é o processo de trabalho híbrida de registado automaticamente. É iniciada pelo Gestor de trabalho. Este processo é utilizado pela gestão da atualização e é transparente para o utilizador. Este processo está presente apenas se a solução de gestão de atualizações está ativada na máquina.

* **diy/Worker.Conf**: Este é o processo de trabalho híbrida DIY. O processo de trabalho híbrida DIY é utilizado para executar runbooks do utilizador sobre o Runbook Worker híbrido. Difere do processo de trabalho híbrida de registado automaticamente apenas em que utiliza uma configuração diferente. Este processo está presente apenas se a solução de automatização do Azure está ativada e a função de trabalho híbrida DIY Linux está registada.

Se o agente do OMS para Linux não está em execução, execute o seguinte comando para iniciar o serviço: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="the-specified-class-doesnt-exist"></a>A classe especificada não existe

Se vir o erro "a classe especificada não existe" `/var/opt/microsoft/omsconfig/omsconfig.log`, o agente do OMS para Linux tem de ser atualizado. Execute o seguinte comando para reinstalar o agente do OMS:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

Para obter passos adicionais sobre como resolver problemas com a gestão de atualizações, consulte [gestão de atualizações: Resolução de problemas](automation-update-management.md#troubleshooting).

## <a name="next-steps"></a>Passos Seguintes

* Para saber como configurar os runbooks para automatizar processos no seu centro de dados no local ou outro ambiente de nuvem, consulte [executar runbooks num Runbook Worker híbrido](automation-hrw-run-runbooks.md).
* Para obter instruções sobre como remover os Runbook Workers híbridos, consulte [remover Azure automatização de Runbook híbridos](automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker).

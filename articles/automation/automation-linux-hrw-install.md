---
title: Função de Trabalho de Runbook Híbrida do Linux da Automatização do Azure
description: Este artigo fornece informações sobre como instalar um Azure Automation Runbook Worker híbrido que permite a execução de runbooks em computadores baseados em Linux no seu local datacenter ou o ambiente de nuvem.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 95f34e5d4fd966c41a30cc68c005237ae5405592
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
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

## <a name="next-steps"></a>Passos Seguintes

* Reveja [executar runbooks num Runbook Worker híbrido](automation-hrw-run-runbooks.md) para saber como configurar os runbooks para automatizar processos no seu centro de dados no local ou outro ambiente de nuvem.
* Para obter instruções sobre como remover os Runbook Workers híbridos, consulte [remover Azure automatização de Runbook híbridos](automation-hybrid-runbook-worker.md#removing-hybrid-runbook-worker)

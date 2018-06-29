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
ms.openlocfilehash: d37dbb85dc85ee8bae0447f18f771dc658de18e3
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060243"
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
|curl | cURL web cliente | 7.15.5|
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

## <a name="troubleshoot"></a>Resolução de problemas

Para saber como resolver os Runbook Workers híbridos, consulte [resolução de problemas Linux os Runbook Workers híbridos](troubleshoot/hybrid-runbook-worker.md#linux)

## <a name="next-steps"></a>Passos Seguintes

* Para saber como configurar os runbooks para automatizar processos no seu centro de dados no local ou outro ambiente de nuvem, consulte [executar runbooks num Runbook Worker híbrido](automation-hrw-run-runbooks.md).
* Para obter instruções sobre como remover os Runbook Workers híbridos, consulte [remover Azure automatização de Runbook híbridos](automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker).

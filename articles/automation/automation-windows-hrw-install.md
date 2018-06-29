---
title: Função de Trabalho de Runbook Híbrida do Windows da Automatização do Azure
description: Este artigo fornece informações sobre como instalar um Azure Automation Runbook Worker híbrido que pode utilizar para executar runbooks em computadores baseados em Windows no seu local datacenter ou o ambiente de nuvem.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e449e6e457c4fa568b5a4de5823014b4dcea82d0
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063953"
---
# <a name="deploy-a-windows-hybrid-runbook-worker"></a>Implementar um Runbook Worker híbrido do Windows

Pode utilizar a funcionalidade de trabalho de Runbook híbrida da automatização do Azure para executar runbooks diretamente no computador que aloja a função e relativamente aos recursos no ambiente para gerir os recursos locais. Os Runbooks são armazenados e geridos na automatização do Azure e, em seguida, entregar uma ou mais computadores designados. Este artigo descreve como instalar o Runbook Worker híbrido num computador Windows.

## <a name="installing-the-windows-hybrid-runbook-worker"></a>Instalar o Runbook Worker híbrido do Windows

Para instalar e configurar um Runbook Worker híbrido do Windows, pode utilizar dois métodos. O método recomendado é utilizar um runbook da automatização para automatizar completamente o processo de configuração de um computador com o Windows. O segundo método é a seguir um procedimento passo a passo para instalar e configurar a função manualmente.

> [!NOTE]
> Para gerir a configuração dos seus servidores que suportam a função de trabalho de Runbook híbrida com configuração de estado pretendido (DSC), terá de adicioná-los como nós de DSC.

Os requisitos mínimos para um Runbook Worker híbrido do Windows são:

* Windows Server 2012 ou posterior.
* Windows PowerShell 4.0 ou posterior ([transferir WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855)). Recomendamos que o Windows PowerShell 5.1 ([transferir WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616)) para uma maior fiabilidade.
* .NET framework 4.6.2 ou posterior.
* Dois núcleos.
* 4 GB de RAM.
* Saída da porta 443 ().

Para obter mais requisitos de rede para o Runbook Worker híbrido, consulte [configuração da sua rede](automation-hybrid-runbook-worker.md#network-planning).

Para obter mais informações sobre servidores de integração para gestão com o DSC, consulte [máquinas de integração para gestão pelo Azure Automation DSC](automation-dsc-onboarding.md).
Se ativar o [solução de gestão de atualizações](../operations-management-suite/oms-solution-update-management.md), qualquer computador com Windows que está ligado à sua área de trabalho do Log Analytics do Azure é automaticamente configurado como um Runbook Worker híbrido para suportar runbooks incluídos nesta solução. No entanto, não está registado com quaisquer grupos de trabalho híbrida já definidos na sua conta de automatização. 

O computador pode ser adicionado a um grupo de trabalho de Runbook híbrida na sua conta de automatização para suportar os runbooks de automatização, desde que estiver a utilizar a mesma conta para a solução e a associação ao grupo de trabalho de Runbook híbrida. Esta funcionalidade foi adicionada à versão 7.2.12024.0 da Função de Trabalho de Runbook Híbrida.

Depois de implementar com êxito um runbook worker, reveja [executar runbooks num Runbook Worker híbrido](automation-hrw-run-runbooks.md) para saber como configurar os runbooks para automatizar processos no seu centro de dados no local ou outro ambiente de nuvem.

### <a name="automated-deployment"></a>Implementação automatizada

Execute os seguintes passos para automatizar a instalação e configuração da função de trabalho do Windows híbrida:

1. Transferir o script New-OnPremiseHybridWorker.ps1 o [galeria do PowerShell](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker/DisplayScript) diretamente a partir do computador que executa a função Runbook Worker híbrido ou de outro computador no seu ambiente. Copie o script para o trabalho.

   O script New-OnPremiseHybridWorker.ps1 requer os seguintes parâmetros durante a execução:

   * *AutomationAccountName* (obrigatório): O nome da sua conta de automatização.
   * *AAResourceGroupName* (obrigatório): O nome do grupo de recursos que está associada a sua conta de automatização.
   * *OMSResourceGroupName* (opcional): O nome do grupo de recursos para a área de trabalho do Operations Management Suite. Se este grupo de recursos não for especificado, *AAResourceGroupName* é utilizado.
   * *HybridGroupName* (obrigatório): O nome de um grupo de trabalho de Runbook híbrida que especificar como um destino para os runbooks que suportam este cenário.
   * *SubscriptionID* (obrigatório): o ID de subscrição do Azure que consta a sua conta de automatização.
   * *WorkspaceName* (opcional): nome da área de trabalho de análise de registos a. Se não tiver uma área de trabalho de análise de registos, o script cria e configura um.

     > [!NOTE]
     > Atualmente, as regiões de automatização apenas suportadas para integração com a análise de registos são **Sudeste da Austrália**, **EUA Leste 2**, **Sudeste asiático**, e  **Europa Ocidental**. Se a sua conta de automatização não está dessas regiões, o script cria uma área de trabalho de análise de registos, mas avisa-o de que este não pode associá-las em conjunto.

1. No seu computador, abra **do Windows PowerShell** do **iniciar** ecrã no modo de administrador.
1. A partir da shell da linha de comandos do PowerShell, procure a pasta que contém o script que transferiu. Alterar os valores para os parâmetros *- AutomationAccountName*, *- AAResourceGroupName*, *- OMSResourceGroupName*, *- HybridGroupName*, *- SubscriptionId*, e *- WorkspaceName*. Em seguida, execute o script.

     > [!NOTE]
     > É-lhe pedido que autenticar com o Azure, depois de executar o script. *Tem* iniciar sessão com uma conta que seja um membro da função de administradores da subscrição e o coadministrador da subscrição.

   ```powershell-interactive
   .\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> -AAResourceGroupName <NameofResourceGroup>`
   -OMSResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
   -SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfLogAnalyticsWorkspace>
   ```

1. É-lhe pedido que aceita instalar NuGet e está a ser solicitado para autenticar com as suas credenciais do Azure.

1. Depois do script estiver concluído, o **grupos de trabalho híbrida** página mostra o novo grupo e o número de membros. Se for um grupo existente, o número de membros é incrementado. Pode selecionar o grupo da lista no **grupos de trabalho híbrida** página e selecione o **híbridos** mosaico. No **híbridos** página, verá cada membro do grupo listado.

### <a name="manual-deployment"></a>Implementação manual

Efetua os primeiros dois passos uma vez para o seu ambiente de automatização e, em seguida, repita os passos restantes para cada computador de trabalho.

#### <a name="1-create-a-log-analytics-workspace"></a>1. Criar uma área de trabalho de análise de registos

Se ainda não tiver uma área de trabalho de análise de registos, crie uma utilizando as instruções apresentadas em [gerir a sua área de trabalho](../log-analytics/log-analytics-manage-access.md). Pode utilizar uma área de trabalho existente se já tiver uma.

#### <a name="2-add-the-automation-solution-to-the-log-analytics-workspace"></a>2. Adicionar a solução de automatização para a área de trabalho de análise de registos

As soluções acrescentam funcionalidades ao Log Analytics. A solução de automatização adiciona funcionalidades de automatização do Azure, incluindo suporte para o Runbook Worker híbrido. Quando adiciona a solução para a sua área de trabalho, este empurra automaticamente componentes de trabalho para o computador de agente será instalado no próximo passo.

Para adicionar o **automatização** solução para a sua área de trabalho de análise de registos, siga as instruções apresentadas em [para adicionar uma solução utilizando a Galeria soluções](../log-analytics/log-analytics-add-solutions.md).

#### <a name="3-install-the-microsoft-monitoring-agent"></a>3. Instalar o Microsoft Monitoring Agent

O Microsoft Monitoring Agent liga-se os computadores para análise de registos. Quando instalar o agente no computador local e ligá-lo à sua área de trabalho, transfere automaticamente os componentes que são necessários para o Runbook Worker híbrido.

Para instalar o agente no computador local, siga as instruções apresentadas em [computadores Windows ligar ao Log Analytics](../log-analytics/log-analytics-windows-agent.md). Pode repetir este processo para vários computadores adicionar vários workers ao seu ambiente.

Quando o agente foi ligado com êxito à análise de registos, estão listadas no **origens ligadas** separador da análise de registos **definições** página. Pode verificar se o agente corretamente transferiu a solução de automatização quando tem uma pasta denominada **AzureAutomationFiles** em C:\Program Files\Microsoft monitorização Agent\Agent. Para confirmar a versão do Runbook Worker híbrido, pode navegar para C:\Program Files\Microsoft monitorização Agent\Agent\AzureAutomation\ e tenha em atenção o \\ *versão* subpasta.

#### <a name="4-install-the-runbook-environment-and-connect-to-azure-automation"></a>4. Instalar o ambiente de runbook e ligue à automatização do Azure

Quando adiciona um agente à análise de registos, a solução de automatização empurra para baixo a **HybridRegistration** módulo do PowerShell, que contém o **Add-HybridRunbookWorker** cmdlet. Utilize este cmdlet para instalar o ambiente de runbook no computador e registá-lo com a automatização do Azure.

Abra uma sessão do PowerShell no modo de administrador e execute os seguintes comandos para importar o módulo de:

```powershell-interactive
cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
Import-Module HybridRegistration.psd1
```

Em seguida, execute o **Add-HybridRunbookWorker** cmdlet utilizando a seguinte sintaxe:

```powershell-interactive
Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>
```

Pode obter as informações necessárias para este cmdlet a partir de **gerir chaves** página no portal do Azure. Abra esta página selecionando o **chaves** opção do **definições** página na sua conta de automatização.

![Página "Gerir chaves"](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* **GroupName** é o nome do grupo de trabalho de Runbook híbrida. Se este grupo já existe na conta de automatização, o computador atual é adicionado ao mesmo. Se este grupo não existir, é adicionada.
* **Ponto final** é o **URL** entrada no **gerir chaves** página.
* **Token** é o **chave de acesso primária** entrada no **gerir chaves** página.

Para receber as informações detalhadas sobre a instalação, utilize o **-Verbose** mudar com **Add-HybridRunbookWorker**.

#### <a name="5-install-powershell-modules"></a>5. Instalar módulos do PowerShell

Os Runbooks, pode utilizar qualquer um dos cmdlets definidos em módulos instalados no seu ambiente de automatização do Azure e atividades. Estes módulos não são implementados automaticamente em computadores no local, como tal, deve instalar manualmente. A exceção é o módulo do Azure, o que é instalado por predefinição e fornece acesso a cmdlets para todos os serviços do Azure e actividades de automatização do Azure.

Porque é o objetivo principal da funcionalidade do Runbook Worker híbrido para gerir recursos locais, provavelmente terá de instalar os módulos que suportam estes recursos. Para obter informações sobre como instalar os módulos do Windows PowerShell, consulte [instalar módulos](http://msdn.microsoft.com/library/dd878350.aspx). 

Módulos que são instalados tem de estar numa localização referenciada pelo **PSModulePath** variável de ambiente, para que o worker híbrido automaticamente pode importá-los. Para obter mais informações, consulte [modificar o caminho de instalação PSModulePath](https://msdn.microsoft.com/library/dd878326%28v=vs.85%29.aspx).

## <a name="troubleshoot"></a>Resolução de problemas

Para saber como resolver os Runbook Workers híbridos, consulte [resolução de problemas Windows os Runbook Workers híbridos](troubleshoot/hybrid-runbook-worker.md#windows)

Para obter passos adicionais sobre como resolver problemas com a gestão de atualizações, consulte [gestão de atualizações: Resolução de problemas](troubleshoot/update-management.md).

## <a name="next-steps"></a>Passos Seguintes

* Para saber como configurar os runbooks para automatizar processos no seu centro de dados no local ou outro ambiente de nuvem, consulte [executar runbooks num Runbook Worker híbrido](automation-hrw-run-runbooks.md).
* Para obter instruções sobre como remover os Runbook Workers híbridos, consulte [remover Azure automatização de Runbook híbridos](automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker).
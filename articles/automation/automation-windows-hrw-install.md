---
title: Função de Trabalho de Runbook Híbrida do Windows da Automatização do Azure
description: Este artigo fornece informações sobre como instalar um Azure Automation Runbook Worker híbrido que permite a execução de runbooks em computadores baseados em Windows no seu local datacenter ou o ambiente de nuvem.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: e63e4afb5c60f193d46e30ab884d72912a6a5054
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
---
# <a name="how-to-deploy-a-windows-hybrid-runbook-worker"></a>Como implementar um Runbook Worker híbrido do Windows

A funcionalidade de trabalho de Runbook híbrida da automatização do Azure permite-lhe executar runbooks diretamente no computador que aloja a função e relativamente aos recursos no ambiente para gerir os recursos locais. Os Runbooks são armazenados e geridos na automatização do Azure e, em seguida, entregar uma ou mais computadores designados. Este artigo decribes como instalar o Runbook Worker híbrido num computador Windows.

## <a name="installing-the-windows-hybrid-runbook-worker"></a>Instalar o Runbook Worker híbrido do Windows

Para instalar e configurar um Runbook Worker híbrido do Windows, existem dois métodos disponíveis. O método recomendado é utilizar um runbook da automatização para automatizar completamente o processo necessário para configurar um computador Windows. O segundo método é a seguir um procedimento passo a passo para instalar e configurar a função manualmente.

> [!NOTE]
> Para gerir a configuração dos seus servidores que suportam a função de trabalho de Runbook híbrida com configuração de estado pretendido (DSC), terá de adicioná-los como nós de DSC.

Seguem-se os requisitos mínimos para um Runbook Worker híbrido do Windows.

* Windows Server 2012 ou posterior.
* É necessário o Windows PowerShell 4.0 ou superior ([transferir o WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855)). 5.1 do Windows PowerShell ([transferir o WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616)) é recomendada para uma maior fiabilidade.
* .NET framework 4.6.2 ou posterior.
* Um mínimo de dois núcleos.
* Um mínimo de 4 GB de RAM.
* Porta 443 (saída)

Para ver os requisitos de rede adicionais para o Runbook Worker híbrido, consulte [configuração da sua rede](automation-hybrid-runbook-worker.md#network-planning).

Para obter mais informações sobre a integração-los para a gestão com o DSC, consulte [máquinas de integração para gestão pelo Azure Automation DSC](automation-dsc-onboarding.md).
Se ativar o [solução de gestão de atualizações](../operations-management-suite/oms-solution-update-management.md), qualquer computador com o Windows à sua área de trabalho de análise de registos é automaticamente configurado como um Runbook Worker híbrido para suportar runbooks incluídos nesta solução. No entanto, não está registado em todos os grupos de trabalho híbrida já definidos na sua conta de automatização. O computador pode ser adicionado a um grupo de trabalho de Runbook híbrida na sua conta de automatização para suportar os runbooks de automatização, desde que estiver a utilizar a mesma conta para a solução e a associação ao grupo de trabalho de Runbook híbrida. Esta funcionalidade foi adicionada à versão 7.2.12024.0 da Função de Trabalho de Runbook Híbrida.

Depois de ter implementado com êxito um runbook worker, reveja [executar runbooks num Runbook Worker híbrido](automation-hrw-run-runbooks.md) para saber como configurar os runbooks para automatizar processos no seu centro de dados no local ou outro ambiente de nuvem.

### <a name="automated-deployment"></a>Implementação automatizada

Execute os seguintes passos para automatizar a instalação e configuração da função de trabalho do Windows híbrida.

1. Transferir o *New-OnPremiseHybridWorker.ps1* script do [galeria do PowerShell](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker/DisplayScript) diretamente a partir do computador que executa a função Runbook Worker híbrido ou de outro computador no seu ambiente e Copie-a de trabalho.

   O *New-OnPremiseHybridWorker.ps1* script requer os seguintes parâmetros durante a execução:

   * *AutomationAccountName* (obrigatório) - o nome da sua conta de automatização.
   * *AAResourceGroupName* (obrigatório) - o nome do grupo de recursos associados a sua conta de automatização
   * *OMSResourceGroupName* (opcional) - o nome do grupo de recursos para a área de trabalho do OMS. Se não for especificado, é utilizado o AAResourceGroupName.
   * *HybridGroupName* (obrigatório) - o nome de um grupo de trabalho de Runbook híbrida que especificar como um destino para os runbooks que suportam este cenário.
   * *SubscriptionID* (obrigatório) - o ID de subscrição do Azure que consta a sua conta de automatização.
   * *WorkspaceName* (opcional) - nome de área de trabalho de análise de registos a. Se não tiver uma área de trabalho de análise de registos, o script cria e configura um.

     > [!NOTE]
     > Atualmente as regiões de automatização apenas suportadas para integração com a análise de registos são - **Sudeste da Austrália**, **EUA Leste 2**, **Sudeste asiático**, e  **Europa Ocidental**. Se a sua conta de automatização não está dessas regiões, o script cria uma área de trabalho de análise de registos, mas avisa-o que este não pode associá-las em conjunto.

1. No seu computador, inicie **do Windows PowerShell** do **iniciar** ecrã no modo de administrador.
1. A shell de linha de comandos do PowerShell, navegue para a pasta que contém o script transferido e executá-lo alterando os valores para parâmetros *- AutomationAccountName*, *- AAResourceGroupName*, *- OMSResourceGroupName*, *- HybridGroupName*, *- SubscriptionId*, e *- WorkspaceName*.

     > [!NOTE]
     > Lhe for pedido para autenticar com o Azure, depois de executar o script. **Tem** iniciar sessão com uma conta que seja um membro da função de administradores da subscrição e o coadministrador da subscrição.

   ```powershell-interactive
   .\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> -AAResourceGroupName <NameofResourceGroup>`
   -OMSResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
   -SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfLogAnalyticsWorkspace>
   ```

1. Lhe for pedido para aceitar instalar **NuGet** e lhe for pedido para autenticar com as suas credenciais do Azure.

1. Depois do script estiver concluído, a página de grupos de trabalho híbrida irá mostrar o novo grupo e o número de membros ou se um grupo existente, o número de membros é incrementado. Pode selecionar o grupo da lista no **grupos de trabalho híbrida** página e selecione o **híbridos** mosaico. No **híbridos** página, verá cada membro do grupo listado.

### <a name="manual-deployment"></a>Implementação manual

Execute os primeiros dois passos uma vez para o seu ambiente de automatização e, em seguida, repita os passos restantes para cada computador de trabalho.

#### <a name="1-create-log-analytics-workspace"></a>1. Criar uma área de trabalho do Log Analytics

Se ainda não tiver uma área de trabalho de análise de registos, em seguida, crie uma utilizando as instruções apresentadas em [gerir a sua área de trabalho](../log-analytics/log-analytics-manage-access.md). Pode utilizar uma área de trabalho existente se já tiver uma.

#### <a name="2-add-automation-solution-to-log-analytics-workspace"></a>2. Adicionar a solução de automatização à área de trabalho de análise de registos

As soluções acrescentam funcionalidades ao Log Analytics. A solução de automatização adiciona funcionalidades de automatização do Azure, incluindo suporte para o Runbook Worker híbrido. Quando adiciona a solução para a sua área de trabalho, este automaticamente empurra para baixo componentes de trabalho para o computador de agente que pretende instalar no próximo passo.

Siga as instruções apresentadas em [para adicionar uma solução utilizando a Galeria soluções](../log-analytics/log-analytics-add-solutions.md) para adicionar o **automatização** solução para a sua área de trabalho de análise de registos.

#### <a name="3-install-the-microsoft-monitoring-agent"></a>3. Instalar o Microsoft Monitoring Agent

O Microsoft Monitoring Agent liga-se os computadores para análise de registos. Quando instalar o agente no computador local e ligá-lo à sua área de trabalho, irá transferir automaticamente os componentes necessários para o Runbook Worker híbrido.

Siga as instruções apresentadas em [computadores Windows ligar ao Log Analytics](../log-analytics/log-analytics-windows-agent.md) para instalar o agente no computador local. Pode repetir este processo para vários computadores adicionar vários workers ao seu ambiente.

Quando o agente foi ligado com êxito à análise de registos, estão listadas no **origens ligadas** separador da análise de registos **definições** página. Pode verificar se o agente corretamente transferiu a solução de automatização quando tem uma pasta denominada **AzureAutomationFiles** em C:\Program Files\Microsoft monitorização Agent\Agent. Para confirmar a versão do Runbook Worker híbrido, pode navegar para C:\Program Files\Microsoft monitorização Agent\Agent\AzureAutomation\ e tenha em atenção o \\ *versão* subpasta.

#### <a name="4-install-the-runbook-environment-and-connect-to-azure-automation"></a>4. Instalar o ambiente de runbook e ligue à automatização do Azure

Quando adiciona um agente à análise de registos, a solução de automatização empurra para baixo a **HybridRegistration** módulo do PowerShell, que contém o **Add-HybridRunbookWorker** cmdlet. Utilize este cmdlet para instalar o ambiente de runbook no computador e registá-lo com a automatização do Azure.

Abra uma sessão do PowerShell no modo de administrador e execute os seguintes comandos para importar o módulo.

```powershell-interactive
cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
Import-Module HybridRegistration.psd1
```

Em seguida, execute o **Add-HybridRunbookWorker** cmdlet utilizando a seguinte sintaxe:

```powershell-interactive
Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>
```

Pode obter as informações necessárias para este cmdlet a partir de **gerir chaves** página no portal do Azure. Abra esta página selecionando o **chaves** opção do **definições** página na sua conta de automatização.

![Descrição geral de trabalho de Runbook híbrida](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* **GroupName** é o nome do grupo de trabalho de Runbook híbrida. Se este grupo já existe na conta de automatização, o computador atual é adicionado ao mesmo. Se ainda não existir, em seguida, é adicionado.
* **Ponto final** é o **URL** campo no **gerir chaves** página.
* **Token** é o **chave de acesso primária** no **gerir chaves** página.

Utilize o **-Verbose** mudar com **Add-HybridRunbookWorker** receber informações detalhadas sobre a instalação.

#### <a name="5-install-powershell-modules"></a>5. Instalar módulos do PowerShell

Os Runbooks, pode utilizar qualquer um dos cmdlets definidos em módulos instalados no seu ambiente de automatização do Azure e atividades. Estes módulos não são implementados automaticamente em computadores no local, como tal, deve instalar manualmente. A exceção é o módulo do Azure, o que é instalado por predefinição, fornecer acesso a cmdlets para todos os serviços do Azure e actividades de automatização do Azure.

Uma vez que o objetivo principal da funcionalidade do Runbook Worker híbrido gerir recursos locais, provavelmente terá de instalar os módulos que suportam estes recursos. Pode fazer referência a [instalar módulos](http://msdn.microsoft.com/library/dd878350.aspx) para obter informações sobre como instalar os módulos do Windows PowerShell. Tem de ser módulos instalados numa localização referenciada pela variável de ambiente de PSModulePath, de modo a que são automaticamente importados pelo worker híbrido. Para obter mais informações, consulte [modificar o caminho de instalação PSModulePath](https://msdn.microsoft.com/library/dd878326%28v=vs.85%29.aspx).

## <a name="next-steps"></a>Próximos Passos

* Reveja [executar runbooks num Runbook Worker híbrido](automation-hrw-run-runbooks.md) para saber como configurar os runbooks para automatizar processos no seu centro de dados no local ou outro ambiente de nuvem.
* Para obter instruções sobre como remover os Runbook Workers híbridos, consulte [remover Azure automatização de Runbook híbridos](automation-hybrid-runbook-worker.md#removing-hybrid-runbook-worker)
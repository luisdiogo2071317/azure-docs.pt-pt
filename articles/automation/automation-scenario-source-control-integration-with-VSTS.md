---
title: Integrar a automatização do Azure com o controle da fonte de serviços do Azure DevOps
description: Cenário explica como configurar a integração com uma conta de automatização do Azure e o controle da fonte de serviços de DevOps do Azure.
services: automation
author: eamonoreilly
ms.author: eamono
keywords: powershell do Azure, serviços do Azure DevOps, controlo de origem, automatização
ms.service: automation
ms.component: process-automation
ms.topic: conceptual
ms.date: 09/26/2018
ms.openlocfilehash: b06e315cc12856976dce87791b423d10f002c6df
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801442"
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-azure-devops"></a>Cenário da automatização do Azure - integração de controlo de origem de automatização com o Azure DevOps

> [!NOTE]
> Há uma nova experiência para controlo de origem. Para saber mais sobre a nova experiência, veja [controlo de origem (pré-visualização)](source-control-integration.md).

Neste cenário, tem um projeto de DevOps do Azure que está a utilizar para gerir runbooks de automatização do Azure ou configurações de DSC sob controle de origem.

Este artigo descreve como integrar o Azure DevOps com o seu ambiente de automatização do Azure para que ocorra de integração contínua para cada check-in.

## <a name="getting-the-scenario"></a>Obter o cenário

Neste cenário consiste em dois runbooks do PowerShell que pode ser importado diretamente a partir da [Galeria de Runbooks](automation-runbook-gallery.md) no portal do Azure ou transferência a partir do [galeria do PowerShell](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbooks

Runbook | Descrição|
--------|------------|
VSTS de sincronização | Importar runbooks ou configurações de controle da fonte de DevOps do Azure quando é feito um check-in. Se executar manualmente, importa e publica todos os runbooks ou configurações para a conta de automatização.| 
Sync-VSTSGit | Importar runbooks ou configurações de DevOps do Azure sob controle de origem do Git quando é feito um check-in. Se executar manualmente, importa e publica todos os runbooks ou configurações para a conta de automatização.|

### <a name="variables"></a>Variáveis

Variável | Descrição|
-----------|------------|
VSToken | Proteger o recurso de variável cria que contém o token de acesso pessoal do Azure DevOps. Pode saber como criar um token de acesso pessoal do Azure DevOps no [página de autenticação do Azure DevOps](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate).

## <a name="installing-and-configuring-this-scenario"></a>Instalar e configurar este cenário

Criar uma [token de acesso pessoal](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate) em DevOps do Azure que utilizar para sincronizar os runbooks ou configurações para a sua conta de automatização.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

Criar uma [variável seguro](automation-variables.md) na conta de automatização para conter o token de acesso pessoal para que o runbook pode autenticar para o Azure DevOps e sincronizar os runbooks ou configurações para a conta de automatização. Pode nomear essa VSToken.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

Importe o runbook que sincroniza os seus runbooks ou configurações para a conta de automatização. Pode utilizar o [runbook de exemplo do Azure DevOps](https://www.powershellgallery.com/packages/Sync-VSTS) ou o [DevOps do Azure com o runbook de exemplo do Git](https://www.powershellgallery.com/packages/Sync-VSTSGit) partir o [galeria do PowerShell](https://www.powershellgallery.com) dependendo se usar a origem de DevOps do Azure controle ou do Azure DevOps com o Git e implementar a sua conta de automatização.

![Galeria do PowerShell](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

Agora, pode [publicar](automation-creating-importing-runbook.md#publishing-a-runbook) este runbook para que possa criar um webhook.

![Pulish runbook](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

Criar uma [webhook](automation-webhooks.md) para este runbook de sincronização-VSTS e preencha os parâmetros conforme mostrado abaixo. Certifique-se de que copiar o url do webhook, conforme necessário para um hook de serviço do Azure DevOps. O VSAccessTokenVariableName é o nome (VSToken) da variável segura que criou anteriormente para conter o token de acesso pessoal.

Integração com o Azure DevOps (sincronização-VSTS.ps1) precisa dos seguintes parâmetros:

### <a name="sync-vsts-parameters"></a>Parâmetros de sincronização-VSTS

Parâmetro | Descrição|
--------|------------|
WebhookData | Contém as informações de checkin enviadas a partir do hook de serviço do Azure DevOps. Deve deixar este parâmetro em branco.| 
ResourceGroup | Este é o nome do grupo de recursos que a conta de automatização faz parte.|
AutomationAccountName | O nome da conta de automatização que sincroniza com Azure DevOps.|
VSFolder | O nome da pasta no DevOps do Azure, onde existem o runbooks e configurações.|
VSAccount | O nome da organização do Azure DevOps.|
VSAccessTokenVariableName | O nome da variável seguro (VSToken) que contém o token de acesso pessoal do Azure DevOps.|

![Webhook](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

Se estiver a utilizar o Azure DevOps com o GIT (sincronização-VSTSGit.ps1) serão necessários os seguintes parâmetros.

Parâmetro | Descrição|
--------|------------|
WebhookData | Isto irá conter as informações de checkin enviadas a partir do hook de serviço do Azure DevOps. Deve deixar este parâmetro em branco.|
ResourceGroup | Este nome do grupo de recursos que a conta de automatização faz parte.|
AutomationAccountName | O nome da conta de automatização que sincroniza com Azure DevOps.|
VSAccount | O nome da organização do Azure DevOps.|
VSProject | O nome do projeto de DevOps do Azure, onde existem o runbooks e configurações.|
GitRepo | O nome do repositório Git.|
GitBranch | O nome do ramo no repositório de Git de DevOps do Azure.|
Pasta | O nome da pasta no ramo de Git de DevOps do Azure.|
VSAccessTokenVariableName | O nome da variável seguro (VSToken) que contém o token de acesso pessoal do Azure DevOps.|

![Webhook GIT](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

Crie um hook de serviço no Azure DevOps para check-ins para a pasta que aciona este webhook na entrada do código. Selecione **Webhooks** como o serviço para integrar com quando cria uma nova subscrição. Pode saber mais sobre conexões de serviço no [documentação de Azure DevOps Service Hooks](https://www.visualstudio.com/docs/marketplace/integrate/service-hooks/get-started).
![Hook de serviço](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

Agora deve ser capaz de fazer todos os registos dos seus runbooks e configurações em DevOps do Azure e tem estas automaticamente sincronizado na sua conta de automatização.

![Saída do runbook de sincronização](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

Se executar este runbook manualmente sem a ser acionada pelo Azure DevOps, pode deixar o parâmetro webhookdata vazio e faz uma sincronização completa a partir da pasta de DevOps do Azure especificada.

Se pretender desinstalar o cenário, remova o hook de serviço do Azure DevOps, elimine o runbook e a variável VSToken.

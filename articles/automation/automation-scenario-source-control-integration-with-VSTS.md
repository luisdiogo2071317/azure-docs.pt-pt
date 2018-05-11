---
title: Integrar o automatização do Azure com o Visual Stuido Team Services, o controlo de origem
description: Cenário explica como configurar a integração com uma conta de automatização do Azure e o controlo de origem do Visual Stuido Team Services.
services: automation
documentationcenter: ''
author: eamonoreilly
ms.author: eamono
keywords: o Azure powershell, VSTS, controlo de código fonte, automatização
ms.service: automation
ms.component: process-automation
ms.topic: article
ms.date: 03/19/2017
ms.openlocfilehash: edd9174a9141c4db36a1b25d5f1147f9c7b2e2c2
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-visual-studio-team-services"></a>Cenário de automatização do Azure – integração de controlo de origem de automatização com o Visual Studio Team Services

Neste cenário, tem um projeto do Visual Studio Team Services que está a utilizar para gerir runbooks de automatização do Azure ou configurações de DSC sob o controlo de origem.
Este artigo descreve como integrar VSTS com o seu ambiente de automatização do Azure para que ocorra integração contínua para cada verificação.

## <a name="getting-the-scenario"></a>Obter o cenário

Este cenário é composta por dois runbooks do PowerShell que pode importar diretamente a partir de [Galeria de Runbooks](automation-runbook-gallery.md) no portal do Azure ou transferência a partir do [galeria do PowerShell](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbooks

Runbook | Descrição| 
--------|------------|
VSTS de sincronização | Importar runbooks ou configurações de controlo de origem VSTS quando é efetuado um verificação-in. Se executar manualmente, importa e publica todos os runbooks ou configurações para a conta de automatização.| 
Sync-VSTSGit | Importar runbooks ou configurações de VSTS sob o controlo de origem do Git quando é efetuado um verificação-in. Se executar manualmente, importa e publica todos os runbooks ou configurações para a conta de automatização.|

### <a name="variables"></a>Variáveis

Variável | Descrição|
-----------|------------|
VSToken | Recurso de variável seguro criar que contém o token de acesso pessoal VSTS. Pode saber como criar um token de acesso pessoal VSTS no [página de autenticação VSTS](/vsts/accounts/use-personal-access-tokens-to-authenticate).
## <a name="installing-and-configuring-this-scenario"></a>Instalar e configurar este cenário

Criar um [token de acesso pessoal](/vsts/accounts/use-personal-access-tokens-to-authenticate) no VSTS que utilizar para sincronizar os runbooks ou configurações na sua conta de automatização.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

Criar um [variável segura](automation-variables.md) na sua conta de automatização para conter o token de acesso pessoal para que o runbook pode autenticar VSTS e sincronizar os runbooks ou configurações para a conta de automatização. Pode atribuir o nome deste VSToken. 

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

Importe o runbook que sincroniza-se os runbooks ou configurações para a conta de automatização. Pode utilizar o [runbook de exemplo VSTS](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) ou [VSTS com o runbook de exemplo do Git] (https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) de PowerShellGallery.com dependendo se utilizar o controlo de origem VSTS ou VSTS com o Git e implementar a sua conta de automatização.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

Agora, pode [publicar](automation-creating-importing-runbook.md#publishing-a-runbook) este runbook para que possa criar um webhook. 
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

Criar um [webhook](automation-webhooks.md) para este runbook VSTS de sincronização e preencha os parâmetros conforme mostrado abaixo. Certifique-se que copiar o url do webhook conforme necessário para um hook de serviço no VSTS. O VSAccessTokenVariableName é o nome (VSToken) da variável segura que criou anteriormente para conter o token de acesso pessoal. 

Integração com VSTS (sincronização-VSTS.ps1) utiliza os seguintes parâmetros:
### <a name="sync-vsts-parameters"></a>Parâmetros de sincronização VSTS

Parâmetro | Descrição| 
--------|------------|
WebhookData | Contém as informações de checkin enviadas a partir de rotina de serviço VSTS. Deve deixar este parâmetro em branco.| 
ResourceGroup | Este é o nome do grupo de recursos que está a ser a conta de automatização.|
AutomationAccountName | O nome da conta de automatização que sincroniza-se com VSTS.|
VSFolder | O nome da pasta no VSTS onde existem os runbooks e configurações.|
VSAccount | O nome da conta do Visual Studio Team Services.| 
VSAccessTokenVariableName | O nome da variável seguro (VSToken) que contém o token de acesso pessoal VSTS.| 


![](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

Se estiver a utilizar VSTS com o GIT (sincronização-VSTSGit.ps1) irá demorar os seguintes parâmetros.

Parâmetro | Descrição|
--------|------------|
WebhookData | Isto irá conter as informações de checkin enviadas a partir de rotina de serviço VSTS. Deve deixar este parâmetro em branco.| ResourceGroup | Este nome do grupo de recursos que está a ser a conta de automatização.|
AutomationAccountName | O nome da conta de automatização que sincroniza-se com VSTS.|
VSAccount | O nome da conta do Visual Studio Team Services.|
VSProject | O nome do projeto no VSTS onde existem os runbooks e configurações.|
GitRepo | O nome do repositório de Git.|
GitBranch | O nome do ramo no repositório de VSTS Git.|
Pasta | O nome da pasta no VSTS Git ramo.|
VSAccessTokenVariableName | O nome da variável seguro (VSToken) que contém o token de acesso pessoal VSTS.|

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

Crie um hook de serviço no VSTS para inícios de verificação para a pasta que aciona o webhook na entrada código. Selecione **Web Hooks** como o serviço integrar quando cria uma nova subscrição. Pode saber mais sobre o serviço hooks na [documentação VSTS serviço Hooks](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

Deve ser capaz de todos os inícios verificação dos seus runbooks e configurações no VSTS e ter automaticamente estes synched na sua conta de automatização.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

Se executar este runbook manualmente sem a ser acionado por VSTS, pode deixar o parâmetro webhookdata vazio e faz uma sincronização completa da pasta VSTS especificada.

Se pretender desinstalar o cenário, remover a rotina de serviço VSTS, elimine o runbook e a variável de VSToken.

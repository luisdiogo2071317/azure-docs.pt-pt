---
title: A implementação contínua App Service do Azure | Microsoft Docs
description: Saiba como ativar a implementação contínua para o Serviço de Aplicações do Azure.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: cephalin;dariagrigoriu
ms.openlocfilehash: d83d1ad74d04356f73f18a744c2d1509b5efc280
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233849"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Implementação contínua App Service do Azure
Este artigo mostra-lhe como configurar a implementação contínua para [App Service do Azure](app-service-web-overview.md). Serviço de aplicações permite que a implementação contínua provém de BitBucket, GitHub e [serviços de equipa do Visual Studio (VSTS)](https://www.visualstudio.com/team-services/) ao extrair nas atualizações mais recentes do seu repositório existente de um destes serviços.

Para saber como configurar a implementação contínua manualmente a partir de um repositório de nuvem não são listado pelo portal do Azure (tais como [GitLab](https://gitlab.com/)), consulte [como configurar a implementação contínua utilizando os passos manuais](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

Publica o seu repositório preparado para um dos serviços de suportados. Para mais informações sobre como publicar o seu projeto nestes serviços, veja [Criar um repositório (GitHub)], [Criar um repositório (BitBucket)], e [Introdução ao VSTS].

## <a name="deploy-continuously-from-github"></a>Implementar continuamente a partir do GitHub

Para ativar a implementação contínua com o GitHub, navegue para a página da aplicação do serviço de aplicações no [portal do Azure](https://portal.azure.com).

No menu à esquerda, clique em **Centro de implementação** > **GitHub** > **autorizar**. Siga as instruções de autorização. 

![](media/app-service-continuous-deployment/github-choose-source.png)

Basta autorizar com o GitHub, uma vez. Se já estiver autorizado, basta clicar **continuar**. Pode alterar a conta GitHub autorizada clicando **alterar conta**.

![](media/app-service-continuous-deployment/github-continue.png)

No **fornecedor de compilações** página, escolha o fornecedor de compilação e clique em > **continuar**.

### <a name="option-1-use-app-service-kudu-build-server"></a>Opção 1: utilizar o servidor de compilação do Kudu do serviço de aplicações

No **configurar** página, selecione a organização, o repositório e o ramo partir do qual pretende implementar continuamente. Quando terminar, clique em **continuar**.

### <a name="option-2-use-vsts-continuous-delivery"></a>Opção 2: utilizar a distribuição contínua a VSTS

> [!NOTE]
> Para o App Service criar a compilação necessária e as definições na sua conta VSTS de versão, a conta do Azure tem de ter a função de **proprietário** na sua subscrição do Azure.
>

No **configurar** na página de **código** secção, selecione a organização, o repositório e o ramo partir do qual pretende implementar continuamente. Quando terminar, clique em **continuar**.

No **configurar** na página de **criar** secção, configurar uma nova conta VSTS ou especificar uma conta existente. Quando terminar, clique em **continuar**.

> [!NOTE]
> Se pretender utilizar uma conta VSTS existente que não esteja listada, é necessário [associar a conta VSTS à sua subscrição do Azure](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

No **teste** página, escolha se pretende ativar os testes de carga, em seguida, clique em **continuar**.

Consoante o [escalão de preço](/pricing/details/app-service/plans/) do seu plano de serviço de aplicações, também poderá ver um **implementar para transição** página. Escolha se pretende [ativar as ranhuras de implementação](web-sites-staged-publishing.md), em seguida, clique em **continuar**.

### <a name="finish-configuration"></a>Concluir configuração

No **resumo** página, verifique as suas opções e clique em **concluir**.

Quando tiver concluído a configuração, consolidações novo no repositório de selecionados são continuamente implementadas na sua aplicação de serviço de aplicações.

![](media/app-service-continuous-deployment/github-finished.png)

## <a name="deploy-continuously-from-bitbucket"></a>Implementar continuamente a partir de BitBucket

Para ativar a implementação contínua com BitBucket, navegue para a página da aplicação do serviço de aplicações no [portal do Azure](https://portal.azure.com).

No menu à esquerda, clique em **Centro de implementação** > **BitBucket** > **autorizar**. Siga as instruções de autorização. 

![](media/app-service-continuous-deployment/bitbucket-choose-source.png)

Basta autorizar com BitBucket uma vez. Se já estiver autorizado, basta clicar **continuar**. Pode alterar a conta de BitBucket autorizada clicando **alterar conta**.

![](media/app-service-continuous-deployment/bitbucket-continue.png)

No **configurar** página, selecione o repositório e ramo a partir do qual pretende implementar continuamente. Quando terminar, clique em **continuar**.

No **resumo** página, verifique as suas opções e clique em **concluir**.

Quando tiver concluído a configuração, consolidações novo no repositório de selecionados são continuamente implementadas na sua aplicação de serviço de aplicações.

## <a name="deploy-continuously-from-vsts"></a>Implementar continuamente a partir de VSTS

Para ativar a implementação contínua com VSTS, navegue para a página da aplicação do serviço de aplicações no [portal do Azure](https://portal.azure.com).

No menu à esquerda, clique em **Centro de implementação** > **VSTS** > **continuar**. 

![](media/app-service-continuous-deployment/vsts-choose-source.png)

No **fornecedor de compilações** página, escolha o fornecedor de compilação e clique em > **continuar**.

### <a name="option-1-use-app-service-kudu-build-server"></a>Opção 1: utilizar o servidor de compilação do Kudu do serviço de aplicações

No **configurar** página, selecione a conta VSTS, projeto, repositório e ramo a partir do qual pretende implementar continuamente. Quando terminar, clique em **continuar**.

### <a name="option-2-use-vsts-continuous-delivery"></a>Opção 2: utilizar a distribuição contínua a VSTS

> [!NOTE]
> Para o App Service criar a compilação necessária e as definições na sua conta VSTS de versão, a conta do Azure tem de ter a função de **proprietário** na sua subscrição do Azure.
>

No **configurar** na página de **código** secção, selecione a conta VSTS, projeto, repositório e ramo a partir do qual pretende implementar continuamente. Quando terminar, clique em **continuar**.

> [!NOTE]
> Se pretender utilizar uma conta VSTS existente que não esteja listada, é necessário [associar a conta VSTS à sua subscrição do Azure](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

No **configurar** na página de **criar** secção, especifique a estrutura de idioma que VSTS deve utilizar para executar as tarefas de compilação para o seu repositório selecionado. Quando terminar, clique em **continuar**.

No **teste** página, escolha se pretende ativar os testes de carga, em seguida, clique em **continuar**.

Consoante o [escalão de preço](/pricing/details/app-service/plans/) do seu plano de serviço de aplicações, também poderá ver um **implementar para transição** página. Escolha se pretende [ativar as ranhuras de implementação](web-sites-staged-publishing.md), em seguida, clique em **continuar**. 

### <a name="finish-configuration"></a>Concluir configuração

No **resumo** página, verifique as suas opções e clique em **concluir**.

Quando tiver concluído a configuração, consolidações novo no repositório de selecionados são continuamente implementadas na sua aplicação de serviço de aplicações.

## <a name="disable-continuous-deployment"></a>Desativar a implementação contínua

Para desativar a implementação contínua, navegue para a página da aplicação do serviço de aplicações no [portal do Azure](https://portal.azure.com).

No menu à esquerda, clique em **Centro de implementação** > **GitHub** ou **VSTS** ou **BitBucket**  >  **Desligar**.

![](media/app-service-continuous-deployment/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="additional-resources"></a>Recursos Adicionais

* [Como investigar problemas comuns da implementação contínua](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Como utilizar o PowerShell para o Azure]
* [Documentação do Git]
* [Kudu do projeto](https://github.com/projectkudu/kudu/wiki)
* [Utilizar o Azure para gerar automaticamente um pipeline de CI/CD para implementar uma aplicação ASP.NET 4](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

[Azure portal]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Como utilizar o PowerShell para o Azure]: /powershell/azureps-cmdlets-docs
[Documentação do Git]: http://git-scm.com/documentation

[Criar um repositório (GitHub)]: https://help.github.com/articles/create-a-repo
[Criar um repositório (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Introdução ao VSTS]: https://www.visualstudio.com/docs/vsts-tfs-overview

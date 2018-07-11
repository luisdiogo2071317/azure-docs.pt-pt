---
title: Implementar uma aplicação do Service Fabric com integração contínua (Team Services) no Azure | Microsoft Docs
description: Neste tutorial, irá aprender a configurar a integração contínua e implementação para uma aplicação do Service Fabric com o Visual Studio Team Services.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/13/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: f3cc4f518278cca915e40bd691c6a7674219916e
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37109397"
---
# <a name="tutorial-deploy-an-application-with-cicd-to-a-service-fabric-cluster"></a>Tutorial: Implementar uma aplicação com CI/CD num cluster do Service Fabric

Este tutorial é a quarta parte de uma série e descreve como configurar a integração contínua e implementação para uma aplicação do Azure Service Fabric com o Visual Studio Team Services.  É necessária uma aplicação do Service Fabric existente. A aplicação criada em [Compilar uma aplicação .NET](service-fabric-tutorial-create-dotnet-app.md) é utilizada como exemplo.

Na terceira parte da série, ficará a saber como:

> [!div class="checklist"]
> * Adicionar controlo de origem ao seu projeto
> * Criar uma definição de compilação no Team Services
> * Criar uma definição de versão no Team Services
> * Implementar e atualizar uma aplicação automaticamente

Nesta série de tutoriais, ficará a saber como:
> [!div class="checklist"]
> * [Criar uma aplicação .NET do Service Fabric](service-fabric-tutorial-create-dotnet-app.md)
> * [Implementar a aplicação num cluster remoto](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Adicionar um ponto final HTTPS a um serviço de front-end ASP.NET Core](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
> * Configurar o CI/CD com o Visual Studio Team Services
> * [Configurar a monitorização e os diagnósticos da aplicação](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar este tutorial:

* Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Instale o Visual Studio 2017](https://www.visualstudio.com/) e as cargas de trabalho de **desenvolvimento no Azure** e **desenvolvimento na Web e em ASP.NET**.
* [Instale o SDK do Service Fabric](service-fabric-get-started.md)
* Crie um cluster do Windows Service Fabric no Azure, por exemplo, [seguindo este tutorial](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* Crie uma [conta do Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).

## <a name="download-the-voting-sample-application"></a>Transferir a aplicação de votação de exemplo

Se não conseguiu criar a aplicação de votação de exemplo na [primeira parte desta série de tutoriais](service-fabric-tutorial-create-dotnet-app.md), pode transferi-la. Numa janela do comando, execute o seguinte comando para clonar o repositório da aplicação de exemplo para o seu computador local.

```git
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a>Preparar um perfil de publicação

Agora que já [criou uma aplicação](service-fabric-tutorial-create-dotnet-app.md) e [implementou a aplicação no Azure](service-fabric-tutorial-deploy-app-to-party-cluster.md), está pronto para configurar a integração contínua.  Em primeiro lugar, prepare um perfil de publicação na sua aplicação para ser utilizado pelo processo de implementação que é executado no Team Services.  O perfil de publicação deve ser configurado para visar o cluster que criou anteriormente.  Inicie o Visual Studio e abra um projeto de aplicação do Service Fabric existente.  No **Explorador de Soluções**, clique com o botão direito do rato na aplicação e selecione **Publicar...**.

Escolha um perfil de destino no seu projeto de aplicação para utilizar para o fluxo de trabalho de integração contínua, por exemplo, a Cloud.  Especifique o ponto final de ligação do cluster.  Marque a caixa de verificação **Atualizar a Aplicação** para que a aplicação seja atualizada para cada implementação no Team Services.  Clique na hiperligação **Guardar** para guardar as definições do perfil de publicação e, em seguida, clique em **Cancelar** para fechar a caixa de diálogo.

![Perfil push][publish-app-profile]

## <a name="share-your-visual-studio-solution-to-a-new-team-services-git-repo"></a>Partilhar a sua solução Visual Studio num novo repositório Git do Team Services

Partilhe os ficheiros de origem da aplicação para um projeto de equipa no Team Services, para que possa gerar compilações.

Crie um novo repositório Git local para o seu projeto, selecionando **Adicionar ao Controlo de Origem** -> **Git** na barra de estado no canto inferior direito do Visual Studio.

Na vista **Push** no **Team Explorer**, selecione o botão **Publicar Repositório Git** em **Push para o Visual Studio Team Services**.

![Repositório Git de push][push-git-repo]

Verifique o seu e-mail e selecione a sua conta na lista pendente **Domínio do Team Services**. Introduza o nome do seu repositório e selecione **Publicar repositório**.

![Repositório Git de push][publish-code]

A publicação do repositório cria um novo projeto de equipa na sua conta com o mesmo nome que o repositório local. Para criar o repositório num projeto de equipa existente, clique em **Avançadas** junto ao nome do **Repositório** e selecione um projeto de equipa. Pode ver o código na Web, selecionando **Ver na Web**.

## <a name="configure-continuous-delivery-with-vsts"></a>Configurar a Entrega Contínua com o VSTS

Uma definição de compilação do Team Services descreve um fluxo de trabalho composto por um conjunto de passos de compilação que são executados sequencialmente. Crie uma definição de compilação que produz um pacote de aplicação do Service Fabric, e outros artefactos, para implementar num cluster do Service Fabric. Saiba mais sobre [Definições de compilação do Team Services](https://www.visualstudio.com/docs/build/define/create). 

Uma definição de versão do Team Services descreve um fluxo de trabalho que implementa um pacote de aplicação num cluster. Quando utilizadas em conjunto, a definição de compilação e a definição de versão executam o fluxo de trabalho completo, começando com os ficheiros de origem e terminando com uma aplicação em execução no cluster. Saiba mais sobre as [definições de versão](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition) do Team Services.

### <a name="create-a-build-definition"></a>Criar uma definição de compilação

Abra um browser e navegue até ao novo projeto de equipa em: [https://&lt;myaccount&gt;.visualstudio.com/Voting/Voting%20Team/_git/Voting](https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting).

Selecione o separador **Compilação e Versão**, em seguida, **Compilações** e, em seguida, **+ Nova definição**.  Em **Selecionar um modelo**, selecione o modelo **Aplicação do Azure Service Fabric** e clique em **Aplicar**.

![Escolher o modelo de compilação][select-build-template]

Em **Tarefas**, introduza "Hosted VS2017" como a **Fila de agentes**.

![Selecionar tarefas][save-and-queue]

Em **Acionadores**, ative a integração contínua, definindo o **Estado do acionador**.  Selecione **Guardar e colocar em fila de espera** para iniciar manualmente uma compilação.

![Selecionar acionadores][save-and-queue2]

As compilações também são acionadas após push ou dar entrada. Para verificar o progresso da compilação, mude para o separador **Compilações**.  Depois de verificar se a compilação é executada com êxito, defina uma definição de versão que implementa a aplicação num cluster.

### <a name="create-a-release-definition"></a>Criar uma definição de versão

Selecione o separador **Compilação e Versão**, **Versões** e, em seguida, **+ Nova definição**.  Em **Selecionar um modelo**, selecione o modelo **Implementação do Azure Service Fabric** na lista e, em seguida, **Aplicar**.

![Escolher o modelo de versão][select-release-template]

Selecione **Tarefas**->**Ambiente 1** e, em seguida, **+Novo** para adicionar uma nova ligação de cluster.

![Adicionar ligação de cluster][add-cluster-connection]

Na vista **Adicionar nova Ligação do Service Fabric**, selecione a autenticação **Baseada em certificado** ou **Azure Active Directory**.  Especifique um nome de ligação "mysftestcluster" e um ponto final de cluster "tcp://mysftestcluster.southcentralus.cloudapp.azure.com:19000" (ou o ponto final do cluster no qual está a implementar).

Para a autenticação baseada em certificado, adicione o **Thumbprint do certificado de servidor** do certificado de servidor utilizado para criar o cluster.  Em **Certificado de cliente**, adicione a codificação base 64 do ficheiro de certificado de cliente. Consulte o pop-up de ajuda sobre esse campo para obter informações sobre como obter essa representação com codificação base 64 do certificado. Além disso, adicione a **Palavra-passe** para o certificado.  Pode utilizar o certificado de servidor ou cluster se não tiver um certificado de cliente separado.

Para as credenciais do Azure Active Directory, adicione o **Thumbprint do certificado de servidor** do certificado de servidor utilizado para criar o cluster e as credenciais que pretende utilizar para ligar ao cluster nos campos **Nome de utilizador** e **Palavra-passe**.

Clique em **Adicionar** para guardar a ligação de cluster.

Em seguida, adicione um artefacto de compilação ao pipeline, para a definição de versão possa encontrar o resultado da compilação. Selecione **Pipeline** e **Artefactos**->**+Adicionar**.  Em **Origem (Definição de compilação)**, selecione a definição de compilação que criou anteriormente.  Clique em **Adicionar** para criar o artefacto.

![Adicionar artefacto][add-artifact]

Ative um acionador de implementação contínua para que uma versão seja criada automaticamente quando a compilação for concluída. Clique no ícone de relâmpago no artefacto, ative o acionador e clique em **Guardar** para guardar a definição de versão.

![Ativar o acionador][enable-trigger]

Selecione **+Versão** -> **Criar Versão** -> **Criar** para criar manualmente uma versão.  Verifique se a implementação foi concluída com êxito e se a aplicação está em execução no cluster.  Abra um browser e navegue para [http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/).  Tome nota da versão da aplicação, que neste exemplo é "1.0.0.20170616.3".

## <a name="commit-and-push-changes-trigger-a-release"></a>Consolidar e emitir alterações, acionar uma versão

Para verificar se o pipeline de integração contínua está a funcionar, dê entrada de algumas alterações de código no Team Services.

À medida que escreve o código, as alterações são automaticamente controladas pelo Visual Studio. Para consolidar as alterações ao repositório Git local, selecione o ícone de alterações pendentes (![Pendente][pending]) na barra de estado na parte inferior direita.

Na vista **Alterações**, no Team Explorer, adicione uma mensagem que descreva a atualização e consolide as alterações.

![Consolidar tudo][changes]

Selecione o ícone da barra de estado de alterações não publicadas (![Alterações não publicadas][unpublished-changes]) ou a vista de Sincronização no Team Explorer. Selecione **Push** para atualizar o código no Team Services/TFS.

![Emitir alterações][push]

A emissão das alterações ao Team Services aciona automaticamente uma compilação.  Quando a definição de compilação for concluída com êxito, uma versão é criada automaticamente e começa a atualizar a aplicação no cluster.

Para verificar o progresso da compilação, mude para o separador **Compilações** no **Team Explorer** no Visual Studio.  Depois de verificar se a compilação é executada com êxito, defina uma definição de versão que implementa a aplicação num cluster.

Verifique se a implementação foi concluída com êxito e se a aplicação está em execução no cluster.  Abra um browser e navegue para [http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/).  Tome nota da versão da aplicação, que neste exemplo é "1.0.0.20170815.3".

![Service Fabric Explorer][sfx1]

## <a name="update-the-application"></a>Atualizar a aplicação

Efetue alterações de código na aplicação.  Guarde e consolide as alterações, seguindo os passos anteriores.

Depois de iniciada a atualização de versão da aplicação, pode ver o progresso da atualização de versão no Service Fabric Explorer:

![Service Fabric Explorer][sfx2]

A atualização de versão da aplicação pode demorar vários minutos. Quando a atualização de versão estiver concluída, a aplicação irá executar a versão seguinte.  Neste exemplo, "1.0.0.20170815.4".

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Adicionar controlo de origem ao seu projeto
> * Criar uma definição de compilação
> * Criar uma definição de versão
> * Implementar e atualizar uma aplicação automaticamente

Avance para o tutorial seguinte:
> [!div class="nextstepaction"]
> [Configurar a monitorização e os diagnósticos da aplicação](service-fabric-tutorial-monitoring-aspnet.md)

<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[save-and-queue]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue.png
[save-and-queue2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue2.png
[select-release-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectReleaseTemplate.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
[add-artifact]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddArtifact.png
[enable-trigger]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/EnableTrigger.png
[sfx1]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Push.png
[continuous-delivery-with-VSTS]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpointDialog.png

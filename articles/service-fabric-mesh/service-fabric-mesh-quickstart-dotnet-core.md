---
title: Início rápido - implementar uma aplicação para a malha de recursos de infraestrutura do serviço do Azure | Documentos da Microsoft
description: Este guia de introdução mostra-lhe como criar um Web site do ASP.NET Core e publicá-lo para o modo de malha do Azure Service Fabric.
services: service--fabric-mesh
documentationcenter: .net
author: tylermsft
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: 93b540908391e8eedd9a1b318f3443f3e782c987
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076239"
---
# <a name="quickstart-create-and-deploy-a-web-app-to-azure-service-fabric-mesh"></a>Início rápido: Criar e implementar uma aplicação web para o modo de malha do Azure Service Fabric

Malha do Azure Service Fabric é um serviço totalmente gerido que permite aos programadores implementar aplicações de microsserviços sem gerir máquinas virtuais, armazenamento, ou de rede.

Neste início rápido que irá criar um novo aplicativo de malha do Service Fabric constituída por uma aplicação web ASP.NET Core, executá-la no cluster de desenvolvimento local e, em seguida, publique-o para executar no Azure.

Precisará de uma subscrição do Azure. Se não tiver uma, pode criar facilmente uma subscrição gratuita do Azure, [criar uma conta gratuita](https://azure.microsoft.com/free/) antes de começar. Também terá [configure o ambiente de desenvolvimento](service-fabric-mesh-howto-setup-developer-environment-sdk.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="create-a-service-fabric-mesh-project"></a>Criar um projeto de malha do Service Fabric

Abra o Visual Studio e selecione **arquivo** > **New** > **Project...**

Na **novo projeto** caixa de diálogo **pesquisa** caixa na parte superior, tipo `mesh`. Selecione o **aplicação do Service Fabric em malha** modelo. (Se não vir o modelo, certifique-se de que instalou o SDK de malha e as ferramentas do VS pré-visualizar conforme descrito em [configurar o ambiente de desenvolvimento](service-fabric-mesh-howto-setup-developer-environment-sdk.md). 

Na **Name** , escreva **ServiceFabricMesh1** e, no **localização** , defina o caminho da pasta de onde serão armazenados os ficheiros para o projeto.

Certifique-se de que **criar diretório para a solução** está selecionada e clique em **OK** para criar o projeto de malha do Service Fabric.

![Visual studio malha de recursos de infraestrutura do serviço de diálogo new project](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-project.png)

### <a name="create-a-service"></a>Criar um serviço

Depois de clicar em **OK**, o **novo serviço do Service Fabric** é apresentada a caixa de diálogo. Selecione o **ASP.NET Core** tipo de projeto, certifique-se **Container OS** está definida como **Windows** e prima **OK** para criar o projeto do ASP.NET Core . 

![Visual studio malha de recursos de infraestrutura do serviço de diálogo new project](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-service-fabric-service.png)

O **novo aplicativo da Web ASP.NET Core** é apresentada a caixa de diálogo. Selecione **aplicação Web** e, em seguida, prima **OK**.

![Aplicação do Visual studio novo ASP.NET core](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-aspnetcore-app.png)

Visual Studio criará o projeto de aplicativo de malha do Service Fabric e o projeto do ASP.NET Core.

## <a name="build-and-publish-to-your-local-cluster"></a>Criar e publicar no seu cluster local

Uma imagem do Docker serão automaticamente criada e publicada no seu cluster local, assim que o seu projeto é carregado. Este processo pode demorar algum tempo. Pode monitorizar o progresso das ferramentas do Service Fabric a **saída** painel se desejar. Selecione o **ferramentas do Service Fabric** item no painel. Pode continuar a trabalhar enquanto a imagem do docker está a ser implementada.

Depois do projeto ter sido criado, prima **F5** para depurar o seu serviço localmente. Quando a implementação local está concluída e o Visual Studio está a executar o seu projeto, uma janela do browser será aberto com uma página Web de exemplo.

Quando terminar o serviço implementado a navegar, pode parar a depuração de seu projeto ao premir **SHIFT+F5** no Visual Studio.

## <a name="publish-to-azure"></a>Publicar no Azure

Para publicar o seu projeto de malha do Service Fabric no Azure, com o botão direito no **malha do Service Fabric project** no Visual studio e selecione **publicar...**

![Visual studio com o botão direito projeto de malha do Service Fabric](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-right-click-publish.png)

Verá uma **publicar a aplicação do Service Fabric** caixa de diálogo.

![Visual studio a malha de recursos de infraestrutura do serviço de caixa de diálogo Publicar](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-dialog.png)

Selecione a sua conta do Azure e subscrição. Escolher uma **localização**. Este artigo usa **E.U.A. Leste**.

Sob **grupo de recursos**, selecione  **\<criar novo grupo de recursos... >**. Uma caixa de diálogo é apresentada onde irá criar um novo grupo de recursos. Este artigo utiliza a **E.U.A. Leste** localização e o grupo de nomes **sfmeshTutorial1RG** (se a sua organização tiver várias pessoas com a mesma subscrição, escolha um nome de grupo exclusivo).  Prima **criar** para criar o grupo de recursos e retornar à caixa de diálogo.

![Visual studio malha do Service Fabric novo recurso de diálogo grupo](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-new-resource-group-dialog.png)

De volta a **publicar a aplicação do Service Fabric** caixa de diálogo, em **Azure Container Registry**, selecione  **\<criar novo registo de contentor... >**. Na **criar registo de contentores** caixa de diálogo, utilize um nome exclusivo para o **nome do Container registry**. Especifique um **localização** (Este tutorial utiliza **E.U.A. Leste**). Selecione o **grupo de recursos** que criou no passo anterior na lista pendente, por exemplo, **sfmeshTutorial1RG**. Definir o **SKU** ao **básica** e, em seguida, prima **criar** para regressar à caixa de diálogo Publicar.

Se obtiver um erro que não foi registado um fornecedor de recursos para a sua subscrição, pode registá-lo. Primeiro veja se o fornecedor de recursos está disponível para a sua subscrição:

```Powershell
Get-AzureRmResourceProvider -ListAvailable
```

Se o fornecedor de registo de contentor (`Microsoft.ContainerRegistry`) está disponível, registrá-la a partir do Powershell:

```Powershell
Connect-AzureRmAccount
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ContainerRegistry
```

![Visual studio malha do Service Fabric novo recurso de diálogo grupo](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-new-container-registry-dialog.png)

Na caixa de diálogo Publicar, prima a **publicar** botão para implementar a sua aplicação de malha de recursos de infraestrutura do serviço de mensagens em fila para o Azure.

Quando publica para o Azure pela primeira vez, a imagem do docker é emitidos via push para o Azure Container Registry (ACR) que demora algum tempo dependendo do tamanho da imagem. Subsequentes publica do mesmo projeto será mais rápido. Pode monitorizar o progresso da implementação, selecionando o **ferramentas do Service Fabric** painel no Visual Studio **saída** janela. Depois de concluir a implementação, o **ferramentas do Service Fabric** saída irá apresentar o endereço IP e porta do seu aplicativo na forma de um URL.

```json
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMesh1\Web1\bin\Any CPU\Release\netcoreapp2.0\Web1.dll
Uploading the images to Azure Container Registry...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://...
```

Abra um browser e navegue para o URL para ver o Web site em execução no Azure.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine todos os recursos que criou para este início rápido. Uma vez que criou um novo grupo de recursos para alojar o ACR e malha de recursos de infraestrutura do serviço de recursos de serviço, que pode eliminar em segurança este grupo de recursos que é uma forma fácil de eliminar todos os recursos associados à mesma.

```azurecli
az group delete --resource-group sfmeshTutorial1RG
```

```powershell
Remove-AzureRmResourceGroup -Name sfmeshTutorial1RG
```

Em alternativa, pode eliminar o grupo de recursos [do portal do Azure](https://portal.azure.com).

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre como criar e implementar aplicações de Mesh do Service Fabric, avance para o tutorial.
> [!div class="nextstepaction"]
> [Criar e implementar uma aplicação de múltiplos serviços web](service-fabric-mesh-tutorial-create-dotnetcore.md)

---
title: Início rápido - criar e implementar uma aplicação web para o modo de malha do Azure Service Fabric | Documentos da Microsoft
description: Este guia de introdução mostra-lhe como criar um Web site do ASP.NET Core e publicá-lo para o modo de malha do Azure Service Fabric.
services: service-fabric-mesh
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
ms.date: 07/17/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: d14420a363cfea23c86f63533a4ea89c5f2fd06f
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39412921"
---
# <a name="quickstart-create-and-deploy-a-web-app-to-azure-service-fabric-mesh"></a>Início rápido: Criar e implementar uma aplicação web para o modo de malha do Azure Service Fabric

O Azure Service Fabric Mesh é um serviço totalmente gerido que permite aos programadores implementar aplicações de microsserviços sem gerir máquinas virtuais, armazenamento ou redes.

Neste início rápido que irá criar um novo aplicativo de malha do Service Fabric constituída por uma aplicação web ASP.NET Core, executá-la no cluster de desenvolvimento local e, em seguida, publique-o para executar no Azure.

Precisará de uma subscrição do Azure. Se não tiver uma, pode criar facilmente uma subscrição gratuita do Azure, [criar uma conta gratuita](https://azure.microsoft.com/free/) antes de começar. Também terá [configure o ambiente de desenvolvimento](service-fabric-mesh-howto-setup-developer-environment-sdk.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="create-a-service-fabric-mesh-project"></a>Criar um projeto do Service Fabric Mesh

Abra o Visual Studio e selecione **arquivo** > **New** > **Project...**

Na caixa de diálogo **Novo Projeto**, na parte superior da caixa **Pesquisar**, escreva `mesh`. Selecione o modelo **Aplicação do Service Fabric Mesh**. (Se não vir o modelo, certifique-se de que instalou o SDK do Mesh e a pré-visualização das ferramentas do VS, conforme descrito em [configurar o ambiente de desenvolvimento](service-fabric-mesh-howto-setup-developer-environment-sdk.md). 

Na **Name** , escreva **ServiceFabricMesh1** e, no **localização** , defina o caminho da pasta de onde serão armazenados os ficheiros para o projeto.

Certifique-se de que **Criar diretório para a solução** está selecionado e clique em **OK** para criar o projeto do Service Fabric Mesh.

![Caixa de diálogo de novo projeto do Service Fabric Mesh do Visual Studio](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-project.png)

### <a name="create-a-service"></a>Criar um serviço

Depois de clicar em **OK**, o **novo serviço do Service Fabric** é apresentada a caixa de diálogo. Selecione o **ASP.NET Core** tipo de projeto, certifique-se **Container OS** está definida como **Windows** e clique em **OK** para criar o projeto do ASP.NET Core . 

![Caixa de diálogo de novo projeto do Service Fabric Mesh do Visual Studio](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-service-fabric-service.png)

O **novo aplicativo da Web ASP.NET Core** é apresentada a caixa de diálogo. Selecione **aplicação Web** e, em seguida, clique em **OK**.

![Nova aplicação ASP.NET Core do Visual Studio](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-new-aspnetcore-app.png)

Visual Studio cria o projeto de aplicação Mesh do Service Fabric e o projeto do ASP.NET Core.

## <a name="build-and-publish-to-your-local-cluster"></a>Criar e publicar no seu cluster local

Uma imagem do Docker é criada e publicada no seu cluster local, assim que o seu projeto é carregado automaticamente. Este processo pode demorar algum tempo. Pode monitorizar o progresso das ferramentas do Service Fabric a **saída** janela se desejar, selecionando o **ferramentas do Service Fabric** item no **saída** janela suspensa. Pode continuar a trabalhar enquanto a imagem do docker está a ser implementada.

Depois do projeto ter sido criado, clique em **F5** para depurar o seu serviço localmente. Quando a implementação local está concluída e o Visual Studio está a executar o seu projeto, uma janela do browser será aberto com uma página Web de exemplo.

Quando terminar o serviço implementado a navegar, parar a depuração de seu projeto ao premir **SHIFT+F5** no Visual Studio.

## <a name="publish-to-azure"></a>Publicar no Azure

Para publicar o seu projeto de malha do Service Fabric no Azure, com o botão direito no **malha do Service Fabric project** no Visual studio e selecione **publicar...**

![Visual studio com o botão direito projeto de malha do Service Fabric](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-right-click-publish.png)

Verá uma **publicar a aplicação do Service Fabric** caixa de diálogo.

![Caixa de diálogo de publicação do Service Fabric Mesh do Visual Studio](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-dialog.png)

Selecione a conta e a subscrição do Azure. Escolha uma **Localização**. Este artigo utiliza **E.U.A. Leste**.

Em **Resource group** (Grupo de recursos), selecione **\<Create New Resource Group...>** (Criar Novo Grupo de Recursos). O **criar grupo de recursos** é apresentada a caixa de diálogo. Definir o **nome do grupo de recursos** e **localização**.  Este início rápido utiliza a **E.U.A. Leste** localização e o grupo de nomes **sfmeshTutorial1RG** (se a sua organização tiver várias pessoas com a mesma subscrição, escolha um nome de grupo de recursos exclusivo).  Clique em **criar** para criar o grupo de recursos e retornar à caixa de diálogo.

![Caixa de diálogo do novo grupo de recursos do Service Fabric Mesh do Visual Studio](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-new-resource-group-dialog.png)

De volta à caixa de diálogo **Publish Service Fabric Application** (Publicar Aplicação do Service Fabric), em **Azure Container Registry**, selecione  **\<Create New Container Registry...>** (Criar Novo Registo de Contentor). Na caixa de diálogo **Create Container Registry** (Criar Registo de Contentor), utilize um nome exclusivo para o **Container registry name** (Nome do registo de contentor). Especifique um **localização** (este início rápido utiliza **E.U.A. Leste**). Selecione o **Grupo de recursos** que criou no passo anterior na lista pendente, por exemplo, **sfmeshTutorial1RG**. Definir o **SKU** ao **básica** e, em seguida, clique em **criar** para regressar à caixa de diálogo Publicar.

![Caixa de diálogo do novo grupo de recursos do Service Fabric Mesh do Visual Studio](media/service-fabric-mesh-quickstart-dotnet-core/visual-studio-publish-new-container-registry-dialog.png)

Na caixa de diálogo Publicar, clique nas **publicar** botão para implementar a sua aplicação de malha de recursos de infraestrutura do serviço de mensagens em fila para o Azure.

Quando publica no Azure pela primeira vez, a imagem do docker é enviada para o Azure Container Registry (ACR), o que demora algum tempo consoante o tamanho da imagem. As publicações subsequentes do mesmo projeto serão mais rápidas. Pode monitorizar o progresso da implementação, selecionando **ferramentas do Service Fabric** no Visual Studio **saída** janela suspensa. Depois de concluída a implementação, a saída **Service Fabric Tools** (Ferramentas do Service Fabric) irá apresentar o endereço IP e a porta da aplicação sob a forma de um URL.

```json
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMesh1\Web1\bin\Any CPU\Release\netcoreapp2.0\Web1.dll
Uploading the images to Azure Container Registry...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://...
```

Abra um browser e navegue para o URL para ver o Web site em execução no Azure:

![Aplicação de Web de malha do Service Fabric em execução](media/service-fabric-mesh-tutorial-deploy-dotnetcore/deployed-web-project.png)

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine todos os recursos que criou para este início rápido. Uma vez que criou um novo grupo de recursos para alojar o ACR e malha de recursos de infraestrutura do serviço de recursos de serviço, que pode eliminar em segurança este grupo de recursos, o que é uma forma fácil de eliminar todos os recursos associados à mesma.

```azurecli
az group delete --resource-group sfmeshTutorial1RG
```

```powershell
Connect-AzureRmAccount
Remove-AzureRmResourceGroup -Name sfmeshTutorial1RG
```

Em alternativa, pode eliminar o grupo de recursos [do portal do Azure](https://portal.azure.com).

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre como criar e implementar aplicações do Service Fabric Mesh, avance para o tutorial.
> [!div class="nextstepaction"]
> [Criar, depurar e implantar uma aplicação web com vários serviços no Service Fabric Mesh](service-fabric-mesh-tutorial-create-dotnetcore.md)
---
title: Tutorial - Implementar uma aplicação do Service Fabric Mesh no Service Fabric Mesh | Microsoft Docs
description: Saiba como publicar uma aplicação do Azure Service Fabric Mesh constituída por um site ASP.NET Core que comunica com um serviço Web de back-end.
services: service-fabric-mesh
documentationcenter: .net
author: TylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: f9dea759f6556bc521dda4efbd27176f1e06452b
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126580"
---
# <a name="tutorial-deploy-a-service-fabric-mesh-web-application"></a>Tutorial: Implementar uma aplicação Web do Service Fabric Mesh

Este tutorial é a terceira parte de uma série e mostra-lhe como publicar uma aplicação Web do Azure Service Fabric Mesh diretamente a partir do Visual Studio.

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Publicar a aplicação no Azure.
> * Verificar o estado de implementação da aplicação.
> * Ver todas as aplicações implementadas na sua subscrição.
> * Ver os registos da aplicação.
> * Limpar os recursos utilizados pela aplicação.

Nesta série de tutoriais, ficará a saber como:
> [!div class="checklist"]
> * [Criar uma aplicação Web do Service Fabric Mesh](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * [Depurar a aplicação localmente](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * Publicar a aplicação no Azure

Ficará a saber como criar uma aplicação do Azure Service Fabric Mesh com um serviço de front-end Web ASP.NET e um serviço de back-end de API Web ASP.NET Core. Em seguida, vai depurar a aplicação no seu cluster de desenvolvimento local e publicá-la no Azure. Quando tiver terminado, terá uma aplicação de tarefas simples que demonstra como fazer uma chamada serviço a serviço numa aplicação Web do Service Fabric Mesh.

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar este tutorial:

* Se não tiver uma subscrição do Azure, pode [criar uma conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

* Certifique-se de que [configurou o ambiente de desenvolvimento](service-fabric-mesh-howto-setup-developer-environment-sdk.md), que inclui a instalação do runtime do Service Fabric, o SDK, o Docker e o Visual Studio 2017.

## <a name="download-the-to-do-sample-application"></a>Transferir a aplicação de tarefas de exemplo

Se não conseguiu criar a aplicação de tarefas de exemplo na [segunda parte desta série de tutoriais](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md), pode transferi-la. Numa janela do comando, execute o seguinte comando para clonar o repositório da aplicação de exemplo para o seu computador local.

```
git clone https://github.com/azure-samples/service-fabric-mesh
```

A aplicação está no diretório `src\todolistapp`.

## <a name="publish-to-azure"></a>Publicar no Azure

Para publicar o seu projeto do Service Fabric Mesh no Azure, clique com o botão direito do rato em **ServiceFabricMeshApp** no Visual Studio e selecione **Publish...** (Publicar)

Em seguida, verá a caixa de diálogo **Publish Service Fabric Application** (Publicar Aplicação do Service Fabric).

![Caixa de diálogo de publicação do Service Fabric Mesh do Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-dialog.png)

Selecione a conta e a subscrição do Azure. Escolha uma **Localização**. Este artigo utiliza **E.U.A. Leste**.

Em **Resource group** (Grupo de recursos), selecione **\<Create New Resource Group...>** (Criar Novo Grupo de Recursos). É apresentada uma caixa de diálogo onde irá criar um novo grupo de recursos. Este artigo utiliza a localização **E.U.A. Leste** e atribui o nome **sfmeshTutorial1RG** ao grupo (se a sua organização tiver várias pessoas a utilizar a mesma subscrição, escolha um nome de grupo exclusivo).  Prima **Create** (Criar) para criar o grupo de recursos e voltar à caixa de diálogo de publicação.

![Caixa de diálogo do novo grupo de recursos do Service Fabric Mesh do Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-new-resource-group-dialog.png)

De volta à caixa de diálogo **Publish Service Fabric Application** (Publicar Aplicação do Service Fabric), em **Azure Container Registry**, selecione  **\<Create New Container Registry...>** (Criar Novo Registo de Contentor). Na caixa de diálogo **Create Container Registry** (Criar Registo de Contentor), utilize um nome exclusivo para o **Container registry name** (Nome do registo de contentor). Especifique uma **Localização** (este tutorial utiliza **E.U.A. Leste**). Selecione o **Grupo de recursos** que criou no passo anterior na lista pendente, por exemplo, **sfmeshTutorial1RG**. Defina o **SKU** como **Basic** (Básico) e, em seguida, prima **Create** (Criar) para voltar à caixa de diálogo de publicação.

![Caixa de diálogo do novo grupo de recursos do Service Fabric Mesh do Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-new-container-registry-dialog.png)

Se receber um erro de que o fornecedor de recursos não foi registado na sua subscrição, pode registá-lo. Primeiro, veja se o fornecedor de recursos está disponível para a sua subscrição:

```Powershell
Get-AzureRmResourceProvider -ListAvailable
```

Se o fornecedor de registo de contentor (`Microsoft.ContainerRegistry`) estiver disponível, registe-o a partir do Powershell:

```Powershell
Connect-AzureRmAccount
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ContainerRegistry
```

Na caixa de diálogo de publicação, prima o botão **Publish** (Publicar) para implementar a aplicação do Service Fabric no Azure.

Quando publica no Azure pela primeira vez, a imagem do docker é enviada para o Azure Container Registry (ACR), o que demora algum tempo consoante o tamanho da imagem. As publicações subsequentes do mesmo projeto serão mais rápidas. Pode monitorizar o progresso da implementação ao selecionar o painel **Service Fabric Tools** (Ferramentas do Service Fabric) na janela **Output** (Saída) do Visual Studio. Depois de concluída a implementação, a saída **Service Fabric Tools** (Ferramentas do Service Fabric) irá apresentar o endereço IP e a porta da aplicação sob a forma de um URL.

```json
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMeshApp\ToDoService\bin\Any CPU\Release\netcoreapp2.0\ToDoService.dll
Uploading the images to Azure Container Registy...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://10.000.38.000:20000.
```

Abra um browser e navegue para o URL para ver o site em execução no Azure.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pode utilizar o Azure Cloud Shell ou uma instalação local da CLI do Azure para os passos restantes.

Se optar por instalar e utilizar a CLI localmente, este tutorial requer que execute uma versão da CLI do Azure que seja a 2.0.35 ou posterior. Executar `az --version` para localizar a versão. Para instalar ou atualizar a versão mais recente da CLI, veja [Instalar a CLI 2.0 do Azure][azure-cli-install].

## <a name="install-the-az-mesh-cli"></a>Instalar az mesh cli
Na linha de comandos da CLI

1) Remova qualquer instalação anterior do módulo da CLI do Azure Service Fabric Mesh.

```cli
az extension remove --name mesh
```

2)  Instale o módulo de extensão da CLI do Azure Service Fabric Mesh. Para a pré-visualização, a CLI do Azure Service Fabric Mesh é escrita como uma extensão da CLI do Azure. No entanto, em pré-visualização pública, é fornecida como parte da CLI do Azure.

```cli
az extension add --source https://sfmeshcli.blob.core.windows.net/cli/mesh-0.8.1-py2.py3-none-any.whl
```

## <a name="check-application-deployment-status"></a>Verificar o estado de implementação da aplicação.

Neste momento, a aplicação foi implementada. Pode verificar o estado com o comando `app show`. 

O nome da aplicação do tutorial é `ServiceMeshApp`. Reúna os detalhes sobre a aplicação com o seguinte comando:

```azurecli-interactive
az mesh app show --resource-group $rg --name ServiceMeshApp
```

## <a name="see-all-applications-currently-deployed-to-your-subscription"></a>Ver todas as aplicações implementadas na sua subscrição.

Pode utilizar o comando "app list" para obter uma lista das aplicações implementadas na sua subscrição.

```cli
az mesh app list --output table
```

## <a name="see-the-application-logs"></a>Ver os registos da aplicação.

Examine os registos da aplicação implementada:

```azurecli-interactive
az mesh code-package-log get --resource-group $rg --application-name ServiceMeshApp --service-name todoservice --replica-name 0 --code-package-name ServiceMeshApp
```

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não forem necessários, elimine todos os recursos que criou. Uma vez que criou um novo grupo de recursos para alojar o ACR e os recursos do serviço Service Fabric Mesh, pode eliminar este grupo de recursos em segurança, o que eliminará todos os recursos associados.

```azurecli
az group delete --resource-group sfmeshTutorial1RG
```

```powershell
Remove-AzureRmResourceGroup -Name sfmeshTutorial1RG
```

em alternativa, pode eliminar o grupo de recursos [do portal do Azure](../azure-resource-manager/resource-group-portal.md#delete-resource-group-or-resources). 

## <a name="next-steps"></a>Passos seguintes

Nesta parte do tutorial, ficou a saber como:
> [!div class="checklist"]
> * Publicar a aplicação no Azure.
> * Verificar o estado de implementação da aplicação.
> * Ver todas as aplicações atualmente implementadas na sua subscrição.
> * Ver os registos da aplicação.
> * Limpar os recursos utilizados pela aplicação.

Agora que concluiu a publicação de uma aplicação do Service Fabric Mesh no Azure, experimente o seguinte:

* Explore a [aplicação de voto de exemplo](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp) para ver outro exemplo de comunicação serviço a serviço.
* Leia [Recursos do Service Fabric](service-fabric-mesh-service-fabric-resources.md)
* Saiba mais sobre o [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest
---
title: Criar um ambiente de desenvolvimento do Kubernetes na cloud | Microsoft Docs
titleSuffix: Azure Dev Spaces
author: ghogen
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: quickstart
description: Desenvolvimento rápido do Kubernetes com contentores e microsserviços no Azure
keywords: Docker, Kubernetes, Azure, AKS, Serviço Azure Kubernetes, contentores
manager: douge
ms.openlocfilehash: 9bee5677aecb235872f50eea75ddc98bc453f426
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34361720"
---
# <a name="quickstart-create-a-kubernetes-development-environment-with-azure-dev-spaces-net-core-and-visual-studio"></a>Início Rápido: Criar um ambiente de desenvolvimento do Kubernetes com o Azure Dev Spaces (.NET Core e Visual Studio)

Neste guia, vai aprender a:

- Criar um ambiente baseado no Kubernetes no Azure otimizado para o desenvolvimento.
- Utilizar o Visual Studio para desenvolver iterativamente código em contentores.

[!INCLUDE[](includes/see-troubleshooting.md)]

[!INCLUDE[](includes/portal-aks-cluster.md)]

## <a name="get-the-visual-studio-tools"></a>Obter as ferramentas do Visual Studio 
1. Instale a versão mais recente do [Visual Studio 2017](https://www.visualstudio.com/vs/)
1. No instalador do Visual Studio, confirme que está selecionada a seguinte Carga de Trabalho:
    * Desenvolvimento ASP.NET e Web
1. Instale a [extensão do Visual Studio para o Azure Dev Spaces](https://aka.ms/get-azds-visualstudio)

Está agora pronto para criar uma aplicação Web ASP.NET com o Visual Studio.

## <a name="create-an-aspnet-web-app"></a>Criar uma aplicação Web ASP.NET

No Visual Studio 2017, crie um projeto novo. Atualmente, o projeto tem de ser uma **aplicação Web ASP.NET Core**. Dê ao projeto o nome “**webfrontend**”.

![](media/get-started-netcore-visualstudio/NewProjectDialog1.png)

Selecione o modelo **Web Application (Model-View-Controller)** e certifique-se de que está a segmetnar **.NET Core** e **ASP.NET Core 2.0** nos dois menus pendentes na parte superior da caixa de diálogo. Clique em **OK** para criar o projeto.

![](media/get-started-netcore-visualstudio/NewProjectDialog2.png)


## <a name="create-a-dev-environment-in-azure"></a>Criar um ambiente de desenvolvimento no Azure

Com o Azure Dev Spaces, pode criar ambientes de desenvolvimento baseados no Kubernetes que são totalmente geridos pelo Azure e otimizados para desenvolvimento. Com o projeto que acabou de criar aberto, selecione **Azure Dev Spaces**, no menu de definições de início, conforme mostrado abaixo.

![](media/get-started-netcore-visualstudio/LaunchSettings.png)

Na caixa de diálogo que é apresentada, confirme que tem sessão iniciada com a conta adequada e, em seguida, selecione um cluster existente.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog.png)

Por agora, deixe o menu pendente **Space** (Espaço) com a predefinição `default`. Vai aprender mais sobre esta opção mais tarde. Selecione a caixa de verificação **Publicly Accessible** (Acessível Publicamente) para que a aplicação Web seja acessível a partir de um ponto final público. Esta definição não é necessária, mas será útil demonstrar alguns conceitos mais adiante nestas instruções. Mas não se preocupe. Em qualquer caso, poderá depurar o seu site com o Visual Studio.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog2.png)

Clique em **OK** para selecionar ou criar o cluster.

Se optar por um cluster que não tenha sido configurado para funcionar com Azure Dev Spaces, verá uma mensagem a perguntar se pretende configurá-lo.

![](media/get-started-netcore-visualstudio/Add-Azure-Dev-Spaces-Resource.png)

Escolha **OK**. 

Será iniciada uma tarefa em segundo plano para essa configuração. Demorará alguns minutos a concluir. Para ver se ainda está a ser criado, passe o rato sobre o ícone **Background tasks** (Tarefas em segundo plano), no canto inferior esquerdo, conforme mostrado na imagem abaixo.

![](media/get-started-netcore-visualstudio/BackgroundTasks.png)

> [!Note]
Não pode depurar a sua aplicação enquanto o ambiente de desenvolvimento não estiver criado.

## <a name="look-at-the-files-added-to-project"></a>Ver os ficheiros adicionados ao projeto
Enquanto aguarda pela criação do ambiente de desenvolvimento, veja os ficheiros que foram adicionados ao projeto quando optou por utilizar este tipo de ambiente.

Em primeiro lugar, pode ver que foi adicionada uma pasta com o nome `charts` e, dentro desta, foi estruturado um [gráfico de Helm](https://docs.helm.sh) para a sua aplicação. Estes ficheiros são utilizados para implementar a aplicação no ambiente de desenvolvimento.

Verá que foi adicionado um ficheiro com o nome `Dockerfile`. Esse ficheiro tem informações necessárias para empacotar a aplicação no formato padrão do Docker. Também é criado um ficheiro `HeaderPropagation.cs`, que vamos abordar mais adiante nestas instruções. 

Por último, vai ver um ficheiro denominado `azds.yaml`, que contém informações de configuração necessária para o ambiente de desenvolvimento, como, por exemplo, se a aplicação deve estar acessível através de um ponto final público.

![](media/get-started-netcore-visualstudio/ProjectFiles.png)

## <a name="debug-a-container-in-kubernetes"></a>Depurar um contentor no Kubernetes
Assim que o ambiente de desenvolvimento for criado, pode depurar a aplicação. Defina um ponto de interrupção no código, por exemplo, na linha 20 do ficheiro `HomeController.cs`, no qual a variável `Message` está definida. Clique em **F5** para iniciar a depuração. 

O Visual Studio irá comunicar com o ambiente de desenvolvimento para criar e implementar a aplicação e, em seguida, abre um browser com a aplicação Web em execução. Poderá parecer que o contentor está a ser executado localmente. Contudo, na verdade, está a ser executado no ambiente de desenvolvimento no Azure. O motivo para o endereço localhost tem que ver com o facto de o Azure Dev Spaces criar um túnel SSH temporário para o contentor que está a ser executado no Azure.

Clique na ligação **About** (Sobre), na parte superior da página, para acionar o ponto de interrupção. Tem acesso total às informações de depuração, tal como aconteceria se o código estivesse a ser executado localmente, como, por exemplo, a pilha de chamadas, as variáveis locais, informações de exceção, etc.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Working with multiple containers and team development](get-started-netcore-visualstudio.md#call-another-container) (Trabalhar com vários contentores e o desenvolvimento em equipa)
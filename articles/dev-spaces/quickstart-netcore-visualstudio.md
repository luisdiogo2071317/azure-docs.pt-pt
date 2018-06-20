---
title: Criar um espaço de programador do Kubernetes na cloud | Microsoft Docs
titleSuffix: Azure Dev Spaces
author: ghogen
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
ms.author: ghogen
ms.date: 06/06/2018
ms.topic: quickstart
description: Desenvolvimento rápido da Kubernetes com contentores e microsserviços no Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contentores
manager: douge
ms.openlocfilehash: 16ec493708f85e9b3819943e131b9f9c3649f27e
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34824643"
---
# <a name="quickstart-create-a-kubernetes-dev-space-with-azure-dev-spaces-net-core-and-visual-studio"></a>Início Rápido: Criar um espaço de programador do Kubernetes com os Espaços de Programador do Azure (.NET Core e Visual Studio)

Neste guia, vai aprender a:

- Configurar os Espaços de Programador do Azure com um cluster Kubernetes gerido no Azure.
- Utilizar o Visual Studio para desenvolver iterativamente código em contentores.
- Depurar o código em execução no cluster.

> [!Note]
> **Se ficar bloqueado** em qualquer altura, veja a secção [Resolução de problemas](troubleshooting.md) ou publique um comentário nesta página. Também pode consultar o [tutorial](get-started-netcore-visualstudio.md) mais detalhado.

## <a name="prerequisites"></a>Pré-requisitos

- Um cluster Kubernetes a executar o Kubernetes 1.9.6, na região EUA Leste, Europa Ocidental ou Leste do Canadá, com o Encaminhamento de Aplicações de Http ativado.

  ![É necessário ativar o Encaminhamento de Aplicações de Http.](media/common/Kubernetes-Create-Cluster-3.PNG)

- Visual Studio 2017 com a carga de trabalho de Desenvolvimento Web instalada. Se não a tiver instalado, transfira-a [aqui](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

## <a name="set-up-azure-dev-spaces"></a>Configurar os Espaços de Programador do Azure

Instale a [extensão do Visual Studio para os Espaços de Programador do Azure](https://aka.ms/get-azds-visualstudio).

## <a name="connect-to-a-cluster"></a>Ligar a um cluster

Em seguida, vai criar e configurar um projeto para Espaços de Programador do Azure.

### <a name="create-an-aspnet-web-app"></a>Criar uma aplicação Web ASP.NET

No Visual Studio 2017, crie um projeto novo. Atualmente, o projeto tem de ser uma **aplicação Web ASP.NET Core**. Atribua o nome **webfrontend** ao projeto.

Selecione o modelo **Aplicação Web (Modelo-Vista-Controlador)** e verifique se está a apontar para **.NET Core** e **ASP.NET Core 2.0**.

### <a name="create-a-dev-space-in-azure"></a>Criar um espaço do programador no Azure

Com o projeto que acabou de criar aberto, selecione **Azure Dev Spaces** no menu pendente de definições de início, conforme mostrado abaixo.

![](media/get-started-netcore-visualstudio/LaunchSettings.png)

Na caixa de diálogo que é apresentada, confirme que tem sessão iniciada com a conta adequada e, em seguida, selecione um cluster existente.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog.png)

Por agora, mantenha o `default` no menu pendente **Espaço**. Selecione a caixa de verificação **Publicly Accessible** (Acessível Publicamente) para que a aplicação Web seja acessível a partir de um ponto final público.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog2.png)

Clique em **OK** para selecionar ou criar o cluster.

Se optar por um cluster que não tenha sido configurado para funcionar com Azure Dev Spaces, verá uma mensagem a perguntar se pretende configurá-lo.

![](media/get-started-netcore-visualstudio/Add-Azure-Dev-Spaces-Resource.png)

Escolha **OK**. 

### <a name="look-at-the-files-added-to-project"></a>Ver os ficheiros adicionados ao projeto
Enquanto aguarda pela criação do espaço de programador, veja os ficheiros que foram adicionados ao projeto quando optou por utilizar os Espaços de Programador do Azure.

- Pode ver que foi adicionada uma pasta com o nome `charts` e que, dentro desta, foi estruturado um [gráfico do Helm](https://docs.helm.sh) para a sua aplicação. Estes ficheiros são utilizados para implementar a aplicação no espaço de programador.
- `Dockerfile` tem as informações necessárias para empacotar a aplicação no formato padrão do Docker.
- `azds.yaml` contém as informações de configuração necessárias ao ambiente de programador, como, por exemplo, se a aplicação deve estar acessível através de um ponto final público.

![](media/get-started-netcore-visualstudio/ProjectFiles.png)

## <a name="debug-a-container-in-kubernetes"></a>Depurar um contentor no Kubernetes
Assim que o ambiente de programador estiver criado, pode depurar a aplicação. Defina um ponto de interrupção no código, por exemplo, na linha 20 do ficheiro `HomeController.cs` no qual está definida a variável `Message`. Clique em **F5** para iniciar a depuração. 

O Visual Studio vai comunicar com o espaço de programador para criar e implementar a aplicação e, em seguida, vai abrir um browser com a aplicação Web em execução. Poderá parecer que o contentor está a ser executado localmente. Contudo, na verdade, está a ser executado no espaço de programador no Azure. O motivo para o endereço localhost tem que ver com o facto de o Azure Dev Spaces criar um túnel SSH temporário para o contentor que está a ser executado no Azure.

Clique na ligação **About** (Acerca de) na parte superior da página para acionar o ponto de interrupção. Tem acesso total às informações de depuração, tal como aconteceria se o código estivesse a ser executado localmente, como, por exemplo, a pilha de chamadas, as variáveis locais, informações de exceção, etc.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Working with multiple containers and team development](get-started-netcore-visualstudio.md#call-another-container) (Trabalhar com vários contentores e o desenvolvimento em equipa)
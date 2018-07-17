---
title: Criar um espaço de programador do Kubernetes na cloud com o .NET Core e o Visual Studio | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 07/09/2018
ms.topic: tutorial
description: Desenvolvimento rápido da Kubernetes com contentores e microsserviços no Azure
keywords: Docker, Kubernetes, Azure, AKS, Serviço Azure Kubernetes, contentores
manager: douge
ms.openlocfilehash: 452746e897f95f12a25522ea9f37ca0254a12d6d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38612874"
---
# <a name="get-started-on-azure-dev-spaces-with-net-core-and-visual-studio"></a>Introdução ao Azure Dev Spaces com o .NET Core e o Visual Studio

Neste guia, vai aprender a:

- Configurar os Espaços de Programador do Azure com um cluster Kubernetes gerido no Azure.
- Utilizar o Visual Studio para desenvolver iterativamente código em contentores.
- Desenvolver de modo independente dois serviços separados e utilizar a deteção do serviço DNS de Kubernetes para fazer uma chamada para outro serviço.
- Desenvolver e testar de forma produtiva o seu código num ambiente de equipa.

[!INCLUDE[](includes/see-troubleshooting.md)]

[!INCLUDE[](includes/portal-aks-cluster.md)]

## <a name="get-the-visual-studio-tools"></a>Obter as ferramentas do Visual Studio
1. Instale a versão mais recente do [Visual Studio 2017](https://www.visualstudio.com/vs/)
1. No instalador do Visual Studio, certifique-se de que a seguinte Carga de Trabalho está selecionada:
    * Desenvolvimento ASP.NET e Web
1. Instale as [Ferramentas do Visual Studio para Kubernetes](https://aka.ms/get-azds-visualstudio)

## <a name="create-a-web-app-running-in-a-container"></a>Criar uma aplicação Web em execução num contentor

Nesta secção, vai criar uma aplicação Web em ASP.NET Core e executá-la num contentor no Kubernetes.

### <a name="create-an-aspnet-web-app"></a>Criar uma aplicação Web ASP.NET

No Visual Studio 2017, crie um projeto novo. Atualmente, o projeto tem de ser uma **aplicação Web ASP.NET Core**. Atribua o nome "**webfrontend**" ao projeto.

![](media/get-started-netcore-visualstudio/NewProjectDialog1.png)

Selecione o modelo **Web Application (Model-View-Controller)** (Aplicação Web (Controlador de Vista de Modelo)) e certifique-se de que está a apontar para **.NET Core** e **ASP.NET Core 2.0** nos dois menus pendentes na parte superior da caixa de diálogo. Clique em **OK** para criar o projeto.

![](media/get-started-netcore-visualstudio/NewProjectDialog2.png)


### <a name="enable-dev-spaces-for-an-aks-cluster"></a>Ativar os Espaços de Programador para um cluster do AKS

Com o projeto que acabou de criar, selecione **Azure Dev Spaces** no menu pendente de definições de início, conforme mostrado abaixo.

![](media/get-started-netcore-visualstudio/LaunchSettings.png)

Na caixa de diálogo apresentada a seguir, certifique-se de que tem sessão iniciada com a conta adequada e, em seguida, selecione um cluster Kubernetes existente.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog.PNG)

Por agora, mantenha a predefinição `default` no menu pendente **Space** (Espaço). Vai aprender mais sobre esta opção posteriormente. Selecione a caixa de verificação **Publicly Accessible** (Acessível Publicamente) para que a aplicação Web seja acessível a partir de um ponto final público. Esta definição não é obrigatória, mas será útil para demonstrar alguns conceitos mais adiante nestas instruções. Mas não se preocupe. Em qualquer caso, poderá depurar o seu site com o Visual Studio.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog2.png)

Clique em **OK** para selecionar ou criar o cluster.

Se optar por um cluster que não tenha sido preparado para funcionar com o Azure Dev Spaces, ser-lhe-á apresentada uma mensagem a perguntar se pretende configurá-lo.

![](media/get-started-netcore-visualstudio/Add-Azure-Dev-Spaces-Resource.png)

Escolha **OK**.

 Será iniciada uma tarefa em segundo plano para executar essa configuração. Esta operação demorará alguns minutos a ser concluída. Para ver se ainda está a ser criado, faça pairar o ponteiro sobre o ícone **Background tasks** (Tarefas em segundo plano) no canto inferior esquerdo da barra de tarefas, conforme mostrado na imagem abaixo.

![](media/get-started-netcore-visualstudio/BackgroundTasks.PNG)

> [!Note]
> Não pode depurar a sua aplicação enquanto o espaço não estiver criado.

### <a name="look-at-the-files-added-to-project"></a>Ver os ficheiros adicionados ao projeto
Enquanto aguarda pela criação do espaço de programador, veja os ficheiros que foram adicionados ao projeto quando optou por utilizar o espaço de programador.

Em primeiro lugar, pode ver que foi adicionada uma pasta com o nome `charts` e que, dentro desta, foi estruturado um [gráfico de Helm](https://docs.helm.sh) para a sua aplicação. Estes ficheiros são utilizados para implementar a aplicação no espaço de programador.

Verá que foi adicionado um ficheiro com o nome `Dockerfile`. Este ficheiro tem as informações necessárias para empacotar a aplicação no formato padrão do Docker.

Por último, verá um ficheiro denominado `azds.yaml`, que contém a configuração em tempo de desenvolvimento que é exigida pelo espaço de programador.

![](media/get-started-netcore-visualstudio/ProjectFiles.png)

## <a name="debug-a-container-in-kubernetes"></a>Depurar um contentor no Kubernetes
Assim que o ambiente de programador estiver criado, pode depurar a aplicação. Defina um ponto de interrupção no código, por exemplo, na linha 20 do ficheiro `HomeController.cs` no qual está definida a variável `Message`. Clique em **F5** para iniciar a depuração. 

O Visual Studio vai comunicar com o espaço de programador para criar e implementar a aplicação e, em seguida, vai abrir um browser com a aplicação Web em execução. Poderá parecer que o contentor está a ser executado localmente. Contudo, na verdade, está a ser executado no espaço de programador no Azure. O motivo para o endereço localhost deve-se ao facto de o Azure Dev Spaces criar um túnel SSH temporário para o contentor que está a ser executado no AKS.

Clique na ligação **About** (Acerca de) na parte superior da página para acionar o ponto de interrupção. Tem acesso total às informações de depuração, tal como aconteceria se o código estivesse a ser executado localmente, como, por exemplo, a pilha de chamadas, as variáveis locais, informações de exceção, etc.

## <a name="iteratively-develop-code"></a>Desenvolver código de forma iterativa

O Azure Dev Spaces não se limita apenas a pôr o código em execução no Kubernetes. Tem que ver com permitir-lhe ver, de forma rápida e iterativa, as alterações ao código serem aplicadas num ambiente do Kubernetes na cloud.

### <a name="update-a-content-file"></a>Atualizar um ficheiro de conteúdo
1. Localize o ficheiro `./Views/Home/Index.cshtml` e faça uma edição ao HTML. Por exemplo, altere a linha 70 que lê `<h2>Application uses</h2>` para algo semelhante a `<h2>Hello k8s in Azure!</h2>`
1. Guarde o ficheiro.
1. Aceda ao seu browser e atualize a página. Deverá ver a página Web mostrar o código HTML atualizado.

O que aconteceu? As edições aos ficheiros de conteúdos, como HTML e CSS, não requerem a recompilação numa aplicação Web .NET Core, pelo que uma sessão F5 ativa sincroniza automaticamente qualquer ficheiro de conteúdos modificado com o contentor em execução no AKS, de modo a que possa ver as edições aos conteúdos de imediato.

### <a name="update-a-code-file"></a>Atualizar um ficheiro de código
A atualização de ficheiros de código exige mais algum trabalho, porque a aplicação .NET Core tem de ser recompilada e produzir binários de aplicação atualizados.

1. Pare o depurador no Visual Studio.
1. Abra o ficheiro de código com o nome `Controllers/HomeController.cs` e edite a mensagem que a página About (Sobre) vai apresentar: `ViewData["Message"] = "Your application description page.";`
1. Guarde o ficheiro.
1. Prima **F5** para iniciar novamente a depuração. 

Em vez de reconstruir e reimplementar uma imagem de contentor nova sempre que forem feitas edições ao código, o que, muitas vezes, irá demorar um tempo considerável, o Azure Dev Spaces recompilará incrementalmente o código dentro do contentor existente para proporcionar um ciclo de edição/depuração mais rápido.

Atualize a aplicação Web no browser e aceda à página About (Sobre). Deverá ver a mensagem personalizada apresentada na IU.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre desenvolvimento em equipa](team-development-netcore-visualstudio.md)

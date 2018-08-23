---
title: Depurar aplicações num contentor local do Docker | Documentos da Microsoft
description: Saiba como modificar uma aplicação que está a executar num contentor do Docker local, atualizar o contentor através de edição e de atualização e definir a depuração de pontos de interrupção
services: azure-container-service
documentationcenter: na
author: ghogen
manager: douge
editor: ''
ms.assetid: 480e3062-aae7-48ef-9701-e4f9ea041382
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/22/2016
ms.author: mlearned
ms.openlocfilehash: 01741ba25ac3a6ab187a08636b7e26ee58dbee90
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/10/2018
ms.locfileid: "42055769"
---
# <a name="debugging-apps-in-a-local-docker-container"></a>Depurar aplicações num contentor do Docker local
## <a name="overview"></a>Descrição geral
Visual Studio 2017 fornece uma forma consistente para desenvolver num contentor do Docker de Linux e validar a sua aplicação localmente.
Não tem de reiniciar o contentor toda vez que fizer um alteração de código.
Este artigo ilustra como usar o recurso de "Editar e atualizar" para iniciar uma aplicação ASP.NET Core Web num contentor do Docker local, faça as alterações necessárias e, em seguida, atualize o browser para ver essas alterações.
Este artigo também mostra como configurar pontos de interrupção para depuração.

> [!NOTE]
> Suporte para contentores do Windows estará disponível numa versão futura
>
>

## <a name="prerequisites"></a>Pré-requisitos
As seguintes ferramentas tem de ser instaladas.

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)
* [SDK do Microsoft ASP.NET Core 1.0](https://go.microsoft.com/fwlink/?LinkID=809122)

Para executar contentores do Docker localmente, terá de um cliente de local docker.
Pode utilizar o [caixa de ferramentas do Docker](https://www.docker.com/products/docker-toolbox), que requer que o Hyper-V para ser desativado ou pode utilizar [Docker para Windows](https://www.docker.com/get-docker), que utiliza o Hyper-V e requer o Windows 10.

Se utilizar a caixa de ferramentas do Docker, precisará [configurar o cliente de Docker](vs-azure-tools-docker-setup.md)

## <a name="1-create-a-web-app"></a>1. Criar uma aplicação Web
[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Adicionar suporte do Docker
[!INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-edit-your-code-and-refresh"></a>3. Editar o seu código e a atualização
Para iterar rapidamente alterações, pode iniciar a aplicação dentro de um contêiner e continuar para fazer alterações, vê-los tal como faria com o IIS Express.

1. Definir a configuração da solução `Debug` e prima  **&lt;CTRL + F5 >** para criar a imagem do docker e executá-la localmente.

    Depois da imagem de contentor foi criada e está em execução num contentor do Docker, o Visual Studio inicia a aplicação Web no browser predefinido.
    Se estiver a utilizar o browser Microsoft Edge ou caso contrário, têm erros, veja [resolução de problemas](vs-azure-tools-docker-troubleshooting-docker-errors.md) secção.
2. Aceda à página About, o que é onde que vamos fazer nossa alterações.
3. Retornar ao Visual Studio e abra `Views\Home\About.cshtml`.
4. Adicione o seguinte conteúdo HTML para o fim do ficheiro e guarde as alterações.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```
5. Exibindo a janela de saída, quando a compilação do .NET é concluída e verá estas linhas, mude novamente para o seu browser e atualize a página About.

   ```
   Now listening on: http://*:80
   Application started. Press Ctrl+C to shut down
   ```
6. As suas alterações foram aplicadas!

## <a name="4-debug-with-breakpoints"></a>4. Depurar com pontos de interrupção
Muitas vezes, as alterações irão precisar de mais inspeção, aproveitando os recursos de depuração do Visual Studio.

1. Retornar ao Visual Studio e abra `Controllers\HomeController.cs`
2. Substitua os conteúdos do método About() com o seguinte:

   ```
   string message = "Your application description page from within a Container";
   ViewData["Message"] = message;
   ````
3. Defina um ponto de interrupção à esquerda do `string message`... linha.
4. Pressionar  **&lt;F5 >** para iniciar a depuração.
5. Navegue para a página sobre ao atingir o ponto de interrupção.
6. Mude para o Visual Studio para ver o ponto de interrupção e inspecionar o valor da mensagem.

   ![][2]

## <a name="summary"></a>Resumo
Com o suporte do Docker no Visual Studio 2017, pode obter a produtividade de trabalhar localmente, com o realismo de produção de desenvolvimento de dentro de um contentor do Docker.

## <a name="troubleshooting"></a>Resolução de problemas
[Resolução de problemas de desenvolvimento do Visual Studio Docker](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Mais informações sobre o Docker com o Visual Studio, Windows e do Azure
* [Ferramentas do docker para Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - criar e implementar contentores do docker
* [Ferramentas do docker para Visual Studio Code](http://aka.ms/dockertoolsforvscode) -serviços de idioma na edição de arquivos de docker, com mais cenários de e2e futuras
* [Informações de contentor do Windows](http://aka.ms/containers)-informações do Windows Server e o servidor Nano
* [Azure Container Service](https://azure.microsoft.com/services/container-service/) - [conteúdo de serviço de contentor do Azure](http://aka.ms/AzureContainerService)
* Para obter mais exemplos de como trabalhar com o Docker, consulte [trabalhar com o Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) partir do [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demonstração](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Para obter mais inícios rápidos da demonstração de HealthClinic.biz, veja [Azure Developer Tools Quickstarts (Inícios Rápidos de Ferramentas de Programador do Azure)](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

## <a name="various-docker-tools"></a>Várias ferramentas de Docker
[Algumas ferramentas de docker excelente (blogue de Steve Lasker)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Boa artigos
[Introdução ao Microsserviços do NGINX](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Apresentações
* [Steve Lasker: VS Live 2016 de Las Vegas - e2e do Docker](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
* [Introdução ao ASP.NET Core @ compilação 2016 – onde na demonstração](https://channel9.msdn.com/Events/Build/2016/B810)
* [Desenvolvendo aplicativos .NET em contentores, Channel 9](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png

---
title: Depurar aplicações num contentor local do Docker | Documentos da Microsoft
description: Saiba como modificar uma aplicação que está a executar num contentor do Docker local, atualizar o contentor através de edição e de atualização e definir a depuração de pontos de interrupção
services: container-service
author: ghogen
manager: douge
ms.assetid: 480e3062-aae7-48ef-9701-e4f9ea041382
ms.service: multiple
ms.topic: article
ms.workload: multiple
ms.date: 09/11/2018
ms.author: ghogen
ms.openlocfilehash: 8844992ffbed2d57ad54a5f0e4a2d0b05c1d277a
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969399"
---
# <a name="debugging-apps-in-a-local-docker-container"></a>Depurar aplicações num contentor do Docker local
## <a name="overview"></a>Descrição geral
Visual Studio 2017 fornece uma forma consistente para desenvolver num contentor do Docker e validar a sua aplicação localmente.
Não tem de reiniciar o contentor toda vez que fizer um alteração de código.
Este artigo ilustra como usar o recurso de "Editar e atualizar" para iniciar uma aplicação ASP.NET Core Web num contentor do Docker local, faça as alterações necessárias e, em seguida, atualize o browser para ver essas alterações.
Este artigo também mostra como configurar pontos de interrupção para depuração.

## <a name="prerequisites"></a>Pré-requisitos
As seguintes ferramentas tem de ser instaladas.

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) com a carga de trabalho de desenvolvimento para a Web instalada.

Para executar contentores do Docker localmente, terá de um cliente de local docker.
Pode utilizar o [caixa de ferramentas do Docker](https://www.docker.com/products/docker-toolbox), que requer que o Hyper-V para ser desativado ou pode utilizar [Docker para Windows](https://www.docker.com/get-docker), que utiliza o Hyper-V e requer o Windows 10.

Se utilizar a caixa de ferramentas do Docker, precisará [configurar o cliente de Docker](vs-azure-tools-docker-setup.md)

## <a name="1-create-a-web-app"></a>1. Criar uma aplicação Web
[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-edit-your-code-and-refresh"></a>2. Editar o seu código e a atualização
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

## <a name="3-debug-with-breakpoints"></a>3. Depurar com pontos de interrupção
Muitas vezes, as alterações irão precisar de mais inspeção, aproveitando os recursos de depuração do Visual Studio.

1. Retornar ao Visual Studio e abra `About.cshtml.cs`
2. Substitua os conteúdos do método OnGet() com o seguinte:

   ```cs
       Message = "Your application description page from within a Container";
   ```

3. Defina um ponto de interrupção para a esquerda da linha de código.
4. Pressionar  **&lt;F5 >** para iniciar a depuração.
5. Navegue para a página sobre ao atingir o ponto de interrupção.
6. Mude para o Visual Studio para ver o ponto de interrupção e inspecionar o valor da mensagem.

   ![][2]

## <a name="summary"></a>Resumo
Com o suporte do Docker no Visual Studio 2017, pode obter a produtividade de trabalhar localmente, com o realismo de produção de desenvolvimento de dentro de um contentor do Docker.

## <a name="troubleshooting"></a>Resolução de problemas
[Resolução de problemas de desenvolvimento do Visual Studio Docker](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Mais informações sobre o Docker com o Visual Studio, Windows e do Azure
* [Desenvolvimento de contentores com o Visual Studio](/visualstudio/containers) -uma página de hub de desenvolvimento do contentor
* [Integração de docker para Pipelines do Azure](http://aka.ms/dockertoolsforvsts) - criar e implementar contentores do docker
* [Ferramentas do docker para Visual Studio Code](http://aka.ms/dockertoolsforvscode) -serviços de idioma na edição de arquivos de docker, com mais cenários de e2e futuras
* [Informações de contentor do Windows](http://aka.ms/containers)-informações do Windows Server e o servidor Nano
* [Serviço Kubernetes do Azure](https://azure.microsoft.com/services/kubernetes-service/) - [documentação do serviço Kubernetes do Azure](/azure/aks)

## <a name="various-docker-tools"></a>Várias ferramentas de Docker
[Algumas ferramentas de docker excelente (blogue de Steve Lasker)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Boa artigos
[Introdução ao Microsserviços do NGINX](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Apresentações
* [Steve Lasker: VS Live 2016 de Las Vegas - e2e do Docker](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
* [Introdução ao ASP.NET Core @ compilação 2016 – onde na demonstração](https://channel9.msdn.com/Events/Build/2016/B810)
* [Desenvolvendo aplicativos .NET em contentores, Channel 9](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png

---
title: Criar um espaço de desenvolvimento Kubernetes na cloud com o .NET Core e o VS Code | Microsoft Docs
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
ms.openlocfilehash: fdaba2afecd453794dbee3b47ef5202700d005d2
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/15/2018
ms.locfileid: "41918801"
---
# <a name="get-started-on-azure-dev-spaces-with-net-core"></a>Introdução ao Azure Dev Spaces com o .NET Core

[!INCLUDE[](includes/learning-objectives.md)]

[!INCLUDE[](includes/see-troubleshooting.md)]

Está agora pronto para criar um espaço de desenvolvimento baseado no Kubernetes no Azure.

[!INCLUDE[](includes/portal-aks-cluster.md)]

## <a name="install-the-azure-cli"></a>Instalar a CLI do Azure
O Azure Dev Spaces só precisa de configuração mínima do computador local. A maior parte da configuração do espaço de desenvolvimento é armazenada na cloud e é partilhável com outros utilizadores. O seu computador local pode executar Windows, Mac ou Linux. Para Linux, são suportadas as seguintes distribuições: Ubuntu (18.04, 16.04 e 14.04), Debian 8 e 9, RHEL 7, Fedora 26+, CentOS 7, openSUSE 42.2 e SLES 12.

Comece por transferir e executar a [CLI do Azure](/cli/azure/install-azure-cli?view=azure-cli-latest). 

> [!IMPORTANT]
> Se já tiver a CLI do Azure instalada, certifique-se de que está a utilizar a versão 2.0.43 ou superior.

[!INCLUDE[](includes/sign-into-azure.md)]

[!INCLUDE[](includes/use-dev-spaces.md)]

[!INCLUDE[](includes/install-vscode-extension.md)]

Enquanto aguarda que o cluster seja criado, pode começar a desenvolver código.

## <a name="create-a-web-app-running-in-a-container"></a>Criar uma aplicação Web em execução num contentor

Nesta secção, vai criar uma aplicação Web em ASP.NET Core e executá-la num contentor no Kubernetes.

### <a name="create-an-aspnet-core-web-app"></a>Criar uma aplicação Web ASP.NET Core
Se tiver o [.NET Core](https://www.microsoft.com/net) instalado, pode criar rapidamente uma aplicação Web ASP.NET Core numa pasta denominada `webfrontend`.
    
```cmd
dotnet new mvc --name webfrontend
```

Em alternativa, **transfira o código de exemplo do GitHub** ao navegar para https://github.com/Azure/dev-spaces e selecione **Clone or Download** (Clonar ou Transferir) para transferir o repositório do GitHub para o seu ambiente local. O código para este guia está em `samples/dotnetcore/getting-started/webfrontend`.

[!INCLUDE[](includes/azds-prep.md)]

[!INCLUDE[](includes/build-run-k8s-cli.md)]

### <a name="update-a-content-file"></a>Atualizar um ficheiro de conteúdo
O Azure Dev Spaces não se limita apenas a pôr o código em execução no Kubernetes. Tem que ver com permitir-lhe ver, de forma rápida e iterativa, as alterações ao código serem aplicadas num ambiente do Kubernetes na cloud.

1. Localize o ficheiro `./Views/Home/Index.cshtml` e faça uma edição ao HTML. Por exemplo, altere a linha 70 que lê `<h2>Application uses</h2>` para algo semelhante a `<h2>Hello k8s in Azure!</h2>`
1. Guarde o ficheiro. Pouco depois, na janela do terminal, verá uma mensagem que diz que um ficheiro no contentor em execução foi atualizado.
1. Aceda ao seu browser e atualize a página. Deverá ver a página Web mostrar o código HTML atualizado.

O que aconteceu? As edições aos ficheiros de conteúdos, como HTML e CSS, não requerem a recompilação numa aplicação Web .NET Core, pelo que um comando `azds up` ativo sincroniza automaticamente qualquer ficheiro de conteúdos modificado com o contentor em execução no Azure, de modo a que possa ver as edições aos conteúdos de imediato.

### <a name="update-a-code-file"></a>Atualizar um ficheiro de código
A atualização de ficheiros de código exige mais algum trabalho, porque a aplicação .NET Core tem de ser recompilada e produzir binários de aplicação atualizados.

1. Na janela de terminal, prima `Ctrl+C` (para parar `azds up`).
1. Abra o ficheiro de código com o nome `Controllers/HomeController.cs` e edite a mensagem que a página About (Sobre) vai apresentar: `ViewData["Message"] = "Your application description page.";`
1. Guarde o ficheiro.
1. Execute `azds up` na janela de terminal. 

Este comando recria a imagem do contentor e reimplementa o gráfico Helm. Para ver as alterações ao código entrarem em vigor na aplicação em execução, aceda ao menu About (Sobre) na aplicação Web.


Mas existe um *método ainda mais rápido* para programar código, que vai explorar na próxima secção. 

## <a name="debug-a-container-in-kubernetes"></a>Depurar um contentor no Kubernetes

[!INCLUDE[](includes/debug-intro.md)]

[!INCLUDE[](includes/init-debug-assets-vscode.md)]


### <a name="select-the-azds-debug-configuration"></a>Selecionar a configuração de depuração do AZDS
1. Para abrir a vista Debug (Depuração), clique no ícone Debug em **Activity Bar** (Barra de Atividades), no lado do VS Code.
1. Selecione **.NET Core Launch (AZDS)** (Iniciar .NET Core [AZDS]) como a configuração de depuração ativa.

![](media/get-started-netcore/debug-configuration.png)

> [!Note]
> Se não vir nenhum comando do Azure Dev Spaces em Command Palette (Paleta de Comandos), confirme que tem instalada a extensão do VS Code para o Azure Dev Spaces. Lembre-se de que a área de trabalho que abriu no VS Code é a pasta que contém azds.yaml.


### <a name="debug-the-container-in-kubernetes"></a>Depurar o contentor no Kubernetes
Prima **F5** para depurar o código no Kubernetes.

Tal como sucede com o comando `up`, o código é sincronizado com o espaço de programador e é criado e implementado um contentor no Kubernetes. Desta vez, obviamente, o depurador está ligado ao contentor remoto.

[!INCLUDE[](includes/tip-vscode-status-bar-url.md)]

Defina um ponto de interrupção num ficheiro de código do lado do servidor, como, por exemplo, a função `Index()` no ficheiro de origem `Controllers/HomeController.cs`. Atualizar a página do browser faz com que o ponto de interrupção seja atingido.

Tem acesso total às informações da depuração, tal como aconteceria se o código estivesse a ser executado localmente, como, por exemplo, a pilha de chamadas, as variáveis locais, as informações de exceção, etc.

### <a name="edit-code-and-refresh"></a>Editar o código e atualizar
Com o depurador ativo, faça uma edição ao código. Por exemplo, modifique a mensagem da página About (Sobre), em `Controllers/HomeController.cs`. 

```csharp
public IActionResult About()
{
    ViewData["Message"] = "My custom message in the About page.";
    return View();
}
```

Guarde o ficheiro e, no **painel Debug actions** (Ações de depuração), clique no botão **Refresh** (Atualizar). 

![](media/get-started-netcore/debug-action-refresh.png)

Em vez de reconstruir e reimplementar uma imagem de contentor nova sempre que forem feitas edições ao código, o que, muitas vezes, irá demorar um tempo considerável, o Azure Dev Spaces recompilará incrementalmente o código dentro do contentor existente para proporcionar um ciclo de edição/depuração mais rápido.

Atualize a aplicação Web no browser e aceda à página About (Sobre). Deverá ver a mensagem personalizada apresentada na IU.

**Agora, tem um método para iterar rapidamente no código e depurar diretamente no Kubernetes.** Em seguida, irá ver como pode criar e chamar um segundo contentor.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre desenvolvimento em equipa](team-development-netcore.md)
